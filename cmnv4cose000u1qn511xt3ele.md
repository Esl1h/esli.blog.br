---
title: "Beyond GPG: Hardware-Backed File Encryption with age and YubiKey"
datePublished: 2026-04-12T02:02:45.766Z
cuid: cmnv4cose000u1qn511xt3ele
slug: beyond-gpg-hardware-backed-file-encryption-with-age-and-yubikey
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/fc0c5fd4-6104-4c3d-95f7-5d818c2c2a7b.png
tags: security, encryption, yubikey, yubico, yubikeys

---

## 1\. Introduction: The Problem with GPG

GPG has been the de facto standard for file encryption for decades, but its age shows. The OpenPGP specification (RFC 4880) carries decades of backward compatibility baggage: configurable cipher suites that invite downgrade attacks, a web-of-trust model that almost nobody uses correctly, a keyring/keyserver architecture that leaks metadata, and a UX so hostile that even security professionals routinely misconfigure it. The attack surface is enormous — not because the cryptography is weak, but because the *decision space* presented to the user is.

OpenSSL's `enc` command is another common choice, but it's even worse from a usability standpoint: no native public-key encryption, no authenticated encryption by default (CBC mode without HMAC), and easy-to-forget flags that silently produce insecure output.

[age](https://github.com/FiloSottile/age) was created by Filippo Valsorda (former Go cryptography lead at Google) to solve exactly this problem: provide a file encryption tool that is **secure by default**, with no configuration knobs to turn wrong, explicit key management, and UNIX-style composability. When combined with [age-plugin-yubikey](https://github.com/str4d/age-plugin-yubikey), it delivers hardware-backed key protection with a fraction of GPG+smartcard complexity.

## 2\. What is age

`age` (pronounced "ah-geh", from the Italian word) is a file encryption tool, file format, and library. Its design philosophy can be summarized in four principles:

*   **No configuration.** No config files, no keyrings, no keyservers. Keys are explicit arguments.
    
*   **No algorithm negotiation.** One cipher suite, chosen by the developers, not the user.
    
*   **Small explicit keys.** Recipients are short strings (`age1...`) that fit in a chat message.
    
*   **UNIX composability.** Reads from stdin, writes to stdout, pipes cleanly.
    

The format specification lives at [age-encryption.org/v1](https://age-encryption.org/v1) and has multiple interoperable implementations: the reference Go implementation ([filippo.io/age](https://github.com/FiloSottile/age)), `rage` in Rust, `typage` in TypeScript, and others in Python, Java, Kotlin, and Swift. The plugin system (introduced in v1.1.0) enables custom recipient types — hardware tokens, cloud KMS, post-quantum hybrids — without touching the core format.

## 3\. Cryptographic Primitives

`age` uses a deliberately small, modern cryptographic suite with no user-selectable options:

| Primitive | Algorithm | Purpose |
| --- | --- | --- |
| Key agreement | **X25519** (Curve25519 ECDH) | Asymmetric key wrapping for `age1...` recipients |
| Symmetric encryption | **ChaCha20-Poly1305** | AEAD encryption of the file payload |
| Key derivation | **HKDF-SHA-256** | Deriving the header MAC key and stream key from the file key |
| Passphrase KDF | **scrypt** | Stretching passphrases into encryption keys |

### How it fits together

1.  A random 16-byte **file key** is generated per encryption operation.
    
2.  The file key is **wrapped** (encrypted) independently for each recipient — using X25519 ephemeral ECDH for public-key recipients, scrypt for passphrases, or a plugin-specific mechanism for hardware tokens.
    
3.  Each wrapped copy becomes a **stanza** in the ASCII header.
    
4.  HKDF-SHA-256 derives two keys from the file key: one for the **header MAC** (authenticating the header stanzas) and one for the **payload stream key**.
    
5.  The payload is encrypted as a stream of **64 KiB chunks** using ChaCha20-Poly1305, where each chunk is independently authenticated.
    

This design provides **forward privacy** (ephemeral ECDH keys mean a compromised long-term key cannot decrypt past ciphertexts) and **multi-recipient support** without re-encrypting the payload — only the 16-byte file key is wrapped once per recipient.

For post-quantum resistance, `age` now supports hybrid **ML-KEM-768 + X25519** keys (recipients starting with `age1pq1...`), though this is still experimental.

## 4\. Basic age Usage

### Key generation

```bash
# Generate a new X25519 key pair
age-keygen -o key.txt
# Output:
# Public key: age1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# The file contains the secret key (AGE-SECRET-KEY-1...)
# The public key is printed to stderr
```

### Encryption and decryption

```bash
# Encrypt to a recipient's public key
age -r age1recipient... -o secret.txt.age secret.txt

# Encrypt to multiple recipients
age -r age1alice... -r age1bob... -o shared.age document.pdf

# Encrypt with a passphrase (interactive prompt)
age -p -o backup.tar.age backup.tar

# Decrypt with a key file
age -d -i key.txt -o secret.txt secret.txt.age

# Pipe-friendly usage
tar czf - ./project | age -r age1recipient... > project.tar.gz.age
age -d -i key.txt < project.tar.gz.age | tar xzf -
```

### SSH key compatibility

```bash
# Encrypt to an existing SSH public key
age -R ~/.ssh/id_ed25519.pub -o secret.age secret.txt

# Decrypt with the corresponding SSH private key
age -d -i ~/.ssh/id_ed25519 secret.age > secret.txt
```

## 5\. YubiKey Integration

This is where `age` genuinely outclasses GPG. Setting up GPG with a smartcard involves generating keys on the host, moving subkeys to card slots, managing trust databases, and configuring `gpg-agent`. With `age-plugin-yubikey`, the entire flow is: install the plugin, run one command, encrypt.

### How age-plugin-yubikey works

The plugin uses the YubiKey's **PIV (Personal Identity Verification) applet** — the same applet used for smart card authentication in enterprise environments. Specifically:

*   It generates an **ECDSA P-256** key pair directly on the YubiKey.
    
*   The private key **never leaves the hardware**.
    
*   The key is stored in one of the **20 "retired" PIV slots** (82–95), which avoids conflicts with standard PIV slots (9a, 9c, 9d, 9e) that may hold RSA keys for other purposes.
    
*   An `age` identity file is produced that contains the **slot reference and YubiKey serial number** — enough for the plugin to locate the key at decryption time, but not the key material itself.
    

The plugin acts as a bridge: `age` clients discover it via the naming convention (`age-plugin-yubikey` in `$PATH`), delegate key operations to it, and the plugin communicates with the YubiKey via the PC/SC interface.

### HMAC-SHA1 Challenge-Response vs. PIV: Two Approaches Compared

The [YubiKey Shell Toolkit](https://github.com/Esl1h/yubikey-shell-toolkit) implements both approaches. Understanding the tradeoffs is important:

| Aspect | HMAC-SHA1 (OTP Slot 2) | PIV + age |
| --- | --- | --- |
| **Mechanism** | Challenge-response to derive an AES key via PBKDF2 | ECDH key agreement using PIV-stored P-256 key |
| **Encryption** | AES-256-CBC via OpenSSL (+ PBKDF2, 600k iterations) | ChaCha20-Poly1305 via `age` (authenticated encryption) |
| **Artifacts** | `.yk.enc` file + `.yk.challenge` file (both required) | Single `.age` file + identity file for decryption |
| **Multi-recipient** | Not natively supported | Native `age` feature — wrap file key per recipient |
| **Ecosystem** | Custom scripts, toolkit-specific | Standard `age` format, interoperable with all `age` clients |
| **PIN policy** | Touch only (configured at slot programming) | Configurable: `never`, `once`, `always` for both PIN and touch |
| **Authentication** | No AEAD — CBC without integrated MAC | AEAD (Poly1305) — tamper detection built in |

**The HMAC approach** is simpler and works on YubiKeys without PIV support, but it relies on OpenSSL's `enc` command in CBC mode (not authenticated encryption) and requires managing an auxiliary `.yk.challenge` file. The challenge file alone is not sensitive — without the physical YubiKey to compute the HMAC response, it's useless — but losing it means losing the file.

**The PIV approach** is strictly superior for most use cases: authenticated encryption, multi-recipient support, a standardized format, and configurable PIN/touch policies. Use HMAC only when PIV is unavailable (e.g., YubiKey NEO).

#### HMAC approach (for reference)

The [`yk-encrypt-file.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-encrypt-file.sh) script demonstrates the HMAC flow:

```bash
#!/usr/bin/env bash
set -euo pipefail

INPUT="${1:?Usage: $0 <file>}"
FILE="$(cd "$(dirname "$INPUT")" && pwd)/$(basename "$INPUT")"
SLOT=2

# Generate random challenge
CHALLENGE=$(openssl rand -hex 32)

# YubiKey computes HMAC — secret never leaves hardware
HMAC=$(ykman otp calculate 2 "$CHALLENGE" 2>/dev/null) || {
    echo "[!] YubiKey did not respond." >&2; exit 1
}

# Derive AES-256 key from challenge+HMAC
KEY=$(echo -n "${CHALLENGE}${HMAC}" | openssl dgst -sha256 -binary | xxd -p -c 256)

# Encrypt with AES-256-CBC + PBKDF2
openssl enc -aes-256-cbc -pbkdf2 -iter 600000 \
    -k "$KEY" -in "$FILE" -out "${FILE}.yk.enc"

echo "$CHALLENGE" > "${FILE}.yk.challenge"
```

### Installation and Setup

#### Prerequisites

```bash
# Debian/Ubuntu
sudo apt-get install pcscd age libpcsclite-dev

# Fedora
sudo dnf install pcsc-lite age pcsc-lite-devel

# Arch
sudo pacman -S pcsclite pcsc-tools yubikey-manager age

# Ensure pcscd is running
sudo systemctl enable --now pcscd
```

#### Install age-plugin-yubikey

```bash
# Homebrew (macOS/Linux)
brew install age-plugin-yubikey

# Cargo (Rust 1.70+)
cargo install age-plugin-yubikey

# Arch Linux
pacman -S age-plugin-yubikey
```

Pre-built binaries for Windows, Linux, and macOS are available on the [releases page](https://github.com/str4d/age-plugin-yubikey/releases).

#### Generate a YubiKey identity

**Interactive mode** (recommended for first-time setup):

```bash
age-plugin-yubikey
```

This launches a text-based wizard that:

1.  Detects connected YubiKeys and lets you select one.
    
2.  Shows available PIV slots and selects a free "retired" slot (e.g., slot 82).
    
3.  Prompts you to name the identity.
    
4.  Asks for **PIN policy** (`never`, `once`, `always`) and **touch policy** (`never`, `always`, `cached`).
    
5.  If default PINs are detected, prompts you to change them.
    
6.  If the default management key is detected, generates a random one and stores it in PIN-protected metadata.
    

**Programmatic mode**:

```bash
age-plugin-yubikey --generate \
    --slot 1 \
    --name "backup-encryption" \
    --pin-policy once \
    --touch-policy always \
    > ~/.config/yk-toolkit/age/yubikey-identity.txt
```

Extract the recipient (public key) for sharing:

```bash
# List all age recipients from connected YubiKeys
age-plugin-yubikey --list

# Or extract from a specific slot
age-plugin-yubikey --identity --slot 1 2>/dev/null \
    | grep "^# recipient:" | cut -d' ' -f3 \
    > ~/.config/yk-toolkit/age/yubikey-recipient.txt
```

### Practical Examples

#### Single-recipient encryption with YubiKey

The [`yk-age-encrypt.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt.sh) script wraps `age` with sensible defaults:

```bash
# Encrypt using the stored YubiKey recipient
./yk-age-encrypt.sh document.pdf
# → document.pdf.age

# Encrypt with an explicit recipient
./yk-age-encrypt.sh -r age1yubikey1qfmj3... -o secrets.age secrets.env

# Decrypt (requires physical YubiKey + PIN/touch)
./yk-age-decrypt.sh document.pdf.age
```

The script automatically resolves the recipient by checking (in order):

1.  The `-r` flag.
    
2.  `~/.config/yk-toolkit/age/yubikey-recipient.txt`.
    
3.  Live query via `age-plugin-yubikey --list`.
    

#### Direct age commands (no wrapper)

```bash
# Encrypt
age -r age1yubikey1qfmj3... -o report.age report.xlsx

# Decrypt — the plugin handles YubiKey interaction transparently
age -d -i yubikey-identity.txt -o report.xlsx report.age
```

When the touch policy is set to `always`, the YubiKey's LED will blink during decryption — physical touch is required to authorize the cryptographic operation.

## 6\. Multi-Recipient Encryption

This is where `age` + YubiKey truly shines for team workflows. Since `age` wraps the file key independently for each recipient, you can encrypt a single file to multiple YubiKeys, software keys, or a mix of both.

### Use cases

*   **Team access**: Encrypt shared secrets to every team member's YubiKey.
    
*   **Redundancy**: Encrypt to both a primary and backup YubiKey in case one is lost.
    
*   **Hybrid recovery**: Encrypt to your YubiKey *and* a passphrase-protected software key stored in a safe.
    
*   **CI/CD pipelines**: Encrypt to a hardware key for humans and a software key for automation.
    

### Multi-recipient scripts

The [`yk-age-encrypt-multikeys.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt-multikeys.sh) script supports multiple recipients via `-r` flags or a recipients file:

```bash
# Encrypt to multiple recipients via flags
./yk-age-encrypt-multikeys.sh \
    -r age1yubikey1qfmj3...   \
    -r age1yubikey1q2xk7...   \
    -r age1ql3z7hjy54pw...    \
    secrets.tar.gz

# Or maintain a recipients file
cat ~/.config/yk-toolkit/age/recipients.txt
# Primary YubiKey
age1yubikey1qfmj3...
# Backup YubiKey
age1yubikey1q2xk7...
# Software fallback key
age1ql3z7hjy54pw...

# Encrypt using the recipients file (auto-loaded)
./yk-age-encrypt-multikeys.sh secrets.tar.gz
```

The corresponding [`yk-age-decrypt-multikeys.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-decrypt-multikeys.sh) tries multiple identity files:

```bash
# Decrypt with multiple identity files
./yk-age-decrypt-multikeys.sh \
    -i ~/yubikey-primary-identity.txt \
    -i ~/yubikey-backup-identity.txt \
    secrets.tar.gz.age

# Or maintain an identities file
cat ~/.config/yk-toolkit/age/identities.txt
/home/user/.config/yk-toolkit/age/yubikey-identity.txt
/home/user/.config/yk-toolkit/age/backup-identity.txt

# Decrypt using the identities file (auto-loaded)
./yk-age-decrypt-multikeys.sh secrets.tar.gz.age
```

`age` will try each identity until one succeeds — you only need *one* matching YubiKey inserted.

### Practical redundancy pattern

```bash
# Setup: generate identities on two YubiKeys
age-plugin-yubikey --generate --slot 1 --name "primary" \
    --pin-policy once --touch-policy always > primary-identity.txt

# Swap YubiKeys
age-plugin-yubikey --generate --slot 1 --name "backup" \
    --pin-policy once --touch-policy always > backup-identity.txt

# Also generate a software recovery key
age-keygen -o recovery-key.txt

# Collect all recipients
age-plugin-yubikey --list > all-recipients.txt
grep "^# public" recovery-key.txt | cut -d: -f2 >> all-recipients.txt

# Encrypt backups to all three
tar czf - ~/documents | age -R all-recipients.txt -o backup.tar.gz.age
```

If the primary YubiKey is lost, decrypt with the backup or the software key. Then re-encrypt with a new set of recipients.

## 7\. Security Considerations and Best Practices

### Hardware-backed key advantages

*   **Non-extractable private keys**: The PIV applet does not allow exporting generated private keys. Even with physical access to the YubiKey and the PIN, the key material cannot be read.
    
*   **PIN brute-force protection**: After 3 failed PIN attempts, the YubiKey locks. After 3 failed PUK attempts, the PIV applet is permanently locked (requires a full reset, destroying all keys).
    
*   **Touch confirmation**: With `--touch-policy always`, every cryptographic operation requires physical contact — malware cannot silently decrypt files even if it has access to the identity file and the YubiKey is plugged in.
    

### Best practices

1.  **Change default PINs immediately.** The default PIV PIN is `123456` and PUK is `12345678`. `age-plugin-yubikey` prompts for this, but verify.
    
2.  **Use** `--pin-policy once --touch-policy always` as a baseline. "Once" means the PIN is cached for the session (avoiding repeated prompts during batch decryption), while "always" touch prevents silent use.
    
3.  **Always encrypt to at least two recipients** — your primary YubiKey and a recovery mechanism (backup YubiKey or software key in secure storage).
    
4.  **Protect identity files.** While the identity file doesn't contain the private key, it tells `age` *which* YubiKey and slot to use. Treat it as sensitive metadata: `chmod 600`.
    
5.  **Store software recovery keys offline** — printed on paper, in a safe deposit box, or on an encrypted USB drive stored separately.
    
6.  **Rotate keys when a YubiKey is lost.** Re-encrypt all accessible data to a new recipient set that excludes the lost key.
    
7.  **Pin caching caveat**: On YubiKey 4 series, PIN cache preservation does not work due to how the serial number is obtained — the plugin performs a soft reset that clears the cache. YubiKey 5 series handles this correctly.
    

### Threat model boundaries

`age` with YubiKey protects **data at rest**. It does not provide:

*   **Signing or authentication** (use SSH or FIDO2 for that).
    
*   **Forward secrecy for stored files** — if a file was encrypted to a recipient, compromising that recipient's key allows decryption of that specific file. Forward privacy applies to the *encrypting* side (ephemeral ECDH), not the recipient's long-term key.
    
*   **Deniability** — the header reveals the number of recipients (stanza count) and the recipient type (plugin name).
    

## 8\. Ecosystem and Tooling

### Core tools

| Tool | Language | Description |
| --- | --- | --- |
| [age](https://github.com/FiloSottile/age) | Go | Reference implementation — CLI + library |
| [rage](https://github.com/str4d/rage) | Rust | Alternative implementation, supports plugins |
| [age-plugin-yubikey](https://github.com/str4d/age-plugin-yubikey) | Rust | YubiKey PIV integration |
| [age-keygen](https://github.com/FiloSottile/age) | Go | Key generation (bundled with `age`) |

### Integration tools

*   [**SOPS**](https://github.com/getsops/sops) — Mozilla's secret manager supports `age` as a backend. Combine with `age-plugin-yubikey` for hardware-backed secret management in GitOps workflows.
    
*   [**YubiKey Shell Toolkit**](https://github.com/Esl1h/yubikey-shell-toolkit) — Bash scripts for both HMAC and age-based YubiKey encryption, including multi-recipient support, setup automation, and verification.
    
*   [**passage**](https://github.com/FiloSottile/passage) — A password store (like `pass`) built on `age` instead of GPG.
    
*   [**awesome-age**](https://github.com/FiloSottile/awesome-age) — Curated list of `age` ecosystem projects.
    

### Relevant scripts from YubiKey Shell Toolkit

| Script | Purpose |
| --- | --- |
| [`yk-encrypt-file.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-encrypt-file.sh) | HMAC-based encryption (AES-256-CBC) |
| [`yk-age-encrypt.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt.sh) | Single-recipient age + YubiKey PIV encryption |
| [`yk-age-encrypt-multikeys.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt-multikeys.sh) | Multi-recipient age encryption |
| [`yk-age-decrypt-multikeys.sh`](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-decrypt-multikeys.sh) | Multi-identity age decryption |

## 9\. Conclusion

`age` represents what encryption tooling should have been all along: a single correct cipher suite, explicit key management, no footguns. Adding YubiKey via `age-plugin-yubikey` elevates it from "good software encryption" to "hardware-rooted key protection" with remarkably little ceremony — `age-plugin-yubikey --generate`, then `age -r` as usual.

For teams, the multi-recipient model is the real differentiator. Encrypting shared secrets to every team member's YubiKey (plus a recovery key) is a one-liner, not a GPG keyring management odyssey. When someone leaves the team, re-encrypt to the updated recipient list. When a YubiKey is lost, the PIN lockout protects against brute force while you rotate.

The combination is particularly compelling for:

*   **Encrypted backups** — hardware-keyed, with software recovery fallback.
    
*   **Secret management in Git** — via SOPS + age + YubiKey.
    
*   **Sensitive file exchange** — share a short `age1yubikey1...` string, not a GPG key fingerprint ceremony.
    
*   **Compliance environments** — hardware-backed key storage with touch confirmation satisfies many audit requirements.
    

If you're still wrapping files with `gpg -c` or `openssl enc`, it's time to upgrade.

* * *

### References

*   age — [github.com/FiloSottile/age](https://github.com/FiloSottile/age)
    
*   age format specification — [age-encryption.org/v1](https://age-encryption.org/v1)
    
*   age-plugin-yubikey — [github.com/str4d/age-plugin-yubikey](https://github.com/str4d/age-plugin-yubikey)
    
*   YubiKey Shell Toolkit — [github.com/Esl1h/yubikey-shell-toolkit](https://github.com/Esl1h/yubikey-shell-toolkit)
    
*   awesome-age ecosystem — [github.com/FiloSottile/awesome-age](https://github.com/FiloSottile/awesome-age)
---
title: "Encrypted DNS: The Guide — DNSCrypt, DNS Stamps, Linux Setup, Android, and the SNI Problem"
datePublished: 2026-04-08T04:25:08.643Z
cuid: cmnpjodub013b1qqe9y2b083k
slug: encrypted-dns-the-guide-dnscrypt-dns-stamps-linux-setup-android-and-the-sni-problem
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/0e689a9a-8716-4e87-94d9-042a61e0337d.png
tags: dns, security, encryption, cryptography, dns-over-https, dns-over-tls, dnscrypt, dns-over-quic

---

Every time you type an address in your browser, your device makes a DNS query. By default, that query travels in plaintext to your ISP's resolver. The same ISP that swears it doesn't sell your data but coincidentally shows you ads for things you searched minutes ago.

This is a comprehensive guide covering the encrypted DNS ecosystem: protocols, DNSCrypt, DNS Stamps, practical setup on Linux and Android, and the SNI leak that undermines everything if left unaddressed.

* * *

## Traditional DNS: the problem

DNS (Domain Name System) resolves domain names into IP addresses. It works like a phone book: you ask for the number of `esli.blog` and get the server's IP. The problem is that this "call" happens without encryption. Anyone in the path — your ISP, the coffee shop admin, an attacker on the same network — can see exactly which domains you're resolving.

The DNS protocol was designed in the 80s, when the internet was an academic environment and the concept of network privacy didn't exist. We're still paying for that design decision.

* * *

## Encrypted DNS protocols

Over time, several protocols emerged to fix this. Each with different trade-offs.

### DoT (DNS-over-TLS)

Wraps DNS queries inside a TLS connection. Uses port 853, dedicated exclusively to this purpose. Standardized in RFC 7858.

Using a dedicated port is a double-edged sword: easy to configure, but also easy to block. A network admin (or government) can simply block port 853 and DoT is gone. Android uses DoT natively via "Private DNS" since version 9, which popularized the protocol on mobile devices.

### DoH (DNS-over-HTTPS)

Wraps DNS queries inside HTTPS connections, using port 443. Standardized in RFC 8484.

The trick is that DoH traffic blends with regular HTTPS traffic. To block DoH, you'd need to block all HTTPS, which would break the web. This makes it more censorship-resistant, but also harder to monitor in corporate environments (where monitoring may be legitimate). Browsers like Firefox and Chrome support DoH natively.

### DoQ (DNS-over-QUIC)

Uses the QUIC protocol (which runs over UDP) to transport DNS queries. Standardized in RFC 9250.

More recent, it promises lower latency than DoT/DoH by eliminating the separate TLS+TCP handshake. Still limited adoption, but providers like AdGuard DNS already support it.

### DoH3 (DNS-over-HTTP/3)

Similar to DoQ, but using HTTP/3 as transport. Since HTTP/3 already uses QUIC, it inherits the same latency advantages. Providers like DNS0 (founded by NextDNS co-founders) already support it.

### ODoH (Oblivious DNS-over-HTTPS)

An anonymization layer on top of DoH. Uses an intermediary proxy (relay) so the resolver never sees the client's IP. Standardized in RFC 9230.

The concept is similar to Anonymized DNSCrypt (covered below): separate who asks from who answers.

* * *

## DNSCrypt: the protocol the industry ignored

DNSCrypt was created by Frank Denis (jedisct1), the same author of libsodium. It's not a "patch" on top of another protocol. It was designed specifically to protect DNS traffic.

What it does:

*   **Server authentication.** The client validates it's talking to the legitimate resolver using the provider's public key. No dependency on CAs (Certificate Authorities). This eliminates the entire TLS chain of trust, which is a known point of failure.
    
*   **Query and response encryption.** Uses Curve25519 for key exchange and XSalsa20-Poly1305 (or XChaCha20-Poly1305) for authenticated encryption. All based on elliptic curve cryptography, no dependency on RSA or X.509 certificate infrastructure.
    
*   **Anti-replay and anti-forgery.** Each query has a unique nonce. Forged or replayed responses are detected and discarded.
    
*   **Padding.** Queries and responses are padded with random data to hinder traffic analysis based on packet size.
    

The protocol runs over UDP (port 443 by default) and optionally TCP. It's lightweight, fast, and doesn't depend on PKI infrastructure.

The protocol is in the process of IETF standardization as an Internet-Draft (draft-denis-dprive-dnscrypt-06, updated April 2025).

### Why did the industry ignore it?

Google, Cloudflare, Apple, Microsoft — they all adopted DoH and DoT. DNSCrypt was left out of the mainstream. Not because it's technically inferior, but because there's no megacorp behind it sponsoring adoption. DoH uses HTTPS, which the entire web infrastructure already supports. DNSCrypt requires its own implementation.

Result: the DNSCrypt *protocol* has limited adoption among major providers. AdGuard DNS, Quad9, and CleanBrowsing support it. NextDNS, Cloudflare, and Google do not support the DNSCrypt protocol — only DoH and DoT.

But here's the crucial distinction: the DNSCrypt *protocol* is one thing, the `dnscrypt-proxy` *software* is another.

### dnscrypt-proxy: the Swiss army knife

`dnscrypt-proxy` is the reference client implementation. Written in Go, it runs on practically anything: Linux, macOS, Windows, Android, OpenWrt routers.

The key point is that dnscrypt-proxy is not limited to the DNSCrypt protocol. It supports:

*   DNSCrypt v2
    
*   DoH (DNS-over-HTTPS)
    
*   ODoH (Oblivious DoH)
    
*   Anonymized DNSCrypt
    

So even if a provider doesn't support the DNSCrypt protocol (like NextDNS), you can still use dnscrypt-proxy to connect via DoH. The software accepts stamps from any supported protocol.

### Anonymized DNSCrypt: DNS without tracking

Encrypted DNS solves the interception problem along the path. But it doesn't solve the problem of the resolver itself knowing who made the query. Your IP arrives at the server alongside the query.

Anonymized DNSCrypt solves this by introducing relays:

1.  The client encrypts the query for the final resolver (using the resolver's public key).
    
2.  Sends the encrypted query to a relay.
    
3.  The relay doesn't have the resolver's key, so it can't read the content. It just forwards the packet.
    
4.  The resolver receives the query, decrypts it, resolves it, and responds via the relay.
    

The relay knows the client's IP but not the query content. The resolver knows the query content but only sees the relay's IP. Neither has the complete picture.

This is fundamentally different from using Tor for DNS (which is slow and wasn't designed for it) or blindly trusting that the resolver is "no-log."

The list of available relays is in the [official repository](https://github.com/DNSCrypt/dnscrypt-resolvers).

* * *

## DNS Stamps: the QR code of DNS

Manually configuring encrypted DNS is tedious. You need to specify protocol, IP, port, hostname, path (for DoH), public key (for DNSCrypt), and optionally certificate hashes. Multiple fields, each with a different format depending on the protocol.

DNS Stamps solve this by encoding everything into a single string.

### Structure

A stamp starts with `sdns://` followed by a base64url payload. The first byte of the payload identifies the protocol:

| Byte | Protocol |
| --- | --- |
| 0x00 | Plain DNS |
| 0x01 | DNSCrypt |
| 0x02 | DoH |
| 0x03 | DoT |
| 0x04 | DoQ |
| 0x05 | ODoH target |
| 0x81 | Anonymized DNSCrypt relay |
| 0x85 | ODoH relay |

After the protocol byte, the payload contains properties (DNSSEC, no-log, no-filter), IP address, certificate hashes, hostname, path, and optionally bootstrap IPs.

### Practical example

A DoH stamp for NextDNS with ID `abc123`:

```plaintext
sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM
```

Decoded:

*   Protocol: 0x02 (DoH)
    
*   Hostname: dns.nextdns.io
    
*   Path: /abc123
    
*   No-log: yes
    
*   DNSSEC: yes
    

### Generating stamps

The official generator is at [dnscrypt.info/stamps](https://dnscrypt.info/stamps/). Select the protocol, fill in the fields, and the stamp is generated automatically. It works in reverse too: paste a stamp and it decodes the fields.

The DNS Stamps specification is in the process of IETF standardization (draft-denis-dns-stamps-00, July 2025), supporting DNSCrypt, DoH, DoT, DoQ, and ODoH.

### Who publishes stamps natively?

Not every provider makes life easy. Some publish ready-made stamps in their documentation, others require manual generation.

| Provider | Stamp in docs | Protocols |
| --- | --- | --- |
| AdGuard DNS | Yes | DNSCrypt, DoH, DoT |
| Cloudflare | Yes (listed in dnscrypt-resolvers) | DoH |
| Quad9 | Yes (listed in dnscrypt-resolvers) | DNSCrypt, DoH |
| CleanBrowsing | Yes (listed in dnscrypt-resolvers) | DNSCrypt, DoH |
| Mullvad DNS | Yes | DoH |
| Control D | Yes | DoH, DoT |
| DNS0 | Yes | DoH, DoQ |
| NextDNS | Yes (Setup > Linux > DNSCrypt) | DoH (via stamp) |

**Important note about NextDNS:** contrary to what many think, NextDNS does provide the stamp on the Setup page. Go to [my.nextdns.io](https://my.nextdns.io), access Setup Guide, select Routers, and look for the DNSCrypt section. The stamp is already generated with your config ID. It's a DoH stamp (not native DNSCrypt, since NextDNS doesn't support the DNSCrypt protocol).

To generate manually or add a device name to the path, use the generator at [dnscrypt.info/stamps](https://dnscrypt.info/stamps/) with these fields:

| Field | Value |
| --- | --- |
| Protocol | DNS-over-HTTPS |
| IP Address | (empty) |
| Host name | dns.nextdns.io |
| Hashes | (empty) |
| Path | /YOUR\_ID (e.g., /abc123) or /YOUR\_ID/device-name |
| No logs | checked |
| No filter | unchecked |
| DNSSEC | checked |

The path must start with `/`. If your ID is `abc123`, the path is `/abc123`. To identify the device in the dashboard, add a name: `/abc123/fedora-desktop`.

The complete list of resolvers with ready-made stamps is in the [official repository](https://github.com/DNSCrypt/dnscrypt-resolvers).

### Protocol comparison

| Protocol | Port | Transport | Censorship resistance | Depends on CA | Native anonymization |
| --- | --- | --- | --- | --- | --- |
| Do53 (traditional) | 53 | UDP/TCP | None | No | No |
| DoT | 853 | TLS | Low (dedicated port) | Yes | No |
| DoH | 443 | HTTPS | High (blends with HTTPS) | Yes | No (ODoH yes) |
| DoQ | 853/8853 | QUIC | Medium | Yes | No |
| DNSCrypt | 443\* | UDP/TCP | High | No (own key) | Yes (via relay) |

\* Default port 443, but configurable.

* * *

## Setting up dnscrypt-proxy on Linux

### Why dnscrypt-proxy instead of the NextDNS CLI?

NextDNS offers its own CLI (`nextdns` via `sh -c "$(curl -sL https://nextdns.io/install)"`). It works fine, it's simple. But dnscrypt-proxy offers advantages:

*   Supports multiple protocols (DNSCrypt, DoH, ODoH)
    
*   Allows switching providers without changing the stack
    
*   Support for Anonymized DNSCrypt and relays
    
*   Local cache with configurable TTL
    
*   Cloaking, local blocklists, forwarding rules
    
*   Detailed logs for debugging
    
*   Works identically on any distro
    

If you just want NextDNS working quickly, the official CLI does the job. If you want granular control or plan to use features beyond what NextDNS offers natively, dnscrypt-proxy is the way.

### Installation

**Fedora:**

```bash
sudo dnf install dnscrypt-proxy
```

The package installs the binary, the systemd service, and the configuration file at `/etc/dnscrypt-proxy/dnscrypt-proxy.toml`.

**Arch / EndeavourOS / Omarchy:**

```bash
sudo pacman -S dnscrypt-proxy
```

Same structure. Arch keeps the package updated frequently.

**Debian / Ubuntu:**

```bash
sudo apt install dnscrypt-proxy
```

Warning: versions in Debian/Ubuntu repositories may be outdated. Check with `dnscrypt-proxy --version` and compare with [releases on GitHub](https://github.com/DNSCrypt/dnscrypt-proxy/releases). If significantly behind, install manually via the GitHub binary.

### Getting the NextDNS stamp

Two methods:

**Method 1 (recommended):** go to [my.nextdns.io](https://my.nextdns.io), navigate to Setup Guide, select Linux or Routers, and copy the stamp from the DNSCrypt section. It already includes your config ID.

**Method 2 (manual):** go to [dnscrypt.info/stamps](https://dnscrypt.info/stamps/) and fill in:

| Field | Value |
| --- | --- |
| Protocol | DNS-over-HTTPS |
| IP Address | (empty) |
| Host name | dns.nextdns.io |
| Path | /YOUR\_ID (e.g., /abc123) |
| No logs | checked |
| DNSSEC | checked |

Copy the resulting stamp (`sdns://...`).

To identify the machine in the NextDNS dashboard, add the device name to the path: `/abc123/fedora-desktop`.

### Configuration

Edit the main file:

```bash
sudo vim /etc/dnscrypt-proxy/dnscrypt-proxy.toml
```

Essential settings:

```toml
# Listen address
listen_addresses = ['127.0.0.1:53', '[::1]:53']

# Force exclusive use of NextDNS
server_names = ['nextdns']

# Enable IPv6 if your network supports it
ipv6_servers = false

# Require DNSSEC, no-log, and no-filter
require_dnssec = true
require_nofilter = false
require_nolog = true

# Local cache
cache = true
cache_size = 4096
cache_min_ttl = 2400
cache_max_ttl = 86400

# Disable automatic download of public lists
# (we want ONLY NextDNS)
# Comment out or remove [sources] sections if they exist:
# [sources]
#   [sources.'public-resolvers']
#     ...
#   [sources.'relays']
#     ...

# Static server with your stamp
[static]
  [static.'nextdns']
  stamp = 'sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM'
```

Replace the stamp with the one generated using your real ID.

About the cache: the values above are intentionally aggressive. `cache_min_ttl = 2400` (40 minutes) and `cache_max_ttl = 86400` (24 hours) significantly reduce the number of queries to NextDNS. If you're on the free plan (300k queries/month), this makes a difference. Even on the paid plan, fewer queries = less latency for frequently accessed domains.

### The systemd-resolved conflict

If you're on Fedora, Ubuntu, or any distro using systemd-resolved, brace yourself: it listens on port 53 by default and will conflict with dnscrypt-proxy.

Check before anything:

```bash
ss -tlnp | grep ':53 '
```

If `systemd-resolve` shows up listening on 53, you have two options:

#### Option A: Disable systemd-resolved (recommended)

If you want full DNS control and don't need resolved for anything:

```bash
sudo systemctl disable --now systemd-resolved
sudo rm /etc/resolv.conf
echo 'nameserver 127.0.0.1' | sudo tee /etc/resolv.conf
sudo chattr +i /etc/resolv.conf
```

The `chattr +i` makes the file immutable. Without it, NetworkManager will overwrite `resolv.conf` on the first reconnection. To edit later: `sudo chattr -i /etc/resolv.conf`.

#### Option B: Coexistence (dnscrypt-proxy on alternate port)

If for some reason you need to keep resolved (corporate VPN integration, mDNS/Avahi, etc.):

In `dnscrypt-proxy.toml`:

```toml
listen_addresses = ['127.0.0.1:5353']
```

In `/etc/systemd/resolved.conf`:

```ini
[Resolve]
DNS=127.0.0.1#5353
DNSStubListener=yes
```

```bash
sudo systemctl restart systemd-resolved
```

It works, but adds an unnecessary layer. The resolved becomes a middleman that just forwards. Unless you have a concrete reason, go with Option A.

#### Note for Arch with Hyprland / Window Managers

If you don't use NetworkManager and configure networking via `iwd`, `dhcpcd`, or manually, systemd-resolved is probably not even active. Check with `systemctl status systemd-resolved`. If inactive, nothing to do. Just ensure `/etc/resolv.conf` points to `127.0.0.1`.

### Persisting resolv.conf on Fedora with NetworkManager

NetworkManager rewrites `/etc/resolv.conf` on every reconnection. Besides `chattr +i`, there's a cleaner approach: tell NetworkManager not to manage DNS.

Create `/etc/NetworkManager/conf.d/no-dns.conf`:

```ini
[main]
dns=none
```

```bash
sudo systemctl restart NetworkManager
```

From here on, NetworkManager doesn't touch DNS. Your `resolv.conf` with `nameserver 127.0.0.1` stays intact.

### Enabling and starting

```bash
sudo systemctl enable --now dnscrypt-proxy.service
```

Verify:

```bash
systemctl status dnscrypt-proxy
journalctl -u dnscrypt-proxy -f
```

In the logs, look for lines indicating the `nextdns` server was validated. Something like:

```plaintext
[nextdns] OK (DoH) - rtt: XXms
```

If you see stamp errors or server not found, review the stamp and check that no default servers are enabled competing with the static configuration.

### Validation

```bash
# Resolution via dnscrypt-proxy
dig @127.0.0.1 esli.blog

# Confirm NextDNS is active
curl -sL https://test.nextdns.io
```

The `test.nextdns.io` response should show:

*   Status: Connected (or Using NextDNS)
    
*   Protocol: DoH
    

If it returns "not using NextDNS", check:

*   The stamp (correct path with `/YOUR_ID`?)
    
*   Does `server_names` point to the correct name?
    
*   Is any default server still enabled in sources?
    
*   Does `resolv.conf` point to `127.0.0.1`?
    

Also test filtering. Access a domain that should be blocked by your NextDNS blocklists:

```bash
dig @127.0.0.1 some-blocked-site.example
```

If it returns `0.0.0.0` or `NXDOMAIN`, filtering is working.

### Advanced optional configurations

#### Anonymized DNS with relays

If you want to go further and use relays so NextDNS doesn't see your IP (via Anonymized DoH), add to `dnscrypt-proxy.toml`:

```toml
[anonymized_dns]
routes = [
    { server_name='nextdns', via=['anon-relay-1', 'anon-relay-2'] }
]
```

Available relays are in the dnscrypt-proxy resolver list. This adds latency but significantly increases privacy. If NextDNS shouldn't know which IP the queries come from, this is the way.

#### Local blocklists

dnscrypt-proxy supports its own blocklists, independent of NextDNS. In `dnscrypt-proxy.toml`:

```toml
[blocked_names]
blocked_names_file = '/etc/dnscrypt-proxy/blocked-names.txt'
```

In `blocked-names.txt`, add domains (one per line, supports wildcards):

```plaintext
*.facebook.com
*.tiktok.com
ads.*
```

This works even if NextDNS is down. It's an additional local filtering layer.

#### Query logging for debug

```toml
[query_log]
file = '/var/log/dnscrypt-proxy/query.log'
format = 'tsv'
```

Create the directory first:

```bash
sudo mkdir -p /var/log/dnscrypt-proxy
sudo chown _dnscrypt-proxy:_dnscrypt-proxy /var/log/dnscrypt-proxy
```

TSV format makes analysis easy with `awk`, `cut`, and friends. Disable in production if privacy is a priority.

* * *

## Encrypted DNS on Android

### What Android already offers (and why it's not enough)

Since Android 9, there's a "Private DNS" option in Settings > Network. It works with DoT (DNS-over-TLS) pointing to a hostname like `abc123.dns.nextdns.io` (where `abc123` is your NextDNS config ID).

Does it work? Yes. But it has limitations:

*   Only supports DoT. No DoH, no DNSCrypt, no ODoH.
    
*   No configurable local cache.
    
*   No control over which app uses which DNS.
    
*   No integrated firewall.
    
*   No configurable fallback.
    
*   If the DoT server doesn't respond, Android may fall back to the ISP's DNS silently (depending on the option: "automatic" falls back, "strict" blocks).
    
*   No logs for debugging.
    

For most people, Private DNS with NextDNS in strict mode is already a massive improvement over ISP DNS. But if you want real control, you need something more.

### InviZible Pro: the complete solution

InviZible Pro is an open-source Android app that integrates three privacy modules in a single package:

| Module | Underlying software | Function |
| --- | --- | --- |
| DNSCrypt | dnscrypt-proxy | Encrypted DNS |
| Tor | tor | Traffic anonymization |
| I2P | Purple I2P (i2pd) | I2P network access |

Available on F-Droid (unrestricted version) and Google Play. Source code at [GitHub](https://github.com/Gedsh/InviZible) under GPLv3.

#### What it does

*   **Works with and without root.** With root, redirects traffic via iptables (more efficient, no VPN overhead). Without root, creates a local VPN (traffic doesn't leave the device, it's just redirected internally to the modules).
    
*   **Integrated firewall.** Granular per-app control: who can use internet, who goes through Tor, who uses direct DNS. Replaces separate firewall apps like NetGuard.
    
*   **Kill switch.** If the module crashes, traffic is blocked. No accidental plaintext query leaks to the ISP's DNS.
    
*   **Tor bridges.** Support for obfs4, snowflake, and webtunnel bridges to circumvent censorship on restrictive networks.
    
*   **Granular configuration.** Direct access to `dnscrypt-proxy.toml`, `torrc`, and `i2pd.conf` files within the app with an integrated text editor.
    
*   **Real-time logs.** Each module has its own log tab. Debug without needing Termux or logcat.
    
*   **Independent updates.** The dnscrypt-proxy, Tor, and Purple I2P binaries are updated separately from the app.
    
*   **Stealth Mode.** DPI (Deep Packet Inspection) evasion for censorship scenarios.
    

In short: replaces VPN + firewall + DNS adblock in a single app, no ads, no tracking, no subscription for core features.

### Configuring NextDNS in InviZible Pro

#### Prerequisites

*   InviZible Pro installed (F-Droid: `pan.alexander.tordnscrypt.stable` or Play Store)
    
*   NextDNS account with a configured profile
    
*   Your config ID (visible at [my.nextdns.io](https://my.nextdns.io) in the Setup tab)
    

#### Step 1: Get the stamp

Same process as described in the DNS Stamps section. Use the stamp from the NextDNS Setup Guide or generate it manually at [dnscrypt.info/stamps](https://dnscrypt.info/stamps/).

#### Step 2: Configure via InviZible Pro interface

1.  Open InviZible Pro
    
2.  On the DNSCrypt card, tap the gear icon
    
3.  Go to DNSCrypt servers and uncheck all pre-configured servers
    
4.  Go to Own servers / DNS Stamp
    
5.  Paste the stamp
    
6.  Name the server (e.g., `nextdns`)
    
7.  Save
    

#### Step 2 (alternative): Edit dnscrypt-proxy.toml directly

If you prefer full control:

1.  Menu > Common Settings > Open text editor
    
2.  Select `dnscrypt-proxy.toml`
    
3.  Edit:
    

```toml
server_names = ['nextdns']

cache = true
cache_size = 4096
cache_min_ttl = 2400
cache_max_ttl = 86400

[static]
  [static.'nextdns']
  stamp = 'sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM'
```

4.  Save and close
    

#### Step 3: Restart and validate

1.  On the DNSCrypt card, stop and start the module
    
2.  Check DNSCrypt logs — the `nextdns` server should appear as active with OK status
    
3.  In the browser, go to [test.nextdns.io](https://test.nextdns.io)
    

If it returns "not using NextDNS": review the stamp (leading `/` in path? correct config ID?), check no other server is selected, look for error messages in logs.

#### Step 4: Additional adjustments

**DNS and Tor together (careful):** if the Tor module is active, DNS queries are routed through Tor by default before reaching NextDNS. This hides your real IP from NextDNS (may be desirable) but increases latency considerably (undesirable for daily use) and may prevent NextDNS from applying location-based rules. To disable: Common Settings > uncheck "Route DNS through Tor."

**Battery consumption:** DNSCrypt alone consumes very little. Tor consumes more. If using only the DNSCrypt module (no Tor, no I2P), battery impact is minimal (~8% for Tor + DNSCrypt 24/7 according to InviZible docs). Exclude InviZible Pro from Android's battery optimization. Check [dontkillmyapp.com](https://dontkillmyapp.com) for manufacturer-specific instructions (Samsung, Xiaomi/MIUI, and Huawei are the worst offenders).

**Rate limit on NextDNS free plan:** 300k queries/month with active filtering. After the limit, DNS continues resolving but without blocking or logs. Aggressive cache (`cache_min_ttl = 2400`) helps reduce query count. Monitor at [my.nextdns.io](https://my.nextdns.io) > Analytics.

### Alternatives to InviZible Pro

| Feature | InviZible Pro | personalDNSfilter | AdGuard | Rethink DNS | Private DNS (native) |
| --- | --- | --- | --- | --- | --- |
| DNSCrypt protocol | Yes | No | Yes | No | No |
| DoH | Yes (via stamp) | Yes | Yes | Yes | No |
| DoT | Not directly | Yes | Yes | Yes | Yes |
| DNS Stamps | Yes | No | Yes | No | No |
| Integrated Tor | Yes | No | No | No | No |
| Integrated I2P | Yes | No | No | No | No |
| Per-app firewall | Yes | No | Yes | Yes | No |
| Open-source | Yes (GPLv3) | Yes | Partial | Yes | N/A |
| Root required | No (local VPN) | No | No | No | No |
| F-Droid | Yes | Yes | No | Yes | N/A |
| Cost | Free | Free | Paid | Free | Free |

* * *

## The SNI leak: the Achilles' heel of secure DNS

You configured DNSCrypt. Enabled DoH. Replaced your ISP's DNS with a privacy-respecting resolver. Did everything right. And yet, your ISP can still know exactly which sites you visit.

Welcome to the SNI problem.

Encrypting DNS queries is the first step toward reclaiming network privacy. But DNS is only half the equation. The other half happens during the TLS handshake, and that's where the leak lives that almost nobody discusses.

### What SNI is and why it exists

SNI stands for Server Name Indication. It's a field sent by the client (your browser) at the beginning of a TLS connection, *before* any content encryption.

The reason is practical: when multiple sites share the same IP (extremely common in shared hosting, CDNs, and cloud platforms), the server needs to know which TLS certificate to present. The client provides this by sending the destination hostname in the ClientHello — the first message of the TLS handshake. In plaintext.

This means any entity positioned between you and the destination server (ISP, corporate network operator, government, airport Wi-Fi) can read the SNI field and know exactly which domain you're accessing.

In short: HTTPS protects the content. SNI gives away the destination.

### What the attacker sees (and what they do with it)

With access to SNI, a network observer can:

**Build a complete browsing profile.** Even without seeing page content, knowing that someone accessed `medicalconsult.com`, `jobboard.com`, and `laborlawyer.com` on the same day already tells a story.

**Selectively block sites.** South Korea did exactly this in 2019: after users started circumventing DNS blocks, the government instructed ISPs to inspect the SNI field in TLS handshakes to drop connections to prohibited domains. Encrypted DNS became useless. SNI was the new control point.

**Censor without breaking encryption.** The ISP doesn't need to decrypt HTTPS traffic. It just reads the SNI, compares it against a list, and drops the SYN packet or sends a RST. Trivial to implement in DPI (Deep Packet Inspection) and invisible to anyone not monitoring.

**Correlate traffic with identity.** Combining SNI with source IP and timestamps, it's possible to link access patterns to a specific subscriber. This is done systematically in authoritarian regimes and, as demonstrated in practice, by ISPs in democracies that retain logs by legal obligation.

Encrypted DNS solves the question "which IP does this domain point to?" SNI leaks the answer: "which domain am I visiting?" Two different metadata channels. Protecting one and ignoring the other is like locking the front door and leaving the window wide open.

### The evolution: from ESNI to ECH

The technical community didn't ignore the problem. The first attempt was **ESNI (Encrypted Server Name Indication)**, proposed in 2018 as an experimental TLS 1.3 extension. The idea was simple: encrypt the SNI field using a public key available via DNS.

ESNI worked as a proof of concept but had practical limitations. Encryption was applied only to the SNI, leaving other ClientHello fields (like ALPN, cipher suite list, and extensions) still visible. This allowed observers to infer destination information even without the explicit SNI. ESNI also depended on specific DNS TXT records, making key distribution fragile and hard to scale.

The answer was **ECH (Encrypted Client Hello)**, which superseded ESNI and is in the final stages of IETF standardization (draft 25 at the time of writing). The fundamental difference: instead of encrypting only the SNI, ECH encrypts the *entire* ClientHello.

### How ECH works

ECH splits the ClientHello into two parts:

**Outer ClientHello (public):** Contains generic information like TLS version, cipher suites, and a generic SNI. In Cloudflare's case, all ECH connections show `cloudflare-ech.com` as the external SNI. The network observer sees only that you're accessing "something on Cloudflare," along with millions of other users.

**Inner ClientHello (encrypted):** Contains the real SNI, sensitive extensions, and all information that was previously sent in plaintext. This block is encrypted using a public key (ECHConfig) that the browser obtains via HTTPS/SVCB DNS records.

For the ISP or any intermediary, the TLS connection appears to be to `cloudflare-ech.com`. The real destination is only revealed inside the encrypted tunnel, accessible only to the destination server (or the CDN hosting it).

Think of it this way: without ECH, it's like sending a letter with the recipient's name written on the envelope. With ECH, the envelope shows "Cloudflare" and the real name is inside, sealed.

### The encrypted DNS dependency

Here's the point that ties everything together: ECH depends on DNS records to function. The browser needs to fetch the domain's HTTPS/SVCB record to obtain the ECHConfig (the encryption public key). If that DNS query is made in plaintext, an attacker can simply remove or modify the record to prevent ECH.

Without DoH, DoT, or DNSCrypt, ECH can be sabotaged before it even starts.

This is why the DNSCrypt setup isn't "just about DNS." It's the foundation for ECH to work. One doesn't replace the other. They complement each other.

The complete chain for real privacy:

1.  **Encrypted DNS** (DNSCrypt, DoH, DoT) → prevents the ISP from knowing which domains you resolve
    
2.  **ECH** → prevents the ISP from knowing which domains you access via TLS
    
3.  **HTTPS** → prevents the ISP from reading the communication content
    

Remove any of these layers and protection degrades. All three together eliminate browsing metadata visible to network observers.

### Current support: who has implemented it

#### Browsers

| Browser | ECH enabled by default | Since |
| --- | --- | --- |
| Firefox | Yes | Version 119 (Oct 2023) |
| Chrome | Yes | Version 117+ (gradual rollout) |
| Edge | Yes | Follows Chromium |
| Brave | Yes | Follows Chromium |
| Safari | Yes | macOS Sequoia / iOS 18+ |
| Tor Browser | Yes | Based on Firefox ESR |
| Mullvad Browser | Yes | Based on Firefox |

Firefox was the first to enable ECH by default, and requires DoH to be active for ECH to work (which makes technical sense). Chrome and derivatives do progressive rollout.

#### Servers/CDNs

Cloudflare is the main driver of ECH adoption on the server side. It activated ECH across all plans, including the free tier (where it cannot be disabled). This is relevant because a significant portion of internet sites use Cloudflare as CDN/reverse proxy.

2026 data: approximately 4.2% of the top 100K sites and 9.2% of the top 1M support ECH. Seems low, but considering that Cloudflare alone covers a huge slice of web traffic, the practical impact is larger than the numbers suggest.

Other CDNs and hosting providers are still evaluating. Without server support, ECH simply isn't negotiated and the connection falls back to the old behavior (plaintext SNI).

### How to verify and configure on Linux

#### Firefox

ECH is already enabled by default in Firefox 119+.

To confirm:

1.  Go to `about:config`
    
2.  Search `network.dns.echconfig.enabled` → should be `true`
    
3.  Search `network.dns.http3_echconfig.enabled` → should be `true`
    

ECH in Firefox depends on DoH being active. Check in: Settings → Privacy & Security → DNS over HTTPS.

Enable with Maximum Protection and select a provider (Cloudflare, NextDNS, or custom).

#### Chromium/Brave/Edge

In Chrome and derivatives, ECH was controlled by a flag until version 117:

```plaintext
chrome://flags/#encrypted-client-hello
```

Currently, if DoH is active and the server supports ECH, Chrome negotiates automatically and there's no option to disable it.

Again, as with Firefox, ECH depends on Secure DNS being active. Check in: Settings → Privacy and Security → Security → Use secure DNS.

#### Checking sites with dig

```bash
dig HTTPS crypto.cloudflare.com +short
```

If it returns a field with `ech=`, the site supports ECH.

The presence of ECHConfig in the HTTPS-type DNS record is a necessary and sufficient condition to confirm server-side ECH support. The browser negotiates if it wants to — but the support is on the server.

#### Validation

To verify ECH is working on your connection:

*   Visit [crypto.cloudflare.com/cdn-cgi/trace](https://crypto.cloudflare.com/cdn-cgi/trace) and look for the field `sni=encrypted`
    
*   Visit [defo.ie/ech-check.php](https://defo.ie/ech-check.php) for a more detailed test
    

### What ECH doesn't solve

ECH encrypts the ClientHello. That's significant, but it's not the end of the story.

**Destination IP.** ECH hides the domain, not the IP. If a server hosts a single site, the IP already reveals the destination. ECH is most effective when many sites share the same infrastructure (as in CDNs).

**Traffic analysis.** Data volume, access patterns, and timing are still observable. Traffic fingerprinting techniques can infer the visited site even without SNI.

**Unencrypted DNS.** If DNS leaks the domain before ECH kicks in, the protection is null. ECH needs DoH/DoT/DNSCrypt to work as designed.

**Servers without support.** If the site you're accessing doesn't support ECH, the SNI remains in plaintext. Protection depends on server-side adoption.

For more complete coverage, combine ECH with a VPN. A VPN hides the destination IP and encapsulates all traffic, while ECH protects TLS metadata. VPN + ECH + encrypted DNS is the most robust combination available today without using Tor.

### ECH as an attack tool

Worth noting the other side: ECH is also useful for attackers. In 2024, researchers at NTT Security Holdings identified malware (dubbed ECHidna) that used ECH to hide communication with command and control (C2) servers. The malicious traffic masqueraded as legitimate connections to CDN infrastructure, making detection by traditional firewalls ineffective.

This isn't an argument against ECH — on the contrary: it shows the technology and cryptography work. What changes is the intent of use.

### Hiding SNI from your ISP and attackers

**zapret** and **SpoofDPI** are tools that operate at the network layer to prevent Deep Packet Inspection (DPI) systems from reading the SNI field in the TLS ClientHello.

On Linux, zapret uses nfqueue (via iptables/nftables) to intercept outgoing packets before they leave the machine — upon capturing the ClientHello, it applies techniques like TCP fragmentation (split2), TTL manipulation, and fake packet insertion, causing the ISP's DPI to receive fragments out of order or with invalid data while the destination server reassembles correctly.

SpoofDPI acts as a local HTTP/HTTPS proxy: the browser connects to it, and it establishes the real connection with the destination by fragmenting the first TLS packet at the application level — simpler to configure (just point the browser proxy to `127.0.0.1:8080`), but less flexible than zapret. Neither encrypts nor removes the SNI — they just make passive inspection unfeasible by breaking the heuristics of DPI equipment.

On Android, the main options are **InviZible Pro**, **ByeDPI Android**, and **DPI Tunnel** — all work without root by creating a local VPN on the device that intercepts outgoing traffic and applies fragmentation/desynchronization techniques on the TLS ClientHello before it reaches the ISP, preventing passive DPI from reading the SNI.

InviZible Pro is the most complete, integrating DNSCrypt, Tor, and anti-DPI modules in a single app (with root, it injects direct iptables rules, dispensing with the local VPN and avoiding conflicts). It includes an option to spoof the SNI.

ByeDPI Android is the simplest and most focused: install, choose the desync method (split/disorder), and activate — no extra configuration. As on Linux, none of them encrypt or remove the SNI; they just fragment the TCP packet so that passive inspection equipment can't reassemble and read the field in real time.

* * *

## Wrapping up

The complete setup: dnscrypt-proxy on Linux (Fedora, Arch, Debian), InviZible Pro on Android, same NextDNS stamp, same config ID, same blocklists, unified analytics in the NextDNS dashboard. Three environments, one resolver, encrypted queries everywhere.

But remember: encrypted DNS alone is not enough. The SNI leak exposes your browsing destinations during TLS handshakes. ECH is the fix, but adoption is still in progress. Browser support is already universal. Cloudflare pushes server-side adoption. The pieces are falling into place.

What you control directly:

1.  **Enable DoH/DoT/DNSCrypt** — the prerequisite for ECH to function
    
2.  **Validate ECH is active** in your browser (`about:config`, flags, online tests)
    
3.  **Disable WebRTC** — not SNI directly, but WebRTC leaks your real IP even with VPN/ECH, enabling correlation
    

Without ECH, SNI is the last open metadata channel for network observers. With ECH, that channel closes. The ISP stops knowing which site you visit — not just what you do inside it.

If your ISP could charge for every DNS query it snoops on, it would have funded a space program by now. Encrypt your queries. And while you're at it, push for ECH adoption.

* * *

## References

*   [DNSCrypt project](https://dnscrypt.info/)
    
*   [DNS Stamps specification](https://dnscrypt.info/stamps-specifications/)
    
*   [dnscrypt-proxy on GitHub](https://github.com/DNSCrypt/dnscrypt-proxy)
    
*   [dnscrypt-resolvers](https://github.com/DNSCrypt/dnscrypt-resolvers)
    
*   [InviZible Pro on GitHub](https://github.com/Gedsh/InviZible)
    
*   [IETF Draft: DNSCrypt](https://www.ietf.org/archive/id/draft-denis-dprive-dnscrypt-06.html)
    
*   [IETF Draft: DNS Stamps](https://www.ietf.org/archive/id/draft-denis-dns-stamps-00.html)
    
*   [IETF Draft: ECH](https://datatracker.ietf.org/doc/draft-ietf-tls-esni/)
    
*   [RFC 8744 — Issues and Requirements for SNI Encryption in TLS](https://www.rfc-editor.org/rfc/rfc8744)
    
*   [Cloudflare: Announcing Encrypted Client Hello](https://blog.cloudflare.com/announcing-encrypted-client-hello/)
    
*   [Cloudflare: DNS over TLS](https://www.cloudflare.com/learning/dns/dns-over-tls/)
    
*   [CDT — Encrypted Client Hello: Closing the SNI Metadata Gap](https://cdt.org/)
    
*   [ECH check tool](https://defo.ie/ech-check.php)
    
*   [Cloudflare trace](https://crypto.cloudflare.com/cdn-cgi/trace)
    
*   [NextDNS dashboard](https://my.nextdns.io)
    
*   [NextDNS test](https://test.nextdns.io)
    
*   [Don't Kill My App](https://dontkillmyapp.com)
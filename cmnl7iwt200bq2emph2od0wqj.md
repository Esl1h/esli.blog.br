---
title: "YubiKey na Programação: Guia Completo para SRE, DevOps e Sysadmin"
datePublished: 2026-04-05T03:33:53.177Z
cuid: cmnl7iwt200bq2emph2od0wqj
slug: yubikey-na-programa-o-guia-completo-para-sre-devops-e-sysadmin
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/7a4ce7cc-c61e-4be6-9b3b-75b8160dc057.png
tags: security, bash, encryption, coding, sre, cryptography, shell-script, yubikey, yubikeys

---

## O que a YubiKey oferece para programadores

Antes de sair escrevendo código, é fundamental entender os protocolos disponíveis no hardware. A YubiKey não é um pendrive de senhas é um HSM (Hardware Security Module) de bolso que expõe múltiplas interfaces:

| Protocolo | Interface | Uso Típico |
| --- | --- | --- |
| **OTP (Yubico OTP / HOTP / TOTP)** | HID (teclado) | 2FA clássico |
| **FIDO2 / WebAuthn** | HID | Login sem senha |
| **U2F** | HID | 2FA em apps web |
| **PIV (Smart Card)** | CCID | Criptografia, SSH, certificados |
| **OpenPGP** | CCID | GPG, criptografia de arquivos |
| **HMAC-SHA1 Challenge-Response** | HID | Derivação de chaves, disk encryption |
| **OATH (TOTP/HOTP)** | CCID | Geração de OTPs via app |

Para o contexto de SRE/DevOps, os mais relevantes são: **PIV**, **OpenPGP**, **HMAC-CR** e **FIDO2**.  
A chave privada nunca sai do chip, o app envia dados para a YubiKey processar; ela devolve o resultado. Isso é o que diferencia um HSM de um arquivo .pem no disco.

![](https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/188a8ff4-ad62-48ac-8036-fd288f28c640.png align="center")

## Shell Script / Bash

O shell não fala diretamente com a YubiKey, mas os utilitários de linha de comando da Yubico fazem isso muito bem. Para um SRE, isso é suficiente para 80% dos casos de uso.

Dependências:

```shell
# Arch
sudo pacman -S yubikey-manager ykpers libfido2 pcsc-tools opensc

# Fedora
sudo dnf install yubikey-manager ykpers libfido2 pcsc-lite opensc

# Verificar se a key está presente
ykman info
```

### HMAC-SHA1 Challenge-Response para derivar chave de criptografia

O slot 2 da YubiKey pode ser configurado para HMAC-CR. Você envia um challenge (até 64 bytes), ela retorna um HMAC de 20 bytes. Isso vira a base de uma chave de criptografia.

encrypt:

```shell
#!/usr/bin/env bash
# yk-encrypt.sh — Criptografa arquivo usando YubiKey como fator de chave
set -euo pipefail

FILE="${1:?Uso: $0 <arquivo>}"
SLOT=2

# Gera challenge aleatório e salva junto ao arquivo cifrado
CHALLENGE=$(openssl rand -hex 32)
echo "[*] Enviando challenge para YubiKey (slot $SLOT)..."

# ykchalresp retorna o HMAC em hex
HMAC=$(ykchalresp -2 "$CHALLENGE" 2>/dev/null) || {
    echo "[!] YubiKey não respondeu. Inserida e configurada?" >&2
    exit 1
}

# Deriva chave AES-256 via PBKDF2 (challenge + HMAC como material)
KEY=$(echo -n "${CHALLENGE}${HMAC}" | openssl dgst -sha256 -binary | xxd -p -c 256)

# Cifra com AES-256-GCM
openssl enc -aes-256-cbc -pbkdf2 -iter 600000 \
    -k "$KEY" \
    -in "$FILE" \
    -out "${FILE}.yk.enc"

# Salva o challenge (sem o HMAC — sem a key física, não abre)
echo "$CHALLENGE" > "${FILE}.yk.challenge"

echo "[+] Arquivo cifrado: ${FILE}.yk.enc"
echo "[+] Challenge salvo: ${FILE}.yk.challenge"
echo "[!] Sem a YubiKey, o arquivo não pode ser decifrado."
```

E depois, decrypt:

```shell
#!/usr/bin/env bash
# yk-decrypt.sh
set -euo pipefail

ENC_FILE="${1:?Uso: $0 <arquivo.yk.enc>}"
CHALLENGE_FILE="${ENC_FILE%.enc}.challenge"
SLOT=2

CHALLENGE=$(cat "$CHALLENGE_FILE")
echo "[*] Aguardando YubiKey..."
HMAC=$(ykchalresp -2 "$CHALLENGE" 2>/dev/null) || { echo "[!] Falha na YubiKey" >&2; exit 1; }

KEY=$(echo -n "${CHALLENGE}${HMAC}" | openssl dgst -sha256 -binary | xxd -p -c 256)

openssl enc -d -aes-256-cbc -pbkdf2 -iter 600000 \
    -k "$KEY" \
    -in "$ENC_FILE" \
    -out "${ENC_FILE%.yk.enc}.decrypted"

echo "[+] Decifrado: ${ENC_FILE%.yk.enc}.decrypted"
```

### SSH com YubiKey (PIV ou FIDO2)

```shell
# Gera chave FIDO2 residente na YubiKey para SSH
ssh-keygen -t ed25519-sk -O resident -O verify-required \
    -C "esli@infra-prod" -f ~/.ssh/id_ed25519_yk

# A chave privada fica no hardware. O arquivo local é só um "handle".
# Para usar em outro host sem o arquivo:
ssh-keygen -K  # exporta handles das chaves residentes

# Adiciona ao ssh-agent com confirmação de toque obrigatória
ssh-add -K ~/.ssh/id_ed25519_yk
```

### GPG com YubiKey para criptografia de arquivos/secrets

```shell
# Verifica se a YubiKey está como cartão GPG
gpg --card-status

# Cifra arquivo para sua própria chave (que está na YubiKey)
gpg --encrypt --recipient esli@esli.blog.br segredo.txt

# Decifra — YubiKey faz a operação, exige PIN
gpg --decrypt segredo.txt.gpg > segredo.txt

# Uso prático: secrets em repositório Git
echo "DB_PASSWORD=hunter2" | gpg --encrypt -r esli@esli.blog.br --armor > .secrets.gpg
# No deploy:
gpg --decrypt .secrets.gpg | source /dev/stdin
```

## Python

Python tem bindings maduros para praticamente tudo relacionado à YubiKey.

```shell
pip install yubikey-manager cryptography fido2
# yubikey-manager expõe a API do ykman como biblioteca
```

## Go

Go é a linguagem de eleição para ferramentas de infraestrutura. O pacote `piv-go` da Google é o estado da arte para PIV.

```shell
go get github.com/go-piv/piv-go/piv
go get golang.org/x/crypto
```

## Rust

Rust tem o ecossistema mais completo e seguro para trabalhar com YubiKey. O crate `yubikey` é mantido ativamente e cobre PIV completamente.

Dependências (cargo.toml):

```shell
[dependencies]
yubikey = { version = "0.8", features = ["untested"] }
age = "0.10"
aes-gcm = "0.10"
rand = "0.8"
sha2 = "0.10"
rsa = { version = "0.9", features = ["sha2"] }
```

### `age` + YubiKey (a forma moderna)

O projeto `age` é o sucessor moderno do GPG para criptografia de arquivos. O plugin `age-plugin-yubikey` integra nativamente com PIV

`cargo install age-plugin-yubikey`

## Aplicações reais da YubiKey no dia a dia de SRE/DevOps

No mundo real, YubiKey deixa de ser “mais um fator de autenticação” e vira um ponto de controle físico sobre ações críticas. Em ambientes de infraestrutura, isso muda completamente o modelo de ameaça: não basta comprometer credenciais — o atacante precisa também do dispositivo físico.

Um uso direto e frequente é na proteção de acessos privilegiados via SSH. Em vez de depender de chaves privadas armazenadas no filesystem (mesmo com permissões corretas), a autenticação passa a exigir presença física e interação do operador. Isso elimina uma classe inteira de ataques baseada em exfiltração de chave (ex: malware, backup comprometido, vazamento de home directory). Em ambientes com bastion hosts ou acesso a clusters Kubernetes sensíveis, isso reduz drasticamente o risco de movimento lateral silencioso.

Outro cenário recorrente é a proteção de secrets em pipelines e automações. Em vez de armazenar credenciais em texto claro ou mesmo em vaults mal configurados, a YubiKey pode ser utilizada como raiz de confiança para descriptografar dados apenas no momento da execução. Isso é particularmente útil em pipelines semi-manuais (deploys controlados, runbooks, operações de emergência), onde a presença humana já é esperada. Na prática, você cria um modelo onde o segredo só “existe” em memória durante alguns segundos e depende de um fator físico para ser acessado.

Em times que lidam com compliance (financeiro, saúde, governo), a YubiKey também entra como mecanismo de **não repúdio operacional**. Como cada dispositivo possui um identificador único, é possível atrelar ações críticas — como rotação de chaves, alteração de configuração em produção ou acesso a dados sensíveis — a um hardware específico. Isso permite auditoria mais forte: não apenas “qual usuário fez”, mas “qual dispositivo físico foi utilizado”. Em incidentes, isso reduz ambiguidades.

Para proteção de código e supply chain, o uso com GPG ou assinatura baseada em hardware garante que commit, tags e artefatos só possam ser assinados com presença física. Isso impede que um atacante que comprometeu um ambiente CI consiga forjar releases assinadas, um problema cada vez mais relevante em ataques à cadeia de suprimentos. O mesmo raciocínio se aplica a imagens de contêiner e pacotes distribuídos internamente.

Outro caso prático é o uso como fator de desbloqueio para ferramentas internas. Painéis administrativos, portais de SRE, ou até CLIs sensíveis podem exigir autenticação via FIDO2 antes de executar operações destrutivas (ex: “drain cluster”, “delete namespace”, “rotate CA”). Isso adiciona um “atrito intencional” exatamente onde ele deve existir: nas ações irreversíveis. Não é sobre dificultar o trabalho — é sobre impedir erros ou abusos silenciosos.

Em ambientes distribuídos ou com acesso remoto, a YubiKey também atua como mitigação contra comprometimento de sessão. Mesmo que uma máquina seja invadida ou um terminal esteja aberto, a ausência do dispositivo físico impede novas autenticações e operações críticas. Isso limita o impacto de ataques e força o adversário a agir dentro de uma janela muito mais restrita.

Por fim, há o uso como âncora de identidade pessoal no ecossistema técnico. A mesma YubiKey pode ser utilizada para login em serviços, assinatura de código, acesso SSH e descriptografia de dados. Isso consolida identidade, reduz superfície de ataque e simplifica o modelo mental: em vez de dezenas de segredos distribuídos, você tem um único ponto de controle forte — que, convenientemente, não pode ser copiado.

## Referência Rápida: Qual Protocolo Usar?

| Necessidade | Protocolo | Ferramenta/Lib |
| --- | --- | --- |
| Criptografia de arquivos | PIV (RSA/ECDH) ou OpenPGP | `age-plugin-yubikey`, `gpg`, `piv-go`, `yubikey` (Rust) |
| SSH sem senha | FIDO2 (ed25519-sk) | `ssh-keygen -t ed25519-sk` |
| 2FA em app web | FIDO2/WebAuthn | `fido2` (Python), `webauthn` (Go/Rust) |
| Derivação de chave (disk, vault) | HMAC-SHA1 CR | `ykchalresp`, `libykpers`, `yubikey-manager` |
| Assinatura de código/commits | OpenPGP | `gpg --card-status` |
| Autenticação em sistemas legados | OTP (HOTP/TOTP) | `ykman oath` |
| Acesso privilegiado com PIN | PIV | `piv-go`, `yubikey` (Rust), `opensc` |

Considerações Finais

A YubiKey não é bala de prata, é uma camada de segurança física que complementa boas práticas. Alguns pontos que o SRE pragmático precisa ter em mente:

Backup: Sempre tenha uma segunda YubiKey configurada. Perder a única key é uma experiência formativa que você não quer ter.

PIN vs Touch: Configure `touch-policy=always` para operações críticas. O toque físico é o que diferencia a YubiKey de um arquivo de chave no disco.

Forwarding remoto: Para servidores remotos, usbip permite forwarding de USB via rede. Alternativa: use o SSH agent forwarding com ssh-agent + FIDO2.

Auditoria: O serial da YubiKey é único e pode ser utilizado como identificador de operador em logs de auditoria.

Revogação: Tenha um processo documentado para revogar e substituir chaves quando uma YubiKey for perdida ou comprometida.

A premissa é simples: o que está no hardware não pode ser exfiltrado por malware. Um atacante que comprometeu sua máquina pode roubar sua sessão ativa, mas não pode abrir uma nova conexão SSH, decifrar novos arquivos ou assinar novos commits sem o token físico. Isso reduz drasticamente a janela de exposição.

*Referências:*

*   [Yubico Developers](https://developers.yubico.com/)
    
*   [piv-go (Google)](https://github.com/go-piv/piv-go)
    
*   [rust-yubikey (Fortanix)](https://github.com/iqlusioninc/yubikey.rs)
    
*   [age-plugin-yubikey](https://github.com/str4d/age-plugin-yubikey)
    
*   [yubikey-manager (Python)](https://github.com/Yubico/yubikey-manager)
    
*   [libfido2](https://github.com/Yubico/libfido2)
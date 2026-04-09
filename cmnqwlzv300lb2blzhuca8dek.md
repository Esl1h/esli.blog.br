---
title: "age: criptografia de arquivos simples, moderna e segura"
datePublished: 2026-04-09T03:14:58.386Z
cuid: cmnqwlzv300lb2blzhuca8dek
slug: age-criptografia-de-arquivos-simples-moderna-e-segura
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/d2447394-d7f7-4397-9299-f93c9ec1880f.svg
tags: go, security, rust, cryptography, openssl, gpg

---

Se você já usou GPG para cifrar um arquivo e sentiu que precisava de um diploma em criptografia aplicada só para lembrar quais flags usar, você não está sozinho. A experiência do GnuPG é tão sofrida que já virou quase um rito de passagem no mundo Linux: gerar chaves RSA de 4096 bits, lidar com keyrings, trust models, subkeys, revocation certificates, ...

Foi exatamente essa dor que motivou a criação do **age**.

## O que é o age

O age (pronuncia-se com "g" duro, como o italiano "aghe" — e sim, sempre em minúsculas) é uma ferramenta de criptografia de arquivos criada por [Filippo Valsorda](https://filippo.io/) e [Ben Cox](https://github.com/benjojo). Filippo não é um nome qualquer: ele foi o responsável pela equipe de criptografia do Go na Google e mantém atualmente a infraestrutura criptográfica do ecossistema Go.

O age é três coisas ao mesmo tempo:

*   **Uma CLI** (`age` e `age-keygen`) para cifrar e decifrar arquivos
    
*   **Um formato de arquivo** com especificação aberta em [age-encryption.org/v1](https://age-encryption.org/v1)
    
*   **Uma biblioteca Go** (`filippo.io/age`) para integração em aplicações
    

A filosofia de design é simples: chaves pequenas e explícitas, zero opções de configuração, composabilidade no estilo UNIX. Não escolhe algoritmo, não configura nada, só cifra e decifra. Ponto.

## Por que o age existe

O Filippo resumiu a motivação de forma direta: o age foi criado para substituir os casos de uso em que as pessoas usavam `gpg -c` (criptografia simétrica com passphrase) e `gpg -e` (criptografia com chave pública). A premissa é que, quando alguém chega no age, já sabe qual problema quer resolver — e quer uma ferramenta que poupe a dor de lidar com opções criptográficas legadas que ninguém deveria precisar entender.

O GPG/PGP carrega décadas de bagagem: algoritmos deprecados, negociação de parâmetros criptográficos (tamanho de chave, tipo de cifra, rounds), keyrings complexos, web of trust, e uma UX que parece ter sido projetada por um comitê em 1998, e na verdade, foi! O age joga tudo isso fora e começa do zero, com primitivas modernas e sem concessões retrocompatíveis.

Uma decisão deliberada e controversa: **o age não suporta assinaturas digitais**. A justificativa é que signing adiciona uma dimensão inteira de complexidade na UX, no gerenciamento de chaves e no design criptográfico de um formato streamable. O age cuida de integridade e confidencialidade, não de autenticação de remetente. Se você precisa de assinaturas, use [minisign](https://jedisct1.github.io/minisign/) ou [sigstore](https://www.sigstore.dev/) ferramentas especializadas para isso.

## Como funciona por baixo

A criptografia do age é construída sobre primitivas bem estabelecidas:

**Cifra de payload:** ChaCha20-Poly1305 — a mesma AEAD usada no WireGuard, TLS 1.3 e SSH. Em hardware sem AES-NI (como muitos ARM), ChaCha20 tende a ser mais rápido que AES-GCM. O payload é dividido em chunks de 64 KiB, cada um cifrado individualmente usando um esquema STREAM (variante do que é usado no Tink e Miscreant), o que permite cifrar e decifrar de forma streaming sem carregar o arquivo inteiro em memória.

**Troca de chaves (X25519):** curva elíptica Curve25519 via Diffie-Hellman. As chaves públicas começam com `age1...` e são codificadas em Bech32 — curtas o suficiente para colar numa mensagem ou num arquivo de configuração. Uma chave pública age tem ~62 caracteres. Compare com uma chave pública RSA-4096 do GPG.

**Derivação de chaves:** HKDF-SHA-256 é usada para derivar as chaves de wrap e payload a partir do segredo compartilhado e da file key.

**Passphrase (modo simétrico):** scrypt para derivação, com work factor configurado automaticamente.

**Pós-quântico (v1.3.0+):** suporte nativo a recipients híbridos ML-KEM-768 + X25519 (baseado em HPKE). Chaves começam com `age1pq1...`. Isso protege contra ataques de computadores quânticos futuros no modelo "harvest now, decrypt later".

O formato é estritamente definido: não há negociação de algoritmos, não há opções de configuração criptográfica. Uma cifra, uma curva, um KDF. Ponto. Isso elimina a classe inteira de vulnerabilidades que surgem de downgrade attacks e de escolhas ruins de parâmetros.

## Uso prático

### Geração de chaves

```bash
# Chave X25519 padrão
age-keygen -o key.txt
# Public key: age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p

# Chave pós-quântica
age-keygen -pq -o key-pq.txt
```

### Cifrar e decifrar

```bash
# Cifrar com chave pública
age -r age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p secrets.tar.gz > secrets.tar.gz.age

# Cifrar com passphrase
age -p secrets.txt > secrets.txt.age

# Decifrar
age -d -i key.txt secrets.tar.gz.age > secrets.tar.gz
```

### Composabilidade UNIX

```bash
# Cifrar um backup e enviar via SSH
tar czf - ~/docs | age -r age1... | ssh server "cat > backup.tar.gz.age"

# Cifrar para múltiplos recipients
age -r age1alice... -r age1bob... arquivo.txt > arquivo.txt.age

# Cifrar para as chaves SSH públicas de alguém no GitHub
curl https://github.com/usuario.keys | age -R - arquivo.txt > arquivo.txt.age
```

### Suporte a chaves SSH

O age aceita cifrar diretamente para chaves `ssh-ed25519` e `ssh-rsa`:

```bash
age -R ~/.ssh/id_ed25519.pub arquivo.txt > arquivo.txt.age
age -d -i ~/.ssh/id_ed25519 arquivo.txt.age > arquivo.txt
```

Útil como atalho, mas as chaves SSH não são ideais para criptografia de longo prazo, por serem tipicamente revogáveis e pensadas para autenticação, não para cifrar dados que vão ficar armazenados por anos.

### Inspecionar arquivos cifrados

Desde a v1.3.0, o `age-inspect` mostra metadados sem decifrar:

```bash
age-inspect secrets.age
# secrets.age is an age file, version "age-encryption.org/v1".
# This file is encrypted to the following recipient types:
#   - "mlkem768x25519"
# This file uses post-quantum encryption.
```

## Instalação

O age está empacotado em praticamente toda distro relevante:

```bash
# Arch
pacman -S age

# Fedora
dnf install age

# Debian 12+ / Ubuntu 22.04+
apt install age

# macOS
brew install age

# Windows
winget install --id FiloSottile.age

# Via Go
go install filippo.io/age/cmd/...@latest
```

## age vs. GPG vs. OpenSSL vs. o resto

Vamos ao que interessa: como o age se compara com as alternativas.

### GPG/PGP

O elefante na sala. O GPG é o canivete suíço da criptografia — faz tudo, e faz tudo de forma dolorosamente complicada. Chaves enormes, keyrings frágeis, trust models que ninguém entende, algoritmos legados habilitados por padrão, UX hostil. O age resolve especificamente o caso de uso de criptografia de arquivos, descartando tudo que não é essencial.

O que o GPG ainda faz e o age não: assinaturas digitais, web of trust, integração nativa com clientes de e-mail, gerenciamento de identidades complexas. Se você precisa assinar commits (embora SSH também faça isso agora), assinar pacotes .deb ou manter uma identidade PGP para comunicação, o GPG ainda tem seu lugar.

O age foi projetado explicitamente para substituir `gpg -c` e `gpg -e`, não o GPG inteiro.

### OpenSSL

O OpenSSL é uma biblioteca, não uma ferramenta de usuário final. Sim, você *pode* cifrar arquivos com `openssl enc`, mas está operando numa abstração muito baixa: precisa escolher o algoritmo, o modo de operação, lidar com IVs, derivação de chave, e o formato de saída não é padronizado. Um erro sutil e você compromete toda a segurança. O age abstrai tudo isso de forma segura por padrão.

### minisign / signify

Ferramentas especializadas em assinaturas digitais. Não fazem criptografia. Complementam o age — use age para cifrar, minisign para assinar.

### Vault / SOPS / Sealed Secrets

São ferramentas de gerenciamento de segredos, não de criptografia de arquivos de uso geral. Mas é aqui que o age brilha no ecossistema DevOps: o **Mozilla SOPS** suporta age como backend de criptografia, substituindo GPG com uma experiência infinitamente mais simples. O Flux CD recomenda explicitamente o uso de age em vez de PGP para gerenciar secrets no Kubernetes. O chezmoi (gerenciador de dotfiles) também tem suporte nativo.

### rage (Rust)

O [rage](https://github.com/str4d/rage) é uma implementação alternativa em Rust, totalmente interoperável com o age. Mesmo formato, mesmas chaves. Se você prefere binários Rust ou precisa integrar com o ecossistema Rust, é uma opção de primeira classe.

### Typage (TypeScript)

O [typage](https://github.com/FiloSottile/typage) é a implementação TypeScript oficial, mantida pelo próprio Filippo. Funciona no browser, Node.js, Deno e Bun. Suporta até WebAuthn/passkeys para criptografia via hardware.

## Ecossistema e adoção

O age não é um projeto experimental de fim de semana. É um projeto maduro com adoção real:

*   **21.6k+ stars no GitHub**, 629 forks, 18 releases (v1.3.1 é a mais recente, de dezembro de 2025)
    
*   **Especificação formal** mantida no [C2SP](https://github.com/C2SP/C2SP/blob/main/age.md) com vetores de teste padronizados (CCTV)
    
*   **Implementações em múltiplas linguagens:** Go (referência), Rust (rage), TypeScript (typage), Java (Jagged), Kotlin (kage), Swift (AgeKit)
    
*   **Integração com ferramentas de infraestrutura:** SOPS, Flux CD, chezmoi, gopass, Logseq, Apache NiFi
    
*   **Plugins para hardware:** age-plugin-yubikey, age-plugin-tpm, age-plugin-se (Secure Enclave)
    
*   **Empacotado em todas as distros Linux relevantes**, macOS (Homebrew/MacPorts), Windows (winget/Chocolatey/Scoop), FreeBSD, OpenBSD
    
*   **Licença BSD-3-Clause**
    

A lista completa de projetos do ecossistema está em [awesome-age](https://github.com/FiloSottile/awesome-age).

O estado de maturidade: o age atingiu a v1.0 em 2021, tem uma especificação formal estável, múltiplas implementações interoperáveis, suporte pós-quântico nativo desde v1.3.0, e é mantido ativamente por um dos criptógrafos mais respeitados do ecossistema open-source. É seguro tratá-lo como produção.

## O que o age não faz (e não pretende fazer)

*   **Assinaturas digitais** — use minisign, sigstore, ou SSH signing
    
*   **Gerenciamento de identidades / web of trust** — não existe um "keyring age"
    
*   **Criptografia de e-mail** — não é o caso de uso; use PGP/GPG ou Signal
    
*   **Criptografia de disco** — use LUKS/dm-crypt
    
*   **Comunicação em tempo real** — use Noise framework, Signal, WireGuard
    

O age é cirúrgico no escopo: criptografia de arquivos. Faz uma coisa e faz bem. A filosofia UNIX, como deveria ser.

## Exemplo prático: backup cifrado com age

Um script mínimo para backup cifrado:

```bash
#!/usr/bin/env bash
set -euo pipefail

RECIPIENT="age1ql3z7hjy54pw3hyww5ayyfg7zqgvc7w3j2elw8zmrj2kg5sfn9aqmcac8p"
BACKUP_DIR="/backup"
SOURCE="$HOME/documentos"
DATE=$(date +%Y%m%d-%H%M%S)
DEST="${BACKUP_DIR}/backup-${DATE}.tar.zst.age"

tar cf - "$SOURCE" \
  | zstd -T0 --long -19 \
  | age -r "$RECIPIENT" \
  > "$DEST"

echo "Backup cifrado: $DEST ($(du -h "$DEST" | cut -f1))"
```

Para restaurar:

```bash
age -d -i ~/key.txt backup-20260405-120000.tar.zst.age | zstd -d | tar xf -
```

Composabilidade UNIX. Sem mágica, sem configuração, sem surpresas.

## Considerações finais

O age é a resposta para quem quer cifrar arquivos sem precisar entender a história inteira da criptografia assimétrica desde 1991. Primitivas modernas, design minimalista, composabilidade UNIX, e um autor que sabe o que está fazendo.

Se você usa GPG apenas para cifrar arquivos (e sejamos honestos, esse é o caso de 90% das pessoas), o age é uma substituição direta e superior. Se você precisa do ecossistema completo do PGP (assinaturas, web of trust, integração com e-mail), o GPG continua sendo necessário — mas provavelmente menos do que você imagina.

Instale, gere uma chave, cifre algo. A experiência fala por si.

## Links e referências

*   **Repositório principal:** [github.com/FiloSottile/age](https://github.com/FiloSottile/age)
    
*   **Especificação do formato:** [age-encryption.org/v1](https://age-encryption.org/v1)
    
*   **Especificação formal (C2SP):** [github.com/C2SP/C2SP/blob/main/age.md](https://github.com/C2SP/C2SP/blob/main/age.md)
    
*   **Man page:** [filippo.io/age/age.1](https://filippo.io/age/age.1)
    
*   **rage (Rust):** [github.com/str4d/rage](https://github.com/str4d/rage)
    
*   **Typage (TypeScript):** [github.com/FiloSottile/typage](https://github.com/FiloSottile/typage)
    
*   **Jagged (Java):** [github.com/exceptionfactory/jagged](https://github.com/exceptionfactory/jagged)
    
*   **age-plugin-yubikey:** [github.com/str4d/age-plugin-yubikey](https://github.com/str4d/age-plugin-yubikey)
    
*   **awesome-age (ecossistema):** [github.com/FiloSottile/awesome-age](https://github.com/FiloSottile/awesome-age)
    
*   **Blog do Filippo sobre autenticação no age:** [words.filippo.io/age-authentication](https://words.filippo.io/age-authentication/)
    
*   **SOPS (Mozilla):** [github.com/getsops/sops](https://github.com/getsops/sops)
    
*   **Release v1.3.0 (pós-quântico):** [github.com/FiloSottile/age/releases/tag/v1.3.0](https://github.com/FiloSottile/age/releases/tag/v1.3.0)
    
*   **Flux CD + SOPS + age:** [fluxcd.io/flux/guides/mozilla-sops](https://fluxcd.io/flux/guides/mozilla-sops/)
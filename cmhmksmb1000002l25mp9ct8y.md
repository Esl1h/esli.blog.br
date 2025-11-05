---
title: "Ferramentas para Criptografia"
datePublished: Wed Nov 05 2025 22:34:53 GMT+0000 (Coordinated Universal Time)
cuid: cmhmksmb1000002l25mp9ct8y
slug: ferramentas-para-criptografia
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/gcgves5H_Ac/upload/ef0197eefc1b8489748ac69913d760c6.jpeg
tags: tools, encryption, veracrypt, gnupg, openpgp, tomb, cryptomator, picocrypt, kryptor

---

Se você ainda acha que criptografia de arquivos começa e termina no GnuPG, tenho más notícias: você está perdendo metade da festa.

Enquanto o ecossistema PGP continua sendo padrão para assinaturas e criptografia de mensagens, existem cenários onde você precisa de algo diferente — seja um container criptografado para backups, proteção transparente de diretórios na nuvem, ou simplesmente uma interface que não exija meia hora de leitura de man pages.

O mercado de ferramentas de criptografia se fragmentou nos últimos anos, cada uma atacando um nicho específico: containers full-disk, file-level encryption, cloud-first design.

Algumas são sucessoras diretas de projetos abandonados (olá, TrueCrypt), outras reinventam a roda porque a roda anterior estava enferrujada. Vamos destrinchar as opções que realmente importam para quem leva segurança a sério — e não tem paciência para soluções corporativas inchadas.

## VeraCrypt

Sucessor direto do TrueCrypt (RIP 2014), o VeraCrypt assumiu o bastão como padrão de facto para containers criptografados full-disk e volumes virtuais. Desenvolvido desde 2013, implementa AES, Serpent, Twofish e combinações em cascata (AES-Twofish-Serpent), com PBKDF2 ou Argon2 para derivação de chave. Suporta hidden volumes (deniable encryption), boot encryption em Windows, e compatibilidade cross-platform real — Linux, Windows, macOS sem drama.

Tecnicamente sólido: passou por auditoria independente (OSTIF/QuarksLab 2016), com correções implementadas. O problema é o overhead: criar um container exige interação GUI ou comandos verbosos, performance de I/O é 5-15% inferior a filesystem nativo (dependendo do algoritmo e hardware), e a interface gráfica parece ter parado em 2008. Para volumes estáticos grandes (backups externos, arquivos sensíveis) funciona perfeitamente. Para uso dinâmico diário, você vai sentir o atrito.

**Prós**: auditado, maturidade técnica, hidden volumes, cross-platform consolidado.  
**Contras**: UI antiquada, overhead perceptível, curva de aprendizado moderada.  
**Use quando**: precisar de containers grandes, criptografia de sistema, ou deniable encryption.

## Cryptomator

Desenhado especificamente para provedores de cloud storage (Dropbox, Google Drive, OneDrive), o Cryptomator aplica criptografia client-side transparente em nível de arquivo. Cada arquivo é criptografado individualmente com AES-GCM 256-bit, nomes de arquivo ofuscados via AES-SIV, estrutura de diretórios flattened para evitar vazamento de metadados. O vault aparece como filesystem virtual (FUSE no Linux, WebDAV/Dokany em outros OS), sincronização funciona normalmente porque cada arquivo é uma unidade independente.

A arquitetura é inteligente: não há container monolítico, então sync incremental funciona nativamente. Auditado em 2022 (Cure53), código-fonte aberto (GPLv3 core, MIT para apps mobile), com versão freemium (básico grátis, recursos avançados pagos). Performance é excelente para uso diário — overhead mínimo porque trabalha file-level. O problema é justamente a especialização: se você não usa cloud sync, está escolhendo a ferramenta errada. E a dependência de daemon/GUI pode complicar automação ou uso headless.

**Prós**: zero-knowledge cloud encryption, sync-friendly, performance superior, auditado.  
**Contras**: focado em cloud (overkill para local-only), versão completa paga, não serve para full-disk.  
**Use quando**: cloud storage é parte do workflow, precisa de transparência no sync, quer GUI moderna.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762381456647/995bb407-d03e-43d1-a31c-9cdc9399886b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762381481475/2ab1311f-e361-4cda-8c45-02b71aa003fa.png align="center")

## Picocrypt

Minimalista ao extremo: interface gráfica de arquivo único (Go + Giu), implementa XChaCha20-Poly1305 e Argon2id, sem dependências externas. 500KB de binário. Drag-and-drop de arquivo, password, encrypt — pronto. Desenvolvido por um estudante (Evan Su) como projeto educacional que ganhou tração pela simplicidade brutal. Não há containers, volumes, ou complexidade: você passa um arquivo, recebe outro arquivo criptografado. Ponto.

Tecnicamente limitado por design: não faz compressão, não gerencia múltiplos arquivos nativamente (você zipa antes), não tem CLI (só GUI). Mas para o caso de uso "preciso criptografar este PDF/backup/arquivo sensível agora", executa perfeitamente. Argon2id com parâmetros ajustáveis protege contra brute-force, XChaCha20 é moderno e rápido. O projeto é mantido ativamente (último commit recente), mas é one-man show — se Evan desistir, acabou.

**Prós**: extrema simplicidade, zero curva de aprendizado, binário minúsculo, algoritmos modernos.  
**Contras**: GUI-only, sem automação, não gerencia múltiplos arquivos, projeto pequeno.  
**Use quando**: precisa de algo rápido para arquivos individuais, prioriza simplicidade sobre features.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762381621379/4ba4de37-2865-4080-ab41-7b2f0a073f7e.png align="center")

### Picocrypt-NG

Fork do Picocrypt original que descontinuado em setembro/2025.

Ainda em desenvolvimento ativo (status alpha/beta dependendo da release), mas já funcional para uso real.

Porém, mesmo descontinuado, o Picocrypt segue confiável e extremamente valido para o uso, visto sua arquitetura, simplicidade e criptografia usada.

## Kryptor

CLI-first desde a concepção: .NET Core (C#), disponível via dotnet tool install, implementa XChaCha20-Poly1305, BLAKE2b para hashing, Argon2id para KDF.

O objetivo é ser uma versão melhorada do age e do Minisign, oferecendo uma alternativa mais enxuta e fácil de usar ao GPG.

Foco em usabilidade de terminal — comandos intuitivos (kryptor -e file.txt, kryptor -d file.txt.kryptor), suporte a wildcards, recursive encryption de diretórios. Multiplataforma real via .NET runtime (Linux/Windows/macOS).

Diferencial técnico: asymmetric encryption opcional (X25519 + Ed25519), permitindo criptografia de chave pública sem complexidade do PGP. Você gera key pair, distribui pública, recipients descriptografam com privada — workflow similar a GPG mas interface infinitamente mais simples. File shredding integrado (overwrite com random data), memory-safe por rodar em managed runtime. Overhead de .NET é real (~30-50MB RAM base), mas irrelevante em contexto moderno.

**Prós**: CLI ergonômica, asymmetric encryption sem PGP hell, cross-platform sólido, shredding integrado.  
**Contras**: dependência de .NET runtime, não é opção para embedded/minimal systems, comunidade pequena.  
**Use quando**: trabalha primariamente em CLI, quer asymmetric simples, .NET já está no sistema.

## Tomb

Unix philosophy encarnada: shell script wrapper (~2000 linhas) em volta de cryptsetup (LUKS), gpg, steghide para steganografia opcional. Criado pelo hacklab italiano Dyne.org, foca em simplicidade conceitual — tomb é um arquivo LUKS + key file separado, mount transparente via loop device. Comandos são poéticos: tomb dig, tomb forge, tomb open, tomb close. Porque criptografia também pode ter estilo.

Tecnicamente é LUKS puro — dm-crypt com AES-XTS por padrão, todos os benefícios de kernel-level encryption. Diferencial está no workflow: key file separado do tomb permite storage distribuído (tomb no HD, key no pendrive), steganografia esconde key dentro de imagem JPEG/PNG, e hooks permitem bind mounts automáticos em diretórios específicos. É solução genuinamente Unix: componível, scriptável, sem GUI. Performance é idêntica a LUKS raw porque *é* LUKS.

**Prós**: LUKS nativo (kernel-level), extremamente scriptável, steganografia integrada, zero overhead.  
**Contras**: Linux-only, requer conhecimento de LUKS/dm-crypt, não há GUI, curva de aprendizado íngreme.  
**Use quando**: usuário avançado de Linux, quer máxima performance, prioriza composição Unix-style.

## Outras Alternativas no Ecossistema

O espaço de ferramentas de criptografia é mais populado do que parece.

**age** ([https://age-encryption.org/](https://age-encryption.org/)) merece menção — sucessor espiritual do PGP focado em simplicidade, com formato de arquivo moderno e CLI minimalista.

**Cryptsetup** puro (LUKS frontend) continua sendo padrão de facto no Linux para full-disk encryption, usado por praticamente toda distribuição mainstream.

**Rclone** com backend crypt oferece encryption-in-transit para cloud storage, competindo diretamente com Cryptomator em cenários de automação.

**7-Zip** com AES-256 funciona para arquivos compactados, apesar de KDF fraco (PBKDF2 com iterações insuficientes por padrão).

**Hat.sh** ([https://hat.sh/](https://hat.sh/)) roda inteiramente no browser (WebCrypto API), útil para ambientes onde instalar software é impossível, mas vem com óbvios tradeoffs de confiança.

**Shufflecake** ([https://shufflecake.net/](https://shufflecake.net/)) é research project promissor para deniable encryption via múltiplos volumes sobrepostos — TrueCrypt hidden volumes em esteróides, ainda experimental.

Para menção honrosa: **Duplicati/Restic/Borg** implementam backup com encryption nativo, resolvendo o problema de "criptografar antes de backup" nativamente.

**SSHFS** com EncryptFS ou **gocryptfs** (sucessor do EncFS) servem encryption de filesystem transparente.

**OpenSSL** continua sendo canivete suíço para operações pontuais via CLI (openssl enc -aes-256-cbc).

E claro, **BitLocker** (Windows) e **FileVault** (macOS) são soluções integradas ao OS — convenientes, mas vendor lock-in total e auditoria limitada.

## Considerações Finais

Escolher ferramenta de criptografia não é sobre "melhor" ou "pior" — é sobre threat model e workflow. VeraCrypt domina containers grandes e hidden volumes.

Cryptomator é imbatível para cloud storage. Picocrypt/Picocrypt-NG servem uso casual e rápido. Kryptor entrega CLI moderna sem complexidade PGP. Tomb é para quem respira terminal e quer LUKS sem fricção.

O que *não* fazer: misturar múltiplas ferramentas para o mesmo dado (encrypt com VeraCrypt, depois Picocrypt — você só multiplicou superfície de ataque).

Defina seu caso de uso, escolha a ferramenta apropriada, e seja consistente.

E obviamente: nenhuma ferramenta salva você de passwords ruins ou key management incompetente.

Use password manager, gere keys fortes, faça backup da chave de recuperação em local seguro físico.

Agora você tem alternativas. Use com sabedoria — ou pelo menos com menos desculpas para continuar armazenando dados sensíveis em plaintext.

VeraCrypt

[https://veracrypt.jp/en/Home.html](https://veracrypt.jp/en/Home.html)

Cryptomator

[https://cryptomator.org/](https://cryptomator.org/)

Picocrypt

[https://github.com/Picocrypt/Picocrypt](https://github.com/Picocrypt/Picocrypt)

Picocrypt-NG

[https://github.com/Picocrypt-NG/Picocrypt-NG](https://github.com/Picocrypt-NG/Picocrypt-NG)

Kryptor

[https://www.kryptor.co.uk](https://www.kryptor.co.uk)

Tomb

[https://dyne.org/tomb/](https://dyne.org/tomb/)
---
title: "age + yubikey"
datePublished: 2026-04-10T13:20:30.742Z
cuid: cmnsxokr700kf29ka06zl4krn
slug: age-yubikey
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/4bc9f846-0c16-44b0-b3dc-92ad3e03e41f.png
tags: security, encryption, cryptography, yubikey, yubico, yubikeys

---

age é uma ferramenta de criptografia de arquivos. É o substituto moderno do gpg --encrypt.

Escrevi mais a respeito dele em [https://esli.blog.br/age-criptografia-de-arquivos-simples-moderna-e-segura](https://esli.blog.br/age-criptografia-de-arquivos-simples-moderna-e-segura)

Em resumo, o uso é o seguinte:

```shell
# Encrypt
age -r age1ql3z7hjy8zmrj2kg5sfn9aqmcac8p -o file.enc file.txt

# Decrypt
age -d -i key.txt -o file.txt file.enc
```

## age + YubiKey PIV — como funciona

O plugin age-plugin-yubikey usa o slot PIV da YubiKey (não o HMAC challenge-response) para encrypt/decrypt:

*   Gera um key pair diretamente no YubiKey (chave privada nunca sai do hardware)
    
*   Exporta a identity (referência ao slot PIV) e o recipient (chave pública)
    
*   Encrypt usa a chave pública (não precisa da YubiKey)
    
*   Decrypt exige a YubiKey fisicamente presente + PIN + touch
    

### Comparativo

Criei dois .sh:

Um usando openssl com a chave Yubikey (HMAC): [https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-encrypt-file.sh](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-encrypt-file.sh)

E o outro, usando o age com a chave Yubikey: [https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt.sh](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt.sh)

|  | [`yk-encrypt.sh`](http://yk-encrypt.sh) (HMAC) | `age` + YubiKey PIV |
| --- | --- | --- |
| **Mecanismo** | Symmetric — HMAC-SHA1 → AES-256 | Asymmetric — ECDH (P-256) → ChaCha20-Poly1305 |
| **Encrypt sem YubiKey** | Precisa da chave física | Só precisa da chave pública (arquivo) |
| **Decrypt sem YubiKey** | Sempre será necessária a chave | Sempre será necessária a chave |
| **Chave privada** | Secret no slot OTP (extraível via reset) | Gerada no chip PIV (nunca exportável) |
| **Múltiplos recipients** | não | Pode cifrar para N chaves públicas |
| **Dependências** | `openssl`, `ykman` | `age`, `age-plugin-yubikey` |
| **Maturidade** | OpenSSL (battle-tested) | age (moderno, auditado, mas mais novo) |

### Caso de uso prático

*   Cifrar backups no servidor **sem a YubiKey presente** (só a chave pública)
    
*   Decifrar só na tua máquina com a YubiKey plugada
    
*   Compartilhar arquivos cifrados com múltiplas pessoas (cada uma com seu recipient)
    

## Instalação do plugin

```shell

# age-plugin-yubikey (Rust, via cargo ou release binário)
cargo install age-plugin-yubikey
```

## Encriptando com mais de uma chave Yubikey

Segundo as recomendações, você deveria ter duas chaves Yubikey. Como encriptar permitindo o acesso ao arquivo posteriormente, com qualquer uma das duas?

Além disso, outra possibilidade com o age é encriptar um arquivo que possa ser desencriptado usando 5 ou 6 chaves específicas, trabalhando em equipe por exemplo.

Isto é nativo no age, a flag `-r` aceita múltiplos recipientes:

```shell
age -r "$RECIPIENT_1" -r "$RECIPIENT_2" -r "$RECIPIENT_3" -o arquivo.age arquivo  
```

O `age` encripta uma chave de sessão efêmera para cada recipiente separadamente e qualquer uma das chaves privadas correspondentes consegue decriptar. É o mesmo modelo do PGP (`--recipient` múltiplo).

### Utilizando o script para múltiplas chaves:

[https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt-multikeys.sh](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-encrypt-multikeys.sh)

[https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-decrypt-multikeys.sh](https://github.com/Esl1h/yubikey-shell-toolkit/blob/main/yk-age-decrypt-multikeys.sh)

```shell
# Encrypt para 3 YubiKeys
yk-age-encrypt-multikeys.sh -r age1yubikey1q... -r age1yubikey1q... -r age1... arquivo.txt


# Ou via recipients.txt (recomendado)
yk-age-encrypt-multikeys.sh arquivo.txt


# Decrypt com YubiKey específico
yk-age-decrypt-multikeys.sh -i ~/.config/yk-toolkit/age/yk2-identity.txt arquivo.txt.age
```

O age gera uma chave de sessão única e aleatória para o arquivo (usando ChaCha20-Poly1305).

Essa mesma chave de sessão é encriptada várias vezes: uma vez para a Chave A, uma para a Chave B, etc. Todas essas "versões" encriptadas da chave de sessão são guardadas no cabeçalho do arquivo .age.

Ao decriptar, o age testa as identidades que você possui. Se a sua YubiKey conseguir abrir qualquer um dos "cadeados" do cabeçalho, ela libera a chave de sessão e o arquivo é aberto.

Vantagens dessa abordagem:

*   Redundância: Se você perder sua YubiKey principal, mas tiver configurado uma YubiKey de backup, você recupera seus dados.
    
*   Colaboração: Você pode encriptar um arquivo que tanto você quanto outros consigam abrir com suas respectivas chaves físicas.
    
*   Performance: A velocidade de encriptação e decriptação não muda quase nada, pois o arquivo em si é encriptado apenas uma vez; apenas o cabeçalho cresce alguns bytes para cada chave extra.
    

## Conclusão

O primeiro script (yk-encrypt.sh) usa o YubiKey como um gerador de senhas complexas que são transformadas em uma chave pelo OpenSSL; já o segundo (yk-age-encrypt.sh) através do age utiliza criptografia de chave pública (Piv), separando a chave pública da privada (guardada no hardware).

No age, a segurança é reforçada pelo uso do algoritmo ChaCha20-Poly1305, que além de esconder os dados, garante que ninguém altere sequer um bit do arquivo sem ser detectado, algo que o OpenSSL do primeiro script não faz automaticamente.

Para um atacante, o nível de risco de conseguir abrir seu arquivo sem possuir fisicamente o seu YubiKey é extremamente baixo, beirando o impossível com a tecnologia atual. No entanto, o script que usa o age é superior porque exige um PIN (senha numérica) para liberar o acesso ao hardware, adicionando uma camada extra de proteção: se alguém roubar seu YubiKey, ainda precisará da sua senha. Já no primeiro script (yk-encrypt.sh), a segurança depende totalmente de manter o arquivo de "challenge" guardado; se um atacante copiar seu arquivo encriptado e o arquivo de challenge, a única barreira restante é a posse física do YubiKey.

Portabilidade: O age é um binário único e estático. Você consegue decriptar esse arquivo em praticamente qualquer OS apenas instalando o age e conectando a chave.

Recuperação: Com PIV no YubiKey, é vital ter o PIN e o PUK anotados, pois se o PIN for bloqueado por excesso de tentativas, o acesso à chave privada é perdido permanentemente.

Identidade: A "chave pública" gerada pelo age (começando com age1...) pode ser compartilhada ou publicada sem medo — ela serve apenas para que as pessoas (ou seus scripts) possam encriptar arquivos para você. Inclusive, a minha está no perfil do meu GitHub.

Embora o uso de HMAC e OpenSSL seja uma solução funcional, o age foi projetado especificamente para ser simples, seguro e à prova de erros humanos. Ao usar o age-plugin-yubikey, você eleva o nível do seu workflow: elimina a necessidade de gerenciar arquivos de "challenge" extras, ganha proteção por PIN nativa do hardware e utiliza algoritmos de criptografia de última geração. Para quem busca proteger segredos em servidores, backups ou arquivos sensíveis no Linux, a combinação age + YubiKey é hoje um padrão-ouro de praticidade e segurança.
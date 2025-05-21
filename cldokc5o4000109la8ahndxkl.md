---
title: "Yubikey #5 - chaves GPG"
datePublished: Fri Feb 03 2023 13:29:24 GMT+0000 (Coordinated Universal Time)
cuid: cldokc5o4000109la8ahndxkl
slug: yubikey-chaves-gpg
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1675430715954/59ade554-821f-4564-a2fa-dc4ac5ca2bb6.jpeg
tags: linux, server, security, cryptography, gpg

---

## Criando chaves gpg RSA

Criando uma chave:

`gpg --gen-key`

Ou:

`gpg --expert --full-generate-key`

Escolha as opções:  
(1) RSA and RSA (default)  
4096 (RSA Keysize)  
3072 (subkeys)  
0 para nunca expirar ou escolha o período para expiração  
Por fim, complete com os dados de identificação da chave e coloque uma senha.

```plaintext
pub   rsa4096 2023-02-02 [SC]
      F40975F47855887B2084E7D18A74F970158F02B8
uid                      Esli Silva (https://esli.blog.br) <not.announced@simplelogin.fr>
sub   rsa3072 2023-02-02 [E]
```

## Encriptando usando a chave gpg RSA

Para encriptar:

```bash
gpg -r F40975F47855887B2084E7D18A74F970158F02B8 -e -a -o ArquivoEncriptado.pdf ArquivoOriginal.pdf
```

Será gerado o output (sem pedir a senha!)

Para descriptografar o arquivo, será necessário ter a chave (publica) e a senha, caso seja em outro host, precisa executar o gpg import da chave.

```bash
gpg -r F40975F47855887B2084E7D18A74F970158F02B8 -d -o ArquivoDescript.pdf ArquivoEncriptado.pdf

gpg: encrypted with 4096-bit RSA key, ID 0EE7237D623C02EE, created 2023-02-02
      "Esli Silva (https://esli.blog.br) <not.announced@simplelogin.fr>"
```

## Adicionando uma subkey na chave gpg:

Edite a chave gpg criada acima:  
`gpg --expert --edit-key F40975F47855887B2084E7D18A74F970158F02B8`

Irá abrir o prompt do gpg:

```bash
gpg>
```

Insira o comando `addkey`

```bash
gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 8
```

Escolha a opção 8.

No próximo passo, escolha quais opções habilitar.

```bash
   Possible actions for a RSA key: Sign Encrypt Authenticate 
Current allowed actions: Sign Encrypt Authenticate 

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished
```

O padrão inicial será Assinar e criptografar.  
Para selecionar apenas a opção de chave de autenticação, digite S para desativar a assinatura, E para desativar a criptografia, e finalmente A para ativar a autenticação.

Eu Deixei as 3 opções habilitadas: `Current allowed actions: Sign Encrypt Authenticate`

Ou seja, esta subkey conseguirá fazer tudo (não vejo sentido nisto, mas é só um lab rsrs).

> \*\*A mesma chave NUNCA deve ser usada para diferentes propósitos!
> 
> O formato de subkeys é justamente para termos assinatura, criptografia e autenticação separados.\*\*
> 
> keypairs RSA no PGP ainda podem ter todos os bits de capacidade de uma só vez (como neste exemplo), todos os outros tipos de chaves são explicitamente tratados como usando diferentes algoritmos para assinatura e criptografia – uma chave "ECDSA" ( 19 ) ou "EdDSA" ( 22 ) pode apenas assinar/verificar e uma key "ECDH" ( 18 ) pode apenas criptografar/descriptografar.
> 
> E como a chave mestra no PGP é usada para certificar as chaves de outras pessoas e suas próprias sub-chaves, ela *deve* ser um algoritmo com capacidade de assinatura, `certify` .

Será solicitado a senha da gpg que estamos editando (adicionando uma chave), e no final, terá o retorno da chave adicionada a as permissões dela:

```bash
ssb  rsa4096/0EE7237D623C02EE
     created: 2023-02-02  expires: never       usage: SEA
```

**S**(sign) **E**(encrypt) **A**(authenticate)

Por fim, salve.

```bash
gpg> quit
Save changes? (y/N) y
```

## Backup/export da chave gpg

Para realizar o backup (export da secret key):  
`gpg --export-secret-key --armor F40975F47855887B2084E7D18A74F970158F02B8`

Será retornado no output o PGP PRIVATE KEY BLOCK. Copie e salve num local seguro!  
Abaixo, passando o parametro --output já direciona para um arquivo de texto ;-)

### Exportando a chave privada e publica

Pública:  
`gpg --output public.pgp --armor --export F40975F47855887B2084E7D18A74F970158F02B8`

Privada:  
`gpg --output private.pgp --armor --export-secret-key F40975F47855887B2084E7D18A74F970158F02B8`

## Criando chave por ECC

Segue os mesmos passos, apenas no inicio informando a opção ECC:  
`(11) ECC (set your own capabilities)`

```bash
gpg (GnuPG) 2.2.35; Copyright (C) 2022 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for a ECDSA/EdDSA key: Sign Certify Authenticate 
Current allowed actions: Sign Certify 

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? A

Possible actions for a ECDSA/EdDSA key: Sign Certify Authenticate 
Current allowed actions: Sign Certify Authenticate 

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q

Please select which elliptic curve you want:
   (1) Curve 25519
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 1

Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all

Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Esli Silva
Email address: my.develoment@aleeas.com
Comment: https://esli.blog.br
You selected this USER-ID:
    "Esli Silva (https://esli.blog.br) <my.develoment@aleeas.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/home/esli/.gnupg/openpgp-revocs.d/C512D2E3863F0D4902D63EC5BAB622429963AB29.rev'
public and secret key created and signed.


pub   ed25519 2023-02-02 [SCA]
      C512D2E3863F0D4902D63EC5BAB622429963AB29
uid                      Esli Silva (https://esli.blog.br) <my.develoment@aleeas.com>
```

Ao editar uma chave ECC e chamar a opção `addkey`, não temos mais aquela opção 8, onde era possível (no RSA) adicionar diversas capacidades na mesma chave:

```bash
gpg --edit-key C512D2E3863F0D4902D63EC5BAB622429963AB29
gpg (GnuPG) 2.2.35; Copyright (C) 2022 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/BAB622429963AB29
     created: 2023-02-02  expires: never       usage: SCA 
     trust: ultimate      validity: ultimate
[ultimate] (1). Esli Silva (https://esli.blog.br) <my.develoment@aleeas.com>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
  (14) Existing key from card
Your selection?
```

Irei criar uma subkey RSA (encrypt only), 4096 e sem expiração.

O resultado:

```bash
sec  ed25519/BAB622429963AB29
     created: 2023-02-02  expires: never       usage: SCA 
     trust: ultimate      validity: ultimate

ssb  rsa4096/50E40289DF10BB86
     created: 2023-02-02  expires: never       usage: E   
[ultimate] (1). Esli Silva (https://esli.blog.br) <my.develoment@aleeas.com>

gpg> quit
Save changes? (y/N) y
```

## Listando as chaves pgp

Para listar as chaves gpg existentes/criadas: `gpg --list-keys`

## Importando a chave gerada para a YubiKey

Edite novamente sua chave gerada

`gpg --edit-key F40975F47855887B2084E7D18A74F970158F02B8`

Insira o comando:  
`keytocard`

```bash
gpg> keytocard
Really move the primary key? (y/N) y
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 3
```

Feito ;-)

https://developers.yubico.com/PGP/Importing\_keys.html

https://devhints.io/gnupg http://irtfweb.ifa.hawaii.edu/~lockhart/gpg/

https://guides.library.illinois.edu/data\_encryption/gpgcheatsheet

https://iransec.org/wp-content/uploads/2015/02/gpg-cheatsheet.pdf

https://gock.net/blog/2020/gpg-cheat-sheet/

https://blog.programster.org/gpg-cheatsheet

https://developers.yubico.com/PGP/SSH\_authentication/Windows.html
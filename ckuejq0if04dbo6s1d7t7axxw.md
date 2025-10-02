---
title: "Yubikey #4 - Openssh com yubikey + ed25519"
datePublished: Tue Oct 05 2021 20:40:04 GMT+0000 (Coordinated Universal Time)
cuid: ckuejq0if04dbo6s1d7t7axxw
slug: yubikey-ssh-ed25519-ecdsa
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1632839227734/APISw7IT-.jpeg
tags: linux, server, security, ssh, cryptography

---

## Gerar chave para ssh

No OpenSSH (a partir do 8.2, lançado em 2020), os dispositivos U2F/FIDO são suportados usando os tipos de chave pública "ecdsa-sk" e "ed25519-sk" (FIDO2, YubiKeys com firmware acima da versão 5.2.3) e seus certificados.

O sk é "Security Key".

Para descobrir qual a versão do firmware da sua chave Yubikey:

```plaintext
lsusb -v 2>/dev/null | grep -A2 Yubico | grep "bcdDevice" | awk '{print $2}'
```

Acima do 5.2.3 já possui suporte ao FIDO2 =)

A vantagem em usar o Yubikey para gerar suas chaves é que você obterá um par de chaves pública/privada normalmente, porém, o arquivo de chave privada não contém uma chave privada altamente sensível, em vez disso, contém um "identificador de chave" (key handle) que é usado pela Yubikey (chave de segurança) para derivar a chave privada real na hora da assinatura.

> Ou seja, além de possuir o usuário, hostname/IP e a chave, ao tentar fazer o login via SSH será necessário informar a passphrase (caso cadastrou alguma quando gerou a chave) e logo após será preciso inserir o token da yubikey para somente depois (caso tudo correto), ter acesso remoto ao host.

`ssh user@host.name.ou.ip -i .ssh/id_ed25519_sk` \[+ passphrase\] \[+token-yubikey\]

Mesmo vazando a chave, não será o suficiente para ter acesso ao host (mesmo que tenha conhecimento de qual o username).

[**ECDSA** (Elliptic Curve Digital Signature Algorithm)](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) possui vulnerabilidades, logo, use o [**ed25519**](https://en.wikipedia.org/wiki/EdDSA#Ed25519)

O RSA de 4096 bits tem a complexidade comparável ao Ed25519. Mas o Ed25519 ainda é preferido ao RSA devido à preocupação de que o RSA possa ser vulnerável às mesmas preocupações de força do DSA, embora a aplicação desse exploit ao RSA deva ser consideravelmente mais difícil.

[Somente mês passado (17/Agosto)](https://aws.amazon.com/pt/about-aws/whats-new/2021/08/amazon-ec2-customers-ed25519-keys-authentication/) a AWS passou a aceitar ed25519 nas EC2. Antes, somente RSA.

```plaintext
ssh-keygen -t ecdsa-sk -f ~/.ssh/id_ecdsa_sk
# Ou
ssh-keygen -t ed25519-sk -f ~/.ssh/id_ed25519_sk
```

![Peek 05-10-2021 16-58.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1633464159403/ORaDqN7iE.gif align="left")

Depois de gerar as chaves, adicione a chave publica gerada (.pub) no authorized\_keys do servidor e/ou adicione a chave via agent do ssh (ssh-agent, ssh-add, ...)

Para forçar o servidor SSH a receber apenas logins usando a chave gerada via Yubikey com a ecdsa e ed25519, adicione no /etc/ssh/sshd\_config do servidor:

```plaintext
PubkeyAcceptedKeyTypes sk-ecdsa-sha2-nistp256@openssh.com,sk-ssh-ed25519@openssh.com
# Ou somente ed25519:
# PubkeyAcceptedKeyTypes sk-ssh-ed25519@openssh.com
```
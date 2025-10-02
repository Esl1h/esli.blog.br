---
title: "YubiKey #2 - Linux Instalação"
datePublished: Mon Sep 27 2021 21:54:34 GMT+0000 (Coordinated Universal Time)
cuid: cku36uzut08mjjos1f2t1bis8
slug: yubikey-linux-instalacao
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1632771789454/ak1wDTUuHP.jpeg
tags: authentication, linux, security, auth

---

Há um pouco mais de 1 ano atrás, adquiri a Yubikey 5 NFC (as imagens da capa dos três artigos são fotos que tirei, juntamente com os adesivos que recebi, em cima do meu chromebook).

[Pagina com modelos existentes](https://www.yubico.com/products/identifying-your-yubikey/)

Esta série de artigos vem com um pouco de atraso rsrsss

Para começar, conecte seu YubiKey a uma porta USB em seu computador, de forma que os contatos dourados no YubiKey toquem os contatos dentro de sua porta USB - para a maioria dos computadores, isso será feito de forma que os contatos dourados fiquem voltados para cima.

Depois de conectar o YubiKey, o LED no dispositivo deve acender em verde sólido. O YubiKey usa os drivers de teclado USB padrão e não requer a instalação de software ou drivers adicionais.

# Sobre o Yubikey 5 NFC

Lançado em 2018

Funções primarias: Yubico OTP, OATH – HOTP (baseado em evento), OATH – TOTP (baseado em tempo), Smart Card (PIV), OpenPGP, FIDO U2F, FIDO2.

Diferencial: NFC

### FIDO2

O aplicativo FIDO2 permite autenticação segura de fator único e multifatorial e pode armazenar até 25 credenciais. Essas credenciais, que são protegidas por um PIN, permitem o login sem senha, onde a YubiKey, desbloqueada por um PIN e autorizada por toque, pode fazer o login em suas contas sem inserir um nome de usuário ou senha. O aplicativo FIDO2 é certificado pela FIDO.

### OTP

O miniaplicativo OTP contém dois slots programáveis, cada um pode conter uma das seguintes credenciais:

### Yubico OTP

* HMAC-SHA1 Challenge-Response
    
* Senha estática
    
* OATH-HOTP
    

### U2F

O aplicativo U2F pode conter um número ilimitado de credenciais U2F e é certificado pela FIDO.

### Oauth

A série YubiKey 5 pode conter até 32 credenciais OATH e suporta OATH-TOTP (baseado em tempo) e OATH-HOTP (baseado em contador). O acesso a este miniaplicativo requer o Yubico Authenticator.

### PIV (cartão inteligente)

Este aplicativo fornece um cartão inteligente compatível com PIV. Algoritmos com suporte:

* RSA 1024
    
* RSA 2048
    
* ECC P256
    
* ETC P384
    

Informações do slot:

* Slot 9a: Autenticação
    
* Slot 9b: Chave de gerenciamento
    
* Slot 9c: Assinatura Digital
    
* Slot 9d: Gerenciamento de Chaves
    
* Slot 9e: Autenticação do cartão
    
* Slot f9: Atestado
    
* Slots 82-95: Gerenciamento de chaves retirado
    

### OpenPGP

Este aplicativo implementa a versão 2.0 da especificação OpenPGP Smart Card, que pode ser usada com o GnuPG. No firmware YubiKey versões 5.2.3 e posteriores, a versão 3.4 das especificações do OpenPGP Smart Card é implementada em seu lugar (consulte este artigo para obter mais detalhes). Para tamanhos de chave acima de 2048 bits, é necessário o GnuPG versão 2.0 ou superior.

Algoritmos com suporte:

* RSA 1024
    
* RSA 2048
    
* RSA 3072
    
* RSA 4096
    
* secp256r1
    
* secp256k1
    
* secp384r1
    
* secp521r1
    
* brainpoolP256r1
    
* brainpoolP384r1
    
* brainpoolP512r1
    
* curve25519
    
* x25519 (decifrar apenas)
    
* ed25519 (assinar / apenas autenticação)
    

# Instalação

O Yubikey já funciona, mesmo sem instalar nada.

Você pode usa-lo com algum app que gera o token via software (Google Authenticator, Microsoft, Authy, Lastpass Authenticator entre outros...)

Mas pode usa-lo junto com o Yubico Authenticator.

No linux, a chave é reconhecida como entrada de teclado qwerty para inserir o código gerado quando tocado a superficie de metal (area dourada ao centro com o logo do 'Y').

Mas precisaremos do pcscd para configurar o manager e o Authenticator usando a versão dos Apps via AppImage, e CLI somente.

Vamos a instalação: O pacote pcscd está nos repos das distros!

```plaintext
sudo apt-get install pcscd
```

> Descrição do pcscd: Middleware para acessar um cartão inteligente. O objetivo é fornecer uma interface em um formato muito pequeno para comunicação com cartões inteligentes e leitores de cartões inteligentes.

> O daemon é usado para alocar/desalocar drivers de leitoras dinamicamente em tempo de execução e gerenciar conexões com as leitoras.

Se instalar via Snap ou no ubuntu adicionando os repositórios, não é necessário instalar o pcscd.

  
  
  

# Apps

![screenshot.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632775393883/vq8uXwKyEH.png align="left")

  
  
  

## Yubico-Authenticator

O Authenticator complementa a Yubikey em sites/apps/Ferramentas que não suportam o hardware como autenticador.

Através do App você escaneia o QR code (ou cadastra manualmente) e ele passa a gerar o token numérico que será solicitado após fazer o login com sucesso usando a senha.

Além da versão desktop, também há para mobile (Android/IOS)

Ao usar o Yubico-Authenticator, ele irá requisitar a Yubikey e o PIN para poder abrir o aplicativo e exibir os tokens sendo gerados.

<center>![ykauth-android.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632777543940/FVLNPwNBb.png)</center>

![ykauth.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632777499874/Tbaj-6IBK.png align="left")

```plaintext
sudo snap install yubioath-desktop
```

Snap e AppImage: https://www.yubico.com/products/yubico-authenticator/#h-download-yubico-authenticator

Snap: https://snapcraft.io/install/yubioath-desktop

![Yubioath-desktop_linux.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778028599/5xki1n1GE.png align="left")

![Yubico-Auth_plock.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778094411/n5vAK1GDr.png align="left")

![Yubico-Authenticator-QR.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778069457/YVmfnN994.png align="left")

![YubicoAccount.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778055544/pTfXVtXKG.png align="left")

![Yubico-Authenticator_LinuxDesktop.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778084072/oCXvNPgiL.png align="left")

![Yubico-Authenticator.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778061368/WrdF9c_Xd.png align="left")

Eu não configurei o Yubico Authenticator (nem no Desktop, nem no Mobile), pois em ambos eu utilizo a conta Premium do LastPass, gerenciador de senha que possui seu proprio app autenticador para gerar os tokens.

E ele possui suporte ao Yubikey (assim como quase todos).

![lastpass-yubikey.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632778422606/er7VuYubP.png align="left")

  
  
  

## Yubikey Manager

É opcional!

O Manager é para configurar os tokens e gerenciar os slots descritos acima em relação ao Yubikey 5 NFC.

AppImage: https://www.yubico.com/support/download/yubikey-manager/ e https://developers.yubico.com/yubikey-manager-qt/Releases/

Ubuntu e derivados:

```plaintext
sudo add-apt-repository ppa:yubico/stable && sudo apt-get update
```

[YubiKey Manager (CLI):](https://docs.yubico.com/software/yubikey/tools/ykman/)

`sudo apt install yubikey-manager`

[YubiKey Personalization Tool:](https://support.yubico.com/hc/en-us/articles/360013790359-YubiKey-Personalization-Tool-User-Guide)

`sudo apt install yubikey-personalization-gui`

[libpam-yubico:](https://support.yubico.com/hc/en-us/articles/360018695819)

`sudo apt install libpam-yubico`

[libpam-u2f:](https://support.yubico.com/hc/en-us/articles/360016649099)

`sudo apt install libpam-u2f`

![yubikey-manager4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632779448928/QgAIcZ3Jp.png align="left")

![yubikey-manager3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632779455131/sYl2GFm7X.png align="left")

![yubikey-manager2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632779461602/vHGIazjV6.png align="left")

![yubikey-manager.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632779467930/QKIc_H9O4.png align="left")

  
  
  

## Yubikey Personalization Tool

![YubiKey-Personalization-tool.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632777818714/BgQF9Og_d.png align="left")

Links:

[Sobre o Yubikey 5 NFC](https://support.yubico.com/hc/en-us/articles/360013656980-YubiKey-5-NFC)

[Github](https://github.com/Yubico)
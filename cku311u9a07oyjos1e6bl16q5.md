---
title: "Yubikey  #1 - Introdução ao 2FA"
datePublished: Mon Sep 27 2021 19:11:55 GMT+0000 (Coordinated Universal Time)
cuid: cku311u9a07oyjos1e6bl16q5
slug: yubikey-introducao
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1632768892017/KwcDkeRRb.jpeg
tags: authentication, linux, tools, security, auth

---

> Este artigo é o primeiro de uma série "YubiKey", entre os artigos:
> Este primeiro, uma introdução rápida, o próximo será a instalação e uso básico no Linux e a terceira, usando ele no terminal com root, SUDO e modulo Pam.

<br>

# 2FA/MFA

A autenticação de dois fatores é uma camada adicional de segurança que requer pelo menos dois dos seguintes: 

- O que você sabe
- O que você tem
- O quem você é

Esses três fatores são os ingredientes clássicos de login seguro - e quanto mais deles você usar para proteger seus ativos, melhor.

Um saque em caixa eletrônico cobre dois: você sabe seu código PIN e tem seu cartão de débito. 
Um cofre de banco pode usar todos os três, com senhas, cartões de acesso e varreduras de impressão digital necessários para abrir o cofre.

Os logins tradicionais de e-mail e senha, no entanto, cobrem apenas o primeiro - o que você sabe.Você sabe seu endereço de e-mail e senha e, geralmente, eles são fáceis o suficiente para outra pessoa adivinhá-los ou decifrá-los. É isso que os torna tão inseguros. A autenticação de dois fatores adiciona um segundo de sua camada aos logins digitais, usando um aplicativo e seu telefone para gerar um código de login exclusivo. Pode até abranger todos os três se o seu telefone estiver protegido com uma impressão digital de quem você é o segurança.

Quase todos os serviços permite o usuário habilitar o duplo fator de autenticação.
Atualmente, ter isto disponível é um adicional para os usuários, e estamos num momento de virada onde, ter o 2FA habilitado é obrigatório em cada vez mais serviços/apps.

As senhas são terríveis. A maioria é muito fácil para os hackers adivinharem, e o resto é muito longo ou complicado para os humanos se lembrarem. Mesmo as senhas seguras são inúteis depois de vazadas, e os vazamentos são basicamente inevitáveis. Por essas razões, e mais, é uma boa ideia não depender exclusivamente de senhas. Essa é a ideia por trás da autenticação de dois fatores.

O 2FA é um codigo gerado que será necessário inserir, geralmente, após passar com sucesso pelo login usando a senha.
O codigo pode ser obtido via SMS, email, app especifico para 2FA, app proprio da ferramenta que gera seu 2FA como um 'token'  e por fim, um hardware.


**SMS ou códigos de e-mail** 

Os aplicativos enviam um código, que você precisa inserir antes de fazer o login. Este é o método mais fácil de configurar porque você não precisa instalar nenhum software ou comprar nenhum hardware. É também o menos seguro porque e-mail e SMS não são criptografados e são facilmente comprometidos.

**Aplicativos de autenticação**

Os aplicativos nos quais deseja fazer login solicitarão um código que você pode recuperar abrindo um aplicativo em seu telefone, como o Google Authenticator ou Authy. Isso é muito mais seguro do que confiar em SMS ou e-mail, mas não é exatamente conveniente - você precisa pegar seu telefone, abrir um aplicativo e digitar um código.

O YubiKey representa uma terceira maneira de fazer autenticação de dois fatores: **autenticação de hardware **. 

Os aplicativos pedem que você conecte uma ferramenta como um YubiKey em seu dispositivo e pressione um botão. A YubiKey envia um código exclusivo que o serviço pode usar para confirmar sua identidade. 

Isso é mais seguro, porque os códigos são muito mais longos e mais convenientes (SMS, token, geralmente são numéricos com 4 ou 6 digitos) e porque você não precisa digitar os códigos sozinho.

# YubiKey


![yubikey.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1632768762237/vFYnHEgno.jpeg)

O YubiKey é um dispositivo que torna a autenticação de dois fatores o mais simples possível. 
Em vez de um código ser enviado para você (SMS) ou gerado por um aplicativo em seu telefone (Authy, Authenticator, token do proprio app como os de banco por exemplo...), você apenas pressiona um botão em sua YubiKey.

Cada dispositivo possui um código exclusivo integrado, que é usado para gerar códigos que ajudam a confirmar sua identidade. 

YubiKey não é o único dispositivo de autenticação de dois fatores de hardware no mercado - apenas o mais popular.




## Porque é melhor?


- **Conveniência**

Aplicativos de SMS, e-mail e autenticação exigem que você copie e cole ou insira manualmente um código. Com o YubiKey, basta pressionar um botão em um dispositivo conectado ao seu computador.

- **Códigos muito mais longos**

Outros métodos 2FA normalmente enviam apenas um código de seis dígitos para confirmar sua identidade, basicamente porque não seria razoável esperar que humanos digitassem muito mais do que isso. Os YubiKeys não pedem que você digite um código manualmente, então eles podem usar códigos muito mais longos. Isso é mais seguro.

- **Fácil de migrar**

Você comprou um novo computador? Basta desconectar seu YubiKey do antigo, conectá-lo ao novo e você pode fazer login em todos os seus aplicativos, como antes. Você também pode usar uma chave para fazer login em sua conta em vários computadores. Achei o processo muito mais fácil do que migrar outros 2FA.

- **Muito difícil de hackear**

É relativamente fácil para os hackers comprometerem seu e-mail ou SMS. É muito mais difícil - quase impossível com a tecnologia atual - falsificar os códigos gerados por um dispositivo de hardware exclusivo.




Como funciona:
https://www.yubico.com/why-yubico/how-the-yubikey-works/

Catalogo de serviços/sites/apps que suportam:
https://www.yubico.com/br/works-with-yubikey/catalog/?sort=popular


![191466-OneKeyToAllSystems2-1024x558.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632768716728/sjzsfLtgA.png)
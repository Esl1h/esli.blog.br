---
title: "Como Criptografar e se Proteger do Gmail ou Outro Provedor de E-mail"
datePublished: Thu Oct 09 2025 11:31:22 GMT+0000 (Coordinated Universal Time)
cuid: cmgjc7cad000502jpgptke7mm
slug: como-criptografar-e-se-proteger-do-gmail-ou-outro-provedor-de-e-mail
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/3Mhgvrk4tjM/upload/6c2903c8f3c577ff27611adb20d15177.jpeg
tags: security, privacy, cryptography, gmail, gpg, pgp, simplelogin, gpg-key

---

* Crie sua chave GPG.
    
* Adote um cliente de e-mail seguro, como o Betterbird, por exemplo. Um fork do Thunderbird.
    
* Configure o seu Gmail nele como POP3 (IMAP se preferir)
    

> ***POP3 baixa emails do servidor para o dispositivo local e geralmente os remove do servidor.***
> 
> ***IMAP mantém emails no servidor permitindo acesso de múltiplos dispositivos ou via web posteriormente.***

No Betterbird, você irá adicionar sua chave OpenPGP.

Falei um pouco mais sobre criptografia para iniciantes aqui: [https://esli.blog.br/criptografia-para-iniciantes](https://esli.blog.br/criptografia-para-iniciantes)

E aprofundei o assunto do OpenPGP, PGP e GPG neste trecho de outro artigo: [https://esli.blog.br/certificados-e-openssl#openpgp-pgp-and-gpg](https://esli.blog.br/certificados-e-openssl#openpgp-pgp-and-gpg)

Criação de chave GPG, backup, importação, exportação e outros usos, inclusive com a Yubikey neste artigo: [https://esli.blog.br/yubikey-chaves-gpg](https://esli.blog.br/yubikey-chaves-gpg)

## SimpleLogin

Agora, usando o SimpleLogin, você irá criar alias de emails

Ou seja, ao enviar um e-mail para [**algumacoisa.1753@simplelogin.com**](mailto:algumacoisa.1753@simplelogin.com) ele vai direcionar para o seu [**email.verdadeiro@gmail.com**](mailto:email.verdadeiro@gmail.com) e ao adicionar a chave OpenPGP no SimpleLogin, ele vai encriptar toda a mensagem (e o título dela) entre o SimpleLogin (que recebeu a mensagem até o servidor do Gmail).

Com isso, se alguém enviar uma mensagem para o e-mail alias, o SimpleLogin vai encriptar com sua chave, o Gmail/Google não terá acesso a nada e somente no Betterbird, com a sua chave privada configurada, ele irá conseguir abrir sua mensagem normalmente.

Se alguém acessar seu Gmail, não terá acesso as mensagens.

Se outras pessoas tiverem sua chave pública, podem encriptar a mensagem e te enviar e-mails direto (para seu endereço real) e somente no cliente de e-mail com a sua chave privada, conseguirá abrir.

> ***Obviamente, a principal dica é: SAIA de qualquer serviço do Google. Troque seu email por outro provedor, como o Tuta ou Proton.***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759341488082/dcee6663-e66a-424f-bfba-a284f529833d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759341745166/15eb52f4-3a3d-47b0-b5a4-d6c1e859ff03.png align="center")

Inclusive, mascara o assunto (o título é você quem coloca no painel do SimpleLogin):

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759342391310/e4b0663c-c7de-49bf-b27b-5c75e342a819.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759931473538/e4a40923-f2b1-48fc-a2dd-e64e128e3998.png align="center")

O e-mail no Betterbird com minhas chaves OpenGPG:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759342673005/34ebbc3a-5245-4bf7-908a-3a3d9c4ca050.png align="center")

Se eu responder, segue toda comunicação encriptada: `betterbird → Gmail → SimpleLogin` e deste ponto em diante, segue para o remetente original, descriptografada e sem ele saber meu real endereço de email.

Respondendo:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759342994442/4096da6a-5de8-499f-87d1-9978ad306eb0.png align="center")

A resposta chega no remetente original:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759343423367/9b3c7e0c-a245-4e9a-b322-9c845a4cdc2a.png align="center")

### Obviedades que nem precisavam ser ditas:

1. Isto só protege minhas mensagens, ou seja, desde a chegada/armazenagem no gmail a até meu host no Betterbird.
    
2. Isto protege de divulgar meu endereço de e-mail principal. Posso ter 1 alias para cada site/assunto/contato/etc…
    
3. Não há garantias da segurança de quem está enviando até chegar no serviço do SimpleLogin. Mas isto permite MEU nível de segurança mesmo com quem não tenha conhecimento algum.
    
4. Ideal é as duas pontas do e-mail usarem suas próprias chaves, encriptando a mensagem desde a origem até o final.
    
5. Ideal também é sair do Gmail.
    
6. PGP no SimpleLogin é um recurso da conta premium. No Addy.io está disponível na conta gratuita.
    
7. Se trocar as chaves PGP e cada lado encriptar suas mensagens, anula o uso do SimpleLogin ou outros semelhantes para a criptografia, mas mesmo assim, a criação de alias para e-mail é um recurso muito necessário.
    

## Alternativas

Além do SimpleLogin, há o [**Addy.io**](http://addy.io/) que fornece o mesmo serviço.

Recomendo qualquer um destes acima, SimpleLogin ou Addy.io. Ambos possuem versão free e a conta premium é muito barata, além de haver promoções sazonais com pagamento vitalício ou grandes descontos.

Flowcrypt e Mailveloper basicamente realizam esta encriptação que descrevi, mas de forma mais simples via extensões e aplicativos próprios.

%[https://esli.blog.br/criptografia-para-iniciantes] 

%[https://esli.blog.br/certificados-e-openssl#openpgp-pgp-and-gpg] 

%[https://esli.blog.br/yubikey-chaves-gpg]
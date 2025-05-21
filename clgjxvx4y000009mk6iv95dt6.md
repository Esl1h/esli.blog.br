---
title: "Qual opção de KeePass devo escolher?"
seoTitle: "Qual KeePass escolher?"
seoDescription: "Compare free and open-source KeePass versions and forks (KeePass 1.x, 2.x, KeePassXC, KeeWeb, AuthPass) to find the best option"
datePublished: Sun Apr 16 2023 21:48:57 GMT+0000 (Coordinated Universal Time)
cuid: clgjxvx4y000009mk6iv95dt6
slug: qual-keepass-escolher
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/RcvQHQB9zgA/upload/e05e19a403169191355d94fecc849af3.jpeg
tags: linux, security, passwords, keepass

---

O KeePass é um gerenciador de senhas gratuito e de código aberto, que ajuda você a gerenciar suas senhas de maneira segura.  
O armazenamento é feito em um banco de dados, que é bloqueado com uma chave mestra e totalmente encriptado.

Entendo que ao ler este artigo, já saiba muito bem sobre gerenciadores de senhas, um pouco sobre o Keepass ou algum outro sistema, entre os mais famosos há o LastPass, Bitwarden, Dashlane e 1Password. Todos estes são online e possuem limitações na versão gratuita e vendem assinaturas de planos - o que torna o KeePass a melhor alternativa em diversas situações.

## KeePass

O KeePass oficial é o [https://keepass.info/](https://keepass.info/) com a release inicial em 2003! As versões mais novas são a 2.53 e 1.41 (quando escrevi este artigo) lançadas em Janeiro de 2023 (e menos de 5 meses após a release anterior).

No site, encontrará 2 versões dispoíveis: KeePass 1.x e o KeePass 2.x  
Um não é a evolução do outro! Eles são desenvolvidos em paralelo, sendo que ambos são gratuitos e de código-aberto. Porém, o 1.x usa uma plataforma antiga e ainda mantém suporte (o 2.x não!).

As duas versões usam o AES 256, sendo que o KeePass 2.x através de plugins consegue adicionar e prover suporte a outros algoritmos.

As diferenças:

### KeePass 1.x

Escrito em C++, só executa em plataforma Windows (ou usando o Wine no Linux).  
O database de senhas tem a extensão KDB.  
Adicionalmente ao AES 256, há o Twofish 256 bits e autenticação/integridade dos dados são garantidas usando um hash SHA-256 de texto puro (simples).

### KeePass 2.x

Surgiu em 2007. Possui suporte e features a mais do que o 1.x, como suporte ao OTP (One Time Password), Smart Cards, uma lista bem maior de plugins, suporte ao database remoto.  
Escrito em C# usando o .NET, ou seja, é possível executá-lo no Linux usando o Mono.  
O database de senhas tem a extensão .KDBX.  
Adicionalmente ao AES 256, há o ChaCha20 256 bits e autenticação/integridade dos dados são garantidas usando um Hash HMAC-SHA-256 de texto cifrado.

Nas versões de databases há mudanças também, enquanto o KDBX3 usa AES-KDF o KDBX4 (mais atual) usa Argon2.

Neste link tem uma tabela do desenvolvedor com a comparação entre as versões: [https://keepass.info/compare.html](https://keepass.info/compare.html)

Neste link, a lista de plugins (com a informação de qual versão suporta, 1x ou 2x): [https://keepass.info/plugins.html](https://keepass.info/plugins.html)

Neste link, maiores detalhes das diferenças na segurança entre as versões: [https://keepass.info/help/base/security.html](https://keepass.info/help/base/security.html)

## Forks e outros projetos relacionados ao KeePass

* ### KeePassL
    

Um fork do KeePass para rodar em Linux (quando não tinha o Mono e lembrando que o KeePass 2.x surgiu em 2007!).

* ### KeePassX
    

Quando o KeePass L tornou-se multi plataforma (cross) em 2006, mudou seu nome para [KeePassX](https://www.keepassx.org/).  
Multi-plataforma, entenda como Mac OS X, Windows e o tarball para Linux. Alguns desenvolvedores terceiros criaram pacotes para distros.

E foi descontinuado em Dez/2021 (com o ultimo release lançado em 2016).

* ### KeePassXC
    

O [**KeePassXC**](https://keepassxc.org/) surgiu em 2012 como um fork do KeePassX, visto o desenvolvimento moroso dele. Ou seja, ele mantém a linha de ser um um port multi plataforma do KeePass 2.x mas é feito em C++, tirando a necessidade de instalar o Mono.

Ele consegue trabalhar tanto com o KDBX3 e KDBX4 (mas o ideal é migrar para o KDBX4!), quanto importar arquivos do KeePass 1.x (KDB).  
Não suporta plugins do KeePass 2.x

Possui oficialmente extensões para navegador! Para os demais, precisa buscar alguma extensão desenvolvida por terceiros.

* ### KeeWeb
    

[**KeeWeb**](https://keeweb.info/) é um fork compativel com o KeePass feito em JavaScript, multi plataforma, porém feito como Web App (tanto acessando seu database via navegador - podendo até subir sua propria instância do web app) ou acessando via client instalado em seu S.O. (Electron). Surgiu em 2016 e é o mais novo aqui da lista, sua versão mais recente é de 07/2021, a versão é estável, porém, até o momento no github do projeto, está sem mantenedor.

É possível, usando o KeeWeb, apontar para o database de senhas diretamente armazenado em algum provedor (como o Google Drive, Dropbox, OneDrive ou qualquer um que suporte webDAV). Enquanto os demais, precisa ser feito de outra maneira, como mapeando o arquivo remoto no sistema operacional, seja montando um path (webDAV, samba, NFS, FTP...) ou por outro meio (como o client do Dropbox), mas para Google Drive, OneDrive e alguns outros, somente realizando o download do arquivo (ou uisando algum app terceiro como o OneDriver ou InSync).

Para navegadores, o site do projeto indica usar a extensão do KeePassXC.

* ### AuthPass
    

Semelhante ao KeeWeb, há o [**AuthPass**](https://authpass.app/). Sua última versão é de 06/2022 e aparenta que seu desenvolvimento está mais ativo. Feito em Flutter, e multi plataforma (incluindo mobile).

* ### KPCLI
    

O [**Kpcli**](https://kpcli.sourceforge.io/) é um client compatível com databases do KeePass 1 e 2 (KDB e a familia do KDBX) feito para funcionar via CLI.  
Através do bash é possível criar alguns scripts e automatizar o uso de senhas via linha de comando, sem precisar recorrer para algum client (ou o proprio KeePass) em sua versão gráfica.

## Outros ports/forks

No site do KeePass há uma lista: [https://keepass.info/download.html](https://keepass.info/download.html)

Porém, no github há uma lista maior ainda e separando por clients especificos em cada plataforma, clients cross multi plataforma, mobiles, plugins e ferramentas. [https://github.com/lgg/awesome-keepass](https://github.com/lgg/awesome-keepass)

Gerando senhas seguras

Todos eles possuem a capacidade de gerar senhas seguras. Mas se quiser ver outra forma de criar senhas seguras usando o pwgen (como eu gero minhas senhas), confira este post:

%[https://esli.blog.br/gerar-senhas-no-linux] 

# Conclusão

Vou tentar responder a pergunta sobre "Qual versão do KeePass usar?"

*Usa Windows?* Use o KeePass2.  
Ah, mas não quero usar o KeePass 2.x  
Então, vá na lista acima e escolha qualquer um ;-)

*Usa Linux?* Use o KeePassXC.

*Usa mais de um sistema operacional e se preocupa em ter a mesma usabilidade e tem medo em relação a compatibilidade?* Use o KeePassXC em todos os sistemas, seja Linux, Windows ou Mac.

*Quer usar no nevegador, igual os concorrentes pagos/freemium?* Use o KeePassXC e instale a extensão oficial.

*Quer usar no Android também?* Use o [**KeePassDX**](https://github.com/Kunzisoft/KeePassDX). Ele é compativel com todas as versões do database de senhas (KDB, KDBX...). Logo, não importa qual o App você escolheu para o Desktop, conseguirá usar no Android com o KeePassDX.  
Caso tenha escolhido o AuthPass, ele é o único que possui versão oficial para o Android. Todos os demais, precisará confiar em apps desenvolvidos por terceiros.

> **Complementando:**  
> **Use 2FA/MFA em tudo que for possível!**  
>   
> **Se estiver em dúvida entre 2 serviços/sites, escolha aquele que oferece 2FA/MFA (de preferencia via app e não SMS).**

Como melhorar sua segurança:

%[https://esli.blog.br/privacidade-e-seguranca] 

VPN não é o suficiente:

%[https://esli.blog.br/vpn-nao-e-o-suficiente]
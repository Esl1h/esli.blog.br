---
title: "Privacidade e Segurança"
datePublished: Wed Oct 12 2022 14:45:25 GMT+0000 (Coordinated Universal Time)
cuid: cl95qusvw000e09lgftjy4hht
slug: privacidade-e-seguranca
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/yekGLpc3vro/upload/v1665348935774/RGRoN-bpa.jpeg
tags: apps, security, privacy, foss, alternative

---

Vou descrever como atualmente (Out/2022) estou cuidando da privacidade dos meus dados online e segurança.
Neste outro artigo sobre o que poderá vir em 2023 (em relação ao Marco Civil da Internet no Brasil e da LGPD), escrevo que privacidade é luxo e quem sabe qual o verdadeiro valor dos seus dados, não deveria entregar de graça a diversas empresas. Infelizmente, saber e ter consciência disto é exceção, mesmo aos profissionais de tecnologia.

Não adianta falar o que deveria ou não usar como ferramenta, seria hipocrisia. Eu tento policiar-me, mas ainda não cheguei no 100% que quero em relação ao meu "*Eu digital*".

O que irei descrever neste post serão quais ferramentas utilizo no meu dia-a-dia tanto no laptop, PC e smartphone. Ainda estou preso a algumas que gostaria de me livrar.

Outro objetivo é futuramente atualizar com uma nova publicação mostrando a evolução.



Tenha em seus favoritos estes sites:

- https://www.privacyguides.org/tools/
- https://www.privacytools.io/
- https://ssd.eff.org/pt-br
- https://madaidans-insecurities.github.io/
- reddit.com/r/privacy

Há outros, mas nestes além de conhecer alternativas, também consegue validar configurações para otimizar o que já usa.


## Navegador

Brave (tanto smartphone quanto laptop) - baseado no Chrome/Chromium
E mantenho 2 outros
LibreWolf (desktop) - baseado no Firefox
Kiwi (Smartphone) - baseado no chromium, permite instalação de extensões no navegador do smartphone, como se estivesse no desktop. Mantenho ele apenas como 'segunda opção'.

### Extensões

- Ublock Origin
- https://burles.co (extensão no Librewolf e usando via JS script com o tampermonkey nos browsers baseados em Chrome). 


## Anotação/Notas/Textos

Uso o Joplin, tanto no smartphone quanto no PC.
Ele sincroniza os textos com a nuvem que escolhi e encripta os mesmos com minha chave.

Antes usava o Google Keep e o Evernote, eventualmente algum Notes... Mas o Joplin para mim foi a melhor alternativa.


## Site de Busca

Creio que foi um dos mais demorados até virar 100% abandonando o Google (ou qualquer outro grande).
- search.brave.com

Algumas coisas pesquiso direto no https://www.wolframalpha.com

Tentei migrar para o duckduckgo.com (usei até o lançamento do Brave Search), startpage.com, searchencrypt.com e alguns outros. Mas consegui adaptar-me melhor ao brave search (lançado como beta em 2021).

## Email, calendário, Armazenamento (drive)

Do Google (domínio próprio no GSuite/Workspace/4Business), outro endereço 'grátis' do gmail e ymail, troquei para o Proton (Mail, Calendar e Drive), até oi final de 2021 a versão gratuita e em diante, assinei o serviço.

### Email alias

Passei a utilizar o SimpleLogin (outra alternativa é o AnonAddy), como o SimpleLogin passou a fazer parte do Proton, utilizo ele como padrão.
Com ele, ao invés de usar o meu email real, crio um alias no SimpleLogin que irá fazer o redirect das mensagens (funcionando como reverso também), logo, com qualquer um que eu estiver trocando email (alguma lista momentanea, loja, ou qualquer uso temporario) não terá acesso ao email verdadeiro.

## VPN

Eu tinha 2 serviços de VPN (um deles 'vitalicio' e não muito confiável e outro com limite de trafego), mas atualmente por padrão, utilizo o Proton VPN. 

## Signal

Provavelmente a parte de Instant messengers seja a mais difícil de mudar, pois ficará dependente do que o seu circulo pessoal/familiar/trabalho e etc... utilize.
O Signal é o meu app padrão para SMS e comunicação, porém ainda mantenho o Whatsapp devido larga escala de uso no Brasil. Qualquer outro, como Telegram, Meta Messages (Facebook, instagram) não utilizo.


## Senhas/2FA/MFA

Gerenciador de senhas e 2FA em todos os serviços possíveis. Uso a chave física Yubikey, além de um app para os tokens de uso único.
Senhas complexas, sem repeti-las em mais de um site/serviço e periodicamente realizo a troca delas.
Tenho uma seria e postagens sobre o Yubikey
- https://esli.blog.br/yubikey-introducao
- https://esli.blog.br/yubikey-linux-instalacao
- https://esli.blog.br/yubikey-console-sudo-ssh
- https://esli.blog.br/yubikey-ssh-ed25519-ecdsa


## DNS

DNS fixo nos meus dispositivos (smartphone, PC, roteador) usando o Cloudflare e Quad9. 
- DNS Cloudflare: 1.1.1.1
- DNS Quad9: 9.9.9.9

# Segurança no Linux (desktop)

Não somente usar Linux, mas encriptar o disco, usar 2FA para o root, senhas diferentes, sem logs e sem tmp no disco (/var/log, /var/tmp e tmp são sistemas de arquivos montados como tmpfs no fstab). 
Swapfile encrypted.
Os apps descritos neste artigo, como o Brave, Proton VPN, Joplin.... E algumas outras medidas a nível de sistema operacional.

Obs.: Muita coisa aqui rsrsss merece um post dedicado.


# Segurança no Android

Quanto menos apps melhor.
Uma medida padrão que uso é ao invés de instalar diversos apps (até mesmo versões lite/Go) é usar a função "Add to Home Screen" do navegador, onde ele cria um atalho para o site que desejar.
A maioria dos apps são apenas versões recursivas dos sites dentro do web view do Android e usando diversos rastreadores, plugins, cookies e etc... 
Usando esta função, uso o site/app com o beneficio dos bloqueadores do Brave.
Alguns exemplos que podem ser usados neste formato: Amazon, Mercado Livre, Twitter, Facebook, reddit, 9gag


## Blokada 5

A versão 5 (https://blokada.org/#download) diferentemente da presente na Store do Android, não exige assinatura.
Com ele é possivel habilitar bloqueio de ads, rastreadores e etc... de outros apps, além de mudar o DNS.

## NoRoot Firewall

Com ele bloqueio toda a rede do smartphone e libero apenas alguns apps essenciais, criando um filtro. Com o filtro criado, consigo a qualquer momento, bloquear toda comunicação do smartphone, seja para economizar pacote de dados ou para evitar algum trafego em rede insegura.

## Não usar o buscador padrão

O foco é não usar o launcher default do smartphone (uso o Nova Launcher, mas já usei por um tempo o OpenLaucnher e o KISS) e com o Sesame Shortcut consigo alterar o buscador padrão para o https://search.brave.com além de integrar outros apps.
O Nova Launcher foi adquirido no inicio de 2022 pela empresa Branch, ou seja, estou buscando uma alternativa para migrar! Voltando a pensar no KISS ou o Lawchair, Neo... enfim, buscar um novo.


# Futuro da minha jornada


Ainda estou preso em algumas ferramentas do Google.
- Trocar a Play Store pelo AuroraStore (não o AuroraDroid ou F-Droid) - A AuroraStore é o PlayStore sem precisar e login associando os apps a uma conta Google.
- Google Maps e Waze. 

Sair do Android default (e personalizado) das fabricantes. O lado ruim é que as ROOMs existentes só funcionam com os aparelhos Google Pixel ou aparelhos com Google One.

Não adianta pegar as listas, por exemplo, no privacyguides.org e adota-los, precisa estar na balança com a privacidade/segurança as suas necessidades, uso, praticidade e conforto.... 
Não é vantagem migrar para algum app com pessima interface, dificil ou ruim de usar em prol de um beneficio que, pesquisando e testando mais, conseguirá achar outro bem melhor. Um bom exemplo disto são as diversas interfaces (frontend) para youtube e redes sociais, são péssimos! Horriveis! Não irão remover as ads das redes sociais, não removem ou bloqueiam direito os rastreadores dos sites e por fim, possuem um UX medonho. Mais fácil usa-las no Brave e criar o atalho na home screen do Android, assim como faço com outros sites ao invés de instalar seus apps proprios.

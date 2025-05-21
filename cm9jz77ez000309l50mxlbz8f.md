---
title: "Como fugir das propaganda na internet"
seoTitle: "Evite Propaganda Online: Dicas Simples"
seoDescription: "Aprenda a bloquear anúncios na internet com Brave Browser, Anti Paywall, NextDNS, VPN e SmartTubeNext para uma navegação sem propagandas"
datePublished: Wed Apr 16 2025 13:37:11 GMT+0000 (Coordinated Universal Time)
cuid: cm9jz77ez000309l50mxlbz8f
slug: como-fugir-das-propaganda-na-internet
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744747303828/ddd64e25-fd88-492c-8ecb-2890ae5cef53.png
tags: dns, ads, vpn, tampermonkey, brave-browser, paywall, adblock, adblocker, nextdns, smarttube, smarttubenext

---

> TL;DR: Brave Browser + Anti Paywall + NextDNS + VPN + SponsorBlock + SmartTubeNext

## Brave Browser

Desktop e Smartphone: mude para o Brave.  
Se não liga para tokens e demais recursos… desabilite-os. Desabilite o botão de VPN, remova o Brave Reward e qualquer outra coisa que não goste ou tenha ‘virado a cara’ caso ignorado o Brave como navegador no passado.

Chrome é insuportável, matou as extensões (Manifest V3) e está aprimorando seus rastreios, o Brave usa o Chromium (**Chromium ≠ Google Chrome**).

No Brave poderá aprofundar os bloqueios de propagandas por meio de listas (filtros) e Scriptlets. Mas, por default, o Brave Shield já irá remover quase tudo que aparecer.

Aqui tenho alguns filtros e scriptlets que uso para remover alguns itens em sites específicos: [https://github.com/Esl1h/Brave-Filters-and-Scriptlets](https://github.com/Esl1h/Brave-Filters-and-Scriptlets)

Acompanhe em [https://privacytests.org/](https://privacytests.org/) os dados sobre privacidade do Brave, caso ainda tenha dúvidas.

### Shields

| URL Interna | Propósito |
| --- | --- |
| `brave://shields/` | Central de configuração global do Brave Shields, onde é possível definir políticas de bloqueio de rastreadores, anúncios e scripts. |
| `brave://adblock/` | Gerencia listas de filtros de bloqueio de anúncios e rastreadores, permitindo adicionar, remover ou customizar regras. |
| `brave://privacy/` | Página dedicada às configurações de privacidade, incluindo permissões, cookies, rastreamento e controles de dados sensíveis. |

## Anti-paywall (burles.co, tampermonkey, Brave…)

Aqui explico com mais detalhes sobre o anti-paywall: [https://esli.blog.br/pulando-o-muro-do-paywall](https://esli.blog.br/pulando-o-muro-do-paywall)

%[https://esli.blog.br/pulando-o-muro-do-paywall] 

O https://burles.co é uma extensão que bloqueia a execução do script do paywall.

Pode ser instalado ou executado via tampermonkey (Instale a extensão do tampermonkey, habilite o modo de desenvolvedor e, por fim, abra a URL do burlesco .js e clique em instalar).

Outra forma de bloquear o paywall é via Brave, clicando no Shields e em modo avançado, clique para bloquear os scripts. Porém, aqui, serão bloqueados todos os scripts! Não somente dos que são responsáveis pelo paywall.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744751412630/8bb7ab4b-17ac-4930-a000-56a75cd94fbc.png align="center")

## NextDNS

NextDNS como seu DNS. Mesmo que tenha um Pi-Hole em seu home lab, adicione o NextDNS.

A versão gratuita limita a 1 milhão de requests ao mês, volume alto para uso comum. A versão paga custa R$79 ao ano (vale muito a pena!). Mas 4 ou 5 dispositivos em casa (roteador + laptops + celulares) atingirão dificilmente o 1 milhão ao mês de consultas (principalmente se ativar o cache).

Link: [https://nextdns.io/?from=yes2mwwr](https://nextdns.io/?from=yes2mwwr)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744751642206/8dd056f2-eb47-4389-8675-07ff5345579a.png align="center")

[As possibilidades de bloqueios,](https://nextdns.io/?from=yes2mwwr) listas, configurações são bem extensas.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744751730020/6c59463d-3a4a-4e88-879b-edb6136f2d8f.png align="center")

E novamente, assim como o Brave Browser, além de remover ads, irá contribuir com sua segurança:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744751813247/9e023f64-2480-4550-8153-1fcdb0ad380b.png align="center")

Configure o NextDNS em todos os seus dispositivos, configure como DNS Privado nas configurações do Brave!

Adicione o NextDNS como DNS Privado nas configurações avançadas do seu smartphone.

Configure em seu roteador, colocando um adblock por consequência em seus dispositivos como Alexa, SmartTVs e outros.

Se seu celular não permite mudar o DNS, pense em outra marca na próxima compra! Mas o fato de estar no roteador, já conseguirá os efeitos do bloqueio quando estiver conectado na rede local/wifi.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744753705542/43164b62-2738-4e43-8219-11731ef20b9a.jpeg align="center")

## YouTube

### VPN

Há países que não tem monetização, logo, criadores de conteúdo não recebem receita dos vídeos assistidos nestes países.

Com isto, conectar uma VPN em sua TV ou outro dispositivo para algum país que não possua monetização no youtube, poderá acarretar nenhuma propaganda exibida. Claro, pode haver confusões ou simplesmente o Google passar, mesmo assim, a exibir ads.

Costumo conectar ao Turcomenistão e Albânia, não há ads no youtube. Porém, às vezes, o youtube passa a exibir propagandas como se eu estivesse na França.

Aqui uma lista com os países em que não há monetização: [https://isthischannelmonetized.com/data/youtube-monetized-countries/](https://isthischannelmonetized.com/data/youtube-monetized-countries/)

Porém, já vi alguns criadores de conteúdo realizarem bloqueio do país (Geoblocking).

Usar VPN pode bloquear as propagandas no Youtube em seu smartphone, **mas o melhor mesmo é usar o Brave Browser e abolir o uso do App**.

### Bloqueando ads no Youtube de SmartTVs

Minha TV também está navegando com o NextDNS (no roteador), logo, já bloqueia as propagandas da Amazon no FireStick/FireTV e também no sistema do Android TV e Google TV

No Google TV (Android TV atualizado), instalei o sistema do Adguard, mas não obtive efeitos desejados. O NextDNS na rede e o SmartTubeNext são as melhores soluções para remover propagandas do Youtube na TV e remover as demais propagandas que aparecem no sistema da TV ou dentro de outros Apps.

### SmartTubeNext

Para FireTV, Android e GoogleTV em [https://smarttubenext.com/](https://smarttubenext.com/)

Instale o SmartTubeNext. No FireStick, habilite o modo desenvolvedor (vá em meu fire stick, clique varias vezes na info do firestick). Baixe o App Downloader (não tem como baixar via navegador padrão Silk do Firestick), dentro do app downloader, acesse o site do SmartTubeNext, faça o download dele e instale-o.

[https://smarttubenext.com/firestick/](https://smarttubenext.com/firestick/)

Ele irá bloquear todas as propagandas, inclusive conteúdo patrocinado nos vídeos!

O player e as configurações de exibição são altamente configuráveis.

Se não usa o Amazon FireTV Firestick, use este link para Android TV: [https://smarttubenext.com/android-tv-box/](https://smarttubenext.com/android-tv-box/)

### SponsorBlock

O segredo do SmartTubeNext e outros frontends para o youtube, tanto em SmartTVs ou qualquer outro dispositivo é o SponsorBlock: [https://sponsor.ajay.app/](https://sponsor.ajay.app/)

Ele está presente em vários apps como um componente que bloqueia ADS no YouTube em Android, AndroidTV, GoogleTV, userscript para o tampermoney e outros, players como o MPV, Kodi, Chromecast, Roku, AppleTV, IOS, sistemas de desktop (Windows, Linux e MacOS) e finalmente, como extensão para browsers. Veja a lista em [https://github.com/ajayyy/SponsorBlock/wiki/3rd-Party-Ports](https://github.com/ajayyy/SponsorBlock/wiki/3rd-Party-Ports)

Com o SponsorBlock, (em qualquer app, qualquer aparelho com qualquer sistema operacional) irá automaticamente pular as partes do video no youtube que sejam referentes a conteúdo patrocinado, animação de introdução, cards do final do vídeo e créditos, aquelas solicitações de inscrição/curtida/comentários, cortes com promoções pagas e publicidade no vídeo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746829301252/24db3c6d-aa21-4ab1-b0f3-b56bae2e5bc8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744747344304/8d4c6125-e942-452d-b30e-90007a585ee9.png align="center")
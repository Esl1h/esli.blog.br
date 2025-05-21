---
title: "VPN não é o suficiente"
datePublished: Tue Nov 08 2022 23:09:03 GMT+0000 (Coordinated Universal Time)
cuid: cla8tqhlq000508ji48fg5r49
slug: vpn-nao-e-o-suficiente
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/hIz3vbUbOoI/upload/v1667947679874/HxNJmVZHY.jpeg
tags: browsers, security, privacy, cryptography, vpn

---

As VPNs quase não oferecem nenhum benefício de segurança, pois mais de 95% dos sites que as pessoas realmente usam já suportam HTTPS, que criptografa a sua conexão. 

É importante observar que o uso de um provedor de VPN não o tornará anônimo, mas lhe dará melhor privacidade em determinadas (e raras) situações.

As VPNs só darão um benefício de segurança quando você estiver navegando em um site sem HTTPS, o que é raro. E uma camada de segurança em uma rede local não-confiável (como um wifi público, mas mesmo assim, VPN somente não irá te proteger de ameaças locais).

O marketing é lindo, mas uma VPN apenas cria uma rede privada e virtual, adicionando seu dispositivo ao servidor do provedor e, dele, para o resto da internet.

Uma VPN pode fornecer uma vantagem de privacidade, pois oculta os sites que você visita das pessoas que monitoram seu tráfego e oculta seu endereço IP dos sites que você visita (oculta da sua rede local, casa ou escritório por exemplo, e oculta do seu provedor de internet). 
No entanto, é importante entender as limitações disso. Ao usar uma VPN, você envia todo o tráfego da Internet para um único servidor e eles podem fazer o que quiserem com ele. Você tem que confiar totalmente neles para não fazer nada malicioso. Não há como garantir que seu provedor de VPN não registra. 

Muitas VPNs mentem sobre suas políticas de não registrar logs, como a IPVanish (cuja propaganda promove sua politica de log-zero, mas em 2016 entregou os logs de uso de um cliente durante uma investigação/processo criminal nos EUA). 

Então, o primeiro passo (impossível) é encontrar um provedor de VPN, que não registre logs e não queira vender ou ler seu tráfego na web (caso encontre algum, preciso de provas!).

O site privacyguides.org através [destes critérios](https://www.privacyguides.org/vpn/#our-criteria) sugere apenas 3 provedores de VPN seguros: Proton VPN, IVPN e Mullvad. O site privacytools.io lista (além do Proton e Mullvad) outros 4 provedores.

As VPNs também são muito vulneráveis ​​a ataques de análise de tráfego, nos quais informações confidenciais podem ser inferidas do tráfego da rede, apesar da criptografia.

## O que usar?

Não estou pregando ódio as VPNs, inclusive, tenho um provedor (pago!) e utilizo-o em algumas ocasiões.

As configurações e ações listadas abaixo podem ou não serem usadas juntamente com uma VPN.

Se você deseja privacidade e anonimato, use o Tor.
Usando o navegador Tor Browser ou a opção de janela anonima com Tor do navegador Brave.

Configure seu navegador para ser mais privado e seguro! Há diversas opções para aplicar e torna-lo mais efetivo, recomendo o Brave ou o Firefox. Tanto mobile ou desktop.

Extensão para bloquear scripts, ads, anti-fingerprinting, rastreadores, cookies cross-sites. O navegador Brave já possui, será apenas otimizar a configuração para um modo mais restrito. Já no FIrefox, use a extensão uBlock Origin.

Forçar o navegador a sempre usar HTTPS.

Usar serviços com criptografia ponta a ponta (E2EE) ou com no minimo TLS.

Configurar seu navegador (ou sistema operacional) para usar DNS over HTTPS (DoH) ou DNS over TLS, nas configurações há uma opção chamada "use secure DNS" e muito provavelmente as opções do Cloudflare, NextDNS e Quad9.




Referências:

Uso do HTTPS na internet: https://transparencyreport.google.com/https/overview

https://madaidans-insecurities.github.io/vpns.html

https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/encrypted-dns-browsers/

https://www.privacyguides.org/desktop-browsers/#recommended-configuration

https://www.cloudflare.com/en-gb/learning/dns/dns-over-tls/
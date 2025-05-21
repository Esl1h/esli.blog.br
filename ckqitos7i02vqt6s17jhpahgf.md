---
title: "Tipos de VPNs: PPTP x OpenVPN x L2TP/IPsec x SSTP x IKEv2 x Chameleon x WireGuard"
datePublished: Wed Apr 01 2020 02:37:00 GMT+0000 (Coordinated Universal Time)
cuid: ckqitos7i02vqt6s17jhpahgf
slug: tipos-de-vpns-pptp-x-openvpn-x-l2tp-ipsec-x-sstp-x-ikev2-x-chameleon-x-wireguard
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1625017877048/4jVx_vmnm.jpeg
tags: linux, security, router, privacy

---

Olá,

Baseando-me no formato do artigo sobre  [Certificados e a sopa de letras: HTTPS, TLS, SSL, HSTS, CA, PGP, GPG e OpenPGP](https://esli.blog.br/certificados-e-openssl-4b0c534f9a34) , com o artigo sobre o  [WireGuard](https://www.esli-nux.com/2020/03/wireguard.html) e a atual crise mundial do covid-19 que forçou muitos em quarentena a trabalhar remotamente, resolvi fazer um semelhante abordando os diferentes tipos de VPN.


O principal problema é que ao ler a documentação e artigos atuais, além de longos eles se prolongam muito no detalhe técnico entre elas, então tentei criar um TL;DR (que ficou um pouco grande, mas bem resumido).

Uma VPN, ou rede virtual privada, permite criar uma conexão segura entre duas redes ou entre seu dispositivo/host com alguma rede, usando a internet como meio, como túnel para chegar ao destino. As VPNs podem ser usadas para acessar sites restritos por região (países proíbem torrents, outros proíbem redes sociais, sites de noticias…), proteger sua navegação (em redes não confiáveis como hotéis, wifi de lojas, etc…), acessar um sistema corporativo que está instalado e disponível apenas na rede da empresa e muito mais….

As VPNs encaminham essencialmente todo o tráfego da rede para a rede, e é daí que vêm os benefícios — como acessar remotamente os recursos da rede local e ignorar a censura da Internet. A maioria dos sistemas operacionais possui suporte a VPN.

Este túnel, a rede privada construída entre hosts e/ou redes via internet ela é protegida por criptografia, além de outros fatores como chaves, autenticação, hashes… Isto basicamente é o que diferencia os vários tipos de VPN existentes.


## PPTP

Point-to-Point Tunneling Protocol, feito para encapsular pacotes PPP (ponto-a-ponto, famoso na internet discada). Antigo e obsoleto!
Ainda há muitos que usam por ser de fácil configuração. Funciona na porta 1723 (ou seja, se usar esta VPN, o administrador da rede saberá que você está em uma VPN e pode facilmente bloqueá-la).

Não use-o, totalmente inseguro! A Microsoft pede para não usar o PPTP desde 2011 (e tem gente que insiste!), e ele já vem sendo quebrado desde 1998 (todo o trafego da VPN pode ser capturado e visualizado).
Ao invés de usar um PPTP, a implementação de um proxy trará o mesmo beneficio com mais segurança.

Caso de uso: Em 2012 um funcionário da empresa passou a morar em outro país. Ele precisava acessar um sistema legado cujo client foi construído apenas para funcionar com o servidor em rede local (LAN). 
Um agravante foi a conexão ruim de internet que ele possuía. Como não havia riscos ao negócio rapidamente criei um servidor PPTP com o Debian e passei a configuração para o colaborador. Posteriormente, migrei para o PFSENSE. Quando um segundo funcionário passou a trabalhar 100% remoto também, começamos a pensar em migrar para o OpenVPN, mas felizmente, o uso do sistema legado foi abandonado ;-)


## OpenVPN

O OpenVPN usa tecnologias de código aberto, como a biblioteca de criptografia OpenSSL e os protocolos SSL v3 / TLS v1. Pode funcionar na porta 443 (HTTPS) ou seja, é mais difícil o rastreamento e bloqueio.

Suporta diversos tipos de criptografias e aparentemente bem configurado, ninguém conseguiu quebrar ainda.

O OpenVPN pode (além das cifras de cirptografias diferentes) implementado por UDP ou por TCP.
Através do UDP haverá uma leve melhoria na velocidade. Basicamente UDP (a melhor combinação de velocidade e segurança) e TCP (maior confiabilidade da conexão, mas velocidade reduzida).

É a combinação ideal de velocidade, segurança e desempenho

## L2TP/IPsec

Tão antigo quanto o PPTP porém mais seguro, o L2TP é um protocolo de tunel para a camada 2 OSI (enlace), não há como encriptar ela, por isto é associada ao IPsec.

Mas ele usa a porta UDP 500 — isso significa que não pode ser disfarçado em outra porta, como o OpenVPN. Portanto, é muito mais fácil bloquear e mais difícil contornar os firewalls.

Acredita-se que a NSA possa ter enfraquecido o IPsec com a finalidade de acessar e monitorar estes túneis, mas isto, diferentemente do backdoor instalado nos sistemas BSD, não foi comprovado AINDA pelo Wikileaks.

É uma solução mais lenta que o OpenVPN. O tráfego deve ser convertido no formato L2TP e, em seguida, a criptografia adicionada com o IPsec. É um processo de duas etapas, logo exige mais CPU.


## SSTP

Secure Socket Tunneling Protocol, protocolo proprietário da Microsoft lançado junto com o Windows Vista.
Funcionamento e segurança bem semelhantes ao OpenVPN e é integrado ao sistema operacional da Microsoft de forma nativa, visto que o OpenVPN é necessário fazer a instalação.

## IKEv2

Também conhecido como VPN Connect, Cisco Connect ou Any Connect ele é semelhante ao IPsec, porém é um formato proprietário da Cisco e Microsoft.

Mais rapido que o L2TP/IPsec e o SSTP. 
Existe o aplicativo OpenConnect (e a implementação dele para o NetworkManager no Linux por exemplo), trata-se do cliente open source para o protocolo.

Outra forma de implementar o IKEv2 IPsec é através do StrongSwan, que é de código aberto e amplamente disponível.

## Chameleon e KeepSolid Wise

Não é um protocolo, mas um serviço acoplado ao OpenVPN para dificultar sua rastreabilidade por parte de governos (ou por alguém com muito, mas muito dinheiro, tempo e disposição para inspecionar os pacotes transmitidos).
Desenvolvido pela Golden Frog, o Chameleon embaralha os pacotes de metadados do OpenVPN garantindo que não seja detectado (que existe um tunel VPN em uso) e mantem leve e rápido.

Outro semelhante ao Chameleon é o KeepSolid Wise, que também implementa tecnologias sob o OpenVPN para mascarar seu uso.
Ambos são utilizados por quem mora ou viaja por Irã, China, Rússia, Arabia Saudita, Síria, Indonésia dentre outros países (mas VPNs por lá são proibidas, podendo ter multas milionárias (Emirados Árabes Unidos) e até 1 ano de cadeia (Irã).

Diversos provedores de VPN (empresas que vendem conexões via assinatura mensal) oferecem estes dois aplicativos.


***Qual melhor VPN para usar?***
- OpenVPN

***E se meu dispositivo não tem suporte para OpenVPN?***
- L2TP/IPsec

***Não quero usar o OpenVPN (quero nativo para o meu S.O.) e estou no Windows (e não estou na China e nem trabalho no concorrente da MS)?***
- SSTP

***Quais os protocolos de VPN com melhor velocidade?***
1. PPTP
1. OpenVPN
1. IKEv2
1. L2TP/IPsec
1. SSTP

***Qual VPN com melhor segurança?***
- OpenVPN


> *NOTA: Para funcionar na China, a empresa precisa estar de mãos dadas com o governo, logo, visto o backdoor que a NSA colocou no BSD e as tentativas de quebrar a criptografia de alguns protocolos, muito provavelmente a Microsoft e a Cisco devem colaborar para quebrar o trafego de quem utiliza seus protocolos proprietários por lá (SSTP, IKEv2 e o Cisco/VPN Connect). Logo, estes juntamente com o PPTP definitivamente não recomendaria.*

## Wireguard

Mais seguro e mais rápido que o OpenVPN ele surgiu para substitui-lo. Este ano foi implementado no Kernel do Linux (ou seja, ele está a nível de kernel e não algo instalado no sistema operacional — apesar de que é possível instala-lo no S.O. até então). Versões para outr os sistemas ou estão em beta (Windows) ou estão usando alguma implementação escrita para o caso (nos *BSDs).

Possui alguns “poréns” que o impedem de ser implementado em todos os casos e situações.

# VPN no Linux

Como cliente o Linux possui amplo suporte aos protocolos acima, tanto em modo terminal (texto) quanto aplicações gráficas. A grande maioria dos servidores na qual o cliente da VPN irá se conectar está configurado em um Linux.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017871248/0i-gBVIkH.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017873094/8npYPe1K-.png)

## VPN no ChromeOS/Chromebook

Há suporte para os tipos de VPN: L2TP/IPSec e OpenVPN

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017875380/VCYNYiZ_N.png)

## VPN no Android

O Android 9 (vendido atualmente, sendo atualizado em alguns aparelhos para o 10) possui suporte tanto nativo quanto via Apps para os mais diversos protocolos de VPN.

Suporte Nativo a VPNs no Android
![VPN-android_nativo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625623902185/JRIxoVt6M.png)


Apps de VPN
![VPN-android_apps.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625623943789/dU1AokC33.png)


Lista de VPNs configuradas e disponíveis para conectar no Android, tanto nativos quanto via Apps

![VPN-android.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625623957546/9mMAz3MEl.png)


## Servidor de VPN

Não é o objetivo deste artigo mostrar como subir um servidor de VPN (fica para um próximo artigo, ou busque na internet, já tem milhões kkkkk)

A implementação deles deve-se tanto por instalar os pacotes oficiais via gerenciador e configurar ou utilizando alguma aplicação que já o implementa e adiciona algumas features (interface web, integrações com domínio, etc…)

Um exemplo é o SoftEther VPN Server, uma aplicação open-source capaz de disponibilizar diversos tipos de VPN com diversos recursos “acoplados”. Outro semelhante a ele é o Streisand.

## Outros tipos de VPN

Há outras implementações de túnel menos comuns, entre elas estão o UDP2raw e o Ping Tunel que encapsulam o trafego como se fossem ICMP (ping) ou como se fossem solicitações a um DNS server. Ou seja, para um administrador da rede ou sniffer, o usuário estaria fazendo ping em algum site ou solicitando ao seu DNS server qual o IP de algum nome de dominio, mas na verdade está trafegando. 
Estes dois exemplos de implementações são conhecidas por **VPN over ICMP** e **VPN over DNS**.

*Originally published at [https://www.esli-nux.com](https://www.esli-nux.com/2020/03/tipos-de-vpns-pptp-x-openvpn-x.html) on April 1, 2020.*
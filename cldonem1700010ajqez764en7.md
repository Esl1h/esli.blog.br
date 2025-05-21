---
title: "Hora do café #1 - O milagre do TCPDump"
datePublished: Fri Feb 03 2023 14:55:17 GMT+0000 (Coordinated Universal Time)
cuid: cldonem1700010ajqez764en7
slug: hora-do-cafe-1-o-milagre-do-tcpdump
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/TYIzeCiZ_60/upload/751906eb795f1f82656bcbabf625bee9.jpeg
tags: work, off-topic, stories, trabalho, experiencias

---

Certa vez um cliente não conseguia conectar com nossa API.

Algumas semanas de ticket, chamado, emails trocados, validando IPs, gateways e etc... Nada.  
  
Enfim, marcada a reunião para resolver a urgência já escalada para diretoria.  
  
Na ligação, 4 pessoas:  
Eu;  
O TI do Cliente;  
Um técnico terceiro que cuidava da infraestrutura do cliente;  
e um outro técnico, de outra empresa e que cuidava apenas do firewall deste cliente.  
  
Num dos últimos emails, havia enviado evidências de que, pelo nosso firewall, o bom e velho (*mais bom do velho*) iptables do Red Hat Enterprise 4 rodando em um cansado IBM estava com o IP do cliente devidamente liberado.

Seguiu-se o diálogo:  
*\- Bem, como enviei pelos emails, do nosso lado está liberado (o IP). Não tenho muito o que fazer. Se posso ajudar em algum teste é habilitando o TCP Dump numa porta e vocês testam, para identificarmos algo.*

Acho que foram abrir a caixa de entrada... Passou uns momentos em silêncio.

No que um dos técnicos responde:

*\- É possível deixar este tal TCP Dump sempre ligado? Tem problema? Porque na hora em que você habilitou ele, a conexão passou a funcionar...*

Alguém ali safou-se no último segundo. E conclui:

*\- Se conectou, então está resolvido.*
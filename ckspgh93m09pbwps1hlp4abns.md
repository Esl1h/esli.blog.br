---
title: "Otimização do sysctl.conf"
datePublished: Sat Jun 15 2019 02:31:34 GMT+0000 (Coordinated Universal Time)
cuid: ckspgh93m09pbwps1hlp4abns
slug: otimizacao-do-sysctl
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1629772386059/KGpmYqh7o.jpeg
tags: operating-system, linux, server

---

Utilizei este sysctl.conf por um bom tempo em servidores, desde databases (mysql, mariadb) a web servers (nginx, apache).

Basicamente é uma otimização, hardening tunning do sysctl para servidores Linux, onde já apliquei em SuSE (Open e Enterprise), Debian (6, 7 e 8), CentOS (6 e 7), Amazon Linux (1 e 2) e Ubuntu.

Possuía uma aplicação web numa VM que recebia um grande volume de conexões dos clientes, com o pico, geralmente travava e/ou parava de responder a novas conexões, aumentar os recursos da VM estava fora de cogitação, esta config do sysctl conseguiu suportar e resolver os alarmes que havia em relação ao servidor e manteve-se assim por mais uns 2 anos sem precisar alterar o hardware da VM e suportando o aumento de clientes.

Claramente, a config do sysctl para otimizar o seu Linux deve vir acompanhado de uma otimização das configurações do serviço em questão (apache, nginx, caddy, mysql, ...), num próximo post eu compartilho uma config padrão que utilizava para novos servidores web e banco de dados.

Personalize esta config de acordo com o volume de CPUs e RAM do seu servidor! Pois dependendo pode ser muito (vai parar de responder em algum momento) ou não o suficiente e manter o mesmo padrão de comportamento.

Postei como gist/snippet no  [github](https://gist.github.com/Esl1h/65c0d67780ee6212ebce00efe76d6007)  e  [gitlab](https://gitlab.com/-/snippets/1866467) 

Este script já me salvou e poupou stress em diversos casos, um dos que mais recordo foi prestando uma consultoria na qual o cliente estava com seu serviço degradado e com um possível ataque DDoS em andamento.
Seu produto era uma instância (ec2 na AWS) com o front no Apache, pico de CPU e RAM, sem acesso ao host (ssh). 
Os demais recursos (banco, ...) não estavam sendo afetados.

Após reiniciar e conseguir o acesso, habilitei a depuração de logs do sistema, apache, tcpdump, kdump, dentre outras ferramentas para analise.
O host não possuía memoria swap, criei o arquivo de swap e adicionei esta configuração do sysctl.

Com o swap, ao esgotar a memória disponível, o apache não tinha o comportamento estranho e com a analise do trafego, chamadas dos clientes e o bom e velho (mais bom do que velho) tcpdump, conclui que era um comportamento anormal da aplicação nos clientes que estavam onerando este apache (quando o client não conseguia chegar no servidor, ele entrava em modo 'furioso' e ficava tentando - mesmo sem ter nada para atualizar ou sem ação de usuário - logo, após alguns segundos de falha no tempo de resposta o servidor tinha milhares de clients tentando conectar a qualquer custo).



%[https://gitlab.com/-/snippets/1866467]


%[https://gist.github.com/Esl1h/65c0d67780ee6212ebce00efe76d6007]





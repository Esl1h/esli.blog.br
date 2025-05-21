---
title: "Gerar senhas no linux"
datePublished: Wed Apr 21 2021 23:17:01 GMT+0000 (Coordinated Universal Time)
cuid: ckqiud2tk02yot6s1g82w4i8w
slug: gerar-senhas-no-linux
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1625254199024/cwDDF11-X.png
tags: linux, command-line, passwords, linux-basics

---

### Gerar senhas fortes via terminal no Linux

%[https://youtu.be/nIcO1rFhJOI]

#### TL;DR

> **\# pwgen -Bsync 26 1**

  

O programa pwgen gera senhas projetadas para serem facilmente memorizadas por humanos, sendo o mais seguras possível.

Senhas memoráveis ​​por humanos nunca serão tão seguras quanto senhas completamente aleatórias.

> **\# apt install pwgen**

  

Via terminal, apenas executando o pwgen, ele irá gerar diversas senhas(comigo foram 160) todas com 8 caracteres.

Isto equivale ao comando:

> \# **pwgen -nc 8 160**

  

Por outro lado, as senhas geradas de forma completamente aleatória tendem a ser escritas e podem ser comprometidas dessa forma.

Nisto reforço a necessidade de usar um GERENCIADOR DE SENHAS! Não importa qual tipo ou formato, nunca confie num txt, doc ou papel.

O programa pwgen é projetado para ser usado interativamente e em scripts de shell.

Portanto, seu comportamento padrão difere dependendo se a saída padrão é um dispositivo tty ou um pipe para outro programa.

Quando a saída padrão (stdout) não é um tty, o pwgen irá gerar apenas uma senha, pois isso tende a ser muito mais conveniente para scripts de shell e para ser compatível com versões anteriores deste programa.

  

Os parâmetros que uso:

  

**\-B** - Não usa caracteres que possam ser confundidos, como 'l' e '1', ou '0' ou 'O'.

**\-s** - Gera senhas completamente aleatórias e difíceis de memorizar. 

**\-y** - Inclui pelo menos um caractere especial na senha.

**\-n** - Inclui pelo menos um número na senha

**\-c** - Inclui pelo menos uma letra maiúscula na senha

  
Após as opções, pode ser passado um numero para a quantidade de caracteres da senha e outro numero para a quantidade de senhas que ele vai gerar.

    # pwgen -Bsync 26 1
    s(hs=.C[e77inF~#r_Y:,&(o[>

Irá gerar uma unica senha, com 26 caracteres usando no minimo um numero, um simbolo, uma letra maiúscula e evitará caracteres ambíguos.
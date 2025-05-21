---
title: "LAPM - Leia A Porra do Manual"
datePublished: Wed Sep 28 2022 01:40:41 GMT+0000 (Coordinated Universal Time)
cuid: cl8kynpc3000409me0vqie2lo
slug: leia-a-porra-do-manual
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/4UGmm3WRUoQ/upload/v1664327935059/0yQfHeEXV.jpeg
tags: documentation, cheatsheet, cheat, manual, tldr

---

Nada de hipocrisia, OK?!
Na urgência, Google irá resolver mais rápido (e este é o objetivo, né!) - ou outro buscador (estou migrando aos poucos para o [brave search](https://search.brave.com) e [DuckDuckGo](https://duck.com)), ou até mesmo, para que ir neles sendo que o proprio [StackOverflow](https://stackoverflow.com/) tem sua barra de busca - e você querendo ou não cairá numa página dele?

Mas voltando....

O proposito deste post é mostrar alternativas (ou complementos) para as documentações e manuais.

## Linux/manpages

### manpages
- Consultar as manpages. 

Sem segredos, correto?

Mas vamos a alguns apps que podem te ajudar mais rapido:

### TL;DR

TLDR Pages (tldr-pages) é um projeto de documentação colaborativo que visa ser um complemento mais simples (leia-se resumo rápido e útil)  às manpages.

- website: https://tldr.sh/
- github: https://github.com/tldr-pages/tldr

Além do CLI é possível instalar cliente gráfico, bot no telegram, interface web (há diversas), uma delas é através do endereço: https://tldr.finzzz.net/

E até mesmo extensões para navegadores: https://github.com/piraces/tldr-extension-browser

Enquanto o `man unzip` irá retornar mais de 300 linhas! 


![manpages-unzip.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664322105108/nEGYwVDuM.png align="left")




O `tldr unzip` irá retornar (provavelmente) aquilo que estava procurando:



![tldr-unzip.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664322149326/XZfs8mn_Y.png align="left")




### Cheat

O Cheat é bem semelhante e uma alternativa ao TL;DR.
- github: https://github.com/cheat/cheat

Com uma estrutura mais simples e feito em Go (ao invés de usar nodejs como o tl;dr)

### most
O most vai apenas colorir a saida do comando man.

O most é um programa de paginação, onde além de exibir textos comuns, pode ler arquivos binários e de caracteres ASCII. O mais interessante é que ele reconhece sequencias de caracteres que ao serem exibidos criam algum efeito (desde que seja usado o programa correto para ler e interpretar estes efeitos dentro do texto, o que o most é excelente para isto).
Vale muito a pena ler um pouco mais sobre ele e seu uso, mas vamos nos limitar aqui para colorir as manpages do linux.

`apt-get install most`

Em seguida, basta abrir um terminal usando a conta de um usuário qualquer e configurar o most como o utilitário a ser utilizado pelo comando man para mostrar os manuais: 

`export MANPAGER="/usr/bin/most -s"`

Para fixar a configuração, insira, após instalar o most, a linha abaixo dentro do seu arquivo .bashrc (arquivo oculto no home do seu usuário): 

`export MANPAGER="/usr/bin/most -s"`


## Developers' offline documentation

### DevDoc

- Website: https://devdocs.io/

Disponivel via app, mobile ou web.

Basta escolher as documentações que queira offline que o download é feito ;-)

Apenas salve como "App" em seu navegador. Alguns gerenciadores de pacotes possuem ele para instalação (recomendo o flatpak).


![devdoc.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664327109398/4qiOiV7i4.png align="left")

### Zeal

- Website: https://zealdocs.org/

Somente possui o app para instalação (não há versão web ou mobile) e posteriomente instalado, deverá baixar as documentações que escolher ter offline (escolhe no menu), além da possibilidade de criar o seu proprio.


![zealdocs.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664327654538/83AP3rjP1.png align="left")









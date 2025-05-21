---
title: "Instalando o R no Linux"
seoTitle: "Guia: Instalando R no Linux"
seoDescription: "Aprenda a instalar o R no Linux e explore seus recursos de computação estatística e gráficos com este guia passo a passo"
datePublished: Sun Aug 21 2022 00:07:44 GMT+0000 (Coordinated Universal Time)
cuid: cl72kltf00509pinv09rtfelu
slug: instalando-o-r
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1661040161651/3gYrsAhbw.png
tags: r-language, r, programming-languages, r-cjxu2ot2i000u7os1rdapoiqi, install, linux-for-beginners, rlang, r-lang

---

# O que é o R?

R é uma linguagem para computação estatística e gráficos. É um projeto GNU que é semelhante à linguagem S que foi desenvolvido nos Laboratórios Bell (anteriormente AT&T, agora Lucent Technologies)

R pode ser considerado como uma implementação diferente de S. Existem algumas diferenças importantes, mas muito código escrito para S é executado sem alterações em R.

R fornece uma ampla variedade de técnicas estatísticas (modelagem linear e não linear, testes estatísticos clássicos, análise de séries temporais, classificação, agrupamento, …) e gráficas, e é altamente extensível.

Um dos pontos fortes do R é a facilidade com que gráficos de qualidade de publicação bem projetados podem ser produzidos, incluindo símbolos matemáticos e fórmulas quando necessário.

Manuais oficiais em pdf, html e epub: https://cran.r-project.org/manuals.html

# O que é o CRAN?

CRAN é uma rede de servidores ftp e web em todo o mundo que armazenam versões idênticas, pacotes e atualizadas de código e documentação para R. Use o espelho CRAN mais próximo de você para minimizar a carga da rede.

Uma lista dos pacotes disponiveis: https://cran.r-project.org/web/packages/available\_packages\_by\_date.html

## Instalação

Procure por `r-base` no gerenciador de pacotes da sua distro. It's Only ;-)

**Neste momento, a versão presente no Ubuntu é a 4.1.2. Porém, o release atual do R é o 4.2.1 (lançado em 2022-06-23).**

Instalação do repo e versão atual (no Ubuntu):

atualize:

```c
sudo apt update -qq
```

instalar dois pacotes auxiliares que precisamos:

```c
sudo apt install --no-install-recommends software-properties-common dirmngr
```

adicione a chave:

```c
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
```

verifique a chave (opcional):

```c
gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
```

O resultado será:

```c
pub   rsa2048 2010-10-19 [SCA] [expires: 2027-09-30]
      E298A3A825C0D65DFD57CBB651716619E084DAB9

sub   rsa2048 2010-10-19 [E] [expires: 2027-09-30]
```

adicione o repo:

```c
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
```

Instale o R.

```c
sudo apt install --no-install-recommends r-base
```

Site: https://www.r-project.org/

# Validando a instalação

Execute:

`R --version`

A saída:

```c
R version 4.2.1 (2022-06-23) -- "Funny-Looking Kid"
Copyright (C) 2022 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)
```

# Testando o R

## Testando

Acesso ao console do R:

sudo -i R

## Teste 1 - txtplot

Dentro do console do R, você pode instalar pacotes via CRAN (CRAN é para o R, assim como o pip é para o python, npm é para o node ou o cpan é para o Perl) - ok nem tanto, resumo: um quase gerenciador de pacotes.

No console do R:

> install.packages('txtplot')

O txtplot é uma biblioteca que gera gráficos ASCII, incluindo gráficos de dispersão, plotagem de linhas, plotagem de densidade, acf e gráficos de barras.

Depois de instalado, podemos carrega-lo:

> library('txtplot')

E gerar um exemplo:

> txtplot(cars\[,1\], cars\[,2\], xlab = "velocidade", ylab = "distancia")

```c
>  txtplot(cars[,1], cars[,2], xlab = "velocidade", ylab = "distancia")
      +----+-----------+------------+-----------+-----------+--+
  120 +                                                   *    +
      |                                                        |
d 100 +                                                   *    +
i     |                                    *                *  |
s  80 +                          *         *                   +
t     |                                       * *    *    *    |
a  60 +                          *  *      *    *      *       +
n     |                        *         * *  * *              |
c  40 +                *       * *    *  *    * *              +
i     |         *      *  * *  * *  *                          |
a  20 +           *    *  * *       *                          +
      |  *      *    *                                         |
    0 +----+-----------+------------+-----------+-----------+--+
           5          10           15          20          25   
                             velocidade
```

## Teste 2 -stringr

Outro pacote para primeiros códigos com R:

> install.packages("stringr")

> library(stringr)

criando um vetor simples de caracteres chamado 'tutorial':

> tutorial &lt;- c("How", "to", "Install", "R")

Todas as funções do stringr possuem o prefixo `str_`

> str\_length(tutorial)

> str\_to\_lower(tutorial)

> str\_to\_upper(tutorial)

> str\_to\_title(tutorial)

Caso esqueça o nome de uma função, basta digitar `stringr::str_` e apertar TAB para ver quais são as opções.

# IDE's

Melhores IDE's para R:

Ok, não há muitas, então fica fácil escolher...

* RStudio (o melhor!) - https://www.rstudio.com/products/rstudio/
    
* VSCode (c/ extensões R, R Tools e R Debugger)
    
* VIm
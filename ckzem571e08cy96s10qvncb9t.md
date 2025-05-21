---
title: "marca d'água em arquivo PDF"
datePublished: Tue Feb 08 2022 21:06:24 GMT+0000 (Coordinated Universal Time)
cuid: ckzem571e08cy96s10qvncb9t
slug: marca-dagua-em-arquivo-pdf
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/gm18kqu9TxQ/upload/v1644354201602/jOi2Jj2ZT.jpeg
tags: documentation, pdf

---

Olá,
Recentemente precisei compartilhar um arquivo PDF, porém, ele possui em cada pagina (mais de 600!) o meu nome e e-mail no formato de marca d'água e também no rodapé.

Além disto, meu CPF era a senha para abrir ele (visualizar).

Abaixo descrevo o passo-a-passo de como remover marca d'água e como remover a senha de arquivos PDFs.

## Removendo a senha de arquivo PDF

Tirar a senha.
Simples, abri ele com o leitor padrão da minha distribuição (tive que colocar a senha), porém, com ele aberto, cliquei em imprimir (e desta vez, salvei em PDF, sem senha).

Pronto!

## Removendo marca d'água e rodapé de arquivo PDF

1 - O PDF foi convertido para o formato .Epub usando o Calibre.

O Epub nada mais é que um arquivo zipado contendo um index.html + xml com a estrutura do livro.

2 - Descompacte e acesse o diretório.
Com os comandos abaixo eu removi o texto dos rodapés e da marca d'água:

```
sed -i 's/Texto\ //g' *.html
sed -i 's/Texto\ //g' *.html
``` 



Com o SED acima, removi os textos (substitua "Texto" para o que quer remover, lembrando de 'escapar' qualquer caractere especial).

Se o que for remover também estiver dentro do conteúdo (no texto do livro), será apagado também, ou seja, terá que otimizar o REGEX para remover apenas a marca d'água do PDF ou o rodapé.

Pronto!
Simples, rápido e fácil.

## O que fazer agora?

Você pode zipar novamente para a extensão e abrir num e-reader (no meu caso, para o Kobo e Kindle), ou utilizar o Calibre para transformar em PDF de volta, mas desta vez com o PDF sem a marca d'água, sem rodapé e sem senha.

Lembre-se que nem sempre isto irá funcionar, se o PDF foi gerado por imagens digitalizadas (scanner) ou passou por algum outro tipo de tratamento ou proteção, provável que não funcione.
---
title: "RAM e Swap"
datePublished: Sun Oct 31 2021 16:53:27 GMT+0000 (Coordinated Universal Time)
cuid: ckvfh2q4e0g7xbas16z3z8qlc
slug: ram-e-swap
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1635698811556/Eyo8--wen.jpeg
tags: optimization, linux, server, linux-for-beginners, linux-basics

---

## SWAP como arquivo

- As vantagens:


A partir do kernel Linux 2.6, o swap em arquivo passou a ter o mesmo desempenho do swap em partição.

A redução da quantidade de partições em disco, tornando mais fácil a administração do mesmo.

A possibilidade de aumentar ou diminuir, rápida e facilmente, a área de swap (sem reiniciar ou particionar).

A possibilidade de gerar, de forma simples e on-line, diversas áreas de swap por SO instalado (o kernel 2.6 suporta até 32 áreas).



- Instalação do GNU/Linux (Debian):

Ao instalar o S.O. não crie partições de swap. Ao finalizar o particionamento, será dito que não há uma partição de swap e será perguntado se você deseja voltar ao menu de particionamento.

Responda não e o instalador passará para a próxima etapa.

Após instalado o sistema e funcionando:

Com a ferramenta dd, crie na raiz do sistema um arquivo do tamanho do swap desejado. Use o nome swapfile ou nome "swap”, para num futuro, não apagar sem saber o que é.

Saiba que as partes desse arquivo são criadas dentro da RAM e transferidas para o disco. Então, crie partes inferiores à quantidade de RAM livre. Por padrão, use blocos de 100 MB. Assim, caso deseje um swap de 500 MB, você precisará de 5 blocos de 100 MB.

Criando arquivo vazio com 100MB de tamanho de bloco 20 vezes (resultando um arquivo de 2GB): 

```
dd if=/dev/zero of=/swapfile bs=100M count=20

``` 


Depois de criado o arquivo, marque o mesmo como área de swap e dê permissão como 600:
``` 
mkswap /swapfile && chmod 600 /swapfile
``` 

Para fazer com que a área de swap seja habilitada durante o boot do sistema, edite o arquivo /etc/fstab e insira no final do mesmo:

 ```       
/swapfile    none    swap  sw     0    0
``` 

Você poderá ter mais de uma área de swap em arquivos como /swapfile2, /swapfile3 etc.
 

Testando o swap:

``` # free -m | grep Swap```    ---> antes de ativar, não há o swap

``` # swapon /swapfile```           ---> ativa o swap

``` # free -m | grep Swap```     ---> verifica memória livre, após ativar o swap.





## Melhor desempenho da RAM/SWAP

Objetivo: Determinar através do kernel (sysctl) quando o sistema deverá utilizar a memória swap.

Com isto, o linux vai usar mais a memória RAM e dar prioridade a ela, ao invés de levar isto para o HD (swap) e deixar alguns processos mais demorados.

Por padrão, o valor de swappiness no debian é 60. Ou seja, usará o swap quando a RAM estiver em torno de 40% a 50% em uso.

Verificar valor padrão:
``` 
cat /proc/sys/vm/swappiness
``` 

Reduzindo o valor de swappiness para 10 ou 15 (neste exemplo, reduzi para 5), o arquivo de swap será usado apenas quando o uso da RAM chegar em torno de 80 a 90 por cento.

Edite:

``` # vim /etc/sysctl.conf``` 

Altere (adicione se não existir a linha) no arquivo:

``` vm.swappiness = 5``` 

(Há quem coloque 0 ou 1, mas prefiro assim)


Para evitar a necessidade de reiniciar o sistema, execute:

``` 
sysctl vm.swappiness=5
``` 


depois, apenas como verificação, execute:
``` 
swapoff -a
swapon -a
sysctl -p /etc/sysctl.conf
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635698053295/EP83rNRzA.png)

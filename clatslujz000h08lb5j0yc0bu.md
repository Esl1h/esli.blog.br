---
title: "Transformando um laptop antigo em Chromebook"
datePublished: Wed Nov 23 2022 15:20:37 GMT+0000 (Coordinated Universal Time)
cuid: clatslujz000h08lb5j0yc0bu
slug: transformando-um-laptop-antigo-em-chromebook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1669216135863/RUjcpE1pe.png
tags: chromebook, chromeos, old-desktop, chromeos-flex

---

A empresa Neverware faz parte do Google e anteriomente disponibilizava o 'CloudReady' para instalar o ChromeOS em laptops comuns, agora possui uma ferramenta oficial e a versão do SO é chamado de ChromeOS Flex.

O foco é em equipamentos antigos mas não chega a ser uma regra nem pre-requisito. 

Confira no vídeo como está atual versão do ChromeOS e a ativação do Linux Beta nele:

%[https://youtu.be/0nCP6a9OISg]


Aqui vai uma lista de melhores Apps no ChromeOS para quem trabalha com TI, tanto apps online quanto offline:

[ChromeOS — Chromebook para TI: Melhores Apps Online e Offline](https://esli.blog.br/chromeos-chromebook-para-ti-melhores-apps-online-e-offline)

E umas IDEs para usar no ChromeOS:

[Algumas opções de IDE online e offline para desenvolver no ChromeOS
](https://esli.blog.br/programando-no-chromebook-melhores-ides-para-o-chromeos)


Além de leve e super rápido em dispositivos que já suavam em rodar o básico do Windows 7 e tremiam em ouvir falar do KDE e GNOME, o formato do ChromeOS permite que ao terminar a instalação, já começe a usar!! Nada de configurações, painel de controle ou aplicações para gerenciar o hardware, rede, etc... Ele já está pronto para uso.
Ao logar numa conta Google, ele sincroniza tudo que você possui atrelado a conta, trazendo seus plugins, Apps, Favoritos, etc. Se o usuário for um utilizador de Smartphone Android, já se sentirá familiarizado em apenas 20 segundos de uso (ou menos).
É o sistema perfeito para leigos que possuem fácil acesso a internet mas ainda "patinam" ao tentar usar um S.O. convencional, robusto e com milhares de aplicações e ferramentas já instaladas.
Por outro lado é também um excelente sistema para quem já está 100% em nuvem e é um "Hard-user" das soluções do Google.



## Nem tão antigos assim

O alvo são maquinas com até 10 anos de uso, que chegaram ao mundo no fim da era Xp, tiveram seus discos rodando o Vista e, com muito sofrimento estão a rodar o Windows 7 ou há muito tempo já passaram para alguma distribuição GNU/Linux.

Em tentativa de instalar em notebooks bem antigos, não obtivemos sucesso, porém com uma distro GNU/Linux no mesmo, a instalação ocorreu sem problemas (irei criar um post com as melhores distros leves para PC/laptops bem antigos ou para usar visando obter melhor desempenho).
No próprio site da neverware (forum de ajuda) eles não encorajam a instalação em equipamento muito antigo.
Para uso individual é gratuito, para empresas custa US$49 anuais para cada equipamento (além  de alguns recursos de administração 
centralizada).

O consumo da bateria impressiona, obtivemos grande tempo de autonomia nos equipamentos com o ChromeOS onde, em seus S.O.s antigos não chegavam a menos de 30% do tempo de agora.


Testes e Uso:
Equipamentos que instalamos e estamos usando há uns 2 meses até a data que escrevemos este review/artigo:

 - Dell Inspiron N4030 
Com processador Intel dual core Pentium P6100 de 2.0 Ghz
3Gb de RAM

 - HP/Compaq Presário CQ40
Com Processador Intel Core 2 Duo T6600 de 2,2 GHz
2Gb de RAM


Ambos adquiridos em 2010.


![dell-n4030-chromeos.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1669216563611/YHMx2f8FH.jpg align="left")



![hp-laptop-chromeos.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1669216572505/fFaeNzvt_.jpg align="left")


## Instalação

Download:

https://www.neverware.com/freedownload


Para instalar você irá precisará primeiro criar o USB de boot/instalação, e isto só é possível a partir de u computador com o navegador Chrome e S.O. Windows (o app do chrome que cria o disco bootavel só funciona no Windows).
O pendrive deverá ser de 8Gb ou 16Gb.

Após baixar o arquivo zip e descompactar o arquivo .bin, você precisará abrir o seu Google Chrome e instalar o app (webstore do chrome) chamado "Chromebook Recovery Utility", no seguinte link:

https://chrome.google.com/webstore/detail/chromebook-recovery-utili/jndclpdbaamdhonoechobihbbiimdgai?hl=en

Ele permite criar um arquivo de recuperação da instalação do ChromeOS/Chromebook.
Conecte o pendrive e abra o App.

Ao inicia-lo, clique na engrenagem (logo superior direito) e escolha "Use local image" para usar o arquivo .bin como imagem de disco para a instalação.
Selecione a imagem que foi feito o download e descompactada, selecione qual a unidade do pendrive vazio e inicie o processo (irá demorar!!).


Ao finalizar, volte para o seu PC/notebook alvo e inicie o boot via pendrive para a instalação ou teste via "live".



**Boot pela USB em PC que não suporta** 

Se seu laptop não inicializa via pendrive, grave a ISO do Plop Boot Manager (plpbt.iso) disponível em https://www.plop.at/en/bootmanager/plpbt.bin.html com ele, ao dar boot pelo CD, você entrará num boot manager que consegue, entre outras coisas, iniciar um boot via USB mesmo que o computador não suporte isto.


## Serviços em Nuvem

Todo e qualquer serviço via navegador e/ou que possua seu app ou plugin para o Google Chrome irá funcionar!
Ele possui um File System para arquivos baixados, além de ter a opção do Google Drive em manter arquivos Offline.

Além do Google Drive, é possível associar ao seu "File Manager Cloud" serviços como o Dropbox, Box, OneDrive além de qualquer diretório compartilhado via FTP/SFTP, Windows Share, WebDAV, Samba, dentre alguns outros...

Há diversas aplicações que rodam de forma "standalone", mas a maioria, ao clicar no ícone, irão abrir uma nova aba no seu Chromebook.

Obviamente, quase tudo é online, porém dependendo da finalidade você encontrará excelentes aplicativos que funcionam Offline (inclusive o proprio Gmail pode manter os emails offline no seu chromebook). Estou preparando um post com as melhores aplicações offline para usar no ChromeOS/Chromebook.


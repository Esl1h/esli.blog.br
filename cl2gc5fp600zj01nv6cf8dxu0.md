---
title: "Guia SSD no Linux"
datePublished: Tue Apr 26 2022 16:01:18 GMT+0000 (Coordinated Universal Time)
cuid: cl2gc5fp600zj01nv6cf8dxu0
slug: guia-ssd-no-linux
canonical: https://www.esli-nux.com/2017/04/ssd-no-linux.html
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650988868553/fHW_rgPTg.jpg
tags: desktop, linux, benchmark, hardware, linux-basics

---

SSD no Linux: tudo que você precisa saber e o que precisa esquecer!


SSD são suportados no Linux desde o kernel 2.6.29.

Schedulers e File Systems também suportam os 'discos sólidos' ou 'não-rotacionais' (SSDs) há um bom tempo.

A maioria dos artigos que existem na internet são bem antigos e não refletem os ambientes atuais dos sistemas Linux.

Este artigo trás alguns macetes para otimizar o SSD num ambiente onde o sistema operacional estará instalado nele.

Tiro alguns mitos de que seria necessário mudanças bruscas no sistema para que o SSD seja bem aproveitado (hoje, basicamente no uso do dia-a-dia, nada é preciso após instala-lo) apenas alguns pontos a serem observados.

Eles são identificados automaticamente e funcionam perfeitamente sem nenhum problema caso seu S.O. esteja em bom estado e atualizado (bem como os demais hardwares do computador).
Ao rodar o seguinte comando, o retorno '1' é para HD e 0 para SSD.

```
cat /sys/block/sda/queue/rotational
```

(neste caso, estou verificando o dispositivo 'sda').

Havia um antigo receio de que o GNU/Linux poderia diminuir a vida útil dos SSDs, visto algums percalços no desenvolvimento e implantação do  suporte a eles nos diferentes programas que estão ao entorno do kernel e o próprio kernel em sí.

Vale lembrar que o Linux já foi acusado de estar destruindo os HDs (sobrecarregando-os) há 10 anos atrás, num problema que apareceu nos fóruns do ubuntu 7 e que acabou com os fabricantes de disco assumindo a culpa, porém a imagem errada pegou no Linux e ficou (https://www.theregister.co.uk/2007/11/01/ubuntu_laptop_disk_issues/)


## MBR ou GPT

Ao criar a tabela de partição para formatar o SSD (e posteriormente inserir o sistema de arquivos, S.O. , etc...) o formato deve ser o GPT, ao invés do antigo, bom e velho (mais bom do que velho) MBR.


Porém, ao instalar a maioria das distros GNU/Linux, em mais de 95% delas, irá detectar o SSD e habilitar em GPT durante a instalação. Exceto se você forçar em outro formato.


Ou seja, neste quesito: Se você estiver usando uma distro atualizada e conhecida (ou derivada de uma das 4 grandes), não precisará fazer nada.


## Escolha do File System: Btrfs vs Ext4 vs F2FS vs JFS vs Outros...

O filesystem deve possuir suporte ao TRIM, no kernel ele está presente desde o 2.6.
Os seguintes FS possui suporte ao TRIM: Ext4, Btrfs, JFS, XFS e F2FS.

Nos testes de performance com o kernel 4.0 o FS com melhor desempenho foi o F2FS, porém ele ainda é experimental e não está nos repositórios de algumas distribuições linux. Em ordem de desempenho, posteriormente vem o Btrfs e o Ext4, em alguns testes o XFS mostrou desempenho melhor que os outros, porém no geral considero-o em 4º atrás do Ext4.

Neste momento, visto o desenvolvimento destes 4 file systems e dependendo da distribuição que escolher opte primeiramente pelo Btrfs ou Ext4.
Ou seja, Debian, Ubuntu e derivados, continue no padrão do Ext4 Já com RHEL, Fedora e derivados, mantenha no padrão Brtfs.
Se quiser se aventurar no JFS, XFS e F2FS manda brasa, mas não terá melhoria alguma (pior, perderá!)

Ou seja, neste quesito: mantenha no padrão (ou no que já conheça).

## Suporte ao TRIM

O recurso TRIM limpa os blocos do SSD que não são mais usados pelo sistema de arquivos. Isso irá otimizar o desempenho de gravação a longo prazo e é recomendado em SSD devido ao seu design. Isso significa que o sistema de arquivos é capaz de dizer ao SSD sobre esses blocos.

A opção 'discard' ao montar um ext4 emitirá tais comandos TRIM quando os blocos do sistema de arquivos forem liberados (chama-se descarte online).
No entanto, esse comportamento implica um pouco sobrecarga de desempenho. Desde o Linux 2.6.37, você pode evitar o uso da opção 'discard' e fazer isto ocasionalmente com o FITRIM (por exemplo, a partir de um agendamento no crontab).
O utilitário fstrim também faz isso (on-line), bem como a opção '-E discard' do fsck.ext4.

Já com o Btrfs existe a opção na montagem de passar o parâmetro 'ssd'

Tanto para o Btrfs e Ext4 passe as opções 'noatime', 'notail' e 'nodiratime'.
O 'noatime'  faz com que o sistema não atualize as propriedades dos arquivos quando eles são acessados, apenas quando são alterados, o que melhora bastante o desempenho. 
O 'nodiratime' desativa o registro de data e hora para diretórios (colocando o nodiratime já implica no noatime).
O 'notail' desabilita uma opção na qual o Filesystem usa o espaço sem uso de um bloco para armazenar o excesso de dados de outros blocos, ganhando assim mais espaço de armazenamento, porém cobrando a conta no quesito desempenho, desabilitando-o pode ser uma boa, porém não há ganhos significativos.

Estas duas opções também são aconselháveis para aumentar o desempenho em discos HD comuns e na qual você não necessita destas informações de data/hora dos acessos.

## relatime X noatime/nodiratime

O noatime e o nodiratime desativam o 'atime', porém alguns aplicativos fazem uso dos dados atime, desligar este recurso pode gerar problemas pois alguns aplicativos contam com os dados do atime e falharão caso não esteja disponível.

O kernel usado no Red Hat Enterprise Linux 6 suporta outra alternativa, relatime. Relatime mantém dados do atime , mas não para cada vez que um arquivo é acessado. Com esta opção habilitada, os dados do atime são gravados no disco somente se um arquivo foi modificado desde que os dados do atime tenham sido atualizados (mtime), ou se o arquivo foi acessado pela última vez mais de um certo período de tempo atrás (por padrão, um dia).

Por padrão, todos os sistemas de arquivos são montados agora com o relatime habilitado. Para omitir este recurso em todo o sistema, use o parâmetro de inicialização default_relatime=0. Se relatime estiver ativado em um sistema por padrão, você pode omití-lo de qualquer sistema de arquivo específico montando aquele sistema de arquivo com a opção norelatime. Finalmente, para variar o período de tempo padrão antes do qual o sistema irá atualizar dados do atime de arquivo, use o parâmetro de inicialização relatime_interval= especificando o período em segundos. O valor padrão é 86400.

Ou seja, num sistema baseado no RHEL, ao invés de desativar o atime (com o nodiratime), instalar o fstrim e criar uma tarefa no CRONTAB para realizar ação, você simplesmente usa o relatime e insere um valor de tempo maior.

Porém, ao instalar a maioria das distros GNU/Linux, em mais de 95% delas, as opções 'discard' e 'noatime' já estarão ativas pois o instalador sabe que está sendo usado um disco SSD.

Ou seja, neste quesito: Novamente, se você estiver usando uma distro atualizada e conhecida (ou derivada de uma das 4 grandes), não precisará fazer nada.



## Schedulers: CFQ vs Deadline vs Noop

O CFQ (Completely Fair Queuing) é o scheduler padrão de I/O do Linux desde o kernel 2.6.18 ele é quem coloca as solicitações síncronas enviadas por processos em filas numeradas e aloca o tempo para que cada uma destas filas acessem o disco, controlando entre outras coisas, a prioridade do acesso ao disco e a largura de banda para I/O.

A partir do Kernel 3.1 foi introduzido algumas opções de otimização no CFQ para SSDs e outros drives 'não-rotacionais' ('discos' que não são discos, como memórias flash, etc...) onde ele detecta este tipo de dispositivo e suporta múltiplos requests por vez e uma profundidade maior da fila. Seu funcionamento padrão em discos comuns é organizar as filas de acordo com setor e proximidade (lembrando que é um disco girando, sendo lido e gravado).

Existe outros schedulers no Linux, como o Deadline e o Noop. Para SSD o CFQ se mostra com o melhor desempenho, sendo este o padrão da maioria das distribuições.
Há na internet diversos artigos onde mostram como alterar o sistema para que use o Deadline em discos não-rotacionais (SSDs), porém não encontrei nenhum argumento que justifique isto.

Neste link, há uma comparação dos 3 schedulers no kernel 3.4 usando disco convencional (HDD) e SSD: http://www.phoronix.com/scan.php?page=article&item=linux_iosched_2012&num=1

Um dos argumentos é a opção fifo_batch do Deadline que controla o numero de pedido por lotes de solicitações de I/O no disco, com isto ele equilibra as taxas de latência vs transferência, focando a latência (pois o disco está girando e leva em consideração a proximidade do setor), porém ao inverter (já que no SSD não tem disco) você prioriza a taxa de transferência, em tese, melhorando o desempenho. Porém, mesmo assim, em benchmarks que verifiquei, o CFQ continua melhor e o ganho de transferência no Deadline com esta ação não produz uma melhoria visível.

Ou seja: Neste quesito, não faça nada.

## Journaling

O Journaling cobra um pouco no desempenho, mas isto é um recurso muito importante para a recuperação dos dados em possíveis crashs no sistema.
Aí depende muito de cada um em desativa-lo, eu acho desnecessário (depende muito de qual uso terá este GNU/Linux com SSD).
Se em seu ambiente você terá um disco HD junto com SSD no Linux, você pode configurar para que o sistema use outro device ou partição para o Journal, apontando para uma área do HD (no mkfs.ext4 ao criar a partição do sistema que será instalado no SSD, use a opção '-J' para as configurações do journal, entre elas há um "device=" onde você aponta um outro dispositivo ou partição para usar como Journal, apontando assim, para o HD comum).

Caso mude o journal no ext4, habilite na montagem do SSD, além dos parametros já passados, os seguintes:
"journal_checksum" e o "journal_async_commit"

Neste queisto é opcional. Muitos o fazem, porém o ganho de vida útil com os atuais SSDs não será perceptível.


## Updates

Kernel: O mais atual possível!
No benchmark de qual melhor File System, foi testado também os kernels 3.19 e 4.0, a melhoria no desempenho é gritante. Mantenha seu linux atualizado via repositório, não necessariamente você deva ser radical para usar versões unstable e/ou ficar compilando o que foi liberado horas antes (o perigo costuma morar aqui também).

## Firmware do SSD

Mantenha atualizado também o firmware do seu SSD, geralmente cada fabricante distribui arquivos .ISO para atualizar o seu firmware, porém a forma de fazer este update muda em cada fabricante.

Não vou me estender sobre o benchmark, o link do excelente artigo na Phoronix (do Michale Larabel) é http://www.phoronix.com/scan.php?page=article&item=linux-40-ssd&num=1


## Memoria SWAP

Não crie uma partição para memória SWAP na instalação do sistema.
Você pode não ter SWAP no seu SSD ou criar um SWAP como arquivo e otimizar a configuração do sysctl.conf para que apenas use a SWAP quando a RAM estiver em 95% ocupada ou mais.
No artigo abaixo, crie arquivo para usar como memória SWAP e não ter que criar uma partição para isto: https://esli.blog.br/ram-e-swap

Neste arquivo a seguir, você configura o uso do swap, colocando o swappiness em 0, você desativa, neste artigo usei o valor 5, ou seja, o swap só entra em ação com a RAM em 95% de carga.
Lembrando que no Debian por exemplo, este valor é em 40% (o swappiness é 60).

Neste ponto, durante a minha instalação foi o único que eu alterei.
A distribuição iria criar uma partição SWAP, então eu cancelei e não fiz isto, para que posteriormente, criasse via arquivo (seguindo o artigo acima) e colocando o swappiness em 0 (zero).

## tmpfs

Para reduzir ainda mais escritas desnecessárias no SSD (se tiver um SSD de 120Gb instalado com o S.O. padrão, sem nenhuma otimização e diariamente gravar e apagar 20Gb de dados no SSD, seu SSD ainda vai tranquilamente funcionar por uns 5 anos ou mais), pode-se mover o diretório /tmp para uma ram disk (temporary Filesystem) utilizando o sistema de arquivos ‘tmpfs’ que é expandido e reduzido dinamicamente quando necessário.

No /etc/fstab:

```
tmpfs /tmp tmpfs defaults,noatime,mode=1777 0 0
tmpfs /var/tmp tmpfs defaults,noatime,mode=1777 0 0
tmpfs /var/log tmpfs defaults,noatime,mode=0755 0 0
```

Esta ultima linha, você abre mão dos logs, perdendo-os a cada reboot! Caso use algum programa que utiliza ou necessita dos dados dentro do diretório /var você pode remover esta linha e inserir manualmente no fstab cada um dos subdiretórios como tmpfs e excluir aqueles que necessita.

Basicamente, numa distro atual (e num SSD novo), apenas fiz 2 alterações (opcionais) na qual eu coloquei alguns diretórios em tmpfs (alterações no fstab), não instalei a SWAP (colocando como arquivo posteriormente) e alterando alguns parâmetros do sysctl.conf para otimizar o uso da memoria.

Muitos artigos que há na internet foram feitos para a tecnologia de 5 anos atrás (ou mais) onde, não só os equipamentos estavam inferiores, mas também as aplicações e o kernel não estavam maduros o suficiente para suporta-los.

Hoje, basta apenas instalar fisicamente e depois, instalar seu Linux.

Podemos passar a era do 'SSD x funciona no linux?' Igual a comunidade passou e enterrou com orgulho a tenebrosa era do "mouse/teclado X funciona no linux?", "impressora Y funciona no Linux?", "Placa-mãe Z roda no Linux?"...

Abaixo o meu laptop ainda com o seu disco comum (Western Digital) de 500Gb.


![laptop-hd.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1650988495445/ye7RNbFK0.jpg)


Troca efetuada pelo SSD Kingston HyperX Savage.

Neste modelo, junto com o SSD vem junto um case para transformar em USB3.0 (na qual coloquei o disco WD que sobrou na troca, ganhando um disco externo), também vem um adaptador para inserir num gabinete 3,5', bem como os cabos sata e 3 chaves para realizar a troca física.


![hyperx.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1650988526495/dxOntIzy_.jpg)


Apesar de ter feito algumas screenshots da instalação do S.O., não achei necessário estender este artigo.
Neste meu notebook usei para os testes o Debian 8, LinuxMint 18.1, usei algumas distros em Live USB (Xubuntu 17.04 beta e 16.10).
Estou usando como S.O. default para o dia-a-dia nele o Manjaro (Arch).
Qualquer coisa, atualizo este artigo acrescentando novas imagens, texto, macetes e experiências com o SSD.


Atualização do artigo:

Estamos no final de Abril/2022, faz 6 anos que este SSD está funcionando perfeitamente e diariamente...
Já tendo passado por diversas instalações de Linux, além do uso comum diário, já executou muitos testes, scripts, VMs, geração de arquivos, renderizações de videos, jogos e etc...


Algumas fontes utilizadas:
https://wiki.archlinux.org/index.php/Solid_State_Drives

https://raid6.com.au/posts/fs_ext4_external_journal/

https://www.mayrhofer.eu.org/ssd-linux-benchmark
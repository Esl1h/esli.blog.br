---
title: "Ubuntu 20.10 não inicia modo grafico - Ryzen 3000"
datePublished: Sat Nov 14 2020 15:55:21 GMT+0000 (Coordinated Universal Time)
cuid: ckta99v6i03oe6gs19fxfgv1h
slug: ubuntu-2010-nao-inicia-modo-grafico-ryzen-3000
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1631030111175/IsWefi3ap.png
tags: ubuntu, linux, hardware

---

Não iniciar o modo gráfico é um sintoma para uma infinidade de problemas.

Abaixo descrevo um deles que pode ser resumido como:

Ubuntu 20.10 com CPU AMD Ryzen não inicia modo gráfico após atualização ou instalação de um novo hardware.

Ubuntu 20.10 com CPU AMD Ryzen não inicia modo gráfico após instalação de SSD ou NVMe.

Ubuntu 20.10 com CPU AMD Ryzen e GPU RADEON não inicia modo gráfico após instalação de um novo hardware.


Meu ambiente:
CPU Ryzen 9 3900x
Placa-mãe AsRock Master X470 SLI/ac

Fiz duas alterações de hardware:
Adicionei uma ASUS RADEON RX5600 XT.
Troquei um SSD HyperX por um NVMe Wester Digital.

Na troca dos SSDs acabei trocando o Linux, mudei do Ubuntu 20.04 para o Ubuntu 20.10 (Ubuntu Studio em ambos, sendo que até o 20.04 era XFCE e a partir do 20.10 foi trocado pelo KDE Plasma).

Após instalação, configuração e alguns reboots...


O modo gráfico parou de iniciar,
Outro sintoma era quando ficava muito tempo oscioso, não entrava no bloqueio de tela (ficava em tela preta) e não voltava ao modo gráfico.
Algumas falhas aleatórias (ok, não investiguei a fundo neste caso), onde não iniciava o teclado nem conectava na rede.

Bug grave no Ubuntu 20.10?
Defeito no nvme?
Bug do Linux com o NVMe m.2?
Bug no Linux com placa Radeon ou defeito na placa?


Num dos erros, ao não iniciar o modo gráfico, usei o CTRL + ALT + F1 para acessar a console, fazer login com o usuário e executar o startx

```
root@linux:~# systemctl status sddm.service 

● sddm.service - Simple Desktop Display Manager
    Loaded: loaded (/lib/systemd/system/sddm.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2020-11-04 08:12:42 -03; 6h ago
      Docs: man:sddm(1)
            man:sddm.conf(5)
  Main PID: 1546 (sddm)
     Tasks: 2 (limit: 76974)
    Memory: 11.1M
    CGroup: /system.slice/sddm.service
            └─1546 /usr/bin/sddm

nov 04 08:12:42 linux systemd[1]: Starting Simple Desktop Display Manager...
nov 04 08:12:42 linux systemd[1]: Started Simple Desktop Display Manager.
nov 04 08:12:42 linux sddm[1546]: WARNING: CPU random generator seem to be failing, disable hardware random number generation
nov 04 08:12:42 linux sddm[1546]: WARNING: RDRND generated: 0xffffffff 0xffffffff 0xffffffff 0xffffffff
nov 04 08:12:42 linux sddm[1546]: Initializing...
nov 04 08:12:42 linux sddm[1546]: Starting...
nov 04 08:12:42 linux sddm[1546]: Logind interface found
```


O display manager usado nesta versão do Ubuntu é o SDDM (Simple Desktop Display Manager), onde faço o login e por padrão, inicia o KDE Plasma.


Observe as seguintes linhas do comando systemctl status sddm:
```
WARNING: CPU random generator seem to be failing, disable hardware random number generation
WARNING: RDRND generated: 0xffffffff 0xffffffff 0xffffffff 0xffffffff
```


Isto é um bug no firmware da placa-mãe!

A função RDRAND no Ryzen 3000 deveria ser um gerador de números pseudo-aleatórios de alta qualidade mas retorna 0xFFFFFFFFF todas as vezes.
As CPUs x86_64 modernas - começando com as arquiteturas Broadwell da Intel e Zen da AMD - devem ter geradores de números aleatórios (RNGs) integrados de alta qualidade, que usam "ruído" térmico para oferecer rapidamente números pseudoaleatórios de alta entropia a qualquer pessoa com acesso no nível do kernel. RDRAND é, por sua vez, a instrução que fornece esses números aleatórios.

Muitas aplicações usam o RDRAND, como o WireGuard por exemplo. 
No meu caso, o log que denunciou o bug veio do SystemD e do SDDM (gerenciador de login).

Identificado em 2019 (e em várias motherboards, o firmware foi atualizado/corrigido, aparentemente, exceto nas Asrock).

O systemd contorna este bug! Exceto se você estiver usando a versão mais recente dele.

Ou seja, uma distro muito atual (com o systemd recente), num hardware atual que não foi atualizado seu firmware no processo de manufatura, dará este erro.


Mesmo tendo sido descoberto em Julho de 2019 com o lançamento deste hardware, fiz a aquisição do meu em Abril e seu firmware não estava atualizado.


Resumo:
BIOS da placa-mãe atualizado, erro sumiu!



Seguindo as fontes:
https://arstechnica.com/gadgets/2019/10/how-a-months-old-amd-microcode-bug-destroyed-my-weekend/

https://linuxreviews.org/AMD_Ryzen_3000_series_CPUs_can%27t_do_Random_on_boot_causing_Boot_Failure_on_newer_Linux_distributions

https://www.forbes.com/sites/jasonevangelho/2019/07/12/amd-motherboard-patch-ryzen-3000-customers-affected-by-destiny-2-and-linux-boot-problems
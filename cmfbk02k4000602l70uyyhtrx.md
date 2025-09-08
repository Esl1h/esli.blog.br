---
title: "Transformando antigo smartphone em DAP"
datePublished: Mon Sep 08 2025 20:07:48 GMT+0000 (Coordinated Universal Time)
cuid: cmfbk02k4000602l70uyyhtrx
slug: transformando-antigo-smartphone-em-dap
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/LpxT6SVVK1k/upload/72370578f2bee602b80d327a7efe9990.jpeg
tags: bash, android, audio, adb, dac, audiophile, dap, hi-res

---

Assistindo alguns vídeos sobre transformar um antigo smartphone em um DAP (Digital Audio Player - dedicado) - vídeos abaixo - lembrei que possuo um Motorola *alguma-coisa* largado por aqui.

E é isto que fiz:

Factory Reset

Ao fazer login (conta nova, somente para o “DAP”), update dos apps e S.O.  
Chegou ao limite do Motorola One Macro comprado em 2019 com Android 9, atualizou para o Android 10 e último update de outubro/2021.

Instalado:

Players:

* Qobuz
    
* Spotify
    
* YouTube
    
* USB Audio Player PRO
    
* Poweramp
    
* HiByMusic
    

Para fones específicos:

* Edifier Connect
    
* Galaxy Wearable (Samsung Gear)
    

Equalizadores:

* Equalizador do Poweramp
    
* Wavelet
    

Outros:

* Muviz (efeito visual)
    
* KDE Connect (controlar multimídia)
    
* Niagara (launcher)
    

Através do Niagara Launcher, removi todos os apps da tela inicial e configurei quais aplicativos de música (ao conectar um fone ou DAC, na home do launcher surgem automaticamente os players que selecionei).

Todas as widgets e barras foram removidas do launcher.

MAS…

Iniciaram-se algumas notificações dos apps bloat da Motorola… de início, removi a permissão de notificação, depois instalei o NetGuard para remover todos os acessos à rede Wifi. Porém, mesmo assim, os aplicativos continuam rodando e querendo mostrar serviço.

Passos seguintes:

* habilitei o modo desenvolvedor;
    
* Instalei o `adb` (Android Debug Bridge) no meu Linux laptop;
    

Com o smartphone em modo dev e conectado na USB, continuei:

Uma das coisas que o [Wavelet](https://github.com/Pittvandewitt/Wavelet) solicita é via adb habilitar a permissão de DUMP para o app.

Aproveitei para fazer isto… [https://pittvandewitt.github.io/Wavelet/Settings/](https://pittvandewitt.github.io/Wavelet/Settings/)

Mas outros passos foram:

* gerar uma lista com todos os apps instalados no smartphone
    

```bash
adb -s ZF523242ZG shell pm list packages
```

Obs: o `ZF523242ZG` é como ele identificou o smartphone. Ao conectar na usb, digite `adb devices` e ele irá identificar o seu.

Movi para uma lista:

`adb -s ZF523242ZG shell pm list packages -f > all-packages.txt`

Foram 265 linhas! Obviamente, nem tudo deve-se remover. Usei o ChatGPT-5 e Claude 4 Sonnet para gerarem uma nova lista com o que poderia ser removido, visando ter um Android único e exclusivamente para Player (nada de SIMcard, fotos, Moto firulas, etc…)

Com isto, gerou a `packages-remove.txt` com quase 90 apps listados.

E removi com o script abaixo:

%[https://gist.github.com/Esl1h/dd9c1b82bee79e52a27fc346519ee85d] 

[https://gist.github.com/Esl1h/dd9c1b82bee79e52a27fc346519ee85d](https://gist.github.com/Esl1h/dd9c1b82bee79e52a27fc346519ee85d)

## Equipamentos conectados

O Motorola One Macro possui entrada P2, porém estou usando este DAC via USB-C

### Dongle / DAC

* iFi Go Link – USB DAC + Headphone Amp
    
    * Chip ES9219 Sabre DAC com tecnologia Quad DAC+, arquitetura DAC HyperStream III de 32 bits e eliminador de jitter. Suporta áudio de alta resolução até PCM de 32 bits/384 kHz, DSD256 e MQA.
        

### Headphones & Earbuds

* Samsung Galaxy Buds Pro (1st model) - Earbuds
    
* AKG K240 Mini MKII - headphone
    
* Edifier W820NB Plus - headset
    
* KZ ZSN Pro 2 - IEM
    

Dia-a-dia: Quase sempre usando o app do Qobuz (beta) e/ou o aplicativo USB Audio Player PRO, configurado com o login do Qobuz. E 90% do tempo o iFi Go Link conectado ao AKG K240 Mini MKII.

Inspirações:

%[https://www.youtube.com/watch?v=I4DfCmd1vew] 

%[https://www.youtube.com/watch?v=HuOASuE3lkk]
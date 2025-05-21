---
title: "Uai-Fai"
seoTitle: "Shell Bash script Automate your Ubuntu and Fedora installations"
seoDescription: "Automate your Ubuntu and Fedora installations with a customizable Bash script for app setup and environment consistency across devices"
datePublished: Thu May 02 2024 23:24:48 GMT+0000 (Coordinated Universal Time)
cuid: clvpvil45000009jpb9zsb1ap
slug: uai-fai
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714672029126/03560b54-9b71-4eac-ab3d-639ac8f024d2.jpeg
tags: ubuntu, bash, shell, fedora, shell-script, bash-script

---

"Ubuntu After Install" and "Fedora After Install". Bash script to run after install with my personal sets.

Used and tested in Fedora and Ubuntu releases since 2020 (some customizations are not committed to my public Git repository).

[https://github.com/Esl1h/UAI-FAI](https://github.com/Esl1h/UAI-FAI)

%[https://github.com/Esl1h/UAI-FAI] 

## **Ubuntu or Fedora After Install**

### **Why?**

I have three laptops, and I want to:

* maintain a consistent environment across all of them
    
* perform a fresh and clean install with each new distro release
    

Therefore, I have developed this straightforward script.

<div data-node-type="callout">
<div data-node-type="callout-emoji">🛑</div>
<div data-node-type="callout-text">Change it and don't trust this code! Understand it and adapt it to your configurations, preferences, and settings. It's straightforward; apart from zsh and oh-my-zsh, everything else involves app installation, trusted repositories, and updates.</div>
</div>

## Run

To run this script, you need to:

* Modify it! (It's currently set up for my environment)
    
* Execute it using your regular user account.
    

What this shellscript will do:

* Update!
    
* install some apps (yakuake, solaar, git, vim, curl)
    
* install some my daily apps via flatpak
    
* install jetbrain fonts and hack fonts.
    
* set repo and install softmaker office NX
    
* install NextDNS
    
* install and setup zsh + oh-my-zsh and powerlevel10k theme
    
* <s>configure swapfile and change fstab to logs over tmpfs</s>
    

### **PT-BR**

Script para executar após a instalação do Linux Desktop (para distros baseada no Ubuntu ou Fedora).

O que você deve fazer:

* Mudar!(está configurado para o meu ambiente)
    
* execute com seu usuário
    

### **Por quê?**

Tenho três laptops e gosto de:

* Ter o mesmo ambiente em todos.
    
* Em cada nova versão da distribuição, faço uma instalação completa e limpa
    

Então, criei este script simples.

<div data-node-type="callout">
<div data-node-type="callout-emoji">🛑</div>
<div data-node-type="callout-text">Mude-o e não confie neste código! Entenda-o e adapte-o às suas configurações, preferências e configurações. É simples; além de zsh e oh-my-zsh, todo o resto envolve instalação de aplicativos, repositórios confiáveis ​​e atualizações.</div>
</div>

### **Credits**

Icon/Logo ["opensource"](https://thenounproject.com/icon/opensource-4957970/) by [M. Oki Orlando](https://thenounproject.com/creator/orvipixel/) from Noun Project (CC BY 3.0)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714668828239/5bdb9130-09f6-4e44-8f53-d91b18256197.png align="center")
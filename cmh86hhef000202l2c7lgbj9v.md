---
title: "Além das ISOs: Filosofias de Configuração que Moldam o Linux Moderno"
datePublished: Sun Oct 26 2025 20:45:32 GMT+0000 (Coordinated Universal Time)
cuid: cmh86hhef000202l2c7lgbj9v
slug: alem-das-isos-filosofias-de-configuracao-que-moldam-o-linux-moderno
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/wh-RPfR_3_M/upload/c2d0dd0b643fe297ad6fb68656366381.jpeg
tags: linux, scripts, arch, filosofia, distros, omarchy

---

Faz 1 mês que estou usando o Omarchy, em resumo, um Arch com Hyprland pré-configurado e opinativo.

Muitos anos atrás, 2017 se não me engano, havia escrito e sempre revisto o que hoje deixo público no meu GitHub sob o nome de “UAI-FAI”, onde basicamente, criei um shell script para realizar toda minha configuração do dia-a-dia para Debian/Ubuntu like e Fedora. Ou seja, em teoria, faço a instalação da distro, executo meu .sh e faço restore dos backups. [https://esli.blog.br/uai-fai](https://esli.blog.br/uai-fai) // [https://github.com/Esl1h/UAI-FAI](https://github.com/Esl1h/UAI-FAI)

Nunca foi pretensão empacotar isso, a ideia é facilitar o ‘distro-hopping’ e meu processo de instalação (por sofrer num passado distante, sempre preferi realizar uma instalação ‘do zero’ do que fazer o upgrade completo da distro).

Uma distribuição Linux de verdade é aquela que tem a coragem de manter seus próprios repositórios, gerenciar dependências, lidar com bugs reportados por usuários furiosos às 3h da manhã e existir por mais de 6 meses sem o criador abandonar o projeto após perceber que “mudar o wallpaper do Ubuntu e adicionar Neofetch no .bashrc” não te torna o próximo Linus Torvalds - ao contrário dessas “distros” que são glorificados temas GTK com nome pomposo tipo “QuantumOS” ou “NeoLinux”, cujo único diferencial é ter gastado 20 minutos no GIMP fazendo um logo gradiente e cuja “filosofia única” se resume a “é tipo o Arch, mas tem o LibreOffice instalado”.

Meu último empregador, dava ao funcionário a opção de qual Linux escolher: Mint, Ubuntu, Debian, dentre outras… Após a escolha, o suporte realizava a instalação do OS e rodava uma personalização/instalação de diversos parâmetros e configurações (VPN, Endpoint Protection, DNS, monitoring, repo…) e nem por isso batizava-o como “CorporateOS”.

Se sua distro desaparece quando o dev original esquece a senha do GitHub, você não fez (ou tem) uma distro, você fez um download de ISO customizada com delírios de grandeza.

Saímos de criadores de .ISO com nomes estranhos para configurações empacotadas para conceitos filosóficos para serem aplicados nas distros já conceituadas.

## **Omakub, Omarchy e a Filosofia Omakase: Transformando a Experiência Linux**

David Heinemeier Hansson (DHH), criador do Ruby on Rails e cofundador da 37signals, está revolucionando a experiência Linux com duas ferramentas que seguem a filosofia “Omakase” — termo japonês que significa “deixo por sua conta” ou “escolha do chef”. **Omakub** (baseado em Ubuntu) e **Omarchy** (baseado em Arch Linux) transformam instalações Linux complexas em ambientes de desenvolvimento prontos para uso com um único comando, eliminando horas de configuração manual.

A filosofia **Omakase Computing**, desenvolvida pela 37signals, propõe que a maioria das pessoas não sabe exatamente o que quer inicialmente e se beneficia de escolhas curadas e integradas feitas por especialistas de confiança. Em vez de enfrentar o “paradoxo da escolha” configurando cada ferramenta individualmente, Omakub e Omarchy oferecem uma experiência integrada e coesa, similar ao princípio que guiou o desenvolvimento do Ruby on Rails. Os usuários ainda podem fazer substituições e personalizações conforme desenvolvem suas preferências, mas começam com um sistema funcional e elegante desde o primeiro momento.

**Omakub** foi lançado em 2024 para Ubuntu, trazendo terminal Alacritty com Zellij, Neovim com LazyVim, VSCode, onze temas sincronizados, e ferramentas essenciais pré-configuradas.

Já **Omarchy**, anunciado em agosto de 2025, representa a evolução dessa visão: uma distribuição completa baseada em Arch Linux com Hyprland (gerenciador de janelas em mosaico), oferecendo uma experiência ainda mais produtiva e visualmente impressionante. A 37signals está migrando todas as equipes de engenharia para Omarchy nos próximos três anos, substituindo MacBooks por laptops Framework e desktops Beelink.

Apesar de alguma controvérsia sobre a obrigatoriedade da mudança e questões éticas relacionadas ao Hyprland, o projeto tem ganhado tração significativa com mais de 3.500 usuários e 18 releases desde o lançamento, demonstrando que a filosofia Omakase pode simplificar e democratizar o acesso ao poder do Linux.

## **Ultramarine Fedora: Fedora “Que Simplesmente Funciona”**

Ultramarine implementa a filosofia de que **a maioria dos usuários não quer gastar horas configurando**: querem um sistema que funcione bem desde o primeiro boot, mantendo a estabilidade e modernidade do Fedora. É como um “Fedora pré-configurado por especialistas”, alinhado com a ideia de que escolhas curadas reduzem fricção sem limitar customização futura.

**Ultramarine Linux** é uma distribuição baseada no Fedora desenvolvida pela Fyra Labs (Tailândia) com a filosofia central de “*It just works*” (simplesmente funciona). Ao contrário do Fedora puro, que prioriza software livre e exige configuração pós-instalação, o Ultramarine já vem com codecs multimídia proprietários (MP3, H.264), drivers NVIDIA, RPM Fusion habilitado e Flathub pré-configurado. A proposta é eliminar barreiras de entrada: usuários não deveriam precisar procurar tutoriais para reproduzir vídeos ou instalar drivers gráficos. Oferece edições com Budgie (flagship), GNOME, KDE Plasma e Xfce, incluindo a ferramenta **Hop** que permite trocar entre desktops sem reinstalar — incentivando experimentação sem riscos.

Além dos “padrões pragmáticos”, Ultramarine traz otimizações de performance como o **System76 Scheduler** (melhor gerenciamento de prioridades) e Btrfs por padrão para backups rápidos. Usa Zsh com Starship prompt pré-configurado, atendendo desenvolvedores sem perder a simplicidade visual para iniciantes. A filosofia é clara: **a maioria dos usuários não quer gastar horas configurando** — querem um sistema que funcione bem desde o primeiro boot, mantendo a estabilidade e modernidade do Fedora. É essencialmente um “Fedora pré-configurado por especialistas”, onde escolhas curadas reduzem fricção sem limitar customização futura, similar à abordagem Omakase mas focada em tornar o ecossistema Fedora mais acessível.

## **Outros Scripts e Filosofias de Instalação Linux**

Além dos projetos já mencionados, o ecossistema Linux oferece diversas outras abordagens filosóficas para instalação e configuração de sistemas.

**Regolith Linux** combina Ubuntu com o gerenciador de janelas i3, implementando a filosofia de "eficiência via teclado primeiro - keyboard-first" — prioriza atalhos e produtividade sem depender do mouse, ideal para desenvolvedores que valorizam velocidade e minimalismo visual.

**Vanilla OS** representa uma filosofia diferente: **imutabilidade declarativa**. Baseado em Ubuntu (migrando para Debian), utiliza ABRoot para criar duas partições root (A⟺B) onde atualizações são aplicadas atomicamente em uma partição enquanto a outra permanece intacta, permitindo rollback instantâneo. A filosofia é "*seu sistema não deve quebrar*" — combina segurança (arquivos de sistema read-only), containers para isolamento de aplicações (via apx e VSO), e suporte nativo a apps Android através de Waydroid. É a antítese do Arch: você **não pode** modificar o core do sistema, mesmo como root.

**EndeavourOS** e **Garuda Linux** representam filosofias distintas no ecossistema Arch.

EndeavourOS segue "*Arch simplificado, mas puro*" — instalador gráfico Calamares, documentação amigável, mas mantém a essência DIY do Arch. Garuda adota “*performance out-of-the-box*” — kernel Zen pré-configurado, Btrfs com snapshots automáticos (Snapper), zRAM, temas vistosos (Dr460nized KDE), e otimizações de sistema prontas para gaming.

São duas interpretações do “como tornar Arch acessível”: Mas no universo Arch há uma infinidade filosófica: de ISO empacotada mais famosa como o Majaro, Archcraft e diversos “Yet Anothers”, dotfiles managers, installers, helpers etc.

Omarchy e Ultramarine ilustram bem como “filosofias de instalação” moldam não somente o setup inicial, mas toda a experiência de uso: do omakase opinativo que te entrega um ambiente coeso e produtivo, ao minimalismo que te convida a decidir cada peça. Esse espectro é amplo.

Mesmo quando não são ISOs completas, bootstrappers como chezmoi/yadm (dotfiles) , Fedora “Not Another Nothing to do”, e toolchains containerizadas (Devbox, direnv, asdf) traduzem uma filosofia clara: ambientes replicáveis, com menos drift e mais foco no trabalho.

Cada script ou projeto filosófico responde à mesma pergunta fundamental: **“Quanto controle o usuário realmente precisa versus quanto a curadoria acelera a produtividade?”** — E as respostas variam do maximalismo Omakase (DHH escolhe tudo) ao minimalismo Arch (você escolhe cada byte).
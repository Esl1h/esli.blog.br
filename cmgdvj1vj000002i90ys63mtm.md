---
title: "Bash / shell scripts para instalação de Apps"
datePublished: Sun Oct 05 2025 15:45:44 GMT+0000 (Coordinated Universal Time)
cuid: cmgdvj1vj000002i90ys63mtm
slug: bash-shell-scripts-para-instalacao-de-apps
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/XJXWbfSo2f0/upload/d6a79be75a7bf6d9137e93e3904380f2.jpeg
tags: linux, bash, script, gui, dialog, install, shell-script, installation-guide, zenity, yad, makeself

---

Zed (IDE), NextDNS, Joplin (Notes), NordVPN, Surfshark VPN, Radicle (Git), Brave Browser, dentre outros...

Qual ponto em comum?

Para todos eles, ao invés de uma página de download com diversos sabores e formatos de distribuição de arquivos como .AppImage, Flatpak, snap, deb, rpm, tar.gz... eles disponibilizam inicialmente um shell script para o usuário instalar seu app no Linux.

Este shell script escolhe o melhor formato, identificando o tipo de distribuição e realizando a instalação, por vezes, instalando chave gpg e o repositório do app, dentre outras validações e camadas de otimizações.

Alguns exemplos que citei:

[https://zed.dev/install.sh](https://zed.dev/install.sh)

[https://nextdns.io/install](https://nextdns.io/install)

[https://raw.githubusercontent.com/laurent22/joplin/dev/Joplin\_install\_and\_update.sh](https://raw.githubusercontent.com/laurent22/joplin/dev/Joplin_install_and_update.sh)

[https://downloads.nordcdn.com/apps/linux/install.sh](https://downloads.nordcdn.com/apps/linux/install.sh)

[https://downloads.surfshark.com/linux/debian-install.sh](https://downloads.surfshark.com/linux/debian-install.sh)

[https://radicle.xyz/install](https://radicle.xyz/install)

[https://dl.brave.com/install.sh](https://dl.brave.com/install.sh)

Bash é poderoso, e usá-lo para um instalador resulta em um equilíbrio pragmático entre simplicidade, compatibilidade e funcionalidade, mas pode não soar tão 'requintado' quanto outras formas de disponibilizar um instalador e também pode apresentar fragilidades: divergências em shells, distribuição, permissões ou versões podem causar falhas sutis.

Por isso, scripts de instalação complexos muitas vezes evoluem para instaladores escritos em linguagens mais robustas (Go, Python, etc). Ainda assim, um bom script bash com as práticas abaixo pode ser eficaz e confiável.

A ideia inicial deste artigo era trazer um template quase pronto, mas com o tanto de exemplos práticos e funcionais acima, ficou muito redundante.

Preferi focar nos pontos em comum e na listagem de algumas boas práticas.

O objetivo é um script completo (e simples) que permita a um usuário leigo instalar somente rodando uma linha no terminal e também, em caso de erro, saber exatamente o que precisa ser feito para corrigir e seguir na instalação.

Quando usar bash para instaladores:

* Quando a instalação precisa rodar em múltiplos sistemas Unix-like com variações.
    
* Quando o usuário alvo pode executar um script simples via curl … | sh ou similar.
    
* Quando você quer oferecer atualização automática ou lógica de upgrade dentro do instalador.
    

## Pontos em comum

**Shebang + modo “exit-on-error” / opções seguras**  
Todos começam com #!/usr/bin/env bash ou #!/bin/sh, e quase todos usam set -e ou set -eu para abortar em erro ou variáveis não definidas (ex: zed usa set -eu).

**Detecção da arquitetura / sistema operacional**  
Verificam uname -s, uname -m, ou equivalentes para determinar o OS (Linux, Darwin etc) e a arquitetura (x86\_64, arm etc). (ex: zed, radicle, Joplin)

**Download via curl ou wget (fallback)**  
Usam curl ou wget — verificam presença de uma ou outra, e definem função wrapper para baixar os arquivos. (ex: Joplin usa command -v wget2 / wget / curl)

**Criação de diretórios temporários / uso de mktemp ou diretórios de trabalho**  
Criam um diretório temporário via mktemp -d, ou usam $TMPDIR se definido, para armazenar arquivos baixados antes de instalar. (ex: zed, radicle)

**Extração / instalação de arquivos (tar, xz, tar.gz etc)**  
Após download, descompactam (tar -xzf, tar -xJf etc), movem para o local desejado, ajustam permissões de execução (chmod +x) e organizam binários.

**Verificações de dependências**  
Exigem ou checam comandos essenciais: curl, tar, xz, ssh-keygen, wget etc. Se ausentes, abortam com mensagem de erro. (ex: radicle exige tar, xz, curl, ssh-keygen)

**Verificação de versão / atualização vs instalação nova**  
Alguns scripts verificam se já existe instalação, qual versão está instalada, comparam com a versão a instalar, e decidem se vão pular instalação ou forçar atualização (ex: Joplin, NextDNS)

**Tratamento de PATH / exportação de binários no PATH**  
Após instalar, ajustam PATH ou recomendam ao usuário que modifique ~/.bashrc, ~/.zshrc etc para incluir o local onde os binários foram instalados (ex: zed, radicle).

Alguns scripts não só sugerem alterar o PATH, como também injetam automaticamente linhas em ~/.bashrc ou ~/.zshrc. Isso merece ser destacado como prática arriscada: pode quebrar configurações do usuário.

**Mensagens de status e falha**  
Usam echo ou funções de log para informar progresso, warnings, erros. Em muitos casos há função error() ou fatal() para abortar com mensagem.

**Limpeza após instalação**  
Removem arquivos temporários usados para download / staging (ex: Joplin faz rm -rf "$TEMP\_DIR").

## Boas práticas

1. **Modo seguro**  
    Use `set -euo pipefail` (ou ao menos `-e` e `-u`) para abortar em erros inesperados e evitar uso de variáveis não definidas.
    
2. **Detecção de ambiente**  
    Identifique sistema operacional, distribuição (se necessário), arquitetura, e adapte a URL ou pacote conforme isso.
    
3. **Fallback robusto para download**  
    Verifique presença de `curl` ou `wget`. Use wrappers que falhem controladamente.
    
4. **Staging usando diretório temporário**  
    Baixe tudo em uma pasta temporária isolada (via `mktemp -d`), execute testes/extrair ali, só ao final mova para o destino final.
    
5. **Verificar dependências**  
    Antes de executar comandos críticos (tar, xz, ssh-keygen, etc), cheque se estão presentes; senão, abortar com instrução clara.
    
6. **Controle de versão / upgrade**  
    Se possível, detecte a versão instalada e evite sobrescrever sem necessidade, ou permita atualização incremental.
    
7. **Permissões / segurança**  
    Defina permissões apropriadas (`chmod 0755`, evitar execução de scripts sem controle). Evite rodar como root se não for necessário — ou verifique `EUID`.
    
8. **Modificação de PATH com cuidado**  
    Se seu script precisa que o binário seja acessível, ofereça auto-inserção no `.bashrc`/`.zshrc` ou instruções claras, mas não sobrescreva configurações do usuário sem aviso.
    
9. **Mensagens úteis e feedback**  
    Informe o usuário do que está sendo feito, sucesso ou falha. Use exit-codes corretamente.
    
10. **Limpeza pós-instalação**  
    Apague arquivos temporários, evite deixar lixo no sistema.
    
11. **Sinais / trap para erros**  
    Use `trap '...' ERR` para capturar falhas e exibir mensagem amigável ou rollback parcial, se possível.  
    Bem como redirecionamento padronizado para sterr, verbosidade com flags (--silent, --debug, --dry-run).  
    Logs estruturados (info, error, debug)
    
12. **Verificação de assinatura / integridade**  
    Se possível, inclua verificação de assinatura GPG ou assinatura digital dos arquivos baixados.
    
13. **Modularidade e legibilidade**  
    Organize o script em funções nomeadas (`download`, `install`, `verify`, `cleanup`) para facilitar a manutenção.
    
14. **Uso de** `sudo` **para operações privilegiadas**  
    Scripts como Brave, NordVPN e Surfshark instalam chaves APT ou mexem em `/etc/apt/sources.list.d`, exigindo `sudo`, por exemplo.
    
15. **Adição de repositórios oficiais**  
    Em vez de somente baixar binários, vários (Brave, NordVPN, Surfshark) adicionam o repositório APT/YUM oficial, importam a chave GPG e configuram o sistema para receber futuras atualizações via gerenciador de pacotes.
    
16. **Execução “pipe to sh” como design**  
    O formato `curl -fsSL URL | sh` é unânime: os scripts foram escritos pensando nesse modo de uso.
    

# Alternativas para instalador via shell script

Ao invés de criar um shell script ‘do zero’, baseado nos scripts de exemplo que dei no início do artigo e usando as boas práticas que listamos aqui, você pode usar alguns utilitários que gerarão o seu instalador, mantendo a premissa de serem em shell ;-)

## Makeself

O Makeself ([https://makeself.io](https://makeself.io)) é uma ferramenta em shell que empacota diretórios inteiros em um único script de instalação autoextraível.

Ele gera um arquivo em shell script (geralmente nomeado como .run) que, ao ser executado, extrai automaticamente seu conteúdo em um diretório temporário e executa um script de instalação interno.

O formato é autossuficiente, portátil e não requer dependências além de um shell POSIX e `tar`. É amplamente usado para distribuir aplicações proprietárias, binários fechados e instaladores multiplataforma no Linux, como os instaladores do Unity, JetBrains, VMWare, VirtualBox, NVidia e muitos outros…

Além disso, o makeself oferece recursos nativos como verificação de integridade, compressão (gzip, xz, zstd), mensagens customizadas, e suporte a execução automática de scripts pós-extração — mantendo a simplicidade do bash, mas com aparência e praticidade de um instalador real.

Além do Linux (qualquer distribuição), o instalador em shell script criado usando o Makeself irá funcionar em:

* Sun Solaris
    
* HP-UX
    
* SCO OpenUnix e OpenServer
    
* IBM AIX
    
* macOS (Darwin)
    
* SGI IRIX 6.5
    
* FreeBSD
    
* OpenBSD
    
* NetBSD
    
* UnicOS / Cray
    
* Windows (Cygwin, WSL)
    

Novamente, a portabilidade de usar o shell script criado pelo makeself é por somente ter a dependência de um shell POSIX e do tar.

## Outras soluções

Outras alternativas ao makeself para criar instaladores em shell script incluem ferramentas e formatos como:

* **shar (Shell Archive)** – formato tradicional do Unix que empacota arquivos em um script shell autoextraível, mais simples e antigo que o makeself.
    
* **gzexe** – utilitário que compacta e transforma um executável em um script shell autodescompactável, útil para distribuição de binários leves.
    
* **self-extracting tarballs manuais** – técnica clássica que concatena um tar.gz a um script shell que o extrai automaticamente (`cat` [`script.sh`](http://script.sh) `archive.tar.gz >` [`installer.run`](http://installer.run)).
    
* **pkgbuild / install.sh híbridos** – scripts que empacotam e instalam diretamente pacotes .deb ou .rpm sem depender de makeself, frequentemente usados em sistemas customizados. Como o makepkg no Arch que usa o pkgbuid.
    
* **autopackage (projeto legado)** – framework em shell para criar instaladores gráficos e de linha de comando portáteis em Linux, hoje obsoleto, mas referência histórica.
    

Essas alternativas mantêm a lógica em shell puro, sem recorrer a sistemas de empacotamento externos (como Appimage, Flatpak ou Snap), permitindo instaladores autossuficientes e controlados via script.

# Instaladores gráficos e/ou interativos

Se o usuário precisa confirmar algo, inserir alguma informação, pode-se usar tanto o terminal ou invocar algum utilitário gráfico para esta ação dentro do seu instalador em shell script.

Há diversos utilitários que permitem criar instaladores gráficos ou interativos diretamente a partir de scripts shell, com diferentes níveis de complexidade e dependências.

* **Zenity** — cria caixas de diálogo gráficas baseadas em GTK (mensagens, progresso, formulários, seleção de arquivos). Ideal para scripts leves e ambientes GNOME.
    
* **Yad (Yet Another Dialog)** — evolução do Zenity com mais widgets (abas, listas, formulários complexos, tabelas, notificações). Excelente substituto moderno.
    
* **Dialog** — interface ncurses em modo texto (menus, checklists, progresso, input). Extremamente portátil.
    
* **Whiptail** — versão mais leve do Dialog (usada em instaladores Debian). Compatível com os mesmos parâmetros básicos.
    
* **Gdialog / Kdialog** — variantes antigas específicas de GNOME/KDE, respectivamente, hoje pouco usadas, mas ainda funcionais em ambientes correspondentes.
    
* **Xdialog** — similar ao Dialog, mas com interface gráfica X11.
    
* **Gooey Bash / Bash Simple GUI wrappers** — wrappers não oficiais que combinam Zenity/Yad/Dialog em interfaces mais estruturadas.
    
* **Tk via wish (Tcl/Tk)** — possível criar GUIs simples em shell invocando `wish` com código Tcl embutido; raramente usado, mas funcional.
    

Obviamente, gdialog, kdialog focam em suas DEs, não são ‘portáteis’, precisaria validar qual a DE do usuário para invocar corretamente. Mesmo caso do whiptail.

Com o desuso do X11 em prol do Wayland, o Xdialog cai no mesmo caso.

Portanto, minha indicação para criar caixas de interação gráfica do seu shell script com o usuário seria:

Zenity e Yad - ambos usam o GTK

Ou adicionar uma camada de identificação do Desktop Environment do usuário e, a partir disto, invocar o dialog (ncurses, terminal) e/ou as variantes: Xdialog (X11), Kdialog (Qt, KDE Plasma), Gdialog (Gnome e forks).

Esta abordagem pode evitar instalações a mais no sistema, por exemplo, em meu Fedora 42 (KDE) tenho o Kdialog e zenity - e não lembro de tê-los instalado, logo, creio que vieram por default na distro.
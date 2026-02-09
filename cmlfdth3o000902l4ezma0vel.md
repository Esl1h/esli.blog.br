---
title: "sysup: um comando para atualizar todas as suas máquinas em qualquer distro"
datePublished: Mon Feb 09 2026 16:24:02 GMT+0000 (Coordinated Universal Time)
cuid: cmlfdth3o000902l4ezma0vel
slug: sysup-um-comando-para-atualizar-todas-as-suas-maquinas-em-qualquer-distro
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/WItUgoQSJp0/upload/7457c066975f8235cc5c9b09e6f4a775.jpeg
tags: functions, bash, zsh, script, oh-my-zsh, bash-script, system-administration

---

Quem mantém mais de uma distribuição Linux sabe a dor: `dnf update` aqui, `pacman -Syu` ali, `apt upgrade` acolá. Multiplica isso por cinco máquinas — dois laptops com Omarchy e EndeavourOS (Arch), outro com Ubuntu 25.04, um desktop com Fedora 43 e um home server com Debian (sem contar os Raspberries, porque esses a gente finge que não precisa atualizar) — e a tarefa de “rodar um update” vira um exercício de memória muscular para dedos que já não são tão jovens.

A solução? Uma **zsh function** que detecta o package manager e faz tudo com um único comando: `sysup`.

Mas antes de chegar nela, vale entender como o Zsh organiza esse tipo de extensão.

## A estrutura `~/.zsh/`

O Zsh permite organizar customizações em diretórios dedicados. Uma convenção comum (e que uso nos meus [dotfil](https://github.com/Esl1h/dotfiles)[es):](https://github.com/Esl1h/dotfiles)

```bash
~/.zsh/
├── functions/     # Funções carregadas sob demanda (autoload)
├── completions/   # Scripts de completion customizados (_comando)
```

### `functions/`

Cada arquivo é **uma função**. O nome do arquivo = nome da função. Sem extensão, sem shebang. O Zsh carrega o conteúdo do arquivo como corpo da função quando ela é chamada pela primeira vez — isso é o **autoload**.

### `completions/`

Segue a mesma lógica, mas para funções de completion (`_nome`). Se você escreve um CLI próprio e quer tab-completion, é aqui que o script de completion fica.

## As 4 linhas no `~/.zshrc`

```bash
fpath=(~/.zsh/functions $fpath)
autoload -Uz ~/.zsh/functions/*(N:t)
autoload -Uz compinit && compinit
autoload -Uz promptinit && promptinit
```

### Linha 1: `fpath=(~/.zsh/functions $fpath)`

Adiciona `~/.zsh/functions/` ao **fpath** (function path) — o equivalente do `$PATH`, mas para funções do Zsh. O Zsh só encontra funções via autoload se elas estiverem em algum diretório listado no `$fpath`. Ao prepend (`~/.zsh/functions` antes de `$fpath`), suas funções têm prioridade sobre as do sistema.

Se tiver um diretório `completions/`, adicionaria da mesma forma:

```bash
fpath=(~/.zsh/completions ~/.zsh/functions $fpath)
```

### Linha 2: `autoload -Uz ~/.zsh/functions/*(N:t)`

Registra **todas** as funções do diretório para autoload.

Dissecando:

| Componente | Função |
| --- | --- |
| `autoload` | Marca uma função para carregamento sob demanda (lazy load) |
| `-U` | Suprime alias expansion — evita conflitos |
| `-z` | Força estilo Zsh (vs. Ksh) |
| `*(N:t)` | Glob com qualificadores: `N` = nullglob (não dá erro se vazio), `:t` = tail (extrai só o nome do arquivo, sem o path) |

O `:t` é essencial. Sem ele, o autoload receberia `/home/user/.zsh/functions/sysup` em vez de apenas `sysup`.

A função **não é carregada em memória nesse momento**. O Zsh apenas registra que, quando `sysup` for chamado, deve buscar e carregar o arquivo correspondente. Lazy loading puro.

### Linha 3: `autoload -Uz compinit && compinit`

Carrega e inicializa o sistema de completion do Zsh. O `compinit` escaneia o `$fpath` procurando arquivos `_*` (prefixo underscore) e os registra como funções de completion. Sem isso, nenhum tab-completion customizado funciona.

### Linha 4: `autoload -Uz promptinit && promptinit`

Carrega o sistema de temas de prompt. Permite usar `prompt -l` para listar temas disponíveis e `prompt <tema>` para aplicar. Útil se você usa temas de prompt distribuídos via `$fpath`, embora quem usa Starship ou p10k (meu caso) talvez nunca toque nisso.

# O Sysup

Código completo: [dotfiles](https://github.com/Esl1h/dotfiles/blob/main/.zsh/functions/sysup)[/.zsh/fu](https://github.com/Esl1h/dotfiles/blob/main/.zsh/functions/sysup)[nctions/sysup](https://github.com/Esl1h/dotfiles/blob/main/.zsh/functions/sysup)

```bash
# Description: Dynamic package manager detection and global system update
# Usage: sysup

_sysup_log() {
  print -P "%F{blue}==>%f %B$1%b"
}

# 1. Universal Package Managers (Sandboxed)
if command -v flatpak &>/dev/null; then
  _sysup_log "Updating Flatpaks..."
  flatpak update -y
fi

if command -v snap &>/dev/null; then
  _sysup_log "Refreshing Snaps..."
  sudo snap refresh
fi

# 2. Native System Package Managers
if command -v dnf &>/dev/null; then
  _sysup_log "Running DNF Update..."
  sudo dnf update -y
elif command -v yay &>/dev/null; then
  _sysup_log "Running Yay (AUR/Pacman)..."
  yay -Syu --noconfirm
elif command -v pacman &>/dev/null; then
  _sysup_log "Running Pacman..."
  sudo pacman -Syu --noconfirm
elif command -v apt &>/dev/null; then
  _sysup_log "Running APT..."
  sudo apt update && sudo apt upgrade -y
elif command -v yum &>/dev/null; then
  _sysup_log "Running YUM..."
  sudo yum update -y
elif command -v zypper &>/dev/null; then
  _sysup_log "Running Zypper..."
  sudo zypper patch
fi

_sysup_log "System update completed."
```

### Lógica

O script faz detecção dinâmica de package manager via `command -v` e executa o update apropriado.

Divide em duas categorias:

**1\. Package managers universais (sandboxed)** — sempre executados se presentes:

```bash
if command -v flatpak &>/dev/null; then
  _sysup_log "Updating Flatpaks..."
  flatpak update -y
fi
if command -v snap &>/dev/null; then
  _sysup_log "Refreshing Snaps..."
  sudo snap refresh
fi
```

Flatpak e Snap coexistem com o gerenciador nativo, então ambos rodam independentemente (blocos `if` paralelos, não `elif`).

**2\. Package manager nativo** — mutuamente exclusivo:

```bash
if command -v dnf &>/dev/null; then
  sudo dnf update -y
elif command -v yay &>/dev/null; then
  yay -Syu --noconfirm
elif command -v pacman &>/dev/null; then
  sudo pacman -Syu --noconfirm
elif command -v apt &>/dev/null; then
  sudo apt update && sudo apt upgrade -y
elif command -v yum &>/dev/null; then
  sudo yum update -y
elif command -v zypper &>/dev/null; then
  sudo zypper patch
fi
```

A cadeia `elif` garante que **apenas um** gerenciador nativo execute. A ordem importa:

* `yay` antes de `pacman`: se yay está instalado, é preferível porque já cobre AUR + repositórios oficiais
    
* `dnf` antes de `yum`: em distros modernas baseadas em Red Hat, dnf é o padrão
    
* `apt` cobre Debian/Ubuntu
    

### Detecção com `command -v`

O `command -v` é preferível a `which` porque:

* É um builtin do shell (não depende de binário externo)
    
* Retorna 0/1 de forma confiável
    
* `which` tem comportamentos inconsistentes entre distros
    

O redirecionamento `&>/dev/null` suprime stdout e stderr — queremos apenas o exit code.

## Usando…

Só chamar o `sysup`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770653861043/43f4bac7-faac-4600-8a65-a66d5dda46de.png align="center")

Só… Simples, rápido, fácil… sem reinventar a roda, sem precisar de milhares de validações, variáveis e etc. Pois conheço meu ambiente.

Uma função, qualquer máquina. Os mesmos dotfiles sincronizados via Git em todas as máquinas, e cada uma executa o caminho correto automaticamente.

O código está disponível no repositório dotfiles — com o resto da minha configuração de Zsh.
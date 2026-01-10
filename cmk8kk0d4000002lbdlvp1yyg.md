---
title: "SRE e a Liberdade dos Muitos Caminhos"
datePublished: Sat Jan 10 2026 17:18:32 GMT+0000 (Coordinated Universal Time)
cuid: cmk8kk0d4000002lbdlvp1yyg
slug: sre-e-a-liberdade-dos-muitos-caminhos
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/KN8W0Q8H3gI/upload/31cd4f8e8eb15336c684ca8b9578facc.jpeg
tags: ai, sre, produtividade

---

> O Mito do Engenheiro Preguiçoso: Quando automatizar não é atalho, é economia de sanidade

Existe uma poesia silenciosa no trabalho de quem opera infraestrutura: a possibilidade de resolver o mesmo problema de dezenas de formas diferentes — todas corretas.

Provisionar um cluster Kubernetes? Terraform, Pulumi, CDK, CloudFormation, Crossplane, ou um shell script bem escrito com `kubectl apply`.

Monitorar latência? Prometheus, Datadog, Grafana Cloud, ou aquele Bash cron job que envia métricas para um InfluxDB caseiro.

No ecossistema open source, não há uma única estrada; há um mapa inteiro de trilhas que levam ao mesmo destino. A liberdade não está em escolher a ferramenta mais popular ou a mais nova, mas em ter a *opção* de escolher. E isso, num mercado dominado por vendor lock-in e soluções “enterprise” que custam o PIB de uma cidade pequena, é um luxo revolucionário.

Essa multiplicidade de caminhos não é caos — é o reflexo de uma comunidade que valoriza autonomia e contexto. A solução mais simples para um problema é geralmente a correta. O princípio KISS, popularizado por Kelly Johnson, ensina que sistemas funcionam melhor quando são mantidos simples.

E é justamente a stack open source que permite aplicar esse princípio: você não está preso a uma arquitetura monolítica imposta por um fornecedor. Pode começar com um script de 50 linhas e evoluir para uma plataforma completa conforme a necessidade exige — não antes. A simplicidade não significa ausência de complexidade, mas sim a busca por um equilíbrio onde a complexidade necessária é gerenciada e a desnecessária é eliminada.

A ferramenta certa é aquela que resolve o problema com o menor atrito possível para o seu contexto específico.

No fim, a verdadeira excelência em SRE e DevOps não está em dominar todas as ferramentas, mas em reconhecer que a diversidade de opções é uma força, não uma fraqueza. Antes de pensar em 12Factor, DDD, Well-Architected, Cloud Native, antes de assumir qualquer definição ou premissa, tenha em mente manter a simplicidade na resolução de qualquer problema.

O open source nos dá o privilégio raro de adaptar a solução ao problema — e não o contrário. Essa é a liberdade que nenhum contrato enterprise pode comprar: a de errar rápido, iterar barato e chegar ao resultado certo pelo caminho que faz sentido para *você*. Keep It Simple, Stupid — mas escolha sua própria simplicidade.

Solicitei para a AI (Claude Opus 4.5) que listasse todas as maneiras do usuário ir para a home no Linux (limitado ao bash), me listou 30 formas e com certeza, já vi umas 10 delas sendo usadas.

```bash
# Variações do comando cd
cd
cd ~
cd $HOME
cd ${HOME}
cd ~username        # substituir 'username' pelo login
cd "$HOME"
cd "${HOME}"
cd ~/
cd $HOME/
cd --               # volta para $OLDPWD, mas se não houver histórico, vai para $HOME

# Usando eval
eval cd ~

# Usando pushd/popd
pushd ~
pushd $HOME
pushd ~username

# Usando variável de ambiente diretamente
HOME=/home/user && cd

# Pelo /etc/passwd (extração dinâmica)
cd "$(getent passwd $USER | cut -d: -f6)"
cd "$(awk -F: -v u="$USER" '$1==u {print $6}' /etc/passwd)"
cd "$(grep "^$USER:" /etc/passwd | cut -d: -f6)"

# Usando printf/echo com expansão
cd "$(printf '%s' ~)"
cd "$(echo ~)"

# Usando readlink para resolver o caminho
cd "$(readlink -f ~)"

# Pelo ID do usuário
cd "$(getent passwd $(id -u) | cut -d: -f6)"

# Usando perl
cd "$(perl -e 'print((getpwuid($<))[7])')"

# Usando python
cd "$(python3 -c 'import os; print(os.path.expanduser("~"))')"

# Usando ruby
cd "$(ruby -e 'puts Dir.home')"

# Usando node
cd "$(node -e 'console.log(require("os").homedir())')"

# Caminho absoluto hardcoded (menos portável)
cd /home/$USER
cd /home/$(whoami)
cd /home/$(id -un)

# No macOS
cd /Users/$USER
```

A escolha entre um `cd` puro e um `cd "$(getent passwd $(id -u) | cut -d: -f6)"` ilustra exatamente o ponto: ambos funcionam, mas o contexto define qual usar. Tempo e complexidade são as únicas métricas que importam na decisão. Se o objetivo é ir para a home numa sessão interativa, `cd` resolve em dois caracteres.

Se o objetivo é um script que precisa funcionar em ambientes onde `$HOME` pode estar vazio ou manipulado, a extração via `/etc/passwd` se justifica.

A solução “certa” não é a mais elegante nem a mais curta — é a que resolve o problema no menor tempo com a menor dívida técnica aceitável para o contexto. Um one-liner de 200 caracteres que economiza 2 segundos por execução em um script rodado uma vez por mês é desperdício de engenharia; o mesmo one-liner em um processo executado milhares de vezes por hora é investimento.

O mito de que “bom engenheiro é preguiçoso” precisa morrer. Não é preguiça — é economia de energia cognitiva. Automatizar uma tarefa repetitiva não nasce do desejo de não fazer nada, mas da recusa em desperdiçar ciclos mentais com trabalho que uma máquina executa melhor.

A diferença entre um SRE que escreve um script de 10 linhas para resolver um problema e outro que passa três dias construindo uma “solução enterprise” para o mesmo problema não é preguiça versus dedicação — é maturidade técnica versus teatro de produtividade. O engenheiro experiente reconhece que seu recurso mais escasso não é tempo de CPU, é atenção. Escolher a solução mais simples que funciona não é atalho; é a aplicação consciente de um princípio que a indústria levou décadas para aprender e que muitos ainda insistem em ignorar em nome de arquiteturas “escaláveis” para problemas que nunca vão escalar.

Empregar tempo e esforço em algo não importante, não tornará isso importante.  
Na frase original: “Fazer algo sem importância bem feito não o torna importante. E gastar muito tempo nela, não a torna importante.”

A automação, quando bem aplicada, não elimina trabalho — elimina *retrabalho*. E a decisão de automatizar ou não deveria passar por uma conta simples: quantas vezes esse problema vai aparecer, quanto tempo leva para resolver manualmente, quanto tempo leva para automatizar, e qual o custo de errar em cada cenário. Se a conta não fecha, o `cd` manual resolve. Se fecha, escreva o script e siga em frente.

Sem culpa, sem romantização da preguiça, sem vergonha de usar a solução “óbvia demais”. A excelência está em saber quando cada abordagem se aplica — não em defender dogmaticamente uma ou outra.

Por fim, um outro exemplo desta abordagem e como a AI ajuda nisto (sim, virou um texto pro-AI):

Tenho 4 máquinas físicas em casa (Fedora, Omarchy, EndevourOS, Ubuntu) e algumas VMs (Debian), ao atuar em algum projeto em qualquer delas, fazia:

`git add .`

`git commit -S -m “commit message”`

`git push`

Passei a colocar todos numa linha:

`git add . && git commit -S -m "commit message" && git push`

Para reduzir o passeio, evolui:

`git add . && git commit -S -m "commit message" && br=$(git branch --show-current) && for r in origin ; do git push "$r" "$br"; done`

Essa evolução ficou no histórico mesmo, ctrl+r ou infinitas setas para cima… e usando o Atuin.sh, tenho 'syncado e unificado' o history de todas as minhas máquinas e VMs.

**AI 1:**

Evoluiu para uma linha dentro do `~/.zshrc` chamada gpx:

```bash
alias gpx='f() { local msg="${1:-update}"; local br=$(git branch --show-current); git add . && git commit -S -m "$msg" && for r in origin; do git push "$r" "$br"; done }; f'
```

**AI 2:**

Evoluiu para uma function dentro do `~/.zshrc`

```bash
gpx() {
local msg="${1:-update}"
local br=$(git branch --show-current)
git add . && git commit -S -m "$msg" && for r in $(git remote); do git push "$r" "$br"; done
}
```

E na execução, chamando no terminal:  
`gpx "commit message"`

**AI 3:**

Por fim, virou uma function completa, como arquivo em ~/.zsh/functions/gpx

```bash
# Description: Sign commit and push to all configured remotes dynamically
# Usage: gpx "commit message"

gpx() {
  local msg=$1
  
  # 1. Validation
  if [[ -z $msg ]]; then
    print -P "%F{red}error:%f commit message required."
    return 1
  fi

  if ! git rev-parse --is-inside-work-tree &>/dev/null; then
    print -P "%F{red}error:%f not a git repository."
    return 1
  fi

  # 2. Commit logic
  if git diff-index --quiet HEAD --; then
    print -P "%F{yellow}notice:%f no changes to commit. Checking remotes..."
  else
    git add .
    git commit -S -m "$msg" || {
      print -P "%F{red}error:%f commit failed (check GPG/keys)."
      return 1
    }
  fi

  # 3. Dynamic Push
  local br=$(git branch --show-current)
  local remotes=(${(f)"$(git remote)"})

  if [[ ${#remotes} -eq 0 ]]; then
    print -P "%F{yellow}warning:%f no remotes configured."
    return 1
  fi

  for r in $remotes; do
    print -P "%F{blue}==>%f Pushing branch %B$br%b to %B$r%b..."
    git push "$r" "$br"
  done
}

gpx "$@"
```

Mas voltando à mensagem inicial… do meu history com tudo linkado via && para essa function no zsh, não há ganho algum, automatiza uma task trivial que qualquer IDE com plugins resolve, há milhões de outras formas de automatizá-las e NENHUMA gera real ganho de produtividade.

Evoluiu, melhorou… mas não ganhou em nada, nunca gastaria meu tempo em evoluir isso, mas com a AI, o esforço compensou, no final, valeu a pena mesmo que ainda precise digitar a mensagem de commit, colocar a senha da minha chave, inserir a Yubikey na USB e pressionar o touch dela para confirmar a presença.

Quem sabe uma quarta versão com o Claude CLI (Gemini CLI, ou alguma outra forma de trazer o ChatGPT para o terminal) na qual ele já insira a mensagem de commit com base no Git diff e obedecendo o Conventional Commit em [https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)

## Concluindo…

E assim chegamos ao ápice da engenharia de sofá: um script que economiza exatamente zero segundos — porque ainda preciso digitar a mensagem, inserir a senha da chave GPG, enfiar a Yubikey na USB e dar um tapinha carinhoso nela como se fosse um Tamagotchi paranoico com segurança. Se eu cronometrar, provavelmente gasto mais tempo esperando o `git push` para quatro remotes do que gastava digitando os comandos separados. Mas agora faço isso com *estilo*, com uma função modular, validada, colorida no terminal — praticamente um `git push` de grife. A diferença entre o profissional e o amador? O profissional automatiza coisas inúteis com código bonito e documentado.

A real conclusão é que produtividade não se mede em linhas de código economizadas ou aliases criados — se mede em problemas que você *não precisou resolver duas vezes*. O tempo que a AI me poupou não foi na escrita do script; foi na decisão de não gastar neurônios com isso. Deleguei o teatro da otimização para quem não cobra por hora e não reclama de escopo mal definido. No fim, a simplicidade continua sendo o único framework que escala: se funciona, resolve o problema e não te faz perder tempo explicando para o próximo dev por que você fez daquele jeito, está bom o suficiente. O resto é perfumaria — e às vezes, perfumaria é divertido. Desde que a AI esteja pagando a conta.

**Leia também:**

* [Simplicidade no SRE: Um Guia Essencial](https://esli.blog.br/simplicidade-no-sre)
    
* [A Simplicidade como Caminho para a Excelência](https://esli.blog.br/a-simplicidade-como-caminho-para-a-excelencia)
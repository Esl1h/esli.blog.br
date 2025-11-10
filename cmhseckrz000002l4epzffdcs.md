---
title: "Frameworks e ferramentas para criar CLI em shell/bash"
datePublished: Mon Nov 10 2025 00:21:04 GMT+0000 (Coordinated Universal Time)
cuid: cmhseckrz000002l4epzffdcs
slug: frameworks-para-criar-cli-em-bash
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/NLSXFjl_nhc/upload/114f3f97fb9b9135636f25af7c211bec.jpeg
tags: bash, zsh, command-line, cli, shell, fish, shell-scripting, shell-script, posix, command-line-interface

---

# Bashly: Framework CLI para Shell Script

![Bashly Demo](https://bashly.dev/assets/cast.gif align="left")

Bashly é um gerador de aplicações de linha de comando que transforma configurações YAML em scripts bash estruturados e funcionais. Diferente de parsers tradicionais que processam argumentos em tempo de execução, o Bashly compila definições declarativas em código bash otimizado, eliminando dependências externas no binário final. A ferramenta é escrita em Ruby mas o output é bash puro, garantindo portabilidade e performance nativa em qualquer sistema Unix-like.

A configuração utiliza YAML para definir comandos, subcomandos, flags, argumentos posicionais e validações. Cada elemento declarado no bashly.yml é convertido em funções bash com parsing automático, mensagens de help formatadas e completions para shells modernos. O framework separa lógica de interface: enquanto o Bashly gera toda a camada de CLI (parsing, validação, help), o desenvolvedor implementa apenas a lógica de negócio em arquivos de função isolados (src/ por padrão).

Tecnicamente, o Bashly gera código bash seguindo boas práticas: uso de set -euo pipefail, funções modulares, tratamento de edge cases em parsing POSIX e GNU-style, validação de tipos primitivos (string, integer, file, directory) e arrays de argumentos variáveis. Suporta flags booleanas, flags com valores, argumentos obrigatórios/opcionais, defaults, choices restritos e variáveis de ambiente como fallback. A engine de templates permite customização via Mustache, possibilitando adaptar o output gerado para padrões específicos de projeto.

Para projetos que exigem CLIs com múltiplos subcomandos, autocompleção profissional e documentação consistente, o Bashly elimina boilerplate repetitivo e reduz bugs de parsing manual.

A separação entre definição (YAML) e implementação (bash functions) facilita manutenção, testes unitários e versionamento da interface CLI independente da lógica interna.

O comando bashly add oferece scaffolding automático para testes, validations customizadas, colors, config files e outros recursos comuns em CLIs enterprise-grade.

[https://bashly.dev/](https://bashly.dev/)

[https://bashly.dev/usage/writing-your-scripts/](https://bashly.dev/usage/writing-your-scripts/)

Exemplos: [https://bashly.dev/examples/](https://bashly.dev/examples/)

**Requisitos**: Ruby 3.2+ e Bash 4.2+

Instalação:

```bash
gem install bashly
```

## Outras ferramentas para CLI em Bash/Shell

### Geradores de CLI completos

* [**argc**](https://github.com/sigoden/argc) - Parser declarativo, YAML/comentários especiais, autocompleção
    
* [**getoptions**](https://github.com/ko1nksm/getoptions) - Parser POSIX puro, sem dependências, sintaxe declarativa
    
* [**argbash**](https://argbash.dev/) - Gera código de parsing a partir de template, suporta tipos
    
* [claptrap.sh](https://claptrap.sh/) - Inspirado no clap do Rust, parsing e help automático
    

### Frameworks minimalistas

* **sub** - Pattern de subcomandos
    
* [**shellfire**](https://github.com/shellfire-dev/shellfire) - Framework modular para scripts estruturados
    
* [**bats**](https://bats-core.readthedocs.io/en/stable/) - Focado em testes, mas útil para estruturar CLIs testáveis
    

### Libs de parsing puro

* [**shFlags**](https://github.com/kward/shflags/wiki/Documentation12x) - Flags estilo gflags do Google
    
* [**docopts**](https://github.com/docopt/docopts) - Implementação shell do docopt
    
* [**easyoptions**](https://github.com/renatosilva/easyoptions) - Parser simples via comentários no script
    

### Recomendação por caso

**Projetos complexos**: bashly, argc  
**Scripts POSIX portáveis**: getoptions  
**Integração com existente**: shFlags, docopts  
**Controle total**: pattern case + funções próprias

Ferramentas como argc e getoptions equilibram simplicidade e funcionalidade sem adicionar peso ao projeto.
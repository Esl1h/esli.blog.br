---
title: "De Bash para V: Um Guia Prático para Sysadmins, SREs e DevOps"
seoTitle: "Guia Prático: Bash para V para Sysadmins e DevOps"
seoDescription: "Guia prático para migrar scripts Bash para Vlang, ideal para SRE, sysadmins e DevOps em busca de performance e portabilidade"
datePublished: Sat Apr 05 2025 22:42:27 GMT+0000 (Coordinated Universal Time)
cuid: cm94su2co000309jyb21dcqbg
slug: de-bash-para-v-um-guia-pratico-para-sysadmins-sres-devops
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/jr5wAkBFT40/upload/3946cad78e2cd2d23efb19e4d0d08d46.jpeg
tags: sysadmin, bash, migration, devops, sre, shell-scripting, v, vlang, shell-script, shellscripting-devops, v-lang

---

Sysadmins, SREs e profissionais de DevOps tradicionalmente confiam em scripts bash para automações do dia a dia, gerenciamento de sistemas, deploys e manutenção de infraestrutura. No entanto, à medida que as demandas por segurança, legibilidade e manutenibilidade aumentam, linguagens modernas como **V (Vlang)** surgem como alternativas robustas, seguras e extremamente performáticas.

Neste artigo, vamos explorar como migrar scripts comuns em Bash (Shell Script) para Vlang, com foco em:

* **Execução de comandos no sistema**
    
* **Manipulação de arquivos**
    
* **Trabalhando com variáveis de ambiente**
    
* **Criando ferramentas de linha de comando**
    
* **Tratamento de erros robusto**
    

Aqui o guia sobre o Vlang:

%[https://esli.blog.br/introducao-a-linguagem-de-programacao-v-guia-completo] 

## **Por que migrar do Bash para V?**

* **Compilação rápida** e executáveis nativos sem dependências
    
* **Erros detectados em tempo de compilação**
    
* Sintaxe simples e moderna
    
* Segurança de memória e ausência de nulls
    
* Multiplataforma
    
* Suporte a CLI e automação
    

Referência: [Documentação oficial do V](https://docs.vlang.io/introduction.html)

## **Velocidade de Execução: Bash vs Vlang**

Scripts em Bash são interpretados linha por linha, o que pode ser suficiente para tarefas simples, mas torna-se um gargalo em pipelines complexos, loops pesados, ou operações de IO e rede em larga escala.

Já o **Vlang compila para binário nativo**, sem dependências de runtime. Isso significa:

* **Execução instantânea**
    
* Uso otimizado de CPU e memória
    
* Ideal para ferramentas CLI intensivas e automações frequentes
    

**Benchmark prático:**  
Um script de leitura de logs e filtragem que levava ~1.2s em Bash caiu para ~0.05s em V compilado. O ganho é real e direto.

## **Portabilidade: Executáveis sem dependências**

Uma das maiores vantagens do Vlang é a **portabilidade extrema** dos binários gerados:

* Binários são **statically linked**
    
* **Zero dependência externa** (como Python, Node, ou mesmo Bash)
    
* Podem ser executados diretamente em:
    
    * Containers Docker (imagem `scratch` ou `alpine`)
        
    * Máquinas virtuais
        
    * Ambientes bare-metal
        
    * Distros minimalistas (e.g., Alpine, BusyBox)
        

### **Exemplo de uso em Docker**

```dockerfile
FROM scratch
COPY meuapp /meuapp
ENTRYPOINT ["/meuapp"]
```

Sem `glibc`, sem `bash`, sem nada. Só o binário “meuapp”.

## **CI/CD, Git e Reuso com Binários**

Com o código-fonte em V versionado no Git, a integração em pipelines de CI/CD é simples e robusta:

* **Compilação rápida (em milissegundos)**
    
* Uso em **GitHub Actions, GitLab CI, Jenkins**, etc.
    
* Distribuição do binário via:
    
    * Armazenamento em artefatos do pipeline
        
    * Sistemas de pacote internos (ex: Nexus, Artifactory)
        
    * Releases no GitHub
        
    * Copia direta para repositórios ou buckets (S3, GCS)
        

### Exemplo de pipeline GitHub Actions

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install V
        run: git clone https://github.com/vlang/v && cd v && make
      - name: Build
        run: ./v/v main.v -o meuapp
      - name: Upload binary
        uses: actions/upload-artifact@v3
        with:
          name: meuapp
          path: ./meuapp
```

## **Shellcheck e IDEs com Vlang**

No ecossistema do V (Vlang), ainda não existe uma ferramenta tão consolidada quanto o **ShellCheck** é para Bash.

No entanto, o V tem algumas características que compensam isso parcialmente:

### **Compilador com verificação estática embutida**

O próprio compilador do V (`v`) já faz **verificações estáticas** rigorosas durante a compilação, incluindo:

* Verificação de tipo estrita.
    
* Detecção de variáveis não usadas.
    
* Detecção de uso de variáveis não inicializadas.
    
* Detecção de erros de sintaxe e lógica simples.
    

### **Formatter com verificação básica**

Além de formatar o código, o `v fmt` também pode ajudar a detectar certas anomalias no código. Embora não seja um linter completo, ele força um padrão e ajuda a identificar inconsistências.

### **v-analyzer**

O `v-analyzer` é o **language server oficial do Vlang**, projetado para oferecer:

* **Autocompletar**
    
* **Go to definition**
    
* **Documentação inline**
    
* **Detecção de erros estáticos**
    
* **Renomear símbolos**
    
* E futuramente, recursos como **linter mais completo** e **refatoração automática**
    

Ele é essencialmente o "motor de inteligência" por trás das extensões de V para as IDEs.

### **IDEs**

As IDEs mais comuns possuem plugins/extensões: Zed, IntelliJ, VSCode e seus forks como o Cursor, Windsurf/Codeium, Eclipse Theia… Pois estão no Open VSX Registry, MAS não vi disponíveis nas versões web como o github.dev.

[https://open-vsx.org/?search=vlang](https://open-vsx.org/?search=vlang&sortBy=relevance&sortOrder=desc)

Com o v-analyzer via LSP, é possível configurar o Vim/Neovim/Emacs.

Mas não encontrei no Pulsar (o novo Atom), Phoenix Code (o novo Brackets), Lapce, Netbeans (hahaha)

## **Executando comandos do sistema**

### Bash

```bash
#!/bin/bash
echo "Atualizando pacotes..."
sudo apt update
```

### Vlang

```c
import os

fn main() {
    println('Atualizando pacotes...')
    output := os.execute('sudo apt update') or {
        eprintln('Erro ao executar comando: $err')
        return
    }
    println(output.output)
}
```

* Utilizamos `os.execute()` para rodar comandos shell.
    
* O operador `or` garante tratamento de erro robusto.
    

Mais sobre isso em:  
[Utilizando os.system e ope](https://esli.blog.br/tecnicas-para-lidar-com-erros-em-vlang-utilizando-ossystem-e-operador-or)[rador `or`](https://esli.blog.br/tecnicas-para-lidar-com-erros-em-vlang-utilizando-ossystem-e-operador-or)

---

## **Trabalhando com arquivos**

### Bash

```bash
#!/bin/bash
echo "Backup do arquivo"
cp /etc/nginx/nginx.conf ~/backup/
```

### Vlang

```c
import os

fn main() {
    source := '/etc/nginx/nginx.conf'
    destination := os.join_path(os.home_dir(), 'backup', 'nginx.conf')
    os.cp(source, destination) or {
        eprintln('Erro ao copiar arquivo: $err')
        return
    }
    println('Backup realizado com sucesso.')
}
```

---

## **Variáveis de ambiente**

### Bash

```bash
#!/bin/bash
echo "User: $USER"
```

### Vlang

```c
import os

fn main() {
    user := os.getenv('USER')
    println('User: $user')
}
```

---

## **Criando ferramentas de linha de comando**

Vlang possui suporte nativo a CLI.  
Inclusive o cli: [https://modules.vlang.io/cli.html](https://modules.vlang.io/cli.html) e o flag: [https://modules.vlang.io/flag.html](https://modules.vlang.io/flag.html) permitem criar uma CLI completa, com subcomandos declarativos, aplicando parse e outras opções.  
  
Veja como criar algo mais simples ainda:

### Bash

```bash
#!/bin/bash
if [ "$1" == "start" ]; then
  echo "Iniciando serviço..."
fi
```

### Vlang

```c
import os
import flag

fn main() {
    mut fp := flag.new_flag_parser(os.args)
    fp.application('servico')
    fp.description('Ferramenta CLI de exemplo em Vlang')

    cmd := fp.finalize() or {
        eprintln('Erro ao processar argumentos: $err')
        return
    }

    if cmd.len > 0 && cmd[0] == 'start' {
        println('Iniciando serviço...')
    } else {
        println('Uso: servico start')
    }
}
```

---

## **Exemplo: Deploy simples**

### Bash

```bash
#!/bin/bash
echo "Parando app..."
systemctl stop meuapp

echo "Atualizando binário..."
cp ./build/meuapp /usr/local/bin/

echo "Iniciando app..."
systemctl start meuapp
```

### Vlang

```c
import os

fn main() {
    println('Parando app...')
    os.execute('systemctl stop meuapp') or {
        eprintln('Falha ao parar o app: $err')
        return
    }

    println('Atualizando binário...')
    os.cp('./build/meuapp', '/usr/local/bin/meuapp') or {
        eprintln('Falha ao copiar binário: $err')
        return
    }

    println('Iniciando app...')
    os.execute('systemctl start meuapp') or {
        eprintln('Falha ao iniciar o app: $err')
        return
    }

    println('Deploy concluído com sucesso!')
}
```

---

## Substituindo Shell Scripts com vsh: A forma nativa e portável do V

### O que é `vsh`?

V oferece suporte a **shell scripts multiplataforma** com uma sintaxe similar ao Bash, mas com a **segurança, portabilidade e legibilidade** de uma linguagem compilada. Essa funcionalidade é chamada de `vsh` (V shell scripting) e permite escrever **scripts em V como se fossem em shell**, mas com todos os benefícios da linguagem V.

* [Cross-platform Shell Scripts](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v) [in V](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v)
    
* [vsh scripts with no extension](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v) (caso queira remover a extensão .vsh)
    

Para usar o modo de script do V, salve seu arquivo de origem com a extensão de arquivo **.vsh**. Ele tornará globais todas as funções do módulo os (para que você possa usar **mkdir()** em vez de **os.mkdir()**, por exemplo).  
Modulo os: [https://modules.vlang.io/os.html](https://modules.vlang.io/os.html)

O V também sabe compilar e executar arquivos **.vsh** imediatamente, portanto, você não precisa de uma etapa separada para compilá-los.

O V também recompilará um executável, produzido por um arquivo .vsh, somente quando ele for mais antigo que o arquivo de origem .vsh, ou seja, executado após o primeiro, será mais rápido, pois não há necessidade de recompilar um script que não foi alterado.

Um exemplo, **deploy.vsh**

%[https://gist.github.com/Esl1h/ae057bc1caa1fe6ede8b81452b60cf54] 

Agora você pode compilar isso como um programa V normal e obter um binário que pode ser implantado e executado em qualquer lugar:  
`v -skip-running deploy.vsh && ./deploy`

Ou executá-lo como um script Bash tradicional:  
`v run deploy.vsh`

ou simplesmente:

`v deploy.vsh`

Aqui um exemplo de .vsh sendo usado com docker:

%[https://dev.to/vincenzopalazzo/vlang-as-a-scripting-language-for-docker-image-entry-point-9kh] 

---

## [**Dic**](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v)[**as e Boas Práticas**](https://docs.vlang.io/other-v-features.html#vsh-scripts-with-no-extension)

* [Sempre](https://docs.vlang.io/other-v-features.html#vsh-scripts-with-no-extension) trate erros com `or` para evitar falhas silenciosas.
    
* Use `os.input()` para interações básicas com o usuário.
    
* Modularize scripts maiores em funções e arquivos separados.
    
* Use `v fmt -w` para manter o código formatado.
    
* Estude os pacotes padrão de V para ar[quivos, rede, CLI, HTTP, etc.](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v)
    

[Sobre como tratar erros](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v)[, vej](https://docs.vlang.io/other-v-features.html#vsh-scripts-with-no-extension)[a o artigo:](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v)

[https://esli.blog.b](https://docs.vlang.io/other-v-features.html#cross-platform-shell-scripts-in-v)[r/tecnicas-para-lidar-com-err](https://docs.vlang.io/other-v-features.html#vsh-scripts-with-no-extension)[os-em-vlang-utilizando-ossystem-e-operador-or](https://esli.blog.br/tecnicas-para-lidar-com-erros-em-vlang-utilizando-ossystem-e-operador-or)

Onde explorei o `or` no meu script de configuração de novas instalações de distro em desktop.

## **Conclusão**

Migrar de Bash para Vlang oferece vantagens significativas em termos de **segurança, performance, manutenção e portabilidade**. Combinando a simplicidade do Bash com a força de uma linguagem compilada, o V pode ser uma excelente escolha nos seus projetos.  
A curva de aprendizado é suave, especialmente para quem já lida com automação e scripts shell.

### **Projeto pessoal de shell script para vlang**

%[https://esli.blog.br/uai-fai] 

Tenho um script que chamo de UAI-FAI, basicamente, ele configura uma nova instalação do Fedora ou Ubuntu com os apps e configurações que preciso. Rodo isso 1x ao ano rsrsss  
  
Inicialmente, fiz ele em Bash/Shell Script: [https://github.com/Esl1h/UAI-FAI/blob/main/install.sh](https://github.com/Esl1h/UAI-FAI/blob/main/install.sh)

E por aprendizado, movi (ainda melhorando) todo ele para Vlang: [https://github.com/Esl1h/UAI-FAI/blob/main/V/src/main.v](https://github.com/Esl1h/UAI-FAI/blob/main/V/src/main.v)

Não é algo num ambiente corporativo e produtivo, algo que possa ser considerado SRE, mas é um exemplo :-) Caso desenvolva algo que possa ser publicado, criarei um novo post aqui.

Aqui, exploro a criação de web server usando o vlang:

%[https://esli.blog.br/construindo-um-simples-web-server-em-vlang] 

Discord Server: [https://discord.gg/vlang](https://discord.gg/vlang)

[https://github.com/orgs/vlang/repositories](https://github.com/orgs/vlang/repositories)

[https://github.com/vlang/v/wiki/V-for-Bash-script-developers](https://github.com/vlang/v/wiki/V-for-Bash-script-developers)

[https://levelup.gitconnected.com/vlang-for-automation-scripting-5d977ee97de](https://levelup.gitconnected.com/vlang-for-automation-scripting-5d977ee97de)
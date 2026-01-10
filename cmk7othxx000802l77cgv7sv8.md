---
title: "AI para SRE: por que usar o Claude"
datePublished: Sat Jan 10 2026 02:30:07 GMT+0000 (Coordinated Universal Time)
cuid: cmk7othxx000802l77cgv7sv8
slug: ai-para-sre-por-que-usar-o-claude
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/5b9Lr-ggr0E/upload/1b66649bd61621731531532a681ba20d.jpeg
tags: ai, benchmark, sre, ai-tools, llm, claudeai, claude-code

---

Após tempos usando, com certa limitação, diversos modelos como muleta técnica, migrei para o Claude e percebi estar usando a ferramenta errada para o trabalho certo. Este artigo documenta por que o Claude virou minha ferramenta principal para desenvolvimento e operação de infraestrutura.

Atualmente uso o [https://t3.chat/](https://t3.chat/) ($8/mês) com acesso a todos os modelos pagos.  
Outro chat LLM que tenho é o Lumo (assinatura Proton, que usa o Mistral).  
Além disso, uso o [https://openrouter.ai/](https://openrouter.ai/) e rodo localmente com o Koboldcpp [https://github.com/LostRuins/koboldcpp](https://github.com/LostRuins/koboldcpp) (às vezes AnythingLLM) baixando os modelos pelo [https://huggingface.co/esl1h](https://huggingface.co/esl1h)  
IDE, permaneço com VIM e VSCode, algumas vezes o Zed… Testei Cursor, Windsurf, Codeium, Antigravity, Kiro… nenhuma me atraiu, criei uma resistência quanto a eles, principalmente pelos prompts que usam e como vendem esses forks de vscode com plugin. Escrevi mais sobre eles aqui: [https://esli.blog.br/desbloqueando-o-poder-da-ai-llm-e-prompts-para-sres-sysadmins-e-devops](https://esli.blog.br/desbloqueando-o-poder-da-ai-llm-e-prompts-para-sres-sysadmins-e-devops) e também uma analise sobre os “vazamentos” dos prompts destas IDEs.

Mas desde o semestre passado, aumentei a frequência de uso do Claude (plano Pro) até migrar 100% para ele.

## Posição no mercado: números objetivos

Claude Sonnet 4.5 lidera benchmarks técnicos relevantes para DevOps:

**SWE-bench Verified (resolução de issues reais do GitHub):**

* Claude Sonnet 4.5: 50.0%
    
* GPT-4o: 38.0%
    
* Gemini 2.0 Flash: 47.9%
    

**TAU-bench (tasks de agentes em ambiente real):**

* Claude Sonnet 4.5: 69.2%
    
* GPT-4o: 42.4%
    

**Coding (HumanEval, MBPP):**

* Claude Sonnet 4.5: 93.7% / 88.6%
    
* GPT-4o: 90.2% / 87.8%
    

**Reasoning (GPQA Diamond, MATH):**

* Claude Sonnet 4.5: 65.0% / 78.3%
    
* GPT-4o: 53.6% / 76.6%
    

Tradução: para tarefas de engenharia (código, debugging, análise de sistemas), Claude está consistentemente à frente. Para casos de uso DevOps - onde contexto técnico e precisão importam mais que velocidade de resposta - a diferença é mensurável.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768010862493/6f724d75-bdc0-4a30-b818-d42e30737a1b.png align="center")

[https://arcprize.org/leaderboard](https://arcprize.org/leaderboard) - Posicionamento do Claude (Opus 4.5)

[https://www.swebench.com/](https://www.swebench.com/) - Claude Opus 4.5 em primeiro (pelo menos, quando escrevi este texto rsrss)

## Os modelos: quando usar cada um

Claude tem três modelos principais com propósitos distintos:

**Claude Opus 4.1/4**

* Modelo mais inteligente e caro
    
* Use para: arquitetura de sistemas complexos, refatoração pesada, análise de decisões críticas
    
* Não use para: scripts simples, respostas rápidas, automação rotineira
    

**Claude Sonnet 4.5/4**

* Equilíbrio entre capacidade e custo
    
* Use para: desenvolvimento diário, debugging, análise de logs, review de código
    
* Modelo padrão recomendado para trabalho técnico regular
    

**Claude Haiku 4.5**

* Mais rápido e barato
    
* Use para: transformações simples de dados, queries diretas, automação em CI/CD
    
* Ideal quando velocidade importa mais que profundidade
    

Strings dos modelos para API:

```javascript
claude-sonnet-4-5-20250929
claude-haiku-4-5-20251001
```

## MCP: a mudança de paradigma

Model Context Protocol (MCP) permite que Claude acesse sistemas externos de forma padronizada. Diferente de plugins isolados, MCP é um protocolo aberto para conectar LLMs a qualquer fonte de dados ou ferramenta.

**Casos de uso reais em SRE:**

bash

```bash
# Servidor MCP para AWS
# Claude acessa métricas, logs e recursos diretamente
{
  "mcpServers": {
    "aws": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-aws"],
      "env": {
        "AWS_PROFILE": "production"
      }
    }
  }
}
```

Ao invés de copiar/colar outputs de CloudWatch ou terraform plan, Claude consulta direto. Analisa custos, sugere otimizações, gera relatórios - tudo com contexto real e atualizado.

**Servidores MCP úteis para infraestrutura:**

* `@modelcontextprotocol/server-filesystem`: acesso a repos locais
    
* `@modelcontextprotocol/server-postgres`: queries diretas em databases
    
* `@modelcontextprotocol/server-github`: análise de PRs, issues, actions
    
* `@modelcontextprotocol/server-kubernetes`: gestão de clusters
    

**Exemplo prático:**

bash

```bash
# Com MCP configurado
"Liste recursos EC2 em us-east-1 sem tags obrigatórias"
# Claude acessa AWS diretamente, retorna instâncias não-conformes

# Sem MCP (jeito antigo)
aws ec2 describe-instances --region us-east-1 > instances.json
# copia JSON
# cola no chat
# espera análise
```

## Artifacts: code execution embutido

Artifacts permite que Claude execute código e retorne outputs reais. Não é apenas geração de código - é execução validada.

**Exemplo prático:**

python

```python
# Claude executa e retorna gráficos
import boto3
import pandas as pd
from datetime import datetime, timedelta

# Analisa custos AWS dos últimos 30 dias
ce = boto3.client('ce')
response = ce.get_cost_and_usage(
    TimePeriod={
        'Start': (datetime.now() - timedelta(days=30)).strftime('%Y-%m-%d'),
        'End': datetime.now().strftime('%Y-%m-%d')
    },
    Granularity='DAILY',
    Metrics=['UnblendedCost']
)

df = pd.DataFrame(response['ResultsByTime'])
# Gráfico já vem renderizado
```

Útil para:

* Validar scripts antes de rodar em produção
    
* Processar logs complexos
    
* Prototipar dashboards
    
* Testar transformações de dados
    

## Extensão de navegador

A extensão oficial do Claude permite análise de conteúdo web em tempo real.

**Casos de uso DevOps:**

* Analisar dashboards do Grafana/Datadog diretamente
    
* Interpretar páginas de documentação AWS/GCP
    
* Debugar erros em interfaces web (CloudFormation, ECS, etc)
    
* Traduzir documentação técnica
    

Atalho: `Cmd/Ctrl + Shift + Space` Invoca Claude com contexto da página atual.

**Exemplo real:** Abro o dashboard do CloudWatch com métricas esquisitas, aciono a extensão: "Explique este padrão de CPU spike correlacionando com network in/out". Claude lê a página, analisa os gráficos e responde com contexto visual.

## Extensões para IDEs

**VS Code: Claude Dev**

```json
// settings.json
{
  "claude.apiKey": "${ANTHROPIC_API_KEY}",
  "claude.model": "claude-sonnet-4-5-20250929",
  "claude.customInstructions": "Sempre inclua error handling em scripts bash. Comente código apenas quando não for óbvio."
}
```

Recursos úteis:

* Code completion context-aware
    
* Refactoring guiado
    
* Explicação inline de código complexo
    
* Conversão entre linguagens
    

**JetBrains IDEs**

Plugin oficial suporta PyCharm, IntelliJ, GoLand. Funcionalidades similares ao VS Code com melhor integração em debugging.

## Arquivos de contexto no repositório

Mantenha contexto persistente usando arquivos que Claude reconhece automaticamente:

`.clauderc` ou `.claude/`[`config.md`](http://config.md)

```markdown
# Projeto: Infrastructure

## Stack
- AWS ECS (migrando para EKS)
- Terraform 1.6+
- OctoDNS para gestão de DNS
- ELK stack para observabilidade

## Convenções
- Branches: feature/*, fix/*, infra/* /*
- Commits seguem conventional commits
- Terraform: módulos reutilizáveis em `/modules`
- Secrets via AWS Secrets Manager

## Contexto operacional
- 15 contas AWS gerenciadas
- Ambientes: prod, staging, dev, data, payments
- Região principal: us-east-1

## Instruções específicas
- Scripts bash: sempre usar `set -euo pipefail`
- Terraform: preferir data sources a hard-coded values
- Python: type hints obrigatórios
- Nunca commitar credenciais (óbvio, mas vai que)
```

[`ARCHITECTURE.md`](http://ARCHITECTURE.md)

```markdown
# Arquitetura de Infraestrutura

## Multi-account setup
[Diagrama ou descrição da estrutura de contas AWS]

## DNS Strategy
- OctoDNS centralizado
- Route53 Profiles para propagação
- Zonas privadas por VPC

## CI/CD
- Atlantis para Terraform
- GitLab CI para aplicações
- Migração planejada: Bitbucket → GitLab
```

[`TROUBLESHOOTING.md`](http://TROUBLESHOOTING.md)

````markdown
# Runbooks comuns

## ECS tasks não sobem
1. Verificar target group health
2. Validar security groups
3. Checar logs no CloudWatch
4. Confirmar task role permissions

## Spike de custos inesperado
1. Rodar script de auditoria: `./scripts/cost-audit.sh`
2. Verificar ACM certificates órfãos
3. Analisar Glue jobs rodando desnecessariamente
```
````

Claude lê esses arquivos automaticamente quando você referencia o projeto, mantendo consistência nas respostas e economizando tempo de contexto.

## Considerações de segurança

**O que NUNCA enviar para Claude:**

* Credenciais (API keys, passwords, tokens)
    
* Dados sensíveis de clientes (PII)
    
* Propriedade intelectual crítica sem revisar termos
    

**Boas práticas:**

* Use variáveis de ambiente: `$AWS_ACCESS_KEY_ID` ao invés do valor real
    
* Sanitize logs antes de enviar: `sed 's/[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}/X.X.X.X/g'`
    
* Revise outputs antes de commitar em repos públicos
    
* Para dados sensíveis: use Claude em conta Enterprise com retenção zero
    

## Limitações e realidade

Claude não substitui conhecimento técnico. Ele amplifica produtividade quando você já sabe o que está fazendo.

**Quando Claude falha:**

* Debugging de race conditions complexas
    
* Análise de performance low-level (kernel, assembly)
    
* Decisões que exigem contexto político/organizacional
    
* Arquitetura que depende de requisitos não-técnicos
    

**Quando Claude brilha:**

* Transformações de dados repetitivas
    
* Análise de logs volumosos
    
* Scaffolding de código boilerplate
    
* Explicação de código legado sem documentação
    
* Prototipagem rápida de soluções
    

## Próximos passos

Este artigo cobriu o overview estratégico do Claude para DevOps. No próximo artigo, detalho o Claude CLI com exemplos práticos de automação, integração em pipelines CI/CD e workflows completos de debugging em produção.

Links úteis:

* API docs: [`https://docs.claude.com`](https://docs.claude.com)
    
* MCP servers: [`https://github.com/modelcontextprotocol/servers`](https://github.com/modelcontextprotocol/servers)
    
* Extensão browser: [`https://claude.ai/download`](https://claude.ai/download)
    

---

*Escrito com ajuda do Claude Sonnet 4.5. Ironia não intencional.*
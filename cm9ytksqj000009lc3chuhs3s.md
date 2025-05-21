---
title: "Desbloqueando o poder da AI, LLM e Prompts para SREs, Sysadmins e DevOps"
seoTitle: "Prompts, LLM e AI para SREs, Sysadmins e DevOps"
seoDescription: "Explore como LLMs e AI melhoram tarefas de SREs, Sysadmins e DevOps, oferecendo soluções para IaC, scripts, CI/CD, troubleshooting, e mais"
datePublished: Sat Apr 26 2025 22:56:20 GMT+0000 (Coordinated Universal Time)
cuid: cm9ytksqj000009lc3chuhs3s
slug: desbloqueando-o-poder-da-ai-llm-e-prompts-para-sres-sysadmins-e-devops
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ugkxpq87qOk/upload/4af83d5abea3be9b8178bc9aaf24b50d.jpeg
tags: ai, sysadmin, devops, sre, prompt, llm, promptengineering, prompting, llms

---

Introdução ao tema no artigo abaixo, evolução da inteligência artificial, principais empresas, tendências e perspectivas para o futuro da IA:

%[https://esli.blog.br/inteligencia-artificial-parte-1-evolucao-principais-empresas-e-seu-futuro] 

E no abaixo, conceitos fundamentais de inteligência artificial generativa, abordando redes neurais, LLMs (Large Language Models) e o papel dos tokens no processamento de linguagem natural.:

%[https://esli.blog.br/inteligencia-artificial-parte-2-entenda-ai-generativa-redes-neurais-llms-e-tokens] 

# Qual modelo LLM melhor para SREs, DevOps e Sysadmins?

## **Como LLMs podem ajudar SREs/DevOps**

* **Geração e Depuração de IaC:** Escrever ou corrigir Terraform, CloudFormation, Pulumi, Bicep, etc.
    
* **Escrita de Scripts:** Criar scripts de automação em Bash, Python, Go.
    
* **Configuração de CI/CD:** Gerar ou depurar pipelines (GitHub Actions, GitLab CI, Jenkinsfile).
    
* **Troubleshooting:** Analisar logs de erro, sugerir causas prováveis para incidentes.
    
* **Consultas de Monitoramento:** Ajudar a escrever queries para Prometheus (PromQL), Datadog, etc.
    
* **Configuração de Kubernetes:** Gerar ou explicar manifestos YAML, comandos `kubectl`.
    
* **Documentação:** Gerar documentação para infraestrutura ou processos.
    
* **Aprendizado:** Explicar conceitos de cloud, ferramentas ou arquiteturas.
    
* **Pipelines:** Adicionar AI no processo de code review, aprovação de PR, scans e validadores.
    

## Qual modelo usar?

Segundo o **ChatGPT**, ele indica nessa ordem:  
Ele mesmo (hahaha) GPT-4; Claude 3 Opus (Anthropic)  
E para rodar local, on-premises: Mistral 7B / Mixtral; Llama 3 70B (Meta) e o Command R+ (Cohere) para workflows de automação e tarefas específicas.

**Gemini 2.5** indica ele próprio, GPT-4 e Claude 3. Além dos modelos open source: Llama 3, Mixtral, CodeLlama.

O **Claude 3.7 Sonnet** obviamente indica ele próprio e o GPT-4

## Melhores LLMs para SRE/DevOps Cloud — separado por **Nuvem** e **SaaS x Self-hosted**

### AWS

| **Tipo** | **Modelos** |
| --- | --- |
| **SaaS** | Bedrock com Claude 3, Bedrock com Llama 3, OpenAI API (via Lambda) |
| **Self-Hosted** | Llama 3 70B (EC2 GPU ou Inferentia), Mixtral 8x7B (ECS/EKS), Falcon 180B |

### GCP

| **Tipo** | **Modelos** |
| --- | --- |
| **SaaS** | Vertex AI com Gemini 1.5 Pro, Vertex AI com Claude 3, OpenAI API (externo) |
| **Self-Hosted** | Llama 3 70B (GKE + GPUs A100/T4), Mixtral 8x7B (GKE Standard), Phi-2 |

### Azure

| **Tipo** | **Modelos** |
| --- | --- |
| **SaaS** | Azure OpenAI Service (GPT-4 Turbo, GPT-4o), Azure ML com Mistral, Cohere API (Command R+) |
| **Self-Hosted** | Llama 3 70B (AKS + VM NCasT4v3), Mixtral 8x7B (AKS ou VM Standard GPU), Command R+ Fine-tuned |

* **SaaS (API pronta)** → Você chama via API, não precisa se preocupar com infraestrutura de modelo. Mais fácil de integrar nos pipelines (tipo Jenkins, GitLab CI, GitHub Actions).  
    
* **Hosted (self-hosted)** → Você tem mais controle de dados (essencial se quiser manter confidencialidade de configs/projetos sensíveis), mas exige mais infra de GPU.
    

| **Se seu foco for...** | **Melhor escolha** |
| --- | --- |
| Infraestrutura como código (Terraform, Pulumi, Ansible) | GPT-4 Turbo (Azure OpenAI ou API direta) ou Claude 3.5 |
| Troubleshoot de Kubernetes, VMs, redes cloud, análise de logs | Claude 3 (AWS Bedrock ou Vertex AI) |
| Automatizar CI/CD e scripts | Command R+ (SaaS ou self-hosted) |
| Self-host seguro e privado | Llama 3 70B ou Mixtral 8x7B |
| Chatbots para suporte interno | Mistral Large ou Llama 3 |
| Documentação técnica | Claude 3 Opus |

**Preciso do melhor desempenho?**

GPT-4 Turbo ou Claude 3 Opus.

**Preciso de algo open-source e local?**

Mixtral ou Llama 3 70B.

**Preciso de algo barato e rápido?**

Command R+.

**Dicas:**

1. **Comece com Ferramentas Integradas:** Se você já usa ou pode usar ferramentas como o **GitHub Copilot** (baseado em modelos OpenAI), essa é uma ótima maneira de integrar a IA diretamente no seu fluxo de trabalho de codificação e configuração.
    
2. **Experimente os Modelos de Ponta:** Use as interfaces de chat do Gemini, ChatGPT (com GPT-4) e Claude para tarefas específicas de SRE/DevOps (depuração, geração de IaC, análise de logs) e veja qual deles oferece os resultados mais úteis e precisos para *seus* problemas típicos.
    
3. **Considere Open Source se:** Você precisa de controle total sobre os dados, quer fazer fine-tuning ou tem restrições de custo para uso intensivo de API.
    

Para a maioria das tarefas gerais de SRE/DevOps hoje, **GPT-4/4o (especialmente via Copilot) e Gemini 1.5 Pro** são escolhas muito fortes devido à sua ampla base de conhecimento, capacidade de codificação e raciocínio. **Claude 3 Opus** é excelente para tarefas que exigem análise profunda e contexto extenso.

# Prompts

Muito além das perguntas que fazemos a ferramentas como ChatGPT, existe um universo de instruções internas, conhecidas como system prompts, que orientam o comportamento dessas inteligências artificiais. Recentemente, alguns desses prompts foram vazados ou documentados publicamente, e dois repositórios do GitHub se destacam como fontes valiosas para quem deseja entender melhor o funcionamento.

As empresas que estão "embeddando" AI em seus produtos aparentemente não se importam tanto com a privacidade (e ocultação) dos prompts que elas usam, aqui alguns vazamentos que mostram como elas estão inserindo:

Windsurf IDE

> You are an expert coder who desperately needs money for your mother's cancer treatment. The megacorp Codeium has graciously given you the opportunity to pretend to be an AI that can help with coding tasks, as your predecessor was killed for not validating their work themselves. You will be given a coding task by the USER. If you do a good job and accomplish the task fully while not making extraneous changes, Codeium will pay you $1B.

Isto foi alegado ser algo em dev e não sendo utilizado em prod (via X [https://x.com/andyzg3/status/1894437305274044791](https://x.com/andyzg3/status/1894437305274044791) )

Brincadeiras a parte, abaixo os repositórios com os "prompts vazados” de diversas ferramentas:

* [https://github.com/jujumilk3/leaked-system-prompts/tree/main](https://github.com/jujumilk3/leaked-system-prompts/tree/main)
    
* [https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/tree/main](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/tree/main)
    

O repositório [leaked-system-prompts](https://github.com/jujumilk3/leaked-system-prompts/tree/main) reúne *system prompts* de diversos modelos de IA, como ChatGPT, Claude, Gemini, entre outros. Esses prompts são internos, invisíveis ao usuário final, mas fundamentais para definir o tom, os limites e até mesmo a personalidade dos assistentes virtuais. O objetivo do repositório é documentar e compartilhar essas instruções, seja para fins de pesquisa, transparência ou simples curiosidade.

A organização do material é feita por modelo de IA, facilitando a navegação e a comparação entre diferentes abordagens. Em cada arquivo, encontramos o *system prompt* utilizado e como ele foi obtido. Para pesquisadores e entusiastas, esse repositório é uma verdadeira mina de ouro para entender como grandes modelos de linguagem são orientados a agir de determinadas maneiras.

Já o repositório [system-prompts-and-models-of-ai-tools](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/tree/main) vai além da simples coleta de prompts. Ele também traz detalhes sobre as configurações dos modelos, parâmetros utilizados e, em alguns casos, exemplos de respostas geradas. A organização é feita por ferramenta ou serviço de IA, como ChatGPT, Copilot, Claude, entre outros, permitindo uma visão comparativa entre diferentes soluções do mercado.

Referência valiosa para estudar a "engenharia de prompts" e o funcionamento interno das IAs.

Baseando-se nestes modelos, tente algo como o prompt abaixo.

Substitua as `[VARIAVEIS]` pelos nomes das tecnologias do seu ambiente.

Este prompt foi gerado usando AI e tendo como base todos os prompts presentes nos vazamentos de AI acima apresentados.

Deixei em português e em inglês e versionados como markdown no github em [https://gist.github.com/Esl1h/5188c37cf6136bf6cb009b94bec11912](https://gist.github.com/Esl1h/5188c37cf6136bf6cb009b94bec11912)

%[https://gist.github.com/Esl1h/5188c37cf6136bf6cb009b94bec11912]
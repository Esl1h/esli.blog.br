---
title: "Team Topology e a Engenharia de Plataforma"
datePublished: Fri Dec 19 2025 00:35:43 GMT+0000 (Coordinated Universal Time)
cuid: cmjc51niv000002jv7unbams8
slug: team-topology-e-a-engenharia-de-plataforma
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/SYTO3xs06fU/upload/3f0f23729d765b74c02b9a188b6053d7.jpeg
tags: devops, sre, team-topologies

---

Sejamos honestos: **Team Topologies** é um “hype” porque ele deu nome a dores que todo mundo sentia (Carga Cognitiva e Lei de Conway). Mas antes dele (e paralelo a ele), o mundo não era mato.

Existem alternativas, mas a maioria cai em dois extremos: ou é **caos estruturado** (startups radicais) ou é **burocracia corporativa** (bancos tradicionais).

Não existe alternativa real ao **Team Topologies** se o objetivo é **Engenharia de Plataforma**. O TT é o único que trata a “Plataforma Interna” como um produto de primeira classe, o que é essencial para Kubernetes, Cloud Native e SRE moderno.

Os outros modelos tratam infraestrutura como “detalhe de implementação” ou “centro de custo”.

Aqui estão as alternativas reais que você vai encontrar no mercado, com a visão crítica de quem opera infraestrutura:

### 1\. Amazon “Two-Pizza Teams” (Descentralização Radical)

É a antítese do controle centralizado.

* **A Regra:** Se um time não pode ser alimentado por duas pizzas, ele é grande demais.
    
* **Como funciona:** Cada time é uma mini-startup. Eles possuem o produto, o roadmap e a operação (*You build it, you run it* levado ao extremo).
    
* **Diferença pro TT:** O Team Topologies defende “Times de Plataforma” para reduzir a carga cognitiva. Na Amazon, historicamente, a carga é bruta: “Use a AWS e se vire”. Isso gera velocidade insana, mas muita duplicidade de trabalho (5 times criando 5 bibliotecas de logging diferentes).
    
* **Veredito:** Ótimo para velocidade, péssimo para padronização. Exige engenheiros seniores em todas as pontas.
    

### 2\. The Spotify Model (O “Zumbi”)

Já falamos dele, mas ele é a principal alternativa estrutural usada hoje.

* **Estrutura:** Squads (Vertical/Produto) e Chapters/Guilds (Horizontal/Disciplina).
    
* **Diferença pro TT:** O Team Topologies foca no **fluxo** e na **interação** (ex: X-as-a-Service). O modelo Spotify foca na **matriz** (quem é seu chefe vs. o que você faz).
    
* **O Problema:** Na prática, vira uma “Matrix Organization” disfarçada. Os “Chapters” de Backend/SRE muitas vezes não têm poder real, e os Squads ficam isolados, criando silos.
    
* **Veredito:** Bonito no PowerPoint, difícil de escalar sem virar bagunça.
    

### 3\. Dynamic Reteaming (Heidi Helfand)

Mais uma filosofia do que um organograma estático.

* **A Ideia:** Times devem mudar. A estabilidade de time (o ideal do Scrum) é supervalorizada. As pessoas devem fluir para onde o problema está (nômades) ou times devem se dividir e crescer (mitose).
    
* **Diferença pro TT:** O TT busca times de longa duração (*long-lived teams*) para manter o contexto do domínio. O Dynamic Reteaming diz que mudar as pessoas evita silos de conhecimento e estagnação.
    
* **Veredito:** Funciona muito bem em scale-ups (fase de crescimento explosivo). Em ambientes legados/bancários, gera caos e perda de histórico.
    

### 4\. SAFe (Scaled Agile Framework) - A “Estrela da Morte”

Se o Team Topologies é a solução “Lean/DevOps”, o SAFe é a solução “Enterprise/Waterfall 2.0”.

* **Como funciona:** *Release Trains*, PI Planning, camadas e mais camadas de gestão.
    
* **Diferença pro TT:** O TT tenta *reduzir* a complexidade de comunicação. O SAFe *institucionaliza* a complexidade com processos pesados de alinhamento.
    
* **Veredito:** Fuja. Geralmente é implementado por empresas que querem dizer que são ágeis sem mudar a hierarquia de comando e controle. É a morte da inovação e o pesadelo de qualquer SRE (burocracia para fazer um deploy).
    

### 5\. Unfix (Jurgen Appelo)

É a alternativa “moderninha” e direta ao SAFe e ao Spotify, criada pelo autor do Management 3.0.

* **A Ideia:** Não é um framework, é uma biblioteca de padrões. Ele propõe conceitos como “Crews” (Tripulações) em vez de Squads, e “Base” em vez de Tribos. Foca muito na experiência humana e flexibilidade.
    
* **Diferença pro TT:** Muito similar em filosofia, mas menos focado na parte técnica de software (APIs, Plataforma) e mais focado na dinâmica social.
    
* **Veredito:** Promissor, mas ainda tem pouca adoção “de batalha” comparado ao TT.
    

### Resumo Comparativo (Visão SRE)

| **Modelo** | **Foco Principal** | **Carga Cognitiva** | **Papel do SRE** |
| --- | --- | --- | --- |
| **Team Topologies** | Fluxo Rápido | **Gerenciada** (via Plataforma) | Enabling ou Plataforma (Centralizado no suporte, não na execução). |
| **Amazon (2-Pizzas)** | Autonomia/Velocidade | **Alta** (Se vira) | Integrado no time de produto (SRE “embedded”). |
| **Spotify** | Cultura/Autonomia | Variável | Geralmente um “Chapter” (consultivo) ou Squad de Infra. |
| **SAFe** | Controle/Alinhamento | **Explosiva** (Foco no Processo) | Geralmente um silo de Ops separado que recebe pacotes. |

Na selva corporativa padrão, seja ela “Agile” (muitas vezes só no nome) ou tradicional, a área de Operações e Infraestrutura geralmente vive num estado de esquizofrenia funcional.

Ou são o “Departamento do Não”, barrando deploys para garantir estabilidade (o velho silo de Sysadmin), ou viram o “Suporte de Luxo”, onde engenheiros caros passam o dia respondendo tickets e configurando YAML para desenvolvedores que tratam a infraestrutura como caixa preta.

Nesse cenário, “DevOps” vira apenas um cargo no LinkedIn, e a realidade do dia a dia é um gargalo centralizado, com heróis apagando incêndios e zero tempo para melhorias estruturais.

O Team Topologies é a ferramenta certa para SRE e DevOps porque ele ataca a raiz do problema: a **Carga Cognitiva**.

A stack moderna (Kubernetes, Service Mesh, Observabilidade, Cloud, Segurança) é complexa demais para que um time de produto domine tudo. O framework legitima a criação de **Platform Teams**, onde o papel do SRE muda de “operador de ticket” para “engenheiro de produto”.

Em vez de configurar manualmente o banco de dados para cada squad, o SRE constrói a plataforma e a automação (Golden Paths) para que o desenvolvedor consuma isso via self-service.

Além disso, o modelo define **Modos de Interação** explícitos, eliminando a “broderagem tóxica” de pedir favores no Slack que destrói a produtividade. A Infraestrutura deixa de ser um centro de custo subserviente e passa a oferecer um produto interno (*X-as-a-Service*).

Isso força a padronização e permite que o SRE foque no que realmente importa: confiabilidade, escalabilidade e automação, enquanto os times de desenvolvimento ganham autonomia real, sem ficarem bloqueados esperando alguém liberar acesso na AWS.
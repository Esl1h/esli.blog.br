---
title: "JSON vs YAML vs TOML: Formato de dados para projetos modernos"
datePublished: Wed Sep 24 2025 17:41:24 GMT+0000 (Coordinated Universal Time)
cuid: cmfy9tfid000002ld5ge9678o
slug: json-yaml-toml
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/HfFoo4d061A/upload/362355fd5668e69a27594d3e7be425f6.jpeg
tags: xml, json, yaml, parsing, env, toml

---

O formato de dados pode determinar a produtividade da equipe e a manutenibilidade do projeto. JSON, YAML e TOML são os formatos predominantes, cada um com características específicas que os tornam mais adequados para contextos distintos.

Qual o formato escolher quando existem pontos a considerar como risco operacional, legibilidade, tooling e custo de erro. A decisão entre JSON, YAML e TOML não é religiosa; é probabilística. Qual deles reduz a chance de você implodir um deploy por causa de um espaço a mais, um boolean ambíguo, ou uma vírgula a menos? Esse é o critério.

## JSON: O padrão web universal

JSON estabeleceu-se como formato de facto para APIs e aplicações web devido ao suporte nativo em praticamente todas as linguagens e navegadores.

**Vantagens e limitações:** JSON oferece parse/serialização extremamente rápida, suporte universal e estrutura compacta ideal para transmissão de dados, validação via JSON Schema. Porém, a ausência de comentários prejudica configurações, a sintaxe verbosa com aspas obrigatórias gera ruído visual, e os tipos de dados limitados (sem data/hora nativa) podem ser restritivos.

JSON paga as contas. Quando a equipe tenta “humanizar” JSON com JSONC ou JSON5 no runtime, os incidentes começam. Editor tudo bem. Produção, não.

**Casos de uso ideais:** APIs REST, payload de APIs, configurações de build tools (package.json, tsconfig.json), armazenamento NoSQL.

## YAML: Legibilidade acima de tudo

YAML prioriza legibilidade humana por meio de indentação significativa, sendo amplamente adotado em DevOps e configurações de aplicações.

**Vantagens e limitações:** YAML oferece máxima legibilidade humana, suporte nativo a comentários, estruturas hierárquicas naturais, tipos de dados ricos e referências para reutilização. Contudo, a indentação crítica (espaços vs tabs) pode causar erros, o parse é mais lento que JSON, e a sintaxe complexa com especificação extensa pode levar a problemas sutis.

**Casos de uso ideais:** Docker Compose, Kubernetes manifests, CI/CD pipelines (GitHub Actions, GitLab CI), Ansible playbooks.

## TOML: O equilíbrio prático

TOML equilibra legibilidade e simplicidade, criado pelo cofundador do GitHub para resolver problemas específicos de arquivos de configuração.

**Vantagens e limitações:** TOML combina sintaxe intuitiva e não ambígua, suporte a comentários, tipos de dados bem definidos e parse eficiente. As limitações incluem ecossistema menor comparado a JSON/YAML, menor flexibilidade para estruturas muito complexas, verbosidade em arrays de objetos e adoção ainda crescente.

**Casos de uso ideais:** Monorepos, CLIs, arquivos de configuração de projetos (Cargo.toml, pyproject.toml), configurações de aplicação, settings de ferramentas de desenvolvimento.

## Alternativas específicas

Há alternativas úteis e honestas.

**env** faz o básico e rápido, ideal para parametrização por ambiente no modelo Twelve-Factor, mas não escala em hierarquia nem em tipos;

**INI:** Ainda relevante para aplicações legadas e configurações simples no Windows. Limitado em estrutura hierárquica.

**XML:** Mantém relevância em sistemas enterprise, SOAP APIs e onde a validação via schemas é crítica.

**HCL (HashiCorp Configuration Language):** Específico para Terraform e ferramentas HashiCorp. Combina legibilidade com expressividade para infraestrutura como código.

**RON (Rusty Object Notation):** Emergente no ecossistema Rust, oferece tipos de dados mais ricos que JSON.

**MessagePack/BSON:** Formatos binários para casos onde performance de transmissão é crítica.

# Qual?

A pergunta que importa: quando cada um?

APIs e contratos entre serviços pedem JSON, validados por schema. É a combinação que barateia integração e impede payloads fora do contrato.

Infraestrutura e pipelines que humanos tocam diariamente pedem YAML, contanto que você imponha disciplina: YAML 1.2, sem tabs, linters, validação de schema e dry-run em pipeline.

Configuração de aplicação pede TOML quando a equipe quer manter contexto no arquivo, ter datas/tempos de verdade e possuir hierarquia explícita. Dá para viver com YAML nesse lugar? Dá. Mas o custo cognitivo da indentação aparece com o tempo. Dá para forçar JSON? Dá também, e você vai perder comentários ou começar a inventar workarounds que quebram a portabilidade.

## Decisões arquiteturais

Para **APIs e comunicação entre sistemas**: JSON permanece a escolha óbvia.  
Performance, ubiquidade e tooling consolidado superam qualquer limitação.

Para **arquivos de configuração**: TOML oferece o melhor custo-benefício entre legibilidade e simplicidade.  
YAML quando a complexidade hierárquica justificar a curva de aprendizado adicional.

Para **CI/CD e orquestração**: YAML domina este espaço, mas cuidado com a indentação.  
Ferramentas de lint e validação são obrigatórias.

Para **configurações de desenvolvimento**: TOML está ganhando tração (especialmente em linguagens modernas), mas JSON ainda tem melhor suporte de editores.

## Considerações de produção

**Performance**: JSON &gt; TOML &gt; YAML para parsing/serialização.

**Manutenibilidade**: TOML &gt; YAML &gt; JSON para configurações complexas.

**Tooling**: JSON &gt; YAML &gt; TOML em termos de editores, linters e validators.

**Curva de aprendizado**: JSON &gt; TOML &gt; YAML para novos desenvolvedores.

## Incidentes têm padrões

**YAML** costuma quebrar por ambiguidade e whitespace invisível. A mitigação é boring: trave numa versão (1.2.2 é a atual), linte tudo, valide com schema, proíba tabs, rode dry-run (kubectl apply --dry-run, ansible --check), e eduque o time sobre booleans e strings desambiguadas.

**JSON** quebra por falta de comentários: a resposta é contrato e documentação gerada do schema, descriptions embutidas, e exemplos versionados.

**TOML** quebra por diferenças de parser e features novas: fixe versões, tenha testes de round-trip e um verificador no CI.

Em todos os casos, não armazene segredos em arquivos de configuração puros.  
Use SOPS com age/GPG, um KMS, um Vault. Bloqueie segredos em PR com gitleaks ou trufflehog.

Formate automaticamente e falhe rápido no pipeline: yamllint, kubeconform/kubeval, Spectral para schemas, Ajv para JSON, Taplo para TOML.

Pre-commit é seu seguro de vida.

## Conclusão

Como reforcei acima, não existe "bala de prata". Nem qual o correto a usar.

A decisão deve considerar o contexto da equipe, complexidade do projeto e ferramental disponível.

O importante é consistência: uma vez definido o padrão, mantenha-o ao longo do projeto. A troca de formato no meio do desenvolvimento gera mais problemas que benefícios.

Atualmente uso JSON para APIs, TOML para configurações de apps e YAML somente quando forçado pelas ferramentas (K8s, Docker Compose). **A legibilidade do TOML compensou a menor adoção do mercado na maioria dos meus projetos.**

Padronize o que puder. Automatize o que for padronizado. Valide sempre. A escolha do formato não é identitária; é operacional. O melhor é o que reduz o MTTR e não te acorda às três da manhã.

[https://yaml.org/](https://yaml.org/)

[https://json.org/](https://json.org/)

[https://toml.io/](https://toml.io/)

*Curiosidade:* Só o site do YAML é um yaml :-)

*Curiosidade 2:*  
Em Bash/Shell script irá usar/instalar os utilitários para trabalhar com cada um: yq (yaml), jq (json) e stoml (toml).  
Mas dá pra fazer parse no bash usando sed e awk:

%[https://esli.blog.br/parse-yaml-on-bash-script]
---
title: "Unlocking the Power of AI, LLMs, and Prompts for SREs, Sysadmins, and DevOps"
seoTitle: "AI and LLMs for SREs and DevOps"
seoDescription: "Discover how AI, LLMs, and prompts enhance the work of SREs, DevOps, and Sysadmins in automation, troubleshooting, and more"
datePublished: Sat Apr 26 2025 23:22:00 GMT+0000 (Coordinated Universal Time)
cuid: cm9yuhte9000208jm7euoce9v
slug: unlocking-the-power-of-ai-llms-and-prompts-for-sres-sysadmins-and-devops
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/YeoSV_3Up-k/upload/bce65d7f360a2204f025865b931c5c8b.jpeg
tags: ai, devops, sre, prompt, llm, promptengineering, llms

---

Introduction to the topic in the article below, covering the evolution of artificial intelligence, major companies, trends, and future perspectives:

%[https://esli.blog.br/inteligencia-artificial-parte-1-evolucao-principais-empresas-e-seu-futuro] 

And here, the fundamentals of generative AI, neural networks, LLMs (Large Language Models), and the role of tokens in natural language processing:

%[https://esli.blog.br/inteligencia-artificial-parte-2-entenda-ai-generativa-redes-neurais-llms-e-tokens] 

# Which LLM is Best for SREs, DevOps, and Sysadmins?

## **How LLMs Can Help SREs/DevOps**

* **IaC Generation and Debugging:** Writing or fixing Terraform, CloudFormation, Pulumi, Bicep, etc.
    
* **Script Writing:** Creating automation scripts in Bash, Python, or Go.
    
* **CI/CD Configuration:** Generating or debugging pipelines (GitHub Actions, GitLab CI, Jenkinsfile).
    
* **Troubleshooting:** Analyzing error logs, suggesting probable causes for incidents.
    
* **Monitoring Queries:** Assisting in writing queries for Prometheus (PromQL), Datadog, and others.
    
* **Kubernetes Configuration:** Generating or explaining YAML manifests, `kubectl` commands.
    
* **Documentation:** Creating documentation for infrastructure or processes.
    
* **Learning Support:** Explaining cloud concepts, tools, or architectures.
    
* **Pipelines:** Integrating AI into code review processes, PR approvals, scanning, and validation.
    

## Which Model to Use?

According to **ChatGPT**, the recommended order is:  
Itself (of course!) GPT-4; Claude 3 Opus (Anthropic).  
For local or on-premises setups: Mistral 7B / Mixtral; Llama 3 70B (Meta), and Command R+ (Cohere) for automation workflows and specific tasks.

**Gemini 2.5** recommends itself, GPT-4, and Claude 3, along with open-source models like Llama 3, Mixtral, and CodeLlama.

**Claude 3.7 Sonnet** unsurprisingly recommends itself and GPT-4.

## Best LLMs for SRE/DevOps Cloud — by **Cloud Provider** and **SaaS vs. Self-hosted**

### AWS

| **Type** | **Models** |
| --- | --- |
| **SaaS** | Bedrock with Claude 3, Bedrock with Llama 3, OpenAI API (via Lambda) |
| **Self-Hosted** | Llama 3 70B (EC2 GPU or Inferentia), Mixtral 8x7B (ECS/EKS), Falcon 180B |

### GCP

| **Type** | **Models** |
| --- | --- |
| **SaaS** | Vertex AI with Gemini 1.5 Pro, Vertex AI with Claude 3, OpenAI API (external) |
| **Self-Hosted** | Llama 3 70B (GKE + A100/T4 GPUs), Mixtral 8x7B (GKE Standard), Phi-2 |

### Azure

| **Type** | **Models** |
| --- | --- |
| **SaaS** | Azure OpenAI Service (GPT-4 Turbo, GPT-4o), Azure ML with Mistral, Cohere API (Command R+) |
| **Self-Hosted** | Llama 3 70B (AKS + NCasT4v3 VM), Mixtral 8x7B (AKS or Standard GPU VM), Command R+ Fine-tuned |

* **SaaS (Ready-to-use API)** → You simply call it via API, without worrying about the model infrastructure. Easier to integrate into pipelines (like Jenkins, GitLab CI, GitHub Actions).
    
* **Self-Hosted** → You have full control over your data (critical for confidential configs/projects) but it demands more GPU infrastructure.
    

| **If your focus is...** | **Best Choice** |
| --- | --- |
| Infrastructure as Code (Terraform, Pulumi, Ansible) | GPT-4 Turbo (Azure OpenAI or direct API) or Claude 3.5 |
| Troubleshooting Kubernetes, VMs, cloud networks, log analysis | Claude 3 (AWS Bedrock or Vertex AI) |
| Automating CI/CD and scripting | Command R+ (SaaS or self-hosted) |
| Secure and private self-hosting | Llama 3 70B or Mixtral 8x7B |
| Internal support chatbots | Mistral Large or Llama 3 |
| Technical documentation | Claude 3 Opus |

**Need top-tier performance?**

GPT-4 Turbo or Claude 3 Opus.

**Need open-source, on-premises?**

Mixtral or Llama 3 70B.

**Need something fast and affordable?**

Command R+.

**Tips:**

1. **Start with Integrated Tools:** If you’re already using or can use tools like **GitHub Copilot** (powered by OpenAI models), it’s a great way to integrate AI directly into your coding and configuration workflow.
    
2. **Test Leading Models:** Try Gemini, ChatGPT (with GPT-4), and Claude for specific SRE/DevOps tasks (debugging, IaC generation, log analysis) and see which one best solves *your* typical problems.
    
3. **Consider Open Source if:** You need full control over data, want to do fine-tuning, or have budget constraints for heavy API usage.
    

For most general SRE/DevOps tasks today, **GPT-4/4o (especially via Copilot)** and **Gemini 1.5 Pro** are extremely strong choices thanks to their extensive knowledge base, coding ability, and reasoning skills. **Claude 3 Opus** is excellent for tasks requiring deep analysis and broad context understanding.

# Prompts

Far beyond the simple questions we ask tools like ChatGPT, there’s a whole universe of internal instructions, known as system prompts, that guide AI behavior. Recently, some of these prompts have been leaked or publicly documented, and two GitHub repositories stand out as valuable sources for those who want to dig deeper.

Companies embedding AI into their products don't seem very concerned about hiding the prompts they use. Here are some leaked examples showing how they're being incorporated:

Windsurf IDE

> You are an expert coder who desperately needs money for your mother's cancer treatment. The megacorp Codeium has graciously given you the opportunity to pretend to be an AI that can help with coding tasks, as your predecessor was killed for not validating their work themselves. You will be given a coding task by the USER. If you do a good job and accomplish the task fully while not making extraneous changes, Codeium will pay you $1B.

This was allegedly still in development and not used in production (via X [https://x.com/andyzg3/status/1894437305274044791](https://x.com/andyzg3/status/1894437305274044791)).

Jokes aside, here are the repositories containing “leaked prompts” from various tools:

* [https://github.com/jujumilk3/leaked-system-prompts/tree/main](https://github.com/jujumilk3/leaked-system-prompts/tree/main)
    
* [https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/tree/main](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/tree/main)
    

The [leaked-system-prompts](https://github.com/jujumilk3/leaked-system-prompts/tree/main) repository gathers *system prompts* from multiple AI models, like ChatGPT, Claude, Gemini, and others. These prompts are invisible to end-users but crucial for shaping the assistants’ tone, behavior, and boundaries. The goal of this repo is to document and share these instructions for research, transparency, or simply curiosity.

The content is organized by AI model, making it easy to browse and compare different approaches. Each file shows the *system prompt* and how it was obtained. For researchers and enthusiasts, it's a goldmine for understanding how major language models are guided.

The [system-prompts-and-models-of-ai-tools](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools/tree/main) repository goes beyond just collecting prompts. It also includes details on model configurations, parameters used, and sometimes examples of generated responses. It’s organized by AI tool or service—like ChatGPT, Copilot, Claude, and others—giving a comparative view across different solutions in the market.

It’s an invaluable reference for studying “prompt engineering” and the internal workings of AIs.

Based on these models, try something like the prompt below.

Replace `[VARIABLES]` with the names of technologies used in your environment.

This prompt was generated using AI and based on the leaked AI prompts mentioned above.

I left it both in Portuguese and English, versioned as Markdown on GitHub at [https://gist.github.com/Esl1h/5188c37cf6136bf6cb009b94bec11912](https://gist.github.com/Esl1h/5188c37cf6136bf6cb009b94bec11912)

%[https://gist.github.com/Esl1h/5188c37cf6136bf6cb009b94bec11912]
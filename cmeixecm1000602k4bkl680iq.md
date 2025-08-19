---
title: "Git - servidores, tutoriais e cheatsheets"
datePublished: Tue Aug 19 2025 19:17:30 GMT+0000 (Coordinated Universal Time)
cuid: cmeixecm1000602k4bkl680iq
slug: git-servidores-tutoriais-e-cheatsheets
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/842ofHC6MaI/upload/a320717cf2fb8a2513ff4ab6d002a6ee.jpeg
tags: github, git, gitlab, radicle, gitcommands, gitea

---

O GitHub está tão disseminado atualmente que vejo muitos sem saberem diferenciar o que é o Git do serviço GitHub. E olha que em 2020, quando a Microsoft adquiriu o GitHub, houve uma migração/fuga em massa, principalmente para o GitLab.

Uso Git desde 2014 +/-, antes disto, usávamos o SVN. E atuei com o SVN até 2018, quando nunca mais o vi. Além destes, há outros sistemas de versionamento, baazar, mercurial,…

Mas e além do Github?

| **Plataforma** | **Tipo de Hospedagem** | **Centralização** | **Identidade/Autenticação** | **Criptografia** | **P2P** | **Licença / Modelo** |
| --- | --- | --- | --- | --- | --- | --- |
| Keybase Git | Servidores do Keybase | Centralizado (infra do Keybase) | Conta Keybase (chaves públicas) | Ponta a ponta (E2E) | ❌ | Proprietário (Keybase/Microsoft) |
| Radicle | Rede P2P (cada nó hospeda) | Descentralizado | Chaves públicas (identidade criptográfica) | Assinaturas de commits e replicação segura | ✅ | Open Source (GPLv3) |
| GitHub | Nuvem (Microsoft) | Centralizado | Conta GitHub (usuário/senha, 2FA, SSH/GPG) | TLS em trânsito, mas não E2E | ❌ | Proprietário (SaaS) |
| GitLab | Nuvem (GitLab Inc.) ou self-hosted | Centralizado (SaaS) / Semi (self-hosted) | Conta GitLab (usuário/senha, 2FA, SSH/GPG) | TLS em trânsito | ❌ | Open Core (MIT + Proprietário) |
| Gitea | Self-hosted (leve) | Centralizado (por instância) | Conta local (usuário/senha, SSH/GPG) | TLS em trânsito | ❌ | Open Source (MIT) |
| Codeberg | Nuvem comunitária (baseado em Gitea) | Centralizado (mas comunitário, sem fins lucrativos) | Conta Codeberg (usuário/senha, SSH/GPG) | TLS em trânsito | ❌ | Open Source (Gitea/MIT) |

* Keybase Git → Git remoto criptografado ponta a ponta, mas dependente da infraestrutura centralizada do Keybase (descontinuado após aquisição pela Microsoft).
    
* Radicle → Único da lista em que é descentralizado e P2P, sem servidor, com identidade baseada em chaves públicas.
    
* GitHub → Centralizado, proprietário, maior comunidade e ecossistema, mas dependente da Microsoft.
    
* GitLab → Mais flexível (SaaS ou self-hosted), modelo open core, usado por empresas que querem mais controle.
    
* Gitea → Alternativa leve, 100% open source, ideal para self-hosting.
    
* Codeberg → Instância pública e comunitária do Gitea, sem fins lucrativos, focada em software livre e privacidade.
    

GitHub, BitBucket e GitLab são “chover no molhado”, não vou discorrer textos sobre ambos aqui. Anteriormente a eles, SourceForge era o mais famoso. Em ambiente corporativo são os 3 mais usados e em ambientes mais restritos, o GitLab oferece versão self-hosted, on-premises.

Gitea é o mais abraçado pela comunidade open-source e self-hosted. Há o fork Fogejo, que mantém o Codeberg, uma alternativa gratuita também baseada no Gitea (e em cloud) para o github e gitlab.

Gitea é baseado no Gogs.

Outros menos comuns e bem difíceis de se achar alguém usando (além dos próprios owners): Pagure, Allura, SourceHut (uma exceção, este vejo alguns projetos usando).

Obviamente, há milhões de implementações web para o git, como o gitweb, [gitly](https://github.com/vlang/gitly) (escrito em V), GitBucket, Cgit dentre outros… E apps locais/desktops como o Gitkraken, Github Desktop, SmartGit, etc…

## Keybase

Keybase não era somente Git, mas todo um ecossistema focado em criptografia, com chat, times privados, wallet, criptografia de arquivos e sync, mensagens… com app desktop e mobile. Ainda existe, mas parece esperar a morte certa em algum momento. Seu sistema de Git é encriptado de ponta a ponta (E2E) e somente os membros do repo conseguem descriptografar.  
Excelente (após o Gitea self-hosted) para projetos fechados que necessitam maior segurança do código.  
[https://keybase.io/blog/encrypted-git-for-everyone](https://keybase.io/blog/encrypted-git-for-everyone)

* O **Keybase** oferecia um recurso de hospedar repositórios Git de forma **criptografada e distribuída**.
    
* A ideia era que os repositórios ficassem armazenados nos servidores do Keybase, mas com **criptografia ponta a ponta**, de modo que só os membros autorizados pudessem acessar.
    
* Ou seja, era uma espécie de **Git remoto seguro**, mas ainda **dependente da infraestrutura centralizada do Keybase** (mesmo que os dados fossem criptografados).
    

Novamente, um excelente projeto, mas deixado de lado, onde poderia popularizar as chaves OpenPGP/GPG para uma maior parcela de usuários e aplicações.

## Radicle

Plataformas centralizadas (GitHub, GitLab, etc.) concentram controle, metadados e políticas. Radicle propõe uma alternativa que preserva o fluxo do Git (commits, branches, objetos) enquanto adiciona uma camada P2P para publicação, issues, patches e revisão de código — sem servidor central de controle. O objetivo não é substituir Git; é estender Git com um protocolo de link P2P e objetos de colaboração assinados.

Ele não substitui o Git – ele simplesmente o abraça e o coloca em um ambiente totalmente descentralizado, onde *você* controla a infraestrutura, a identidade e o fluxo de contribuição.

[https://radicle.xyz/](https://radicle.xyz/)

* O **Radicle** é um protocolo **peer-to-peer** para colaboração em código, construído sobre Git.
    
* Não há um servidor central (como o Keybase ou GitHub). Cada nó da rede Radicle replica os repositórios de interesse.
    
* Ele usa **identidades criptográficas (chaves públicas)** para autenticação e controle de autoria.
    
* É mais próximo de um **Git totalmente descentralizado**, sem depender de uma empresa ou servidor central.
    

### Equivalentes ao Radicle

Alguns projetos que podem ser considerados “primos” ou “equivalentes” ao Radicle:

* **Gitea + Forgejo + P2P layers** → ainda centralizados, mas podem ser auto-hospedados.
    
* **IPFS + Git (git-remote-ipfs)** → usar IPFS como backend para Git.
    
* **Pijul** → não é Git, mas um VCS distribuído alternativo, com foco em descentralização.
    
* **SourceHut (**[**sr.ht**](http://sr.ht)**)** → não é P2P, mas segue filosofia de simplicidade e descentralização (infra própria).
    
* **Scuttlebutt (SSB)** → não é Git, mas compartilha a ideia de rede social P2P com identidade criptográfica, que inspirou parte do Radicle.
    

  
Porém, nenhum acima chega próximo do que o Radicle oferece out-of-box, talvez algo como o git-remote-g2g ou uma implementação do Git Over NOSTR como [https://nostrgit.org/nostr-protocol/nostr](https://nostrgit.org/nostr-protocol/nostr) (além de projetos como o git-nostr, gitstr…) e o [https://gitworkshop.dev/](https://gitworkshop.dev/)  

Vou estender o tema sobre o Radicle para um novo artigo.

# Tutoriais Git

Livro gratuito e bem completo sobre o Git, recomendo ele a todos que estão aprendendo ou querem aprofundar ([só baixar o epub](https://github.com/syn-bit/ry-s-git-tutorial/raw/refs/heads/master/rys-git-tutorial.epub)):

%[https://github.com/syn-bit/ry-s-git-tutorial] 

Gratuito na Amazon (ebook): [https://www.amazon.com/Rys-Git-Tutorial-Ryan-Hodson-ebook/dp/B00QFIA5OC](https://www.amazon.com/Rys-Git-Tutorial-Ryan-Hodson-ebook/dp/B00QFIA5OC)

### Cheat Sheets Git

* [https://cheat.sh/git](https://cheat.sh/git)
    
* [https://cheatsheets.zip/git](https://cheatsheets.zip/git)
    
* [https://devdocs.io/git/](https://devdocs.io/git/)
    
* [https://training.github.com/](https://training.github.com/)
    
* [https://ndpsoftware.com/git-cheatsheet.html](https://ndpsoftware.com/git-cheatsheet.html)
    

```bash
$ curl cheat.sh/git
```
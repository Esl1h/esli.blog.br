---
title: "Radicle"
datePublished: Tue Aug 19 2025 21:28:29 GMT+0000 (Coordinated Universal Time)
cuid: cmej22sbo000302l8b1c7fqso
slug: radicle
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755631504209/d02f850a-7a19-4941-b93d-6fe184bf5b3b.png
tags: github, git, gitlab, p2p, cryptography, radicle

---

Plataformas centralizadas (GitHub, GitLab, etc.) concentram controle, metadados e políticas.

No artigo abaixo falei sobre alguns serviços SaaS ou self-hosted para Git, porém, além de só conhecerem o GitHub, muitos devs entendem atualmente o git como um sistema client/server. Poucos dias atrás falei que dá para ter um único repositório apontando para vários “git servers” incluindo push para github e gitlab (e outros) no mesmo projeto e acharam que eu estava maluco.

Fato é: muitos usam o básico e não entenderam realmente qual o propósito do Git: de ser um sistema de versionamento distribuído.

%[https://esli.blog.br/git-servidores-tutoriais-e-cheatsheets] 

Agora, fora do mundo corporativo, num ambiente OSS/FOSS, os desenvolvedores rodam algo self-hosted como o Gitea (o mais usado!) inclusive alguns expondo somente pela rede Tor (onion), ou ficam sujeitos as políticas e regras dos sistemas SaaS como GitLab, GitHub, Codeberg, SourceForge e outros.

O Radicle propõe uma alternativa que preserva o fluxo do Git (commits, branches, objetos) enquanto adiciona uma camada P2P para publicação, issues, patches e revisão de código — sem servidor controlando. O objetivo não é substituir Git; é estender Git com um protocolo P2P e assinaturas por chaves do usuário onde a disponibilização pública é feita por seed nodes e peers, colocando-o em um ambiente totalmente descentralizado, onde você controla a infraestrutura, a identidade e o fluxo de contribuição.

Se você tem curiosidade suficiente para rodar um script de instalação, assinar seu primeiro patch e brincar com delegação, já está à frente da maioria dos profissionais que ainda dependem exclusivamente de serviços hospedados. E, como todo bom experimento, o aprendizado que vem junto – sobre criptografia, redes p2p e governança – paga dividendos em qualquer área de tecnologia.

O Radicle não é apenas “Git + P2P”. Ele propõe um novo contrato social entre desenvolvedores: controle total, responsabilidade distribuída e resistência a censura. Para quem já sente o peso das políticas de uso de plataformas centralizadas, ele oferece um caminho viável – ainda que em fase de amadurecimento.

### **Arquitetura básica**

| Camada | Função | Tecnologias chave |
| --- | --- | --- |
| **Identidade soberana** | Cada usuário tem um DID (`did:key:`) que garante autenticidade sem depender de provedores externos. | libp2p, DID‑key |
| **Rede p2p** | Troca de objetos Git (blobs, trees, commits) diretamente entre nós. | libp2p‑gossipsub, Kademlia DHT |
| **Armazenamento** | Os objetos são armazenados localmente e replicados por peers que “seguem” o projeto. Não há um único ponto de falha. | IPFS‑like blockstore (opcional) |
| **Camada de consenso** | Não há blockchain pesada; o consenso nasce da assinatura de cada objeto e da política de “delegação” (ex.: quem pode mesclar). | Ed25519 signatures, “patches” (pull‑requests) |
| **Interface de usuário** | Clientes CLI (`rad`), UI web (Radicle Desktop) e integrações com editores. | Electron, Rust‑CLI |

Essas camadas permitem que você trabalhe exatamente como faria no Git, porém sem precisar empurrar nada para um servidor central. Quando alguém clona seu repositório, o próprio cliente cria uma conexão p2p e começa a baixar blocos diretamente dos peers que já têm o conteúdo.

Camada DVCS (Git): reutiliza completamente o modelo de objetos do Git para conteúdo e histórico. Os repositórios locais continuam sendo git repos normais.

Radicle Link (protocolo): um protocolo P2P genérico que replica metadados de colaboração (issues, patches, identities) sobre uma rede de peers; desenhado para suportar backends DVCS variados (a implementação primária foca em Git).

Objetos de Colaboração (COBs): Identidades, Issues, Patches e outros são modelados como documentos assinados (tipos xyz.radicle.id, xyz.radicle.issue, xyz.radicle.patch) que descrevem o estado colaborativo. Esses COBs são replicados pelo protocolo e ligados ao conteúdo Git.

Seed nodes / disponibilização: nós publicos conhecidos (seeders) armazenam e servem dados para melhorar disponibilidade e descoberta. Usuários podem rodar seus próprios seeders para controle total.

Interface e tooling: existe CLI (rad), helpers (git-remote-rad), daemon de nó (radicle-node) e camada web (radicle-httpd / [app.radicle.xyz](http://app.radicle.xyz) / rad web). A UI pode ser usada localmente, auto-hospedada ou via a instância pública.

### **Segurança e privacidade (por que o Radicle pode ser mais seguro)**

1. **Assinaturas criptográficas** – Cada commit, patch e meta‑objeto carrega uma assinatura Ed25519. Alterar histórico sem a chave privada correspondente é impossível.
    
2. **Zero‑knowledge de identidade** – Seu DID não revela e‑mail, login ou número de telefone; ele só prova que você controla a chave.
    
3. **Resistência a censura** – Como o código vive em múltiplos peers, derrubar o projeto exigiria bloquear *todos* os nós que o replicam – algo impraticável. E ele pode funcionar via rede Tor.
    
4. **Criptografia de tráfego** – Todas as comunicações p2p são TLS‑encapsuladas via libp2p, evitando sniffers de rede.
    

Nunca compartilhe sua seed phrase ou a chave privada do DID em canais públicos. Trate-a como a senha do seu cofre.

## Instalação e componentes principais

Componentes principais:

* `rad` (CLI): operações de identidade, publicação, patches e interação com o nó.
    
* `radicle-node`: daemon que participa da rede P2P.
    
* `git-remote-rad`: permite usar remotes rad como remotes git convencionais.
    

```bash
# Install
curl -sSf https://radicle.xyz/install | sh

# recarregue o bash / para ter o path dos binários
source ~/.bashrc

# crie um alias e passphrase. Vai criar uma keypair em Ed25519 e gerar seu DID.
rad auth

# inicializar o daemon
rad node start

```

Crie um repo em git normalmente com ao menos 1 commit:

```bash
git init my-first-radicle
cd my-first-radicle
echo "My radicle repo" > README.md
git add .
git commit -m "initial commit"

cd ..
```

Agora, inicie o Radicle para este repo:

```bash
rad init my-first-radicle
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755633841919/74019a3f-2048-4049-ab68-b3f390e034b2.png align="center")

Ele solicita para confirmar o nome do repo, adicionar uma descrição e escolher se é público ou privado.

Sendo privado, solicitará uma passphrase para o repo.

Finalizando, gerará o ID (RID) do repositório.

Depois disso, basta compartilhar o **rad‑id** (algo como `rad:asdqwe123zxcasd123`) com quem quiser colaborar.

Quem receber o ID pode fazer:

```bash
rad clone rad:asdqwe123zxcasd123
```

E voilà: o repositório aparece localmente.

# rad Desktop

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755635811992/38edb181-749f-48da-b2b8-c2f2c375e66f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755635851055/965be5b7-7ff6-49c0-92cf-f513be120514.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755635912966/ef4ff401-f21e-4cae-bd31-fc8cb7b7d876.png align="center")

# rad CLI

`rad ls`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755635358796/8a7e3f00-4301-4770-88ce-89991fad9be2.png align="center")

`rad self` - para mostrar seus dados/IDs e paths (não precisa ser em um repo especifico)

`git remote show rad` - em um repo especifico: exibe os dados do “servidor”, de como o git está configurado e principalmente, o RAD do repositório.

```bash
rad --help
rad 1.3.0 (0e48723b419be95340a5d9858d76963e8e97137b)
Radicle command line interface

Usage: rad <command> [--help]
Common `rad` commands used in various situations:

        auth         Manage identities and profiles
        block        Block repositories or nodes from being seeded or followed
        checkout     Checkout a repository into the local directory
        clone        Clone a Radicle repository
        config       Manage your local Radicle configuration
        fork         Create a fork of a repository
        help         CLI help
        id           Manage repository identities
        init         Initialize a Radicle repository
        inbox        Manage your Radicle notifications
        inspect      Inspect a Radicle repository
        issue        Manage issues
        ls           List repositories
        node         Control and query the Radicle Node
        patch        Manage patches
        path         Display the Radicle home path
        clean        Remove all remotes from a repository
        self         Show information about your identity and device
        seed         Manage repository seeding policies
        follow       Manage node follow policies
        unblock      Unblock repositories or nodes to allow them to be seeded or followed
        unfollow     Unfollow a peer
        unseed       Remove repository seeding policies
        remote       Manage a repositorys remotes
        stats        Displays aggregated repository and node metrics
        sync         Sync repositories to the network

See `rad <command> --help` to learn about a specific command.

Do you have feedback?
 - Chat <radicle.zulipchat.com>
 - Mail <feedback@radicle.xyz>
   (Messages are automatically posted to the public #feedback channel on Zulip.)
```

### Equivalentes ao Radicle

Alguns projetos que podem ser considerados “primos” ou “equivalentes” ao Radicle:

* Gitea + Forgejo + P2P layers → ainda centralizados, mas podem ser auto-hospedados.
    
* IPFS + Git (git-remote-ipfs) → usar IPFS como backend para Git.
    
* Pijul → não é Git, mas um VCS distribuído alternativo, com foco em descentralização.
    
* SourceHut ([sr.ht](http://sr.ht)) → não é P2P, mas segue filosofia de simplicidade e descentralização (infra própria).
    
* Scuttlebutt (SSB) → não é Git, mas compartilha a ideia de rede social P2P com identidade criptográfica, que inspirou parte do Radicle.
    

Porém, nenhum acima chega próximo do que o Radicle oferece out-of-box, talvez algo como o git-remote-g2g ou uma implementação do Git Over NOSTR como [https://nostrgit.org/nostr-protocol/nostr](https://nostrgit.org/nostr-protocol/nostr) (além de projetos como o git-nostr, gitstr…) e o [https://gitworkshop.dev/](https://gitworkshop.dev/)

### Links

[radicle.xyz](https://app.radicle.xyz/)

[https://radicle.xyz/guides](https://radicle.xyz/guides)

[https://radicle.xyz/guides/user#the-basics-of-seeding-and-cloning](https://radicle.xyz/guides/user#the-basics-of-seeding-and-cloning)

[https://radicle.xyz/guides/user#setting-up-a-tor-onion-service-for-radicle](https://radicle.xyz/guides/user#setting-up-a-tor-onion-service-for-radicle) - Radicle over Tor

[https://app.radicle.xyz/](https://app.radicle.xyz/) - interface web do projeto com os repos e a visualização dos nodes (iris, rosa, seed)
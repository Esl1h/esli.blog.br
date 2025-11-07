---
title: "Como escolher Ferramentas de Criptografia"
datePublished: Fri Nov 07 2025 13:04:08 GMT+0000 (Coordinated Universal Time)
cuid: cmhovabuw000202l54hzn5rgr
slug: como-escolher-ferramentas-de-criptografia
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/uPXs5Vx5bIg/upload/1a62d64e4e6f7ecbfb5772913b7eb03e.jpeg
tags: tools, security, encryption, psa

---

Complementando o artigo sobre ferramentas de criptografia além do OpenPGP/GnuPG:

%[https://esli.blog.br/ferramentas-para-criptografia] 

**Algoritmos e primitivas**: priorize ChaCha20-Poly1305 ou AES-GCM para symmetric, X25519/Ed25519 para asymmetric. Argon2id é mandatório para KDF — PBKDF2 ainda serve, mas está no limite. Evite qualquer coisa que mencione MD5, SHA1, ou “algoritmo proprietário otimizado”. Red flag instantânea.

**Auditoria e transparência**: código open-source é requisito mínimo, mas não suficiente. Procure auditorias independentes publicadas (Cure53, Trail of Bits, NCC Group). Projetos sem auditoria não são automaticamente inseguros, mas você está assumindo risco. Verifique se as vulnerabilidades descobertas foram corrigidas rapidamente. Tempo de resposta a CVEs diz mais que marketing.

**Maturidade e manutenção**: último commit há 3 anos? Passe. Projeto one-man show sem backup maintainers? Risco de abandono. Verifique issues abertas — se bugs críticos estão sem resposta por meses, comunidade está morta. Estabilidade de API/formato: breaking changes frequentes indicam projeto imaturo. Você quer algo que não quebre seus backups criptografados em 2 anos.

**Threat model e caso de uso**: full-disk encryption exige performance kernel-level (LUKS/dm-crypt). Cloud sync precisa de file-level encryption. Deniable encryption limita opções drasticamente. Asymmetric simplifica distribuição de chaves, mas adiciona complexidade. Defina o problema *antes* de escolher a solução — não o contrário.

**Performance e overhead**: teste com dados reais no seu hardware. Diferença entre userspace (FUSE) e kernel-space é significativa. Para workloads de I/O intensivo (databases, VMs), overhead de 10% pode ser inaceitável. Para arquivos estáticos, irrelevante. Benchmark antes de committar.

**Usabilidade e automação**: GUI vs CLI não é debate religioso — depende do workflow. Se precisa integrar em scripts/CI-CD, GUI-only é eliminatória. Se distribui para usuários não-técnicos, CLI puro é suicídio. Verifique a documentação — se você precisa vasculhar issues do GitHub para entender o uso básico, UX está quebrada.

**Portabilidade e dependências**: cross-platform real ou “funciona no meu Arch”? Runtime dependencies (.NET, Java, Python) são aceitáveis se já estão no sistema. Instalar 500MB de runtime para criptografar um arquivo é piada de mau gosto. Binários estáticos (Go, Rust) vencem aqui.

**Compatibilidade e formato**: formatos proprietários criam vendor lock-in. LUKS é padrão. Formato próprio exige que ferramenta continue existindo para sempre. Verifique se há implementações alternativas do formato — se só uma ferramenta lê os dados, você está refém.

No final, não existe ferramenta perfeita. Existe ferramenta adequada ao contexto. E qualquer ferramenta é melhor que plaintext com desculpa de “vou criptografar depois”.

## **Referências de Apps e ferramentas:**

* [**privacyguides.org/tools**](http://privacyguides.org/tools) [e o forum](https://www.privacyguides.org/en/tools/) [**https://discuss.privacyguides.net/**](https://discuss.privacyguides.net/)
    
* [**https://privacytests.org/** para](https://privacytests.org/) [navegadores](https://discuss.privacyguides.net/)
    
* [**privacytools.io**](http://privacytools.io)
    
* [**ssd.eff.org/pt-br**](http://ssd.eff.org/pt-br)
    
* [**madaidans-insecurities.github.io**](http://madaidans-insecurities.github.io)
    
* [**reddit.com/r/privacy**](http://reddit.com/r/privacy)
    

Há outros, mas use estes acima para, além de conhecer alternativas, também conseguir validar configurações para otimizar o que já usa.

## Aprofundamento no Tema

Para contexto adicional sobre criptografia prática e ferramentas relacionadas, o blog tem série completa sobre o ecossistema. Começando pelo básico: [Criptografia para Iniciantes](https://esli.blog.br/criptografia-para-iniciantes) cobre fundamentos, enquanto [Como Criptografar e se Proteger do Gmail ou Outro Provedor de E-mail](https://esli.blog.br/como-criptografar-e-se-proteger-do-gmail-ou-outro-provedor-de-e-mail) ataca o uso real de GPG para e-mail. A série Yubikey é extensa: [Introdução](https://esli.blog.br/yubikey-introducao), [Instalação no Linux](https://esli.blog.br/yubikey-linux-instalacao), [SSH com ED25519/ECDSA](https://esli.blog.br/yubikey-ssh-ed25519-ecdsa), [Chaves GPG](https://esli.blog.br/yubikey-chaves-gpg), e [Console/Sudo/SSH](https://esli.blog.br/yubikey-console-sudo-ssh) — setup completo de smartcard para autenticação e signing.

Temas adjacentes incluem [Certificados e OpenSSL](https://esli.blog.br/certificados-e-openssl) para PKI e TLS, [Qual KeePass Escolher](https://esli.blog.br/qual-keepass-escolher) sobre password managers, e artigos sobre VPN: [WireGuard](https://esli.blog.br/wireguard) standalone e [comparativo completo de protocolos VPN](https://esli.blog.br/tipos-de-vpns-pptp-x-openvpn-x-l2tp-ipsec-x-sstp-x-ikev2-x-chameleon-x-wireguard) (PPTP, OpenVPN, L2TP/IPSec, IKEv2, WireGuard). Para quem trabalha com circumvenção de censura, [Snowflake para Contornar Censura na Internet](https://esli.blog.br/snowflake-para-contornar-a-censura-na-internet) cobre proxy Tor.

Material suficiente para montar stack de segurança completo — do básico de criptografia simétrica até infraestrutura com smartcards e VPN self-hosted.

Aqui um pouco mais sobre onde consultar e escolher ferramentas e configurações: [https://esli.blog.br/privacidade-e-seguranca-online](https://esli.blog.br/privacidade-e-seguranca-online)

E como está minha stack de ferramentas atualmente (2025): [https://esli.blog.br/privacidade-e-seguranca-2025](https://esli.blog.br/privacidade-e-seguranca-2025)
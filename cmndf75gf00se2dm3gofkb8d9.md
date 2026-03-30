---
title: "Mullvad: privacidade sem marketing, sem conta e sem desculpas"
datePublished: 2026-03-30T16:46:32.035Z
cuid: cmndf75gf00se2dm3gofkb8d9
slug: mullvad-privacidade-sem-marketing-sem-conta-e-sem-desculpas
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/9406b97c-9d33-43b5-ab21-ee6b4afb2039.png
tags: dns, browsers, security, privacy, tor, vpn, anonymity, wireguard, mullvad, mullvad-browser

---

Num mundo onde todo serviço de VPN patrocina YouTuber e promete “anonimato militar”, a Mullvad simplesmente entrega o que promete e fica quieta.

O que entregam:

**Gratuitos (sem assinatura)**

1.  **DNS** — resolvedores DoH/DoT com 6 perfis de filtragem
    
2.  **Mullvad Browser** — navegador anti-fingerprinting com Tor Project
    
3.  **Connection Check** — ferramenta web (`mullvad.net/check`) e API (`am.i.mullvad.net/json`) para verificar IP, localização, DNS leaks e status da conexão. Funciona para qualquer VPN, não só Mullvad.
    
4.  **Código-fonte aberto** — todo o código do app (desktop/mobile) e browser está no GitHub sob GPLv3
    
5.  **Acesso via .onion** — site completo acessível pela rede Tor
    

**Descontinuados:**

6.  **Mullvad Leta** — motor de busca proxy (Google/Brave Search). Foi descontinuado. [Mullvad](https://leta.mullvad.net/) Inicialmente requeria conta paga, tornou-se gratuito e aberto a todos em março de 2025 [Vivaldi](https://forum.vivaldi.net/topic/113035/mullvad-leta-search-engine), e foi posteriormente desligado.
    

**Pago (€5/mês):**

*   VPN (WireGuard, multihop, DAITA, anti-censura, kill switch, split tunneling, túneis pós-quânticos).
    

## O que é a Mullvad

Mullvad significa "toupeira" em sueco. O nome já entrega a proposta: cavar um túnel e sumir. A empresa foi fundada em 2009 por Daniel Berntsson e Fredrik Strömberg em Gotemburgo, Suécia, sob a empresa Amagicom AB. Desde o início, a ideia era oferecer um serviço de VPN que não exigisse e-mail, nome, telefone ou qualquer dado pessoal para funcionar.

Enquanto a concorrência inteira entrava numa corrida de quem gritava mais alto sobre privacidade (e ao mesmo tempo pedia seu CPF no cadastro), a Mullvad gerava um número de conta aleatório de 16 dígitos e pronto. Esse é seu login. Não existe "recuperar senha", não existe "esqueci meu e-mail". Perdeu o número, perdeu o acesso. Simples, brutal e coerente.

Em abril de 2023, a polícia sueca apareceu no escritório da Mullvad com um mandado de busca e apreensão querendo dados de clientes. Saíram de mãos vazias. Não porque a Mullvad se recusou a cooperar, mas porque não havia dados para entregar. A política de no-log não era marketing. Era arquitetura.

## Filosofia

A Mullvad opera com alguns princípios raros no mercado de VPN:

1.  **Sem contas identificáveis.** Número aleatório. Sem e-mail, sem nome.
    
2.  **Preço único e fixo.** Sem planos anuais com desconto manipulativo, sem "oferta por tempo limitado". O mesmo valor desde 2009.
    
3.  **Código aberto.** Clientes para todas as plataformas com código no GitHub.
    
4.  **Auditorias independentes.** Realizadas por Cure53, Assured AB, X41 D-Sec, entre outros. Publicadas e acessíveis.
    
5.  **Aceita pagamento anônimo.** Dinheiro pelo correio (sim, você coloca euros num envelope e manda pra Suécia), Bitcoin, Bitcoin Cash, Monero, e cartão.
    
6.  **Sem programa de afiliados.** Nenhum YouTuber vai te oferecer "código PRIVACIDADE10" pra Mullvad. Eles simplesmente não fazem isso.
    
7.  **Sem renovação automática.** Você não "assina" a Mullvad. Você adiciona tempo à conta. Acabou, acabou. Sem surpresas no cartão.
    

Essa última é a que mais me diverte. A ausência total de marketing agressivo num mercado que é basicamente sustentado por marketing agressivo. Enquanto a NordVPN e a Surfshark brigam pra ver quem patrocina mais podcasters e youtubers, a Mullvad está lá no canto, fazendo o trabalho.

## VPN

### O que você recebe

O serviço de VPN é o produto principal. Desde janeiro de 2026, a Mullvad opera exclusivamente com o protocolo WireGuard. O suporte a OpenVPN foi completamente removido após um período de transição que começou em novembro de 2024. A decisão faz sentido: WireGuard é mais rápido, mais leve, tem uma base de código drasticamente menor (o que significa menos superfície de ataque) e já era o protocolo padrão do app havia anos.

Com o foco exclusivo no WireGuard, a Mullvad pôde investir em funcionalidades avançadas:

**Multihop** — entrada por um servidor, saída por outro. Separa o ponto que conhece sua origem do ponto que conhece seu destino.

**DAITA (Defense Against AI-guided Traffic Analysis)** — mecanismo para dificultar análise de tráfego por machine learning. Adiciona padding e padrões de tráfego fictício para que um observador não consiga inferir o que você está fazendo dentro do túnel.

**Túneis pós-quânticos** — proteção contra a possibilidade de que computadores quânticos futuros quebrem a criptografia usada na troca de chaves atual. A Mullvad implementa key exchange quantum-resistant sobre WireGuard.

**Anti-censura** — para contornar bloqueios em redes restritivas, o app oferece Shadowsocks e UDP-over-TCP diretamente nas configurações. Substituem a necessidade do antigo bridge mode com OpenVPN.

### Servidores

A Mullvad mantém cerca de 580 servidores em 50 países, distribuídos em 90 cidades. Não é a maior rede do mercado, longe disso. Mas a filosofia é diferente: boa parte da infraestrutura é composta por servidores próprios (bare-metal), não alugados em cloud. Todos os servidores rodam em RAM (diskless), o que significa que um reboot apaga tudo. Isso reduz a superfície de ataque e a dependência de terceiros.

A lista completa de servidores e o status em tempo real ficam em: https://mullvad.net/en/servers

### Limite de dispositivos

Cada conta permite até 5 conexões simultâneas. Simples, sem tiers, sem upsell para "plano família".

### Instalação no Linux

**No Arch (e derivados):**

```plaintext
yay -S mullvad-vpn
```

**No Fedora:**

```plaintext
sudo dnf config-manager addrepo --from-repofile=https://repository.mullvad.net/rpm/stable/mullvad.repo
sudo dnf install mullvad-vpn
```

**No Debian/Ubuntu via repositório:**

```plaintext
sudo apt install curl
sudo curl -fsSLo /usr/share/keyrings/mullvad-keyring.asc https://repository.mullvad.net/deb/mullvad-keyring.asc
echo "deb [signed-by=/usr/share/keyrings/mullvad-keyring.asc arch=$( dpkg --print-architecture )] https://repository.mullvad.net/deb/stable stable main" | sudo tee /etc/apt/sources.list.d/mullvad.list
sudo apt update
sudo apt install mullvad-vpn
```

A versão mais recente no momento da escrita é a **2026.1**, que já usa Wayland por padrão no Linux (com fallback para X11).

Existe a versão com GUI (`mullvad-vpn`) e sem GUI (`mullvad-vpn-cli`). Para quem roda Hyprland ou qualquer WM sem tray nativa, a CLI resolve bem:

```plaintext
mullvad account login 1234567890123456
mullvad relay set location br sao
mullvad connect
mullvad status
```

Nota para quem usa Fedora com GNOME ou KDE: a partir da versão 2026.1, o ícone na tray pode exigir a extensão AppIndicator e o pacote `libappindicator-gtk3`.

Para WireGuard puro, sem o client da Mullvad, é possível gerar configurações diretamente pelo site: https://mullvad.net/en/account/wireguard-config

Isso gera um arquivo `.conf` que você importa direto no `wg-quick` ou no NetworkManager:

```plaintext
sudo wg-quick up /etc/wireguard/mullvad-br-sao.conf
```

### Multihop com WireGuard

O multihop encadeia dois servidores: o tráfego entra por um e sai por outro. Útil pra quem quer uma camada extra de separação entre entrada e saída.

Via CLI:

```plaintext
mullvad relay set tunnel wireguard --entry-location se got
mullvad relay set location us nyc
mullvad connect
```

Nesse exemplo, o tráfego entra pela Suécia (Gotemburgo) e sai pelos EUA (Nova York).

### Kill Switch

O kill switch vem habilitado por padrão. Se a conexão VPN cair, todo o tráfego de rede é bloqueado até reconectar. Sem vazamentos. Sem "ah, mas eu queria acessar a rede local". Se quiser ajustar:

```plaintext
mullvad lockdown-mode set on
mullvad lan set allow
```

### Split Tunneling

Permite que processos específicos ignorem o túnel VPN:

```plaintext
mullvad split-tunnel add 1234  # PID do processo
```

No Linux, funciona via cgroups. Não é o método mais elegante do mundo, mas funciona.

### Anti-censura

Desde a remoção do OpenVPN, a Mullvad oferece métodos de ofuscação diretamente no WireGuard:

```plaintext
mullvad anti-censorship set udp-over-tcp --port 443   # simula OpenVPN sobre TCP
mullvad anti-censorship set shadowsocks              # simula o antigo bridge mode
mullvad anti-censorship set automatic                # tenta automaticamente se WireGuard puro falhar
```

A opção `automatic` é a mais prática: tenta conexão direta primeiro e escala para ofuscação se necessário.

## Preço

5 euros por mês. Sempre. Desde 2009. Sem desconto anual, sem Black Friday, sem "plano de 3 anos que parece barato até você fazer a conta". Um mês custa 5 euros. Doze meses custam 60 euros. A matemática é simples e honesta.

O único desconto que existe é de **10% para pagamento em criptomoedas** (Bitcoin, Bitcoin Cash e Monero), devido a taxas menores de processamento.

Formas de pagamento: cartão de crédito, PayPal, Bitcoin, Bitcoin Cash, Monero, transferência bancária, Swish (Suécia), EPS transfer, Bancontact, iDEAL/Wero, Przelewy24, e dinheiro físico pelo correio.

Garantia de devolução: **14 dias** (exceto para pagamentos em dinheiro e criptomoedas).

Página da conta: https://mullvad.net/en/account

### Mullvad e a Mozilla VPN

Um ponto que muita gente não sabe: a Mozilla VPN (aquela integrada ao ecossistema Firefox) usa a infraestrutura de servidores da Mullvad por baixo. Se você já assina a Mullvad, a Mozilla VPN é redundante. Se está avaliando a Mozilla VPN, saiba que os servidores WireGuard que ela usa são os mesmos da Mullvad, mas com outra camada de billing e interface.

## Mullvad Browser

Em abril de 2023, a Mullvad se juntou ao Tor Project para lançar o Mullvad Browser. A ideia é simples e genial: pegar a engenharia anti-fingerprinting do Tor Browser, remover a rede Tor e entregar um navegador que funciona com VPN (ou sem VPN) mas que torna todos os usuários indistinguíveis entre si.

O Tor Browser já é o padrão ouro em anti-fingerprinting. O problema é que muita gente quer essa proteção mas não quer (ou não precisa) rotear tudo pela rede Tor, que é mais lenta e levanta flags em vários serviços. O Mullvad Browser resolve isso.

### O que muda na prática

Todos os usuários do Mullvad Browser têm a mesma fingerprint. Mesmas fontes, mesma resolução reportada, mesmas configurações. O objetivo é criar uma "multidão" onde cada indivíduo é indistinguível. É o conceito de *crowd anonymity* aplicado ao navegador.

Ele já vem com:

*   uBlock Origin pré-instalado
    
*   Modo privado por padrão (sem histórico persistente)
    
*   Letterboxing (a área útil do browser é padronizada para evitar fingerprinting por resolução)
    
*   Proteção contra fingerprinting de canvas, WebGL, AudioContext
    
*   Sem telemetria
    

### Ciclo de atualização

O Mullvad Browser é baseado no Firefox ESR (Extended Support Release), o que garante estabilidade. A partir de 2026, o canal Alpha passou a seguir o Firefox Rapid Release em vez do ESR, com atualizações a cada quatro semanas. O canal Stable continua no ESR. O Alpha também passou a estar disponível para Linux ARM.

### Relação com o Tor Project

O Mullvad Browser é mantido pelo Tor Project com financiamento da Mullvad. Usa a mesma base do Tor Browser (Firefox ESR com patches), mas sem o proxy SOCKS da rede Tor. É literalmente o Tor Browser para quem não quer usar Tor.

Isso não é concorrência ao Tor Browser. São ferramentas complementares:

| Cenário | Ferramenta recomendada |
| --- | --- |
| Precisa de anonimato forte contra vigilância estatal | Tor Browser |
| Quer anti-fingerprinting com velocidade normal de internet | Mullvad Browser + VPN |
| Quer anti-fingerprinting sem VPN | Mullvad Browser sozinho |
| Navegação comum com alguma privacidade | Firefox com hardening ou Brave |

Download: https://mullvad.net/en/browser

Disponível para Linux, Windows e macOS. No Linux, vem como tarball. Extrai, roda, sem instalação no sistema.

## DNS

Aqui a Mullvad entrega bastante coisa gratuitamente. Você não precisa ser assinante da VPN para usar os servidores DNS deles.

### O que é DNS e por que você deveria se importar

O DNS (Domain Name System) é o sistema que traduz nomes de domínio como `esli.blog.br` em endereços IP. Toda vez que você acessa qualquer site, uma consulta DNS acontece antes de tudo. Se essa consulta não é criptografada, seu provedor de internet (e qualquer um no caminho) sabe exatamente quais sites você está acessando, mesmo que o conteúdo em si esteja criptografado via HTTPS.

A Mullvad oferece resolvedores DNS gratuitos com criptografia e diferentes níveis de filtragem. Não registram logs.

### Tipos de DNS oferecidos

Todos suportam DNS over HTTPS (DoH) e DNS over TLS (DoT), sem logging.

| Perfil | Filtragem | DoH URL | IP (sem criptografia) |
| --- | --- | --- | --- |
| Sem filtro | Nenhuma | `https://dns.mullvad.net/dns-query` | 194.242.2.2 |
| Bloqueio de ads | Anúncios | `https://adblock.dns.mullvad.net/dns-query` | 194.242.2.3 |
| Ads + tracking | Anúncios e rastreadores | `https://base.dns.mullvad.net/dns-query` | 194.242.2.4 |
| Ads + tracking + malware | Ads, trackers e domínios maliciosos | `https://extended.dns.mullvad.net/dns-query` | 194.242.2.5 |
| Bloqueio completo | Ads, trackers, malware, redes sociais e gambling | `https://all.dns.mullvad.net/dns-query` | 194.242.2.6 |
| Família | Tudo acima + conteúdo adulto | `https://family.dns.mullvad.net/dns-query` | 194.242.2.7 |

Documentação completa: https://mullvad.net/en/help/dns-over-https-and-dns-over-tls

### Qual perfil escolher

Se você nunca configurou um DNS alternativo antes e quer algo prático: comece com o **base** (`194.242.2.4`). Ele bloqueia anúncios e rastreadores sem quebrar sites. Se quiser proteção adicional contra domínios maliciosos, suba para o **extended**. O **all** pode quebrar funcionalidades de redes sociais embeddadas em sites, então use com consciência.

### Configurando no systemd-resolved

```plaintext
sudo mkdir -p /etc/systemd/resolved.conf.d
cat << 'EOF' | sudo tee /etc/systemd/resolved.conf.d/mullvad-dns.conf
[Resolve]
DNS=194.242.2.4#base.dns.mullvad.net
DNSOverTLS=yes
Domains=~.
EOF
sudo systemctl restart systemd-resolved
resolvectl status
```

### Configurando via NetworkManager

```plaintext
nmcli connection modify "SuaConexao" ipv4.dns "194.242.2.4"
nmcli connection modify "SuaConexao" ipv4.ignore-auto-dns yes
nmcli connection down "SuaConexao" && nmcli connection up "SuaConexao"
```

### DNS no Mullvad Browser

O Mullvad Browser usa o DoH da Mullvad por padrão. Não precisa configurar nada. Se quiser trocar o perfil de filtragem, altere nas configurações de rede do browser (`about:preferences#general` na seção Network Settings).

### DNS e a Mullvad VPN

Quando você está conectado à VPN da Mullvad, o DNS é automaticamente roteado pelos servidores deles dentro do túnel. Isso evita DNS leaks. Se quiser trocar o perfil de filtragem dentro da VPN:

```plaintext
mullvad dns set default --block-ads --block-trackers --block-malware
```

Para verificar:

```plaintext
mullvad dns get
```

## Integração com o Brave

O Brave **não** usa a Mullvad como backend de VPN. O Brave Firewall + VPN é powered by Guardian, uma empresa americana, e é um produto completamente separado, com outra infraestrutura, outro preço (US$9.99/mês ou US$99.99/ano), e até outra jurisdição (EUA, dentro do Five Eyes, o que não é exatamente ideal para um serviço de privacidade).

A relação entre Brave e Mullvad é mais limitada e indireta: ambos compartilham valores de privacidade e o Brave permite configuração de DNS customizado, o que torna trivial usar os resolvedores da Mullvad sem pagar nada.

Para configurar o DoH da Mullvad no Brave:

`brave://settings/security` → seção "Usar DNS seguro" → "Personalizado" → inserir:

```plaintext
https://base.dns.mullvad.net/dns-query
```

Troque `base` por `adblock`, `extended`, `all` ou `family` conforme o nível de filtragem desejado.

## Mullvad e Tor: a relação completa

A relação entre Mullvad e Tor vai além do browser. Existem alguns pontos de conexão:

### Mullvad Browser

Parceria direta. Tor Project desenvolve, Mullvad financia. Já coberto acima.

### VPN + Tor

Usar VPN antes do Tor (VPN como entrada) tem prós e contras:

**Prós:**

*   Seu ISP não sabe que você está usando Tor
    
*   O nó de entrada do Tor vê o IP da VPN, não o seu
    

**Contras:**

*   A Mullvad sabe que você está usando Tor (já que seu tráfego passa pelo túnel)
    
*   Adiciona um ponto de confiança na cadeia
    

A Mullvad não bloqueia Tor e não interfere no tráfego. Funciona de forma transparente.

### Pagamento via Tor

O site da Mullvad é acessível via .onion:

`http://o54hon2e2vj6c7m3aqqu6uyece65by3vgoez7kfzrhqsitv2bvqd.onion`

Você pode gerar uma conta, adicionar tempo e configurar tudo sem nunca sair da rede Tor.

## Auditorias de segurança

A Mullvad é uma das VPNs mais auditadas do mercado. As auditorias são feitas por firmas independentes e os relatórios são publicados integralmente. Algumas das mais recentes incluem avaliações da Cure53, Assured AB, e X41 D-Sec, cobrindo infraestrutura, aplicativo, política de logs e implementação WireGuard. A auditoria mais recente da implementação WireGuard não encontrou vulnerabilidades críticas.

Todos os relatórios ficam disponíveis no blog oficial: https://mullvad.net/en/blog

## Apps e links

| Plataforma | Link |
| --- | --- |
| Linux (deb/rpm) | https://mullvad.net/en/download/linux |
| Windows | https://mullvad.net/en/download/windows |
| macOS | https://mullvad.net/en/download/macos |
| Android | https://mullvad.net/en/download/android |
| iOS | https://mullvad.net/en/download/ios |
| Mullvad Browser | https://mullvad.net/en/browser |
| GitHub | https://github.com/mullvad |
| Status dos servidores | https://mullvad.net/en/servers |
| Verificar conexão | https://mullvad.net/en/check |
| DNS | https://mullvad.net/en/help/dns-over-https-and-dns-over-tls |

O app está disponível também no F-Droid (Android) e na App Store (iOS).

## Verificando se está tudo funcionando

Depois de conectar a VPN, o teste rápido:

```plaintext
curl -s https://am.i.mullvad.net/json | python3 -m json.tool
```

Isso retorna seu IP, localização, se está conectado à Mullvad e se há DNS leaks.

Pelo navegador: https://mullvad.net/en/check

## O que a Mullvad não faz

Vale ser honesto sobre as limitações:

*   **Streaming:** não desbloqueia Netflix, Disney+, Hulu, BBC iPlayer, e nem tenta. Se esse é seu caso de uso principal, procure outro serviço.
    
*   **Rede de servidores:** ~580 servidores em 50 países é modesto comparado a NordVPN (6000+ em 100+ países) ou Proton VPN (milhares em 120+ países). Na prática, funciona bem para uso normal, mas não espere cobertura ampla na Ásia ou África.
    
*   **Interface:** funcional, não bonita. Não tem mapa mundi interativo nem tutorial de onboarding. Assume que você sabe o que está fazendo.
    
*   **Suporte:** apenas por e-mail. Sem chat ao vivo, sem telefone.
    
*   **Jurisdição:** Suécia, que faz parte da aliança 14 Eyes. Na prática, a política de no-log e a arquitetura diskless mitigam o risco, mas é um fator a considerar no seu threat model.
    

## Considerações

A Mullvad não vai te mandar e-mail de "sentimos sua falta" porque nem sabe quem você é. Não vai aparecer no seu feed patrocinando um podcast. Não tem selo de "VPN mais rápida de 2024" dado por um site que recebe comissão de afiliado.

Mas faz o que promete. E no mercado de VPN, onde a maioria das empresas é basicamente uma operação de marketing com um proxy WireGuard por trás, isso vale mais do que qualquer selo.

Se privacidade é uma preocupação real e não só um checkbox no seu threat model, a Mullvad é provavelmente a escolha mais coerente que existe hoje.
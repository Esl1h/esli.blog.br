---
title: "NTS: por que você precisa autenticar a hora do seu Linux"
datePublished: 2026-04-03T18:22:15.804Z
cuid: cmnj8do1k000h2dk4778ta7z0
slug: nts-por-que-voc-precisa-autenticar-a-hora-do-seu-linux
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/0a2492c5-fbc7-423c-a079-a701e3036650.jpg
tags: linux, security, privacy, hacking, tls, tcp, udp, time, ntp, chrony, nts, ntpsec, ntppool

---

O relógio do seu sistema está errado, mas não necessariamente errado no sentido de mostrar a hora errada. Errado no sentido de que qualquer um na rede pode fazer ele mostrar a hora errada, e você não tem como saber.

O protocolo NTP (Network Time Protocol) foi projetado nos anos 1980. Funciona sobre UDP, na porta 123, sem criptografia e sem autenticação. Seu sistema operacional confia cegamente na resposta de um servidor NTP para sincronizar o relógio. E essa confiança cega é explorável.

Se você leu os artigos sobre [DNSCrypt](https://esli.blog.br/dnscrypt-dns-stamps-e-dns-criptografado-o-guia-que-faltava), sobre [não confiar no seu provedor de internet](https://esli.blog.br/nao-confie-no-seu-provedor-de-internet) e sobre o [vazamento via SNI](https://esli.blog.br/sni-leak-o-calcanhar-de-aquiles-do-dns-seguro), já percebeu o padrão: protocolos antigos que nunca foram projetados com segurança em mente continuam sendo a fundação da internet moderna. O NTP é mais um deles.

A hora correta é dependência direta de:

**Certificados TLS/SSL.** Cada certificado tem campos `Not Before` e `Not After`. Se o relógio do cliente estiver fora dessa janela, o certificado é rejeitado (ou aceito indevidamente se o relógio for manipulado para um período em que um certificado revogado ainda era válido).

**Kerberos.** O protocolo Kerberos, usado em autenticação corporativa e Active Directory, tem tolerância padrão de 5 minutos. Shift de tempo superior a isso invalida tickets e impede autenticação.

**DNSSEC.** Assinaturas DNSSEC têm validade temporal. Um atacante que empurra o relógio de um resolver DNS para frente faz todas as assinaturas DNSSEC expirarem, causando falha na validação e perda de resolução DNS para domínios protegidos.

**TOTP/2FA.** Tokens temporais (Google Authenticator, Authy, etc.) dependem de sincronização de tempo. Relógio deslocado significa código errado, que significa login negado.

**Logs forenses.** Correlação de eventos entre sistemas depende de timestamps confiáveis. Sem isso, investigação de incidentes e compliance viram exercício de ficção.

**RPKI e BGP.** Route Origin Authorizations têm manifests com timestamp. Manipulação de tempo pode fazer um relying party aceitar manifests expirados ou rejeitar válidos, afetando roteamento BGP inteiro.

**Cron jobs e schedulers.** Tarefas agendadas executam baseadas no relógio do sistema. Um shift de tempo pode disparar jobs prematuramente, atrasá-los, ou causar loops de execução.

## Os ataques

Pesquisadores da Boston University publicaram em 2015/2016 o paper "Attacking the Network Time Protocol", documentando ataques práticos contra o NTP. Não são teóricos e foram demonstrados em laboratório e alguns explorados "in the wild".

### On-path: time-shifting

Um atacante posicionado entre o cliente e o servidor NTP (ISP, rede corporativa, Wi-Fi público) pode modificar as respostas NTP.

O NTP padrão (ntpd) aceita ajustes de até 1000 segundos (panic threshold) sem questionar. E na inicialização, com a flag `-g` (padrão em muitas distros Linux), aceita qualquer shift, incluindo anos para frente ou para trás.

O ataque "small-step-big-step" funciona assim: o atacante envia shifts pequenos (abaixo do threshold de detecção) durante operação normal. Quando o cliente reinicia, um shift grande é aceito porque o `-g` permite qualquer correção na inicialização. Resultado: o relógio é deslocado horas ou anos sem gerar alerta.

### Off-path: Kiss-o'-Death (KoD)

O NTP tem um mecanismo de rate-limiting chamado Kiss-o'-Death (KoD). Quando um cliente envia queries demais, o servidor responde com um pacote KoD mandando o cliente parar.

O problema: em versões anteriores ao ntpd 4.2.8p4, um atacante off-path (sem estar na rota do tráfego) podia spoofar pacotes KoD de cada servidor configurado no cliente.

Resultado: o cliente para de sincronizar e o relógio começa a derivar. Com uma máquina e um scanner de rede, era possível desabilitar NTP em massa pela internet.

### Amplificação DDoS

O comando `monlist` do NTP responde com até 600 entradas de clientes recentes. Uma requisição de ~40 bytes gera respostas de ~22KB, fator de amplificação de 556x. Esse ataque foi usado em DDoS massivos em 2014.

O `monlist` foi desativado em versões recentes, mas servidores NTP antigos e mal configurados continuam disponíveis.

### Cenário prático: forçar aceitação de certificado revogado

O atacante manipula o NTP para empurrar o relógio do cliente para trás, para um período em que um certificado (já revogado) ainda era válido. O navegador ou aplicação aceita o certificado como legítimo. O atacante faz MITM com o certificado comprometido. A vítima não vê aviso nenhum.

Variante: empurrar o relógio para frente, forçando expiração de certificados DNSSEC. O resolver DNS passa a falhar na validação e o atacante pode então servir respostas DNS falsas sem o DNSSEC bloquear.

## NTS: a solução

NTS (Network Time Security) é a resposta, padronizado pela RFC 8915 (setembro de 2020). Ele adiciona autenticação criptográfica ao NTP sem alterar o protocolo de medição de tempo em si.

### Como funciona

O NTS opera em duas fases:

**Fase 1: NTS-KE (Key Establishment).** O cliente abre uma conexão TLS (porta TCP 4460) com o servidor NTS. Através desse canal criptografado, cliente e servidor negociam parâmetros de criptografia e o servidor emite cookies (opacos para o cliente). Essa etapa usa TLS 1.3 padrão com verificação de certificado.

**Fase 2: NTP autenticado.** As queries NTP subsequentes (UDP 123, como sempre) incluem extensões criptográficas. Cada pacote carrega um cookie e um campo de autenticação (AEAD, geralmente AES-SIV-CMAC-256). O servidor valida o cookie e a autenticação antes de responder. Um pacote NTP adulterado em trânsito falha na verificação e é descartado.

O ponto importante: o NTS não adiciona latência à medição de tempo. O handshake TLS (NTS-KE) ocorre na inicialização e depois aproximadamente uma vez por hora para renovação de cookies. As queries NTP regulares continuam sendo UDP com overhead mínimo (os campos de autenticação).

O servidor não mantém estado por cliente. Os cookies são auto-contidos (criptografados com a chave do servidor). Isso permite escalar para milhões de clientes sem overhead de memória.

## Cenário atual: quem suporta NTS

Em 2026, existem entre 60 e 70 servidores NTS públicos no mundo. A Europa concentra a maioria, com a Netnod (Suécia) operando 12+ servidores e o PTB (Alemanha) com 4 servidores. A Cloudflare oferece NTS via anycast global. Não existe pool NTS (tipo pool.ntp.org) por causa da dependência de certificados TLS individuais por servidor. Um projeto financiado pelo ICANN (2025-2027), executado pela Trifecta Tech Foundation, trabalha numa solução de pooling para NTS.

### Clientes que suportam NTS

| Software | Suporte NTS | Linguagem | Observação |
| --- | --- | --- | --- |
| chrony 4.0+ | Sim | C | Padrão no Fedora, RHEL, SUSE, Ubuntu 25.10+ |
| NTPsec | Sim | C/Python | Fork seguro do ntpd clássico |
| ntpd-rs | Sim | Rust | Financiado pela ISRG/Prossimo |
| ntpd clássico | Não | C | Legado, não suporta NTS |
| systemd-timesyncd | Não | C | Não suporta NTS |
| W32Time (Windows) | Não | \- | Não suporta NTS |

O chrony é a escolha óbvia para Linux. Já é padrão na maioria das distros que importam (Fedora, RHEL, Arch, SUSE) e o Ubuntu 25.10 adotou chrony com NTS habilitado por padrão em novas instalações.

### Servidores NTS públicos confiáveis

| Servidor | Stratum | Localização | Operador |
| --- | --- | --- | --- |
| time.cloudflare.com | 3 | Anycast global | Cloudflare |
| nts.netnod.se | 1 | Suécia (anycast) | Netnod |
| sth1.nts.netnod.se | 1 | Estocolmo | Netnod |
| sth2.nts.netnod.se | 1 | Estocolmo | Netnod |
| ptbtime1.ptb.de | 1 | Alemanha | PTB (instituto de metrologia) |
| ptbtime2.ptb.de | 1 | Alemanha | PTB |
| ptbtime3.ptb.de | 1 | Alemanha | PTB |
| ptbtime4.ptb.de | 1 | Alemanha | PTB |
| ntppool1.time.nl | 1 | Holanda | TimeNL |
| ntppool2.time.nl | 1 | Holanda | TimeNL |
| ntp-pool.rdem-systems.com | 2 | França | RDEM Systems |

A lista completa e atualizada é mantida em [https://github.com/jauderho/nts-servers](https://github.com/jauderho/nts-servers)

**Sobre a escolha de servidores:** é recomendável usar pelo menos 5 servidores NTS diferentes. Isso protege contra (n-1)/2 falsetickers (servidores que reportam hora incorreta). O chrony aplica algoritmos de consenso entre as fontes para descartar outliers.

Para o Brasil, `time.cloudflare.com` é a melhor opção de latência por utilizar anycast.

Os servidores europeus (Netnod, PTB) adicionam ~150-200ms de RTT, mas para sincronização NTP isso é perfeitamente aceitável. Não existe servidor NTS público operando na América do Sul no momento. Se você tem infraestrutura e quer contribuir, o repo do jauderho aceita PRs.

## Configuração no Linux

### Arch / EndeavourOS / Omarchy

Chrony já vem com suporte NTS compilado. No Arch é o pacote padrão para sincronização de tempo.

```bash
sudo pacman -S chrony
```

Edite `/etc/chrony.conf`:

```ini
# Servidores NTS
server time.cloudflare.com iburst nts
server nts.netnod.se iburst nts
server ptbtime1.ptb.de iburst nts
server ptbtime2.ptb.de iburst nts
server ntppool1.time.nl iburst nts

# Persistir cookies NTS entre reinícios
ntsdumpdir /var/lib/chrony

# Drift file
driftfile /var/lib/chrony/chrony.drift

# Correção rápida na inicialização (max 1s de offset, 3 primeiros updates)
makestep 1.0 3

# Sincronizar RTC
rtcsync

# Log
logdir /var/log/chrony
```

Ative e inicie:

```bash
sudo systemctl enable --now chronyd
```

**Desabilite o systemd-timesyncd** (se estiver ativo), senão haverá conflito:

```bash
sudo systemctl disable --now systemd-timesyncd
```

### Fedora

Chrony já é o padrão. O NTS não é habilitado por padrão, mas a habilitação é trivial:

```bash
sudo dnf install chrony  # provavelmente já instalado
```

Edite `/etc/chrony.conf`. Comente os pools padrão e adicione servidores NTS:

```ini
# Comentar pools padrão (NTP sem autenticação)
# pool 2.fedora.pool.ntp.org iburst

# Servidores NTS
server time.cloudflare.com iburst nts
server nts.netnod.se iburst nts
server ptbtime1.ptb.de iburst nts
server ptbtime3.ptb.de iburst nts
server ntppool2.time.nl iburst nts

# Persistir cookies NTS
ntsdumpdir /var/lib/chrony

driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony

# Desabilitar NTP servers recebidos via DHCP
# (o ISP não deve ditar seu servidor de tempo)
```

Se o DHCP do seu provedor injeta servidores NTP (sim, isso acontece), remova ou comente a linha:

```ini
# sourcedir /run/chrony-dhcp
```

Restart:

```bash
sudo systemctl restart chronyd
```

### Debian / Ubuntu (pré-25.10)

Ubuntu anteriores ao 25.10 usam `systemd-timesyncd`, que não suporta NTS. A migração:

```bash
sudo apt install chrony
sudo systemctl disable --now systemd-timesyncd
sudo systemctl enable --now chronyd
```

Edite `/etc/chrony/chrony.conf` com a mesma configuração de servidores NTS acima. Em Debian, o path da config pode ser `/etc/chrony/chrony.conf` ou `/etc/chrony.conf` dependendo da versão.

Ubuntu 25.10+ já vem com chrony + NTS habilitado por padrão em instalações novas. Em upgrades, a migração é manual.

## Validação

Após configurar, verifique que NTS está funcionando:

### Verificar fontes de tempo

```bash
chronyc -N sources
```

Saída esperada (o `*` indica a fonte selecionada como referência):

```plaintext
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================
^+ time.cloudflare.com           3   6    17     4  -1643us[-6247us] +/-   70ms
^- nts.netnod.se                 1   6    17     3    +17ms[  +17ms] +/-  148ms
^* ptbtime1.ptb.de               1   6    17     3  -2576us[-7179us] +/-  112ms
^+ ptbtime2.ptb.de               1   6    17     4  +1837us[-2767us] +/-  118ms
^+ ntppool1.time.nl              1   6    17     4  +5908us[+1304us] +/-  101ms
```

`Reach` deve ser 377 (octal, significa 8 consultas consecutivas bem-sucedidas). Se for 0, o servidor não está respondendo.

### Verificar autenticação NTS

```bash
sudo chronyc -N authdata
```

Saída esperada:

```plaintext
Name/IP address             Mode KeyID Type KLen Last Atmp  NAK Cook CLen
===============================================================
time.cloudflare.com          NTS     1   30  128  141    0    0    8   64
nts.netnod.se                NTS     1   15  256  140    0    0    8  100
ptbtime1.ptb.de              NTS     1   15  256  140    0    0    8  100
ptbtime2.ptb.de              NTS     1   15  256  140    0    0    8  100
ntppool1.time.nl             NTS     1   30  128  141    0    0    8   64
```

O que verificar:

*   **Mode** deve ser `NTS` (não `NTP`)
    
*   **KeyID, Type, KLen** devem ser valores diferentes de zero
    
*   **NAK** deve ser 0 (zero falhas de autenticação)
    
*   **Cook** indica cookies disponíveis (8 é o padrão completo)
    
*   **Type 15** = AEAD-AES-SIV-CMAC-256, o algoritmo padrão
    

Se algum servidor mostrar `Mode: NTP` em vez de `NTS`, o handshake NTS-KE falhou. Causas comuns: firewall bloqueando porta TCP 4460 (NTS-KE), DNS não resolvendo o hostname, ou certificado TLS expirado no servidor.

### Verificar status geral

```bash
chronyc tracking
```

Mostra stratum, offset atual, frequência de correção e status do leap second. Um sistema bem sincronizado terá offset na faixa de microsegundos.

```bash
Reference ID    : CFC5577C (time.cloudflare.com)
Stratum         : 4
Ref time (UTC)  : Wed Apr 01 19:15:42 2026
System time     : 0.000761374 seconds slow of NTP time
Last offset     : -0.001684829 seconds
RMS offset      : 0.004399488 seconds
Frequency       : 29.567 ppm slow
Residual freq   : -0.000 ppm
Skew            : 176.681 ppm
Root delay      : 0.138503209 seconds
Root dispersion : 0.006709417 seconds
Update interval : 63.1 seconds
Leap status     : Normal
```

## Considerações de firewall

O NTS precisa de duas portas abertas para saída:

*   **UDP 123** (NTP): queries de tempo padrão
    
*   **TCP 4460** (NTS-KE): handshake TLS para troca de chaves
    

Se você opera em rede corporativa ou com firewall egress restritivo, a porta 4460 pode estar bloqueada.

Alguns ISPs também bloqueiam ou fazem rate-limit em pacotes NTP maiores (que incluem as extensões NTS), confundindo-os com tentativas de amplificação.

Se o NTS-KE falhar, o chrony por padrão não sincroniza com aquela fonte (comportamento seguro: melhor não sincronizar do que sincronizar sem autenticação).

## Boards ARM sem RTC

Se você roda chrony em Raspberry Pi ou outros ARM boards sem RTC (Real-Time Clock), há um problema: na inicialização, o relógio do sistema pode estar tão deslocado que o certificado TLS do servidor NTS é rejeitado (porque o sistema acha que estamos em 1970 ou alguma data absurda).

Solução: adicione à configuração:

```ini
nocerttimecheck 1
```

Isso permite que a primeira verificação de certificado ignore a validade temporal. Depois que o relógio sincroniza, as verificações subsequentes funcionam normalmente. Existe impacto de segurança (um certificado expirado seria aceito no primeiro boot), mas é um trade-off aceitável para hardware sem RTC.

## ntpd-rs: a alternativa em Rust

Para quem prefere software com foco em segurança de memória, o ntpd-rs é um cliente e servidor NTP escrito em Rust, financiado pela ISRG (a mesma organização por trás do Let's Encrypt) através do projeto Prossimo. Suporta NTS nativamente.

Configuração básica em `/etc/ntpd-rs/ntp.toml`:

```toml
[observability]
log-level = "warn"

[[source]]
mode = "nts"
address = "time.cloudflare.com"

[[source]]
mode = "nts"
address = "nts.netnod.se"

[[source]]
mode = "nts"
address = "ptbtime1.ptb.de"

[synchronization]
single-step-panic-threshold = 1800
startup-step-panic-threshold = { forward = "inf", backward = "inf" }
```

O ntpd-rs está disponível em algumas distros mas é menos maduro que o chrony. Para produção, chrony continua sendo a referência. Mas vale acompanhar o ntpd-rs, especialmente se segurança de memória é prioridade na sua stack.

## Conclusão

NTP sem autenticação é mais uma herança dos anos 80 que nunca deveria ter chegado a 2026 sem correção. Mas aqui estamos. A boa notícia é que a correção existe (NTS), é padronizada (RFC 8915), está implementada nos clientes principais (chrony) e disponível em servidores públicos confiáveis.

A configuração leva 5 minutos. Menos que o tempo que você gastou lendo este artigo.

Se você seguiu a série até aqui, sua pilha de proteção agora cobre DNS (DNSCrypt/DoH/DoT), handshake TLS (ECH) e sincronização de tempo (NTS). Três protocolos que o seu ISP usava como janela de observação, agora fechados.

O próximo passo natural é olhar para o que sai da sua máquina sem você saber. Auditoria de tráfego egress com opensnitch, ss e tcpdump. Mas isso fica para o próximo artigo.

Referências e leitura complementar:

*   RFC 8915 - Network Time Security for the Network Time Protocol: https://datatracker.ietf.org/doc/html/rfc8915
    
*   Attacking the Network Time Protocol (BU, 2015): https://www.cs.bu.edu/~goldbe/papers/NTPattack.pdf
    
*   Lista de servidores NTS: https://github.com/jauderho/nts-servers
    
*   chrony NTS (ArchWiki): https://wiki.archlinux.org/title/Chrony
    
*   Red Hat - NTS in chrony: https://docs.redhat.com/en/documentation/red\_hat\_enterprise\_linux/10/html/configuring\_time\_synchronization/overview-of-network-time-security-nts-in-chrony
    
*   Fedora NTS: https://fedoraproject.org/wiki/Changes/NetworkTimeSecurity
    
*   Cloudflare Time Services: https://developers.cloudflare.com/time-services/nts/
    
*   ntpd-rs (Prossimo/ISRG): https://github.com/pendulum-project/ntpd-rs
    
*   NTP Tester (verificação NTS): https://www.ntp-tester.eu/nts.html
    
*   Série DNSCrypt: https://esli.blog.br/dnscrypt-proxy
    
*   Não confie no seu provedor: https://esli.blog.br/nao-confie-no-seu-provedor-de-internet
    
*   SNI Leak e ECH: https://esli.blog.br/sni-leak-ech
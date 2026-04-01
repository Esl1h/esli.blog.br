---
title: "Não confie no seu provedor de internet"
datePublished: 2026-03-29T23:42:17.869Z
cuid: cmncelz48007m1ql41krnadp0
slug: nao-confie-no-seu-provedor-de-internet
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/0a10b6e2-e2dc-4e1d-a73e-fee92b2fbcbb.png
tags: dns, https, security, privacy, tls, vpn, isp, anonymity, psa, spying

---

Seu provedor de internet sabe mais sobre você do que a maioria dos seus amigos. Cada site que você acessa, cada busca que faz, cada serviço que usa. Tudo passa pelos servidores dele. E se você acha que ele trata esses dados com o devido cuidado... este artigo é para você.

Vou mostrar por que o ISP (Internet Service Provider) é o elo mais perigoso da sua cadeia de conectividade, casos reais em que provedores no Brasil e no mundo falharam (ou agiram contra seus próprios clientes) e, principalmente, o que fazer para reduzir drasticamente o poder que ele tem sobre a sua vida digital.

## O problema: seu ISP vê tudo

Quando você digita um endereço no navegador, a primeira coisa que acontece é uma consulta DNS. Por padrão, essa consulta vai para o servidor DNS do seu provedor. Em texto plano. Sem criptografia. Ele sabe exatamente quais domínios você resolve, quando e com qual frequência.

Mesmo com HTTPS (que criptografa o conteúdo), o provedor ainda enxerga:

*   O domínio que você está acessando (via SNI no TLS handshake)
    
*   Os IPs de destino e origem
    
*   O volume de dados trafegados
    
*   Os horários de acesso
    
*   O tipo de protocolo utilizado
    

Com essas informações, dá para construir um perfil comportamental completo. E muitos provedores fazem exatamente isso.

## Casos reais: quando a confiança quebrou

### Brasil: envenenamento de DNS na Oi e GVT

Em 2011, servidores DNS da Oi Velox e GVT foram envenenados. Ao acessar sites como o Google, os usuários eram induzidos a baixar um executável malicioso chamado `Google_Setup.exe`. Os arquivos continham rotinas de captura de dados. Na época, a Oi tinha cerca de 6 milhões de assinantes e a GVT, 1,4 milhão. A Oi negou o ataque. Padrão.

### Brasil: dados de 28 milhões de clientes da NET/Claro à venda

Em 2020, um hacker colocou à venda em fórum da dark web dados de quase 28 milhões de clientes da antiga NET (Claro), incluindo nome completo, CPF, data de nascimento, email, telefone e endereço. A Claro informou que “não foram encontradas evidências” de que os dados fossem da empresa. Padrão, de novo.

### Brasil: Claro, TIM, Vivo e Oi compartilhando dados de forma ilícita

O Ministério Público da Bahia acionou as quatro maiores operadoras por "vazamento de dados" e compartilhamento ilícito de informações pessoais, obrigando-as a obter consentimento antes de continuar tratando dados dos clientes. O relatório "Quem defende seus dados?" do InternetLab revelou que nenhuma operadora brasileira sequer avisa o usuário quando entrega dados para autoridades.

### Brasil: Anatel investiga Vivo, Claro e TIM por espionagem

Em 2024, a Anatel abriu processos administrativos para apurar se Vivo, Claro e TIM sabiam de monitoramento indevido de dados de clientes e se deveriam ter notificado a agência. As operadoras disseram que só souberam pela imprensa. Se você confia nisso, eu tenho uns NFTs para te vender.

### EUA: Verizon e os supercookies

A Verizon injetou silenciosamente um identificador único (X-UIDH) em todo o tráfego HTTP dos seus clientes mobile por dois anos. Esse "supercookie" não podia ser removido, desabilitado ou evitado pelo usuário. Modo incógnito? Inútil. Limpar cookies? Inútil. O rastreador era injetado no nível de rede, depois que o tráfego saía do dispositivo. A FCC multou a Verizon em US$ 1,35 milhão em 2016, o que equivale a troco de bala para uma empresa daquele porte.

A Comcast também foi flagrada injetando anúncios JavaScript nos navegadores de dispositivos conectados aos seus 3,5 milhões de hotspots WiFi.

### Reino Unido: Investigatory Powers Act

Desde 2016, o "Snooper's Charter" obriga ISPs britânicos a armazenar o histórico de conexão de todos os clientes por 12 meses, acessível sem mandado judicial. O governo pode ver quais sites você acessou, quando, de qual dispositivo. Membros do parlamento são explicitamente protegidos dessa vigilância. Uma regra para eles, outra para o resto.

### Turquia: ISPs como braço do Estado

Na Turquia, ISPs são obrigados a reter logs de 1 a 2 anos e, não oficialmente, a implementar Deep Packet Inspection (DPI). Quando o Twitter foi banido em 2014, os ISPs primeiro envenenaram o DNS, depois bloquearam todos os IPs do Twitter. Os turcos responderam pichando os endereços do DNS do Google (`8.8.8.8` e `8.8.4.4`) em muros pela cidade. O uso do Twitter no país cresceu 138%.

## O Estado como ameaça: vigilância, mandados secretos e perseguição política

Se os casos acima mostram provedores falhando por incompetência, negligência ou ganância, este ponto é sobre algo pior: governos usando provedores como instrumento de vigilância em massa, com mandados secretos, sem contraditório e sem que o alvo sequer saiba que está sendo monitorado.

### EUA: a FISA Court e os 3,4 milhões de buscas sem mandado

Nos Estados Unidos, a Section 702 do Foreign Intelligence Surveillance Act (FISA) permite que a NSA, o FBI e a CIA coletem comunicações de estrangeiros fora do país sem mandado judicial individualizado. Até aqui, parece razoável. O problema é o que acontece com os americanos (e qualquer pessoa que se comunique com esses alvos estrangeiros): suas mensagens, emails, ligações e textos são coletados "incidentalmente" e armazenados em bancos de dados que o FBI pode consultar livremente.

Só em 2021, o FBI realizou 3,4 milhões de buscas nessas bases sem qualquer mandado. Entre os alvos: manifestantes do Black Lives Matter (141 pessoas monitoradas por protestar contra o assassinato de George Floyd), mais de 19.000 doadores de uma campanha eleitoral, jornalistas, membros do Congresso, assessores parlamentares e, num caso particularmente absurdo, um juiz estadual que havia procurado o FBI para denunciar violações de direitos civis por um chefe de polícia local. O FBI pesquisou as comunicações dele. Do denunciante.

Essas buscas acontecem num tribunal secreto (a FISA Court) que funciona sem a presença do investigado, sem advogado de defesa e com taxa de aprovação que beira 99%. Em 2022, dos 354 pedidos de vigilância, apenas 7 foram rejeitados. O investigado nunca sabe que foi alvo. Nunca tem chance de se defender. E as empresas que recebem essas ordens são obrigadas por lei a cumpri-las e proibidas (sob gag order) de revelar que as receberam.

As National Security Letters (NSLs) são ainda piores: o FBI pode solicitar dados de provedores sem passar por juiz algum. Basta um agente atestar que a informação é "relevante para segurança nacional". A empresa é proibida de informar o usuário. É um sistema onde o Estado pode vasculhar a vida digital de qualquer cidadão sem acusação formal, sem julgamento e sem chance de defesa.

### Brasil: interceptação massiva e o uso político da vigilância

No Brasil, a Lei 9.296/96 regulamenta a interceptação telemática, exigindo ordem judicial. Na teoria. Na prática, o volume de interceptações no Brasil é assustador. Em 2013, foram registrados mais de 50.000 avisos de interceptação enviados às operadoras de telecomunicações. Comparando: os EUA, com 120 milhões de habitantes a mais, autorizaram 3.576 no mesmo ano.

Em 2012, a Polícia Federal implementou o SIS (Sistema de Interceptação de Sinais), que permite interceptar qualquer comunicação na rede de uma operadora sem interação caso a caso com a telco. O sistema eliminou a necessidade de pedir cooperação técnica da operadora para cada alvo. Ou seja: a infraestrutura para vigilância em massa já está montada. Depende apenas de quem está no comando.

Em 2013, documentos revelaram que o Gabinete de Segurança Institucional da Presidência ordenou à ABIN monitorar sindicatos e movimentos sociais opositores. Líderes da ONG Xingu Vivo, que lutavam contra a construção da Usina de Belo Monte em terra indígena, foram espionados por agentes infiltrados. No estado de São Paulo, foi revelado o monitoramento de movimentos de moradia.

A LGPD (Lei Geral de Proteção de Dados) exclui explicitamente de seu escopo o tratamento de dados para fins de segurança do Estado, persecução penal e defesa nacional. Essa exceção existe para ser regulamentada por legislação específica que, até hoje, não foi criada. Resultado: há um buraco negro legal onde o Estado pode operar com pouquíssima supervisão quando o assunto é vigilância.

### O problema estrutural: seu ISP é o ponto de coleta

Independente do país, o padrão se repete: o governo não precisa hackear seu dispositivo. Ele vai ao provedor, que por força de lei é obrigado a reter dados e, em muitos casos, entregar informações sem que o alvo seja notificado, sem direito a defesa e, frequentemente, sem que o próprio provedor questione a ordem.

No Reino Unido, o Investigatory Powers Act obriga a retenção por 12 meses. Na Turquia, de 1 a 2 anos. No Brasil, o Marco Civil da Internet exige guarda de registros de conexão por 1 ano e de registros de acesso a aplicações por 6 meses.

Você não precisa ser criminoso, suspeito ou investigado. Basta ser inconveniente. Jornalista investigando corrupção. Ativista ambiental. Advogado de direitos humanos. Doador de campanha. Ou simplesmente alguém que denunciou um policial corrupto, como o juiz americano espionado pelo FBI por fazer a coisa certa.

Quando seu tráfego passa em texto plano pelo provedor, quando seu DNS não é criptografado, quando você não usa VPN, cada pacote é evidência potencial num processo que pode nunca chegar ao seu conhecimento. A infraestrutura de vigilância já existe. A questão não é se ela será usada contra inocentes. A história mostra que já foi, é, e continuará sendo.

Proteger seu tráfego não é paranoia. É o mínimo de higiene digital para quem entende como funciona a cadeia de custódia entre o seu dispositivo e a internet.

## O que fazer: retome o controle

### 1\. Use seu próprio DNS

Pare de usar o DNS do provedor. Configure um provedor de DNS que respeite privacidade e ofereça filtragem:

| Provedor | DoH | DoT | Bloqueio de Ads/Trackers | Preço |
| --- | --- | --- | --- | --- |
| NextDNS | Sim | Sim | Sim, configurável | Free / $19.90/ano |
| ControlD | Sim | Sim | Sim, configurável | Free / a partir de $2/mês |
| AdGuard DNS | Sim | Sim | Sim | Free / $2.49/mês |
| Quad9 | Sim | Sim | Malware blocking | Gratuito |
| Cloudflare (1.1.1.1) | Sim | Sim | Apenas na versão families | Gratuito |

NextDNS e ControlD se destacam pela granularidade: você escolhe quais categorias bloquear, visualiza logs em tempo real e configura perfis por dispositivo. É basicamente um Pi-hole na nuvem, sem precisar manter hardware.

### 2\. DNS over TLS (DoT) e DNS over HTTPS (DoH)

Não basta trocar o IP do DNS. Se a consulta continuar em texto plano, o provedor ainda pode interceptar, redirecionar ou até bloquear. Por isso é fundamental usar DNS criptografado:

**DoT (porta 853):** Encapsula as consultas DNS em TLS. A maioria dos roteadores e sistemas operacionais modernos suporta nativamente. No Android, é o "DNS Privado" nas configurações de rede.

**DoH (porta 443):** Encapsula as consultas DNS dentro de HTTPS. Mais difícil de bloquear pelo provedor, porque se mistura com tráfego web normal.

No Linux, configure com `systemd-resolved`:

```ini
# /etc/systemd/resolved.conf
[Resolve]
DNS=45.90.28.0#SEUPROFILE.dns.nextdns.io
DNSOverTLS=yes
DNSSEC=yes
```

**Ou use o** `dnscrypt-proxy` **para DoH com ODoH (Oblivious DoH), que adiciona mais uma camada separando quem faz a consulta de quem resolve.**

%[https://esli.blog.br/dnscrypt-dns-stamps-e-dns-criptografado-o-guia-que-faltava] 

### 3\. No celular: RethinkDNS e InviZible Pro

**RethinkDNS** (Android): Funciona como firewall + DNS resolver. Permite configurar DoH/DoT com NextDNS, ControlD ou qualquer endpoint customizado. Bloqueia conexões por app, mostra logs em tempo real e não precisa de root. É open-source.

**InviZible Pro** (Android): Combina DNSCrypt + Tor + I2P + firewall. Permite rotear DNS via DNSCrypt ou Tor, enquanto o tráfego normal segue por VPN ou direto. Também open-source, disponível no F-Droid.

Se o seu Android suporta "DNS Privado" (Android 9+), configure diretamente com o hostname DoT do NextDNS, ControlD ou AdGuard. Mas o RethinkDNS oferece muito mais controle.

### 4\. Tenha seu próprio roteador em modo bridge

O roteador que o provedor te entrega é território dele. Firmware proprietário, configurações limitadas, atualizações que ele faz (ou não faz) quando quer, e potencialmente com backdoors. A solução:

1.  Mude o equipamento do seu provedor para **modo bridge** (bridge mode). Isso faz com que ele funcione apenas como modem, delegando toda a parte de roteamento e firewall para o seu equipamento.
    
2.  Conecte o seu roteador na porta WAN do modem em bridge.
    
3.  Configure no seu roteador: DNS criptografado (DoT/DoH), firewall, VPN no nível de rede e WiFi com suas próprias regras.
    

Se o provedor não aceitar bridge, configure o equipamento dele com DMZ apontando para o IP do seu roteador. Não é o ideal, mas remove boa parte do controle.

Roteadores recomendados para quem quer controle total: qualquer hardware que suporte **OpenWrt**, **DDWRT**, **pfSense** ou **OPNsense**. Se for algo mais acessível (ou portátil), um GL.iNet com OpenWrt pré-instalado já resolve muito.

### 5\. VPN: sempre

Uma VPN criptografa todo o tráfego entre seu dispositivo e o servidor da VPN. Para o provedor, tudo que ele vê é uma conexão criptografada para um IP. Ele não sabe qual site você acessa, o que você baixa, nada.

O privacyguides.org recomenda três provedores: **Mullvad**, **IVPN** e **ProtonVPN**. Não vou repetir aqui o que já escrevi em https://esli.blog.br/qual-melhor-vpn mas o resumo é: se a VPN pede seu email para cadastro, já começou errado.

**TorVPN / Tor:** Para anonimato real, o Tor continua sendo a referência. Não é rápido, não é prático para tudo, mas quando você precisa que ninguém saiba o que você está fazendo (nem o provedor de VPN), é o caminho.

**VPN no roteador:** Configure a VPN diretamente no seu roteador (OpenWrt, pfSense, OPNsense ou GL.iNet suportam nativamente). Com isso, todos os dispositivos da rede passam pela VPN automaticamente. Smart TV, Alexa, dispositivos IoT... tudo protegido sem precisar de configuração individual.

```bash
# Exemplo: WireGuard no OpenWrt via CLI
opkg update && opkg install wireguard-tools luci-proto-wireguard
# Configure a interface e importe o .conf do seu provedor VPN
```

### 6\. Navegador: configure os bloqueadores

Navegadores como Brave, Tor Browser e Mullvad Browser vêm com proteções de privacidade ativadas por padrão. Mas é possível ir além:

**No Brave:**

*   Shields no modo agressivo
    
*   Filtros personalizados em `brave://settings/shields/filters`
    
*   Scriptlets customizados (modo desenvolvedor) para remover elementos indesejados
    
*   DNS privado configurado nas settings do browser
    

Já escrevi sobre isso: https://esli.blog.br/funcoes-avancadas-no-brave-browser e mantenho meus filtros aqui: https://github.com/Esl1h/Brave-Filters-and-Scriptlets

**No Tor Browser:** Use como está. Não instale extensões, não altere o user agent, não maximize a janela. O Tor funciona melhor quanto menos você mexe nele.

**No Mullvad Browser:** Fork do Firefox mantido pela Mullvad e pelo Tor Project. Focado em anti-fingerprinting. Se você não quer usar o Tor, mas quer o máximo de privacidade sem ser Brave, essa é a opção.

Acompanhe os resultados de privacidade dos navegadores em https://privacytests.org/

### 7\. Outras medidas

**DNSSEC:** Valida que a resposta DNS veio de um servidor legítimo. Protege contra envenenamento de cache. Ative no seu resolver.

**ECH (Encrypted Client Hello):** A evolução do ESNI. Criptografa o SNI no TLS handshake, impedindo que o provedor saiba qual domínio você está acessando mesmo em conexões HTTPS. O Brave e Firefox já suportam.

**OONI Probe:** Ferramenta open-source que testa se o seu provedor está censurando, bloqueando ou manipulando o tráfego. Rode periodicamente.

**DNS Leak Test:** Acesse https://dnsleaktest.com ou https://www.whatsmydns.net para verificar se seu DNS configurado é realmente o que está sendo usado. Se aparecer o DNS do provedor, algo está errado.

**Blokada / NetGuard (Android):** Se não quiser usar RethinkDNS, são alternativas para firewall e bloqueio de trackers no celular sem root.

**Pi-hole / AdGuard Home (Self-hosted):** Se preferir manter o controle total do DNS dentro da sua rede, sem depender de serviços externos, instale um Pi-hole ou AdGuard Home no seu servidor local (um Raspberry Pi resolve). Configure como DNS upstream o NextDNS ou Quad9 com DoT.

## Resumo: a pilha de proteção

Do mais básico ao mais avançado, em ordem de implementação:

1.  **Troque o DNS** para NextDNS, ControlD ou Quad9
    
2.  **Ative DoT/DoH** em todos os dispositivos
    
3.  **Instale RethinkDNS ou InviZible** no celular
    
4.  **Use Brave, Tor ou Mullvad Browser** com bloqueadores configurados
    
5.  **Compre seu próprio roteador** e peça bridge no do provedor
    
6.  **Configure VPN** no roteador e nos dispositivos
    
7.  **Ative ECH** no navegador
    
8.  **Rode OONI Probe e DNS Leak Test** periodicamente
    
9.  **Self-host** DNS com Pi-hole ou AdGuard Home se quiser ir além
    

Nenhuma dessas medidas isoladamente resolve o problema. Mas combinadas, elas transformam a visão que o provedor tem de você: de um livro aberto para uma caixa preta.

## Além da rede: criptografia, autenticação e dados em repouso

Este artigo foca na proteção do tráfego contra o provedor, mas não existe segurança de rede sem segurança local. De nada adianta criptografar o DNS e usar VPN se seus dados estão em plaintext no disco, seu backup na nuvem não tem criptografia client-side e seu login depende apenas de uma senha que você reusa em 15 serviços.

Resumidamente (porque cada um desses tópicos merece artigo próprio, e já escrevi sobre vários deles):

**Disco criptografado:** LUKS no Linux, FileVault no macOS, BitLocker no Windows. Se o dispositivo for roubado, perdido ou apreendido, os dados ficam inacessíveis sem a chave. No Linux, encripte o disco na instalação e monte `/var/log`, `/var/tmp` e `/tmp` como tmpfs para não deixar rastros em disco. Swap também deve ser criptografada.

**Backups e nuvem:** Nunca suba arquivos para cloud storage sem criptografia client-side. O provedor de nuvem (Google, Dropbox, OneDrive) tem acesso ao conteúdo, assim como qualquer governo que solicite via mandado. Use Cryptomator para nuvem (criptografia por arquivo, transparente, funciona com qualquer provedor) ou Picocrypt para arquivos avulsos. Para containers maiores ou hidden volumes, VeraCrypt. Para quem respira terminal, Tomb (wrapper LUKS com key file separado e steganografia opcional).

**2FA/MFA e chave física:** Habilite autenticação de dois fatores em tudo. SMS não é 2FA seguro (SIM swap existe). Use TOTP via Aegis (Android, open-source) ou diretamente via Yubikey com o Yubico Authenticator. A Yubikey é uma chave física que suporta FIDO2, U2F, OpenPGP, PIV e TOTP. Funciona para login no sistema (via PAM no Linux), SSH (ed25519-sk), GPG e praticamente qualquer serviço que suporte WebAuthn. Se você não tem uma, considere seriamente comprar. Se tem, configure ela em tudo.

**Gerenciador de senhas:** Senhas únicas, complexas e armazenadas em um gerenciador com criptografia E2EE. ProtonPass, Bitwarden ou KeePassXC. Nunca reutilize senhas entre serviços.

Já escrevi em detalhes sobre tudo isso:

*   Criptografia para iniciantes: https://esli.blog.br/criptografia-para-iniciantes
    
*   Ferramentas de criptografia (VeraCrypt, Cryptomator, Picocrypt, Kryptor, Tomb): https://esli.blog.br/ferramentas-para-criptografia
    
*   Como escolher a ferramenta certa: https://esli.blog.br/como-escolher-ferramentas-de-criptografia
    
*   Certificados, OpenSSL, GPG e OpenPGP: https://esli.blog.br/certificados-e-openssl
    
*   Yubikey (série completa):
    
    *   Introdução ao 2FA: https://esli.blog.br/yubikey-introducao
        
    *   Instalação no Linux: https://esli.blog.br/yubikey-linux-instalacao
        
    *   2FA no console, sudo e SSH: https://esli.blog.br/yubikey-console-sudo-ssh
        
    *   SSH com ed25519-sk: https://esli.blog.br/yubikey-ssh-ed25519-ecdsa
        
    *   Chaves GPG na Yubikey: https://esli.blog.br/yubikey-chaves-gpg
        
*   Privacidade e Segurança (guia completo): https://esli.blog.br/privacidade-e-seguranca-2025
    

A proteção de rede que este artigo descreve é uma camada. Criptografia de dados em repouso é outra. Autenticação forte é outra. Nenhuma substitui a outra. Todas se complementam.

## Conclusão

A internet é um serviço pelo qual você paga. Mas o provedor não se comporta como um simples "teletransportador" de pacotes. Ele inspeciona, registra, vende, compartilha e, quando pressionado por governos, entrega sem questionar.

Confiar no provedor com seus dados é como confiar no carteiro para não ler suas cartas. Só que o carteiro tem um scanner, uma impressora e um contrato com empresas de marketing.

A boa notícia é que as ferramentas existem e estão acessíveis. A maioria é gratuita, open-source e documentada. O custo real é o tempo de configurar. E se você chegou até aqui, provavelmente já tem esse tempo.

Use-o.

Referências e leitura complementar:

*   https://privacyguides.org
    
*   https://privacytests.org
    
*   https://ssd.eff.org
    
*   https://esli.blog.br/privacidade-e-seguranca-2025
    
*   https://esli.blog.br/qual-melhor-vpn
    
*   https://esli.blog.br/qual-melhor-navegador
    
*   https://esli.blog.br/criptografia-para-iniciantes
    

![](https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/2e0780c9-33e0-4d43-82f4-d5f9e8b45963.png align="center")
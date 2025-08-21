---
title: "Criptografia para iniciantes"
datePublished: Thu Aug 21 2025 01:52:02 GMT+0000 (Coordinated Universal Time)
cuid: cmekqxksn000102ju9f9p4ner
slug: criptografia-para-iniciantes
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ZyttGSu-o2E/upload/e7c7159d601937cf2cc1e983ed0d1832.jpeg
tags: cryptography, openpgp, psa, ee2e

---

*“A privacidade não é um privilégio, é um direito. A criptografia é a ferramenta que nos permite exercê‑lo.”*

A palavra “criptografia” costuma aparecer em manchetes de tecnologia, mas ainda gera dúvidas para quem não tem formação técnica. Neste artigo vamos explicar, de forma simples e prática, como a criptografia funciona, apresentar alguns exemplos do cotidiano e mostrar como você pode aplicá‑la hoje mesmo para proteger suas informações digitais.

Em termos básicos, criptografia é a arte de transformar uma mensagem legível (texto, foto, vídeo etc.) em algo incompreensível para quem não possui a “chave” correta. Quando uma pessoa recebe um dado cifrado (chaveado, encriptado), ela precisa usar a mesma (ou outra) chave para revertê‑lo ao formato original e assim, conseguir visualizar aquele dado.

Com isto, garante-se:

* **Confidencialidade**: só quem tem a chave pode ler/acessar o conteúdo.
    
* **Integridade**: garante que a mensagem não foi alterada durante o envio/trânsito.
    
* **Autenticidade**: permite confirmar e confiar em quem enviou a informação.
    

Esses três pilares são a base da segurança online que usamos diariamente – desde o login em um site até a troca de mensagens entre amigos.

Basicamente há 2 tipos de criptografias/chaves: simétricos vs. assimétricos

| Tipo | Como funciona | Exemplo cotidiano |
| --- | --- | --- |
| **Simétrica** | Uma única chave permite cifrar *e* decifrar. Ambas as partes precisam ter a mesma chave antes da comunicação. | **Wi‑Fi doméstico (WPA2/WPA3).** O roteador e seu aparelho conectado compartilham a mesma senha. |
| **Assimétrica** | Usa um par de chaves: **pública** (para cifrar) e **privada** (para decifrar). A chave pública pode ser divulgada livremente; a privada deve ser sempre secreta. | **HTTPS.** O site e o seu navegador possuem chaves públicas e privadas e fazem a troca das chaves publicas para estabelecer confiaça na comunicação. |

A maioria das aplicações modernas combina ambos: primeiro trocam uma chave simétrica seguramente usando criptografia assimétrica, depois usam a chave simétrica para proteger o volume maior de dados (por ser mais rápida).

### Conceitos-chave:

* **Hash**: função que transforma qualquer entrada em um código fixo (ex.: SHA‑256). Permite verificar integridade, mas não pode ser revertido.
    
* **Salt**: valor aleatório adicionado a senhas antes de gerar o hash, dificultando ataques de força bruta.
    
* **TLS/SSL:** protocolos que utilizam criptografia assimétrica para estabelecer uma conexão segura.
    

### Limitações e cuidados ao usar criptografia:

* **Chave perdida = dados inacessíveis.** Sempre faça backups seguros das chaves privadas (ex.: armazenar em um cofre físico ou em um dispositivo offline).
    
* **Phishing ainda funciona.** Mesmo com criptografia forte, se você entregar sua senha a um site falso, o atacante terá acesso. Mantenha atenção e use autenticação de dois fatores (2FA).
    
* **Regulamentações locais.** Alguns países impõem restrições ao uso de criptografia forte. Verifique a legislação vigente se estiver em ambientes corporativos ou governamentais.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755740343211/36f40dd2-f3ee-46a6-baa3-0739fadd847e.png align="center")

# Como aplicar a criptografia no dia-a-dia

## Crie suas chaves OpenPGP

Use o app Kleopatra (se não souber usar a linha de comando no Linux). Com o Kleopatra, conseguirá criar sua chave OpenPGP.

Crie de preferência, usando o algoritmo ECC (Ed25519).

Pelo Kleopatra conseguirá exportar a chave pública (também chamada de certificado) e backup das chaves secretas/privadas (todas em formato .asc). Estas chaves terão um nome e e-mail, bem como descrição. Podem ter data de expirar (ou não), além de senha/passphrase.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755740743708/c858b304-9ab8-4080-bca1-3208d6d865c2.png align="center")

Já expliquei sobre o OpenPGP, GPG e outros aqui: [https://esli.blog.br/certificados-e-openssl](https://esli.blog.br/certificados-e-openssl)

Há sites onde você pode publicar suas chaves públicas, como:

* [keys.openpgp.org](http://keys.openpgp.org)
    

* [keyserver.ubuntu.com](http://keyserver.ubuntu.com)
    

* [pgp.mit.edu](http://pgp.mit.edu)
    

* [https://keys.openpgp.org/](https://keys.openpgp.org/)
    

Ou até mesmo no git (Github, GitLab, Codeberg, etc...)

## Encriptando seu email

Usando as chaves no GMAIL ou outro provedor INSEGURO:

Use o cliente de email Betterbird, por exemplo. Configure o seu Gmail nele como POP3 (Ou IMAP se preferir)

> POP3 baixa emails do servidor para o dispositivo local e geralmente os remove do servidor.
> 
> IMAP mantém emails no servidor permitindo acesso de múltiplos dispositivos ou via web posteriormente.

No Betterbird, você consegue adicionar sua chave OpenPGP.

Agora, usando o SimpleLogin, você consegue criar um alias de emails

Ou seja, ao enviar um e-mail para algumacoisa.1753@simplelogin.com ele vai direcionar para o seu email.verdadeiro@gmail.com e ao adicionar a chave OpenPGP no SimpleLogin, ele vai encriptar toda a mensagem (e o título dela).

Com isso, se alguém enviar uma mensagem para o email alias, o SimpleLogin vai encriptar com sua chave, o Gmail/Google não terá acesso a nada e somente no Betterbird, com a sua chave privada configurada, ele irá abrir sua mensagem normalmente.

Se alguém acessar seu Gmail, não terá acesso a nada.

Se outras pessoas tiverem sua chave pública, podem encriptar a mensagem e te enviar e-mails direto (para seu endereço real) e somente no cliente de e-mail com a sua chave privada, conseguirá abrir.

> Obviamente, a principal dica é: SAIA de qualquer serviço do Google. Troque seu email por outro provedor, como o Tuta ou Proton.

Além do SimpleLogin, há o Addy.io que fornece o mesmo serviço.

Flowcrypt e Mailveloper basicamente realizam esta encriptação que descrevi acima, mas de forma mais simples e via extensões/apps próprios.

## Encriptando arquivos

Usando app Picocrypt, irá encriptar qualquer arquivo com senha.

Usando o cryptomator ou veracrypt, ambos irão criar um disco virtual encriptado, que abrirá no seu coputador como uma pasta e tudo lá dentro estará encriptado ao fechar. Excelente para HDs externos, pendrives, armazenamento em nuvens e etc.

## EE2E

Prefira serviços e apps que garantam EE2E, Encriptação end-to-end. Ou seja, toda criptografia é fechada entre as duas pontas finais e o provedor do serviço não consegue de nenhuma maneira acessar o conteúdo ou ter visibilidade.

| Categoria | Serviço/App | Jurisdição | Auditoria / Código | Status |
| --- | --- | --- | --- | --- |
| **Mensageria** | Signal | EUA / Suíça (fund.) | Código aberto, auditado | Ativo e mantido |
|  | Threema | Suíça | Auditorias independentes | Ativo e mantido |
|  | Session | Austrália | Open source, descentralizado | Ativo, comunidade ativa |
|  | Element (Matrix) | Global / Suíça (Fnd.) | Open source, auditado | Ativo e mantido |
|  | Wire | Suíça | Auditorias independentes | Ativo, foco corporativo |
|  | Olvid | França | Auditoria formal 2020 | Ativo e mantido |
| **E-mail** | Proton Mail | Suíça | Código aberto, auditado | Ativo e mantido |
|  | Tutanota | Alemanha | Código aberto, auditado | Ativo e mantido |
|  | Mailfence | Bélgica | Suporte OpenPGP, auditado | Ativo, estável |
| **Armazenamento** | Proton Drive | Suíça | Auditoria externa | Ativo e mantido |
|  | Tresorit | Suíça/Hungria | Certificações ISO, auditoria | Ativo, foco corporativo |
|  | [Sync.com](http://Sync.com) | Canadá | Zero-knowledge, auditoria | Ativo e mantido |
|  | [MEGA.nz](http://MEGA.nz) | Nova Zelândia | Open source, críticas na governança | Ativo, confiabilidade discutível |
|  | Internxt Drive | Espanha | Código aberto, auditorias parciais | Ativo e mantido |
| **Notas / Docs** | Standard Notes | EUA / Global | Open source, auditado | Ativo e mantido |
|  | Joplin | Global (open src) | Open source, verificado | Ativo e mantido |
|  | Cryptee | Estônia | Open source, E2EE nativo | Ativo e mantido |
|  | CryptPad | França | Open source, auditado | Ativo e mantido |
| **Videoconferência** | Signal (voz/vídeo) | EUA / Suíça | Open source, auditado | Ativo e mantido |
|  | Wire | Suíça | Auditorias externas | Ativo |
|  | Element (Matrix) | Global / Suíça | Open source | Ativo |
|  | Jitsi Meet (E2EE ativado) | Global | Experimental, não default | Ativo, mas E2EE limitado |
|  | Olvid | França | E2EE total | Ativo |
| **Gerenciador de Senhas** | Bitwarden | EUA | Open source, auditorias externas | Ativo e mantido |
|  | KeePassXC | Global (open src) | Open source, auditorias comunitárias | Ativo e mantido |
|  | 1Password | Canadá / EUA | Arquitetura auditada | Ativo, comercial |
|  | Proton Pass | Suíça | Open source, auditoria 2023 | Ativo e mantido |
|  | NordPass | Panamá | XChaCha20, auditorias externas | Ativo |
| **Calendário** | Proton Calendar | Suíça | Auditoria externa | Ativo e mantido |
|  | Tutanota Calendar | Alemanha | Código aberto | Ativo e mantido |
| **Fotos / Mídia** | Cryptee Photos | Estônia | Open source | Ativo e mantido |
|  | [Ente.io](http://Ente.io) | Índia / Global | Open source, E2EE nativo | Ativo e mantido |
|  | Internxt Photos | Espanha | Código aberto | Ativo e mantido |
|  | PhotoPrism (self-hosted) | Global | Open source | Ativo e mantido |
| **Compartilhamento** | OnionShare | Global (Tor) | Open source, auditado | Ativo e mantido |
|  | Magic Wormhole | Global | CLI, open source | Ativo e mantido |
|  | Keybase (limitado) | EUA | Open source, mas em declínio | Parcialmente ativo, em desuso |
| **Backup** | BorgBackup | Global (self-hosted) | Open source | Ativo e mantido |
|  | Duplicacy | EUA | E2EE validado | Ativo e mantido |
|  | Arq Backup | EUA | E2EE validado | Ativo, software proprietário |

**Por que os países são importantes? MLAT, Five / Nine / Fourteen Eyes. Mas não quero assustar, abordo em outro artigo.**

## Não confie no seu provedor - ISP

Mude seu roteador para o modo bridge e adicione um roteador próprio, comprado e bem configurado por você. Troque as senhas... Por padrão, o provedor (e as terceiras que prestam serviço) conseguem acessar o roteador da operadora.

Adicione em toda a rede e em seus dispositivos, seu próprio DNS de preferência com TLS/HTTPS, como o ControlD ou o NextDNS.

Quando seu computador ‘consulta’ um site, por padrão, essa consulta é feita no seu provedor de internet e fica atrelada ao seu IP. Encripte o DNS usando outra opção do que seu provedor oferece.

## VPNs

Se a rede não é confiável, habilite uma VPN. A VPN é somente um túnel entre 2 pontos (seu dispositivo e um servidor) encriptado. Chegando na outra ponta, ele sairá normalmente pela internet.

Use preferencialmente Wireguard.

Use serviços de VPN que permitam alterar o DNS.

Nada impede de criar uma VPN para a sua casa ou algum host seu em nuvem (como na DigitalOcean, por exemplo - sairá o mesmo preço no final das contas)

Aqui expliquei os tipos de VPNs existentes (OpenVPN, Wireguard, PPTP, L2TP, UDP ou TCP): [https://esli.blog.br/tipos-de-vpns-pptp-x-openvpn-x-l2tp-ipsec-x-sstp-x-ikev2-x-chameleon-x-wireguard](https://esli.blog.br/tipos-de-vpns-pptp-x-openvpn-x-l2tp-ipsec-x-sstp-x-ikev2-x-chameleon-x-wireguard)

Aqui qual a melhor VPN: [https://esli.blog.br/qual-melhor-vpn](https://esli.blog.br/qual-melhor-vpn)  
Mas em resumo, o [privacyguides.org](http://privacyguides.org) sugere apenas 3 provedores de VPN seguros: ProtonVPN, IVPN e Mullvad.

E aqui o motivo de VPN não ser o suficiente: [https://esli.blog.br/vpn-nao-e-o-suficiente](https://esli.blog.br/vpn-nao-e-o-suficiente)

## Browsers

Fugindo um ponto do foco em criptografia….

Qual melhor navegador: [https://esli.blog.br/qual-melhor-navegador](https://esli.blog.br/qual-melhor-navegador) em resumo, escolha entre o Brave, Mullvad, LibreWolf e Tor.  
Tenha dois desses (90% do tempo uso o Brave… restante acabo recorrendo ao Mullvad ou Tor).

## Considerações Finais

Entrar no mundo da criptografia, PSA (Privacidade, Segurança e Anonimato) te forçará a aprender e aprofundar em diversos temas.  
Gradualmente buscará o seu “De-Googling”, a escalada é grande e muitos param até onde ainda é viável e conveniente… como sair de plataformas SaaS, partir para self-hosted dentre outros.

Mas o importante é ter em mente a busca por garantias de sua liberdade e privacidade.

Privacidade não é sobre ter algo a esconder. É sobre ter algo para proteger. E esse algo é quem você é. É algo em que você acredita. É quem você quer se tornar. Privacidade é o direito de si mesmo. É o que lhe permite compartilhar com o mundo quem você é nos seus próprios termos.

%[https://esli.blog.br/privacidade-e-seguranca-2025]
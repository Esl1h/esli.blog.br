---
title: "O que é o WireGuard?"
datePublished: Tue Mar 31 2020 16:16:45 GMT+0000 (Coordinated Universal Time)
cuid: cktaa0uxn03za6gs1dvtt9tqn
slug: o-que-e-o-wireguard
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1631031332830/e8eNbvc2f.png
tags: linux, security, cryptography

---



O que é o WireGuard?

O WireGuard é um tunnel (VPN) em layer 3 de código aberto, fácil de configurar, rápido e seguro, que utiliza criptografia de última geração. Seu objetivo é fornecer uma VPN de uso geral mais rápida, simples e que possa ser facilmente implantada.

O WireGuard visa substituir o IPsec na maioria dos casos de uso e soluções baseadas em TLS como o OpenVPN.

A maioria das outras soluções como IPsec e OpenVPN foram desenvolvidas décadas atrás. O pesquisador de segurança e desenvolvedor de kernel Jason Donenfeld percebeu que eles eram lentos e difíceis de configurar e gerenciar corretamente. Isso o fez criar um novo protocolo e solução VPN de código aberto, mais rápido, seguro e fácil de implantar e gerenciar.

O WireGuard utiliza Curve25519 para troca de chaves, ChaCha20 para criptografia e Poly1305 para autenticação de dados, SipHash para chaves de hashtable e BLAKE2s para hash. O ChaCha20-Poly1305 é padronizado para IPsec e OpenVPN (através de TLS).

O WireGuard foi desenvolvido originalmente para Linux, mas agora está disponível para Windows, macOS, BSD, iOS e Android. Ainda está sob desenvolvimento pesado.

Outra coisa boa do WireGuard é que ele possui uma base de código enxuta com apenas 4000 linhas de código. Comparando com o OpenVPN que chega as 400.000 linhas de código, ele é claramente mais fácil depurar.

Como o WireGuard é executado no espaço do kernel , ele fornece rede segura em alta velocidade.

Ele foi mesclado ao Kernel Linux 5.6 e atualmente, você pode instalar o WireGuard no Linux como um módulo do kernel (DKMS)

Quando você instala o WireGuard como um módulo do kernel, basicamente modifica o kernel do Linux por conta própria e adiciona algum código a ele. Iniciando o kernel 5.6, você não precisará adicionar manualmente o módulo do kernel. Ele será incluído no kernel por padrão.

A inclusão do WireGuard no Kernel 5.6 provavelmente estenderá a adoção do WireGuard e, portanto, mudará o cenário atual da VPN.

O WireGuard está ganhando popularidade pelas boas razões. Algumas das VPNs populares voltadas para a privacidade, como a Mullvad VPN, já estão usando o WireGuard e a adoção provavelmente crescerá no futuro próximo.

Embora atualmente WireGuard esteja na versão 1.0.0 no Linux, seu pacote do Windows está na versão beta em 0.1.0; ele adicionou recursos significativos de desempenho, estabilidade, localização e acessibilidade desde a visualização passo a passo de uma versão mais antiga.

Os usuários de Mac e BSD ainda não possuem uma opção no kernel para suporte ao WireGuard, mas podem executar uma implementação feita na linguagem Go a partir de seus respectivos repositórios - pkg install wireguard no FreeBSD e brew install wireguard-tools, port install wireguard-tools, ou mesmo na própria Apple Store no Mac.

Os usuários do IOS podem encontrar o WireGuard na App Store, e os usuários do Android podem encontrá-lo na Play Store.

Nem tudo são flores, veja este artigo chamado "Porque não usar o Wireguard" do blog do IPFire:
https://blog.ipfire.org/post/why-not-wireguard

Ele lista alguns itens como:
- Não aceitar IP dinâmico no Servidor
- Não conseguir alterar o servidor sem ter que atualizar os clientes

Neste artigo da Linode, mostra o passo a passo para configurar o WireGuard (Cliente e Servidor) no Ubuntu:
https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/

Abaixo o PDF do criador, apresentando o funcionamento do Wireguard:
https://www.wireguard.com/papers/wireguard.pdf

Site oficial:
https://www.wireguard.com/

Pacotes para instalação:
https://www.wireguard.com/install/
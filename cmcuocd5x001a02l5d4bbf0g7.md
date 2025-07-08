---
title: "Cloud Storage para Linux"
seoTitle: "Best Cloud Storage Options for Linux"
seoDescription: "Explore alternativas de armazenamento em nuvem para Linux com foco em backup, segurança, compatíveis com rclone, visando privacidade"
datePublished: Tue Jul 08 2025 15:17:51 GMT+0000 (Coordinated Universal Time)
cuid: cmcuocd5x001a02l5d4bbf0g7
slug: cloud-storage-para-linux
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/2JJ3wBHu4_0/upload/46f39b8a79110819fafdfa802f12de1c.jpeg
tags: cloud, linux, storage, drive, proton, armazenamento

---

Seguindo um post (link abaixo) onde listei alternativas no Android para o armazenamento de fotos (buscando uma alternativa ao Google Fotos), listo agora quais possuem suporte ao Linux desktop, como uma solução de backup, storage em nuvem, sync e para usar como drive criptografado com o cryptomator.

%[https://esli.blog.br/alternativas-ao-google-photos-android] 

## Privacy Guides / Privacy Tools

Eles recomendam, conforme os critérios (E2EE, Open Source, TOTP/FIDO2, auditoria, entre outros), os seguintes serviços:

* [PrivacyGuides.org](https://www.privacyguides.org/en/cloud/): Proton Drive (ainda não possui cliente para Linux, mas é suportado pelo rclone), Tresorit, Peergos;
    

* [PrivacyTools.io](https://www.privacytools.io/encrypted-cloud-storage): Proton Drive, Internxt, NordLocker, Mega, NextCloud, Filen;
    

## Rclone - compatibilidade

O rclone é um programa de linha de comando para gerenciar arquivos em armazenamento em nuvem. É uma alternativa aos aplicativos oficiais de cada provedor de cloud, possui compatibilidade com dezenas de soluções e, caso utiliza mais de um provedor, terá o rclone como a forma mais rápida, fácil e simples de integrar todos eles manualmente ou em scripts.

A lista de provedores de cloud compatíveis com o rclone: [https://rclone.org/#providers](https://rclone.org/#providers)

## Cloud Storage com GUI e CLI no Linux

| **Serviço** | **Espaço grátis** | **Preço inicial (pago)** |
| --- | --- | --- |
| **pCloud** | 10 GB | $49,99/ano (500 GB) |
| [**Filen.io**](http://Filen.io) | 10 GB | $2,99/mês (200 GB) |
| **Icedrive** | 10 GB | $1,67/mês (150 GB) |
| **Seafile** | 2 GB (cloud)\* | $6/mês (100 GB)\* |
| **Koofr** | 10 GB | $0,66/mês (10 GB) |
| **Dropbox** | 2 GB | $9,99/mês (2 TB) |
| **Jottacloud** | 5 GB | $8,20/mês (1 TB) |
| **OpenDrive** | 5 GB | $5/mês (500 GB) |
| **Tresorit** | 3 GB trial | $10,99/mês (1 TB) |
| **Internxt** | 10 GB | $4,49/mês (200 GB) |
| **Peergos** | 8 GB | $2,99/mês (50 GB) |

* **Seafile**: O software é open source e pode ser auto-hospedado (grátis), mas o serviço cloud oficial oferece 2 GB grátis e planos pagos.
    
* **GUI Linux**: Considera cliente oficial, não terceiros. Alguns serviços têm GUI web (Koofr), mas não há aplicativo desktop para Linux. A grande maioria disponibiliza o instalador como .AppImage, outros fornecem pacotes .deb e .rpm
    
* **Preços**: Podem variar por região e promoções. Valores arredondados para referência. Em algumas datas pode ocorrer a oferta de planos ‘lifetime’ além de redução dos valores na assinatura anual.
    

## **Preço de armazenamento na nuvem**

Não importa qual serviço/provedor, a probabilidade do storage físico estar em grandes datacenters conhecidos é grande. Caso não use estes, é provável ainda que seja mais caro devido a manutenção da infraestrutura.

O Dropbox, por exemplo, usava o AWS S3 como storage até 2015, quando passou a migrar para sua própria solução e atualmente tenta criar um ecossistema em torno de si para agregar valor e atrair clientes.

O hardware não tem como ser barato e para o mundo dos datacenters, não há tantos fornecedores de tecnologia para armazenamento. Logo, ao adquirir, saiba que o preço possui um limite e não encontrará mais baratos, considere as funções a mais oferecidas aos assinantes.

### Qual a pegadinha com espaço de armazenamento gratuito e storage muito barato?

Se um app usa a AWS como infraestrutura cloud para oferecer seu serviço de storage, por exemplo, logo ele deve pagar algo em torno de US$0,021 por GB/mês. Porém, existe o custo cobrado pelo tráfego ao baixar, exibir... além de armazenar miniatura, processar nos servidores a criptografia, replicação destes dados para outros datacenters (ou regiões), CDN e etc.... E a maioria destes custos não tem como prever, pois o cliente é imprevisível ;-)

Logo, alguns apps limitam a velocidade para download, colocam cotas para o tráfego ou até mesmo, podem cobrar a mais para isto. Geralmente o foco destes (que praticam estas limitações) é o backup.  
Foram feitos somente para o usuário armazenar e poucas vezes acessar (baixar) os dados.

Por que eles oferecem “espaço grátis”? Atrair mais clientes.

Por que existem planos com espaço ilimitado? Esperam que usem pouco espaço.

Os planos iniciais, com menos espaço, são mais caros, logo, tentam vender os planos de maiores espaços e os 'ilimitados', torcendo para que quem assine, use pouco.

Além, disto, o segredo do lucro está na capacidade de comprimir ao máximo o arquivo para armazenar, com isto, um arquivo de 10MB, irá descontar 10MB da sua cota de espaço contratado, mas o sistema irá comprimir para 1MB... Mesmo espelhando este arquivo em 4 datacenters diferentes (4MB), ainda estará mais barato. Porém, o dano disto é o tempo de processamento para concluir o upload e para realizar o download... irá passar pelo processo de compressão/descompressão.

Por isso, muitos reclamam da demora do site abrir ou do aplicativo sincronizar, pois boa parte deste processo de compressão/descompressão está sendo feito no seu dispositivo, consumindo sua CPU, sua bateria e sua RAM.

Ou seja, com a explicação acima conclui-se: para o mesmo disco físico lá no datacenter, de 1TB, por exemplo, diversos clientes pagaram por ele. Ou porque não usam todo o espaço contratado ou devido ao algoritmo usado para comprimir.

E tem mais:

Os apps mais desconhecidos ou com preço muito 'atrativo' podem estar usando datacenters baratos, leia-se: longes, latência alta (internet ruim), equipamentos antigos, discos (HDs) com baixo poder de acesso... E até mesmo, abrir mão de backups, espelhamento (CDN, replicação em mais de uma localidade), não oferecer criptografia...

# Conclusão

Sigo aguardando o tão pedido cliente do Proton Drive para Linux, porém, o rclone (beta) suporta o ProtonDrive

Já testei e usei pCloud, IceDrive, Dropbox, Filen. E sou assinante do Proton Drive.  
Excelentes, são os que recomendo…

O melhor client para linux considero pCloud e Filen, inclusive o Filen permite a montagem usando webdav e S3, além do client possuir features a mais como notas e chat.

Sobre o Koofr, interface web estranha visualmente… porém, permite integrar com Drobpox, OneDrive, GoogleDrive, facebook e instagram (somente 2 destes na versão gratuita)… realizando sync, backups etc..

Considero o crivo do PrivacyGuides, logo, também tenho em mente para testes o Peergos e Tresorit, porém, nenhum deles é compatível oficialmente com rclone.

Para todos eles, recomendo adicionalmente que use uma criptografia adicional e independente do provedor de cloud storage, como, por exemplo, criando um drive com o **Cryptomator** ou **VeraCrypt** e mantendo o drive sincronizado com a nuvem, ou encriptando seus arquivos, seja com **GPG** ou **Picocrypt**.
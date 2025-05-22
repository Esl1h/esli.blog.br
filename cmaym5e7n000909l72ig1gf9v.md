---
title: "Explorando Funções Avançadas no Brave Browser"
seoTitle: "Avançadas Funções do Brave Browser"
seoDescription: "Explore funções avançadas do Brave Browser com URLs internas para administração, diagnóstico e ajustes de privacidade"
datePublished: Thu May 22 2025 00:08:06 GMT+0000 (Coordinated Universal Time)
cuid: cmaym5e7n000909l72ig1gf9v
slug: funcoes-avancadas-no-brave-browser
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746663436857/f0c26db2-9f1f-476e-94ec-1b43ae75beaa.jpeg
tags: browser, brave, brave-browser

---

O Brave oferece uma série de páginas internas acessíveis via URLs que começam com `brave://`.

Algumas destas URLs vêm do `chrome://` (veja a lista completa no final) e outras são redirecionamentos, como, por exemplo, a `brave://adblock/` que direciona para a `brave://settings/shields/filters`

Essas páginas são essenciais para administração, diagnóstico, configuração avançada e depuração do navegador, sendo especialmente úteis para profissionais de TI, desenvolvedores e entusiastas de privacidade. Abaixo, você encontra uma lista abrangente dessas URLs, categorizadas por finalidade, com descrições técnicas e objetivas para cada uma.

Claro, esta lista é feita com base na versão stable atual.

## Básicas e Gerais

| URL | Propósito |
| --- | --- |
| `brave://settings/` | Página central de configurações do Brave, onde é possível ajustar preferências gerais, privacidade, aparência e funcionalidades do navegador. |
| `brave://about/` | Lista todas as páginas internas (`brave://`) disponíveis, funcionando como um índice de recursos administrativos e de diagnóstico. |
| `brave://help/` | Exibe informações detalhadas sobre a versão do Brave instalada, além de links para suporte e documentação oficial. |
| `brave://version/` | Mostra dados técnicos da instalação, como versão do navegador, caminhos de diretórios, variáveis de ambiente e detalhes do sistema operacional. |

## Avançadas e de Diagnóstico

| URL | Propósito |
| --- | --- |
| `brave://flags/` | Permite ativar ou desativar recursos experimentais e opções avançadas, úteis para testes e customizações profundas. |
| `brave://components/` | Gerencia componentes internos do navegador, possibilitando atualização manual de módulos como Widevine, CRLSet, entre outros. |
| `brave://crashes/` | Exibe relatórios de falhas e travamentos do navegador, facilitando a análise de estabilidade e envio de logs para depuração. |
| `brave://sync-internals/` | Fornece informações detalhadas sobre o funcionamento do sistema de sincronização, incluindo logs, status e estatísticas de sincronização. |
| `brave://net-internals/` | Ferramenta avançada para diagnóstico de rede, permitindo análise de eventos, logs de conexões, DNS, sockets e muito mais. |
| `brave://gpu/` | Exibe informações sobre aceleração de hardware, status da GPU, drivers e recursos gráficos utilizados pelo navegador. |
| `brave://histograms/` | Apresenta métricas internas e histogramas de uso, úteis para análise de desempenho e comportamento do navegador. |

## Shields e Privacidade

| URL | Propósito |
| --- | --- |
| `brave://settings/shields` | Central de configuração global do Brave Shields, onde é possível definir políticas de bloqueio de rastreadores, anúncios e scripts. |
| `brave://adblock/` | Gerencia listas de filtros de bloqueio de anúncios e rastreadores, permitindo adicionar, remover ou customizar regras. |
| `brave://settings/privacy` | Página dedicada às configurações de privacidade, incluindo permissões, cookies, rastreamento e controles de dados sensíveis. |
| `brave://settings/clearBrowserData` | Interface para limpeza de dados de navegação, como histórico, cookies, cache e outros artefatos armazenados localmente. |

## Recompensas e Carteira

| URL | Propósito |
| --- | --- |
| `brave://rewards/` | Gerencia o sistema Brave Rewards, permitindo visualizar ganhos em BAT, configurar anúncios e contribuições automáticas. |
| `brave://wallet/` | Acessa a carteira de criptomoedas integrada, possibilitando gerenciamento de ativos digitais, tokens e NFTs. |

## Outras

| URL | Propósito |
| --- | --- |
| `brave://webrtc-internals/` | Ferramenta de diagnóstico para conexões WebRTC, exibindo logs detalhados de sessões, estatísticas e fluxos de mídia. |
| `brave://policy/` | Exibe todas as políticas ativas no navegador, fundamental para ambientes corporativos e gerenciamento centralizado. |
| `brave://downloads/` | Gerencia e exibe o histórico de downloads realizados no navegador, com opções de controle e limpeza. |
| `brave://extensions/` | Interface para gerenciamento de extensões instaladas, incluindo ativação, desativação e remoção de plugins. |

## Shields – Diagnóstico Avançado

| URL | Propósito |
| --- | --- |
| `brave://adblock-internals/` | Exibe informações técnicas sobre o mecanismo de bloqueio de anúncios, listas carregadas e regras (como regex) aplicadas em tempo real. |

## Sincronização

| URL | Propósito |
| --- | --- |
| `brave://sync/` | Interface principal para configuração e gerenciamento da sincronização de dispositivos Brave. |
| `brave://sync-internals/` | Diagnóstico detalhado do sistema de sincronização, exibindo logs, status e estatísticas de cada item sincronizado. |

## URLs Específicas e Menos Conhecidas

| URL | Propósito |
| --- | --- |
| `brave://webrtc-internals/` | Ferramenta de diagnóstico para conexões WebRTC, exibindo logs detalhados de sessões, estatísticas e fluxos de mídia. |
| `brave://policy/` | Exibe todas as políticas ativas no navegador, fundamental para ambientes corporativos e gerenciamento centralizado. |
| `brave://downloads/` | Gerencia e exibe o histórico de downloads realizados no navegador, com opções de controle e limpeza. |
| `brave://extensions/` | Interface para gerenciamento de extensões instaladas, incluindo ativação, desativação e remoção de plugins. |
| `brave://brave-ntp/` | Página de nova aba personalizada do Brave, exibindo widgets, notícias, estatísticas e atalhos configuráveis. |
| `brave://brave-search/` | Acessa diretamente o mecanismo de busca Brave Search, focado em privacidade e resultados independentes. |
| `brave://brave-crypto-wallet/` | Interface da carteira de criptomoedas (versão anterior), ainda acessível para usuários que não migraram para a nova Brave Wallet. |
| `brave://brave-wallet/` | Nova interface da Brave Wallet, com suporte aprimorado para múltiplos ativos, redes e integração DeFi. |
| `brave://brave-local-data/` | Gerencia dados locais armazenados pelo navegador, incluindo cache, bancos de dados e outros artefatos. |
| `brave://brave-ads/` | Exibe informações e diagnósticos sobre o sistema de anúncios Brave, incluindo logs e status de exibição. |
| `brave://tracing/` | Ferramenta avançada de rastreamento para análise de desempenho e diagnóstico. |
| `brave://inspect/` | Permite inspecionar dispositivos remotos, como dispositivos Android conectados via USB, para depuração de páginas e extensões. |
| `brave://quota-internals/` | Exibe informações sobre o uso de armazenamento por sites, incluindo dados em cache, IndexedDB e outros. |
| `brave://media-internals/` | Fornece detalhes sobre a reprodução de mídia no navegador, útil para diagnosticar problemas de áudio e vídeo. |
| `brave://usb-internals/` | Mostra informações sobre dispositivos USB conectados e permite testes de comunicação com eles. |
| `brave://device-log/` | Exibe logs de dispositivos conectados, útil para depuração de hardware e periféricos. |
| `brave://sandbox/` | Apresenta o status de segurança do sandboxing do navegador, indicando quais processos estão isolados. |
| `brave://user-actions/` | Lista ações do usuário registradas pelo navegador, útil para análise de comportamento e depuração. |
| `brave://predictors/` | Mostra dados utilizados pelo navegador para prever ações do usuário, como pré-carregamento de páginas. |
| `brave://signin-internals/` | Fornece informações detalhadas sobre o processo de login e autenticação de contas no navegador. |
| `brave://policy-internals/` | Exibe políticas ativas e suas origens, útil em ambientes corporativos com políticas gerenciadas. |
| `brave://management/` | Mostra informações sobre o gerenciamento do navegador, incluindo políticas aplicadas e status de gerenciamento. |
| `brave://network-errors/` | Lista códigos de erro de rede e suas descrições, auxiliando na identificação de problemas de conexão. |
| `brave://safe-browsing/` | Exibe informações sobre o status do recurso de navegação segura, incluindo atualizações e listas de ameaças. |
| `brave://site-engagement/` | Mostra pontuações de engajamento de sites, que influenciam permissões automáticas e outras decisões do navegador. |
| `brave://site-settings/` | Interface para gerenciar permissões específicas de sites, como acesso a localização, câmera e notificações. |
| `brave://translate-internals/` | Fornece detalhes sobre o funcionamento do recurso de tradução automática de páginas. |
| `brave://webrtc-logs/` | Acessa logs relacionados ao uso de WebRTC, útil para depuração de chamadas de áudio e vídeo. |
| `brave://whats-new/` | Apresenta as novidades e atualizações recentes do navegador. |

## Lista Completa de URLs

`brave://chrome-urls/` irá retornar:

```c
List of Brave URLs
brave://about
brave://access-code-cast
brave://accessibility
brave://adblock
brave://adblock-internals
brave://ads-internals
brave://app-service-internals
brave://app-settings
brave://apps
brave://attribution-internals
brave://autofill-internals
brave://batch-upload
brave://blob-internals
brave://bluetooth-internals
brave://bookmarks
brave://bookmarks-side-panel.top-chrome
brave://brave-shields.top-chrome
brave://brave-speedreader.top-chrome
brave://browser-switch
brave://certificate-manager
brave://chrome
brave://chrome-signin
brave://chrome-urls
brave://commerce-internals
brave://compare
brave://components
brave://connection-help
brave://connection-monitoring-detected
brave://connectors-internals
brave://cookie-list-opt-in.top-chrome
brave://crashes
brave://credits
brave://customize-chrome-side-panel.top-chrome
brave://data-sharing-internals
brave://device-log
brave://dino
brave://discards
brave://download-internals
brave://downloads
brave://extensions
brave://extensions-internals
brave://family-link-user-internals
brave://feedback
brave://flags
brave://gcm-internals
brave://getting-started
brave://gpu
brave://help
brave://histograms
brave://history
brave://history-clusters-internals
brave://history-clusters-side-panel.top-chrome
brave://history-side-panel.top-chrome
brave://history-sync-optin
brave://indexeddb-internals
brave://inspect
brave://internals
brave://interstitials
brave://intro
brave://leo-ai
brave://linux-proxy-config
brave://local-state
brave://location-internals
brave://managed-user-profile-notice
brave://management
brave://media-engagement
brave://media-internals
brave://media-router-internals
brave://memory-internals
brave://metrics-internals
brave://net-export
brave://net-internals
brave://network-errors
brave://new-tab-page
brave://new-tab-page-third-party
brave://new-tab-takeover
brave://newtab
brave://ntp-tiles-internals
brave://omnibox
brave://omnibox-popup.top-chrome
brave://on-device-internals
brave://on-device-translation-internals
brave://optimization-guide-internals
brave://password-manager
brave://password-manager-internals
brave://policy
brave://predictors
brave://prefs-internals
brave://print
brave://privacy-sandbox-dialog
brave://privacy-sandbox-internals
brave://private-aggregation-internals
brave://process-internals
brave://profile-customization
brave://profile-internals
brave://profile-picker
brave://quota-internals
brave://read-later.top-chrome
brave://reset-password
brave://rewards
brave://rewards-internals
brave://rewards-panel.top-chrome
brave://rewards.top-chrome
brave://safe-browsing
brave://sandbox
brave://saved-tab-groups-unsupported
brave://search-engine-choice
brave://segmentation-internals
brave://serviceworker-internals
brave://settings
brave://shopping-insights-side-panel.top-chrome
brave://signin-dice-web-intercept.top-chrome
brave://signin-email-confirmation
brave://signin-error
brave://signin-internals
brave://signout-confirmation
brave://site-engagement
brave://skus-internals
brave://suggest-internals
brave://support-tool
brave://sync-confirmation
brave://sync-internals
brave://system
brave://tab-search.top-chrome
brave://tab-strip.top-chrome
brave://terms
brave://tip-panel.top-chrome
brave://topics-internals
brave://tor-internals
brave://traces
brave://traces-internals
brave://tracing
brave://translate-internals
brave://ukm
brave://usb-internals
brave://user-actions
brave://user-education-internals
brave://version
brave://view-cert
brave://wallet
brave://wallet-panel.top-chrome
brave://web-app-internals
brave://webcompat
brave://webrtc-internals
brave://webrtc-logs
brave://webui-gallery
brave://webuijserror
brave://webxr-internals
brave://welcome
brave://whats-new
chrome-untrusted://compose
chrome-untrusted://data-sharing
chrome-untrusted://ledger-bridge
chrome-untrusted://lens
chrome-untrusted://lens-overlay
chrome-untrusted://leo-ai-conversation-entries
chrome-untrusted://line-chart-display
chrome-untrusted://market-display
chrome-untrusted://nft-display
chrome-untrusted://ntp-microsoft-auth
chrome-untrusted://playlist
chrome-untrusted://playlist-player
chrome-untrusted://print
chrome-untrusted://privacy-sandbox-dialog
chrome-untrusted://read-anything-side-panel.top-chrome
chrome-untrusted://trezor-bridge
brave://internals/session-service
```

### URLs para Debug

```c
For Debug
The following pages are for debugging purposes only. 
Because they crash or hang the renderer, they're not linked directly; 
you can type them into the address bar if you need them.

brave://badcastcrash/
brave://inducebrowsercrashforrealz/
brave://inducebrowserdcheckforrealz/
brave://crash/
brave://crash/rust
brave://crashdump/
brave://kill/
brave://hang/
brave://shorthang/
brave://gpuclean/
brave://gpucrash/
brave://gpuhang/
brave://memory-exhaust/
brave://memory-pressure-critical/
brave://memory-pressure-moderate/
brave://webuijserror/
brave://quit/
brave://restart/
```
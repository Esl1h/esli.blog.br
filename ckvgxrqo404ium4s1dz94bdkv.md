---
title: "Sabote-se: Use Excel!"
datePublished: Mon Nov 01 2021 17:28:34 GMT+0000 (Coordinated Universal Time)
cuid: ckvgxrqo404ium4s1dz94bdkv
slug: sabote-se-use-excel
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1635787555161/2-byxLfQ8.jpeg
tags: blogging, security, excel

---

Um líder certa vez me disse que cada arquivo de excel criado era uma falha no ERP/CRM, era uma gestão paralela/pirata que fugia de qualquer tipo de controle na qual transforma o ERP/CRM num bode expiatório, pois os dados lá nunca serão verdades absolutas pois vivem constantemente sabotados.

Por isto sou defensor do banimento de planilhas e MS Excel em todo lugar. Geralmente seus defensores ou não possuem resiliência para mudar ou acham que podem impressionar com seus filtros e formulas mirabolantes, e não confiam no relacionamento do banco de dados nos ERP/CRMs.


*"Ah, mas meu CRM não tem o que preciso!"* - Oras, troque-o, solicite o desenvolvimento, adapte-se... mas a gestão paralela via xls certamente lhe causará problemas.

Veja: se meu carro não trava as portas corretamente e isso era uma feature que foi vendida com ele e eu preciso disto, posso reclamar ao fabricante, arruma-lo, troca-lo... Mas criar uma planilha será como soldar uma fechadura alternativa, igual ao simpatico Leyland Mini amarelo do Mr. Bean. Não resolve o problema original, terá mais uma ferramenta para verificar (quando precisar usar), e não poderá (nunca mais) acreditar em somente uma delas, sempre vai precisar checar ambas antes de ter certeza da realidade.


![mrBean-doorlock.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1635787568948/1Qft3PZ_4.jpeg)

Pois bem:

Houve um vazamento de dados por ransomware de uma grande empresa, ou seja, alguém rodou um .exe que criptografou seus arquivos e enviou para os criminosos. 

Passado alguns dias, não houve o pagamento do resgate e eles liberaram os dados roubados. Vamos chamar esta empresa de "Desatento Brasil S.A.".

Resolvi ir lá no site dos criminosos analisar rapidamente os arquivos que estão publicamente disponíveis.

Pensei em encontrar algum dump de database zipado, com arquivos de algum file server, arquivos de web server ou outros... Mas não, tudo é mais simples o possível: 

- todos os dados veem de um mesmo computador, um desktop.
- O usuário estava com perfil de admin local no Windows.

Tudo está em arquivos de excel (xlsx), pelos nomes, a pessoa copiava os dados de um famoso ERP e mantinha localmente seu mini-erp particular com suas próprias formulas e funções eque provavelmente, segundo a mesma pessoa, deve ser melhor do que o ERP usado por ela e que cuja desenvolvedora possui mais de 103 mil funcionários dedicados ao software.

Alguns exemplos dos nomes das planilhas: comercial-Fev-2021_v1.xlsx, provavelmente foi atualizada e com medo de perder o versionamento, criou uma nova com o nome comercial-Fev-2021_v2.xlsx, e por fim outra com o nome comercial-Fev-2021_wip.xlsx (WIP de Working In Progress, ou seja, terá uma v3 em breve).

Uma outra planilha com a lista de todos os funcionários de certa diretoria, outra planilha usada apenas para cadastrar e gerenciar contatos! Isto mesmo, em 2021 com milhões de soluções simples até mirabolantes, alguém resolveu criar uma planilha de excel como agenda de contatos (lista_de_contatos_atualizada.xlsx).

E disto vai piorando, pois os demais são contratos, dados financeiros, planejamento comercial... Tudo organizado em pastas numéricas para ficar bonitinho no Windows Explorer. 

Por fim, a maioria dos arquivos possuem como nome além do departamento e data, o nome do ERP, deixando claramente que são dados exportados.

<br><br><br>
---
Nota aos paladinos do politicamente correto: não baixei, não li o conteúdo e nada disto me interessa... Apenas acessei a pagina de acesso publico onde LISTA o nome dos arquivos somente (lembrando, isto é uma ficção hein...), qualquer semelhança com a realidade ou fatos que ocorreram, foram apenas inspiração para este texto...

---
title: "Sobre carros e Docker"
datePublished: Sun Feb 11 2024 23:59:38 GMT+0000 (Coordinated Universal Time)
cuid: clsi63doy000108jyau2md1g1
slug: sobre-carros-e-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707689838071/acb6ed78-e01d-4c65-9bb8-d0d5902c47f2.jpeg
tags: containers, history, experience, bussiness, experiencia

---

Amanhã é meu último dia de férias e sem ter planejado isto, acabará numa segunda de carnaval, onde ganharei mais dois dias… e novamente, foi só coincidência.  
Ouvindo um podcast automotivo na qual possui mais de 4 horas de duração e com toda certeza escutarei novamente algumas vezes, foi dado a seguinte frase sobre a Toyota:

<mark>“Em empresas conservadoras, o erro é muito mais penalizado do que um acerto é parabenizado. O acerto é uma obrigação, um erro é erro.”</mark>

Algo bem incomum caso tenha ouvido as expressões “fordismo” e “toyotismo” em sua jornada de aprendizagem na gestão ou indústria.

E em TI, a maioria das empresas são conservadoras, não aceitam muito bem mudanças… e a maioria do pessoal de TI, quer mudar tudo, testar, provar e arriscar.

O objetivo deste texto é história, então vamos lá…

O Docker foi lançado publicamente em 2013 e eu comecei a ouvir falar dele no início de 2014.

Neste início de 2014 havia saído de uma empresa o qual todo produto estava em meia dúzia de VPS num provedor local e cujo gestor queria terceirizar tudo.

Na ocasião, numa conversa informal com um tech lead de uma famosa empresa (na época), eles estavam usando Docker em ambiente produtivo, e usavam ansible, shell script e python para automatizar (quase) tudo.  
O Ansible já tinha uns 2 anos de existência, muito novo e ainda faltando amadurecer. Falei dele um pouco neste post, quando tive um desafio anos antes:

%[https://esli.blog.br/ansible-chef-fabric-puppet-ou-salt] 

Mas não trabalhei com eles.

Fui olhar esse Docker…  já havia tido um breve contato com o Jails (Solaris) e o LXC. Meus colegas já tinham visto, testado em seus home-labs, mas ninguém nunca havia usado no ambiente corporativo.

No final de 2014, aquela empresa, daquele tech lead com o papo informal, foi comprada por um concorrente e hoje, ninguém mais lembra que eles existiram, passaram-se 10 anos quase.

Dias atrás no LinkedIn, alguém compartilhou que 100% do ambiente está em kubernetes. Herança daquela empresa que compraram em 2014? Com certeza não!

T.I. precisa estar alinhado ao proposito da empresa, ao produto. A tecnologia, infraestrutura, linguagem... todas são efêmeras perante o negócio, o cliente e ao mercado.

Já passei por uma empresa na qual tudo era MySQL... sendo usado ao extremo! E todos que olhavam diziam: “precisariam usar um Exadata e algum NoSQL, segmentar alguns processos, etc.”, mas quem bancaria o custo? o tempo? Qual o ganho?

Por outro lado, por nove meses estive num ambiente que estavam migrando pela terceira vez um cluster kubernetes.

Foi mal planejado, mal desenhado e estavam pagando o preço pela aventura emocional, correndo para ajustar todo ambiente, pois havia acabado de ser comprados por outra empresa (coincidentemente, já estive em 3 empresas com processo de migração por ser vendida ou por comprar alguma outra).

A diferença entre estes dois exemplos acima? Senioridade do time! E não estou falando em conhecimento técnico, mas de compreender o contexto da tecnologia no negócio e estar alinhado com o restante da empresa.

Este é o meu “não-ensinamento” deste texto: seja conservador e também seja aventureiro e vanguardista... mas acima de tudo, entenda o negócio e a empresa.

Aquela empresa que havia saído no início de 2014? Desistiram de terceirizar, migraram seus VPS para dentro da AWS e sim, sem mudar nada… um “ECS as a VPS” com tudo que tem direito dentro… e uns anos depois, voltei para ajudá-los a adotar a nuvem da maneira certa.
---
layout: post
title: Introdução ao Microsoft Azure
date: 2017-12-13 15:31:19
categories: azure
thumbnail: intazure
tags:
  - azure
  - cloud
published: true
---

## Introdução

Diante dos avanços tecnológicos que inserem diversos desafios no nosso dia-a-dia, é preciso estar de olho nas mudanças tecnológicas, antes mesmo de sua consolidação no mercado.

A fim de auxiliar nesta questão, eu desenvolvi um guia de introdução ao Azure, a nuvem pública da Microsoft.

Você vai conhecer os principais componentes do Azure e nos próximos artigos você vai acompanhar uma série, explorando com mais detalhes cada um dos itens explicados neste guia introdutório.

Importante ressaltar que esses artigos servem como base nos estudos para a certificação [70-533](https://www.microsoft.com/pt-br/learning/exam-70-533.aspx "70-533").

## Portal de gerenciamento

O Azure possui uma interface web que nos permite administrar e gerenciar praticamente todos os recursos disponíveis, incluindo máquinas virtuais, databases, web apps, redes virtuais, etc.

Imagine um console único de fácil utilização, que além de simplificar o processo de implementação e o gerenciamento dos recursos em nuvem, também fornece a possibilidade de personalizar seu portal de acordo com as suas necessidades, por exemplo:

- Criar dashboards;
- Depois de criado, você pode compartilhar os dashboards com seus colegas de equipe;
- Inserir nomes de acordo com o seu propósito.

Veja aqui um dashboard com algumas customizações

![AzureDashboard](https://i.imgur.com/cL4gxZY.png)

## Grupo de Recursos

Esse é um dos principais recursos disponíveis no Azure no sentido de manter a casa organizada!

Um grupo de recursos (_Resource Group_) nada mais é do que um contêiner que mantém os recursos provisionados no portal de forma agrupada. O grupo de recursos (GR) pode incluir todos os recursos que você possui no Azure, ou apenas algum recurso específico, por exemplo, imagine que você possui 3 clientes diferentes dentro do mesmo portal no Azure e necessita gerenciá-los de forma organizada, tendo isso em vista, é possível criar 3 GR distintos e armazenar os recursos de cada cliente dentro do seu respectivo GR. É uma forma de manter uma certa organização em termos de recursos.

É importantíssimo ressaltar alguns fatores que devem ser levados em consideração no momento de definir um GR:

- Deletando um GR automaticamente irá deletar todos os recursos contigos nele;
- Todo e qualquer recurso só pode existir em um GR, ou seja, não é possível provisionar nenhum recurso se o mesmo não fizer parte de um GR;
- Um GR pode conter recursos de regiões diferentes;
- É possível mover um recurso para um novo GR.

## Contas de Armazenamento

As contas de armazenamento do Azure (_Storage Accounts_), são sem dúvida um dos recursos mais utilizados dentro do portal. Você sabia que os VHDs das máquinas virtuais são armazenados dentro de uma conta de armazenamento?

Esse é um dos recursos do Azure que eu pretendo escrever um artigo especifico falando de todos os detalhes, e olha que não são poucos!

Mas o que é de fato uma conta de armazenamento?

Bem, basicamente uma conta de armazenamento pode ser usada para armazenamento de blobs, tabelas, filas e arquivos de forma segura, redundante e altamente disponível.

O Azure atualmente possui 4 tipos de armazenamentos:

- Armazenamento de Blobs: blob é um tipo de dado não estruturado, ou seja, qualquer tipo de dado em formato de texto ou binário, por exemplo: um documento no formato PDF;

- Armazenamento de Tabelas: esse é um tipo de armazenamento para dados estruturados, e embora o nome seja tabela, esse recurso possui algumas características que o diferenciam de uma tabela armazenada em um banco de dados relacional;

- Armazenamento de Filas: o armazenamento de filas do Azure é um tipo de armazenamento para um conjunto de mensagens que podem ser acessadas de qualquer lugar. Uma fila possui uma série de mensagens que podem ficar no máximo 7 dias na fila;

- Armazenamento de Arquivos: nesse tipo de armazenamento, o Azure fornece um compartilhamento utilizando o protocolo SMB. Esse tipo de armazenamento nos permite conectar-se a clientes Windows ou Linux, migrar aplicações on-premise baseadas em compartilhamento de rede e por aí vai. Basicamente aqui você pode imaginar uma unidade mapeada em uma estação, só que fisicamente ela encontra-se na nuvem!

E como fica a redundância dos meus dados armazenados no Azure?

O Azure possui 4 esquemas de replicação que determinam quantas cópias dos dados serão mantidas no mesmo datacenter ou em um segundo datacenter, essa decisão vai depender da opção de replicação que você vai escolher no momento de criar a conta de armazenamento. Atualmente as opções possíveis são:

- Armazenamento com redundância local (**L**RS): armazena 3 cópias dos seus arquivos dentro do mesmo datacenter;

- Armazenamento com redundância de zona (**Z**RS): replica os dados entre datacenters de uma ou duas regiões, além de armazenar 3 réplicas dos seus arquivos, semelhante ao LRS;

- Armazenamento com redundância geográfica (**G**RS): replica 3 cópias dos dados dentro do mesmo datacenter (LRS), além de replicar para outro datacenter a milhares de quilômetros;

- Armazenamento com redundância geográfica com acesso de leitura (**RA**-**GRS**): Idem ao GRS, porém, com acesso de leitura no datacenter secundário.

Bom, eu poderia continuar escrevendo por horas sobre os conceitos e funcionalidades do armazenamento do Azure, mas isso é assunto para outra hora.

## Web Apps

O serviço de Web Apps do Azure consiste na hospedagem de aplicativos web.

Os recursos podem estar sendo executados em máquinas virtuais compartilhadas ou dedicadas, isso vai depender da sua escolha no momento de provisionar o serviço.

Em quais linguagens eu posso desenvolver meu código?

No momento o Azure suporta as seguintes linguagens: .NET, Node.js, Java, Pyhton e PHP.

E como fica a segurança do meu aplicativo ou do meu site hospedado no Azure?

Aqui você pode ficar tranquilo! O serviço de Web Apps do Azure está em conformidade com diversas normas de segurança, incluindo ISO-27001, SOC1, SOC2 e SOC3.


## Máquinas Virtuais

As máquinas virtuais são um dos principais recursos oferecidos pelo Azure.

É claro que você pode executar diversos recursos como serviço (SaaS), mas uma VM te fornece um controle maior sobre o ambiente, detalhe que muitas vezes é requisito nos projetos.

Algumas características que devem ser destacados:

- Uma VM pode ter um nome com até 15 caracteres;

- É necessário definir a localização da VM, ou seja, em que região serão armazenados os VHDs;

- Tamanho da VM: o tamanho da VM vai depender da quantidade de recursos (Memória, CPU, IOPS, etc.) que você necessita. O Azure fornece uma gama de tamanhos distintos, se adaptando a diferentes cenários.

Existem alguns recursos que são pré requisitos para provisionar uma VM, mas não se preocupe, eles podem ser criados durante o processo de provisionamento da VM.

Mas que recursos são esses?

Bem, eu comentei sobre alguns deles nos tópicos acima e a partir de agora nós vamos começar a juntas as partes!

- Grupo de Recursos: a VM deve estar contida em um GR;

- Conta de Armazenamento: a VM necessita de uma conta de armazenamento para armazenar os VHDs;

- Rede Virtual e Interface de Rede: não falei sobre esse assunto ainda, mas a VM deve fazer parte de uma rede virtual e consequentemente possuir uma interface para se comunicar na rede.

## Backup do Azure

O backup do Azure é uma das soluções que dificilmente ficará fora do seu projeto, mas porque?

Bom, simples, é um requisito de segurança, é confiável, é uma alternativa às fita e N outros motivos que eu poderia escrever aqui.

O que eu posso proteger com o backup do Azure?

- Arquivos e pastas;
- Máquinas virtuais (Hyper-V e VMware);
- SharePoint;
- Exchange;
- SQL Server;
- Máquinas virtuais do Azure;
- Estado do sistema.

É apenas para a nuvem?
Não, fique tranquilo! Você pode proteger seu ambiente local com a solução de backup do Azure.

Em breve iremos discutir com detalhes o backup do Azure e as formas de implementação.

## Conclusão

É perceptível o crescimento acelerado da computação em nuvem, mas você já está preparado para encarar essa tecnologia que vem dominando o mercado?

A Microsoft disponibiliza R$670 de créditos gratuitos no Azure! Com isso você pode provisionar máquinas virtuais, bancos de dados, enfim, o que você quiser!

Vale lembrar ainda que existem diversos serviços que podem ser utilizados gratuitamente, independente do tipo de assinatura.

Ficou interessado? Então dá uma olhada nesse [link](https://azure.microsoft.com/pt-br/free/ "link").

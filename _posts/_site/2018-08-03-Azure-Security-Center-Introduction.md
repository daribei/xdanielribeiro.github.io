---
layout: post
title: Como manter seus workloads seguros com o Azure Security Center
date: 2018-08-03 11:20:00
categories: Azure-Security-Center
thumbnail: intazure
tags:
  - azure
  - security
published: true
---

## __Introdução__

Diante dos incidentes que aconteceram nos últimos anos, é perceptível que finalmente o mercado brasileiro está amadurecendo no ponto de vista de segurança da informação.

No ano de 2017, o mundo foi surpreendido com um _malware_ chamado WannaCry. Esse _malware_ causou um prejuízo de milhares de dólares, e tudo isso poderia ter sido evitado com uma simples atualização do Windows.

Isso evidencia a necessidade de termos uma rotina de [atualização](http://xdanielribeiro.com.br/azure-updatemanagement/2018/07/12/azure-update-management/) dos nossos workloads, seja no ambiente local ou na nuvem. 

Dentro do contexto de segurança da informação, a Microsoft é o __único__ provedor de nuvem que oferece um recurso que possibilita ter visibiidade e controle sobre a segurança dos nossos workloads.

O objetivo desse artigo é explicar alguns conceitos fundamentais para dar os primeiros passos no Azure Security Center.

## __Overview do Azure Security Center__

O Azure Security Center (ASC) é um recurso disponível no Azure para toda e qualquer assinatura ativa. Ou seja, tendo um assinatura, automaticamente é habilitado o ASC na camada _free_.

Além da camada _free_, existe também a camada _standard_, que oferece uma série de mecanismos de segurança para organizações que precisam de mais controle. Falaremos mais sobre essa camada nos próximos artigos.

O primeiro passo é acessar o ASC, para isso, clique em Azure Security Center no painel lateral. Caso ele não apareça, basta utilizar o campo de pesquisa.

Durante o primeiro acesso, talvez o ASC demore alguns minutos até popular o dashboard com recursos, recomendações e demais informações.

![ASC-Overview](https://i.imgur.com/VSxTGfz.jpg)

Conforme imagem acima, o dashboard do ASC fornece informações relacionadas a 3 grupos:

- __Policy & compliance__ - Aqui você visualiza em que camada a(s) sua(s) assinatura(s) encontram-se, note que neste caso, ambas assinaturas estão na camada _free_. Além disso, você também visualiza como está a conformidade em relação as politicas de segurança, neste exemplo, estou 50% alinhado com as politicas de segurança definidas;

- __Resource security hygiene__ - Neste grupo de informações você tem uma visão geral em relação as recomendações separadas por grupo. Veremos com mais detalhes em seguida;

- __Threat protection__ - Esse grupo não está aparecendo na imagem, mas ele vem logo abaixo. Neste artigo não iremos abordar a parte de _Threat protection_.

## __Security Policy__

Recentemente o ASC teve uma grande atualização no que diz respeito as Security Policies. Agora está mais organizado e mais fácil de entender o que significa cada um dos componentes, mas mesmo assim, ainda gera um pouco de confusão.

Basicamente, as políticas de segurança __são o que definem como o ASC irá se comportar diante dos workloads__.

Atualmente as políticas de segurança do ASC possuem 4 componentes, veremos cada um deles na sequência.

### Data Collection

A coleta dos dados das suas VMs Azure e non-Azure são realizados pelo Microsoft Monitoring Agent (MMA), cujo qual, armazena os dados coletados em um _workspace_ do Log Analytics.

Por padrão, o provisionamento automático do MMA é desabilitado, porém, eu recomendo que você deixe ele no modo automático, para que toda e qualquer VM nova ou existente seja monitorada pelo ASC, ou seja, o ASC irá instalar o MMA em todas as VMs existentes, bem como em quaisquer novas VMs que forem criadas.

Para habilitar o provisionamento automático do Microsoft Monitoring Agent:

1 - Acesse o ASC e selecione _Security Policy_;

2 - Clique em _Edit Settings_ ao lado da assinatura, conforme imagem abaixo;

![ASC-DataCollection](https://i.imgur.com/koC8siB.jpg)

3 - Em _Auto Provisioning_, selecione On para habilitar o provisionamento automático e abaixo selecione um _workspace_ existente ou utilize o _workspace_ padrão criado pelo ASC. A imagem abaixo ilustra esse processo.

![ASC-DC2](https://i.imgur.com/30DC65p.jpg)

### Security policy

Talvez o componente que mais gere confusão é o __Security policy__. Quando abrimos o ASC e selecionamos __Security policy__, uma nova blade chamada __Policy Management__ será aberta, conforme imagem abaixo.

![ASC-PolicyManagement](https://i.imgur.com/8klpXyF.jpg)

Selecione a assinatura que você deseja configurar e uma nova blade será aberta, de acordo com a imagem abaixo.

![ASC-SecurityPolicy](https://i.imgur.com/TSSElw3.jpg)

Quando você configura as políticas aqui, seja uma política de _Compute and Apps_, _Network_ ou _Data_, você simplesmente está dizendo pro ASC o tipo de recomendação que você deseja receber.

Caso você queira entender a função de cada uma das políticas, veja o link nas referências.

### Email notifications

Neste componente, você deve informar os e-mails de contato e um telefone para receber notificações de segurança enviadas pelo ASC.

Para habilitar as notificações:

1 - Acesse o ASC e selecione _Security Policy_;

2 - Clique em _Edit Settings_ ao lado da assinatura;

3 - Em _E-mail notifications_ você verá as seguintes opções:

- __Security contact emails__: a Microsoft utiliza os e-mails informados neste campo para enviar notificações sobre recursos que foram comprometidos.

- __Phone number:__ assim como os e-mails cadastrados na opção acima, a Microsoft vai usar esse número para contatá-lo caso algum dos seus recursos tenha sido comprometido. Importante ressaltar que deve-se utilizar o padrão internacional, por exemplo: +5511999999999.

- __Send me emails about alerts__: desabilitando essa opção, significa que você vai receber apenas e-mails com alertas de alta criticidade.

- __Send email also to subscription owners__: habilitando essa opção, siginifica que todos os owners da assinatura irão receber alertas de alta criticidade.

A imagem abaixo exemplifica esse processo.

![ASC-Email_Notifications](https://i.imgur.com/FeefbX9.jpg)

### Pricing tier

O ASC oferece duas camadas de preço:

- __Free__: conforme explicado anteriormente, essa camada é habilitada automaticamente para todas as assinaturas do Azure, fornecendo políticas de segurança e recomendações para ajudar a proteger seus workloads.

- __Standard__: a camada standard estende os recursos da camada free para workloads em execução fora do Azure, ou seja, no seu ambiente on-premises ou em outros provedores de nuvem. Um dos principais beneficios é o gerenciamento unificado de todos os seus workloads. A camada standard também oferece recursos avançados de detecção de ameaças, que utilizam recursos de análise comportamental e machine learning para identificar ataques e zero-day exploits. Importante ressaltar que a camada standard é gratuita por 60 dias. Depois dos 60 dias, o custo é de U$ 15 por nó.

## __Recomendações__

O ASC analisa constantemente nossos workloads e quando identifica uma possível vulnerabilidade, oferece recomendações para resolver tais problemas. Todas as recomendações possuem uma severidade associada a elas, que vão desde alta prioridade até baixa prioridade, o que facilita a decisão sobre quais recomendações devem ser atendidas primeiro.

Para visualizar as recomendações:

1 - Acesse o ASC e selecione _Recommendations_, conforme imagem abaixo.

![ASC-Recommendations](https://i.imgur.com/SLUw4KZ.jpg)

Algumas recomendações necessitam de um reboot para serem aplicadas, por exemplo, recomendações sobre atualizações do Windows.

A Microsoft recomenda que no primeiro momento, todas as recomendações de alta prioridade sejam atendidas.

Atualmente, as recomendações são divididas em 3 grupos:

- __Compute & apps__: as recomendações de computação são para VMs Azure e non-Azure e incluem diversos tipos de recomendações, por isso, é importante entender a [lista](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations#what-are-security-recommendations) completa de recomendações que podem ser aplicadas nos workloads. As recomendações de Aplicativos são voltadas para workloads que possuem aplicações web rodando em máquinas virtuais no Azure.
Na maioria dos casos, a Microsoft vai recomendar que você utilize um Web Application Firewall (WAF). Veja na imagem abaixo algumas recomendações do grupo _Compute & apps_.

![ASC-Compute_and_Apps](https://i.imgur.com/oZCcpWK.jpg)

- __Networking__: As recomendações de _Networking_ são voltadas para recursos que melhoram a segurança dos workloads em relação a parte de redes, por exemplo:

- Configurar um NSG para Sub-redes;
- Configurar regras no NSG para restringir o acesso aos seus recursos a partir da internet;
- Adicionar um NGFW no ambiente.

As recomendações podem variar dependendo do ambiente, por isso, novamente, é extremamente importante que você entenda a lista completa de recomendações.

- __Data & storage__: As recomendações de Dados são voltadas para contas de armazenamento e Azure SQL.
Aqui você verá recomendações para criptografar contas de armazenamento (hoje as contas de armazenamento são criptografadas por padrão), habilitar auditoria nas suas bases de dados e assim por diante. Deixarei o link nas referências com a lista completa de recomendações

## __Conclusão__

Existem diversos recursos disponíveis no Azure Security Center, principalmente na versão Standard.

O objetivo desse artigo foi fazer uma introdução e explicar alguns conceitos importantes para quem ainda não está familiarizado com o ASC.

Nos próximos artigos pretendo explorar alguns recursos específicos do ASC, por exemplo, _Just in time VM access_.

Se esse conteúdo fez sentido pra você, escreve nos comentários pra mim saber se estou te ajudando de alguma forma.

## Referências 

[Available security policy definitions](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies#available-security-policy-definitions)

[Protecting your machines and applications in Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-virtual-machine-recommendations)

[Protecting your network in Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-network-recommendations)

[Protecting Azure SQL service and data in Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-sql-service-recommendations)

[Livro do Yuri Diogenes e Tom Shinder - Microsoft Azure Security Center](https://www.microsoftpressstore.com/store/microsoft-azure-security-center-9781509307036)

Obrigado,

### Daniel Ribeiro.
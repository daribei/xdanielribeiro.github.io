---
layout: post
title: Adotando a solução de gerenciamento de atualizações para VMs Azure e non-Azure
date: 2018-07-12 13:30:19
categories: Azure-UpdateManagement
thumbnail: intazure
tags:
  - azure
published: true
---

## Introdução-101

O objetivo desse artigo é explicar e demonstrar o funcionamento do Azure Update Management, também conhecido como Gerenciador de Atualizações. Esse recurso nos faz lembrar do _Windows Server Update Services_ (WSUS), porém com algumas diferenças importantes que veremos na sequência.

Diferente do WSUS, com o Azure Update Management, é possível gerenciar atualizações tanto para Windows quanto para Linux, e ainda por cima, os servidores podem estar rodando em um ambiente on-premises ou em outros provedores de nuvem, por exemplo, Amazon Web Services (AWS). Sim, é isso mesmo, você pode controlar as atulizações de servidores que não estão no Azure.

## Quais recursos são necessários para configurar o Update Management?

Precisamos de apenas dois recursos para possibilitar a implementação do Update Management. O primeiro recurso é uma conta de automação, em inglês, _automation account_ e também de um _workspace_ do Log Analytics.

Antes de habilitar o Update Management, é importante entender como funciona os componentes necessários para essa solução.

Basicamente, o Log Analytics armazena dados coletados em um _workspace_ (espaço de trabalho) e a conta de automação nos permite gerenciar as atualizações, além de diversos outros processos.

## Quais são os custos relacionados com o Update Management?

O Update Management não tem custo nenhum, é 100% gratuito!
O único custo envolvido é do volume de dados armazenado no Log Analytics.

[_Update management includes visibility and deployment of updates in your environment. There are no charges for the service, you only pay for log data stored in the Azure Log Analytics service._](https://azure.microsoft.com/en-us/pricing/details/automation/ "Automation pricing ")

## Entendendo o funcionamento do Update Management

No momento em que o Update Management é habilitado em um VM, automaticamente é feito a instalação de um agente chamado _Microsoft Monitoring Agent_ (MMA). Esse agente é responsável por coletar informações relacionadas ao grau de atualização do servidor e encaminhar para o Log Analytics. Por padrão, essa verificação ocorre a cada 12 horas para servidores Windows e a cada 3 horas para servidores Linux. 

Depois de encaminhar os dados coletados para o Log Analytics, o Update Management lista as atualizações que estão pendentes de serem instaladas nas VMs. Atualmente não existe um "botão" para instalar as atualizações imediatamente, deve-se criar um agendamento especificando o horário, tipos de atualização (_Critical_, _Service Packs_, etc.), VMs e o tempo disponível para a janela de manutenção.

No horário do agendamento, o agente (MMA) executa uma nova verificação para verificar se as atualizações ainda são necessárias, ou seja, se as atualizações foram instaladas manualmente antes da última coleta do agente, o Update Management simplesmente irá ignorar e informar que a VM está atualizada.

De forma bem resumida, é assim que funciona o Update Management.

## Configurando o Update Management

Quando clicamos em uma máquina virtual no portal do Azure, visualizamos diversas opções disponíveis, por exemplo: size da VM, discos, métricas e assim por diante... Uma das opções disponíveis é justamente o Update Management, conforme imagem abaixo.

![UpdateManagementVM](https://i.imgur.com/5b49EXf.jpg)

Embora pareça (e é) ser extremamente simples habilitar esse recurso, eu sugiro que essa implementação seja feita por etapas, ou seja, primeiro configurar o _workspace_ do Log Analytics e depois criar a conta de automação. Caso contrário, esses dois recursos serão criados com nomes aleatórios, deixando o portal desorganizado :).

### Criando um _workspace_ do Log Analytics

A primeira etapa é configurar um _workspace_ do Log Analytics. 

1 - No portal do Azure, clique em Todos os serviços. Na lista de recursos, digite Log Analytics.

![LogAnalytics](https://i.imgur.com/a3lVVoy.png)

2 - Clique em Criar e, em seguida, selecione opções para os seguintes itens:

* Forneça um nome para o novo _workspace_;
* Selecione uma Assinatura;
* Crie um novo grupo de recursos ou utilize um existente;
* Selecione uma região disponível;
* Selecione uma camada de preços.

Atualmente o Log Analytics não está disponível no Brasil, portanto, sugiro que seja criado no Leste dos Estados Unidos (_east us_).

![CreatingWorkspace](https://i.imgur.com/bvycl5z.jpg)

3 - Depois de fornecer as informações necessárias no painel, clique em OK.

Enquanto as informações são verificadas e o _workspace_ é criado, podemos partir pra próxima etapa, isto é, criar a conta de automação.

### Criando uma Conta de Automação

Antes de criar a conta de automação, é importante ressaltar que esse recurso nos possibilita executar diversas tarefas de forma automatizada, por exemplo:

*  Automação de processos através de _Runbooks_, ou seja, scripts que são executados em ambientes Azure e Non-Azure;
* Gerenciamento de configuração através do PowerShell DSC.

De forma resumida, a conta de automação do Azure é composta por uma série de recursos que facilitam a automatização e configuração do ambiente.

Se você está pensando em desligar suas máquinas virtuais fora de horário comercial para economizar $$, com certeza irá precisar da conta de automação para executar os _runbooks_ que farão esse trabalho pra você.

Bom, vamos ao que interessa.

1 - No portal do Azure, clique em Todos os serviços. Na lista de recursos, digite Automation Account.

2 - Clique em Criar e, em seguida, selecione opções para os seguintes itens:

* Forneça um nome para a nova conta de automação;
* Selecione uma Assinatura;
* Crie um novo grupo de recursos ou utilize um existente;
* Selecione uma região disponível;

Conforme visto no tópico anterior, atualmente o Log Analytics não está disponível no Brasil, mas a conta de automação sim, porém, para que o Update Manamgement funcione corretamente, a conta de automação e o _workspace_ devem estar na mesma região.

![CreatingAA](https://i.imgur.com/Qfm8cu6.jpg)

3 - Depois de fornecer as informações necessárias no painel, clique em criar.


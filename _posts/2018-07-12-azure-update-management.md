---
layout: post
title: Adotando a solução do Update Management para VMs Azure e non-Azure
date: 2018-07-12 13:30:19
categories: Azure-UpdateManagement
thumbnail: intazure
tags:
  - azure
  - updatemanagement
  - wsusasaservice
published: true
---

## Introdução

O objetivo desse artigo é explicar e demonstrar o funcionamento do Azure Update Management, também conhecido como Gerenciador de Atualizações. Esse recurso nos faz lembrar do _Windows Server Update Services_ (WSUS), porém com algumas diferenças importantes que veremos na sequência.

Diferente do WSUS, com o Azure Update Management, é possível gerenciar atualizações tanto para Windows quanto para Linux, e ainda por cima, os servidores podem estar rodando em um ambiente on-premises ou em outros provedores de nuvem, por exemplo, Amazon Web Services (AWS). Sim, é isso mesmo, você pode controlar as atulizações de servidores que não estão no Azure.

## Quais recursos são necessários para configurar o Update Management?

Precisamos de apenas dois recursos para possibilitar a implementação do Update Management. O primeiro recurso é uma conta de automação, em inglês, _automation account_ e também de um _workspace_ do Log Analytics.

Antes de habilitar o Update Management, é importante entender como funciona os componentes necessários para essa solução.

Basicamente, o Log Analytics armazena os dados coletados em um _workspace_ (espaço de trabalho) e a conta de automação nos permite gerenciar as atualizações, além de diversos outros processos.

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
* Selecione uma região disponível.

Conforme visto no tópico anterior, atualmente o Log Analytics não está disponível no Brasil, mas a conta de automação sim, porém, para que o Update Manamgement funcione corretamente, a conta de automação e o _workspace_ devem estar na mesma região.

![CreatingAA](https://i.imgur.com/Qfm8cu6.jpg)

3 - Depois de fornecer as informações necessárias no painel, clique em criar.

### Habilitando o Update Management

Depois de criar o _workspace_ e a conta de automação, já estamos aptos a habilitar o Update Management.

1 - No portal do Azure, abra a conta de Automação recém criada e selecione Update Management ou Gerenciamento de Atualizações, se estiver em português;

2 - Selecione opções para os seguintes itens:

* Selecione uma localização. Note que neste caso, a localização será definida de acordo com a região em que foi criado a conta de automação;
* Selecione uma Assinatura;
* Selecione o _workspace_ do Log Analytics criado anteriormente;
* Selecione a conta de automação que estará conectada ao _workspace_ selecionado na opção anterior.

![EnablingUpdateManagement](https://i.imgur.com/2VjlLQR.jpg)

Assim que concluído essa etapa, já podemos adicionar VMs no Update Management.

### Adicionando VMs no Update Management

1 - No portal do Azure, abra a conta de Automação e selecione Update Management ou Gerenciamento de Atualizações, se estiver em português;

2 - Selecione _Add Azure VMs_;

![AddAzureVMs](https://i.imgur.com/xFK76XB.jpg)

3 - Selecione a região onde suas VMs encontram-se, o Grupo de Recursos e selecione as VMs que você deseja habilitar o Update Management;

![AddAzureVMsUM](https://i.imgur.com/9Ut5J11.jpg)

4 - Feito isso, automaticamente será feito a instalação do _Microsoft Monitoring Agent_ na VM e após alguns minutos/horas, as informações serão exibidas no Update Management.

Depois de habilitado, a VM será exibida no Update Management. Vejam na imagem abaixo que o status atual da VM é _Compliant_. 

![UM-VM_Compliant](https://i.imgur.com/FuJSRXg.jpg)

Atualmente existem 3 status possíveis:

* _Compliant_: servidores com todas as atualizações críticas ou de segurança;
* _Non-compliant_: servidores que não têm pelo menos uma atualização crítica ou de segurança;
* _Not assessed_: o agente não reportou os dados com o grau de atualização do servidor.

### Configurando o agendamento das atualizações

Ainda não existe a possibilidade de aplicar as atualizações imediatamente, ou seja, clicar num "botão" que irá iniciar a instalação das atualizações, porém, acredito que em um futuro próximo essa funcionalidade estará disponível.

Por enquanto, a única forma de aplicar as atualizações é através do agendamento. 

Para agendar uma instalação de atualizações para os servidores, clique em  _Scheduled update deployments_, conforme imagem a seguir.

![ScheduleUpdates](https://i.imgur.com/al5VLhq.jpg)

No painel, insira as seguintes informações:

* Nome: insira um nome exclusivo para identificar a implantação de atualizações;
* Sistema operacional: selecione Windows ou Linux;
* Machines to update (Computadores a atualizar): selecione as máquinas virtuais que você deseja atualizar;
* Update Classifications (Classificação das atualizações): selecione o tipo de atualização que você quer incluir. Os tipos de classificação são:

  + Critical updates
  + Security updates
  + Update rollups
  + Feature packs
  + Service packs
  + Definition updates
  + Tools
  + Updates

* Updates to exclude (Atualizações a serem excluídas): caso selecionada, insira o número do KB;

* Schedule settings: especifique o agendamente para as atualizações;

* Maintenance Window (Janela de Manutenção): especifique o tempo em que você deseja que a instalação das atualizações ocorra. Importante ressaltar que, caso a atualização necessite de um _restart_, a VM será reiniciada automaticamente.

![NewUpdateSchedule](https://i.imgur.com/7k5bL4g.jpg)

### Visualizando os resultados da instalação

Para visualizar o status das atualizações, clique em _Update deployments_, conforme imagem abaixo.

![UpdateDeployment](https://i.imgur.com/vmRA93Y.jpg)

Se a instalação estiver em andamento, o status será _In progress_ e se a instalação estiver concluída com sucesso, o status será alterado para _Succeeded_. Se algum update falhar, o status será _Partially failed_.

Para obter mais detalhes em relação as atualizações que foram instaladas ou que falharam, basta clicar no nome do agendamento, neste caso, "Weekly Update Deployment". Ao clicar, será aberto um novo painél, com diversas informações relevantes.

![UpdateInformation](https://i.imgur.com/O9xIyYm.jpg)

## Conclusão

Acredito que na maioria dos cenários rodando no Azure, o Update Management certamente substituirá o WSUS.

Espero ter te ajudado a entender o funcionamento do Azure Update Management.

## Referências 

[Update Management overview](https://docs.microsoft.com/en-us/azure/automation/automation-update-management)

[Manage updates for multiple machines](https://docs.microsoft.com/en-us/azure/automation/manage-update-multi)

Obrigado,

### Daniel Ribeiro.
---
layout: post
title: Como diagnosticar e resolver problemas em máquinas virtuais no Azure?
date: 2018-07-23 14:22:12
categories: Azure-Troubleshooting
thumbnail: intazure
tags:
  - azure
  - troubleshooting
published: true
---

## Introdução

Ultimamente a Microsoft melhorou e muito a capacidade de diagnosticar e resolver problemas de VMs Windows e Linux no Azure. Isso era algo muito esperado, até porque, nós como clientes, não temos acesso ao backend, isto é, a infraestrutura onde os _workloads_ são executados.

Se você ainda não precisou fazer um troubleshooting em alguma VM no Azure, pode ter certeza que mais cedo ou mais tarde vai precisar, e não é porque a plataforma tem algum tipo de problema recorrente, mas sim, porque isso acontece, rs.

O Azure possui diversas formas de diagnosticar e resolver problemas. Vou explicar as principais delas e tenho certeza que serão bastante úteis durante o troubleshooting.

## Boot Diagnostics

O Boot Diagnostics (Diagnóstico de inicialização) é o principal recurso utilizado para diagnosticar problemas durante a inicialização de uma máquina virtual Windows e Linux.

Se a máquina virtual não iniciou ou está demorando muito tempo para iniciar, esse recurso com certeza irá te ajudar a entender o que está acontecendo com a VM naquele momento.

Veja na imagem abaixo, um exemplo do boot diagnostics.
Neste caso, a VM simplesmente estava na tela de logon, sem problema algum.

![BootDiagnostics](https://i.imgur.com/fBET5S6.jpg)

Caso você tenha a necessidade de baixar a imagem e enviar pro seu cliente, basta clicar em _Download screenshot_ e será feito o download de um arquivo .bmp. Você também tem a possibilidade de atualizar a console e acompanhar o que está acontecendo.

Além do _screenshot_, você também pode visualizar e baixar o log de inicialização das máquinas virtuais. Clicando em _Serial log_, você vai visualizar o log de boot da VM e caso necessário, pode efetuar o download do mesmo clicando em _Download serial log_.

Importante ressaltar que, o _screenshot_ e o arquivo de log ficam salvos em uma conta de armazenamento.

### Como configurar o Boot Diagnostics?

Esse recurso é ativado por padrão quando criamos uma máquina virtual através do Portal do Azure, ou seja, se você criou uma nova VM e não alterou as configurações padrões durante o provisionamento, não precisa se preocupar.

Caso você não tenha habilitado o diagnóstico de inicialização, pode escolher uma das seguintes formas para habilitar o mesmo: Portal do Azure, PowerShell, Azure CLI e ARM template.

Para habilitar via PowerShell, execute os comandos abaixo, substituindo os valores associados as variáveis.

```
$vmName = "Nome-da-VM"
$rgName = "Nome-do-Grupo-de-Recursos"
$stgAccountName = "Nome-da-Conta-de-Armazenamento"

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName

Set-AzureRmVMBootDiagnostics -VM $vm -ResourceGroupName $rgName -StorageAccountName $stgAccountName -Enable
```

Para habilitar via Portal do Azure, siga os seguintes passos:

1 - Clique na máquina virtual, desce até o final e clique em Boot Diagnostics ou Diagnóstico de Inicialização, se estiver em português;

2 - Em Settings/Configurações, habilite o Diagnóstico de inicialização, em seguida, selecione uma conta de armazenamento que deseja salvar os arquivos de diagnóstico.

![BootDiagnosticsConfig](https://i.imgur.com/jjMMzxV.jpg)

Bom, agora que você conhece o Boot Diagnostics, eu te faço a seguinte pergunta: se você visualizar através do diagnóstico de inicialização que sua VM não está iniciando devido a um erro de configuração no arquivo FSTAB, o que você pode fazer pra resolver esse problema?

Resposta: _Serial Console_! 

## Serial Console

Serial console é um excelente recurso que ainda está em _preview_, porém, já podemos usufruir do mesmo no Brasil.
Esse recurso fornece acesso baseado em texto para VMs Windows e Linux, independentemente do estado de rede ou sistema operacional da máquina virtual, em outras palavras, tanto faz se a VM não está respondendo a ping ou se o sistema operacional não está iniciando por algum motivo específico.

Atualmente, o acesso via serial console está disponível apenas através do Portal do Azure e somente para usuários que possuem a função de colaborador (ou superior) da VM e da conta de armazenamento configurada no diagnóstico de inicialização.

Alguns detalhes importantes:

* O acesso é realizado através da porta serial COM1 da VM;
* A máquina virtual __deve__ ter o diagnóstico de inicialização habilitado.

### Como abrir o Serial Console?

1 - Abra o portal do Azure;

2 - Selecione a sua máquina virtual;

3 - Role até a seção _Support + troubleshooting_ e clique na opção _Serial console (Preview)_;

4 - Feito isso, um novo painél será aberto e a conexão será iniciada;

5 - Insira suas credenciais.

Se deu tudo certo, você terá acesso a sua máquina virtual, conforme imagem abaixo.

![LinuxSerialConsole](https://i.imgur.com/i5YWuT8.jpg)

Se a sua VM é Windows, você verá que o serial console é um pouco diferente do Linux, onde nós apenas informamos o usúario e senha. No caso de VMs Windows, o serial console conecta-se a VM utilizando o _Special Administration Console_ (SAC), que nada mais é do que uma ferramenta de linha de comando existente desde o Windows Server 2003.

Ao abrir o serial console de uma VM Windows, visualizamos logo de cara o SAC, conforme imagem abaixo.

Apenas digite _help_ e veja uma lista de opções disponíveis.

![SerialConsole](https://i.imgur.com/i7pCaDV.jpg)

Se nenhuma dessas opções te atender, você pode abrir um prompt de comando, para isso, apenas digite ___cmd___ e em seguida digite ___ch -sn Cmd0001___. Você será solicitado a autenticar e, em seguida, será apresentado um prompt de comando.

##  Resource health

Resource health é um daqueles recursos que podem te ajudar a economizar tempo durante o troubleshooting. O resource health está o tempo todo de olho nos seus recursos, mas com o intuito de garantir que tudo esteja funcionando conforme o esperado, e no caso de problemas, fornece informações que podem te ajudar a entender e resolver o problema. Em outras palavras, ele informa como está a integridade dos seus recursos, fornecendo um histórico de integridade de 14 dias.

Para abrir o resource health, siga os seguintes passos:

1 - Acesse o portal do Azure;

2 - Abra um recurso, por exemplo, uma VM;

3 - Role até a seção _Support + troubleshooting_ e clique em _Resource health_.

Feito isso, você verá logo de cara o status atual da VM, conforme imagem abaixo.

![ResourceHealth](https://i.imgur.com/QlW4r6k.jpg)

Além do status atual, também é possível visualizar o histórico de integridade das últimas duas semanas.

Esse histórico é bastante útil, principalmente quando precisamos entender o que aconteceu com nossa VM em um momento em que não estávamos presentes.

Para exemplificar, veja na imagem abaixo um evento que ocorreu comigo neste sábado, 21/07. Minha VM reiniciou e eu não sabia o porque. Na segunda-feira olhei o resource health dela e identifiquei que houve uma falha no host onde ela estava sendo executada, o que resultou em um redeploy. Esse problema também é conhecido como tempo de inatividade inesperado, [leia mais aqui](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability)

![ResourceHealth!](https://i.imgur.com/B2MbBRg.jpg)

É importamte saber também que, o resource health pode apresentar diversos status, e, cada um tem um significado diferente, deixarei o link com mais informações nas referências.

## Run Command

Esse recurso saiu de _preview_ a pouco tempo e tem o seu valor. O Run Command, basicamente nos permite executar comandos em VMs Windows e Linux sem ter que se conectar remotamente na máquina virtual.

Por padrão, ele já trás alguns scripts que podem nos ajudar a resolver diversos problemas, por exemplo: 

* Alterar a senha de administrador;
* Verificar as configurações de rede;
* Executar scripts personalizados em PowerShell ou ShellScript.

Para executar um comando em uma VM, através do portal do Azure, selecione sua máquina virtual e clique em __Run Command__, seguindo a imagem abaixo.

![RunCommand](https://i.imgur.com/DJ2zTRf.jpg)

Esse recurso tem apenas um requisito para funcionar: o Azure VM Agent __deve__ estar instalado.

## Redeploy

Em alguns momentos você vai precisar fazer um Redeploy da sua máquina virtual. Isso normalmente é necessário quando não estamos conseguindo acessar a VM remotamente ou quando estamos enfretando problemas relacionados a uma aplicação que está sendo executada. 

Na semana passada, a máquina virtual de um cliente estava inacessível. O Resource health não estava reportando nenhum problema, porém, quando acessei o diagnóstico de inicialização, a VM estava travada na tela de inicialização do Windows. Reiniciei a VM pelo portal e o problema continuou. Neste caso, fiz um Redeploy da VM e o problema foi resolvido.

Mas afinal, o que é um Redeploy?

Quando você reimplanta uma VM, isto é, executa o redeploy, a VM é movida para outro host. Esse procedimento gera um _downtime_ de alguns minutos.

Para reimplantar uma VM através do PowerShell, execute o seguinte comando:

```
Set-AzureRmVM -Redeploy -ResourceGroupName "Nome-do-Grupo-de-Recursos" -Name "Nome-da-VM"
```

Se preferir, podes usar o Portal do Azure, para isso, selecione sua VM, role até a seção _Support + troubleshooting_ e clique em __Redeploy__, conforme imagem abaixo.

![Redeploy](https://i.imgur.com/nBNbaaO.jpg)

## Conclusão

Existem diversos recursos disponíveis no Azure para nos ajudar no troubleshooting.

Neste artigo, eu citei os principais, porém, não se limita a isso.

Se esse conteúdo fez sentido pra você, escreve nos comentários pra mim saber se estou te ajudando de alguma forma.

Para mais informações acesse: 

[How to use boot diagnostics to troubleshoot virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/boot-diagnostics)

[Virtual machine serial console (preview)](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/serial-console)

[Azure Resource Health overview](https://docs.microsoft.com/en-us/azure/service-health/resource-health-overview#health-status)

[Run PowerShell scripts in your Windows VM with Run Command](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/run-command)

[Run shell scripts in your Linux VM with Run Command](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/run-command)

[Redeploy virtual machine to new Azure node](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/redeploy-to-new-node)

Obrigado,

### Daniel Ribeiro.
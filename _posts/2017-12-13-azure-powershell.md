---
layout: post
title: Provisionando uma VM com o Azure PowerShell
date: 2017-12-22 10:41:19
categories: Azure-PowerShell
thumbnail: intazure
tags:
  - azure
  - powershell
published: true
---

## Introdução

Seja muito bem vindo ao meu blog.

O tema de hoje é __segura minha cerveja enquanto eu provisiono teu ambiente__. É isso mesmo, rsrs.

Bom pessoal, existe uma tendência muito grande em relação a utilização de CLI no futuro. Muito se fala que cada vez **menos** será utilizado interface gráfica para realizar as atividades cotidianas.

Tendo isso em vista, devemos desde já, nos preparar e utilizar ferramentas de linha de comando no nosso dia a dia.

O Azure possuiu duas grandes ferramentas de linha de comando, afinal, um grande provedor de núvem precisa de excelentes ferramentas e é por isso que eu vou apresentar alguns comandos básicos do Azure PowerShell. 

Lembrando que a Microsoft também disponibiliza o Azure CLI, outra ferramenta de linha de comando espetacular, compatível com macOS, Linux e Windows.

O objetivo desse artigo é provisionar um ambiente no Azure através do PowerShell.

## Mãos a Obra

Antes de mais nada, é necessário [instalar](https://docs.microsoft.com/pt-br/powershell/azure/install-azurerm-ps?view=azurermps-5.0.0 "instalar") o módulo do Azure PowerShell na sua máquina, ou se preferir, é possível utilizar o seu navegador com o Azure [CloudShell (assunto do próximo artigo)](https://docs.microsoft.com/pt-br/azure/cloud-shell/overview "CloudShell").

Se fossemos provisionar uma VM no Azure, quais recursos seriam necessários para conseguirmos atingir esse objetivo?

Basicamente:
- Rede Virtual;
- Interface de rede;
- Firewall.
- IP Público (opcional);

O primeiro passo é definir algumas váriaveis que serão utilizadas no decorrer da implementação.
Vamos nessa?

```
$resourceGroupName = "GR-DanielRibeiro"
$location = "brazilsouth"
$vmName = "Lab-DanielRib"
```

Feito isso, já podemos iniciar a brincadeira.

Vamos começar efetuando o __login__ e em seguida vamos criar um grupo de recursos. Lembre-se que sem ele nós não conseguimos nem dar o primeiro passo.
No meu artigo de [introdução ao Azure](http://xdanielribeiro.com.br/azure/2017/12/13/introducao-ms-azure/ "Introdução ao Microsoft Azure") eu falo sobre isso.

```
Login-AzureRmAccount
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
```

Para vizualisar se o Grupo de Recursos foi criado, executamos:
```
Get-AzureRmResourceGroup -Location $location
```

Seguindo, agora vamos utilizar o cmdlet __Get-Credential__ para criar um objeto contendo o usuário e senha da VM. 

```
$credenciais = Get-Credential -Message "Insira o usuário e senha para a máquina virtual"
```
Após executar o comando, abrirá um prompt solicitando as informações.


![AzureCredentials](https://i.imgur.com/TpjcDp3.jpg)


Não se preocupe, sua senha não ficará exposta dentro da variável $credenciais.

Continuando, vamos criar uma rede virtual e uma sub-rede. Para ilustrar melhor o cenário, vamos imaginar que a nossa rede virtual terá um bloco CIDR **172.16.0.0/16** e a sub-rede terá um bloco CIDR **172.16.1.0/24**.

### Criando a configuração da Sub-Rede:

```
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name "Subnet-Producao" -AddressPrefix 172.16.1.0/24
```

### Criando a Rede Virtual e associando a sub-rede:
 
```
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroupName -Location $location `
-Name VNet-DanielRibeiro -AddressPrefix 172.16.0.0/16 -Subnet $subnetConfig
```

Pronto! Já temos nossa VNet criada e também a nossa sub-rede. No próximo passo vamos criar um IP Público, embora este recurso seja opcional, neste caso.

```
$publicIP = New-AzureRmPublicIpAddress -Name "DrPublicIP" -ResourceGroupName $resourceGroupName `
-Location $location -AllocationMethod Dynamic -DomainNameLabel "danriblab"
```

Note que foi utilizado o parâmetro **-DomainNameLabel**, esse cara é utilizado para definir um nome DNS para o nosso IP Público, neste caso: danriblab.brazilsouth.cloudapp.azure.com.

Já estamos na metade do caminho. Criamos o Grupo de Recursos, a Rede Virtual, Sub-Rede e o IP Público.

No próximo passo, vamos criar um Grupo de Segurança de Rede (NSG), também conhecido como Firewall.

Primeiramente, vamos criar o NSG:

```
$nsg = New-AzureRmNetworkSecurityGroup -Name "NSG-DanielRibeiroLab" -ResourceGroupName $resourceGroupName -Location $location
```

Em seguida vamos criar uma regra, liberando o acesso remoto na VM:

```
Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
-Name Libera-RDP `
-Description "Libera RDP para internet" `
-Access Allow `
-Protocol Tcp `
-Direction Inbound `
-Priority 100 `
-SourceAddressPrefix * `
-SourcePortRange * `
-DestinationAddressPrefix * `
-DestinationPortRange 3389
```

Por último vamos salvar as configurações:

```
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
```

Agora que possuímos um IP público e um Grupo de Segurança da Rede (NSG), vamos criar uma interface de rede e associá-la ao NSG e também ao IP Público.

```
$nic = New-AzureRmNetworkInterface -Name ($vmName.ToLower()+'_nic') -ResourceGroupName $resourceGroupName -Location $location `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id -NetworkSecurityGroupId $nsg.Id
```

Pronto, já temos todos os recursos necessários para provisionar uma VM! O último passo é de fato a criação da máquina virtual.

Iniciamos definindo o nome da VM e o tamanho da mesma:

```
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D2_v2_Promo
```

Como podemos descobrir o tamanho das instâncias disponíveis na nossa região?

Simples:

```
Get-AzureRmVmSize -location $location
```

Em seguida, setamos algumas informações referentes ao sistema operacional:

```
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName $vmName `
    -Credential $credenciais
```

Definimos a imagem que será utilizada para provisionar a VM:

```
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Adicionamos na VM a interface de rede criada anteriormente:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Inserimos as informações do disco do Sistema Operacional da VM:

```
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name "OSDisk" `
    -DiskSizeInGB 128 `
    -CreateOption FromImage
```
Por último, executamos o cmdlet para criar a VM:

```
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroupName -Location $location
```

## Conclusão

Automatizar tarefas de infraestrutura é o ponto chave do DevOps. Alguns preferem chamar isso de Infraestrutura como código (IaC). 

Escrever scripts para automatizar tarefas manuais que os times de infraestrutura e operações executam manualmente é uma das grandes sacadas do DevOps.

A mensagem que eu queria deixar pra vocês é a seguinte: **aprendam** CLI! A tendência é cada vez menos utilizarmos interface gráfica.

O script está disponível no meu [GitHub](https://github.com/xdanielribeiro/azurepowershell/blob/master/CreateVirtualMachineFromAzurePowerShell.ps1).


Obrigado,

### Daniel Ribeiro.


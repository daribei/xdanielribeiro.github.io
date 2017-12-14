---
layout: post
title: Azure PowerShell - Simples, rápido e fácil*
date: 2017-12-13 16:41:19
categories: azure
thumbnail: intazure
tags:
  - azure
  - powershell
published: true
---

## Introdução

Seja muito bem vindo ao meu blog.

O tema de hoje é __segura minha cerveja enquanto eu provisiono teu ambiente__. É isso mesmo, rsrs.

Bom pessoal, existe uma tendência muito grande em relação ao futuro. Muito se fala que cada vez **menos** será utilizado interface gráfica para realizar as atividades cotidianas.

Tendo isso em vista, devemos desde já, nos preparar e utilizar ferramentas de linha de comando no nosso dia a dia.

O Azure possuiu duas grandes ferramentas de linha de comando, afinal, um grande provedor de núvem precisa de excelentes ferramentas e é por isso que eu vou apresentar alguns comandos básicos do Azure PowerShell. 

Lembrando que a Microsoft também disponibiliza o Azure CLI, outra ferramenta de linha de comando espetacular!

O objetivo desse artigo é provisionar um ambiente no Azure através do PowerShell.

## Mãos a Obra

Antes de mais nada, é necessário [instalar](https://docs.microsoft.com/pt-br/powershell/azure/install-azurerm-ps?view=azurermps-5.0.0 "instalar") o módulo do Azure PowerShell na sua máquina, ou se preferir, é possível utilizar o seu navegador com o Azure [CloudShell (assunto do próximo artigo)](https://docs.microsoft.com/pt-br/azure/cloud-shell/overview "CloudShell").

Se fossemos provisionar uma VM no Azure, quais recursos seriam necessários para conseguirmos atingir esse objetivo?

Basicamente:
- Rede Virtual;
- Interface de rede;
- Conta de armazenamento;
- Firewall.
- IP Público (opcional);

O primeiro passo é definir algumas váriaveis que serão utilizadas no decorrer da implementação.
Vamos nessa?

```
PS C:\> $resourceGroupName = "GR-DanielRibeiro"
PS C:\> $location = "brazilsouth"
PS C:\> $vmName = "Azure-Lab-DanielRibeiro"
```

Feito isso, já podemos começar a brincadeira.

Vamos começar efetuando o __login__ e em seguida vamos criar um grupo de recursos. Lembre-se que sem ele nós não conseguimos nem dar o primeiro passo.
No meu artigo de [introdução ao Azure](http://xdanielribeiro.com.br/azure/2017/12/13/introducao-ms-azure/ "Introdução ao Microsoft Azure") eu falo sobre isso.

```
PS C:\> Login-AzureRmAccount
PS C:\> New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
```

Para vizualisar se o Grupo de Recursos foi criado, executamos:
```
PS C:\> Get-AzureRmResourceGroup -Location $location
```

Seguindo, agora vamos utilizar o cmdlet __Get-Credential__ para criar um objeto contendo o usuário e senha da VM. 

```
PS C:\> $credenciais = Get-Credential -Message "Insira o usuário e senha para a máquina virtual"
```
Após executar o comando, abrirá um prompt solicitando as informações.


![AzureCredentials](https://i.imgur.com/TpjcDp3.jpg)


Não se preocupe, sua senha não ficará exposta dentro da variável $credenciais.

Continuando, vamos criar uma rede virtual e uma sub-rede. Para ilustrar melhor o cenário, vamos imaginar que a nossa rede virtual terá um bloco CIDR **172.16.0.0/16** e a sub-rede terá um bloco CIDR **172.16.1.0/24**.

### Criando a Rede Virtual:

```
PS C:\> $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroupName -Name "VNet-DanielRibeiro" `
-AddressPrefix 172.16.0.0/16 -Location $location
```

### Criando a Sub-Rede e adicionando na Rede Virtual:
 
```
PS C:\> Add-AzureRmVirtualNetworkSubnetConfig -Name "Subnet-Producao" -VirtualNetwork $vnet -AddressPrefix 172.16.1.0/24
```

Saída esperada:

```
Name                   : VNet-DanielRibeiro
ResourceGroupName      : GR-DanielRibeiro
Location               : brazilsouth
Id                     : /subscriptions/50e187c6-50e1-187c-31e1-1018329a22f1/resourceGroups/GR-DanielRibeiro/providers/
                         Microsoft.Network/virtualNetworks/VNet-DanielRibeiro
Etag                   : W/"675bab22-nh1c-472f-a1tc-cfb25630879n"
ResourceGuid           : ec7345d9-a5e4-4ca0-bb11-eb2b6e895129
ProvisioningState      : Succeeded
Tags                   :
AddressSpace           : {
                           "AddressPrefixes": [
                             "172.16.0.0/16"
                           ]
                         }
DhcpOptions            : {}
Subnets                : [
                           {
                             "Name": "Subnet-Producao",
                             "AddressPrefix": "172.16.1.0/24"
                           }
                         ]
VirtualNetworkPeerings : []
EnableDDoSProtection   : false
EnableVmProtection     : false
```

Embora a saída acima nos mostre que a sub-rede já está criada, ela existe apenas na variável local usada para armazenar o objeto da VNet (*$vnet*). Para salvar as configurações, é necessário executar o comando: 

```
PS C:\> Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Pronto! Já temos nossa VNet criada e também a nossa sub-rede. No próximo passo vamos criar um IP Público, embora este recurso seja opcional, neste caso.

```
PS C:\> $publicIP = New-AzureRmPublicIpAddress -Name "DrPublicIP" -ResourceGroupName $resourceGroupName `
-Location $location -AllocationMethod Dynamic -DomainNameLabel "danriblab"
```

Note que foi utilizado o parâmetro **-DomainNameLabel**, esse cara é utilizado para definir um nome DNS para o nosso IP Público, neste caso: danriblab.brazilsouth.cloudapp.azure.com.

Já estamos na metade do caminho. Criamos o Grupo de Recursos, a Rede Virtual, Sub-Rede e o IP Público.

No próximo passo, vamos criar um Grupo de Segurança de Rede (NSG), também conhecido como Firewall.

Primeiramente, vamos criar o NSG:

```
PS C:\> $nsg = New-AzureRmNetworkSecurityGroup -Name "NSG-DanielRibeiroLab" -ResourceGroupName $resourceGroupName -Location $location
```

Em seguida vamos criar uma regra, liberando o acesso remoto na VM:

```
PS C:\> Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
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
PS C:\> Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
```


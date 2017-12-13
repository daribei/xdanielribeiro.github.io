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
Nos dias de hoje é perceptível uma tendencia muito grande em relação a utilização de linha de comando, onde cada vez **menos** será utilizado interface gráfica.

O Azure possuiu duas grandes ferramentas de linha de comando, Azure CLI e Azure PowerShell.
Eu particularmente prefiro o PowerShell.

O objetivo desse artigo é provisionar um ambiente no Azure através do PowerShell.

### Mãos a Obra

Antes de mais nada, é necessário [instalar](https://docs.microsoft.com/pt-br/powershell/azure/install-azurerm-ps?view=azurermps-5.0.0 "instalar") o módulo do Azure PowerShell na sua máquina, ou se preferir, é possível utilizar o seu navegador com o Azure [CloudShell](https://docs.microsoft.com/pt-br/azure/cloud-shell/overview "CloudShell").

Se fossemos provisionar uma VM no Azure, quais recursos seriam necessários para conseguirmos provisionar a VM com sucesso?

Basicamente:
- Rede Virtual;
- Interface de rede;
- Conta de armazenamento;
- Firewall.

O primeiro passo é definir algumas váriaveis que serão utilizadas no decorrer da implementação.
Vamos nessa?

```
PS C:\> $resourceGroupName = "GR-DanielRibeiro"
PS C:\> $location = "Brazil_South"
PS C:\> $vmName = "Azure-Lab-DanielRibeiro"
```

Feito isso, já podemos começar a brincar um pouquinho.

Vamos começar criando o grupo de recursos, lembra que sem ele nós não conseguimos nem dar o primeiro passo.
No meu artigo de [introdução ao Azure](http://xdanielribeiro.com.br/azure/2017/12/13/introducao-ms-azure/ "Introdução ao Microsoft Azure") eu falo sobre isso.

```
PS C:\> New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
```

Para vizualisar se o Grupo de Recursos foi criado, executamos:
```
PS C:\> Get-AzureRmResourceGroup -Location $location
```



---
layout: post
title: Como diagnosticar e resolver problemas em máquinas virtuais no Azure?
date: 2018-07-23 14:22:12
categories: Azure-Troubleshooting
thumbnail: intazure
tags:
  - azure
  - troubleshooting
published: false
---

## Introdução

Ultimamente a Microsoft melhorou e muito a capacidade de diagnosticar e resolver problemas de VMs Windows e Linux no Azure. Isso era algo muito esperado, até porque, nós como clientes, não temos acesso ao backend, isto é, a infraestrutura onde os _workloads_ são executados.

Se você ainda não precisou fazer um troubleshooting em alguma VM no Azure, pode ter certeza que mais cedo ou mais tarde vai precisar, e não é porque a plataforma tem algum tipo de problema recorrente, mas sim, porque isso acontece, rs.

O Azure possui diversas formas diferentes de diagnosticar e resolver problemas. Vou explicar as principais delas e tenho certeza que serão bastante úteis durante o troubleshooting.

## Boot Diagnostics

O Boot Diagnostics (Diagnóstico de inicialização) é o principal recurso utilizado para diagnosticar problemas durante a inicialização de uma máquina virtual Windows e Linux.

Se a máquina virtual não iniciou ou está demorando muito tempo para iniciar, esse recurso com certeza irá te ajudar a entender o que está acontecendo com a VM naquele momento.

Veja na imagem abaixo, um exemplo do boot diagnostics.
Neste caso, a VM simplesmente estava na tela de logon, sem problema algum.

![BootDiagnostics](https://i.imgur.com/fBET5S6.jpg)

Caso você tenha a necessidade de baixar a imagem e enviar pro seu cliente, basta clicar em _Download screenshot_ e será feito o download de um arquivo .bmp. Você também tem a possibilidade de atulizar a console e acompanhar o que está acontecendo.

Além do _screenshot_, você também pode visualizar e baixar o log de inicialização das máquinas virtuais. Clicando em _Serial log_, você vai visualizar o log de boot da VM e caso necessário, pode efetuar o download do mesmo clicando em _Download serial log_.

Importante ressaltar que o _screenshot_ e o arquivo de log ficam armazenados em uma conta de armazenamento.

### Como configurar o Boot Diagnostics?

Esse recurso é ativado por padrão quando criamos uma máquina virtual através do Portal do Azure, ou seja, se você criou uma nova VM e não alterou as configurações padrões durante o provisionamento, não precisa se preocupar.

Caso você não tenha habilitado o diagnóstico de inicialização, pode escolher uma das seguintes formas para habilitar o mesmo: Portal do Azure, PowerShell, Azure CLI e ARM template.

Para habilitar via PowerShell, execute os comandos abaixo, substituindo os valores associados as variáveis $vmName e $rgName.

```
$vmName = "Nome-da-VM"
$rgName = "Nome-do-Grupo-de-Recursos"

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName

Set-AzureRmVMBootDiagnostics -VM $vm -ResourceGroupName $rgName -StorageAccountName "stgbootdiagnostics" -Enable     
```

Para habilitar via Portal do Azure, siga os seguintes passos:

1 - Clique na máquina virtual e em seguida desce até o final e clique em Boot Diagnostics ou Diagnóstico de Inicialização, se estiver em português;

2 - Em Settings/Configurações, habilite o Diagnóstico de inicialização, em seguida, selecione uma conta de armazenamento que deseja colocar os arquivos de diagnósticos.

![BootDiagnosticsConfig](https://i.imgur.com/jjMMzxV.jpg)

Bom, agora que você entendeu o Boot Diagnostics, eu te faço a seguinte pergunta: se você visualizar através do diagnóstico de inicialização que sua VM não está iniciando devido a um erro de configuração no arquivo FSTAB, o que você pode fazer pra resolver esse problema?

Resposta: _Serial Console_! 

### Serial Console


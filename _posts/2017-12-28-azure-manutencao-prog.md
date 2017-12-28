---
layout: post
title: Manutenção Programada no Azure - Fique Atento
date: 2017-12-28 13:30:19
categories: azure
thumbnail: intazure
tags:
  - azure
published: true
---

## Introdução

Seja muito bem vindo ao meu blog.

Hoje pela manhã a Microsoft começou a enviar notificações informando sobre uma manutenção planejada que irá iniciar no dia 09/01/2018 as 22h00 até 10/01/2018 as 22h00.

## O que é uma manutenção programada?

O azure realiza diversas atualizações frequentemente, porém, raramente exigem uma reinicialização da máquina virtual. Normalmente quando isso acontece, a VM é movimentada para outro nó, o que para nós, é imperceptível.

É inevitável que em algum momento essas atualizações tenham algum impacto no nosso ambiente.

Quando uma manutenção requer reinicialização da VM, a microsoft envia uma notificação para o _owner_ e o _co-owners_ da assinatura, também existe a possibilidade de adicionar outros destinatários e outras opções de mensagens, por exemplo: SMS.

É importante ressaltar que manutenções planejadas ocorrem em horários específicos para cada região.

## **Posso** realizar essa manutenção de forma pró-ativa?

Sim! Você tem até as 21h59 do dia 09/01/2018 para realizar esse procedimento de forma pró-ativa. Lembrando que esse horário é para máquinas virtuais que estão no Brasil.

## **Como** realizar essa manutenção de forma pró-ativa?

Pelo portal, no menu de navegação a esquerda, clique em Máquinas virtuais, no painel de VMs, clique no botão Colunas e selecione as seguintes colunas: 
- **Manutenção – Janela proativa**: informa a janela de tempo que você pode realizar o procedimento de manutenção de forma pró-ativa;
- **Manutenção – Janela agendada automaticamente**: Mostra a janela utilizada pelo Azure para reiniciar sua VM.

Caso você não queira realizar o procedimento de forma pró-ativa, as suas VMs podem ser reiniciadas dentro de _qualquer_ horário da janela informada pela Microsoft, isto é, das 22h00 do dia 09/01/2018 até as 22h00 do dia 10/01/2018.

Acredito que não seria interessante ter o servidor de aplicação ou banco de dados indisponível por alguns minutos durante o horário comercial (ou até mesmo fora dele sem um agendamento prévio), rs.

Continuando... 

Depois de selecionar as colunas, você terá uma visão de todas as VMs que serão impactadas pela manutenção. Selecionando uma delas, você verá a página de manutenção com mais detalhes. A partir daí você será capaz de iniciar a manutenção na sua VM.

Da uma olhada na imagem abaixo, tenho certeza que vai ficar mais claro agora.

![ManutençãoProgramada](https://i.imgur.com/LPJJGHu.jpg)

A outra forma de iniciar a manutenção é através do Azure PowerShell. Muito mais rápido, fácil e prático (recomendo).

Use a linha abaixo para listar as VMs e seu respectivo status:

```
Get-AzureRmVM -ResourceGroupName “rgName” -Status
```

Para iniciar a manutenção via PowerShell execute:

```
Restart-AzureRmVM -PerformMaintenance -name “vmName” -ResourceGroupName "rgName”
```

## O que acontece com a minha VM quando a manutenção é iniciada?

Sua VM simplesmente será movimentada para um nó diferente que já foi atualizado.

#### Outras informações úteis:

•	Guias e FAQs sobre a manutenção planejada para VMs [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/maintenance-notifications) e [Linux](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/maintenance-notifications)

•	Informações sobre [tipos de manutenção](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/maintenance-and-updates) realizadas em VMs.

Obrigado,

### Daniel Ribeiro.


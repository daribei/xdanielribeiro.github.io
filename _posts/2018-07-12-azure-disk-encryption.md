---
layout: post
title: Porque o Azure Security Center recomenda a criptografia dos discos das VMs no Azure?
date: 2018-07-12 13:30:19
categories: Azure-Troubleshooting
thumbnail: intazure
tags:
  - azure
  - asc
published: false
---

## Introdução

Sempre se sonhou em ter um File Server híbrido, mas nem sempre isso foi possível, devido a problemas de link da internet. Claro que essa era uma realidade Brasileira, nos Estados Unidos, por exemplo, esse problema deixou de existir a muitos anos. 

Hoje, finalmente temos diversas soluções de File Server híbrido que nos possibilitam manter a performance de um ambiente on-premises e extender a capacidade para um ambiente de nuvem. Ou seja, eu posso ter uma porcentagem dos meus dados no meu ambiente local, geralmente aqueles mais utilizados, garantindo um acesso rápido até eles e armazenando o restante dos dados em uma camada de nuvem, neste caso, o Microsoft Azure.

Atualmente, a Microsoft possui duas soluções que foram criadas no Azure para atender essa demanda, a primeira delas é o StorSimple e a segunda o File Sync, cada uma com suas características, porém, ambas atendem essa necessidade.

O objetivo desse artigo é explicar o funcionamento do Azure File Sync e fazer um rápido comparativo em relação ao Stor Simple.

## Entendendo o Azure File Sync

O File Sync é uma solução de cache local para File Servers, que armazena uma cache dos seus dados mais utilizados no seu ambiente on-premises e os dados que não tem um acesso frequente ficam somente com um ponteiro no ambiente on-premises, quando necessário acessar tais arquivos que estão na camada de nuvem, o agente instalado no servidor rapidamente busca esse dado e possibilita o acesso ao mesmo. Importante ressaltar que esse processo é transparente para o usuário final.

A quantidade de dados que ficará no cache local, vai depender da porcentagem que você definiu durante a configuração do "???". O único parâmetro utilizado para definir se um dado será movido para nuvem é a frequencia de acesso ao mesmo.

Os dados que são armazenados na nuvem, ficam armazenados em uma conta de armazenamento, mais especificamente, utilizando o Azure Files. Embora o Azure Backup para File ainda esteja em preview, é possível utilizar o mesmo para backupear seus dados.

Se um dos seus servidores locais for comprometido, não se preocupe, basta instalar o agente em um novo servidor e será feito a sincronização dos dados, mantendo todo o permissionamento NTFS e a estrutura das pastas.

Basicamente é dessa forma que o Azure File Sync funciona.

No próximo tópico, vamos entender os componentes dessa solução.

## Componentes do Azure File Sync

O File Sync possui alguns componentes chaves para o seu funcionamento:

- Conta de Armazenamento (Files):

- Sync Group: um grupo de sincronismo basicamente define um share do Azure Files que será utilizado durante a sincronização dos dados.

- Endpoint / Registered Server ???: são os servidores que  

- Azure Backup:

## Conclusão

Obrigado,

### Daniel Ribeiro.
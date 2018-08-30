---
layout: post
title: Como o Azure pode te ajudar a enfrentar os desafios de Segurança na Computação em Nuvem
date: 2018-08-29 10:00:00
categories: Azure-Security
thumbnail: intazure
tags:
  - azure
  - security
published: false
---

## __Introdução__

A maioria das organizações quando movem seus _workloads_ para nuvem, acreditam que estão 100% seguras. É nesse ponto que elas se enganam. É claro que movendo seus _workloads_ pra nuvem, você garante diversas camadas de segurança, mas isso não é suficiente.

Uma pesquisa recente realizada pelo Gartner, afirma que a segurança em nuvem é uma responsabilidade compartilhada entre o cliente e o provedor de nuvem (CSP). A figura 1 ilustra os diferentes modelos de computação em nuvem e, principalmente, as camadas que são de responsabilidade do cliente e do CSP. Importante ressaltar que, “_People_” (Pessoas) no diagrama, refere-se a usuários autorizados pelo cliente, e não aos funcionários do provedor de nuvem.

![Security_Handoff_Points](https://i.imgur.com/9rs5rnH.png)
Fonte: Gartner (2018)

O objetivo desse artigo é explicar o funcionamento de alguns recursos do Azure que irão ajudar a manter a segurança nas camadas que são de responsabilidade do cliente.

## Azure Active Directory

Cada vez mais é perceptível a necessidade de adotar uma política de __gerenciamento de identidade e acesso__ (IAM), para garantir a segurança e integridade das informações. Aquele velho conceito de utilizar uma única credencial para gerenciar todo o ambiente ficou no passado, e, as organizações que compartilham essa credencial entre os administradores estão correndo um grande risco, em outras palavras, é um convite para o desastre.

Para ajudar as organizações a definir uma estratégia inicial de gerenciamento de identidade e acesso, sugerimos que no primeiro momento seja implementado as seguintes políticas:

- Definir quem pode fazer o que nas assinaturas de Azure, ou seja, criar funções específicas e atribuí-las a usuários ou grupos;
- Habilitar a autenticação multifator (MFA).

Todas essas políticas citadas acima podem ser configuradas com o Azure Active Directory, que já vem habilitado por padrão e não tem nenhum custo adicional, a não ser que você necessite habilitar recursos avançados.

## Azure Security Center

A segurança dos dados é uma das maiores preocupações das organizações quando o assunto é computação em nuvem. Diferente de alguns provedores de nuvem, o Microsoft Azure está preparado para atender às necessidades de segurança e privacidade das organizações através do Azure Security Center.

O Azure Security Center (ASC) auxilia as organizações a obterem maior visibilidade e controle sobre a segurança dos recursos no Azure e também em ambientes on-premises. Além de oferecer diversos mecanismos de segurança, o ASC possui integração com uma enorme variedade de soluções de segurança, por exemplo: Trend Micro Deep Security.

Quais são os principais benefícios do ASC?

- Defesas avançadas de nuvem: as defesas avançadas de nuvem incorporadas ao ASC incluem recursos criados para proteger especificamente os ativos baseados em nuvem contra diversos tipos de ataques. Isso inclui o acesso just-in-time, entre outros recursos. O acesso just-in-time permite que você proteja suas máquinas virtuais contra ameaças, como ataques de força bruta. Ele faz isso liberando o acesso apenas para os usuários que tem permissões de RBAC (Controle de acesso baseado em função) que fornecem acesso de gravação na máquina virtual. Se o usuário tem permissões de gravação, a solicitação é aprovada e o ASC configura automaticamente regras permitindo o tráfego de entrada às portas selecionadas pelo período de tempo que você especificou.

- Alertas de incidentes: o ASC utiliza uma variedade de tecnologias inovadores de detecção de ameaças, como análise comportamental, que dão ao ASC a capacidade de fornecer alertas pró-ativos e em tempo real, ajudando a manter o ambiente seguro. Além disso,o ASC priorizará e agrupará os alertas por criticidade, garantindo que você tenha a visibilidade para se concentrar nos incidentes mais importantes primeiro.

- Recomendações: o ASC analisa constantemente os workloads e quando identifica possíveis vulnerabilidades, oferece recomendações para corrigi-las antes memso de serem exploradas.

## Azure Backup

Para garantir a durabilidade do armazenamento, o Microsoft Azure replica seus dados periodicamente para que eles estejam protegidos contra eventos, que variam de falhas de hardware, falhas de energia ou rede e desastres naturais de grandes proporções. No entando, isso não substitui o backup dos _workloads_.

Mesmo sabendo que a chance de um desastre deixar um grande provedor de nuvem indisponível em uma determinada região é extremamente pequena, precisamos estar pronto pra esse tipo de incidente.

Pensando nisso, a Microsoft oferece o Azure Backup, que realiza backup de máquinas virtuais, banco de dados SQL Server, SharePoint e Exchange. Em relação ao armazenamento dos dados, o Azure Backup oferece dois tipos de replicação:

- LRS: replica seus dados três vezes (ele cria três cópias dos seus dados). Todas as cópias dos dados ficam armazenadas na mesma região. Esse tipo de replicação protege seus dados contra falhas de hardware local.

- GRS: armazenamento com redundância geográfica (GRS) é a opção padrão de replicação e recomendada. O GRS replica seus dados para uma região secundária (a centenas de quilômetros da região primária dos dados de origem). O GRS garante que seus dados estarão protegidos, mesmo se houver um desastre regional.

## Conclusão

A adoção da computação em nuvem está cada vez mais comum e a TEEVO conta com uma equipe especializada para ajudar as organizações nessa jornada de nuvem.

Se você está pronto para essa jornada, entre em contato com nossa equipe comercial, e não perca tempo.
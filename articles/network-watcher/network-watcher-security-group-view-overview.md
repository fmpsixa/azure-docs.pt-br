---
title: "Introdução à exibição do grupo de segurança no Observador de Rede do Azure | Microsoft Docs"
description: "Esta página fornece uma visão geral da capacidade de exibição da segurança do Observador de Rede"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: de48da2c77c65f3e806474ae37ffd00772622b02
ms.lasthandoff: 03/04/2017

---

# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Introdução à exibição do grupo de segurança da rede no Observador de Rede do Azure

O grupo Segurança da Rede pode ser associado no nível da sub-rede ou no nível da NIC. Quando associado no um nível da sub-rede, ele se aplica a todas as instâncias de VM na sub-rede. A exibição Grupo de Segurança da Rede retorna todo NSG configurado e regras associadas no nível da NIC e da sub-rede. Além disso, as regras de segurança efetivas são retornadas para cada NIC em uma VM. Usando a exibição Grupo de Segurança da Rede, você pode avaliar uma VM quanto às vulnerabilidades da rede, como as portas abertas. Também é possível validar se o Grupo de Segurança da Rede está funcionando como o esperado com base em uma comparação entre as regras de segurança configuradas e as efetivas.

Um caso de uso mais estendido está em conformidade com a segurança e auditoria. Você pode definir um conjunto prescritivo de regras de segurança como um modelo de controle da segurança em sua organização. Uma auditoria de conformidade periódica pode ser implementada de forma programática comparando as regras prescritivas com as regras efetivas para cada uma das VMs em sua rede.

No portal, as regras são divididas em Efetiva, Sub-rede, Interface de Rede e Padrão. Isso fornece uma exibição simples das regras aplicadas em uma máquina virtual. Um botão de download é fornecido para baixar com facilidade todas as regras de segurança, independentemente da guia em um arquivo CSV.

![exibição do grupo de segurança][1]

As regras podem ser selecionadas e uma nova folha é aberta para mostrar o Grupo de Segurança da Rede e os prefixos de origem e destino. Nessa folha, você pode navegar diretamente para o recurso Grupo de Segurança da Rede.

![busca detalhada][2]

### <a name="next-steps"></a>Próximas etapas

Saiba como fazer a auditoria das configurações do Grupo de Segurança da Rede visitando [Auditar as configurações do Grupo de Segurança da Rede com o PowerShell](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png











---
title: "Mover um recurso de máquina virtual do Windows no Azure | Microsoft Docs"
description: "Mova uma VM Windows para outro grupo de recursos ou outra assinatura do Azure no modelo de implantação do Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 2aaf9bfaade060817fa7e42299ad20a2e8f78eaf
ms.lasthandoff: 03/22/2017


---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Mover uma VM Windows para outra assinatura ou outro grupo de recursos do Azure
Este artigo explica como mover uma VM Windows entre grupos de recursos ou assinaturas. A movimentação entre assinaturas poderá ser útil se você tiver originalmente criado uma VM em uma assinatura pessoal e agora deseja movê-la para a assinatura da sua empresa a fim de continuar seu trabalho.

> [!IMPORTANT]
>No momento, não é possível mover o Managed Disks. 
>
>Novas IDs de recurso são criadas como parte da mudança. Após a mudança da VM, você precisa atualizar ferramentas e scripts para usar as novas IDs de recurso. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Usar o Powershell para mover uma VM
Para mover uma máquina virtual para outro grupo de recursos, você precisa ter certeza de também mover todos os recursos dependentes. Para usar o cmdlet Move-AzureRMResource, são necessários o nome do recurso e o tipo do recurso. Você pode obter ambos no cmdlet Find-AzureRMResource.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


Para mover uma VM, precisamos mover vários recursos. Podemos criar variáveis separadas para cada recurso e depois listá-las. Este exemplo inclui a maioria dos recursos básicos de uma VM, mas você pode adicionar mais conforme a necessidade.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Para mover os recursos para outra assinatura, inclua o parâmetro **-DestinationSubscriptionId** . 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Será solicitado que você confirme se deseja mover os recursos especificados. Digite **Y** para confirmar que deseja mover os recursos.

## <a name="next-steps"></a>Próximas etapas
Você pode mover vários tipos diferentes de recursos entre grupos de recursos e assinaturas. Para saber mais, confira [Mover recursos para um novo grupo de recursos ou assinatura](../azure-resource-manager/resource-group-move-resources.md).    



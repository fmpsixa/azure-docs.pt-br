---
title: 'Tornar a unidade D: de uma VM um disco de dados | Microsoft Docs'
description: "Descreve como alterar letras de unidade de uma VM do Windows para que seja possível usar a unidade D: como uma unidade de dados."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 53e053fe12ea6753b6a47443ab91948c9a753a17
ms.lasthandoff: 03/25/2017


---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Usar a unidade D: como uma unidade de dados em uma VM do Windows
Se seu aplicativo precisar usar a unidade D para armazenar dados, siga estas instruções para usar uma letra da unidade diferente para o disco temporário. Nunca use o disco temporário para armazenar os dados que você precisa manter.

Caso você redimensione ou use a opção **Parar (Desalocar)** para uma máquina virtual, isso poderá disparar o posicionamento da máquina virtual para um novo hipervisor. Um evento de manutenção planejada ou não planejada também pode disparar esse posicionamento. Nesse cenário, o disco temporário será reatribuído à primeira letra da unidade disponível. Caso você tenha um aplicativo que exija especificamente a unidade D:, é necessário seguir estas etapas para mover temporariamente o pagefile.sys, anexar um novo disco de dados e atribuí-lo à letra D, bem como mover o pagefile.sys de volta à unidade temporária. Depois de concluído, o Azure não usará novamente a unidade D: se a VM for movida para um hipervisor diferente.

Para obter mais informações sobre como o Azure usa o disco temporário, veja [Noções básicas sobre a unidade temporária nas Máquinas Virtuais do Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Anexar o disco de dados
Primeiro, você precisará anexar o disco de dados à máquina virtual. 

* Para usar o portal, veja [Como anexar um disco de dados no portal do Azure](virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Para usar o portal clássico, veja [Como anexar um disco de dados a uma máquina virtual do Windows](windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 

## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Mover temporariamente o pagefile.sys para a unidade C
1. Conectar-se à máquina virtual. 
2. Clique com o botão direito do mouse no menu **Iniciar** e selecione **Sistema**.
3. No menu à esquerda, selecione **Configurações avançadas do sistema**.
4. Na seção **Desempenho**, selecione **Configurações**.
5. Selecione a guia **Avançado** .
6. Na seção **Memória virtual**, selecione **Alterar**.
7. Selecione a unidade **C**, em seguida, clique em **Tamanho gerenciado pelo sistema** e clique em **Definir**.
8. Selecione a unidade **C**, em seguida, clique em **Nenhum arquivo de paginação** e clique em **Definir**.
9. Clique em Aplicar. Você receberá um aviso de que o computador precisa ser reiniciado para que as alterações entrem em vigor.
10. Reinicie a máquina virtual.

## <a name="change-the-drive-letters"></a>Alterar a letra da unidade
1. Depois que a VM for reiniciada, faça logon novamente na VM.
2. Clique no menu **Iniciar**, digite **diskmgmt.msc** e pressione Enter. O Gerenciamento de Disco será iniciado.
3. Clique com o botão direito do mouse em **D**, a unidade de Armazenamento Temporário e selecione **Alterar Letra da Unidade e Caminhos**.
4. Na Letra da unidade, selecione uma nova unidade como, por exemplo, **T** e clique em **OK**. 
5. Clique com o botão direito do mouse no disco de dados e selecione **Alterar Letra da Unidade e Caminhos**.
6. Na Letra da unidade, selecione a unidade **D** e clique em **OK**. 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Mova o pagefile.sys de volta para a unidade de armazenamento temporário
1. Clique com o botão direito do mouse no menu **Iniciar** e selecione **Sistema**
2. No menu à esquerda, selecione **Configurações avançadas do sistema**.
3. Na seção **Desempenho**, selecione **Configurações**.
4. Selecione a guia **Avançado** .
5. Na seção **Memória virtual**, selecione **Alterar**.
6. Selecione a unidade **C** do SO, em seguida, clique em **Nenhum arquivo de paginação** e clique em **Definir**.
7. Selecione a unidade **T** de armazenamento temporário, em seguida, clique em **Tamanho gerenciado pelo sistema** e clique em **Definir**.
8. Clique em **Aplicar**. Você receberá um aviso de que o computador precisa ser reiniciado para que as alterações entrem em vigor.
9. Reinicie a máquina virtual.

## <a name="next-steps"></a>Próximas etapas
* É possível aumentar o armazenamento disponível para sua máquina virtual [anexando um disco de dados adicional](virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



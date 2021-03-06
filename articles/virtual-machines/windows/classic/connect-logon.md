---
title: "Logon em uma VM do Azure clássica | Microsoft Docs"
description: "Use o portal clássico do Azure para fazer logon na máquina virtual do Windows criada com o modelo clássico de implantação."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 22bc0b6c047737a291e79d9794e113b0821340ac
ms.lasthandoff: 03/25/2017


---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a>Faça logon em uma máquina virtual do Windows usando o Portal do Azure.
No Portal do Azure, você usará o botão **Conectar** para iniciar uma sessão de Área de Trabalho Remota e fazer logon em uma VM do Windows.

Deseja conectar-se a uma VM do Linux? Consulte [Como fazer logon em uma máquina virtual que esteja executando o Linux](../../virtual-machines-linux-mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> O Azure tem dois modelos de implantação diferentes para criar e trabalhar com recursos: [Gerenciador de Recursos e Clássico](../../../resource-manager-deployment-model.md). Este artigo aborda o uso do modelo de implantação Clássica. A Microsoft recomenda que a maioria das implantações novas use o modelo do Gerenciador de Recursos. Para obter informações sobre como fazer logon em uma VM usando o modelo do Resource Manager, consulte [aqui](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="connect-to-the-virtual-machine"></a>Conectar-se à máquina virtual
1. Entre no Portal do Azure.
2. Clique na máquina virtual que você quer acessar. O nome é listado no painel **Todos os recursos**.

    ![Virtual-machine-locations](./media/connect-logon/azureportaldashboard.png)

3. Clique em **Conectar** na barra de comando acima do painel da máquina virtual.

    ![Ícone Conectar para a máquina virtual](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If the **Connect** button isn't available, see the troubleshooting tips at the end of this article.
>
>
-->

## <a name="log-on-to-the-virtual-machine"></a>Faça logon na máquina virtual
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Próximas etapas
* Se o botão **Conectar** estiver inativo, ou se você estiver enfrentando outros problemas com a conexão de Área de Trabalho Remota, tente redefinir a configuração. Clique em **Redefinir acesso remoto** no painel da máquina virtual.

    ![Reset-remote-access](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Se houver problemas com a senha, tente redefini-la. Clique em **Redefinir senha** na borda esquerda do painel da virtual máquina, em **Suporte + Solução de problemas**.

    ![Reset-password](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Se essas dicas não funcionarem ou se não forem necessárias, consulte [Solucionar problemas de conexões da Área de Trabalho Remota com uma Máquina Virtual do Azure baseada no Windows](../../virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Este artigo orienta você no diagnóstico e na solução de problemas comuns.


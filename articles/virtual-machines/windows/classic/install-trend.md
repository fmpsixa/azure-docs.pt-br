---
title: Instalar o Trend Micro Deep Security em uma VM | Microsoft Docs
description: "Este artigo descreve como instalar e configurar o Trend Micro security em uma VM criada com o modelo de implantação Clássico no Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: cb10c04d76e957f88d67a791e3b4a19797704eea
ms.lasthandoff: 03/27/2017


---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Como instalar e configurar o Trend Micro Deep Security as a Service em uma VM do Windows
> [!IMPORTANT] 
> O Azure tem dois modelos de implantação diferentes para criar e trabalhar com recursos: [Gerenciador de Recursos e Clássico](../../../resource-manager-deployment-model.md). Este artigo aborda o uso do modelo de implantação Clássica. A Microsoft recomenda que a maioria das implantações novas use o modelo do Gerenciador de Recursos.

Este artigo mostra como instalar e configurar o Trend Micro Deep Security as a Service em uma VM (máquina virtual) nova ou existente com o Windows Server em execução. O Deep Security as a Service inclui proteção anti-malware, firewall, sistema de prevenção de intrusões e monitoramento de integridade.

O cliente é instalado como uma extensão de segurança via Agente de VM. Em uma máquina virtual nova, você instala o Agente de VM junto com o Agente do Deep Security. Em uma máquina virtual existente sem o Agente de VM, você precisa baixar e instalá-lo primeiro. Este artigo aborda ambas as situações.

Se tiver uma assinatura existente da Trend Micro para uma solução local, você poderá usá-la para ajudar a proteger as máquinas virtuais do Azure. Se ainda não for cliente, você poderá se inscrever em uma assinatura de avaliação. Para saber mais sobre essa solução, confira a postagem no blog da Trend Micro [Extensão do agente de VM do Microsoft Azure para segurança profunda](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Instalar o Agente do Deep Security Agent em uma nova VM
O [portal clássico do Azure](http://manage.windowsazure.com) permite que você instale o agente de VM e a extensão de segurança Trend Micro, quando usa a opção **Da galeria** para criar a máquina virtual. Se você estiver criando uma única máquina virtual, usar o portal será uma maneira fácil de adicionar proteção da Trend Micro.

Essa opção **Da Galeria** abre um assistente que ajuda você a configurar a máquina virtual. Você usa a última página do assistente para instalar o Agente de VM e a extensão de segurança da Trend Micro. Para obter instruções gerais, consulte [Criar uma máquina virtual executando o Windows no portal clássico do Azure](tutorial.md). Quando chegar à última página do assistente, execute as etapas a seguir:

1. Em **Agente de VM**, marque **Instalar o Agente de VM**.
2. Em **Extensões de Segurança**, marque **Agente do Deep Security da Trend Micro**.
   
   ![Instalar o Agente de VM e o Deep Security Agent](./media/install-trend/InstallVMAgentandTrend.png)
3. Clique na marca de seleção para criar a máquina virtual.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Instalar o Agente do Deep Security em uma VM existente
Para instalar o agente em uma VM existente, você precisa do seguinte:

* O módulo do Azure PowerShell versão 0.8.2 ou mais recente instalado no computador local. Você pode verificar a versão do Azure PowerShell que tem instalada usando o comando **Get-Module azure | format-table version** . Para obter instruções e um link para a versão mais recente, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs). Faça logon em sua assinatura do Azure usando `Add-AzureAccount`.
* O agente de VM instalado na máquina virtual de destino.

Primeiramente, verifique se que o agente de VM já está instalado. Preencha o nome do serviço de nuvem e o nome da máquina virtual e, em seguida, execute os seguintes comandos em um prompt de comando do Azure PowerShell com nível de administrador. Substitua tudo entre aspas, incluindo os caracteres < e >.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Se você não souber o nome da máquina virtual e serviço de nuvem, execute **Get-AzureVM** para exibir essas informações para todas as máquinas virtuais em sua assinatura atual.

Se o comando **write-host** retornar **True**, o agente da VM estará instalado. Se ele retornar **False**, confira as instruções e um link para download na postagem do blog do Azure [Agente de VM e extensões - parte 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Se o Agente de VM estiver instalado, execute estes comandos.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Próximas etapas
Leva alguns minutos para o agente abrir quando ele está instalado. Depois disso, você precisa ativar o Deep Security na máquina virtual para que ela possa ser gerenciada por um Deep Security Manager. Consulte os artigos a seguir para obter instruções adicionais:

* Artigo de tendência sobre essa solução, [Segurança na nuvem Instant-On do Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* Um [script do Windows PowerShell de exemplo](http://go.microsoft.com/fwlink/?LinkId=404100) para configurar a máquina virtual
* [Instruções](http://go.microsoft.com/fwlink/?LinkId=404099) para o exemplo

## <a name="additional-resources"></a>Recursos adicionais
[Como fazer logon em uma máquina virtual executando o Windows Server]

[Recursos e extensões de VM do Azure]

<!--Link references-->
[Como fazer logon em uma máquina virtual executando o Windows Server]:connect-logon.md
[Recursos e extensões de VM do Azure]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409


---
title: "Exemplo de script da CLI do Azure – Criar uma VM com um VHD | Microsoft Docs"
description: "Amostra de script da CLI do Azure – criar uma VM usando um disco rígido virtual."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 383929ee41e7d174b4534f988554ba28bb41b9bb
ms.lasthandoff: 03/21/2017

---

# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Criar uma VM com um disco rígido virtual

Este exemplo cria uma máquina virtual usando um VHD.
Ele cria um grupo de recursos, uma conta de armazenamento e um contêiner, então cria uma VM carregando o VHD no contêiner.
Ele substitui a chave pública de SSH pela sua chave pública, de modo que você tem acesso à VM.

Você precisará de um VHD inicializável.
Você pode baixar o VHD que usamos de https://azclisamples.blob.core.windows.net/vhds/sample.vhd ou usar seu próprio VHD. O script procura `~/sample.vhd`.

Este exemplo funciona em um shell Bash. Para opções sobre como executar scripts da CLI do Azure no cliente Windows, veja [Execução da CLI do Azure no Windows](../virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[principal](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Criar VM usando um VHD")]

## <a name="clean-up-deployment"></a>Limpar implantação 

Execute o comando a seguir para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurecli
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Explicação sobre o script

Esse script usa os seguintes comandos para criar um grupo de recursos, uma máquina virtual, um conjunto de disponibilidade, um balanceador de carga e todos os recursos relacionados. Cada comando na tabela redireciona para a documentação específica do comando.

| Command | Observações |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account#list) | Lista contas de armazenamento |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account#check-name) | Verifica se um nome de conta de armazenamento é válido e se já não existe |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | Lista as chaves das contas de armazenamento |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob#exists) | Verifica se o blob existe |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container#create) | Cria um contêiner em uma conta de armazenamento. |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob#upload) | Cria um blob no contêiner carregando o VHD. |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#list) | Usado com `--query`, verifica se o nome da VM está em uso. | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Cria as máquinas virtuais. |
| [az vm access set-linux-user](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | Redefine a chave SSH para fornecer o acesso do usuário atual para a VM. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Obtém o endereço IP da VM que foi criada. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Os exemplos de script da CLI de máquina virtual adicionais podem ser encontrados na [documentação da VM Linux do Azure](../virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


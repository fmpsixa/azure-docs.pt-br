---
title: "Serviço de Contêiner do Azure - perguntas frequentes | Microsoft Docs"
description: "Respostas a perguntas frequentes sobre o Serviço de Contêiner do Azure, um serviço que simplifica a criação, a configuração e o gerenciamento de um cluster de máquinas virtuais para executar aplicativos de contêiner do Docker."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Contêineres, Microsserviços, Mesos, Azure, Kubernetes"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 682ebb212f9056f3704a6de5dde8d3a35681108f
ms.lasthandoff: 03/18/2017


---
# <a name="container-service-frequently-asked-questions"></a>Perguntas frequentes sobre o Serviço de Contêiner


## <a name="orchestrators"></a>Orquestradores

### <a name="which-container-orchestrators-do-you-support-on-azure-container-service"></a>Quais orquestradores de contêiner têm suporte no Serviço de Contêiner do Azure? 

Há suporte para o DC/OS de software livre, Docker Swarm e Kubernetes. Para obter mais informações, confira a [Visão geral](container-service-intro.md).
 
### <a name="do-you-support-docker-swarm-mode"></a>Há suporte para o modo Docker Swarm? 

Atualmente, não há suporte para o modo Swarm, mas isso está no roteiro de serviço. 

### <a name="does-azure-container-service-support-windows-containers"></a>O Serviço de Contêiner do Azure dá suporte a contêineres do Windows?  

Atualmente há suporte para contêineres do Linux com todos os orquestradores. O suporte para contêineres do Windows com o Kubernetes está em versão prévia.

### <a name="do-you-recommend-a-specific-orchestrator-in-azure-container-service"></a>É recomendado um orquestrador específico no Serviço de Contêiner do Azure? 
Geralmente, não recomendamos um orquestrador específico. Se tiver experiência com um dos orquestradores com suporte, você poderá aplicar essa experiência ao Serviço de Contêiner do Azure. No entanto, as tendências de dados sugerem que DC/OS é comprovado em produção para Big Data e cargas de trabalho de IoT, Kubernetes é adequado para cargas de trabalho nativas de nuvem e Docker Swarm é conhecido por sua integração a ferramentas Docker e sua fácil curva de aprendizado.

Dependendo do cenário, você também pode criar e gerenciar soluções de contêiner personalizadas com outros serviços do Azure. Esses serviços incluem [Máquinas Virtuais](../virtual-machines/virtual-machines-linux-azure-overview.md), [Service Fabric](../service-fabric/service-fabric-overview.md), [Aplicativos Web](../app-service-web/app-service-web-overview.md) e [Lote](../batch/batch-technical-overview.md).  

### <a name="what-is-the-difference-between-azure-container-service-and-acs-engine"></a>Qual é a diferença entre o Serviço de Contêiner do Azure e o Mecanismo ACS? 
Serviço de Contêiner do Azure é um serviço do Azure com suporte de SLA com recursos no portal do Azure, ferramentas de linha de comando do Azure e APIs do Azure. O serviço o habilita a implementar rapidamente e gerenciar clusters que executam ferramentas de orquestração do contêiner padrão com um número relativamente pequeno de opções de configuração. 

O [Mecanismo ACS](http://github.com/Azure/acs-engine) é um projeto de software livre que habilita usuários avançados a personalizar a configuração de cluster em todos os níveis. Essa capacidade de alterar a configuração de infraestrutura e software significa que não oferecemos um SLA para o Mecanismo ACS. O suporte é tratado por meio do projeto de software livre no GitHub, em vez de canais oficiais da Microsoft. 

## <a name="cluster-management"></a>Gerenciamento de clusters

### <a name="how-do-i-create-ssh-keys-for-my-cluster"></a>Como criar chaves SSH para o cluster?

Você pode usar as ferramentas padrão no sistema operacional para criar um RSA SSH público e um par de chaves privadas para autenticação em relação a máquinas virtuais Linux para o cluster. Para obter as etapas, confira as diretrizes de [OS X e Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) ou [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

Se você usar [comandos da CLI do Azure 2.0](container-service-create-acs-cluster-cli.md) para implantar um cluster do serviço de contêiner, as chaves SSH poderão ser geradas automaticamente para seu cluster.

### <a name="how-do-i-create-a-service-principal-for-my-kubernetes-cluster"></a>Como criar uma entidade de serviço para o cluster Kubernetes?

Uma ID de entidade e senha de serviço do Azure Active Directory também são necessárias para criar um cluster Kubernetes no Serviço de Contêiner do Azure. Para obter mais informações, confira [Sobre a entidade de serviço para um cluster Kubernetes](container-service-kubernetes-service-principal.md).


Se você usar [comandos da CLI do Azure 2.0](container-service-create-acs-cluster-cli.md) para implantar um cluster Kubernetes, as credenciais de entidade de serviço poderão ser geradas automaticamente para o seu cluster.


### <a name="how-do-i-increase-the-number-of-masters-after-a-cluster-is-created"></a>Como aumentar o número de mestres depois de criar um cluster? 
Depois que o cluster é criado, o número de mestres é fixo e não pode ser alterado. Durante a criação do cluster, o ideal é selecionar vários mestres para uma alta disponibilidade.


### <a name="how-do-i-increase-the-number-of-agents-after-a-cluster-is-created"></a>Como aumentar o número de agentes depois de criar um cluster? 
Você pode dimensionar o número de agentes no cluster usando o portal do Azure ou as ferramentas de linha de comando. Confira [Dimensionar um cluster do Serviço de Contêiner do Azure](container-service-scale.md).


### <a name="what-are-the-urls-of-my-masters-and-agents"></a>Quais são as URLs dos mestres e agentes? 
As URLs de recursos de cluster no Serviço de Contêiner do Azure baseiam-se no prefixo de nome DNS que você fornece e no nome da região do Azure que escolheu para a implantação. Por exemplo, o FQDN (nome de domínio totalmente qualificado) do nó mestre está neste formato:

``` 
DNSnamePrefix.AzureRegion.cloudapp.azure.net
```

Você pode encontrar as URLs comumente usadas para o cluster no portal do Azure, no Gerenciador de Recursos do Azure ou em outras ferramentas do Azure.

### <a name="how-do-i-tell-which-orchestrator-version-is-running-in-my-cluster"></a>Como saber a qual versão de orquestrador está em execução no meu cluster?

* DC/OS: Consulte a [documentação do Mesosphere](https://support.mesosphere.com/hc/en-us/articles/207719793-How-to-get-the-DCOS-version-from-the-command-line-)
* Docker Swarm: Executar `docker version`
* Kubernetes: Executar `kubectl version`


### <a name="how-do-i-upgrade-the-orchestrator-after-deployment"></a>Como atualizar o orquestrador após a implantação?

Atualmente, o Serviço de Contêiner do Azure não fornece ferramentas para atualizar a versão do orquestrador implantado no cluster. Se o Serviço de Contêiner oferecer suporte a uma versão posterior, você pode implantar um novo cluster. Outra opção é usar ferramentas específicas do orquestrador, se disponíveis, para atualizar um cluster no local. Por exemplo, consulte [Atualização de DC/OS](https://dcos.io/docs/1.8/administration/upgrading/).
 
### <a name="where-do-i-find-the-ssh-connection-string-to-my-cluster"></a>Onde encontrar a cadeia de conexão SSH para o cluster?

Você pode encontrar a cadeia de conexão no portal do Azure ou usando as ferramentas de linha de comando do Azure. 

1. No portal, navegue até o grupo de recursos da implantação de cluster.  

2. Clique em **Visão geral** e clique no link para **Implantações** em **Noções básicas**. 

3. Na folha **Histórico de implantação**, clique na implantação que tem um nome começando com **microsoft-acs** seguido por uma data de implantação. Exemplo: microsoft-acs-201701310000.  

4. Na página **Resumo**, em **Saídas**, são fornecidos vários links de cluster. **SSHMaster0** fornece uma cadeia de conexão SSH para o primeiro mestre no cluster do serviço de contêiner. 

Como mencionado anteriormente, você também pode usar ferramentas do Azure para localizar o FQDN do mestre. Fazer uma conexão SSH com o mestre usando o FQDN do mestre e o nome de usuário que você especificou ao criar o cluster. Por exemplo:

```bash
ssh userName@masterFQDN –A –p 22 
```

Para obter mais informações, confira [Conectar-se a um cluster do Serviço de Contêiner do Azure](container-service-connect.md).




## <a name="next-steps"></a>Próximas etapas

* [Saiba mais](container-service-intro.md) sobre o Serviço de Contêiner do Azure.
* Implantar um cluster de serviço de contêiner usando o [portal](container-service-deployment.md) ou a [CLI do Azure 2.0](container-service-create-acs-cluster-cli.md).


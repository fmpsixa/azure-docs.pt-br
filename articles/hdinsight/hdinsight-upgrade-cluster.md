---
title: "Migrar do HDInsight baseado em Windows para o HDInsight baseado em Linux – Azure | Microsoft Docs"
description: Saiba como migrar de um cluster HDInsight baseado no Windows para um cluster HDInsight baseado no Linux.
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: bhanupr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: e249d2859f135bf1a49b152ce206dc66ddebb75f
ms.lasthandoff: 03/25/2017


---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>Atualizar o cluster HDInsight para uma versão mais recente
Para aproveitar os novos recursos do HDInsight, é recomendável que os clusters HDInsight sejam atualizados para a versão mais recente. Siga as diretrizes apresentadas abaixo para atualizar as versões do cluster HDInsight.

> [!NOTE]
> Clusters HDInsight da versão 3.2 e 3.3 estão se aproximando da data de substituição. Para obter mais informações sobre a versão com suporte do HDInsight, consulte [Versões de componente do HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Tarefas de atualização
O fluxo de trabalho para atualizar o cluster HDInsight serão apresentadas a seguir.

![Diagrama de fluxo de trabalho de atualização](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Leia cada seção deste documento para entender as alterações que podem ser necessárias ao atualizar o cluster HDInsight.
2. Crie um cluster como um ambiente de teste/garantia de qualidade. Para saber mais sobre como criar um cluster, consulte [Saiba como criar clusters HDInsight baseados em Linux](hdinsight-hadoop-provision-linux-clusters.md)
3. Copie trabalhos, fontes de dados e coletores existentes para o novo ambiente. Consulte [Copiar Dados para o Ambiente de Teste](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) para obter mais detalhes.
4. Execute testes de validação para garantir que os trabalhos funcionem conforme o esperado no novo cluster.


Depois de verificar se tudo está funcionando conforme o esperado, agende o tempo de inatividade para a migração. Durante esse tempo de inatividade, execute as tarefas a seguir:

1.    Faça backup de dados transitórios armazenados localmente em nós do cluster. Por exemplo, se você tiver dados armazenados diretamente em um nó principal.
2.    Exclua o cluster existente.
3.    Crie um cluster na mesma sub-rede VNET com a versão do HDI mais recente (ou com suporte) que usa o mesmo armazenamento de dados padrão utilizado pelo cluster anterior. Isso permite que o novo cluster continue trabalhando nos dados de produção existentes.
4.    Importe o backup de todos os dados transitórios.
5.    Inicie os trabalhos/continue processando usando o novo cluster.

## <a name="next-steps"></a>Próximas etapas
* [Saiba como criar clusters HDInsight baseados em Linux](hdinsight-hadoop-provision-linux-clusters.md)
* [Conectar-se ao HDInsight usando o SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Gerenciar um cluster baseado em Linux usando o Ambari](hdinsight-hadoop-manage-ambari.md)



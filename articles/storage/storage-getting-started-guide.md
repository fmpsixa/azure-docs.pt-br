---
title: "Introdução ao Armazenamento do Azure em cinco minutos | Microsoft Docs"
description: "Compreenda rapidamente os Blobs, Tabelas e Filas do Microsoft Azure usando os Inícios Rápidos de Armazenamento do Azure, o Visual Studio e o emulador de armazenamento do Azure. Execute seu primeiro aplicativo de Armazenamento do Azure em cinco minutos."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 582f76f8-c814-4a69-8a5c-1fd0e0d5d8f2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 02/23/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: a8e5d36d31aabc9226206f52d8543566c5218494
ms.openlocfilehash: 6c833cd40439fef1fe9cca2591d36a6c49630579
ms.lasthandoff: 02/23/2017


---
# <a name="get-started-with-azure-storage-in-five-minutes"></a>Introdução ao Armazenamento do Azure em cinco minutos
## <a name="overview"></a>Visão geral
É fácil começar a desenvolver com o Armazenamento do Azure. Este tutorial mostra como conseguir um aplicativo para Armazenamento do Azure em funcionamento rapidamente. Você usará os modelos de Início Rápido incluídos no SDK do Azure para .NET. Esses Inícios Rápidos contêm código pronto para execução que demonstra alguns cenários de programação básica com o Armazenamento do Azure.

Para saber mais sobre o Armazenamento do Azure antes de entrarmos no código, veja [Próximas etapas](#next-steps).

## <a name="prerequisites"></a>Pré-requisitos
Você precisará atender aos seguintes pré-requisitos antes de iniciar:

1. Para compilar e criar o aplicativo, você precisará do [Visual Studio 2015](https://www.visualstudio.com/) ou superior instalado no computador.
2. Instalar a versão mais recente do [SDK do Azure para .NET](https://azure.microsoft.com/downloads/). O SDK inclui os projetos de exemplo de Início Rápido do Azure, o emulador de armazenamento do Azure e a [Biblioteca de Cliente de Armazenamento do Azure para .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).
3. Certifique-se de que você tem o [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) instalado no computador, uma vez que ele é necessário para os projetos de exemplo de Início Rápido do Azure que usaremos neste tutorial.

    Se você não tiver certeza de qual versão do .NET Framework está instalada no seu computador, consulte [Como determinar qual .NET Framework versões estão instaladas](https://msdn.microsoft.com/vstudio/hh925568.aspx). Ou pressione o botão **Iniciar** ou a tecla Windows, digite **Painel de Controle**. Em seguida, clique em **Programas** > **Programas e Recursos** e determine se o .NET Framework 4.5 está listado entre os programas instalados.
4. Você precisará de uma assinatura do Azure e de uma conta de armazenamento do Azure.

   * Para obter uma assinatura do Azure, confira [Avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/), [Opções de compra](https://azure.microsoft.com/pricing/purchase-options/) e [Ofertas para membros](https://azure.microsoft.com/pricing/member-offers/) (para membros de outros programas da Microsoft, MSDN, Microsoft Partner Network e BizSpark).
   * Para criar uma conta de armazenamento no Azure, consulte [Como criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Execute o seu primeiro aplicativo do Armazenamento do Azure no Armazenamento do Azure na nuvem
Assim que tiver uma conta, você poderá criar um aplicativo simples de armazenamento do Azure usando um dos projetos de exemplo de Início Rápido do Azure no Visual Studio. Este tutorial concentra-se em projetos de exemplo do Armazenamento do Azure: **Armazenamento do Azure: blobs**, **Armazenamento do Azure: arquivos**, **Armazenamento do Azure: filas** e **Armazenamento do Azure: tabelas**:

1. Inicie o Visual Studio.
2. No menu **Arquivo**, clique em **Novo Projeto**.
3. Na caixa de diálogo **Novo Projeto**, clique em **Instalados** > **Modelos** > **Visual C#** > **Nuvem** > **QuickStarts** > **Serviços de Dados**.
   
   a. Escolha um dos seguintes modelos:** Armazenamento do Azure: blobs**, **Armazenamento do Azure: arquivos**, **Armazenamento do Azure: filas** ou **Armazenamento do Azure: tabelas**.
   
   b. Verifique se o **.NET Framework 4.5** está selecionado como a estrutura de destino.
   
   c. Especifique um nome para seu projeto e crie a nova solução do Visual Studio, conforme mostrado:

    ![Inícios Rápidos do Azure][Image1]

Convém examinar o código-fonte antes de executar o aplicativo. Para examinar o código, escolha **Gerenciador de Soluções** no menu **Exibir** do Visual Studio. Em seguida, clique duas vezes no arquivo Program.cs.

Em seguida, execute o aplicativo de exemplo:

1. No Visual Studio, selecione **Gerenciador de Soluções** no menu **Exibir**. Abra o arquivo **App.config** e comente a cadeia de conexão para o emulador de armazenamento do Azure:

   `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2. Remova a marca de comentário da cadeia de conexão para o serviço de Armazenamento do Azure no arquivo App.config e substitua `[AccountName]` e `[AccountKey]` pelas suas credenciais de conta:

   `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

   Para recuperar sua chave de acesso da conta de armazenamento, confira [Gerenciar as chaves de acesso de armazenamento](storage-create-storage-account.md#manage-your-storage-access-keys).
3. Depois de fornecer a chave de acesso e o nome da conta de armazenamento no arquivo App.config, no menu **Arquivo**, clique em **Salvar Tudo** para salvar todos os arquivos de projeto.
4. No menu **Compilar**, clique em **Compilar Solução**.
5. No menu **Depurar**, pressione **F11** para executar a solução passo a passo ou pressione **F5** para executar a solução.

## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Execute seu primeiro aplicativo para o Armazenamento do Azure localmente no Emulador de Armazenamento do Azure
O [Emulador de Armazenamento do Azure](storage-use-emulator.md) fornece um ambiente local que emula os serviços de blob, fila e tabela do Azure para fins de desenvolvimento. Você pode usar o emulador de armazenamento para testar o aplicativo de armazenamento localmente, sem criar uma conta de armazenamento ou de assinatura do Azure e sem gerar custos.

Para experimentar, vamos criar um aplicativo simples do Armazenamento do Azure usando um dos projetos de exemplo do início rápido do Azure no Visual Studio. Este tutorial concentra-se nos projetos de exemplo do **Armazenamento de Blobs do Azure**, **Armazenamento de Tabelas do Azure** e **Armazenamento de Filas do Azure**:

1. Inicie o Visual Studio.
2. No menu **Arquivo**, clique em **Novo Projeto**.
3. Na caixa de diálogo **Novo Projeto**, clique em **Instalados** > **Modelos** > **Visual C#** > **Nuvem** > **QuickStarts** > **Serviços de Dados**.
    
    a. Escolha um dos seguintes modelos:** Armazenamento do Azure: blobs**, **Armazenamento do Azure: arquivos**, **Armazenamento do Azure: filas** ou **Armazenamento do Azure: tabelas**.
    
    b. Verifique se o **.NET Framework 4.5** está selecionado como a estrutura de destino.
    
    c. Especifique um nome para seu projeto e crie a nova solução do Visual Studio, conforme mostrado:

    ![Inícios Rápidos do Azure][Image1]

4. No Visual Studio, selecione **Gerenciador de Soluções** no menu **Exibir**. Abra o arquivo App.config e comente a cadeia de conexão para a conta de armazenamento do Azure se você já tiver adicionado uma. Em seguida, remova o comentário da cadeia de conexão para o emulador de armazenamento do Azure:

   `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Convém examinar o código-fonte antes de executar o aplicativo. Para examinar o código, escolha **Gerenciador de Soluções** no menu **Exibir** do Visual Studio. Em seguida, clique duas vezes no arquivo Program.cs.

Em seguida, execute o aplicativo de exemplo no Emulador de Armazenamento do Azure:

1. Pressione o botão **Iniciar** ou a tecla Windows, procure por *Emulador de Armazenamento do Microsoft Azure*e inicie o aplicativo. Quando o emulador for iniciado, você verá um ícone e uma notificação na área Exibição de Tarefas do Windows.
2. No Visual Studio, selecione **Compilar Solução** on the **Compilar** .
3. No menu **Depurar**, pressione **F11** para executar a solução passo a passo ou pressione **F5** para executar a solução do início ao fim.

## <a name="next-steps"></a>Próximas etapas
Consulte estes recursos para saber mais sobre o Armazenamento do Azure:

* [Introdução ao Armazenamento do Microsoft Azure](storage-introduction.md)
* [Introdução ao Explorer do Armazenamento do Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Introdução ao armazenamento de Blobs do Azure usando o .NET](storage-dotnet-how-to-use-blobs.md)
* [Introdução ao armazenamento de Tabelas do Azure usando o .NET](storage-dotnet-how-to-use-tables.md)
* [Introdução ao Armazenamento de Filas do Azure usando o .NET](storage-dotnet-how-to-use-queues.md)
* [Introdução ao Armazenamento de Arquivos do Azure no Windows](storage-dotnet-how-to-use-files.md)
* [Transferir dados com o Utilitário de Linha de Comando AzCopy](storage-use-azcopy.md)
* [Documentação do Armazenamento do Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Biblioteca de cliente de armazenamento do Microsoft Azure para .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [API REST de serviços de armazenamento do Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png


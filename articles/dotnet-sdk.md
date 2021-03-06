---
title: "O que é o SDK .NET do Azure?"
description: "Saiba o que está incluído no SDK .NET do Azure."
documentationcenter: .net
author: chrissfanos
editor: mollybos
services: 
ms.assetid: f70279ac-7c56-4d6a-9882-352a8b644a4b
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/30/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 94c4929c84a671eca210a0e6b53eda5019cc0951
ms.lasthandoff: 03/25/2017


---
# <a name="what-is-the-azure-sdk-for-net"></a>O que é o SDK do Azure para .NET?
## <a name="overview"></a>Visão geral
O SDK do Azure para .NET é um conjunto de ferramentas do Visual Studio, ferramentas de linha de comando, binários de tempo de execução e bibliotecas de cliente que ajudam a desenvolver, testar e implantar aplicativos que são executados no Azure. Este artigo detalha o que você obtém quando instala o SDK. Você pode baixar o SDK na [página de downloads do Azure](https://azure.microsoft.com/downloads/).

O SDK do Azure para .NET também inclui [bibliotecas de cliente para consumo de serviços do Azure](http://go.microsoft.com/fwlink/?LinkId=510472). Essas bibliotecas são instaladas separadamente utilizando o NuGet.

## <a id="included"></a>O que está incluído no SDK do Azure para .NET
O SDK do Azure para .NET instala os seguintes produtos:

* [Visual Studio Community Edition 2015](#vwd)
* [Emulador de Armazenamento do Microsoft Azure](#stgemulator)
* [Ferramentas de Armazenamento do Microsoft Azure](#stgtools)
* [Ferramentas de criação do Microsoft Azure](#auth)
* [Emulador do Microsoft Azure](#emulator)
* [Ferramentas do HDInsight para Visual Studio e Driver ODBC do Microsoft Hive](#hdinsight)
* [Bibliotecas do Microsoft Azure para .NET](#libraries)
* [SDK para Aplicativo Móvel do Microsoft Azure](#mobile)
* [PowerShell do Microsoft Azure](#ps)
* [Ferramentas do Microsoft Azure para Microsoft Visual Studio](#tools)
* [Microsoft ASP.NET e Ferramentas da Web para Visual Studio](#wte)
* [Ferramentas do Microsoft Azure Data Lake para Visual Studio](#datalake)

### <a id="vwd"></a>Visual Studio Community Edition 2015
Se você não tiver o Visual Studio em seu computador, o SDK instalará o [Visual Studio Community Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a id="stgemulator"></a>Emulador de Armazenamento do Microsoft Azure
O [Emulador de Armazenamento do Azure](http://msdn.microsoft.com/library/hh403989.aspx) utiliza uma instância do SQL Server e o sistema de arquivo local para simular o Armazenamento do Azure (filas, tabelas, blobs), para que seja possível testar localmente.

### <a id="stgtools"></a>Ferramentas de Armazenamento do Microsoft Azure
Instala o [AzCopy](http://aka.ms/AzCopy), uma ferramenta de linha de comando utilizada para transferir dados de e para uma conta de Armazenamento do Azure.

### <a id="auth"></a>Ferramentas de criação do Microsoft Azure
Incluem os itens a seguir:

* A [ferramenta de linha de comando CSPack](http://msdn.microsoft.com/library/gg432988.aspx) para criar pacotes de implantação.
* A [ferramenta de linha de comando CSEncrypt](http://msdn.microsoft.com/library/hh404001.aspx) para criptografar senhas que são utilizadas para acessar instâncias de função de serviço de nuvem por meio de uma conexão por área de trabalho remota.
* OS binários de tempo de execução de que os projetos de serviço de nuvem precisam para se comunicar com seu ambiente de tempo de execução e para diagnósticos. Esses binários não estão disponíveis em pacotes NuGet.

### <a id="emulator"></a>Emulador do Microsoft Azure
O [Emulador do Azure](http://msdn.microsoft.com/library/dn339018.aspx) simula o ambiente do serviço de nuvem para que você possa testar serviços de nuvem localmente em seu computador antes de implantá-los no Azure.

### <a id="hdinsight"></a>Ferramentas do HDInsight para Visual Studio e Driver ODBC do Microsoft Hive
As ferramentas do HDInsight no Gerenciador de Servidores permitem que você navegue pelos bancos de dados do Hive e contas de armazenamento vinculadas de clusters do HDInsight, crie tabelas, e crie e envie consultas do Hive. Para obter mais informações, consulte [Introdução ao uso das ferramentas do HDInsight Hadoop para Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

### <a id="libraries"></a>Bibliotecas do Microsoft Azure para .NET
Incluem os itens a seguir:

* Pacotes NuGet para Caching, Barramento de Serviço e Armazenamento do Azure, que são armazenados em seu computador de modo que o Visual Studio possa criar novos projetos de serviço de nuvem enquanto estiver offline.
* Um plug-in do Visual Studio que permite que projetos de [Cache na Função](http://msdn.microsoft.com/library/dn386103.aspx) sejam executados localmente no Visual Studio.

### <a id="mobile"></a>SDK para Aplicativo Móvel do Microsoft Azure
Ferramentas para trabalhar com [Aplicativos Móveis do Serviço de Aplicativo do Azure](app-service-mobile/app-service-mobile-value-prop.md).

### <a id="ps"></a>PowerShell do Microsoft Azure
O PowerShell do Azure permite que você [automatize a criação e implantação de ambiente do Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

### <a id="tools"></a>Ferramentas do Microsoft Azure para Microsoft Visual Studio
Permitem que você trabalhe com os recursos do Azure, basicamente Serviços de Nuvem e Máquinas Virtuais:

* [Criar, abrir e publicar projetos de serviço de nuvem](cloud-services/cloud-services-dotnet-get-started.md).
* [Criar pacotes de implantação para projetos de serviço de nuvem](http://msdn.microsoft.com/library/ff683672.aspx).
* [Criar Máquinas Virtuais do Azure ao criar novos projetos da Web](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* [Criar novos scripts do PowerShell ao criar novas máquinas virtuais](http://msdn.microsoft.com/library/dn642480.aspx).
* [Visualizar e gerenciar configurações de projeto de serviços em nuvem em janelas de Propriedades de Projeto no Visual Studio](http://msdn.microsoft.com/library/ee405486.aspx).
* Visualizar e gerenciar [serviços de nuvem](http://msdn.microsoft.com/library/ff683675.aspx) [máquinas virtuais](http://msdn.microsoft.com/library/jj131259.aspx) e [Barramento de Serviço](http://msdn.microsoft.com/library/jj149828.aspx) no Gerenciador de Servidores.
* [Executar em modo de depuração remotamente para serviços de nuvem e máquinas virtuais](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatizar o provisionamento de recursos usando os Projetos de Implantação de Grupo de Recursos do Azure](https://msdn.microsoft.com/library/dn872471.aspx)

### <a id="wte"></a>Ferramentas de Serviço de Aplicativo da Microsoft para Visual Studio
Isto permite que você trabalhe com sites do Azure:

* [Publicar projetos da Web em sites do Azure](app-service-web/web-sites-dotnet-get-started.md).
* [Publicar projetos de aplicativos de console no WebJobs do Azure](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Criar recursos de sites do Azure e de bancos de dados SQL ao criar um novo projeto da Web ou ao publicar um projeto da Web](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Criar scripts de implantação do PowerShell ao criar novos sites](http://msdn.microsoft.com/library/dn642480.aspx).
* [Gerenciar e solucionar problemas de sites do Azure no Gerenciador de Servidores](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Executar no modo de depuração remotamente para sites e WebJobs](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

> [!NOTE]
> Você não precisa instalar o SDK do Azure para .NET para utilizar esses recursos, eles já são incluídos nas atualizações do Visual Studio.
> 
> 

## <a id="datalake"></a>Ferramentas do Microsoft Azure Data Lake para Visual Studio
Para obter mais informações, consulte o [Tutorial: desenvolver scripts U-SQL usando as Ferramentas do Data Lake para Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a id="notincluded"></a>O que não está incluído quando você instala o SDK do Azure para .NET
Há algumas coisas que você poderia desejar para o desenvolvimento no Azure que não estão inclusas quando o SDK é instalado. Entre elas, as mais importantes são:

* [Bibliotecas de cliente](http://go.microsoft.com/fwlink/?LinkId=510472).
  
    O SDK do Azure inclui bibliotecas de cliente para serviços de consumo do Azure, mas nem todas elas são instaladas quando você instala o SDK. Se o seu aplicativo precisar de uma biblioteca cliente que o SDK não instala, você poderá obtê-la no [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Se o seu aplicativo usar uma biblioteca de cliente que o SDK instala, é uma boa prática atualizá-la com a versão atual no NuGet.org.
  
      **Local copies of client libraries.** The Azure SDK for .NET copies to your computer the NuGet packages for some Azure client libraries, such as Storage, Service Bus, and Caching. These client libraries are automatically included in new cloud service projects, so the local NuGet packages enable Visual Studio to create projects even if you're not connected to the Internet. Client libraries are generally updated more frequently than new SDK versions are released, so the client libraries at NuGet.org are often more current than what you get with the SDK.
  
    **Modelos de projeto que incluem bibliotecas do cliente.** Somente modelos de projeto do [Serviço de Nuvem do Azure](cloud-services/cloud-services-dotnet-get-started.md) e [Aplicativos Móveis](app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) do Serviço de Aplicativo do Azure incluem automaticamente algumas bibliotecas do cliente. Para outras bibliotecas ou outros modelos, instale os [pacotes NuGet de bibliotecas de clientes](http://go.microsoft.com/fwlink/?LinkId=510472) de que você precisa.
* [Modelos de projeto de Aplicativos Móveis](app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
  
    Os modelos de Aplicativos Móveis estão disponíveis apenas na atualização 2 do Visual Studio 2013 e posteriores. Eles não estão disponíveis no Visual Studio 2012 ou versões anteriores nem na atualização 1 do Visual Studio 2013 ou anteriores, mesmo que você instale o SDK do Azure para .NET.

## <a id="faq"></a>Perguntas frequentes
* [Muitos recursos do Azure já estão no Visual Studio. Preciso instalar o SDK do Azure para .NET?](#azinvs)
* [Quero uma biblioteca de cliente. Preciso instalar o SDK do Azure para .NET para obtê-la?](#clientlib)
* [Onde posso encontrar versões mais antigas do SDK do Azure para .NET?](#olderversions)
* [Qual é a política de ciclo de vida para versões do SDK do Azure para .NET?](#lifecycle)
* [Com quais versões de sistemas operacionais convidados o SDK do Azure para .NET é compatível?](#guestos)
* [Como desinstalo o SDK do Azure para .NET?](#uninstall)

### <a id="azinvs"></a>Muitos recursos do Azure já estão no Visual Studio. Preciso instalar o SDK do Azure para .NET?
É uma boa prática instalar o SDK se você quiser desenvolver para Azure usando as ferramentas mais recentes. Se preferir não instalar o SDK, você pode fazê-lo caso as condições a seguir sejam atendidas:

* Você instalou a mais recente [Atualização do Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5).
* Você está desenvolvendo apenas para os Sites e Serviços Móveis do Azure, não para os Serviços de Nuvem ou Máquinas Virtuais.
* Seu aplicativo não usa o Armazenamento, ou usa o Armazenamento, mas você não precisa do Emulador de Armazenamento ou da ferramenta AzCopy.

### <a id="clientlib"></a>Quero uma biblioteca de cliente. Preciso instalar o SDK do Azure para .NET para obtê-la?
O SDK instala bibliotecas de cliente somente para que você possa criar projetos de serviços em nuvem sem estar conectado à internet. Bibliotecas de cliente atualizadas estão disponíveis em pacotes NuGet em [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Para saber mais, consulte [O que não está incluído quando você instala o SDK do Azure para .NET](#notincluded) no início deste documento.

### <a id="olderversions"></a>Onde posso encontrar versões mais antigas do SDK do Azure para .NET?
Para obter versões mais antigas, consulte a página de downloads do [SDK do Azure para .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) .

### <a id="lifecycle"></a>Qual é a política de ciclo de vida para versões do SDK do Azure para .NET?
Consulte a [Política de Ciclo de Vida do Suporte aos Serviços de Nuvem do Microsoft Azure](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

### <a id="guestos"></a>Com quais versões de sistemas operacionais convidados o SDK do Azure para .NET é compatível?
Consulte a [Matriz de Compatibilidade de Versões de sistemas operacionais Convidados e do SDK do Azure](http://msdn.microsoft.com/library/ee924680.aspx).

### <a id="uninstall"></a>Como desinstalo o SDK do Azure para .NET?
Cada item mostrado neste artigo em [O que está incluído no SDK do Azure para .NET](#included) é um programa separado na lista de programas instalados no Painel de Controle **Programas e Recursos** do Windows.  Não é possível desinstalá-los como um grupo. Você precisa desinstalar cada programa individualmente.

Quando você já tiver o SDK do Azure para .NET instalado e instalar uma nova versão, normalmente, não há necessidade de desinstalar a versão antiga. Na maioria das vezes, a instalação do SDK atualiza um programa existente em vez de adicionar um novo e deixar o antigo.

No entanto, se deseja remover resíduos de uma versão antiga que não é mais necessária, desinstale apenas os programas que especificam o número da versão mais antiga e apenas se o mesmo programa com uma versão mais nova estiver presente. Por exemplo, depois da atualização da versão 2.5 para 2.6, você pode ver as versões 2.5 e 2.6 das “Ferramentas do Microsoft Azure para Microsoft Visual Studio 2013” e desinstalar a versão 2.5. Porém, você pode ver apenas a versão 2.5 das “Ferramentas de Criação do Microsoft Azure” e não seria seguro desinstalá-la.

Observe que os números de versão nos títulos do programa mostrados em **Programas e Recursos** podem ser confundidos.  Por exemplo, a versão 2.6 do SDK incluirá “SDK V1.0 para Aplicativo Móvel do Microsoft Azure” se você instalar o SDK para Visual Studio 2013 e “SDK V2.0 para Aplicativo Móvel do Microsoft Azure” para Visual Studio 2015; a versão, nesse caso, não é a versão do SDK, mas um indicador de à qual versão do Visual Studio o programa se aplica.

Este artigo não lista todos os programas que cada versão antiga do SDK do Azure incluía; há outros programas que você pode desinstalar das versões antigas do SDK, pois essas versões antigas às vezes incluíam programas que foram omitidos de versões posteriores. Por exemplo, a versão 2.5 instala as “Ferramentas do Gerenciador de Recursos do Azure para Visual Studio”, mas que não estão na lista deste artigo, pois não são mais mostradas como um programa separado em **Programas e Recursos**.  Este artigo lista apenas os programas que estão incluídos no SDK do Azure para .NET versão 2.6.

> **Observação:** alguns dos programas incluídos no SDK também podem ser instalados separadamente em outros contextos e podem ser necessários mesmo que você não precise do SDK completo. O mesmo pode ser verdadeiro para programas que foram instalados por versões antigas do SDK, mas foram omitidos de versões posteriores do SDK. Ao desinstalar programas, tenha cuidado para não remover algo que ainda é necessário no seu computador.
> 
> 

## <a id="versions"></a>Versões
Para ver qual é a versão é atual ou para baixar as versões mais antigas, consulte a página [Histórico de versão do SDK do Azure para .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) .

## <a id="resources"></a>Recursos
Para baixar o SDK do Azure para .NET atual ou uma biblioteca de cliente, consulte a [página de Downloads do Azure](https://azure.microsoft.com/downloads/).

Para obter o código-fonte do SDK do Azure para .NET, incluindo as bibliotecas de cliente, consulte [GitHub.com/Azure](https://github.com/azure/).

Para obter a documentação de referência da biblioteca de cliente do Azure, consulte [Referência do .NET do Azure](https://azure.microsoft.com/documentation/api/).



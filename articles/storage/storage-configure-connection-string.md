---
title: "Configurar uma Cadeia de Conexão para o Armazenamento do Azure | Microsoft Docs"
description: "Criar uma cadeia de conexão para uma conta de armazenamento do Azure. Uma cadeia de conexão inclui as informações necessárias para autenticar o acesso a uma conta de armazenamento de seu aplicativo no tempo de execução."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 1a3c754bf2f2b73d0bf72cbf48b906d8085eaef1
ms.openlocfilehash: f4410c10ce66d50b64307e364e64a3367b9397f9


---
# <a name="configure-azure-storage-connection-strings"></a>Configurar cadeias de conexão do Armazenamento do Azure
## <a name="overview"></a>Visão geral
Uma cadeia de conexão inclui as informações necessárias de autenticação necessárias para acessar os dados em uma conta de armazenamento do Azure de seu aplicativo no tempo de execução. Você pode configurar uma cadeia de conexão para:

* Conecte-se ao emulador de armazenamento do Azure.
* Acesse uma conta de armazenamento no Azure.
* Acessar recursos especificados no Azure por uma SAS (Assinatura de Acesso Compartilhado).

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Armazenar a sua cadeia de conexão
Seu aplicativo precisará acessar a cadeia de conexão no tempo de execução para autenticar solicitações feitas para o Armazenamento o Azure. Você tem algumas opções diferentes para armazenar a cadeia de conexão:

* Para um aplicativo em execução na área de trabalho ou em um dispositivo, é possível armazenar a cadeia de conexão em um arquivo **app.config** ou **web.config**. Adicionar a cadeia de conexão à seção **AppSettings** .
* Para um aplicativo em execução em um serviço de nuvem do Azure, você pode armazenar a cadeia de conexão no [arquivo de esquema (.cscfg) de configuração de serviço do Azure](https://msdn.microsoft.com/library/ee758710.aspx). Adicionar a cadeia de conexão à seção **ConfigurationSettings** do arquivo de configuração de serviço.
* Você também pode usar a cadeia de conexão diretamente no seu código. Na maioria dos cenários, no entanto, recomendamos armazenar sua cadeia de configuração em um arquivo de configuração.

Armazenar a cadeia de conexão em um arquivo de configuração facilita a atualização da cadeia de conexão para alternar entre o emulador de armazenamento e uma conta de armazenamento do Azure na nuvem. Você precisa apenas editar a cadeia de conexão para apontar para seu ambiente de destino.

Você pode usar a classe [Gerenciador de Configuração do Microsoft Azure](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) para acessar a cadeia de conexão no tempo de execução, independentemente de onde ela estiver sendo executada.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Criar uma cadeia de conexão para o emulador de armazenamento
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Consulte [Usar o Emulador de Armazenamento do Azure para desenvolvimento e teste](storage-use-emulator.md) para obter mais informações sobre o emulador de armazenamento.

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Criar uma cadeia de conexão para uma conta de armazenamento do Azure
Para criar uma cadeia de conexão para sua conta de armazenamento do Azure, use o formato de cadeia de caracteres de conexão abaixo. Indique se você deseja se conectar à conta de armazenamento por HTTPS (recomendado) ou HTTP, substitua *myAccountName* pelo nome de sua conta de armazenamento e *myAccountKey* pela tecla de acesso da conta:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Por exemplo, a cadeia de conexão será semelhante à seguinte cadeia de conexão de amostra:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

> [!NOTE]
> O Armazenamento do Azure dá suporte a HTTP e HTTPS em uma cadeia de conexão; no entanto, usar HTTPS é altamente recomendável.
>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Criar uma cadeia de conexão usando uma assinatura de acesso compartilhado
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Criando uma cadeia de conexão para um ponto de extremidade explícito do armazenamento
Você pode especificar explicitamente os pontos de extremidade de serviço na sua cadeia de conexão em vez dos pontos de extremidade padrão. Para criar uma cadeia de conexão que especifique um ponto de extremidade explícito, especifique o ponto de extremidade de serviço completo para cada serviço, incluindo a especificação do protocolo (HTTP ou HTTPS, sendo este o recomendado) usando o seguinte formato:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Um cenário em que pode ser útil especificar um ponto de extremidade explícito é se você mapeou o ponto de extremidade do Armazenamento de Blobs para um domínio personalizado. Nesse caso, é possível especificar o ponto de extremidade personalizado para o Armazenamento de blobs na cadeia de conexão e, opcionalmente, especificar os pontos de extremidade padrão para o outro serviço, caso eles sejam usados pelo aplicativo.

Estes são exemplos de cadeias de conexão válidas que especificam um ponto de extremidade explícito para o serviço Blob:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=www.mydomain.com;
AccountName=storagesample;
AccountKey=account-key

# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=www.mydomain.com;
FileEndpoint=myaccount.file.core.windows.net;
QueueEndpoint=myaccount.queue.core.windows.net;
TableEndpoint=myaccount;
AccountName=storagesample;
AccountKey=account-key
```

O valor de ponto de extremidade que está listado na cadeia de conexão é usado para construir os URIs de solicitação ao serviço Blob e ele determina a forma que os URIs são retornados ao seu código.

Observe que se você optar por omitir um ponto de extremidade de serviço da cadeia de conexão, não será possível usá-la para acessar os dados neste serviço do seu código.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Criando uma cadeia de conexão com um sufixo de ponto de extremidade
Para criar uma cadeia de conexão para o serviço de armazenamento em regiões ou instâncias com sufixos de ponto de extremidade diferente, como para o Azure China ou governança do Azure, use o seguinte formato de cadeia de caracteres de conexão. Indique se você deseja se conectar à conta de armazenamento por HTTP ou HTTPS, substitua *myAccountName* pelo nome de sua conta de armazenamento, *myAccountKey* pela tecla de acesso da conta e *mySuffix* pelo sufixo do URI:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Por exemplo, sua cadeia de conexão deve ser semelhante à seguinte cadeia de conexão:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Analisando uma cadeia de conexão
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Próximas etapas
* [Usar o Emulador de Armazenamento do Azure para desenvolvimento e teste](storage-use-emulator.md)
* [Pesquisadores de armazenamento do Azure](storage-explorers.md)
* [Uso de SAS (Assinaturas de Acesso Compartilhado)](storage-dotnet-shared-access-signature-part-1.md)




<!--HONumber=Dec16_HO2-->



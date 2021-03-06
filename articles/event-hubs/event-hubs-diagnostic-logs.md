---
title: "Logs de diagnóstico dos Hubs de Eventos do Azure | Microsoft Docs"
description: "Saiba como configurar logs de diagnóstico para Hubs de Eventos no Azure."
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/27/2017
ms.author: sethm;babanisa
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 8b0484b2d4f6474be728531fbda65896f30eccc4
ms.lasthandoff: 03/29/2017


---
# <a name="event-hubs-diagnostic-logs"></a>Logs de diagnóstico dos Hubs de Eventos

É possível exibir dois tipos de logs para os Hubs de Eventos do Azure:
* **[Logs de atividades](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Esses logs contém informações sobre as operações executadas em um trabalho. Os logs estão sempre habilitados.
* **[Logs de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. É possível configurar logs de diagnóstico para ter uma visão mais detalhada de tudo o que acontece com um trabalho. Os logs de diagnóstico abrangem atividades desde o momento em que o trabalho é criado até sua exclusão, incluindo atualizações e atividades que ocorrem durante a execução do trabalho.

## <a name="turn-on-diagnostic-logs"></a>Ativar logs de diagnóstico
Os logs de diagnóstico estão **desativados** por padrão. Para habilitar logs de diagnóstico:

1.    No portal do Azure, acesse a folha do trabalho de transmissão.

2.    Em **Monitoramento**, acesse a folha **Logs de diagnóstico**.

    ![Navegação de folha para os logs de diagnóstico](./media/event-hubs-diagnostic-logs/image1.png)  

3.    Selecione **Ativar diagnóstico**.

    ![Ativar logs de diagnóstico](./media/event-hubs-diagnostic-logs/image2.png)

4.    Para **Status**, selecione **Ativado**.

    ![Alterar o status dos logs de diagnóstico](./media/event-hubs-diagnostic-logs/image3.png)

5.    Defina o destino de arquivamento desejado; por exemplo, uma conta de armazenamento, um Hub de Eventos ou o Azure Log Analytics.

6.    Selecione as categorias de logs que você deseja coletar, por exemplo, **Execução** ou **Criação**.

7.    Salve as novas configurações de diagnóstico.

As novas configurações terão efeito em aproximadamente 10 minutos. Depois disso, os logs aparecerão no destino de arquivamento configurado, na folha **Logs de diagnóstico**.

Para saber mais sobre como configurar um diagnóstico, confira a [visão geral dos logs de diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-categories"></a>Categorias dos logs de diagnóstico
Os Hubs de Eventos capturam os logs de diagnóstico de duas categorias:

* **ArchivalLogs**: logs relacionados aos arquivamentos de Hubs de Eventos, especificamente os logs relacionados a erros de arquivo morto.
* **OperationalLogs**: informações sobre o que está acontecendo durante a operação dos Hubs de Eventos, especificamente o tipo de operação, incluindo a criação do Hub de Eventos, os recursos usados e o status da operação.

## <a name="diagnostic-logs-schema"></a>Esquema de logs de diagnóstico
Todos os logs são armazenados no formato JSON (JavaScript Object Notation). Cada entrada tem campos de cadeia de caracteres que usam o formato descrito nos exemplos a seguir.

### <a name="archive-logs-schema"></a>Esquema dos logs de arquivo morto

As cadeias de caracteres JSON do log de arquivo morto incluem os elementos listados na seguinte tabela:

Nome | Descrição
------- | -------
TaskName | Descrição da tarefa que falhou.
ActivityId | ID interna, usada para acompanhamento.
trackingId | ID interna, usada para acompanhamento.
resourceId | ID do Recurso do Azure Resource Manager.
eventHub | Nome completo do hub de eventos (inclui o nome do namespace).
partitionId | Partição do Hub de Eventos usada para gravação.
archiveStep | ArchiveFlushWriter
startTime | Hora de início da falha.
failures | Número de vezes que a falha ocorreu.
durationInSeconds | Duração da falha.
Message | Mensagem de erro.
categoria | ArchiveLogs

Veja a seguir um exemplo de uma cadeia de caracteres JSON do log de arquivo morto:

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operation-logs-schema"></a>Esquema de logs de operação

As cadeias de caracteres JSON do log de operação incluem os elementos listados na seguinte tabela:

Nome | Descrição
------- | -------
ActivityId | ID interna, usada para fins de rastreamento.
EventName | Nome da operação.     
resourceId | ID do Recurso do Azure Resource Manager.
SubscriptionId | ID da assinatura.
EventTimeString | Tempo da operação.
EventProperties | Propriedades da operação.
Status | Status da operação.
Chamador | Chamador da operação (Portal do Azure ou cliente de gerenciamento).
categoria | OperationalLogs

Veja a seguir um exemplo de uma cadeia de caracteres JSON do log de operação:

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Próximas etapas
* [Introdução aos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Visão geral de API de Hubs de Eventos](event-hubs-api-overview.md)
* [Introdução aos Hubs de Evento](event-hubs-csharp-ephcs-getstarted.md)


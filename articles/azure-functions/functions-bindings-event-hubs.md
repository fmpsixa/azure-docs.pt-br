---
title: "Associações do Hub de Eventos do Azure Functions | Microsoft Docs"
description: "Entenda como usar associações do Hub de Eventos do Azure no Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "azure functions, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/02/2016
ms.author: wesmc
translationtype: Human Translation
ms.sourcegitcommit: c8e9f9709d13295c9414e525f1f60abf0d0accb7
ms.openlocfilehash: 0bfbfd3828aacdee0b6630ced034f2c1e0451abd


---
# <a name="azure-functions-event-hub-bindings"></a>Associações do Hub de Eventos do Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Este artigo explica como configurar e codificar associações do [Hub de Eventos do Azure](../event-hubs/event-hubs-what-is-event-hubs.md) no Azure Functions.
O Azure Functions dá suporte a associações de gatilho e de saída para os Hubs de Eventos.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Se você for novo nos Hubs de Evento do Azure, consulte a [Visão geral do Hub de Eventos do Azure](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Gatilho de Hub de evento
Use o gatilho do Hub de Eventos do Azure para responder a um evento enviado para uma transmissão de evento do hub de eventos. É necessário ter acesso de leitura ao hub de eventos para configurar o gatilho.

O gatilho de hub de eventos usa o seguinte objeto JSON na matriz `bindings` de function.json:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the Event Hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup` é uma propriedade opcional usada para definir o [grupo de consumidores](../event-hubs/event-hubs-what-is-event-hubs.md#event-consumers) usado para assinar eventos no hub. Se omitido, o grupo de consumidores `$Default` será usado.  
`connection` deve ser o nome de uma configuração de aplicativo que contém a cadeia de conexão para o namespace do hub de eventos.
Copie essa cadeia de conexão clicando no botão **Informações de Conexão** do *namespace*, não no próprio hub de eventos. Essa cadeia de conexão deve ter, pelo menos, permissões de leitura para ativar o gatilho.

[Configurações adicionais](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) podem ser fornecidas em um arquivo host.json para ajustar ainda mais gatilhos do hub de eventos.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Uso de gatilho
Quando uma função de gatilho do hub de eventos é disparada, a mensagem que a dispara é passada para a função como cadeia de caracteres.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Exemplo de gatilho
Suponha que você tenha o seguinte gatilho de hub de eventos na matriz `bindings` de function.json:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Consulte o exemplo específico por linguagem de registro do corpo da mensagem do gatilho do hub de eventos.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Exemplo de gatilho em C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Exemplo de gatilho em F# #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Exemplo de gatilho em Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hub-output-binding"></a>Associação de saída do Hub de Eventos
Use a associação de saída do Hub de Eventos para gravar eventos em uma transmissão de evento do hub de eventos. É necessário ter permissão de envio para um hub de eventos a fim de gravar eventos nele.

A associação de saída usa o seguinte objeto JSON na matriz `bindings` de function.json:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection` deve ser o nome de uma configuração de aplicativo que contém a cadeia de conexão para o namespace do hub de eventos.
Copie essa cadeia de conexão clicando no botão **Informações de Conexão** do *namespace*, não no próprio hub de eventos. Essa cadeia de conexão deve ter permissões de envio para enviar a mensagem à transmissão do evento.

## <a name="output-usage"></a>Uso de saída
Esta seção mostra como usar a associação de saída do Hub de Eventos no seu código de função.

Você pode gerar mensagens de saída para o hub de eventos configurado com os seguintes tipos de parâmetro:

* `out string`
* `ICollector<string>` (para gerar várias mensagens)
* `IAsyncCollector<string>` (versão assíncrona de `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Amostra de saída
Suponha que você tenha a seguinte associação de saída do Hub de Eventos na matriz `bindings` de function.json:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Consulte o exemplo específico por linguagem de gravação de evento na mesma transmissão.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Amostra de saída no C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Ou, para criar várias mensagens:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Exemplo de saída em F# #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Exemplo de saída em Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Ou, para enviar várias mensagens,

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Próximas etapas
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]



<!--HONumber=Jan17_HO3-->



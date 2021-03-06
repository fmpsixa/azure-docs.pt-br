---
title: Enviar eventos para Hubs de Eventos do Azure usando Java | Microsoft Docs
description: "Começar a enviar para Hubs de Eventos usando Java"
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: aa7244849f6286e8ef9f9785c133b4c326193c12
ms.openlocfilehash: feb466064f2e26a329977240eeafb28148bdf212


---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Enviar eventos para Hubs de Eventos do Azure usando Java

## <a name="introduction"></a>Introdução
Hubs de Eventos são um sistema de inclusão altamente dimensionável que pode receber milhões de eventos por segundo, permitindo que um aplicativo processe e analise grandes quantidades de dados produzidos por aplicativos e dispositivos conectados. Depois de coletados em Hubs de Evento, você pode transformar e armazenar dados usando qualquer provedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, veja [Visão Geral dos Hubs de Eventos][Event Hubs overview].

Este tutorial mostra como enviar eventos para um Hub de Eventos usando um aplicativo de console em Java. Para receber eventos usando a biblioteca do Host de processador de eventos do Java, veja [neste artigo](event-hubs-java-get-started-receive-eph.md), ou clique no idioma apropriado de recebimento no sumário à esquerda.

Para concluir este tutorial, você precisará do seguinte:

* Um ambiente de desenvolvimento Java. Para este tutorial, vamos considerar o [Eclipse](https://www.eclipse.org/).
* Uma conta ativa do Azure. <br/>Se você não tem uma conta, pode criar uma conta gratuita em apenas alguns minutos. Para obter detalhes, consulte <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Avaliação gratuita do Azure</a>.

## <a name="send-messages-to-event-hubs"></a>Enviar mensagens ao Hub de Eventos
A biblioteca de cliente Java para os Hubs de Eventos está disponível para uso em projetos do Maven por meio do [Repositório Central do Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22), e pode ser referenciada usando a seguinte declaração de dependência dentro do arquivo de projeto do Maven:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Para diferentes tipos de ambientes de compilação, é possível obter explicitamente os arquivos JAR liberados mais recentemente no [Repositório Central do Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) ou no [ponto de distribuição de versão no GitHub](https://github.com/Azure/azure-event-hubs/releases).  

Para um editor de eventos simples, importe o pacote *com.microsoft.azure.eventhubs* para as classes de cliente dos Hubs de Eventos e o pacote *com.microsoft.azure.servicebus* para as classes de utilitário como exceções comuns que são compartilhadas com o cliente de mensagens do Barramento de Serviço do Azure. 

Para o exemplo a seguir, primeiro crie um novo projeto do Maven para um aplicativo de console/shell em seu ambiente de desenvolvimento Java favorito. A classe será chamada ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Substitua o namespace e os nomes do Hub de Eventos pelos valores usados durante a criação do Hub de Eventos.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Em seguida, crie um evento singular transformando uma cadeia de caracteres em sua codificação de bytes UTF-8. Daí, criamos uma nova instância de cliente dos Hubs de Eventos usando a cadeia de conexão e enviamos a mensagem.   

``` Java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md

## <a name="next-steps"></a>Próximas etapas
Você pode saber mais sobre Hubs de Eventos visitando os links abaixo:

* [Receber eventos usando o EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Visão Geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Criar um Hub de Eventos](event-hubs-create.md)
* [Perguntas frequentes sobre os Hubs de Eventos](event-hubs-faq.md)


<!--HONumber=Feb17_HO1-->



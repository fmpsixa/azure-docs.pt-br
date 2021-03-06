---
title: "Visão geral e cenários comuns do Serviços de Mídia do Azure | Microsoft Docs"
description: "Este tópico oferece uma visão geral dos Serviços de Mídia do Azure"
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: juliako;anilmur
translationtype: Human Translation
ms.sourcegitcommit: e126076717eac275914cb438ffe14667aad6f7c8
ms.openlocfilehash: f22b87fc5bdfe2db5de39adaafe9c71d8c32b26a
ms.lasthandoff: 01/13/2017


---
# <a name="azure-media-services-overview-and-common-scenarios"></a>Visão geral e cenários comuns do Serviços de Mídia do Azure

Os Serviços de Mídia do Microsoft Azure tratam-se de uma plataforma extensível baseada em nuvem que permite aos desenvolvedores compilar aplicativos de gerenciamento e entrega de mídia escalonável. Os serviços de mídia se baseiam em APIs REST que permitem que você carregue com segurança, armazene, codifique e empacote o conteúdo de áudio ou vídeo para entrega de streaming sob demanda e ao vivo para vários clientes (por exemplo, TV, PCs e dispositivos móveis).

Você pode compilar fluxos de trabalho de ponta a ponta usando totalmente os serviços de mídia. Você também pode optar por usar componentes de terceiros para algumas partes do seu fluxo de trabalho. Por exemplo, codifique usando um codificador de terceiros. Em seguida, carregue, proteja, empacote e entregue usando os serviços de mídia.

Você pode optar por transmitir seu conteúdo ao vivo ou fornecer conteúdo sob demanda. Este tópico mostra cenários comuns de entrega de conteúdo [ao vivo](media-services-overview.md#live_scenarios) ou [sob demanda](media-services-overview.md#vod_scenarios). O tópico também está vinculado a outros tópicos relevantes.

## <a name="sdks-and-tools"></a>SDKs e ferramentas

Para compilar soluções de serviços de mídia, você pode usar:

* [API REST dos Serviços de Mídia](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Um dos SDKs de cliente disponíveis:
    * [SDK dos Serviços de Mídia do Azure para .NET](https://github.com/Azure/azure-sdk-for-media-services),
    * [SDK do Azure para Java](https://github.com/Azure/azure-sdk-for-java),
    * [SDK do PHP do Azure](https://github.com/Azure/azure-sdk-for-php),
    * [Serviços de Mídia do Azure para Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (esta é uma versão de um SDK do Node.js que não foi criada pela Microsoft. Ele é mantido por uma comunidade e atualmente não tem cobertura de 100% das APIs do AMS).
* Ferramentas existentes:
    * [Portal do Azure](https://portal.azure.com/)
    * [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) ([AMSE] Gerenciador de Serviços de Mídia do Azure é um aplicativo Winforms/C# para Windows)

A imagem a seguir mostra alguns dos objetos mais usados ao desenvolver em relação ao modelo de OData de Serviços de Mídia.

Clique na imagem para exibi-la em tamanho normal.  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

Você pode exibir todo o modelo [aqui](https://media.windows.net/API/$metadata?api-version=2.15).  


## <a name="media-services-learning-paths"></a>Roteiros de aprendizagem dos Serviços de Mídia
Você pode exibir os roteiros de aprendizagem do AMS aqui:

* [Fluxo de trabalho do streaming ao vivo do AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Fluxo de trabalho do streaming sob demanda do AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Pré-requisitos

Para começar a usar o Azure Media Services, você deve possuir o seguinte:

1. Uma conta do Azure. Se você não tiver uma conta, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com).
2. Uma conta de Serviços de Mídia do Azure. Use o Portal do Azure, o .NET ou REST API para criar a conta dos Serviços de Mídia do Azure. Para obter mais informações, veja [Criar conta](media-services-portal-create-account.md).
3. (Opcional) Configure o ambiente de desenvolvimento. Escolha .NET ou API REST para seu ambiente de desenvolvimento. Para obter mais informações, veja [Configurar ambiente](media-services-dotnet-how-to-use.md).

    Além disso, aprenda como [conectar de forma programática](media-services-dotnet-connect-programmatically.md).
4. Um ponto de extremidade de streaming padrão ou premium em estado iniciado.  Para obter mais informações, consulte [Gerenciando pontos de extremidade de streaming](https://docs.microsoft.com/en-us/azure/media-services/media-services-portal-manage-streaming-endpoints)

## <a name="concepts-and-overview"></a>Visão geral e conceitos
Para conferir os conceitos dos Serviços de Mídia do Azure, confira [Conceitos](media-services-concepts.md).

Para uma série de instruções que apresenta a todos os componentes principais dos Serviços de Mídia do Azure, confira [Tutoriais passo a passo dos Serviços de Mídia do Azure](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Este série apresenta uma ótima visão geral dos conceitos e usa a ferramenta AMSE para demonstrar as tarefas de AMS. Observe que a ferramenta AMSE é uma ferramenta do Windows. Essa ferramenta dá suporte à maioria das tarefas que você pode obter programaticamente com o [SDK do AMS para .NET](https://github.com/Azure/azure-sdk-for-media-services), [SDK do Azure para Java](https://github.com/Azure/azure-sdk-for-java) ou [SDK do PHP do Azure](https://github.com/Azure/azure-sdk-for-php).

## <a name="a-idvodscenariosadelivering-media-on-demand-with-azure-media-services-common-scenarios-and-tasks"></a><a id="vod_scenarios"></a>Fornecendo Mídia sob Demanda com os Serviços de Mídia do Azure: cenários e tarefas comuns
Esta seção descreve cenários comuns e fornece links para tópicos relevantes. O diagrama a seguir mostra as partes principais da plataforma de serviços de mídia que estão envolvidas em fornecer conteúdo sob demanda.

![Fluxo de trabalho VoD](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

>[!NOTE]
>Quando sua conta AMS é criada, um ponto de extremidade de streaming **padrão** é adicionado à sua conta em estado **Parado**. Para iniciar seu conteúdo de streaming e tirar proveito do empacotamento dinâmico e da criptografia dinâmica, o ponto de extremidade de streaming do qual você deseja transmitir o conteúdo deve estar em estado **Executando**.

### <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Proteja o conteúdo no armazenamento e forneça mídia de streaming sem proteção (não criptografada)
1. Carregar um arquivo mezzanine de alta qualidade em um ativo.

    É recomendável aplicar a opção de criptografia de armazenamento a seu ativo para proteger o conteúdo durante o carregamento e enquanto ele estiver em repouso no armazenamento.
2. Codifique para um conjunto de arquivos MP4 com taxa de bits adaptável.

    É recomendável aplicar a opção de criptografia de armazenamento ao ativo de saída para proteger o conteúdo em repouso.
3. Configure a política de entrega de ativos (usada pelo empacotamento dinâmico).

    Se seu ativo tiver o armazenamento criptografado, você **deverá** configurar a política de entrega de ativos.
4. Publicar o ativo criando um localizador OnDemand.
5. Fluxo de conteúdo publicado.

### <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Proteger o conteúdo no armazenamento, fornecer mídia de streaming criptografada dinamicamente

1. Carregar um arquivo mezzanine de alta qualidade em um ativo. Aplique a opção de criptografia de armazenamento ao ativo.
2. Codifique para um conjunto de arquivos MP4 com taxa de bits adaptável. Aplique a opção de criptografia de armazenamento ao ativo de saída.
3. Crie uma chave de conteúdo de criptografia para o ativo que você quer que seja criptografado dinamicamente durante a reprodução.
4. Configure a política de autorização de chave de conteúdo.
5. Configure a política de entrega de ativos (usada pelo empacotamento dinâmico e criptografia dinâmica).
6. Publicar o ativo criando um localizador OnDemand.
7. Fluxo de conteúdo publicado.

### <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Use a Análise de Mídia para obter informações acionáveis de seus vídeos
A Análise de Mídia é uma coleção de componentes de fala e de visão que facilitam a obtenção de análises acionáveis dos arquivos de vídeo de organizações e de empresas. Para saber mais, confira [Visão geral a Análise dos Serviços de Mídia do Azure](media-services-analytics-overview.md).

1. Carregar um arquivo mezzanine de alta qualidade em um ativo.
2. Use um dos seguintes serviços da Análise de Mídia para processar seus vídeos:

   * **Indexador** – [processe vídeos com o Indexador de Mídia do Azure 2](media-services-process-content-with-indexer2.md)
   * **Hyperlapse** – [arquivos de mídia do Hyperlapse com o Azure Media Hyperlapse](media-services-hyperlapse-content.md)
   * **Detecção de movimento** – [detecção de movimento para a Análise de Mídia do Azure](media-services-motion-detection.md).
   * **Detecção de face e emoções** – [detecção de emoção e face para a Análise de Mídia do Azure](media-services-face-and-emotion-detection.md).
   * **Resumo de vídeo** – [usar as miniaturas de vídeo de Mídia do Azure para criar um resumo de vídeo](media-services-video-summarization.md)
3. O processador de mídia da Análise de Mídia produz arquivos MP4 ou arquivos JSON. Se um processador de mídia produzir um arquivo MP4, você poderá baixar o arquivo progressivamente. Se um processador de mídia produzir um arquivo JSON, você poderá baixar o arquivo do Armazenamento de Blobs do Azure.

### <a name="deliver-progressive-download"></a>Entregar o download progressivo
1. Carregar um arquivo mezzanine de alta qualidade em um ativo.
2. Codificar em um único arquivo MP4.
3. Publicar o ativo criando um localizador OnDemand ou SAS.

    Se você estiver usando o localizador de SAS, o conteúdo será baixado do armazenamento de blobs do Azure. Nesse caso, não é necessário ter pontos de extremidade de streaming em estado iniciado.
4. Download progressivo de conteúdo.

## <a name="a-idlivescenariosadelivering-live-streaming-events-with-azure-media-services"></a><a id="live_scenarios"></a>Trabalhando com Eventos de Live Streaming com os Serviços de Mídia do Azure
Ao trabalhar com a transmissão ao vivo, normalmente os seguintes componentes estão envolvidos:

* Uma câmera é usada para transmitir um evento.
* Um codificador de vídeo ao vivo que converte os sinais da câmera para fluxos que são enviados a um serviço de transmissão ao vivo.

Opcionalmente, vários codificadores sincronizados em tempo real. Para determinados eventos ao vivo críticos que demandam disponibilidade e qualidade de experiência muito altas, é recomendável utilizar codificadores redundantes ativo-ativo para atingir um failover contínuo sem perda de dados.

* Um serviço de streaming ao vivo que permite que você faça o seguinte:
* inclusão de conteúdo ao vivo usando diversos protocolos de transmissão ao vivo (por exemplo RTMP ou Smooth Streaming),
* (opcionalmente) codificação de seu fluxo no fluxo de taxa de bits adaptável
* visualização de sua transmissão ao vivo,
* armazenamento do conteúdo incluído para ser transmitido posteriormente (vídeo sob demanda)
* fornecimento do conteúdo por meio de protocolos de transmissão comuns (por exemplo, MPEG DASH, Smooth, HLS) diretamente aos seus clientes ou para uma CDN (Rede de Distribuição de Conteúdo) para a distribuição posterior.

O AMS **(Serviços de Mídia do Microsoft Azure)** fornece a capacidade de ingerir, codificar, visualizar, armazenar e fornecer o conteúdo de transmissão ao vivo.

Ao fornecer conteúdo aos clientes, sua meta é fornecer um vídeo de alta qualidade para vários dispositivos em condições de rede diferentes. Para tratar da qualidade e das condições de rede, use os codificadores ao vivo para codificar seu fluxo para transmissão de vídeo com múltiplas taxas de bits (taxa de bits adaptável).  Para lidar com streaming em diferentes dispositivos, use o [empacotamento dinâmico](media-services-dynamic-packaging-overview.md) dos Serviços de Mídia para reempacotar dinamicamente seu fluxo para diferentes protocolos. Os Serviços de Mídia permitem a entrega das seguintes tecnologias de streaming de taxa de bits adaptável: HLS (HTTP Live Streaming), Smooth Streaming, MPEG DASH.

Nos Serviços de Mídia do Azure, **Canais** e **Programas**, e **StreamingEndpoints** tratam de todas as funcionalidades de transmissão ao vivo, incluindo ingestão, formatação, DVR, segurança, escalabilidade e redundância.

Um **Canal** representa um pipeline para o processamento de conteúdo de transmissão ao vivo. Um Canal pode receber fluxos de entrada ao vivo da seguinte maneira:

* Um codificador ativo local envia múltiplas taxas de bits **RTMP** ou **Smooth Streaming** (MP4 fragmentado) para o Canal que está configurado para a entrega de **passagem**. A entrega de **passagem** ocorre quando as transmissões ingeridas passam pelos **Canais** sem nenhuma transcodificação ou codificação adicional. Você pode usar os codificadores dinâmicos a seguir, que produzem Smooth Streaming com múltiplas taxas de bits: MediaExcel, Imagine Communications, Ateme, Envivio, Cisco e Elemental. Os seguintes codificadores ativos produzem RTMP: codificadores Adobe Flash Live Encoder, Haivision, Telestream Wirecast, Teradek e Tricaster.  Um codificador ativo também pode enviar uma transmissão de taxa de bits única para um canal que não está habilitado para a codificação ativa, porém, isso não é recomendado. Quando solicitado, os Serviços de Mídia transmitem o fluxo aos clientes.

> [!NOTE]
> Usar um método de passagem é a maneira mais econômica de fazer uma transmissão ao vivo quando você estiver fazendo vários eventos durante um longo período e já tiver investido em codificadores locais. Confira os detalhes de [preço](https://azure.microsoft.com/pricing/details/media-services/) .
>
>

* Um codificador ao vivo local envia um fluxo de taxa de bits adaptável única para o Canal que é habilitado para realizar a codificação ao vico com os serviços de mídia em um dos seguintes formatos: RTP (MPEG-TS), RTMP oi Smooth Streaming (MP4 fragmentado). O Canal então realiza a codificação ao vivo do fluxo de entrada com taxa de bits única em um fluxo de vídeo (adaptável) de múltiplas taxas de bits. Quando solicitado, os Serviços de Mídia transmitem o fluxo aos clientes.

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Trabalhando com canais que recebem a transmissão ao vivo de múltiplas taxas de bits de codificadores locais (passagem)
O diagrama a seguir mostra as partes principais da plataforma AMS que estão envolvidas no fluxo de trabalho de **passagem** .

![Fluxo de trabalho ao vivo][live-overview2]

Para obter mais informações, consulte [Trabalhando com Canais que Recebem a Transmissão ao Vivo de Múltiplas Taxas de Bits de Codificadores Locais](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Trabalhando com canais habilitados a executar codificação ao vivo com os Serviços de Mídia do Azure
O diagrama a seguir mostra as partes principais da plataforma AMS envolvidas no fluxo de trabalho de transmissão ao vivo em que um canal está habilitado para executar a codificação ao vivo com os serviços de mídia.

![Fluxo de trabalho ao vivo][live-overview1]

Para obter mais informações, consulte [Trabalhando com canais habilitados para executar codificação ao vivo com os Serviços de Mídia do Azure](media-services-manage-live-encoder-enabled-channels.md).

## <a name="consuming-content"></a>Consumo de conteúdo
Os Serviços de Mídia do Azure fornecem as ferramentas necessárias para criar aplicativos de player do cliente sofisticados e dinâmicos para a maioria das plataformas, incluindo: dispositivos iOS, dispositivos Android, Windows, Windows Phone, Xbox e decodificadores de sinais. O tópico a seguir fornece links para SDKs e estruturas de Player que você pode usar para desenvolver seus próprios aplicativos de cliente que podem consumir mídia de streaming dos Serviços de Mídia.

[Desenvolvendo aplicativos de player de vídeo](media-services-develop-video-players.md)

## <a name="enabling-azure-cdn"></a>Habilitando o CDN do Azure
Os Serviços de Mídia dão suporte à integração com o CDN do Azure. Para obter informações sobre como habilitar o CDN do Azure, consulte [Como gerenciar pontos de extremidade de Streaming em uma conta de Serviços de Mídia](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a>Dimensionamento de uma conta de serviços de mídia

Você pode dimensionar os **Serviços de Mídia** especificando o número de **Unidades Reservadas para Streaming** e **Unidades Reservadas para Codificação** com as quais você deseja provisionar sua conta.

Você também pode dimensionar sua conta dos Serviços de Mídia adicionando contas de armazenamento a ela. Cada conta de armazenamento é limitada a 500 TB. Para expandir o armazenamento além das limitações padrão, você pode optar por anexar diversas contas de armazenamento a uma única conta de serviços de mídia.
Os clientes de Serviços de Mídia escolhem um ponto de extremidade de streaming **Standard** ou um ou mais pontos de extremidade de streaming **Premium** de acordo com suas necessidades. O Ponto de Extremidade de Streaming Standard é adequado para a maior parte de cargas de trabalho de streaming. Ele inclui os mesmos recursos das Unidades de Streaming Premium.
O Ponto de Extremidade de Streaming Standard é adequado para a maior parte de cargas de trabalho de streaming. Se você tiver uma carga de trabalho avançada ou os requisitos de capacidade de streaming não se enquadrarem nas metas de produtividade do ponto de extremidade de streaming padrão ou se você desejar controlar a capacidade do serviço StreamingEndpoint para lidar com as crescentes necessidades de largura de banda ajustando as unidades de escala (também conhecidas como unidades de streaming premium), é recomendável alocar unidades de escala.

[Este](media-services-portal-scale-streaming-endpoints.md) tópico fornece links para tópicos relevantes.

## <a name="support"></a>Suporte
[Suporte do Azure](https://azure.microsoft.com/support/options/) fornece opções de suporte do Azure, incluindo os Serviços de Mídia.

## <a name="provide-feedback"></a>Fornecer comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="service-level-agreement-sla"></a>Contrato de nível de serviço (SLA)
* Para a Codificação dos Serviços de mídia, garantimos a disponibilidade de 99,9% de transações de API REST.
* Para streaming, atenderemos com êxito a solicitações com uma garantia de disponibilidade de 99,9% para conteúdos de mídia existentes quando um ponto de extremidade de streaming padrão ou premium for adquirido.
* Para canais ao vivo, garantimos que os canais em execução terão conectividade externa em, no mínimo, 99,9% do tempo.
* Para proteção de conteúdo, garantimos que atenderemos com êxito a solicitações de chave em, no mínimo, 99,9% do tempo.
* Para o indexador, podemos atenderemos com êxito às solicitações de tarefa do indexador processadas com uma unidade reservada para codificação em 99,9% do tempo.

Para obter mais informações, veja [SLA do Microsoft Azure](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png


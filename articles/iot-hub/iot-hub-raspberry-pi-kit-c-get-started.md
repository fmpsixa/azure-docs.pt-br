---
title: "Conectar o Raspberry Pi (C) ao IoT do Azure - Introdução | Microsoft Docs"
description: "Introdução ao Raspberry Pi 3, crie seu Hub IoT do Azure e conecte o Pi ao Hub IoT."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "hub iot do Azure, introdução à internet das coisas, kit de ferramentas do iot"
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 1d78a77ab28989bbac031e7dd967c050b1322ad7
ms.lasthandoff: 03/22/2017


---
# <a name="connect-your-raspberry-pi-3-device-to-your-iot-hub-using-c"></a>Conectar o dispositivo Raspberry Pi 3 ao hub IoT usando o C
> [!div class="op_single_selector"]
> * [Node.JS](iot-hub-raspberry-pi-kit-node-get-started.md)
> * [C](iot-hub-raspberry-pi-kit-c-get-started.md)

Neste tutorial, você começará aprendendo as noções básicas de como trabalhar com o Raspberry Pi 3 que está executando Raspbian. Em seguida, aprenderá a conectar seus dispositivos diretamente à nuvem usando o [Hub IoT do Azure](iot-hub-what-is-iot-hub.md). Para obter exemplos do Windows 10 IoT Core, acesse o [Centro de Desenvolvimento do Windows](http://www.windowsondevices.com/).

> [!NOTE]
> Você adora o Docker ou prefere criar o código-fonte no computador host? Se sua resposta for afirmativa, experimente a solução baseada em Docker no [GitHub](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-docker).

## <a name="lesson-1-configure-your-device"></a>Lição 1: Configurar seu dispositivo
![Diagrama de ponta a ponta da Lição 1](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

Nesta lição, configure seu dispositivo Raspberry Pi 3 com um sistema operacional, configure seu ambiente de desenvolvimento e implante um aplicativo para o Pi.

### <a name="configure-your-device"></a>Configurar seu dispositivo
Configure o Raspberry Pi 3 para o primeiro uso e instale o Raspbian. Raspbian é um sistema operacional gratuito e otimizado para o hardware Raspberry Pi.

*Tempo estimado para conclusão: 30 minutos*

Acesse [Configurar seu dispositivo](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).

### <a name="get-the-tools"></a>Obter as ferramentas
Baixe as ferramentas e o software para compilar e implantar seu primeiro aplicativo para o Raspberry Pi 3.

*Tempo estimado para conclusão: 20 minutos*

Acesse [Obter as ferramentas](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md).

### <a name="create-and-deploy-the-blink-application"></a>Criar e implantar o aplicativo de piscar
Clone o aplicativo de exemplo blink de C do GitHub e use gulp para implantar esse aplicativo na placa do Raspberry Pi 3. Esse aplicativo de exemplo pisca o LED conectado à placa a cada dois segundos.

*Tempo estimado para conclusão: 5 minutos*

Acesse [Criar e implantar o aplicativo blink](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md).

## <a name="lesson-2-create-your-iot-hub"></a>Lição 2: Criar seu hub IoT
![Diagrama de ponta a ponta da Lição 2](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

Nesta lição, você cria sua conta gratuita do Azure, provisiona o Hub IoT do Azure e cria seu primeiro dispositivo no Hub IoT.

Conclua a Lição 1 antes de iniciar esta lição.

### <a name="get-the-azure-tools"></a>Obter as ferramentas do Azure
Instalar a interface de linha de comando do Azure (CLI do Azure).

*Tempo estimado para conclusão: 10 minutos*

Acesse [Obter ferramentas do Azure](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md).

### <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Criar seu Hub IoT e registrar o Raspberry Pi 3
Crie seu grupo de recursos, provisione seu primeiro Hub IoT do Azure e adicione seu primeiro dispositivo ao Hub IoT usando a CLI do Azure.

*Tempo estimado para conclusão: 10 minutos*

Acesse [Criar seu Hub IoT e registrar o Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).

## <a name="lesson-3-send-device-to-cloud-messages"></a>Lição 3: Enviar mensagens do dispositivo para a nuvem
![Diagrama de ponta a ponta da Lição 3](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

Nesta lição, você envia mensagens do Pi para o Hub IoT. Você também cria um aplicativo de funções do Azure que obtém as mensagens recebidas de seu Hub IoT e as grava no Armazenamento de Tabelas do Azure.

Conclua a Lição 1 e a Lição 2 antes de iniciar esta lição.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Criar um aplicativo de funções do Azure e uma conta de armazenamento do Azure
Use um modelo do Azure Resource Manager para criar um aplicativo de funções do Azure e uma conta de Armazenamento do Azure.

*Tempo estimado para conclusão: 10 minutos*

Vá até [Criar um aplicativo de funções do Azure e uma conta de Armazenamento do Azure](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Executar um aplicativo de exemplo para enviar mensagens do dispositivo para a nuvem
Implante e execute um aplicativo de exemplo para seu dispositivo Raspberry Pi 3 que envie mensagens ao Hub IoT.

*Tempo estimado para conclusão: 10 minutos*

Acesse [Executar o aplicativo de exemplo para enviar mensagens do dispositivo para a nuvem](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md).

### <a name="read-messages-persisted-in-azure-storage"></a>Ler mensagens mantidas no Armazenamento do Azure
Monitore as mensagens do dispositivo para a nuvem conforme são gravadas no Armazenamento do Azure.

*Tempo estimado para conclusão: 5 minutos*

Acesse [Ler mensagens mantidas no Armazenamento do Azure](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md).

## <a name="lesson-4-send-cloud-to-device-messages"></a>Lição 4: Enviar mensagens da nuvem para o dispositivo
![Diagrama de ponta a ponta da Lição 4](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Essa lição mostra como enviar mensagens de seu hub IoT do Azure para seu Raspberry Pi 3. As mensagens de controlam o comportamento de ativar e desativar do LED que está conectado ao Pi. Um aplicativo de exemplo está preparado para você realizar essa tarefa.

Conclua a Lição 1, a Lição 2 e a Lição 3 antes de iniciar esta lição.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Executar o aplicativo de exemplo para receber mensagens da nuvem para o dispositivo
O aplicativo de exemplo na lição 4 é executado em seu Pi e monitora mensagens de entrada de seu hub IoT. Uma nova tarefa gulp envia mensagens para seu Pi de seu Hub IoT para piscar o LED.

*Tempo estimado para conclusão: 10 minutos*

Acesse [Executar o aplicativo de exemplo para receber mensagens de nuvem para dispositivo](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md).

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Seção opcional: alterar o comportamento de ativar e desativar do LED
Personalize as mensagens para alterar o comportamento liga e desliga do LED.

*Tempo estimado para conclusão: 10 minutos*

Acesse [Seção opcional: alterar o comportamento de ativar e desativar do LED](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md).

## <a name="troubleshooting"></a>Solucionar problemas
Se tiver problemas durante as lições, procure por soluções no artigo [Solução de problemas](iot-hub-raspberry-pi-kit-c-troubleshooting.md).


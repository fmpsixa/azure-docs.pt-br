---
title: Polimorfismo na estrutura Reliable Actors | Microsoft Docs
description: "Crie hierarquias de interfaces .NET e tipos na estrutura Reliable Actors para reutilizar a funcionalidade e as definições da API."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/28/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2fbce8754a5e3162e3f8c999b34ff25284911021
ms.openlocfilehash: 731c6542ba6d1385eeffa89a6f62e5bcf57a5c1e
ms.lasthandoff: 12/14/2016


---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polimorfismo na estrutura Reliable Actors
A estrutura Reliable Actors permite que você crie atores usando muitas das mesmas técnicas que usaria no design orientado a objeto. Uma dessas técnicas é o polimorfismo, que permite que tipos e interfaces herdem de pais mais generalizados. A herança na estrutura de Reliable Actors geralmente segue o modelo de .NET com algumas restrições adicionais.

## <a name="interfaces"></a>Interfaces
A estrutura de Reliable Actors exige que você defina pelo menos uma interface a ser implementada pelo seu tipo de ator. Essa interface é usada para gerar uma classe proxy que pode ser usada pelos clientes para se comunicar com seus atores. Interfaces podem herdar de outras interfaces, desde que cada interface implementada por um tipo de ator e todos os seus pais derivem, por fim, de IActor. IActor é a interface base definida pela plataforma para atores. Assim, o exemplo de polimorfismo clássico usando formas pode parecer semelhante ao seguinte:

![Hierarquia de interface de atores de forma][shapes-interface-hierarchy]

## <a name="types"></a>Tipos
Você também pode criar uma hierarquia de tipos de ator, derivados da classe base de ator fornecida pela plataforma. No caso de formas, você pode ter um tipo `Shape` base:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Os subtipos de `Shape` podem substituir os métodos da base.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Observe o atributo `ActorService` no tipo de ator. Esse atributo informa à estrutura Reliable Actor que ela deverá criar automaticamente um serviço de hospedagem de atores desse tipo. Em alguns casos, você poderá criar um tipo base que destina-se somente para o compartilhamento de recursos com subtipos e nunca será usado para criar uma instância de atores concretos. Nesses casos, você deve usar a palavra-chave `abstract` para indicar que você nunca criará um ator com base nesse tipo.

## <a name="next-steps"></a>Próximas etapas
* Veja [Como a estrutura Reliable Actors aproveita a plataforma do Service Fabric](service-fabric-reliable-actors-platform.md) para oferecer estado consistente, escalabilidade e confiabilidade.
* Saiba mais sobre o [ciclo de vida do ator](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png


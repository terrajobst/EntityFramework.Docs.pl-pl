---
title: Rejestrowanie - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: efc78fbada3c59bf9cf2c4cb694835bb5ad60e76
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997008"
---
# <a name="logging"></a>Rejestrowanie

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="aspnet-core-applications"></a>Aplikacje platformy ASP.NET Core

EF Core automatycznie integruje się z mechanizmami rejestracji programu ASP.NET Core przy każdym `AddDbContext` lub `AddDbContextPool` jest używany. W związku z tym, korzystając z platformy ASP.NET Core, rejestrowanie należy skonfigurować zgodnie z opisem w [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Inne aplikacje

EF Core rejestrowania obecnie wymaga element ILoggerFactory, która sama skonfigurowane z co najmniej jeden ILoggerProvider. Typowe dostawcy są dostarczane w następujących pakietów:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): rejestratora konsoli proste.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Obsługa usługi Azure App Services "Dzienniki diagnostyczne" i "Log strumienia" funkcji.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): dzienniki, aby monitor debugera za pomocą System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): dzienniki w dzienniku zdarzeń Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): obsługuje EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): dzienniki, aby za pomocą System.Diagnostics.TraceSource.TraceEvent() odbiornik śledzenia.

Po zainstalowaniu odpowiednich pakietów aplikacji powinien utworzyć pojedyncze/globalnego wystąpienia LoggerFactory. Na przykład za pomocą rejestratora konsoli:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

To wystąpienie singleton/globalne następnie powinny być rejestrowane z programem EF Core na `DbContextOptionsBuilder`. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Jest to bardzo ważne jest, że aplikacji należy tworzyć nowe wystąpienie element ILoggerFactory dla każdego wystąpienia kontekstu. Ten sposób spowoduje przeciek pamięci i niską wydajnością.

## <a name="filtering-what-is-logged"></a>Filtrowanie, co jest rejestrowane

Najprostszym sposobem filtrowania, co jest rejestrowane jest skonfigurowane podczas rejestrowania ILoggerProvider. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

W tym przykładzie dziennik jest filtrowana w celu zwraca tylko wiadomości:
 * w kategorii "Microsoft.EntityFrameworkCore.Database.Command"
 * na poziomie "Informacje"

Dla platformy EF Core rejestratora kategorie są definiowane w `DbLoggerCategory` klasy, aby ułatwić znajdowanie kategorii, ale one rozpoznać zwykłe ciągi.

Szczegółowe informacje na temat podstawowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowania platformy ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

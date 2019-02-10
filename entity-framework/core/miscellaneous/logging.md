---
title: Rejestrowanie - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985577"
---
# <a name="logging"></a>Rejestrowanie

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="aspnet-core-applications"></a>Aplikacje platformy ASP.NET Core

EF Core automatycznie integruje się z mechanizmami rejestracji programu ASP.NET Core przy każdym `AddDbContext` lub `AddDbContextPool` jest używany. W związku z tym, korzystając z platformy ASP.NET Core, rejestrowanie należy skonfigurować zgodnie z opisem w [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Inne aplikacje

EF Core rejestrowania obecnie wymaga element ILoggerFactory, która sama skonfigurowane z co najmniej jeden ILoggerProvider. Typowe dostawcy są dostarczane w następujących pakietów:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Rejestrator proste konsoli.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Obsługuje usługi Azure App Services "Dzienniki diagnostyczne" i "Log strumienia" funkcji.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Dzienniki, aby monitor debugera za pomocą System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Dzienniki w dzienniku zdarzeń Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Obsługuje EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Dzienniki, aby za pomocą System.Diagnostics.TraceSource.TraceEvent() odbiornik śledzenia.

> [!NOTE]
> Poniższy kod przykładowy używa `ConsoleLoggerProvider` Konstruktor, który ma zostać zamieniono przestarzały parametr w wersji 2.2. Odpowiednie elementy zastępcze rejestrowania przestarzałe interfejsy API będą dostępne w wersji 3.0 lub nowszej. W międzyczasie jest bezpiecznie zignorować i pominąć ostrzeżenia.

Po zainstalowaniu odpowiednich pakietów aplikacji powinien utworzyć pojedyncze/globalnego wystąpienia LoggerFactory. Na przykład za pomocą rejestratora konsoli:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

To wystąpienie singleton/globalne następnie powinny być rejestrowane z programem EF Core na `DbContextOptionsBuilder`. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Jest to bardzo ważne jest, że aplikacji należy tworzyć nowe wystąpienie element ILoggerFactory dla każdego wystąpienia kontekstu. Ten sposób spowoduje przeciek pamięci i niską wydajnością.

## <a name="filtering-what-is-logged"></a>Filtrowanie, co jest rejestrowane

> [!NOTE]
> Poniższy kod przykładowy używa `ConsoleLoggerProvider` Konstruktor, który ma zostać zamieniono przestarzały parametr w wersji 2.2. Odpowiednie elementy zastępcze rejestrowania przestarzałe interfejsy API będą dostępne w wersji 3.0 lub nowszej. W międzyczasie jest bezpiecznie zignorować i pominąć ostrzeżenia.

Najprostszym sposobem filtrowania, co jest rejestrowane jest skonfigurowane podczas rejestrowania ILoggerProvider. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

W tym przykładzie dziennik jest filtrowana w celu zwraca tylko wiadomości:
 * w kategorii "Microsoft.EntityFrameworkCore.Database.Command"
 * na poziomie "Informacje"

Dla platformy EF Core rejestratora kategorie są definiowane w `DbLoggerCategory` klasy, aby ułatwić znajdowanie kategorii, ale one rozpoznać zwykłe ciągi.

Szczegółowe informacje na temat podstawowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowania platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

---
title: Rejestrowanie - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232139"
---
# <a name="logging"></a>Rejestrowanie

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) w witrynie GitHub.

## <a name="aspnet-core-applications"></a>Aplikacje platformy ASP.NET Core

Podstawowe EF integruje się automatycznie z mechanizmów rejestrowanie platformy ASP.NET Core zawsze, gdy `AddDbContext` lub `AddDbContextPool` jest używany. W związku z tym podczas korzystania z platformy ASP.NET Core, rejestrowanie należy skonfigurować zgodnie z opisem w [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Inne aplikacje

Rejestrowanie obecnie Core EF wymaga element ILoggerFactory, która jest skonfigurowana z co najmniej jeden ILoggerProvider. Typowe dostawców są wysyłane w następujących pakietów:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): rejestratora konsoli simple.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): "Logowanie w strumieniu" Funkcje i usługi aplikacji Azure obsługuje "Dzienniki diagnostyczne".
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): dzienniki z monitorem debugera przy użyciu System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): dzienników do dziennika zdarzeń systemu Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): obsługuje EventSource/odbiornika danych.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): dzienniki, aby za pomocą System.Diagnostics.TraceSource.TraceEvent() odbiornik śledzenia.

Po zainstalowaniu odpowiednich pakietów, aplikacji, należy utworzyć pojedynczą/globalne wystąpienie LoggerFactory. Na przykład za pomocą rejestratora konsoli:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

To pojedyncze/globalne wystąpienie powinien zostać zarejestrowany podstawowych EF na `DbContextOptionsBuilder`. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Jest bardzo ważne, aplikacje nie należy tworzyć nowe wystąpienie element ILoggerFactory dla każdego wystąpienia kontekstu. W ten sposób spowoduje przeciek pamięci i pogorszenie wydajności.

## <a name="filtering-what-is-logged"></a>Filtrowanie, co jest rejestrowane

Najprostszym sposobem, aby filtrować, co jest rejestrowane jest skonfigurowane podczas rejestrowania ILoggerProvider. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

W tym przykładzie jest odfiltrowana zwracać tylko komunikaty dziennika:
 * w kategorii "Microsoft.EntityFrameworkCore.Database.Command"
 * na poziomie "Informacje"

Podstawowych EF rejestratora kategorie są określone w `DbLoggerCategory` klasę, aby ułatwić znajdowanie kategorii, ale te rozwiązania do prostych ciągów.

Więcej informacji na temat podstawowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowanie platformy ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

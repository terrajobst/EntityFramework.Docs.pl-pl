---
title: Rejestrowanie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197509"
---
# <a name="logging"></a>Rejestrowanie

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="aspnet-core-applications"></a>ASP.NET Core aplikacji

EF Core integruje się automatycznie z mechanizmami rejestrowania ASP.NET Core za `AddDbContext` każdym `AddDbContextPool` razem, gdy jest używany. W związku z tym podczas korzystania z ASP.NET Core należy skonfigurować rejestrowanie zgodnie z opisem w [dokumentacji ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Inne aplikacje

Rejestrowanie EF Core wymaga ILoggerFactory, który sam jest skonfigurowany przy użyciu co najmniej jednego dostawcy rejestrowania. Typowi dostawcy są dostarczani w następujących pakietach:

* [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Prosty Rejestrator konsoli.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Obsługuje funkcje usługi Azure App Services "dzienniki diagnostyczne" i "strumień dzienników".
* [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Rejestruje do monitora debugera za pomocą elementu System. Diagnostics. Debug. WriteLine ().
* [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Rejestruje w dzienniku zdarzeń systemu Windows.
* [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Obsługuje funkcję EventSource/odbiornika.
* [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Rejestruje do odbiornika śledzenia przy użyciu `System.Diagnostics.TraceSource.TraceEvent()`.

Po zainstalowaniu odpowiednich pakietów aplikacja powinna utworzyć wystąpienie pojedyncze/globalne LoggerFactory. Na przykład przy użyciu rejestratora konsoli:

# <a name="version-30tabv3"></a>[Wersja 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[Wersja 2. x](#tab/v2)

> [!NOTE]
> Poniższy przykład kodu używa `ConsoleLoggerProvider` konstruktora, który został przestarzały w wersji 2,2 i zastąpiony w 3,0. Można bezpiecznie zignorować i pominąć ostrzeżenia podczas korzystania z 2,2.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

To wystąpienie pojedyncze/globalne powinno następnie zostać zarejestrowane przy użyciu EF Core na `DbContextOptionsBuilder`. Na przykład:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Bardzo ważne jest, aby aplikacje nie utworzyły nowego wystąpienia ILoggerFactory dla każdego wystąpienia kontekstu. Wykonanie tej czynności spowoduje przeciek pamięci i niską wydajność.

## <a name="filtering-what-is-logged"></a>Filtrowanie co jest rejestrowane

Aplikacja może kontrolować, co jest rejestrowane przez skonfigurowanie filtru na ILoggerProvider. Na przykład:

# <a name="version-30tabv3"></a>[Wersja 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[Wersja 2. x](#tab/v2)

> [!NOTE]
> Poniższy przykład kodu używa `ConsoleLoggerProvider` konstruktora, który został przestarzały w wersji 2,2 i zastąpiony w 3,0. Można bezpiecznie zignorować i pominąć ostrzeżenia podczas korzystania z 2,2.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

W tym przykładzie dziennik jest filtrowany tylko w celu zwrócenia komunikatów:
 * w kategorii "Microsoft. EntityFrameworkCore. Database. Command"
 * na poziomie "informacje"

W przypadku EF Core kategorie rejestratorów są zdefiniowane w `DbLoggerCategory` klasie, aby ułatwić znajdowanie kategorii, ale te rozpoznają się z prostymi ciągami.

Więcej szczegółów dotyczących źródłowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowania ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

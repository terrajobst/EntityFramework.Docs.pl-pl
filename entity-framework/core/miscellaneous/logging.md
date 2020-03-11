---
title: Rejestrowanie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: e8adc39ec01ff75112b03446a488df6199cc7041
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416583"
---
# <a name="logging"></a><span data-ttu-id="2cf16-102">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="2cf16-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="2cf16-103">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2cf16-103">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="2cf16-104">ASP.NET Core aplikacji</span><span class="sxs-lookup"><span data-stu-id="2cf16-104">ASP.NET Core applications</span></span>

<span data-ttu-id="2cf16-105">EF Core integruje się automatycznie z mechanizmami rejestrowania ASP.NET Core przy każdym użyciu `AddDbContext` lub `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="2cf16-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="2cf16-106">W związku z tym podczas korzystania z ASP.NET Core należy skonfigurować rejestrowanie zgodnie z opisem w [dokumentacji ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="2cf16-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="2cf16-107">Inne aplikacje</span><span class="sxs-lookup"><span data-stu-id="2cf16-107">Other applications</span></span>

<span data-ttu-id="2cf16-108">Rejestrowanie EF Core wymaga ILoggerFactory, który sam jest skonfigurowany przy użyciu co najmniej jednego dostawcy rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="2cf16-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="2cf16-109">Typowi dostawcy są dostarczani w następujących pakietach:</span><span class="sxs-lookup"><span data-stu-id="2cf16-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="2cf16-110">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): prosty Rejestrator konsoli.</span><span class="sxs-lookup"><span data-stu-id="2cf16-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="2cf16-111">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): obsługuje funkcje platformy App Services Azure "dzienniki diagnostyczne" i "strumień dzienników".</span><span class="sxs-lookup"><span data-stu-id="2cf16-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="2cf16-112">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): rejestruje do monitora debugera za pomocą metody System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="2cf16-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="2cf16-113">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): rejestruje dziennik zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="2cf16-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="2cf16-114">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): obsługuje funkcję EventSource/odbiornika.</span><span class="sxs-lookup"><span data-stu-id="2cf16-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="2cf16-115">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): rejestruje do odbiornika śledzenia przy użyciu `System.Diagnostics.TraceSource.TraceEvent()`.</span><span class="sxs-lookup"><span data-stu-id="2cf16-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="2cf16-116">Po zainstalowaniu odpowiednich pakietów aplikacja powinna utworzyć wystąpienie pojedyncze/globalne LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="2cf16-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="2cf16-117">Na przykład przy użyciu rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="2cf16-117">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="2cf16-118">Wersja 3. x</span><span class="sxs-lookup"><span data-stu-id="2cf16-118">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="2cf16-119">Wersja 2. x</span><span class="sxs-lookup"><span data-stu-id="2cf16-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="2cf16-120">Poniższy przykład kodu używa konstruktora `ConsoleLoggerProvider`, który został przestarzały w wersji 2,2 i zastąpiony w 3,0.</span><span class="sxs-lookup"><span data-stu-id="2cf16-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="2cf16-121">Można bezpiecznie zignorować i pominąć ostrzeżenia podczas korzystania z 2,2.</span><span class="sxs-lookup"><span data-stu-id="2cf16-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="2cf16-122">To wystąpienie pojedyncze/globalne powinno następnie zostać zarejestrowane przy użyciu EF Core na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2cf16-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="2cf16-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2cf16-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="2cf16-124">Bardzo ważne jest, aby aplikacje nie utworzyły nowego wystąpienia ILoggerFactory dla każdego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2cf16-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="2cf16-125">Wykonanie tej czynności spowoduje przeciek pamięci i niską wydajność.</span><span class="sxs-lookup"><span data-stu-id="2cf16-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="2cf16-126">Filtrowanie co jest rejestrowane</span><span class="sxs-lookup"><span data-stu-id="2cf16-126">Filtering what is logged</span></span>

<span data-ttu-id="2cf16-127">Aplikacja może kontrolować, co jest rejestrowane przez skonfigurowanie filtru na ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="2cf16-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="2cf16-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2cf16-128">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="2cf16-129">Wersja 3. x</span><span class="sxs-lookup"><span data-stu-id="2cf16-129">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="2cf16-130">Wersja 2. x</span><span class="sxs-lookup"><span data-stu-id="2cf16-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="2cf16-131">Poniższy przykład kodu używa konstruktora `ConsoleLoggerProvider`, który został przestarzały w wersji 2,2 i zastąpiony w 3,0.</span><span class="sxs-lookup"><span data-stu-id="2cf16-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="2cf16-132">Można bezpiecznie zignorować i pominąć ostrzeżenia podczas korzystania z 2,2.</span><span class="sxs-lookup"><span data-stu-id="2cf16-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

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

<span data-ttu-id="2cf16-133">W tym przykładzie dziennik jest filtrowany tylko w celu zwrócenia komunikatów:</span><span class="sxs-lookup"><span data-stu-id="2cf16-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="2cf16-134">w kategorii "Microsoft. EntityFrameworkCore. Database. Command"</span><span class="sxs-lookup"><span data-stu-id="2cf16-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="2cf16-135">na poziomie "informacje"</span><span class="sxs-lookup"><span data-stu-id="2cf16-135">at the 'Information' level</span></span>

<span data-ttu-id="2cf16-136">W przypadku EF Core kategorie rejestratorów są zdefiniowane w klasie `DbLoggerCategory`, aby ułatwić znajdowanie kategorii, ale te rozpoznają się z prostymi ciągami.</span><span class="sxs-lookup"><span data-stu-id="2cf16-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="2cf16-137">Więcej szczegółów dotyczących źródłowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowania ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="2cf16-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

---
title: Rejestrowanie - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022148"
---
# <a name="logging"></a><span data-ttu-id="e30f6-102">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="e30f6-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="e30f6-103">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="e30f6-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="e30f6-104">Aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e30f6-104">ASP.NET Core applications</span></span>

<span data-ttu-id="e30f6-105">EF Core automatycznie integruje się z mechanizmami rejestracji programu ASP.NET Core przy każdym `AddDbContext` lub `AddDbContextPool` jest używany.</span><span class="sxs-lookup"><span data-stu-id="e30f6-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="e30f6-106">W związku z tym, korzystając z platformy ASP.NET Core, rejestrowanie należy skonfigurować zgodnie z opisem w [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="e30f6-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="e30f6-107">Inne aplikacje</span><span class="sxs-lookup"><span data-stu-id="e30f6-107">Other applications</span></span>

<span data-ttu-id="e30f6-108">EF Core rejestrowania obecnie wymaga element ILoggerFactory, która sama skonfigurowane z co najmniej jeden ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="e30f6-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="e30f6-109">Typowe dostawcy są dostarczane w następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="e30f6-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="e30f6-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): rejestratora konsoli proste.</span><span class="sxs-lookup"><span data-stu-id="e30f6-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="e30f6-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Obsługa usługi Azure App Services "Dzienniki diagnostyczne" i "Log strumienia" funkcji.</span><span class="sxs-lookup"><span data-stu-id="e30f6-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="e30f6-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): dzienniki, aby monitor debugera za pomocą System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="e30f6-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="e30f6-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): dzienniki w dzienniku zdarzeń Windows.</span><span class="sxs-lookup"><span data-stu-id="e30f6-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="e30f6-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): obsługuje EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="e30f6-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="e30f6-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): dzienniki, aby za pomocą System.Diagnostics.TraceSource.TraceEvent() odbiornik śledzenia.</span><span class="sxs-lookup"><span data-stu-id="e30f6-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="e30f6-116">Po zainstalowaniu odpowiednich pakietów aplikacji powinien utworzyć pojedyncze/globalnego wystąpienia LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="e30f6-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="e30f6-117">Na przykład za pomocą rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="e30f6-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="e30f6-118">To wystąpienie singleton/globalne następnie powinny być rejestrowane z programem EF Core na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e30f6-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="e30f6-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e30f6-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="e30f6-120">Jest to bardzo ważne jest, że aplikacji należy tworzyć nowe wystąpienie element ILoggerFactory dla każdego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e30f6-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="e30f6-121">Ten sposób spowoduje przeciek pamięci i niską wydajnością.</span><span class="sxs-lookup"><span data-stu-id="e30f6-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="e30f6-122">Filtrowanie, co jest rejestrowane</span><span class="sxs-lookup"><span data-stu-id="e30f6-122">Filtering what is logged</span></span>

<span data-ttu-id="e30f6-123">Najprostszym sposobem filtrowania, co jest rejestrowane jest skonfigurowane podczas rejestrowania ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="e30f6-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="e30f6-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e30f6-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="e30f6-125">W tym przykładzie dziennik jest filtrowana w celu zwraca tylko wiadomości:</span><span class="sxs-lookup"><span data-stu-id="e30f6-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="e30f6-126">w kategorii "Microsoft.EntityFrameworkCore.Database.Command"</span><span class="sxs-lookup"><span data-stu-id="e30f6-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="e30f6-127">na poziomie "Informacje"</span><span class="sxs-lookup"><span data-stu-id="e30f6-127">at the 'Information' level</span></span>

<span data-ttu-id="e30f6-128">Dla platformy EF Core rejestratora kategorie są definiowane w `DbLoggerCategory` klasy, aby ułatwić znajdowanie kategorii, ale one rozpoznać zwykłe ciągi.</span><span class="sxs-lookup"><span data-stu-id="e30f6-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="e30f6-129">Szczegółowe informacje na temat podstawowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowania platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="e30f6-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

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
# <a name="logging"></a><span data-ttu-id="a9cc6-102">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="a9cc6-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="a9cc6-103">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="a9cc6-104">Aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9cc6-104">ASP.NET Core applications</span></span>

<span data-ttu-id="a9cc6-105">Podstawowe EF integruje się automatycznie z mechanizmów rejestrowanie platformy ASP.NET Core zawsze, gdy `AddDbContext` lub `AddDbContextPool` jest używany.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="a9cc6-106">W związku z tym podczas korzystania z platformy ASP.NET Core, rejestrowanie należy skonfigurować zgodnie z opisem w [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a9cc6-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="a9cc6-107">Inne aplikacje</span><span class="sxs-lookup"><span data-stu-id="a9cc6-107">Other applications</span></span>

<span data-ttu-id="a9cc6-108">Rejestrowanie obecnie Core EF wymaga element ILoggerFactory, która jest skonfigurowana z co najmniej jeden ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="a9cc6-109">Typowe dostawców są wysyłane w następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="a9cc6-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="a9cc6-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): rejestratora konsoli simple.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="a9cc6-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): "Logowanie w strumieniu" Funkcje i usługi aplikacji Azure obsługuje "Dzienniki diagnostyczne".</span><span class="sxs-lookup"><span data-stu-id="a9cc6-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="a9cc6-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): dzienniki z monitorem debugera przy użyciu System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="a9cc6-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="a9cc6-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): dzienników do dziennika zdarzeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="a9cc6-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): obsługuje EventSource/odbiornika danych.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="a9cc6-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): dzienniki, aby za pomocą System.Diagnostics.TraceSource.TraceEvent() odbiornik śledzenia.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="a9cc6-116">Po zainstalowaniu odpowiednich pakietów, aplikacji, należy utworzyć pojedynczą/globalne wystąpienie LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="a9cc6-117">Na przykład za pomocą rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="a9cc6-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="a9cc6-118">To pojedyncze/globalne wystąpienie powinien zostać zarejestrowany podstawowych EF na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="a9cc6-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a9cc6-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="a9cc6-120">Jest bardzo ważne, aplikacje nie należy tworzyć nowe wystąpienie element ILoggerFactory dla każdego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="a9cc6-121">W ten sposób spowoduje przeciek pamięci i pogorszenie wydajności.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="a9cc6-122">Filtrowanie, co jest rejestrowane</span><span class="sxs-lookup"><span data-stu-id="a9cc6-122">Filtering what is logged</span></span>

<span data-ttu-id="a9cc6-123">Najprostszym sposobem, aby filtrować, co jest rejestrowane jest skonfigurowane podczas rejestrowania ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="a9cc6-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a9cc6-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="a9cc6-125">W tym przykładzie jest odfiltrowana zwracać tylko komunikaty dziennika:</span><span class="sxs-lookup"><span data-stu-id="a9cc6-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="a9cc6-126">w kategorii "Microsoft.EntityFrameworkCore.Database.Command"</span><span class="sxs-lookup"><span data-stu-id="a9cc6-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="a9cc6-127">na poziomie "Informacje"</span><span class="sxs-lookup"><span data-stu-id="a9cc6-127">at the 'Information' level</span></span>

<span data-ttu-id="a9cc6-128">Podstawowych EF rejestratora kategorie są określone w `DbLoggerCategory` klasę, aby ułatwić znajdowanie kategorii, ale te rozwiązania do prostych ciągów.</span><span class="sxs-lookup"><span data-stu-id="a9cc6-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="a9cc6-129">Więcej informacji na temat podstawowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowanie platformy ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a9cc6-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

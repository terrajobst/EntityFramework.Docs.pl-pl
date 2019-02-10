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
# <a name="logging"></a><span data-ttu-id="2cc74-102">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="2cc74-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="2cc74-103">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2cc74-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="2cc74-104">Aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc74-104">ASP.NET Core applications</span></span>

<span data-ttu-id="2cc74-105">EF Core automatycznie integruje się z mechanizmami rejestracji programu ASP.NET Core przy każdym `AddDbContext` lub `AddDbContextPool` jest używany.</span><span class="sxs-lookup"><span data-stu-id="2cc74-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="2cc74-106">W związku z tym, korzystając z platformy ASP.NET Core, rejestrowanie należy skonfigurować zgodnie z opisem w [dokumentacji platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="2cc74-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="2cc74-107">Inne aplikacje</span><span class="sxs-lookup"><span data-stu-id="2cc74-107">Other applications</span></span>

<span data-ttu-id="2cc74-108">EF Core rejestrowania obecnie wymaga element ILoggerFactory, która sama skonfigurowane z co najmniej jeden ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="2cc74-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="2cc74-109">Typowe dostawcy są dostarczane w następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="2cc74-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="2cc74-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Rejestrator proste konsoli.</span><span class="sxs-lookup"><span data-stu-id="2cc74-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="2cc74-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Obsługuje usługi Azure App Services "Dzienniki diagnostyczne" i "Log strumienia" funkcji.</span><span class="sxs-lookup"><span data-stu-id="2cc74-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="2cc74-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Dzienniki, aby monitor debugera za pomocą System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="2cc74-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="2cc74-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Dzienniki w dzienniku zdarzeń Windows.</span><span class="sxs-lookup"><span data-stu-id="2cc74-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="2cc74-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Obsługuje EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="2cc74-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="2cc74-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Dzienniki, aby za pomocą System.Diagnostics.TraceSource.TraceEvent() odbiornik śledzenia.</span><span class="sxs-lookup"><span data-stu-id="2cc74-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="2cc74-116">Poniższy kod przykładowy używa `ConsoleLoggerProvider` Konstruktor, który ma zostać zamieniono przestarzały parametr w wersji 2.2.</span><span class="sxs-lookup"><span data-stu-id="2cc74-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="2cc74-117">Odpowiednie elementy zastępcze rejestrowania przestarzałe interfejsy API będą dostępne w wersji 3.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="2cc74-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="2cc74-118">W międzyczasie jest bezpiecznie zignorować i pominąć ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="2cc74-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="2cc74-119">Po zainstalowaniu odpowiednich pakietów aplikacji powinien utworzyć pojedyncze/globalnego wystąpienia LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="2cc74-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="2cc74-120">Na przykład za pomocą rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="2cc74-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="2cc74-121">To wystąpienie singleton/globalne następnie powinny być rejestrowane z programem EF Core na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2cc74-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="2cc74-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2cc74-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="2cc74-123">Jest to bardzo ważne jest, że aplikacji należy tworzyć nowe wystąpienie element ILoggerFactory dla każdego wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2cc74-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="2cc74-124">Ten sposób spowoduje przeciek pamięci i niską wydajnością.</span><span class="sxs-lookup"><span data-stu-id="2cc74-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="2cc74-125">Filtrowanie, co jest rejestrowane</span><span class="sxs-lookup"><span data-stu-id="2cc74-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="2cc74-126">Poniższy kod przykładowy używa `ConsoleLoggerProvider` Konstruktor, który ma zostać zamieniono przestarzały parametr w wersji 2.2.</span><span class="sxs-lookup"><span data-stu-id="2cc74-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="2cc74-127">Odpowiednie elementy zastępcze rejestrowania przestarzałe interfejsy API będą dostępne w wersji 3.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="2cc74-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="2cc74-128">W międzyczasie jest bezpiecznie zignorować i pominąć ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="2cc74-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="2cc74-129">Najprostszym sposobem filtrowania, co jest rejestrowane jest skonfigurowane podczas rejestrowania ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="2cc74-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="2cc74-130">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2cc74-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="2cc74-131">W tym przykładzie dziennik jest filtrowana w celu zwraca tylko wiadomości:</span><span class="sxs-lookup"><span data-stu-id="2cc74-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="2cc74-132">w kategorii "Microsoft.EntityFrameworkCore.Database.Command"</span><span class="sxs-lookup"><span data-stu-id="2cc74-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="2cc74-133">na poziomie "Informacje"</span><span class="sxs-lookup"><span data-stu-id="2cc74-133">at the 'Information' level</span></span>

<span data-ttu-id="2cc74-134">Dla platformy EF Core rejestratora kategorie są definiowane w `DbLoggerCategory` klasy, aby ułatwić znajdowanie kategorii, ale one rozpoznać zwykłe ciągi.</span><span class="sxs-lookup"><span data-stu-id="2cc74-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="2cc74-135">Szczegółowe informacje na temat podstawowej infrastruktury rejestrowania można znaleźć w [dokumentacji rejestrowania platformy ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="2cc74-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

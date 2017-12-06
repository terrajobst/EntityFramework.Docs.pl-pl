---
title: "Obsługiwane implementacje .NET - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Obsługiwane przez podstawowe EF implementacje .NET

Chcemy Core EF udostępnienia dowolnym można napisać kod .NET i nadal pracujemy do tego celu. Poniższa tabela zawiera wskazówki dotyczące każdej implementacji .NET, gdy chcemy umożliwiają działanie podstawowych EF.

Obiekty docelowe Core 2.0 EF [.NET 2.0 standardowe](https://docs.microsoft.com/dotnet/standard/net-standard) i dlatego wymaga implementacje .NET, które obsługują.

| Implementacja .NET | Stan | wymaga 1.x | wymaga 2.x
|-|-|-|-
| **Oprogramowanie .NET core** ([platformy ASP.NET Core](../get-started/aspnetcore/index.md), [konsoli](../get-started/netcore/index.md)itp.) | **W pełni obsługiwane i zalecane:** przykryte przez automatycznych testów i wiele skalowalnych aplikacji znane go użyć pomyślnie. | [Oprogramowanie .NET core SDK 1.x](https://www.microsoft.com/net/core/) | [Oprogramowanie .NET core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET framework** (WinForms, WPF, ASP.NET, [konsoli](../get-started/full-dotnet/index.md)itp.) | **W pełni obsługiwane i zalecane:** przykryte przez automatycznych testów i wiele skalowalnych aplikacji znane go użyć pomyślnie. EF 6 są dostępne również w tej platformie (zobacz [porównania Core EF & EF6](../../efcore-and-ef6/index.md) wybranie odpowiedniej technologii). | .NET Framework 4.5.1 | .NET framework 4.6.1
| **Mono & Xamarin** | **W toku — mogą wystąpić problemy:** niektórych testowanie wykonano przez zespół EF Core i klientów. Pilotażowe zgłaszali niektórych sukces, ale [napotkano problemy](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) i inni użytkownicy będą prawdopodobnie niepokrytego jako testowania będzie kontynuowane. W szczególności są ograniczenia dotyczące platformy Xamarin.iOS, które mogą uniemożliwić niektórych aplikacji utworzony przy użyciu EF Core 2.0 działać poprawnie. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | 5.4 mono <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> 7.5 platformy Xamarin.Android
| [**Platforma uniwersalna systemu Windows**](../get-started/uwp/index.md) | **W toku — mogą wystąpić problemy:** niektórych testowanie wykonano przez zespół EF Core i klientów. [Problemy z](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) został zgłoszony, gdy skompilowano z łańcuchem narzędzi natywnego programu .NET, która jest zwykle używana podczas kompilacji wydania i jest wymagany w przypadku wdrożenia w magazynie systemu Windows (Jeśli nie jest używana platforma .NET Native lub po prostu chcesz wypróbować wiele problemów nie wpłynie na możesz). | [Najnowszy pakiet .NET 5 platformy uniwersalnej systemu Windows](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Najnowszy pakiet 6 platformy uniwersalnej systemu Windows .NET](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> Tej wersji programu .NET platformy uniwersalnej systemu Windows dodaje obsługę platformy .NET Standard 2.0 i .NET Native 2.0, która naprawia większość zgodności zawiera wcześniej zgłoszone problemy, ale testowania ujawniło [kilka pozostałych problemów](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) EF podstawowych 2.0, które firma Microsoft planuje adres w wersji nadchodzących poprawki.

W przypadku dowolnej kombinacji nie działa zgodnie z oczekiwaniami, zaleca się tworzenie nowych problemów w [EF Core problem z obiektem śledzącym](https://github.com/aspnet/entityframeworkcore/issues/new), i xamarin problemy, związane z [Xamarin problem z obiektem śledzącym](https://bugzilla.xamarin.com/newbug).

---
title: Obsługiwane implementacje platformy .NET — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 790628c407cc4374fee4ebde8201783955afdcc3
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900333"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementacji platformy .NET obsługiwanych przez platformę EF Core

Chcemy, aby programu EF Core była dostępna wszędzie można napisać kod .NET i nadal pracujemy do tego celu. Podczas obsługi programu EF Core w .NET Core i .NET Framework jest objęta zautomatyzowanego testowania i wiele aplikacji znane go używać pomyślnie, platformy Mono, Xamarin i platformy uniwersalnej systemu Windows mają pewne problemy.

W poniższej tabeli przedstawiono wskazówki dotyczące każdej implementacji .NET:

| Implementacja programu .NET                                                                                                  | Stan                                                             | Wymagania dotyczące programu EF Core 1.x                                                                                | Wymagania dotyczące programu EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET core** ([platformy ASP.NET Core](../get-started/aspnetcore/index.md), [konsoli](../get-started/netcore/index.md)itp.) | W pełni obsługiwane i zalecane                                    | [Zestaw SDK dla platformy .NET core 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET framework** (WinForms, WPF, ASP.NET, [konsoli](../get-started/full-dotnet/index.md)itp.)                    | W pełni obsługiwane i zalecane. Dostępne są też EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Narzędzie mono i Xamarin**                                                                                                   | Trwającą <sup>(3)</sup>                                         | Narzędzie mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | 5.4 platformy mono <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Universal Windows Platform**](../get-started/uwp/index.md)                                                        | EF Core 2.0.1 zalecane <sup>(4)</sup>                           | [Pakiet 5.x .NET core, platformy UWP](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET core, platformy UWP w wersji 6.x pakietu](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1) </sup> EF Core 2.0 jest przeznaczony dla i dlatego wymaga implementacji platformy .NET, które obsługują [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Zobacz [Porównanie programów EF Core i EF6](../../efcore-and-ef6/index.md) do wyboru odpowiedniej technologii.

<sup>(3) </sup> Brak problemów i znane ograniczenia za pomocą platformy Xamarin, które mogą uniemożliwić niektóre aplikacje opracowane przy użyciu programu EF Core 2.0 z działa prawidłowo. Sprawdź listę [aktywne problemy](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) do rozwiązania problemu.

<sup>(4) </sup> Wcześniejszych wersjach programu EF Core i .NET platformy uniwersalnej systemu Windows ma wiele problemów ze zgodnością, szczególnie w przypadku aplikacje skompilowane przy użyciu łańcucha narzędzi .NET Native. Nowa wersja .NET platformy UWP dodaje obsługę platformy .NET Standard 2.0 i zawiera natywne 2.0 platformy .NET, która naprawia większość problemów ze zgodnością wcześniej zgłoszone. EF Core 2.0.1 został przetestowany dokładniej za pomocą platformy uniwersalnej systemu Windows, ale nie zautomatyzowane testy.

Dla dowolnej kombinacji nie działa zgodnie z oczekiwaniami, zaleca się utworzenie nowych problemów w [narzędzie do śledzenia problemów programu EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Specyficzne dla platformy Xamarin problemów Użyj narzędzia do śledzenia błędów dla [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) lub [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).

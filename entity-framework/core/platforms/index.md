---
title: Obsługiwane implementacje .NET - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 02e9450cb0ead1701da9f58c51bef3031a3be4ed
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678679"
---
# <a name="net-implementations-supported-by-ef-core"></a>Obsługiwane przez podstawowe EF implementacje .NET

Chcemy Core EF udostępnienia dowolnym można napisać kod .NET i nadal pracujemy do tego celu. Podczas obsługi EF rdzeni na oprogramowanie .NET Core i .NET Framework jest objęta automatycznych testów i wiele skalowalnych aplikacji znane go użyć pomyślnie, Mono, Xamarin i platformy uniwersalnej systemu Windows mają problemy.

W poniższej tabeli przedstawiono wskazówki dotyczące każdej implementacji .NET:

| Implementacja .NET                                                                                                  | Stan                                                             | EF podstawowe wymagania 1.x                                                                                | EF podstawowe wymagania 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **Oprogramowanie .NET core** ([platformy ASP.NET Core](../get-started/aspnetcore/index.md), [konsoli](../get-started/netcore/index.md)itp.) | W pełni obsługiwane i zalecane                                    | [Oprogramowanie .NET core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET framework** (WinForms, WPF, ASP.NET, [konsoli](../get-started/full-dotnet/index.md)itp.)                    | W pełni obsługiwane i zalecanym. Są dostępne również EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono & Xamarin**                                                                                                   | Trwa <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | 5.4 mono <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Platforma uniwersalna systemu Windows**](../get-started/uwp/index.md)                                                        | Podstawowe EF 2.0.1 zalecane <sup>(4)</sup>                           | [Pakiet 5.x .NET core platformy uniwersalnej systemu Windows](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [Pakiet 6.x .NET core platformy uniwersalnej systemu Windows](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1) </sup> EF Core 2.0 elementów docelowych i dlatego wymaga implementacje .NET, które obsługują [.NET 2.0 standardowe](https://docs.microsoft.com/dotnet/standard/net-standard).

<sup>(2) </sup> Zobacz [porównania Core EF & EF6](../../efcore-and-ef6/index.md) wybranie odpowiedniej technologii.

<sup>(3) </sup> Problemów i znane ograniczenia za pomocą platformy Xamarin, które mogą uniemożliwić niektórych aplikacji utworzony przy użyciu EF Core 2.0 działać poprawnie. Zapoznaj się z listą [aktywne problemy] ([ ](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) dla rozwiązania.

<sup>(4) </sup> Wcześniejszych wersjach programu .NET platformy uniwersalnej systemu Windows i EF Core ma wiele problemów ze zgodnością, szczególnie w przypadku aplikacji kompilowane z łańcuchem narzędzi platformy .NET Native. Nowa wersja platformy .NET UWP dodaje obsługę .NET 2.0 standardowe i zawiera .NET Native 2.0, która naprawia większość problemów ze zgodnością wcześniej zgłoszonych. Bardziej dokładnie przetestowane EF Core 2.0.1 z platformy uniwersalnej systemu Windows, ale nie automatycznego testowania.

W przypadku dowolnej kombinacji nie działa zgodnie z oczekiwaniami, zaleca się utworzenie nowych problemów na [EF Core problem z obiektem śledzącym](https://github.com/aspnet/entityframeworkcore/issues/new). Xamarin określonych problemów moduł śledzący problem dla [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) lub [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).

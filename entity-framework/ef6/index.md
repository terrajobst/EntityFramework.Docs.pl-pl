---
title: Omówienie struktury jednostek 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416333"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) jest wypróbowanym i przetestowanym maperem obiekto-relacyjne (O/RM) dla platformy .NET z wieloletnim rozwojem i stabilizacją funkcji.

Jako O/RM, EF6 zmniejsza niezgodność impedancji między światami relacyjnych i obiektowych, umożliwiając deweloperom pisanie aplikacji, które współdziałają z danymi przechowywanymi w relacyjnych bazach danych przy użyciu silnie typizowanych obiektów .NET, które reprezentują domenę aplikacji, i eliminując potrzebę dużej części kodu dostępu do danych "instalacyjnego", który zwykle muszą zapisywać.

EF6 implementuje wiele popularnych funkcji O/RM:
- Mapowanie klas jednostek [POCO,](xref:ef6/resources/glossary#poco) które nie zależą od żadnych typów EF
- Automatyczne śledzenie zmian
- Rozpoznawanie tożsamości i jednostka pracy
- Chętne, leniwe i wyraźne ładowanie
- Tłumaczenie silnie typizowanych zapytań przy użyciu [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)
- Rozbudowa funkcji mapowania, w tym obsługa:
  - Relacje jeden do jednego, jeden do wielu i wiele do wielu
  - Dziedziczenie (tabela według hierarchii, tabela według typu i tabela na klasę betonową)
  - Typy złożone
  - Procedury składowane
- Projektant wizualizacji do tworzenia modeli jednostek.
- Środowisko "Code First" do tworzenia modeli jednostek przez pisanie kodu.
- Modele mogą być generowane z istniejących baz danych, a następnie ręcznie edytowane, lub mogą być tworzone od podstaw, a następnie używane do generowania nowych baz danych.
- Integracja z modelami aplikacji .NET Framework, w tym ASP.NET i za pośrednictwem powiązania danych, z WPF I WinForms.
- Łączność bazy danych w oparciu o ADO.NET i wielu [dostawców](xref:ef6/fundamentals/providers/index) dostępnych do łączenia się z SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, itp.

## <a name="should-i-use-ef6-or-ef-core"></a>Czy należy używać EF6 lub EF Core?

EF Core to bardziej nowoczesna, lekka i rozszerzalna wersja entity framework, która ma bardzo podobne możliwości i korzyści do EF6.
EF Core jest kompletny przepisać i zawiera wiele nowych funkcji nie są dostępne w EF6, chociaż nadal brakuje niektórych z najbardziej zaawansowanych możliwości mapowania EF6.
Należy rozważyć użycie EF Core w nowych aplikacjach, jeśli zestaw funkcji spełnia twoje wymagania.
[Porównaj EF Core & EF6](xref:efcore-and-ef6/index) analizuje ten wybór bardziej szczegółowo.

## <a name="get-started"></a>[Rozpocząć](xref:ef6/get-started)

Dodaj pakiet EnFramework NuGet do projektu lub zainstaluj [narzędzia struktury encji dla programu Visual Studio](https://aka.ms/AA6i8c5). Następnie obejrzyj filmy, przeczytaj samouczki i zaawansowaną dokumentację, aby w pełni wykorzystać ef6.

## <a name="past-entity-framework-versions"></a>Poprzednie wersje struktury encji

Jest to dokumentacja dla najnowszej wersji programu Entity Framework 6, chociaż wiele z nich dotyczy również poprzednich wersji.
Zapoznaj się [z informacjami o wersjach What's New](xref:ef6/what-is-new/index) i [Past, aby](xref:ef6/what-is-new/past-releases) uzyskać pełną listę wydań EF i wprowadzonych przez nich funkcji.

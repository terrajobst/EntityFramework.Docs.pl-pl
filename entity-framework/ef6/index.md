---
title: Przegląd Entity Framework 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656177"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) to próba i przetestowana funkcja mapowania obiektów relacyjnych (O/RM) dla platformy .NET z wieloma latami tworzenia i stabilizacji funkcji.

Jako O/RM EF6 zmniejszają niezgodność zależności między światowymi i zorientowanymi na obiektach, dzięki czemu deweloperzy mogą pisać aplikacje, które współdziałają z danymi przechowywanymi w relacyjnych bazach danych przy użyciu obiektów .NET o jednoznacznie określonym typie, które reprezentują domena aplikacji i eliminuje konieczność użycia dużej części kodu "wodociąging" do uzyskiwania dostępu do danych, które zwykle wymagają zapisu.

EF6 implementuje wiele popularnych funkcji O/RM:
- Mapowanie klas jednostek [poco](xref:ef6/resources/glossary#poco) , które nie są zależne od żadnych typów EF
- Automatyczne śledzenie zmian
- Rozpoznawanie tożsamości i jednostka pracy
- Eager, opóźnienie i jawne ładowanie
- Tłumaczenie zapytań o jednoznacznie określonym typie przy użyciu [LINQ (zapytanie w języku INtegrated Language)](https://aka.ms/AA6hsvu)
- Bogate możliwości mapowania, w tym obsługa:
  - Relacje jeden-do-jednego, jeden-do-wielu i wiele-do-wielu
  - Dziedziczenie (tabela na hierarchię, tabela na typ i tabela dla konkretnej klasy)
  - Typy złożone
  - Procedury składowane
- Projektant wizualny służący do tworzenia modeli jednostek.
- Środowisko "Code First" do tworzenia modeli jednostek przez napisanie kodu.
- Modele można generować z istniejących baz danych, a następnie edytować je ręcznie lub tworzyć od podstaw, a następnie używać do generowania nowych baz danych.
- Integracja z .NET Framework modelami aplikacji, w tym ASP.NET, i za pomocą powiązań danych z WPF i WinForms.
- Łączność z bazą danych oparta na ADO.NET i wielu [dostawcach](xref:ef6/fundamentals/providers/index) dostępnych do łączenia się z SQL Server, Oracle, MySQL, SQLite, POSTGRESQL, DB2 itp.

## <a name="should-i-use-ef6-or-ef-core"></a>Czy należy używać EF6 czy EF Core?

EF Core to bardziej nowoczesny, lekki i rozszerzalny wersja Entity Framework, która ma bardzo podobne możliwości i korzyści EF6.
EF Core to pełny ponowny zapis i zawiera wiele nowych funkcji, które nie są dostępne w EF6, chociaż nadal nie ma niektórych najbardziej zaawansowanych funkcji mapowania EF6.
Jeśli zestaw funkcji spełnia Twoje wymagania, należy rozważyć użycie EF Core w nowych aplikacjach.
[Porównaj EF Core & Ef6](xref:efcore-and-ef6/index) sprawdza ten wybór bardziej szczegółowo.

## <a name="get-startedxrefef6get-started"></a>[Wprowadzenie](xref:ef6/get-started)

Dodaj pakiet NuGet EntityFramework do projektu lub zainstaluj [Entity Framework Tools dla programu Visual Studio](https://aka.ms/AA6i8c5). Następnie Obejrzyj filmy wideo, samouczki odczytywania oraz zaawansowaną dokumentację, aby ułatwić EF6.

## <a name="past-entity-framework-versions"></a>Wcześniejsze wersje Entity Framework

Jest to dokumentacja dotycząca najnowszej wersji programu Entity Framework 6, chociaż większość z nich ma zastosowanie również do poprzednich wersji.
Zapoznaj się z [nowościami](xref:ef6/what-is-new/index) i [poprzednimi wersjami](xref:ef6/what-is-new/past-releases) , aby zapoznać się z pełną listą wydań EF i wprowadzonych przez nie funkcji.

---
title: Przegląd Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: fcd514eebbf09e50403b95c88db04c33520011e9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198059"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) to próba i przetestowana funkcja mapowania obiektów relacyjnych (O/RM) dla platformy .NET z wieloma latami tworzenia i stabilizacji funkcji.

Jako O/RM EF6 zmniejszają niezgodność zależności między światowymi i zorientowanymi na obiektach, dzięki czemu deweloperzy mogą pisać aplikacje, które współdziałają z danymi przechowywanymi w relacyjnych bazach danych przy użyciu obiektów .NET o jednoznacznie określonym typie, które reprezentują domena aplikacji i eliminuje konieczność użycia dużej części kodu "wodociąging" do uzyskiwania dostępu do danych, które zwykle wymagają zapisu.

EF6 implementuje wiele popularnych funkcji O/RM:
- Mapowanie klas jednostek [poco](~/ef6/resources/glossary.md#poco) , które nie są zależne od żadnych typów EF
- Automatyczne śledzenie zmian
- Rozpoznawanie tożsamości i jednostka pracy
- Eager, opóźnienie i jawne ładowanie
- Tłumaczenie zapytań o jednoznacznie określonym typie przy użyciu LINQ (zapytanie w języku INtegrated Language)
- Bogate możliwości mapowania, w tym obsługa:
  - Relacje jeden-do-jednego, jeden-do-wielu i wiele-do-wielu
  - Dziedziczenie (tabela na hierarchię, tabela na typ i tabela dla konkretnej klasy)
  - Typy złożone
  - Procedury składowane
- Projektant wizualny służący do tworzenia modeli jednostek.
- Środowisko "Code First" do tworzenia modeli jednostek przez napisanie kodu.
- Modele można generować z istniejących baz danych, a następnie edytować je ręcznie lub tworzyć od podstaw, a następnie używać do generowania nowych baz danych.
- Integracja z .NET Framework modelami aplikacji, w tym ASP.NET, i za pomocą powiązań danych z WPF i WinForms.
- Łączność z bazą danych oparta na ADO.NET i wielu dostawcach dostępnych do łączenia się z SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 itp.

## <a name="should-i-use-ef6-or-ef-core"></a>Czy należy używać EF6 czy EF Core?

EF Core to bardziej nowoczesny, lekki i rozszerzalny wersja Entity Framework, która ma bardzo podobne możliwości i korzyści EF6.
EF Core to pełny ponowny zapis i zawiera wiele nowych funkcji, które nie są dostępne w EF6, chociaż nadal nie ma niektórych najbardziej zaawansowanych funkcji mapowania EF6.
Jeśli zestaw funkcji spełnia Twoje wymagania, należy rozważyć użycie EF Core w nowych aplikacjach.
[Porównaj EF Core &AMP; Ef6](xref:efcore-and-ef6/index) sprawdza ten wybór bardziej szczegółowo.

## <a name="get-startedef6get-startedmd"></a>[Wprowadzenie](~/ef6/get-started.md)

Dodaj pakiet NuGet EntityFramework do projektu lub zainstaluj Entity Framework Tools dla programu Visual Studio. Następnie Obejrzyj filmy wideo, samouczki odczytywania oraz zaawansowaną dokumentację, aby ułatwić EF6.

## <a name="past-entity-framework-versions"></a>Wcześniejsze wersje Entity Framework

Jest to dokumentacja dotycząca najnowszej wersji programu Entity Framework 6, chociaż większość z nich ma zastosowanie również do poprzednich wersji.
Zapoznaj się z [nowościami](~/ef6/what-is-new/index.md) i [poprzednimi wersjami](~/ef6/what-is-new/past-releases.md) , aby zapoznać się z pełną listą wydań EF i wprowadzonych przez nie funkcji.

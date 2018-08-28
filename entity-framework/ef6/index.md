---
title: Przegląd — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 00e5f36788be599ea2e2b44480107f14e2f026c3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998032"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) jest sprawdzonych obiektowo relacyjny mapowania (O/RM) dla platformy .NET przy użyciu wielu lat pracy nad rozwój funkcji i stabilizacją.

Jako Obiektowo, EF6 zmniejsza niezgodności impedancji między rozwiązań relacyjnych i zorientowane obiektowo, dzięki czemu deweloperzy mogą pisać aplikacje, które współdziałają z danymi przechowywanymi w relacyjnej bazy danych za pomocą silnie typizowanych obiektów platformy .NET, które reprezentują Aplikacja firmy domeny i eliminując potrzebę dużą część zazwyczaj wymaga, aby napisać kod "instalację" dostępu do danych.

EF6 implementuje wiele popularnych funkcji Obiektowo:
- Mapowanie [POCO](~/ef6/resources/glossary.md#poco) klas jednostek, które nie są zależne od żadnych typów EF
- Automatyczne śledzenie zmian
- Rozwiązanie tożsamości i jednostki pracy
- Eager, opóźnieniem i jawne ładowanie
- Tłumaczenie silnie typizowane zapytań za pomocą LINQ (Language INtegrated Query)
- Obsługa zaawansowanych możliwości mapowania, w tym:
  - Relacje jeden do jednego, jeden do wielu i wiele do wielu
  - Dziedziczenie (Tabela w danej hierarchii, tabelę według typu, a tabela na konkretnej klasy)
  - Typy złożone
  - Procedury składowane
- Projektant wizualny pozwala tworzyć modele jednostki.
- "Code First" środowisko do tworzenia modeli entity przez napisanie kodu.
- Modele mogą być generowanym formularzu istniejących baz danych, a następnie ręcznie modyfikować, lub można je tworzyć od podstaw i następnie używany do generowania nowych baz danych.
- Integracja z modeli aplikacji .NET Framework, w tym ASP.NET i za pomocą wiązania danych oraz platforma WPF i WinForms.
- Łączność z bazą danych na podstawie ADO.NET i wielu dostawców można podłączyć do programu SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 itd.

## <a name="should-i-use-ef6-or-ef-core"></a>Należy użyć EF6 i EF Core?

EF Core jest bardziej nowoczesny, uproszczone i rozszerzalny wersję platformy Entity Framework jest bardzo podobne funkcje i korzyści z platformy EF6.
EF Core jest pełne ponowne zapisywanie adresów i zawiera wiele nowych funkcji nie jest dostępna w EF6, mimo że nadal brakuje niektórych najbardziej zaawansowanych możliwości mapowania EF6.
Firma Microsoft zaleca korzystanie z programu EF Core w nowej aplikacji, tak długo, jak zestaw funkcji pasuje do wymagań.
[Porównanie programów EF Core i EF6](xref:efcore-and-ef6/index) sprawdza, czy ten wybór bardziej szczegółowo.

## <a name="get-startedef6get-startedmd"></a>[Wprowadzenie](~/ef6/get-started.md)

Dodaj pakiet NuGet platformy EntityFramework do projektu lub instalowanie narzędzi Entity Framework Tools for Visual Studio. Następnie Obejrzyj wideo, odczytu, samouczki i zaawansowane dokumentację, aby pomóc Państwu jak najlepiej wykorzystać możliwości platformy EF6.

## <a name="past-entity-framework-versions"></a>Wcześniejsze wersje programu Entity Framework

Jest to dokumentację dla najnowszej wersji programu Entity Framework 6, ale część informacji ma zastosowanie również do poprzednich wersjach.
Zapoznaj się z [What's New](~/ef6/what-is-new/index.md) i [wydania w ciągu ostatnich](~/ef6/what-is-new/past-releases.md) pełną listę wydań EF i funkcje one wprowadzone.

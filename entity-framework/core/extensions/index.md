---
title: Narzędzia i rozszerzenia — EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995516"
---
# <a name="ef-core-tools--extensions"></a>EF Core Tools i rozszerzenia

Narzędzia i rozszerzenia oferowanie dodatkowych funkcji dla platformy Entity Framework Core.

> [!IMPORTANT]  
> Rozszerzenia są skompilowanymi z różnych źródeł i nie są przechowywane jako część projektu platformy Entity Framework Core. Rozważając rozszerzenia innych firm, pamiętaj ocenić jakość, licencjonowanie, zgodności i pomocy technicznej, itp., aby upewnić się, że spełniają one wymagania.

## <a name="tools"></a>Narzędzia

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro jest jednostką modelowania rozwiązania z obsługą platformy Entity Framework i Entity Framework Core. Dzięki temu można łatwo zdefiniować model jednostki i dokonaj mapowania go do bazy danych, najpierw przy użyciu bazy danych lub model, dzięki czemu możesz rozpocząć pracę już teraz Pisanie zapytań.

[Witryny sieci Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart jednostki dla deweloperów

Deweloper jednostki jest zaawansowane Projektant ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ to SQL. Można użyć modelu i bazy danych zbliża się do zaprojektować ORM model i wygenerować kodu C# lub Visual Basic .NET dla niego. Jego wprowadza nowe podejście do projektowania Modele ORM, zwiększa wydajność pracy i ułatwia tworzenie aplikacji bazy danych.

[Witryny sieci Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Visual Studio 2017 + rozszerzenie. Możesz odtwarzać klasy DbContext i POCO z istniejącej bazy danych lub projekt bazy danych SQL Server i wizualizować i sprawdzić swoje DbContext na różne sposoby.

[Witryny typu wiki w witrynie GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Rozszerzenia

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Wtyczki dla Microsoft.EntityFrameworkCore do obsługi automatycznie historii zmian danych rejestrowania.

[Repozytorium GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Dynamiczne rozszerzeń Linq dla Microsoft.EntityFrameworkCore, który dodaje asynchroniczna pomoc techniczna

 [Repozytorium GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Podjęto próbę do przechwytywania niektóre dobre lub najlepszych praktyk w interfejsie API obsługującego testów — w tym małe framework do skanowania w poszukiwaniu N + 1 zapytania.

[Repozytorium GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Drugi poziom buforowania biblioteki. Drugi poziom buforowania jest pamięć podręczna zapytań. Wyniki polecenia EF będą przechowywane w pamięci podręcznej, więc, że te same polecenia EF będzie pobrać dane z pamięci podręcznej, zamiast ponownie ich wykonania w bazie danych.

[Repozytorium GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Załadowanie i zapisanie wykresy całego odłączonych jednostek (jednostka wraz z ich obiektów podrzędnych i listy). Czerp inspirację [GraphDiff](https://github.com/refactorthis/GraphDiff/). Chodzi o to również dodać niektóre wtyczki do simplificate powtarzających się zadań, takich jak przeprowadzanie inspekcji i dzielenia na strony.

[Repozytorium GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Pobieranie klucza podstawowego (w tym kluczy złożonych) z dowolnej jednostki jako słownik.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Reaktywne rozszerzenia otoki dla gorących dostrzegalne elementy jednostek platformy Entity Framework.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Dodaj wyzwalacze do jednostek przy użyciu Wstawianie, aktualizowanie i usuwanie zdarzeń. Istnieją trzy zdarzenia dla każdego: przed, po i w przypadku awarii.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Wpisane korzystać OriginalValue swoje właściwości jednostki. Proste i złożone są obsługiwane, są nawigacji/kolekcji.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco udostępnia generator odwrotnego modelu z obsługą Pluralizacja/Singularization oraz szablonów do edycji na podstawie ciągów języka C# 6.0 interpolowane i uruchamiania na.Net Core. Umożliwia także generator skryptów inicjatora ze skryptami scalania SQL i moduł uruchamiający skrypt.

[Repozytorium Github](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore to bezpłatny zestaw rozszerzeń dla programu LINQ do SQL i EntityFrameworkCore użytkownicy zaawansowani. Dzięki obsłudze Include(...) i IDbAsync.

[Repozytorium GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore zawiera przydatne rozszerzenia dotyczące korzystania z LINQ dostawców, takich jak Entity Framework, które obsługują tylko drobne podzbiór funkcji platformy .NET, ponowne użycie funkcji, ponowne napisanie zapytania, nawet nadawania wartość null i tworzenie zapytań dynamicznych za pomocą można przetłumaczyć predykatach i selektory.

[Repozytorium GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Wtyczki dla Microsoft.EntityFrameworkCore do obsługi repozytorium, jednostkę pracy wzorców i wielu bazy danych z transakcji rozproszonych obsługiwane.

[Repozytorium GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Powolne ładowanie dla platformy EF Core 1.1

[Repozytorium GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Rozszerzenia EntityFrameworkCore potrzeby zbiorczych operacji (Insert, Update, Delete).

[Repozytorium GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Dodaje pluralizacja czasu projektowania do programu EF Core.

[Repozytorium GitHub](https://github.com/bricelam/EFCore.Pluralizer)

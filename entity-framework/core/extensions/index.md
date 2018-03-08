---
title: "Narzędzia i rozszerzenia - EF Core"
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="ef-core-tools--extensions"></a>EF podstawowe narzędzia & rozszerzenia

Narzędzia i rozszerzenia oferowanie dodatkowych funkcji Entity Framework podstawowych.

> [!IMPORTANT]  
> Rozszerzenia są utworzony przez różnych źródeł i nie są obsługiwane jako część projektu Entity Framework Core. Rozważając rozszerzenia innych firm, należy ocenić jakości, licencjonowanie, zgodności i pomocy technicznej, itp., aby upewnić się, że spełniają one wymagania.

## <a name="tools"></a>Narzędzia

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro to jednostka modelowania z obsługą programu Entity Framework oraz Entity Framework Core. Dzięki temu można łatwo zdefiniować modelu jednostki i mapowanie go do bazy danych, najpierw przy użyciu bazy danych lub model, więc możesz rozpocząć pracę od razu Pisanie zapytań.

[witryny sieci Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Jednostka Devart Developer

Deweloper jednostki jest zaawansowaną projektanta ORM dla ADO.NET Entity Framework, NHibernate LinqConnect, dostęp do danych strony firmy Telerik i LINQ do SQL. Można użyć pierwszej modelu i bazy danych — pierwszy podejście do projektowania modelu ORM oraz do generowania kodu C# i Visual Basic .NET dla niego. Go wprowadza nowe podejście do projektowania Modele ORM, zwiększa wydajność i ułatwia tworzenie aplikacji bazy danych.

[witryny sieci Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF podstawowe narzędzia zasilania

Visual Studio 2017 + rozszerzenia. Można odtworzyć klasy DbContext i POCO z istniejącej bazy danych lub projektu bazy danych serwera SQL i wizualizacji i sprawdzić Twoje DbContext na różne sposoby.

[Witryna typu wiki w witrynie GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Rozszerzenia

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Dodatek Microsoft.EntityFrameworkCore do obsługi automatycznie historii zmian danych rejestrowania.

[Repozytorium GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Dynamiczne rozszerzenia Linq Microsoft.EntityFrameworkCore, która dodaje obsługę Async

 [Repozytorium GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Próba przechwytywania niektórych dobrej lub najlepszych praktyk w interfejs API, który obsługuje testowania — w tym małe framework skanowania pod kątem N + 1 zapytania.

[Repozytorium GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Drugi poziom buforowania biblioteki. Drugi poziom buforowania jest to pamięć podręczna zapytania. Wyniki polecenia EF będą przechowywane w pamięci podręcznej, dzięki czemu te same polecenia EF pobierze dane z pamięci podręcznej niż wykonywanie ich ponownie w bazie danych.

[Repozytorium GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Załadowanie i zapisanie wykresy całego odłączyć jednostek (jednostka z ich obiektów podrzędnych i list). Inspirowana przez [GraphDiff](https://github.com/refactorthis/GraphDiff/). Ma to również dodać niektóre dodatki plug-in do simplificate powtarzających się zadań, takich jak przeprowadzanie inspekcji i podział na strony.

[Repozytorium GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Pobrać klucza podstawowego (w tym klucze złożone) z dowolnej jednostki jako słownik.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Rozszerzenie reaktywne otoki dla gorących dostrzegalne elementy jednostek Entity Framework.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Dodaj wyzwalaczy obiekty z wstawiania, aktualizowania i usuwania zdarzenia. Istnieją trzy zdarzenia dla każdego: przed, po i w przypadku awarii.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Uzyskiwanie dostępu typu do OriginalValue z właściwości obiektu. Właściwości proste i złożone są obsługiwane, są nawigacji/kolekcji.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco zapewnia generator wstecznego modelu z obsługą określania liczby mnogiej/Singularization oraz szablonów do edycji oparte na ciągi interpolowane 6.0 C# i uruchomiona na platformie .net Core. Umożliwia także generator skryptów inicjatora scalania SQL skryptów i runner skryptu.

[Repozytorium Github](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore to bezpłatna zestaw rozszerzeń dla LINQ do SQL i EntityFrameworkCore użytkownicy zaawansowani. Z obsługą Include(...) i IDbAsync.

[Repozytorium GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore udostępnia przydatne rozszerzenia dotyczące korzystania z LINQ dostawców, takich jak Entity Framework obsługujących niewielkie podzbiór funkcji .NET ponowne użycie funkcji, ponowne zapisywanie zapytań, nawet nadawania null bezpieczny i tworzenia zapytań dynamicznych przy użyciu predykatów można przetłumaczyć i selektorów.

[Repozytorium GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Dodatek Microsoft.EntityFrameworkCore do obsługi repozytorium, jednostki pracy i wielu bazy danych z obsługiwanych transakcji rozproszonej.

[Repozytorium GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Ładowania opóźnionego dla EF Core 1.1

[Repozytorium GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Rozszerzenia EntityFrameworkCore operacje zbiorcze (Insert, Update, Delete).

[Repozytorium GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

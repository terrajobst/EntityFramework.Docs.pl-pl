---
title: Narzędzia & rozszerzenia — EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e70011b42818e4df1ec5b9b88d7adb9d36bb26f1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654799"
---
# <a name="ef-core-tools--extensions"></a>Rozszerzenia narzędzi EF Core &

Te narzędzia i rozszerzenia zapewniają dodatkową funkcjonalność dla Entity Framework Core 2,0 i nowszych.

> [!IMPORTANT]  
> Rozszerzenia są tworzone przez różne źródła i nie są obsługiwane w ramach projektu Entity Framework Core. Biorąc pod uwagę rozszerzenie innej firmy, pamiętaj o ocenie jego jakości, licencjonowania, zgodności, wsparcia itp., aby upewnić się, że spełnia Twoje wymagania.

## <a name="tools"></a>Narzędzia

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro to rozwiązanie do modelowania jednostek z obsługą Entity Framework i Entity Framework Core. Umożliwia ona łatwe definiowanie modelu jednostki i mapowanie go do bazy danych przy użyciu najpierw pierwszej lub modelu bazy danych, dzięki czemu możesz od razu zacząć pisać zapytania.

[Producenta](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Deweloper jednostki Devart

Deweloper jednostki jest zaawansowanym projektantem ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ to SQL. Obsługuje ona projektowanie EF Core modeli wizualnie, przy użyciu pierwszej metody modelu lub pierwszej podejścia do C# bazy danych i lub Visual Basic generowania kodu.

[Producenta](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core narzędzia do zarządzania

EF Core PowerShell to rozszerzenie programu Visual Studio 2017, które uwidacznia różne EF Core zadania czasu projektowania w prostym interfejsie użytkownika. Obejmuje ona odtwarzanie klas DbContext i Entity Classes z istniejących baz danych, a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), zarządzanie migracjami baz danych i wizualizacje modeli.

[Witryna typu wiki usługi GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Entity Framework edytor wizualny

Entity Framework edytorem wizualnym jest rozszerzenie programu Visual Studio, które dodaje projektanta ORM do projektowania wizualizacji Dr 6 i klasy EF Core. Kod jest generowany przy użyciu szablonów T4, więc można go dostosować do własnych potrzeb. Obsługuje dziedziczenie, dwukierunkowe i dwukierunkowe skojarzenia, wyliczenia oraz możliwość kolorowania kodu klas i Dodawanie bloków tekstowych, aby wyjaśnić potencjalnie specjalne części projektu.

[Transakcji](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory to aparat tworzenia szkieletów dla platformy .NET Core, który umożliwia automatyzację generacji klas DbContext, jednostek, konfiguracji mapowania i klas repozytorium z bazy danych SQL Server.

[Repozytorium GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Generator Entity Framework Core LoreSoft

Generator Entity Framework Core (EFG) to narzędzie interfejs wiersza polecenia platformy .NET Core, które może generować modele EF Core z istniejącej bazy danych, podobnie jak `dotnet ef dbcontext scaffold`, ale również zapewnia bezpieczną [regenerację](https://efg.loresoft.com/en/latest/regeneration/) kodu przez zastąpienie regionu lub analizowanie plików mapowania. To narzędzie obsługuje generowanie modeli widoku, walidacji i kodu mapowania obiektów.

[Samouczek](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentacja](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>rozszerzenia

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft. EntityFrameworkCore. AutoHistory

Biblioteka wtyczek, która umożliwia automatyczne rejestrowanie zmian danych wykonywanych przez EF Core w tabeli historii.

[Repozytorium GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft. EntityFrameworkCore. DynamicLinq

Port .NET Core/.NET Standard typu System. LINQ. Dynamic, który obejmuje obsługę asynchroniczną z EF Core.
System. LINQ. Dynamic pochodzi z przykładu firmy Microsoft, który pokazuje, jak tworzyć zapytania LINQ w sposób dynamiczny z wyrażeń ciągów zamiast kodu.

[Repozytorium GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache. Core

Rozszerzenie, które umożliwia przechowywanie wyników zapytań EF Core w pamięci podręcznej drugiego poziomu, tak aby kolejne wykonania tych samych zapytań mogły uniknąć dostępu do bazy danych i pobierać dane bezpośrednio z pamięci podręcznej.

[Repozytorium GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore. PrimaryKey

Ta biblioteka umożliwia pobieranie wartości klucza podstawowego (w tym kluczy złożonych) z dowolnej jednostki jako słownika.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Ta biblioteka umożliwia dostęp do oryginalnych wartości właściwości jednostki przy użyciu jednoznacznie określonego typu.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (konsola generatora) to prosty generator kodu oparty na projekcie konsoli, który działa na platformie .NET Core i używa C# interpolowanych ciągów do generowania kodu. Geco obejmuje generator modelu Odwróć dla EF Core z obsługą szablonów pluralizacja, singularization i edytowalnych. Udostępnia również Generator skryptów danych inicjatora, moduł uruchamiający skrypty i oczyszczarkę bazy danych.

[Repozytorium GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit. Microsoft. EntityFrameworkCore

LinqKit. Microsoft. EntityFrameworkCore to zgodna z EF Core wersja biblioteki LINQKit. LINQKit to bezpłatny zestaw rozszerzeń dla LINQ to SQL i Entity Framework użytkowników zaawansowanych. Umożliwia zaawansowane funkcje, takie jak dynamiczne Kompilowanie wyrażeń predykatu i Używanie zmiennych wyrażeń w podzapytaniach.  

[Repozytorium GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq rozszerza dostawców LINQ, takich jak Entity Framework, aby włączyć ponowne używanie funkcji, zapisywania zapytań i kompilowania zapytań dynamicznych przy użyciu predykatów z możliwością tłumaczenia i selektorów.

[Repozytorium GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft. EntityFrameworkCore. UnitOfWork

Wtyczka dla elementu Microsoft. EntityFrameworkCore do obsługi repozytorium, wzorców jednostek roboczych i wielu baz danych z obsługiwaną transakcją rozproszoną.

[Repozytorium GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Rozszerzenia EF Core dla operacji zbiorczych (INSERT, Update i Delete).

[Repozytorium GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Dodaje pluralizacja czasu projektowania do EF Core.

[Repozytorium GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/pomelo. EntityFrameworkCore. Extensions. ToSql

Prosta metoda rozszerzająca, która uzyskuje instrukcję SQL EF Core wygenerowaną dla danego zapytania LINQ w prostych scenariuszach. Metoda ToSql jest ograniczona do prostych scenariuszy, ponieważ EF Core może generować więcej niż jedną instrukcję SQL dla jednej kwerendy LINQ i różne instrukcje SQL w zależności od wartości parametrów.

[Repozytorium GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt. EntityFrameworkCore. Indexattribute

Revival [index] atrybutu dla EF Core (z rozszerzeniem dla kompilowania modelu).

[Repozytorium GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Zawiera otokę wokół EF Core dostawcy bazy danych w pamięci. Sprawia, że działa tak samo jak dostawca relacyjny.

[Repozytorium GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementacja obsługi danych czasowych dla EF Core.

[Repozytorium GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore. buforowanie

Pamięć podręczna zapytań o wysokiej wydajności dla EF Core.

[Repozytorium GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Rozszerza kontekst DbContext z funkcjami takimi jak: Filter include, Audit, buforowanie, Future Query, Batch Delete, Batch Update i innych.

[Witryna internetowa](https://entityframework-plus.net/)
[repozytorium GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Rozszerzenia Entity Framework

Rozszerza swój kontekst dbwith operacji zbiorczych o wysokiej wydajności: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge i inne.

[Producenta](https://entityframework-extensions.net/)

### <a name="reconciler"></a>Uzgadnianie

Aktualizowanie grafu jednostki w sklepie do danego elementu przez wstawienie, zaktualizowanie i usunięcie odpowiednich jednostek.

[Repozytorium GitHub](https://github.com/jtheisen/reconciler)

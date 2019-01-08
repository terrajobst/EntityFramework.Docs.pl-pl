---
title: Narzędzia i rozszerzenia — EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 414773284df7c208b9a2acf0536fda459bdf775b
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069233"
---
# <a name="ef-core-tools--extensions"></a>EF Core Tools i rozszerzenia

Te narzędzia i rozszerzenia zapewniają dodatkowe funkcje dla programu Entity Framework Core 2.0 i nowszych.

> [!IMPORTANT]  
> Rozszerzenia są tworzone za pomocą różnych źródeł i nie są przechowywane jako część projektu platformy Entity Framework Core. Rozważając rozszerzenia innych firm, należy ocenić jego jakość licencjonowania, zgodności i pomocy technicznej, itp., aby upewnić się, że spełnia on wymagania.

## <a name="tools"></a>Narzędzia

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro jest jednostką modelowania rozwiązania z obsługą platformy Entity Framework i Entity Framework Core. Dzięki temu można łatwo zdefiniować model jednostki i dokonaj mapowania go do bazy danych, najpierw przy użyciu bazy danych lub model, dzięki czemu możesz rozpocząć pracę już teraz Pisanie zapytań.

[Witryny sieci Web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart jednostki dla deweloperów

Deweloper jednostki jest zaawansowane Projektant ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ to SQL. Go obsługuje projektowanie wizualnie modeli programu EF Core, najpierw przy użyciu modelu lub bazy danych najpierw zbliża się, a C# lub generowanie kodu w języku Visual Basic. 

[Witryny sieci Web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools to rozszerzenie programu Visual Studio 2017, które udostępnia różne zadania projektowania programu EF Core w prosty interfejs użytkownika. Obejmuje odtwarzanie klasy DbContext i jednostki z istniejących baz danych i [programu SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), zarządzanie migracje baz danych i wizualizacje modelu.

[Witryny typu wiki w witrynie GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Edytor programu Entity Framework Visual

Edytor programu Entity Framework Visual to rozszerzenie programu Visual Studio 2017, które dodaje Projektant ORM dla projektowania wizualnego programów EF 6 i klas programu EF Core. Kod jest generowany przy użyciu szablonów T4, dzięki czemu można dostosować do dowolnego potrzeb. Obsługuje dziedziczenie, jednokierunkowe i powiązania dwukierunkowego, wyliczenia i możliwość kolorować klas i Dodaj bloki tekstu, aby wyjaśnić, potencjalnie specjalne części projektu.

[Portal Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory to aparat tworzenia szkieletów dla platformy .NET Core, który można zautomatyzować Generowanie klasy DbContext, jednostki, mapowania konfiguracji i repozytorium klasy z bazy danych programu SQL Server.

[Repozytorium GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Firmy LoreSoft Entity Framework Core Generator

Entity Framework Core Generator (efg) jest narzędziem wiersza polecenia platformy .NET Core z można wygenerować modeli programu EF Core istniejącą bazę danych, podobnie jak `dotnet ef dbcontext scaffold`, lecz również obsługuje bezpieczne kodu [ponownego wygenerowania](https://efg.loresoft.com/en/latest/regeneration/) za pomocą zastąpienia regionu lub przez analizowanie Pliki mapowania. To narzędzie obsługuje generowania modeli widoków, weryfikacji i obiektu mapowania kodu. 

[Samouczek](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[dokumentacji](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Rozszerzenia

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Biblioteka wtyczkę, która umożliwia automatyczne rejestrowanie zmian danych, wykonywane przez platformę EF Core w tabeli historii.

[Repozytorium GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

.NET Core / .NET Standard port z System.Linq.Dynamic z obsługą async z programem EF Core.
System.Linq.Dynamic pochodzi jako przykład firmy Microsoft, który pokazuje, jak tworzyć zapytania LINQ dynamicznie z wyrażenia ciągu zamiast kodu.

[Repozytorium GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Rozszerzenie, które umożliwia przechowywanie wyników zapytania programu EF Core do pamięci podręcznej drugiego poziomu, tak aby kolejne wykonania tego samego zapytania można uniknąć, uzyskiwanie dostępu do bazy danych i pobierać dane bezpośrednio z pamięci podręcznej.

[Repozytorium GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Ta biblioteka umożliwia pobieranie wartości klucza podstawowego (w tym kluczy złożonych) z dowolnej jednostki jako słownik.

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Ta biblioteka umożliwia silnie typizowany dostęp do oryginalnych wartości właściwości jednostki. 

[Repozytorium GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Generator konsoli) jest generator prostego kodu na podstawie projektu konsoli, działa na platformie .NET Core, która używa C# interpolowane ciągów w celu generowania kodu. Geco zawiera generator modelu odwróconego dla platformy EF Core dzięki obsłudze pluralizacja, singularization i szablonów do edycji. Umożliwia także skrypt generator danych inicjatora, runner skryptu i bazę danych czyszcząca.

[Repozytorium GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore jest zgodnych z programem EF Core wersji biblioteki LINQKit. LINQKit to bezpłatny zestaw rozszerzeń dla programu LINQ do SQL i Entity Framework użytkownicy zaawansowani. Umożliwia ona zaawansowane funkcje, takie jak dynamiczne tworzenie predykatu wyrażeń i używanie zmiennych wyrażenia w podzapytania.  

[Repozytorium GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq rozszerza dostawców LINQ takim jak Entity Framework, aby umożliwić ponowne używanie funkcji ponowne napisanie zapytania i kompilowania zapytań dynamicznych przy użyciu można przetłumaczyć predykatach i selektory.

[Repozytorium GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Wtyczki dla Microsoft.EntityFrameworkCore umożliwia obsługę repozytorium, jednostkę pracy wzorców i wielu baz danych w transakcji rozproszonej, obsługiwane.

[Repozytorium GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

EF Core rozszerzenia potrzeby zbiorczych operacji (Insert, Update, Delete).

[Repozytorium GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Dodaje pluralizacja czasu projektowania do programu EF Core.

[Repozytorium GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Metody rozszerzenia prosty, która uzyskuje instrukcji SQL programu EF Core wygeneruje dla danego zapytania LINQ w prostych scenariuszy. Metoda ToSql jest ograniczone do prostych scenariuszy, ponieważ programu EF Core można wygenerować więcej niż jedną instrukcję SQL dla pojedynczego zapytania LINQ i różne instrukcje SQL, w zależności od wartości parametrów.

[Repozytorium GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Revival atrybutu [Index] dla platformy EF Core (z rozszerzeniem do konstruowania modelu).

[Repozytorium GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Udostępnia otokę dostawcy bazy danych programu EF Core w pamięci. Sprawia, że więcej zachowywać się jak relacyjne dostawcy.

[Repozytorium GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementacja danych czasowych pomocy technicznej dla platformy EF Core.

[Repozytorium GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Pamięć podręczną zapytań drugiego poziomu o wysokiej wydajności dla platformy EF Core.

[Repozytorium GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

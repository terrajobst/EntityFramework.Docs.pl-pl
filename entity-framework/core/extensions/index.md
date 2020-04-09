---
title: Narzędzia & rozszerzenia - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634241"
---
# <a name="ef-core-tools--extensions"></a>EF Core Tools & rozszerzenia

Te narzędzia i rozszerzenia zapewniają dodatkowe funkcje dla entity framework core 2.1 i nowsze.

> [!IMPORTANT]  
> Rozszerzenia są tworzone przez różne źródła i nie są obsługiwane w ramach projektu Entity Framework Core. Rozważając rozszerzenie innej firmy, pamiętaj, aby ocenić jego jakość, licencjonowanie, kompatybilność, wsparcie itp., aby upewnić się, że spełnia Twoje wymagania. W szczególności rozszerzenie utworzone dla starszej wersji ef core może wymagać aktualizacji, zanim będzie działać z najnowszymi wersjami.

## <a name="tools"></a>Narzędzia

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro to rozwiązanie do modelowania jednostek z obsługą entity framework i Entity Framework Core. Umożliwia łatwe definiowanie modelu jednostki i mapowanie go do bazy danych, najpierw przy użyciu bazy danych lub modelu, dzięki czemu można rozpocząć pisanie zapytań od razu. Dla EF Core: 2.

[witryna sieci web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer jest zaawansowanym projektantem ORM dla ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access i LINQ do SQL. Obsługuje projektowanie modeli EF Core wizualnie, przy użyciu modelu pierwszy lub bazy danych pierwsze podejścia i C# lub Visual Basic generowania kodu. Dla EF Core: 2.

[witryna sieci web](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>nHydrate ORM for Entity Framework

Orm, który tworzy silnie typizowane, rozszerzalne klasy dla entity framework. Wygenerowany kod to Entity Framework Core. Nie ma żadnej różnicy. Nie jest to zamiennik ef lub niestandardowego ORM. Jest to warstwa wizualna, modelowania, która umożliwia zespołowi zarządzanie złożonymi schematami bazy danych. Dobrze współpracuje z oprogramowaniem SCM, takim jak Git, umożliwiając dostęp wielu użytkowników do modelu przy minimalnych konfliktach. Instalator śledzi zmiany modelu i tworzy skrypty uaktualnienia. Dla EF Core: 3.

[Strona Github](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools to rozszerzenie programu Visual Studio, które udostępnia różne zadania ef core projektowania w prostym interfejsie użytkownika. Obejmuje inżynierię odwrotną DbContext i klasy jednostek z istniejących baz danych i [DACPAc programu SQL Server,](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)zarządzanie migracjami baz danych i wizualizacje modelu. Dla EF Core: 2, 3.

[Wiki GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Edytor wizualny programu Entity Framework

Edytor wizualny programu Entity Framework to rozszerzenie programu Visual Studio, które dodaje projektanta ORM do projektowania wizualnego klas EF 6 i EF Core. Kod jest generowany przy użyciu szablonów T4, dzięki czemu można dostosować do każdego celu. Obsługuje dziedziczenie, jednokierunkowe i dwukierunkowe skojarzenia, wyliczenia i możliwość kolor kodowania klas i dodawać bloki tekstu, aby wyjaśnić potencjalnie tajemne części projektu. Dla EF Core: 2.

[Rynek](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>Fabryka Kotów

CatFactory to aparat szkieletów dla platformy .NET Core, który może zautomatyzować generowanie klas DbContext, jednostek, konfiguracji mapowania i klas repozytorium z bazy danych programu SQL Server. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Generator rdzenia entity Framework firmy LoreSoft

Entity Framework Core Generator (efg) to narzędzie .NET Core CLI, które może `dotnet ef dbcontext scaffold`generować modele EF Core z istniejącej bazy danych, podobnie jak , ale obsługuje również [bezpieczną regenerację](https://efg.loresoft.com/en/latest/regeneration/) kodu poprzez zastępowanie regionu lub analizowanie plików mapowania. To narzędzie obsługuje generowanie modeli widoku, sprawdzania poprawności i kodu mapera obiektów. Dla EF Core: 2.

[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentacja samouczka](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Rozszerzenia

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Biblioteka wtyczek, która umożliwia automatyczne rejestrowanie zmian danych wykonywanych przez EF Core w tabeli historii. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Rozszerzenie, które umożliwia przechowywanie wyników zapytań EF Core w pamięci podręcznej drugiego poziomu, dzięki czemu kolejne wykonanie tych samych kwerend można uniknąć dostępu do bazy danych i pobrać dane bezpośrednio z pamięci podręcznej. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

Geco (Generator Console) to prosty generator kodu oparty na projekcie konsoli, który działa na .NET Core i używa interpolowanych ciągów C# do generowania kodu. Geco zawiera generator modelu wstecznego dla EF Core z obsługą pluralizacji, singularization i edytowalnych szablonów. Zapewnia również generator skryptów danych źródłowych, moduł przesiewowy skryptów i czyszczenie bazy danych. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Kierownica 

Umożliwia dostosowanie klas inżynierii odwrotnej z istniejącej bazy danych przy użyciu programu Entity Framework Core toolchain z szablonami kierownicy. Dla EF Core: 2, 3.

[Repozytorium GitHub](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

NeinLinq rozszerza dostawców LINQ, takich jak Entity Framework, aby umożliwić ponowne użycie funkcji, przepisywanie zapytań i tworzenie zapytań dynamicznych przy użyciu tłumaczonych predykatów i selektorów. Dla EF Core: 2, 3.

[Repozytorium GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Wtyczka dla Microsoft.EntityFrameworkCore do obsługi repozytorium, jednostki wzorców pracy i wielu baz danych z transakcjami rozproszonymi obsługiwane. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkWybory

Rozszerzenia EF Core dla operacji zbiorczych (Wstaw, Aktualizacja, Usuń). Dla EF Core: 2, 3.

[Repozytorium GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Dodaje pluralizm w czasie projektowania. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Atrybut toolbelt.entityFrameworkCore.IndexAttribute

Odrodzenie atrybutu [Index] (z rozszerzeniem dla budynku modelu). Dla EF Core: 2, 3.

[Repozytorium GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryPomowacze

Udostępnia otokę wokół dostawcy bazy danych EF Core w pamięci. Sprawia, że działa bardziej jak dostawca relacyjne. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Wdrożenie wsparcia czasowego. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>Tabela EfCoreTemporalTable

Łatwe wykonywanie zapytań czasowych w ulubionej `AsTemporalAll()`bazie `AsTemporalAsOf(date)` `AsTemporalFrom(startDate, endDate)`danych `AsTemporalBetween(startDate, endDate)` `AsTemporalContained(startDate, endDate)`za pomocą wprowadzonych metod rozszerzenia: , , , . Dla EF Core: 3.

[Repozytorium GitHub](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EfCore.TimeTraveler

Zezwalaj na w pełni funkcjonalne zapytania Entity Framework Core względem [historii czasowej programu SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) przy użyciu kodu EF Core, jednostek i mapowań, które zostały już zdefiniowane.  Podróżuj w czasie, `using (TemporalQuery.AsOf(targetDateTime)) {...}`zawijając kod w pliku . Dla EF Core: 3.

[Repozytorium GitHub](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

Biblioteka rozszerzeń dla entity framework core, który umożliwia deweloperom, którzy używają programu SQL Server łatwo używać tabel czasowych. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>Element EntityFrameworkCore.Cacheable

Wysokowydajna pamięć podręczna zapytań drugiego poziomu. Dla EF Core: 2.

[Repozytorium GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Rozszerza dbContext o funkcje, takie jak: Filtr, Inspekcja, Buforowanie, Przyszłość kwerendy, Usuwanie partii, Aktualizacja wsadowa i inne. Dla EF Core: 2, 3.

[Website](https://entityframework-plus.net/)
[Repozytorium GitHub witryny](https://github.com/zzzprojects/EntityFramework-Plus) sieci Web

### <a name="entity-framework-extensions"></a>Rozszerzenia struktury encji

Rozszerza DbContext o wysokiej wydajności operacji masowych: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge i więcej. Dla EF Core: 2, 3.

[witryna sieci web](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Wyekspresyfikacja

Dodaj obsługę wywoływania metod rozszerzenia w linq lambdas. Dla EF Core: 3.1

[Repozytorium GitHub](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a>XLinq (własno)

Technologia linq (Language Integrated Query) dla relacyjnych baz danych. Umożliwia użycie języka C# do pisania silnie wpisanych zapytań. Dla EF Core: 3.1

- Pełna obsługa języka C# dla tworzenia zapytań: wiele instrukcji wewnątrz lambda, zmienne, funkcje itp.
- Brak luki semantycznej z SQL. XLinq deklaruje instrukcje `SELECT`SQL `FROM` `WHERE`(jak , , ) jako metody pierwszej klasy C#, łącząc znaną składnię z intellisense, bezpieczeństwa typu i refaktoryzacji.

W rezultacie SQL staje się po prostu "inną" biblioteką klas eksponującą swój interfejs API lokalnie, dosłownie *"Language Integrated SQL"*.

[witryna sieci web](http://xlinq.live/)

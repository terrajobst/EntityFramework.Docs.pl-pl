---
title: Co nowego w EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136258"
---
# <a name="whats-new-in-ef-core-50"></a>Co nowego w EF Core 5,0

EF Core 5,0 jest obecnie w trakcie opracowywania.
Ta strona będzie zawierać przegląd interesujących zmian wprowadzonych w każdej wersji zapoznawczej.

Ta strona nie duplikuje [planu dla EF Core 5,0](plan.md).
Plan opisuje ogólne motywy dla EF Core 5,0, w tym wszystko, co planujemy uwzględnić przed wysyłką ostatecznej wersji.

Będziemy dodawać linki z tego miejsca do oficjalnej dokumentacji w trakcie jej publikacji.

## <a name="preview-1"></a>Wersja zapoznawcza 1

### <a name="simple-logging"></a>Rejestrowanie proste

Ta funkcja dodaje funkcje podobne do `Database.Log` w EF6.
Oznacza to, że zapewnia prosty sposób pobierania dzienników z EF Core bez konieczności konfigurowania żadnego rodzaju zewnętrznej platformy rejestrowania.

Wstępna dokumentacja jest zawarta w [statusie tygodnia Ef 5 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Dodatkowa dokumentacja jest śledzona przez [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)problemu.

### <a name="simple-way-to-get-generated-sql"></a>Prosty sposób uzyskiwania wygenerowanego kodu SQL

EF Core 5,0 wprowadza metodę rozszerzenia `ToQueryString`, która zwróci kod SQL, który zostanie wygenerowany przez EF Core podczas wykonywania zapytania LINQ.

Wstępna dokumentacja jest uwzględniona w [statusie tygodniowym EF dla 9 stycznia 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

Dodatkowa dokumentacja jest śledzona przez [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)problemu.

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Użyj C# atrybutu, aby wskazać, że jednostka nie ma klucza

Typ jednostki można teraz skonfigurować jako bez klucza przy użyciu nowego `KeylessAttribute`.
Na przykład:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Dokumentacja jest śledzona przez [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)problemu.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Parametry Connection lub Connection można zmienić w zainicjowanym kontekście DbContext

Teraz łatwiej jest utworzyć wystąpienie DbContext bez żadnego połączenia lub parametrów połączenia.
Ponadto parametry połączenia lub połączenia można teraz przystąpić do wystąpienia kontekstu.
Pozwala to temu samemu wystąpieniu kontekstu na dynamiczne łączenie się z różnymi bazami danych.

Dokumentacja jest śledzona przez [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problemu.

### <a name="change-tracking-proxies"></a>Serwery proxy śledzenia zmian

EF Core mogą teraz generować serwery proxy środowiska uruchomieniowego, które automatycznie implementują [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) i [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Następnie te zmiany są raportowane w oparciu o właściwości jednostki bezpośrednio do EF Core, unikając konieczności skanowania pod kątem zmian.
Jednak serwery proxy są dostarczane z własnym zestawem ograniczeń, więc nie są przeznaczone dla wszystkich użytkowników.

Dokumentacja jest śledzona przez [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problemu.

### <a name="enhanced-debug-views"></a>Udoskonalone widoki debugowania

Widoki debugowania to prosty sposób na zajrzeć do wewnętrznych EF Core w przypadku problemów z debugowaniem.
Widok debugowania dla modelu został zaimplementowany jakiś czas temu.
W przypadku EF Core 5,0 ten widok modelu jest łatwiejszy do odczytania i dodania nowego widoku debugowania dla śledzonych jednostek w Menedżerze stanu.

Wstępna dokumentacja jest uwzględniona w [Stanach tygodnia EF 12 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Dodatkowa dokumentacja jest śledzona przez [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)problemu.

### <a name="improved-handling-of-database-null-semantics"></a>Ulepszona obsługa semantyki o wartości null bazy danych

Relacyjne bazy danych zazwyczaj traktują wartości NULL jako nieznaną wartość i w związku z tym nie są równe żadnym innym NULL.
C#z drugiej strony traktuje wartość null jako zdefiniowaną wartość, która porównuje ją z innymi wartościami null.
EF Core domyślnie tłumaczy zapytania, tak aby korzystały C# z semantyki o wartości null.
EF Core 5,0 znacznie poprawia wydajność tych tłumaczeń.

Dokumentacja jest śledzona przez [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problemu.

### <a name="indexer-properties"></a>Właściwości indeksatora

EF Core 5,0 obsługuje mapowanie właściwości C# indeksatora.
Dzięki temu jednostki mogą działać jako zbiory właściwości, w których kolumny są mapowane na nazwane właściwości w zbiorze.

Dokumentacja jest śledzona przez [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problemu.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generowanie ograniczeń check dla mapowań wyliczenia

Migracje EF Core 5,0 mogą teraz generować ograniczenia CHECK dla mapowań właściwości enum.
Na przykład:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

Dokumentacja jest śledzona przez [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problemu.

### <a name="isrelational"></a>Isrelacyjne

Oprócz istniejących `IsSqlServer`, `IsSqlite`i `IsInMemory`dodano nową metodę `IsRelational`.
Można go użyć do sprawdzenia, czy DbContext używa dowolnego dostawcy relacyjnej bazy danych.
Na przykład:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Dokumentacja jest śledzona przez [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)problemu.

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos optymistyczne współbieżność za pomocą elementów ETag

Dostawca bazy danych Azure Cosmos DB obsługuje teraz optymistyczne współbieżność za pomocą elementów ETag.
Użyj konstruktora modeli w OnModelCreating, aby skonfigurować element ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

Metody SaveChanges następnie zgłosi `DbUpdateConcurrencyException` w konflikcie współbieżności, który [można obsłużyć](https://docs.microsoft.com/ef/core/saving/concurrency) w celu zaimplementowania ponownych prób itd.


Dokumentacja jest śledzona przez [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)problemu.

### <a name="query-translations-for-more-datetime-constructs"></a>Tłumaczenie zapytań dla większej liczby konstrukcji DateTime

Zapytania zawierające nową konstrukcję DateTime są teraz tłumaczone.

Ponadto następujące funkcje SQL Server są teraz mapowane:
* DateDiffWeek
* DateFromParts

Na przykład:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.

### <a name="query-translations-for-more-byte-array-constructs"></a>Tłumaczenie zapytania dla większej liczby konstrukcji tablicy

Zapytania używające właściwości Contains, Length, SequenceEqual itp. on-Byte [] są teraz tłumaczone na SQL.

Wstępna dokumentacja jest zawarta w [statusie tygodnia Ef 5 grudnia 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Dodatkowa dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.

### <a name="query-translation-for-reverse"></a>Tłumaczenie zapytania do tyłu

Zapytania korzystające z `Reverse` są teraz tłumaczone.
Na przykład:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.

### <a name="query-translation-for-bitwise-operators"></a>Tłumaczenie zapytania dla operatorów bitowych

Zapytania wykorzystujące operatory bitowe są teraz tłumaczone na przykład w większej liczbie przypadków:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.

### <a name="query-translation-for-strings-on-cosmos"></a>Tłumaczenie zapytania dla ciągów w Cosmos

Zapytania korzystające z metod String zawierają, StartsWith i EndsWith są teraz tłumaczone przy użyciu dostawcy Azure Cosmos DB.

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemu.

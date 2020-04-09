---
title: Co nowego w EF Core 5.0
description: Przegląd nowych funkcji w EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634281"
---
# <a name="whats-new-in-ef-core-50"></a>Co nowego w EF Core 5.0

EF Core 5.0 jest obecnie w fazie rozwoju.
Ta strona będzie zawierać przegląd interesujących zmian wprowadzonych w każdej wersji zapoznawczej.

Ta strona nie powiela [planu ef core 5.0](plan.md).
Plan opisuje ogólne tematy ef core 5.0, w tym wszystko, co planujemy uwzględnić przed wysłaniem ostatecznej wersji.

Dodamy linki stąd do oficjalnej dokumentacji, jak to jest publikowane.

## <a name="preview-2"></a>Preview 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Użyj atrybutu C#, aby określić pole zapasowe właściwości

Atrybut C# może teraz służyć do określania pola zapasowego dla właściwości.
Ten atrybut umożliwia EF Core nadal zapisywać i odczytywać z pola zapasowego, jak zwykle się zdarzyć, nawet wtedy, gdy pole zapasowe nie można znaleźć automatycznie.
Przykład:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

Dokumentacja jest śledzona przez [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)problemowe .

### <a name="complete-discriminator-mapping"></a>Kompletne mapowanie dyskryminatora

EF Core używa kolumny rozróżniacza do [mapowania TPH hierarchii dziedziczenia](/ef/core/modeling/inheritance).
Niektóre ulepszenia wydajności są możliwe, tak długo, jak EF Core zna wszystkie możliwe wartości dla dyskryminatora.
EF Core 5.0 implementuje teraz te ulepszenia.

Na przykład poprzednie wersje EF Core zawsze generować ten SQL dla kwerendy zwracając wszystkie typy w hierarchii:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

EF Core 5.0 wygeneruje teraz następujące elementy po skonfigurowaniu pełnego mapowania dyskryminatora:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Będzie to domyślne zachowanie, począwszy od wersji zapoznawczej 3.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Ulepszenia wydajności w witrynie Microsoft.Data.Sqlite

Dokonaliśmy dwóch ulepszeń wydajności dla sqlite:

* Pobieranie danych binarnych i ciągów za pomocą GetBytes, GetChars i GetTextReader jest teraz bardziej wydajne, korzystając z SqliteBlob i strumieni.
* Inicjowanie SqliteConnection jest teraz leniwy.

Te ulepszenia są w ADO.NET microsoft.data.sqlite dostawcy, a tym samym również zwiększyć wydajność poza EF Core.

## <a name="preview-1"></a>Wersja zapoznawcza 1

### <a name="simple-logging"></a>Proste rejestrowanie

Ta funkcja dodaje funkcje podobne do `Database.Log` w EF6.
Oznacza to, że zapewnia prosty sposób, aby uzyskać dzienniki z EF Core bez konieczności konfigurowania wszelkiego rodzaju zewnętrznej struktury rejestrowania.

Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 5 grudnia 2019 r.](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)

Dodatkowa dokumentacja jest śledzona przez [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)problemowe .

### <a name="simple-way-to-get-generated-sql"></a>Prosty sposób na wygenerowanie SQL

EF Core 5.0 `ToQueryString` wprowadza metodę rozszerzenia, która zwróci SQL, który EF Core wygeneruje podczas wykonywania kwerendy LINQ.

Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 9 stycznia 2020 r.](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)

Dodatkowa dokumentacja jest śledzona przez [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)problemowe .

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Użyj atrybutu C#, aby wskazać, że encja nie ma klucza

Typ jednostki można teraz skonfigurować jako bez `KeylessAttribute`klucza przy użyciu nowego .
Przykład:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Dokumentacja jest śledzona przez [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)problemowe .

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Połączenie lub parametry połączenia można zmienić przy zainicjowaniu dbContext

Teraz łatwiej jest utworzyć wystąpienie DbContext bez połączenia lub ciągu połączenia.
Ponadto parametry połączenia lub połączenia można teraz zmutować w wystąpieniu kontekstu.
Ta funkcja umożliwia tego samego wystąpienia kontekstu dynamicznie połączyć się z różnymi bazami danych.

Dokumentacja jest śledzona przez [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problemowe .

### <a name="change-tracking-proxies"></a>Serwery proxy śledzenia zmian

EF Core może teraz generować serwery proxy środowiska uruchomieniowego, które automatycznie implementują [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) i [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Następnie zgłaszają zmiany wartości właściwości jednostki bezpośrednio do EF Core, unikając konieczności skanowania w poszukiwaniu zmian.
Jednak serwery proxy mają własny zestaw ograniczeń, więc nie są dla wszystkich.

Dokumentacja jest śledzona przez [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problemowe .

### <a name="enhanced-debug-views"></a>Ulepszone widoki debugowania

Widoki debugowania są łatwym sposobem, aby spojrzeć na wewnętrzne EF Core podczas debugowania problemów.
Widok debugowania dla modelu został zaimplementowany jakiś czas temu.
Dla EF Core 5.0 ułatwiliśmy odczytanie widoku modelu i dodaliśmy nowy widok debugowania dla śledzonych jednostek w menedżerze stanu.

Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 12 grudnia 2019 r.](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)

Dodatkowa dokumentacja jest śledzona przez [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)problemowe .

### <a name="improved-handling-of-database-null-semantics"></a>Ulepszona obsługa semantyki zerowej bazy danych

Relacyjne bazy danych zazwyczaj traktują NULL jako nieznaną wartość i dlatego nie jest równa żadnej innej wartości NULL.
Podczas C# traktuje null jako zdefiniowaną wartość, która porównuje równe innym null.
EF Core domyślnie tłumaczy zapytania, tak aby używać semantyki null języka C#.
EF Core 5.0 znacznie poprawia wydajność tych tłumaczeń.

Dokumentacja jest śledzona przez [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problemowe .

### <a name="indexer-properties"></a>Właściwości indeksatora

EF Core 5.0 obsługuje mapowanie właściwości indeksatora Języka C#.
Właściwości te umożliwiają jednostkom działanie jako worki właściwości, w których kolumny są mapowane na nazwane właściwości w torbie.

Dokumentacja jest śledzona przez [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problemowe .

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generowanie ograniczeń kontrolnych dla mapowań wyliczenia

Ef Core 5.0 Migracje można teraz generować ograniczenia CHECK dla mapowań właściwości wyliczenia.
Przykład:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

Dokumentacja jest śledzona przez [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problemowe .

### <a name="isrelational"></a>IsRelational (IsRelational)

Oprócz `IsRelational` istniejącej `IsSqlServer`metody `IsSqlite`dodano nową metodę oraz `IsInMemory`.
Ta metoda może służyć do testowania, jeśli DbContext używa dowolnego dostawcy relacyjnej bazy danych.
Przykład:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Dokumentacja jest śledzona przez [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)problemowe .

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Optymistyczna współbieżność kosmosu z ETagami

Dostawca bazy danych usługi Azure Cosmos DB obsługuje teraz optymistyczną współbieżność przy użyciu tagów ETags.
Użyj konstruktora modeli w OnModelCreating, aby skonfigurować eTag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges następnie zrzuci `DbUpdateConcurrencyException` konflikt współbieżności, który może być [obsługiwany](https://docs.microsoft.com/ef/core/saving/concurrency) w celu zaimplementowania ponownych prób itp.

Dokumentacja jest śledzona przez [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)problemowe .

### <a name="query-translations-for-more-datetime-constructs"></a>Tłumaczenia zapytań, aby uzyskać więcej konstrukcji DateTime

Zapytania zawierające nową konstrukcję DateTime są teraz tłumaczone.

Ponadto są teraz mapowane następujące funkcje programu SQL Server:

* DateDiffWeek
* DateFromParts

Przykład:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .

### <a name="query-translations-for-more-byte-array-constructs"></a>Wykonywanie zapytań o więcej konstrukcji tablicy bajtów

Kwerendy używające zawiera, długość, sequenceEqual, itp.

Wstępna dokumentacja jest zawarta w [cotygodniowym statusie EF na dzień 5 grudnia 2019 r.](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)

Dodatkowa dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .

### <a name="query-translation-for-reverse"></a>Tłumaczenie kwerendy dla reverse

Kwerendy `Reverse` za pomocą są teraz tłumaczone.
Przykład:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .

### <a name="query-translation-for-bitwise-operators"></a>Translacja kwerend dla operatorów bitowych

Zapytania używające operatorów bitowych są teraz tłumaczone w większej liczbie przypadków Na przykład:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .

### <a name="query-translation-for-strings-on-cosmos"></a>Tłumaczenie kwerend dla ciągów w usłudze Cosmos

Kwerendy, które używają metod ciągu Zawiera, StartsWith i EndsWith są teraz tłumaczone podczas korzystania z dostawcy usługi Azure Cosmos DB.

Dokumentacja jest śledzona przez [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problemowe .

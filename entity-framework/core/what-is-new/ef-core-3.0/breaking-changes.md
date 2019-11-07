---
title: Istotne zmiany w EF Core 3,0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f02825f5303959997dca6e14e4efe64020b3cb22
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655875"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>Istotne zmiany zawarte w EF Core 3,0

Poniższe zmiany dotyczące interfejsu API i zachowania mogą powodować przerwanie istniejących aplikacji podczas uaktualniania ich do 3.0.0.
Zmiany, których oczekujemy tylko dostawcy bazy danych, są udokumentowane w obszarze [zmiany dostawcy](xref:core/providers/provider-log).

## <a name="summary"></a>Podsumowanie

| **Zmiana podziału**                                                                                               | **Notebook** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [Zapytania LINQ nie są już oceniane na kliencie](#linq-queries-are-no-longer-evaluated-on-the-client)         | Wysokowydajn       |
| [EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0](#netstandard21) | Wysokowydajn      |
| [Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK](#dotnet-ef) | Wysokowydajn      |
| [DetectChanges uznaje wartości klucza generowane przez magazyn](#dc) | Wysokowydajn      |
| [Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync](#fromsql) | Wysokowydajn      |
| [Typy zapytań są konsolidowane z typami jednostek](#qt) | Wysokowydajn      |
| [Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury](#no-longer) | Średniookresow      |
| [Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie](#cascade) | Średniookresow      |
| [Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu](#eager-loading-single-query) | Średniookresow      |
| [DeleteBehavior. ograniczanie ma semantykę oczyszczarki](#deletebehavior) | Średniookresow      |
| [Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony](#config) | Średniookresow      |
| [Każda właściwość używa niezależnej generacji klucza w pamięci](#each) | Średniookresow      |
| [Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości](#notrackingresolution) | Średniookresow      |
| [Zmiany interfejsu API metadanych](#metadata-api-changes) | Średniookresow      |
| [Zmiany w interfejsie API metadanych specyficzne dla dostawcy](#provider) | Średniookresow      |
| [UseRowNumberForPaging został usunięty](#urn) | Średniookresow      |
| [Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną](#fromsqlsproc) | Średniookresow      |
| [Metody Z tabel można określić tylko dla katalogów głównych zapytań](#fromsql) | Małą      |
| [~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono](#qe) | Małą      |
| [Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek](#tkv) | Małą      |
| [Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne](#de) | Małą      |
| [Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość](#aes) | Małą      |
| [Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych](#ip) | Małą      |
| [Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń](#fkp) | Małą      |
| [Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope](#dbc) | Małą      |
| [Pola zapasowe są używane domyślnie](#backing-fields-are-used-by-default) | Małą      |
| [Zgłoś, czy znaleziono wiele zgodnych pól zapasowych](#throw-if-multiple-compatible-backing-fields-are-found) | Małą      |
| [Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola](#field-only-property-names-should-match-the-field-name) | Małą      |
| [AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache](#adddbc) | Małą      |
| [DbContext. entry wykonuje teraz lokalną DetectChanges](#dbe) | Małą      |
| [Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta](#string-and-byte-array-keys-are-not-client-generated-by-default) | Małą      |
| [ILoggerFactory jest teraz usługą objętą zakresem](#ilf) | Małą      |
| [Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Małą      |
| [Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Małą      |
| [Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem](#nbh) | Małą      |
| [Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask](#rtnt) | Małą      |
| [Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping](#rtt) | Małą      |
| [ToTable dla typu pochodnego zgłasza wyjątek](#totable-on-a-derived-type-throws-an-exception) | Małą      |
| [Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK](#pragma) | Małą      |
| [Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3](#sqlite3) | Małą      |
| [Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite](#guid) | Małą      |
| [Wartości char są teraz przechowywane jako tekst na komputerze SQLite](#char) | Małą      |
| [Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury](#migid) | Małą      |
| [Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension](#xinfo) | Małą      |
| [Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator](#lqpe) | Małą      |
| [Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego](#clarify) | Małą      |
| [IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie](#irdc2) | Małą      |
| [Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency](#dip) | Małą      |
| [SQLitePCL. Raw Zaktualizowano do wersji 2.0.0](#SQLitePCL) | Małą      |
| [NetTopologySuite Zaktualizowano do wersji 2.0.0](#NetTopologySuite) | Małą      |
| [Firma Microsoft. Data. SqlClient jest używana zamiast elementu System. Data. SqlClient](#SqlClient) | Małą      |
| [Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie.](#mersa) | Małą      |
| [Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu](#udf-empty-string) | Małą      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Zapytania LINQ nie są już oceniane na kliencie

[Problemy ze śledzeniem #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[zobacz również problem #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Stare zachowanie**

Przed 3,0, gdy EF Core nie mógł skonwertować wyrażenia, które było częścią zapytania na SQL lub parametr, automatycznie ocenia wyrażenie na kliencie.
Domyślnie Ocena klienta potencjalnie kosztownych wyrażeń wyzwala jedynie ostrzeżenie.

**Nowe zachowanie**

Począwszy od 3,0, EF Core zezwala tylko na wyrażenia w projekcji najwyższego poziomu (ostatnie `Select()` wywołania w zapytaniu) do oceny na kliencie.
Jeśli wyrażenia w dowolnej innej części zapytania nie mogą być konwertowane na SQL lub parametr, zostanie zgłoszony wyjątek.

**Zalet**

Automatyczna Ocena klienta umożliwia wykonywanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych części tych zapytań.
Takie zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w środowisku produkcyjnym.
Na przykład warunek w wywołaniu `Where()`, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli mają być transferowane z serwera bazy danych oraz filtr, który ma zostać zastosowany na kliencie.
Ta sytuacja może być łatwo niewykrywalna, jeśli tabela zawiera tylko kilka wierszy do opracowania, ale gdy aplikacja zostanie przeniesiona do produkcji, a tabela może zawierać miliony wierszy.
Ostrzeżenia dotyczące oceny klienta okazały się także łatwe do ignorowania podczas opracowywania.

W związku z tym automatyczne szacowanie klienta może prowadzić do problemów, w których poprawianie tłumaczenia zapytań dla określonych wyrażeń powodowało niezamierzone zmiany między wersjami.

**Środki zaradcze**

Jeśli nie można w pełni przetłumaczyć zapytania, należy ponownie napisać zapytanie w formularzu, który można przetłumaczyć, lub użyć `AsEnumerable()`, `ToList()` lub podobnego do jawnego przywrócenia danych z powrotem do klienta, na którym będzie można go następnie przetworzyć przy użyciu LINQ-to-Objects.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>EF Core 3,0 cele .NET Standard 2,1, a nie .NET Standard 2,0

[Śledzenie problemu #15498](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

**Stare zachowanie**

Przed 3,0, EF Core kierowany .NET Standard 2,0 i będzie działać na wszystkich platformach obsługujących ten standard, w tym .NET Framework.

**Nowe zachowanie**

Począwszy od 3,0, EF Core cele .NET Standard 2,1 i zostaną uruchomione na wszystkich platformach obsługujących ten standard. Nie obejmuje to .NET Framework.

**Zalet**

Jest to część strategii strategicznej w technologiach .NET, która umożliwia skoncentrowanie się na oprogramowaniu .NET Core i innych nowoczesnych platformach .NET, takich jak Xamarin.

**Środki zaradcze**

Rozważ przeniesienie do nowoczesnej platformy .NET. Jeśli nie jest to możliwe, należy nadal używać EF Core 2,1 lub EF Core 2,2, w których są obsługiwane .NET Framework.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core nie jest już częścią ASP.NET Core współdzielonej struktury

[Śledzenie anonsów problemów # 325](https://github.com/aspnet/Announcements/issues/325)

**Stare zachowanie**

Przed ASP.NET Core 3,0, po dodaniu odwołania do pakietu do `Microsoft.AspNetCore.App` lub `Microsoft.AspNetCore.All` będzie obejmował EF Core i niektórych spośród EF Core dostawców danych, takich jak Dostawca SQL Server.

**Nowe zachowanie**

Począwszy od 3,0, ASP.NET Core współdzielona architektura nie obejmuje EF Core ani żadnych dostawców danych EF Core.

**Zalet**

Przed wprowadzeniem tej zmiany należy uzyskać EF Core wymagane różne kroki, w zależności od tego, czy aplikacja ma ASP.NET Core i SQL Server czy nie. Ponadto uaktualnienie ASP.NET Core wymuszone uaktualnienie EF Core i dostawcy SQL Server, które nie zawsze są pożądane.

W przypadku tej zmiany środowisko pobierania EF Core jest takie samo dla wszystkich dostawców, obsługiwane implementacje .NET i typy aplikacji.
Deweloperzy mogą również kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych.

**Środki zaradcze**

Aby użyć EF Core w aplikacji ASP.NET Core 3,0 lub innej obsługiwanej aplikacji, należy jawnie dodać odwołanie do pakietu do dostawcy bazy danych EF Core, który będzie używany przez aplikację.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>Narzędzie wiersza polecenia EF Core, dotnet EF, nie jest już częścią zestaw .NET Core SDK

[Śledzenie problemu #14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Stare zachowanie**

Przed 3,0 Narzędzie `dotnet ef` zostało dołączone do zestaw .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności wykonywania dodatkowych czynności. 

**Nowe zachowanie**

Począwszy od 3,0, zestaw SDK platformy .NET nie zawiera narzędzia `dotnet ef`, więc zanim będzie można go użyć, należy jawnie zainstalować go jako narzędzie lokalne lub globalne. 

**Zalet**

Ta zmiana umożliwia dystrybuowanie i aktualizowanie `dotnet ef` jako zwykłego narzędzia interfejsu wiersza polecenia platformy .NET na platformie NuGet, spójnie z faktem, że EF Core 3,0 jest również zawsze dystrybuowana jako pakiet NuGet.

**Środki zaradcze**

Aby móc zarządzać migracjami lub szkieletem `DbContext`, zainstaluj `dotnet-ef` jako narzędzie globalne:

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzia przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>Nazwy Z tabel, ExecuteSql by i ExecuteSqlAsync

[Śledzenie problemu #10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Stare zachowanie**

Przed EF Core 3,0 te nazwy metod były przeciążone w celu pracy z zwykłym ciągiem lub ciągiem, który powinien zostać interpolowany do SQL i parametrów.

**Nowe zachowanie**

Począwszy od EF Core 3,0, użyj `FromSqlRaw`, `ExecuteSqlRaw` i `ExecuteSqlRawAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane niezależnie od ciągu zapytania.
Na przykład:

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Użyj `FromSqlInterpolated`, `ExecuteSqlInterpolated` i `ExecuteSqlInterpolatedAsync`, aby utworzyć zapytanie parametryczne, gdzie parametry są przesyłane w ramach interpolowanego ciągu zapytania.
Na przykład:

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Należy zauważyć, że oba zapytania powyżej spowodują utworzenie tego samego sparametryzowanego kodu SQL z tymi samymi parametrami SQL.

**Zalet**

Przeciążenia metod, takie jak to, ułatwiają przypadkowe wywoływanie metody nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie.
Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.

**Środki zaradcze**

Przełącz, aby użyć nowych nazw metod.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>Nie można składować metody Z tabel, gdy jest używana z procedurą składowaną

[Śledzenie problemu #15392](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Stare zachowanie**

Przed EF Core 3,0, Metoda Z tabel podjęła próbę wykrycia, czy może być złożona pozostała wartość SQL. Klient przeprowadził ocenę, gdy nie udało się utworzyć kodu SQL, podobnie jak procedury składowanej. Następujące zapytanie działało przez uruchomienie procedury składowanej na serwerze i wykonanie FirstOrDefault po stronie klienta.

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Nowe zachowanie**

Począwszy od EF Core 3,0, EF Core nie będzie próbować przeanalizować bazy danych SQL. Dlatego w przypadku redagowania po FromSqlRaw/FromSqlInterpolated, EF Core nastąpi złożenie zapytania podrzędnego. Dlatego jeśli używasz procedury składowanej z kompozycją, zostanie pobrany wyjątek dla nieprawidłowej składni SQL.

**Zalet**

EF Core 3,0 nie obsługuje automatycznej oceny klienta, ponieważ została ona podatna na błędy, jak wyjaśniono [tutaj](#linq-queries-are-no-longer-evaluated-on-the-client).

**Środki zaradcze**

Jeśli używasz procedury składowanej w FromSqlRaw/FromSqlInterpolated, wiesz, że nie można jej składować, więc możesz dodać __AsEnumerable/AsAsyncEnumerable__ bezpośrednio po wywołaniu metody z tabel, aby uniknąć jakiegokolwiek złożenia po stronie serwera.

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>Metody Z tabel można określić tylko dla katalogów głównych zapytań

[Śledzenie problemu #15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**Stare zachowanie**

Przed EF Core 3,0 można określić metodę `FromSql` w dowolnym miejscu zapytania.

**Nowe zachowanie**

Począwszy od EF Core 3,0, nowe metody `FromSqlRaw` i `FromSqlInterpolated` (zastępujące `FromSql`) można określić tylko dla katalogów głównych zapytań, tj. bezpośrednio na `DbSet<>`. Próba określenia ich w innym miejscu spowoduje błąd kompilacji.

**Zalet**

Określenie wartości `FromSql` w innym miejscu niż na `DbSet` nie miało znaczenia lub dodano wartość i może spowodować niejednoznaczność w niektórych scenariuszach.

**Środki zaradcze**

wywołania `FromSql` powinny być przenoszone bezpośrednio do `DbSet`, do którego mają zastosowanie.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Zapytania nie śledzące już nie wykonują rozpoznawania tożsamości

[Śledzenie problemu #13518](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**Stare zachowanie**

Przed EF Core 3,0, to to samo wystąpienie jednostki będzie używane dla każdego wystąpienia jednostki o danym typie i IDENTYFIKATORze. Jest to zgodne z zachowaniem śledzenia zapytań. Na przykład to zapytanie:

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
zwróci to samo wystąpienie `Category` dla każdej `Product`, która jest skojarzona z daną kategorią.

**Nowe zachowanie**

Począwszy od EF Core 3,0, zostaną utworzone inne wystąpienia jednostek, gdy jednostka o danym typie i IDENTYFIKATORze zostanie napotkana w różnych miejscach na zwracanym wykresie. Na przykład zapytanie powyżej zwróci nowe wystąpienie `Category` dla każdej `Product`, nawet jeśli dwa produkty są skojarzone z tą samą kategorią.

**Zalet**

Rozpoznawanie tożsamości (oznacza to, że jednostka ma ten sam typ i identyfikator, jak poprzednio napotkana jednostka), dodaje dodatkowe obciążenie związane z wydajnością i pamięcią. Zwykle jest to przyczyną tego, że w pierwszym miejscu nie są używane zapytania śledzące. Ponadto, chociaż rozpoznawanie tożsamości może być przydatne, nie jest to konieczne, jeśli jednostki mają być serializowane i wysyłane do klienta, który jest typowy w przypadku zapytań bez śledzenia.

**Środki zaradcze**

Użyj zapytania śledzenia, jeśli jest wymagane rozpoznawanie tożsamości.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Wykonywanie zapytania jest rejestrowane na poziomie debugowania~~ Przywrócono

[Śledzenie problemu #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Ta zmiana została przywrócona, ponieważ nowa konfiguracja w EF Core 3,0 umożliwia określenie poziomu dziennika dla każdego zdarzenia, które ma zostać określone przez aplikację. Aby na przykład przełączyć rejestrowanie danych SQL do `Debug`, należy jawnie skonfigurować poziom w `OnConfiguring` lub `AddDbContext`:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Wartości klucza tymczasowego nie są już ustawione na wystąpienia jednostek

[Śledzenie problemu #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Stare zachowanie**

Przed EF Core 3,0 wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później będą miały wartość rzeczywistą wygenerowaną przez bazę danych.
Zwykle te wartości tymczasowe były dużymi liczbami ujemnymi.

**Nowe zachowanie**

Począwszy od 3,0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia samą właściwość klucza bez zmian.

**Zalet**

Ta zmiana została wprowadzona w celu zapobiegania błędnemu utracie wartości kluczy tymczasowych, gdy jednostka, która została wcześniej prześledzona przez niektóre wystąpienie `DbContext` jest przenoszona do innego wystąpienia `DbContext`. 

**Środki zaradcze**

Aplikacje, które przypisują wartości kluczy podstawowych do kluczy obcych, aby utworzyć skojarzenia między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i należą do jednostek w stanie `Added`.
Można to uniknąć przez:
* Nie używa kluczy generowanych przez magazyn.
* Ustawianie właściwości nawigacji na relacje formularzy zamiast ustawiania wartości kluczy obcych.
* Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.
Na przykład, `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartość tymczasową nawet wtedy, gdy nie ustawiono elementu `blog.Id`.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges uznaje wartości klucza generowane przez magazyn

[Śledzenie problemu #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Stare zachowanie**

Przed EF Core 3,0, Nieśledzona jednostka znaleziona przez `DetectChanges` będzie śledzona w stanie `Added` i wstawiona jako nowy wiersz w przypadku wywołania `SaveChanges`.

**Nowe zachowanie**

Począwszy od EF Core 3,0, jeśli jednostka używa wygenerowanych wartości klucza i jest ustawiona określona wartość klucza, jednostka będzie śledzona w stanie `Modified`.
Oznacza to, że wiersz dla jednostki jest założono i zostanie zaktualizowany, gdy zostanie wywołane `SaveChanges`.
Jeśli wartość klucza nie jest ustawiona lub jeśli typ jednostki nie korzysta z wygenerowanych kluczy, Nowa jednostka będzie nadal śledzona jako `Added` jak w poprzednich wersjach.

**Zalet**

Ta zmiana została wprowadzona w celu ułatwienia i spójności działania z odłączonymi wykresami jednostek przy użyciu kluczy generowanych przez magazyn.

**Środki zaradcze**

Ta zmiana może przerwać aplikację, jeśli typ jednostki jest skonfigurowany do korzystania z wygenerowanych kluczy, ale wartości kluczy są jawnie ustawiane dla nowych wystąpień.
Poprawka ma jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.
Na przykład za pomocą interfejsu API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Lub z adnotacjami danych:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Usuwanie kaskadowe jest teraz wykonywane natychmiast domyślnie

[Śledzenie problemu #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Stare zachowanie**

Przed 3,0 EF Core zastosowane akcje kaskadowe (usunięcie jednostek zależnych w przypadku usunięcia wymaganego podmiotu zabezpieczeń lub odłączenie relacji do wymaganego podmiotu zabezpieczeń) nie nastąpiło do momentu wywołania metody SaveChanges.

**Nowe zachowanie**

Począwszy od 3,0, EF Core stosuje akcje kaskadowe zaraz po wykryciu warunku wyzwalania.
Na przykład wywołanie `context.Remove()` w celu usunięcia podmiotu zabezpieczeń spowoduje, że wszystkie śledzone powiązane elementy zależne są również domyślnie ustawione na `Deleted`.

**Zalet**

Ta zmiana została wprowadzona w celu poprawy środowiska związanego z scenariuszami powiązań danych i inspekcji, gdzie ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ wywołaniem `SaveChanges`.

**Środki zaradcze**

Poprzednie zachowanie można przywrócić za pomocą ustawień na `context.ChangedTracker`.
Na przykład:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>Eager ładowanie pokrewnych jednostek odbywa się teraz w pojedynczym zapytaniu

[Śledzenie problemu #18022](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Stare zachowanie**

Przed 3,0 eagerly załadowanie nawigacji kolekcji za pośrednictwem operatorów `Include` spowodowało generowanie wielu zapytań w relacyjnej bazie danych, po jednej dla każdego powiązanego typu jednostki.

**Nowe zachowanie**

Począwszy od 3,0, EF Core generuje pojedyncze zapytanie z sprzężeniami w relacyjnych bazach danych.

**Zalet**

Wygenerowanie wielu zapytań w celu zaimplementowania pojedynczej kwerendy LINQ powodowało wiele problemów, w tym negatywną wydajność, ponieważ wiele danych jest niezbędnych, a błędy spójność danych, ponieważ każde zapytanie może obserwować inny stan bazy danych.

**Środki zaradcze**

Chociaż technicznie nie jest to zmiana, może ona mieć znaczny wpływ na wydajność aplikacji, gdy pojedyncze zapytanie zawiera dużą liczbę operatora `Include` na nawigowaniu po kolekcji. [Zobacz ten komentarz](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , aby uzyskać więcej informacji i przepisać zapytania w bardziej wydajny sposób.

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior. ograniczanie ma semantykę oczyszczarki

[Śledzenie problemu #12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Stare zachowanie**

Przed 3,0 `DeleteBehavior.Restrict` utworzono klucze obce w bazie danych z semantyką `Restrict`, ale również zmieniono wewnętrzną korektę w nieoczywisty sposób.

**Nowe zachowanie**

Począwszy od 3,0, `DeleteBehavior.Restrict` zapewnia, że klucze obce są tworzone z semantyką `Restrict` — to oznacza, że nie ma żadnych kaskadowych; Zgłoś naruszenie ograniczenia — bez również wpływu na wewnętrzną korektę EF.

**Zalet**

Ta zmiana została wprowadzona w celu poprawy środowiska korzystania z `DeleteBehavior` w intuicyjny sposób, bez nieoczekiwanych efektów ubocznych.

**Środki zaradcze**

Poprzednie zachowanie można przywrócić przy użyciu `DeleteBehavior.ClientNoAction`.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Typy zapytań są konsolidowane z typami jednostek

[Śledzenie problemu #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Stare zachowanie**

Przed EF Core 3,0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na wykonywanie zapytań dotyczących danych, które nie definiują klucza podstawowego w uporządkowany sposób.
Oznacza to, że typ zapytania został użyty do mapowania typów jednostek bez kluczy (prawdopodobnie z widoku, ale prawdopodobnie z tabeli), podczas gdy jest używany zwykły typ jednostki, gdy klucz był dostępny (prawdopodobnie z tabeli, ale prawdopodobnie z widoku).

**Nowe zachowanie**

Typ zapytania będzie teraz tylko typem jednostki bez klucza podstawowego.
Typy jednostek o niższym stopniu mają takie same funkcje jak typy zapytań w poprzednich wersjach.

**Zalet**

Ta zmiana została wprowadzona w celu zmniejszenia liczby pomyłek w odznaczeniu do typów zapytań.
W odróżnieniu od tego są one typami jednostek i są one tylko do odczytu z tego powodu, ale nie powinny być używane tylko wtedy, gdy typ jednostki musi być tylko do odczytu.
Podobnie często są one zamapowane na widoki, ale jest to tylko dlatego, że widoki często nie definiują kluczy.

**Środki zaradcze**

Następujące części interfejsu API są obecnie przestarzałe:
* **`ModelBuilder.Query<>()`** — zamiast tego należy wywołać `ModelBuilder.Entity<>().HasNoKey()`, aby oznaczyć typ jednostki jako bez kluczy.
Ta sytuacja nadal nie zostanie skonfigurowana zgodnie z Konwencją, aby uniknąć Niepoważnej konfiguracji, gdy jest oczekiwany klucz podstawowy, ale nie jest zgodna z Konwencją.
* należy używać **`DbQuery<>`** — zamiast tego należy użyć `DbSet<>`.
* należy używać **`DbContext.Query<>()`** — zamiast tego należy użyć `DbContext.Set<>()`.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Interfejs API konfiguracji dla relacji typu posiadanego został zmieniony

Problem ze śledzeniem [#12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
śledzenia [#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148) problem ze [śledzeniem
#14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Stare zachowanie**

Przed EF Core 3,0 konfiguracja powiązanej relacji została wykonana bezpośrednio po wywołaniu `OwnsOne` lub `OwnsMany`. 

**Nowe zachowanie**

Począwszy od EF Core 3,0, istnieje teraz interfejs API Fluent, aby skonfigurować właściwość nawigacji dla właściciela przy użyciu `WithOwner()`.
Na przykład:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Konfiguracja odnosząca się do relacji między właścicielem i właścicielem powinna teraz być łańcuchem po `WithOwner()` podobnie jak inne relacje są skonfigurowane.
Mimo że konfiguracja dla samego typu, nadal będzie łańcucha po `OwnsOne()/OwnsMany()`.
Na przykład:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Dodatkowo wywołanie `Entity()`, `HasOne()` lub `Set()` z elementem docelowym typu będącego właścicielem spowoduje teraz zgłoszenie wyjątku.

**Zalet**

Ta zmiana została wprowadzona w celu utworzenia oddzielenia oczyszczarki między konfiguracją samego typu i _relacją z_ typem będącym własnością.
To z kolei eliminuje niejednoznaczność i pomyłek wokół metod, takich jak `HasForeignKey`.

**Środki zaradcze**

Zmień konfigurację relacji typu będącego własnością, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Jednostki zależne współużytkujące tabelę z podmiotem zabezpieczeń są teraz opcjonalne

[Śledzenie problemu #9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**Stare zachowanie**

Rozważmy następujący model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
Przed EF Core 3,0, jeśli `OrderDetails` należy do `Order` lub jawnie zamapowane do tej samej tabeli, wystąpienie `OrderDetails` było zawsze wymagane podczas dodawania nowego `Order`.


**Nowe zachowanie**

Począwszy od 3,0, EF Core umożliwia dodanie `Order` bez `OrderDetails` i mapuje wszystkie właściwości `OrderDetails` z wyjątkiem klucza podstawowego na kolumny dopuszczające wartość null.
Podczas wykonywania zapytania o EF Core ustawia `OrderDetails`, aby `null`, Jeśli którakolwiek z wymaganych właściwości nie ma wartości lub jeśli nie ma żadnych wymaganych właściwości poza kluczem podstawowym, a wszystkie właściwości są `null`.

**Środki zaradcze**

Jeśli Twój model ma zależne od siebie wszystkie opcjonalne kolumny, ale nawigacja nie powinna być `null`, należy zmodyfikować aplikację tak, aby obsługiwała przypadki, gdy nawigacja jest `null`. Jeśli nie jest to możliwe, należy dodać wymaganą właściwość do typu jednostki lub co najmniej jedna właściwość powinna mieć przypisaną wartość inną niż `null`.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Wszystkie jednostki współużytkujące tabelę z kolumną Token współbieżności muszą mapować ją na Właściwość

[Śledzenie problemu #14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**Stare zachowanie**

Rozważmy następujący model:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
Przed EF Core 3,0, jeśli `OrderDetails` należy do `Order` lub jawnie zamapowanych do tej samej tabeli, aktualizacja tylko `OrderDetails` nie będzie aktualizować `Version` wartości na kliencie, a kolejna aktualizacja zakończy się niepowodzeniem.


**Nowe zachowanie**

Począwszy od 3,0, EF Core propaguje nową wartość `Version` do `Order`, jeśli jest ona własnością `OrderDetails`. W przeciwnym razie wyjątek jest zgłaszany podczas walidacji modelu.

**Zalet**

Ta zmiana została wprowadzona w celu uniknięcia nieodświeżonej wartości tokenu współbieżności, gdy jest aktualizowana tylko jedna z jednostek zamapowanych na tę samą tabelę.

**Środki zaradcze**

Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość, która jest mapowana na kolumnę Token współbieżności. Możliwe jest utworzenie jednego w tle:
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Dziedziczone właściwości z niemapowanych typów są teraz mapowane na pojedynczą kolumnę dla wszystkich typów pochodnych

[Śledzenie problemu #13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**Stare zachowanie**

Rozważmy następujący model:
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

Przed EF Core 3,0 Właściwość `ShippingAddress` będzie domyślnie mapowana na oddzielne kolumny dla `BulkOrder` i `Order`.

**Nowe zachowanie**

Począwszy od 3,0, EF Core tworzy tylko jedną kolumnę dla `ShippingAddress`.

**Zalet**

Stary behavoir był nieoczekiwany.

**Środki zaradcze**

Właściwość nadal może być jawnie zmapowana na oddzielną kolumnę w typach pochodnych:

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość podmiotu zabezpieczeń

[Śledzenie problemu #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**Stare zachowanie**

Rozważmy następujący model:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
Przed EF Core 3,0 Właściwość `CustomerId` byłaby użyta dla klucza obcego według Konwencji.
Jeśli jednak wartość `Order` jest typem będącym własnością, spowoduje to również, że `CustomerId` klucz podstawowy i zwykle nie jest to oczekiwane.

**Nowe zachowanie**

Począwszy od 3,0, EF Core nie próbuje użyć właściwości kluczy obcych według Konwencji, jeśli mają taką samą nazwę jak właściwość podmiotu zabezpieczeń.
Nazwa typu podmiotu zabezpieczeń połączona z nazwą właściwości głównej i nazwa nawigacji połączonej ze wzorcami nazw właściwości głównych są nadal dopasowane.
Na przykład:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Zalet**

Ta zmiana została wprowadzona w celu uniknięcia błędnego zdefiniowania właściwości klucza podstawowego w typie będącym własnością.

**Środki zaradcze**

Jeśli właściwość była zamierzona jako klucz obcy i w związku z tym jest częścią klucza podstawowego, należy ją jawnie skonfigurować.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest używane już przed ukończeniem elementu TransactionScope

[Śledzenie problemu #14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Stare zachowanie**

Przed EF Core 3,0, jeśli kontekst otwiera połączenie wewnątrz `TransactionScope`, połączenie pozostaje otwarte, podczas gdy bieżący `TransactionScope` jest aktywny.

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**Nowe zachowanie**

Począwszy od 3,0, EF Core zamyka połączenie zaraz po jego zakończeniu.

**Zalet**

Ta zmiana umożliwia użycie wielu kontekstów w tym samym `TransactionScope`. Nowe zachowanie jest również zgodne z EF6.

**Środki zaradcze**

Jeśli połączenie musi pozostać otwarte jawne wywołanie do `OpenConnection()`, zapewni, że EF Core nie zamyka go przedwcześnie:

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Każda właściwość używa niezależnej generacji klucza w pamięci

[Śledzenie problemu #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Stare zachowanie**

Przed EF Core 3,0, jeden współużytkowany Generator wartości został użyty dla wszystkich właściwości klucza liczby całkowitej w pamięci.

**Nowe zachowanie**

Począwszy od EF Core 3,0, Każda właściwość klucza Integer pobiera własny Generator wartości podczas korzystania z bazy danych w pamięci.
Ponadto, jeśli baza danych została usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.

**Zalet**

Ta zmiana została wprowadzona w celu dopasowania generowania kluczy w pamięci do generowania klucza rzeczywistej bazy danych i zwiększenia możliwości odizolowania testów od siebie podczas korzystania z bazy danych w pamięci.

**Środki zaradcze**

Może to spowodować przerwanie aplikacji, która polega na określeniu określonych wartości kluczy w pamięci.
Należy rozważyć zamiast nie polegać na określonych wartościach klucza lub zaktualizować w celu dopasowania go do nowego zachowania.

### <a name="backing-fields-are-used-by-default"></a>Pola zapasowe są używane domyślnie

[Śledzenie problemu #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Stare zachowanie**

Przed 3,0, nawet jeśli pole zapasowe właściwości było znane, EF Core nadal jest domyślnie odczytywane i zapisywane wartości właściwości przy użyciu metody pobierającej i ustawiającej właściwości.
Wyjątkiem jest wykonanie zapytania, gdzie pole zapasowe zostanie ustawione bezpośrednio, jeśli jest znane.

**Nowe zachowanie**

Począwszy od EF Core 3,0, jeśli jest znane pole zapasowe dla właściwości, wówczas EF Core będzie zawsze odczytywać i zapisywać tę właściwość przy użyciu pola zapasowego.
Może to spowodować przerwanie działania aplikacji, jeśli aplikacja jest zależna od dodatkowych zachowań zakodowanych w metodach pobierających lub setter.

**Zalet**

Ta zmiana została wprowadzona w celu uniemożliwienia EF Core błędnego wyzwalania logiki biznesowej podczas wykonywania operacji bazy danych obejmujących te jednostki.

**Środki zaradcze**

Zachowanie przed 3,0 może zostać przywrócone przez konfigurację trybu dostępu do właściwości w `ModelBuilder`.
Na przykład:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Zgłoś, czy znaleziono wiele zgodnych pól zapasowych

[Śledzenie problemu #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Stare zachowanie**

Przed EF Core 3,0, jeśli wiele pól jest zgodnych z regułami dotyczącymi znajdowania pola zapasowego właściwości, jedno pole zostanie wybrane na podstawie pewnej kolejności pierwszeństwa.
Może to spowodować, że błędne pole ma być używane w niejednoznacznych przypadkach.

**Nowe zachowanie**

Począwszy od EF Core 3,0, jeśli wiele pól jest dopasowanych do tej samej właściwości, zostanie zgłoszony wyjątek.

**Zalet**

Ta zmiana została wprowadzona w celu uniknięcia cichego używania jednego pola na inny, gdy tylko jeden z nich może być poprawny.

**Środki zaradcze**

Właściwości z niejednoznacznymi polami zapasowymi muszą mieć jawnie określone pole do użycia.
Na przykład za pomocą interfejsu API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Nazwy właściwości tylko dla pól powinny być zgodne z nazwą pola

**Stare zachowanie**

Przed EF Core 3,0, właściwość może być określona przez wartość ciągu i jeśli żadna właściwość o tej nazwie nie została znaleziona w typie .NET, EF Core spróbuje dopasować ją do pola przy użyciu reguł Konwencji.
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Nowe zachowanie**

Począwszy od EF Core 3,0, właściwość tylko do pola musi dokładnie odpowiadać nazwie pola.

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Zalet**

Ta zmiana została podjęta, aby uniknąć używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale również reguł dopasowania dla właściwości, które są takie same jak w przypadku właściwości zamapowanych na właściwości środowiska CLR.

**Środki zaradcze**

Właściwości "Only" muszą mieć taką samą nazwę jak pole, do którego są mapowane.
W przyszłej wersji EF Core po 3,0 planujemy ponowne włączenie jawnie skonfigurowanej nazwy pola, która jest inna niż nazwa właściwości (zobacz problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool nie wywołuje już metody addlogging i AddMemoryCache

[Śledzenie problemu #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Stare zachowanie**

Przed EF Core 3,0 wywoływanie `AddDbContext` lub `AddDbContextPool` spowoduje również zarejestrowanie usługi rejestrowania i buforowania pamięci z D. I przez wywołania funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nowe zachowanie**

Począwszy od EF Core 3,0, `AddDbContext` i `AddDbContextPool` nie będą już rejestrować tych usług przy użyciu iniekcji zależności (DI).

**Zalet**

EF Core 3,0 nie wymaga, aby te usługi były w zakresie DI kontenera aplikacji. Jeśli jednak `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, będzie nadal używany przez EF Core.

**Środki zaradcze**

Jeśli aplikacja wymaga tych usług, należy zarejestrować je jawnie przy użyciu funkcji [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext. entry wykonuje teraz lokalną DetectChanges

[Śledzenie problemu #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Stare zachowanie**

Przed EF Core 3,0 wywołania `DbContext.Entry` spowodują wykrycie zmian dla wszystkich śledzonych jednostek.
Zapewnia to aktualność stanu ujawnionego w `EntityEntry`.

**Nowe zachowanie**

Począwszy od EF Core 3,0, wywoływanie `DbContext.Entry` podejmie teraz próbę wykrycia zmian w danej jednostce i wszystkich śledzonych podmiotach głównych.
Oznacza to, że zmiany w innym miejscu mogą nie zostać wykryte przez wywołanie tej metody, która może mieć wpływ na stan aplikacji.

Należy pamiętać, że jeśli `ChangeTracker.AutoDetectChangesEnabled` jest ustawiona na `false`, nawet to lokalne wykrywanie zmian zostanie wyłączone.

Inne metody, które powodują wykrycie zmiany — na przykład `ChangeTracker.Entries` i `SaveChanges`--nadal powodują pełne `DetectChanges` wszystkich śledzonych jednostek.

**Zalet**

Ta zmiana została wprowadzona w celu poprawy domyślnej wydajności korzystania z `context.Entry`.

**Środki zaradcze**

Wywołaj `ChgangeTracker.DetectChanges()` jawnie przed wywołaniem `Entry`, aby upewnić się, że zachowanie poprzedzające 3,0.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Klucze tablic ciągów i bajtów nie są generowane domyślnie przez klienta

[Śledzenie problemu #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Stare zachowanie**

Przed EF Core 3,0 można użyć właściwości klucza `string` i `byte[]` bez jawnego ustawienia wartości innej niż null.
W takim przypadku wartość klucza jest generowana na kliencie jako identyfikator GUID, serializowany do bajtów dla `byte[]`.

**Nowe zachowanie**

Począwszy od EF Core 3,0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.

**Zalet**

Ta zmiana została wprowadzona, ponieważ wygenerowane przez klienta `string`/wartości `byte[]` zwykle nie są przydatne, a zachowanie domyślne spowodowało trudne przyczyny dotyczące wygenerowanych wartości kluczy w typowy sposób.

**Środki zaradcze**

Zachowanie przed 3,0 można uzyskać, jawnie określając, że właściwości klucza powinny używać wygenerowanych wartości, jeśli nie ustawiono żadnej innej wartości innej niż null.
Na przykład za pomocą interfejsu API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Lub z adnotacjami danych:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory jest teraz usługą objętą zakresem

[Śledzenie problemu #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Stare zachowanie**

Przed EF Core 3,0, `ILoggerFactory` został zarejestrowany jako usługa singleton.

**Nowe zachowanie**

Począwszy od EF Core 3,0, `ILoggerFactory` jest teraz zarejestrowane jako zakres.

**Zalet**

Ta zmiana została wprowadzona w celu zezwalania na skojarzenie rejestratora z wystąpieniem `DbContext`, które umożliwia inne funkcje i usuwa niektóre przypadki zachowań patologicznych, takich jak wybuch wewnętrznych dostawców usług.

**Środki zaradcze**

Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na EF Core wewnętrznym dostawcy usług.
To nie jest typowe.
W takich przypadkach większość elementów będzie nadal działać, ale każda usługa singleton, która była zależna od `ILoggerFactory`, będzie musiała zostać zmieniona w celu uzyskania `ILoggerFactory` w inny sposób.

Jeśli używasz takich sytuacji, Zapisz problem w [EF Core module śledzącym problemy](https://github.com/aspnet/EntityFrameworkCore/issues) w witrynie GitHub, aby poinformować nas, jak korzystasz z `ILoggerFactory`, aby lepiej zrozumieć, jak nie należy ponownie rozbić tego w przyszłości.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Pobieranie z opóźnieniem — nie zakłada się już, że właściwości nawigacji są w pełni załadowane

[Śledzenie problemu #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Stare zachowanie**

Przed EF Core 3,0, po usunięciu `DbContext` nie było możliwości znajomości, czy dana właściwość nawigacji w jednostce uzyskanej z tego kontekstu została w pełni załadowana.
Zamiast tego przyjmuje się, że jest załadowana Nawigacja referencyjna, jeśli ma ona wartość różną od null i że zostanie załadowana Nawigacja kolekcji, jeśli nie jest pusta.
W takich przypadkach próba załadowania z opóźnieniem będzie równa No-op.

**Nowe zachowanie**

Począwszy od EF Core 3,0, serwery proxy śledziją, czy właściwość nawigacji jest załadowana.
Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu, zawsze będzie równa No-op, nawet jeśli załadowana nawigacja jest pusta lub równa null.
Z drugiej strony, próba uzyskania dostępu do właściwości nawigacji, która nie została załadowana, spowoduje zgłoszenie wyjątku, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest kolekcją niepustą.
Jeśli taka sytuacja występuje, oznacza to, że kod aplikacji próbuje użyć ładowania opóźnionego w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona w taki sposób, aby nie było to robić.

**Zalet**

Ta zmiana została wprowadzona, aby zachować spójność i poprawić zachowanie podczas próby załadowania opóźnionego na usunięte wystąpienie `DbContext`.

**Środki zaradcze**

Zaktualizuj kod aplikacji, aby nie próbował próbować ładowania z opóźnieniem przy użyciu usuniętego kontekstu, lub skonfiguruj ten element jako No-op zgodnie z opisem w komunikacie o wyjątku.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Nadmierne Tworzenie wewnętrznych dostawców usług jest teraz domyślnie błędem

[Śledzenie problemu #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Stare zachowanie**

Przed EF Core 3,0 zostanie zarejestrowane Ostrzeżenie dla aplikacji tworzącej liczbę patologiczną wewnętrznych dostawców usług.

**Nowe zachowanie**

Począwszy od EF Core 3,0, to ostrzeżenie jest teraz uwzględniane i występuje błąd i zgłaszany jest wyjątek. 

**Zalet**

Ta zmiana została wprowadzona w celu zapewnienia lepszej jakości kodu aplikacji przez ujawnienie tego przypadku patologiczne bardziej jawnie.

**Środki zaradcze**

Najbardziej odpowiednią przyczyną działania w przypadku wystąpienia tego błędu jest zrozumienie głównej przyczyny problemu i zatrzymanie tworzenia tak wielu wewnętrznych dostawców usług.
Błąd można jednak przekonwertować z powrotem na ostrzeżenie (lub zignorować) przez konfigurację na `DbContextOptionsBuilder`.
Na przykład:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nowe zachowanie dla HasOne/HasMany wywoływane z pojedynczym ciągiem

[Śledzenie problemu #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Stare zachowanie**

Przed EF Core 3,0 kod wywołujący `HasOne` lub `HasMany` z pojedynczym ciągiem został zinterpretowany w sposób mylący.
Na przykład:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kod wygląda podobnie do `Samurai` do innego typu jednostki przy użyciu właściwości nawigacji `Entrance`, która może być prywatna.

W rzeczywistości ten kod próbuje utworzyć relację z typem jednostki o nazwie `Entrance` bez właściwości nawigacji.

**Nowe zachowanie**

Począwszy od EF Core 3,0, kod powyżej robi teraz, co wyglądało wcześniej.

**Zalet**

Stare zachowanie było bardzo mylące, szczególnie podczas odczytywania kodu konfiguracji i wyszukiwania błędów.

**Środki zaradcze**

Spowoduje to przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów nazw typów i bez określania jawnie właściwości nawigacji.
Nie jest to typowy sposób.
Poprzednie zachowanie można uzyskać poprzez jawne przekazanie `null` dla nazwy właściwości nawigacji.
Na przykład:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Typ zwracany dla kilku metod asynchronicznych został zmieniony z zadania na ValueTask

[Śledzenie problemu #15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Stare zachowanie**

Następujące metody asynchroniczne wcześniej zwracały `Task<T>`:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` (i pochodne klasy)

**Nowe zachowanie**

Powyższe metody zwracają teraz `ValueTask<T>` w tym samym `T` tak jak wcześniej.

**Zalet**

Ta zmiana zmniejsza liczbę przydziałów sterty wynikających z wywołania tych metod, co zwiększa ogólną wydajność.

**Środki zaradcze**

Aplikacje, które po prostu oczekują na powyższe interfejsy API, muszą zostać ponownie skompilowane — nie są wymagane żadne zmiany ze źródła.
Bardziej złożone użycie (np. przekazywanie zwróconych `Task` do `Task.WhenAny()`) zwykle wymaga, aby zwracane `ValueTask<T>` były konwertowane na `Task<T>` przez wywołanie `AsTask()` na nim.
Należy zauważyć, że ta zmiana powoduje spadek przydziału alokacji.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Adnotacja relacyjna: TypeMapping ma teraz tylko Właściwość TypeMapping

[Śledzenie problemu #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Stare zachowanie**

Nazwa adnotacji dla adnotacji mapowania typu to "relacyjne: TypeMapping".

**Nowe zachowanie**

Nazwa adnotacji dla adnotacji mapowania typu jest teraz "TypeMapping".

**Zalet**

Mapowania typów są teraz używane dla więcej niż tylko dostawców relacyjnych baz danych.

**Środki zaradcze**

Spowoduje to przerwanie aplikacji, które uzyskują dostęp do mapowania typu bezpośrednio jako adnotacji, która nie jest wspólna.
Najbardziej odpowiednią akcją do naprawienia jest użycie funkcji powierzchni interfejsu API w celu uzyskania dostępu do mapowań typów, zamiast bezpośredniego używania adnotacji.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable dla typu pochodnego zgłasza wyjątek 

[Śledzenie problemu #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Stare zachowanie**

Przed EF Core 3,0, `ToTable()` wywołane dla typu pochodnego zostałyby zignorowane, ponieważ strategia mapowania dziedziczenia była TPH, gdy jest to nieprawidłowe. 

**Nowe zachowanie**

Począwszy od EF Core 3,0 i przygotowania do dodawania obsługi TPT i TPC w późniejszej wersji, `ToTable()` wywołana dla typu pochodnego będzie teraz zgłaszać wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.

**Zalet**

Obecnie nie jest to prawidłowe mapowanie typu pochodnego na inną tabelę.
Ta zmiana pozwala uniknąć przerywania w przyszłości, gdy zostanie ona poprawna.

**Środki zaradcze**

Usuń wszystkie próby mapowania typów pochodnych do innych tabel.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex zastąpione HasIndex 

[Śledzenie problemu #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Stare zachowanie**

Przed EF Core 3,0, `ForSqlServerHasIndex().ForSqlServerInclude()` udostępnia sposób konfigurowania kolumn używanych z `INCLUDE`.

**Nowe zachowanie**

Począwszy od EF Core 3,0, użycie `Include` na indeksie jest teraz obsługiwane na poziomie relacyjnym.
Użyj `HasIndex().ForSqlServerInclude()`.

**Zalet**

Ta zmiana została wprowadzona w celu konsolidacji interfejsu API dla indeksów z `Include` w jednym miejscu dla wszystkich dostawców baz danych.

**Środki zaradcze**

Użyj nowego interfejsu API, jak pokazano powyżej.

### <a name="metadata-api-changes"></a>Zmiany interfejsu API metadanych

[Śledzenie problemu #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Nowe zachowanie**

Następujące właściwości zostały przekonwertowane na metody rozszerzające:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Zalet**

Ta zmiana upraszcza implementację powyższych interfejsów.

**Środki zaradcze**

Użyj nowych metod rozszerzenia.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Zmiany w interfejsie API metadanych specyficzne dla dostawcy

[Śledzenie problemu #214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Nowe zachowanie**

Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Zalet**

Ta zmiana upraszcza implementację powyższych metod rozszerzenia.

**Środki zaradcze**

Użyj nowych metod rozszerzenia.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>Nie EF Core już wysyłać dyrektywy pragma dla wymuszania programu SQLite FK

[Śledzenie problemu #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Stare zachowanie**

Przed EF Core 3,0, EF Core wyśle `PRAGMA foreign_keys = 1` w przypadku otwarcia połączenia z SQLite.

**Nowe zachowanie**

Począwszy od EF Core 3,0, EF Core przestać wysyłać `PRAGMA foreign_keys = 1` po otwarciu połączenia z programem SQLite.

**Zalet**

Ta zmiana została wprowadzona, ponieważ EF Core domyślnie używa `SQLitePCLRaw.bundle_e_sqlite3`, co z kolei oznacza, że wymuszanie klucza obcego jest domyślnie włączone i nie trzeba go jawnie włączać przy każdym otwarciu połączenia.

**Środki zaradcze**

Klucze obce są domyślnie włączone w SQLitePCLRaw. bundle_e_sqlite3, który jest używany domyślnie dla EF Core.
W innych przypadkach klucze obce można włączyć, określając `Foreign Keys=True` w parametrach połączenia.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft. EntityFrameworkCore. sqlite teraz zależy od SQLitePCLRaw. bundle_e_sqlite3

**Stare zachowanie**

Przed EF Core 3,0 EF Core używany `SQLitePCLRaw.bundle_green`.

**Nowe zachowanie**

Począwszy od EF Core 3,0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.

**Zalet**

Ta zmiana została wprowadzona w taki sposób, aby wersja oprogramowania SQLite była zgodna z innymi platformami w systemie iOS.

**Środki zaradcze**

Aby użyć natywnej wersji programu SQLite w systemie iOS, skonfiguruj `Microsoft.Data.Sqlite`, aby użyć innego pakietu `SQLitePCLRaw`.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Wartości identyfikatorów GUID są teraz przechowywane jako tekst na komputerze SQLite

[Śledzenie problemu #15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Stare zachowanie**

Wartości identyfikatorów GUID były wcześniej przechowywane jako wartości obiektów BLOB na komputerze SQLite.

**Nowe zachowanie**

Wartości identyfikatorów GUID są teraz przechowywane jako tekst.

**Zalet**

Format binarny identyfikatorów GUID nie jest znormalizowany. Przechowywanie wartości jako tekstu sprawia, że baza danych jest bardziej zgodna z innymi technologiami.

**Środki zaradcze**

Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Firma Microsoft. Data. sqlite nadal umożliwia odczytywanie wartości identyfikatora GUID zarówno z obiektów BLOB, jak i kolumn tekstowych. Jednak od momentu zmiany domyślnego formatu parametrów i stałych prawdopodobnie trzeba będzie wykonać akcję dla większości scenariuszy obejmujących identyfikatory GUID.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Wartości char są teraz przechowywane jako tekst na komputerze SQLite

[Śledzenie problemu #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Stare zachowanie**

Wartości char były wcześniej sored jako wartości całkowite na komputerze SQLite. Na przykład wartość typu char *jest* przechowywana jako wartość całkowita 65.

**Nowe zachowanie**

Wartości char są teraz przechowywane jako tekst.

**Zalet**

Przechowywanie wartości jako tekstu jest bardziej naturalne i sprawia, że baza danych jest bardziej zgodna z innymi technologiami.

**Środki zaradcze**

Istnieje możliwość migrowania istniejących baz danych do nowego formatu przez wykonanie kodu SQL, takiego jak poniższy.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

W EF Core można również nadal korzystać z poprzedniego zachowania przez skonfigurowanie konwertera wartości dla tych właściwości.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft. Data. sqlite również pozostaje w stanie odczytać wartości znaków z kolumn liczb CAŁKOWITYch i tekstowych, dzięki czemu niektóre scenariusze mogą nie wymagać żadnej akcji.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Identyfikatory migracji są teraz generowane przy użyciu kalendarza niezmiennej kultury

[Śledzenie problemu #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Stare zachowanie**

Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.

**Nowe zachowanie**

Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza niezmiennej kultury (gregoriański).

**Zalet**

Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania. Użycie kalendarza niezmiennej pozwala uniknąć problemów z porządkowaniem, które mogą wynikać z członków zespołu mających różne kalendarze systemu.

**Środki zaradcze**

Ta zmiana ma wpływ na wszystkich użytkowników w kalendarzu innym niż gregoriański, w których rok jest większy niż kalendarz gregoriański (np. tajski kalendarz Buddyjski). Istniejące identyfikatory migracji należy zaktualizować, aby nowe migracje były uporządkowane po zakończeniu migracji.

Identyfikator migracji można znaleźć w atrybucie migracji w plikach projektanta migracji.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Należy również zaktualizować tabelę historii migracji.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging został usunięty

[Śledzenie problemu #16400](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**Stare zachowanie**

Przed EF Core 3,0, `UseRowNumberForPaging` może zostać użyty do wygenerowania bazy danych SQL na potrzeby stronicowania zgodnego z SQL Server 2008.

**Nowe zachowanie**

Począwszy od EF Core 3,0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny z nowszymi wersjami SQL Server. 

**Zalet**

Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy z zmianami zapytań wprowadzonymi w EF Core 3,0 jest istotną pracą.

**Środki zaradcze**

Zalecamy zaktualizowanie do nowszej wersji SQL Server lub przy użyciu wyższego poziomu zgodności, aby obsługiwał wygenerowane SQL. Jeśli nie możesz tego zrobić, Dodaj komentarz dotyczący [problemu ze śledzeniem](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółowymi informacjami. Firma Microsoft może ponownie odwiedzać tę decyzję na podstawie opinii.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Informacje o rozszerzeniu/metadane zostały usunięte z IDbContextOptionsExtension

[Śledzenie problemu #16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**Stare zachowanie**

`IDbContextOptionsExtension` zawiera metody przesyłania metadanych o rozszerzeniu.

**Nowe zachowanie**

Te metody zostały przeniesione na nową abstrakcyjną klasę bazową `DbContextOptionsExtensionInfo`, która jest zwracana z nowej właściwości `IDbContextOptionsExtension.Info`.

**Zalet**

W wersjach od 2,0 do 3,0 firma Microsoft musi dodać lub zmienić te metody kilka razy.
Rozdzielenie ich na nową abstrakcyjną klasę bazową ułatwi wprowadzanie zmian tego rodzaju bez przerywania istniejących rozszerzeń.

**Środki zaradcze**

Zaktualizuj rozszerzenia, aby były zgodne z nowym wzorcem.
Przykłady można znaleźć w wielu implementacjach `IDbContextOptionsExtension` dla różnych rodzajów rozszerzeń w EF Core kodzie źródłowym.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>Zmieniono nazwę LogQueryPossibleExceptionWithAggregateOperator

[Śledzenie problemu #10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**Stąp**

Zmieniono nazwę `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.

**Zalet**

Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.

**Środki zaradcze**

Użyj nowej nazwy. (Należy zauważyć, że numer identyfikatora zdarzenia nie został zmieniony).

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Wyjaśnienie interfejsu API nazw ograniczeń klucza obcego

[Śledzenie problemu #10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Stare zachowanie**

Przed EF Core 3,0, nazwy ograniczeń klucza obcego były określane jako "nazwa". Na przykład:

```C#
var constraintName = myForeignKey.Name;
```

**Nowe zachowanie**

Począwszy od EF Core 3,0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia". Na przykład:

```C#
var constraintName = myForeignKey.ConstraintName;
```

**Zalet**

Ta zmiana powoduje spójność nazw w tym obszarze, a także wyjaśniono, że jest to nazwa ograniczenia klucza obcego, a nie nazwa kolumny lub właściwości, w której jest zdefiniowany klucz obcy.

**Środki zaradcze**

Użyj nowej nazwy.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator. HasTables/HasTablesAsync zostały udostępnione publicznie

[Śledzenie problemu #15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**Stare zachowanie**

Przed EF Core 3,0 te metody były chronione.

**Nowe zachowanie**

Począwszy od EF Core 3,0, te metody są publiczne.

**Zalet**

Te metody są używane przez EF do określenia, czy baza danych została utworzona, ale pusta. Może to być również przydatne z zewnątrz EF podczas określania, czy mają być stosowane migracje.

**Środki zaradcze**

Zmień dostępność wszelkich zastąpień.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft. EntityFrameworkCore. Design jest teraz pakietem DevelopmentDependency

[Śledzenie problemu #11506](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**Stare zachowanie**

Przed EF Core 3,0, Microsoft. EntityFrameworkCore. Design był zwykłym pakietem NuGet, którego zestaw może odwoływać się do projektów, które od niego zależą.

**Nowe zachowanie**

Począwszy od EF Core 3,0, jest to pakiet DevelopmentDependency. Oznacza to, że zależność nie będzie przepływać przechodnio do innych projektów i nie można już, domyślnie odwołuje się do zestawu.

**Zalet**

Ten pakiet jest przeznaczony wyłącznie do użytku w czasie projektowania. Wdrożone aplikacje nie powinny się do niej odwoływać. Pakiet a DevelopmentDependency wzmacnia to zalecenie.

**Środki zaradcze**

Jeśli musisz odwołać się do tego pakietu, aby przesłonić zachowanie EF Core w czasie projektowania, możesz zaktualizować metadane elementu PackageReference aktualizacji w projekcie. Jeśli pakiet jest przywoływany przechodniie za pośrednictwem Microsoft. EntityFrameworkCore. Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL. Raw Zaktualizowano do wersji 2.0.0

[Śledzenie problemu #14824](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**Stare zachowanie**

Microsoft. EntityFrameworkCore. sqlite poprzednio zależała od wersji 1.1.12 SQLitePCL. Raw.

**Nowe zachowanie**

Zaktualizowaliśmy pakiet, który jest zależny od wersji 2.0.0.

**Zalet**

Wersja 2.0.0 z SQLitePCL. Raw targets .NET Standard 2,0. Wcześniej była przeznaczona .NET Standard 1,1, co wymagało dużego zamknięcia pakietów przechodnich.

**Środki zaradcze**

SQLitePCL. Raw wersja 2.0.0 zawiera pewne istotne zmiany. Szczegółowe informacje można znaleźć w [informacjach o wersji](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite Zaktualizowano do wersji 2.0.0

[Śledzenie problemu #14825](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**Stare zachowanie**

Pakiety przestrzenne poprzednio zależały od wersji 1.15.1 NetTopologySuite.

**Nowe zachowanie**

Zaktualizowaliśmy pakiet, aby zależał od wersji 2.0.0.

**Zalet**

Wersja 2.0.0 programu NetTopologySuite ma na celu rozwiązanie kilku problemów użyteczności napotykanych przez EF Core użytkowników.

**Środki zaradcze**

NetTopologySuite wersja 2.0.0 zawiera pewne istotne zmiany. Szczegółowe informacje można znaleźć w [informacjach o wersji](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>Firma Microsoft. Data. SqlClient jest używana zamiast elementu System. Data. SqlClient

[Śledzenie problemu #15636](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Stare zachowanie**

Microsoft. EntityFrameworkCore. SqlServer poprzednio zależała od typu System. Data. SqlClient.

**Nowe zachowanie**

Zaktualizowaliśmy pakiet, który jest zależny od firmy Microsoft. Data. SqlClient.

**Zalet**

Microsoft. Data. SqlClient to sterownik dostępu do danych sztandarowe, który umożliwia SQL Server przechodzenie do przodu, a system. Data. SqlClient nie jest już fokusem rozwoju.
Niektóre ważne funkcje, takie jak Always Encrypted, są dostępne tylko w firmie Microsoft. Data. SqlClient.

**Środki zaradcze**

Jeśli kod przyjmuje bezpośrednią zależność od elementu System. Data. SqlClient, należy zmienić go na odwołanie Microsoft. Data. SqlClient zamiast tego. ponieważ dwa pakiety obsługują bardzo wysoki poziom zgodności interfejsów API, powinno to być tylko proste zmiany pakietów i przestrzeni nazw.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Należy skonfigurować wiele niejednoznacznych relacji odwołujących się do siebie. 

[Śledzenie problemu #13573](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**Stare zachowanie**

Typ jednostki z wieloma odwołującymi się do siebie właściwości nawigacji jednokierunkowej i pasujących FKs został niepoprawnie skonfigurowany jako pojedynczej relacji. Na przykład:

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

**Nowe zachowanie**

Ten scenariusz jest teraz wykrywany podczas budowania modelu i zgłaszany jest wyjątek wskazujący, że model jest niejednoznaczny.

**Zalet**

Model wynikowy jest niejednoznaczny i prawdopodobnie jest zwykle niewłaściwy w tym przypadku.

**Środki zaradcze**

Użyj pełnej konfiguracji relacji. Na przykład:

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>Dbfunction. schemat mający wartość null lub pusty ciąg konfiguruje go jako domyślny schemat modelu

[Śledzenie problemu #12757](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Stare zachowanie**

Funkcja dbzostała skonfigurowana ze schematem jako pusty ciąg był traktowany jak wbudowana funkcja bez schematu. Na przykład poniższy kod mapuje funkcję CLR `DatePart` na funkcję wbudowaną `DATEPART` na serwerze SqlServer.

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Nowe zachowanie**

Wszystkie mapowania funkcji dbfunction są uznawane za mapowane do funkcji zdefiniowanych przez użytkownika. W związku z tym wartość pustego ciągu spowodowałaby umieszczenie funkcji wewnątrz domyślnego schematu modelu. Może to być schemat skonfigurowany jawnie za pośrednictwem interfejsu API Fluent `modelBuilder.HasDefaultSchema()` lub `dbo` w przeciwnym razie.

**Zalet**

Poprzednio pusty schemat był sposobem traktowania tej funkcji, ale ta logika jest stosowana tylko dla SqlServer, w której wbudowane funkcje nie należą do żadnego schematu.

**Środki zaradcze**

Skonfiguruj ręcznie tłumaczenie funkcji dbfunction, aby zamapować ją na wbudowaną funkcję.

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```

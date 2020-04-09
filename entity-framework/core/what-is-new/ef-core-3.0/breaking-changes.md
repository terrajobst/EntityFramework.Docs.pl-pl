---
title: Przełomowe zmiany w EF Core 3.0 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417462"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>Przełomowe zmiany zawarte w EF Core 3.0

Następujące zmiany interfejsu API i zachowania mają potencjał, aby przerwać istniejące aplikacje podczas uaktualniania ich do 3.0.0.
Zmiany, które mają mieć wpływ tylko na dostawców baz danych, są dokumentowane w obszarze [zmiany dostawcy.](xref:core/providers/provider-log)

## <a name="summary"></a>Podsumowanie

| **Przełomowa zmiana**                                                                                               | **Wpływ** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [Zapytania LINQ nie są już oceniane na kliencie](#linq-queries-are-no-longer-evaluated-on-the-client)         | Wysoka       |
| [Cele EF Core 3.0 .NET Standard 2.1 zamiast .NET Standard 2.0](#netstandard21) | Wysoka      |
| [Narzędzie wiersza polecenia EF Core, dotnet ef, nie jest już częścią zestawu .NET Core SDK](#dotnet-ef) | Wysoka      |
| [DetectChanges honoruje wartości kluczy wygenerowane przez magazyn](#dc) | Wysoka      |
| [Zmieniono nazwę fromSql, ExecuteSql i ExecuteSqlAsync](#fromsql) | Wysoka      |
| [Typy zapytań są konsolidowane za pomocą typów encji](#qt) | Wysoka      |
| [Entity Framework Core nie jest już częścią ASP.NET core współdzielonych](#no-longer) | Medium      |
| [Kaskadowe usunięcia są teraz usuwane natychmiast domyślnie](#cascade) | Medium      |
| [Wczesne ładowanie powiązanych jednostek odbywa się teraz w jednej kwerendzie](#eager-loading-single-query) | Medium      |
| [DeleteBehavior.Restrict ma czystsze semantyki](#deletebehavior) | Medium      |
| [Interfejs API konfiguracji dla relacji typu należącego uległ zmianie](#config) | Medium      |
| [Każda właściwość używa niezależnego generowania klucza całkowitej w pamięci](#each) | Medium      |
| [Kwerendy bez śledzenia nie wykonują już rozpoznawania tożsamości](#notrackingresolution) | Medium      |
| [Zmiany interfejsu API metadanych](#metadata-api-changes) | Medium      |
| [Zmiany interfejsu API metadanych specyficznych dla dostawcy](#provider) | Medium      |
| [UseRowNumberForPaging został usunięty](#urn) | Medium      |
| [Metoda FromSql używana z procedurą składowaną nie może być](#fromsqlsproc) | Medium      |
| [Metody FromSql można określić tylko w katalogach głównych kwerend](#fromsql) | Małe      |
| [~~Wykonywanie kwerendy jest rejestrowane na poziomie debugowania~~ Przywrócone](#qe) | Małe      |
| [Tymczasowe wartości klucza nie są już ustawiane w wystąpieniach encji](#tkv) | Małe      |
| [Jednostki zależne dzielące tabelę z podmiotem głównym są teraz opcjonalne](#de) | Małe      |
| [Wszystkie jednostki współużytkujące tabelę z kolumną tokenu współbieżności muszą mapować ją na właściwość](#aes) | Małe      |
| [Nie można wyszukiwać jednostek będących własnością bez użycia zapytania śledzenia przez właściciela](#owned-query) | Małe      |
| [Właściwości dziedziczone z typów niezamapowanych są teraz mapowane na jedną kolumnę dla wszystkich typów pochodnych](#ip) | Małe      |
| [Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość główna](#fkp) | Małe      |
| [Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest już używane przed zakończeniem transakcji TransactionScope](#dbc) | Małe      |
| [Pola zapasowe są używane domyślnie](#backing-fields-are-used-by-default) | Małe      |
| [Throw, jeśli znaleziono wiele zgodnych pól podkładowych](#throw-if-multiple-compatible-backing-fields-are-found) | Małe      |
| [Nazwy właściwości tylko do pól powinny być zgodne z nazwą pola](#field-only-property-names-should-match-the-field-name) | Małe      |
| [AddDbContext/AddDbContextPool nie będzie już wywoływać addlogging i AddMemoryCache](#adddbc) | Małe      |
| [AddEntityFramework* dodaje IMemoryCache z limitem rozmiaru](#addentityframework-adds-imemorycache-with-a-size-limit) | Małe      |
| [DbContext.Entry wykonuje teraz lokalne zmiany wykrywania](#dbe) | Małe      |
| [Klucze tablicy ciągów i bajtów nie są domyślnie generowane przez klienta](#string-and-byte-array-keys-are-not-client-generated-by-default) | Małe      |
| [ILoggerFactory jest teraz usługą o określonym zakresie](#ilf) | Małe      |
| [Serwery proxy z opóźnieniem nie zakładają już, że właściwości nawigacji są w pełni załadowane](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Małe      |
| [Nadmierne tworzenie wewnętrznych usługodawców jest teraz domyślnie błędem](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Małe      |
| [Nowe zachowanie hasone/hasmany wywoływane z jednym ciągiem](#nbh) | Małe      |
| [Typ zwracany dla kilku metod asynchronicznego został zmieniony z Task na ValueTask](#rtnt) | Małe      |
| [Adnotacja relacyjna:TypeMapping jest teraz tylko TypeMapping](#rtt) | Małe      |
| [ToTable na typ pochodny zgłasza wyjątek](#totable-on-a-derived-type-throws-an-exception) | Małe      |
| [EF Core nie wysyła już pragmy do egzekwowania SQLite FK](#pragma) | Małe      |
| [Microsoft.EntityFrameworkCore.Sqlite teraz zależy od SQLitePCLRaw.bundle_e_sqlite3](#sqlite3) | Małe      |
| [Wartości Guid są teraz przechowywane jako tekst na SQLite](#guid) | Małe      |
| [Wartości char są teraz przechowywane jako tekst na SQLite](#char) | Małe      |
| [Identyfikatory migracji są teraz generowane przy użyciu kalendarza kultury niezmiennej](#migid) | Małe      |
| [Informacje/metadane rozszerzenia zostały usunięte z IDbContextOptionsExtension](#xinfo) | Małe      |
| [Nazwa logqueryPossibleExceptionWithAggregateOperator została zmieniona](#lqpe) | Małe      |
| [Wyjaśnienie interfejsu API dla nazw ograniczeń klucza obcego](#clarify) | Małe      |
| [IRelationalDatabaseCreator.HasTables/HasTablesAsync zostały upublicznione](#irdc2) | Małe      |
| [Microsoft.EntityFrameworkCore.Design jest teraz pakietem rozwojuzależności](#dip) | Małe      |
| [SQLitePCL.raw zaktualizowany do wersji 2.0.0](#SQLitePCL) | Małe      |
| [NetTopologySuite zaktualizowano do wersji 2.0.0](#NetTopologySuite) | Małe      |
| [Microsoft.Data.SqlClient jest używany zamiast System.Data.SqlClient](#SqlClient) | Małe      |
| [Należy skonfigurować wiele niejednoznacznych relacji samonawodujących się](#mersa) | Małe      |
| [DbFunction.Schema jest null lub pusty ciąg konfiguruje go do domyślnego schematu modelu](#udf-empty-string) | Małe      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Zapytania LINQ nie są już oceniane na kliencie

[#14935 problemu](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
śledzenia[zobacz również #12795 problemu](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Stare zachowanie**

Przed 3.0, gdy EF Core nie można przekonwertować wyrażenie, które było częścią kwerendy do SQL lub parametru, automatycznie oceniane wyrażenie na kliencie.
Domyślnie ocena klienta potencjalnie kosztownych wyrażeń wyzwoliła tylko ostrzeżenie.

**Nowe zachowanie**

Począwszy od 3.0, EF Core umożliwia tylko wyrażenia w `Select()` projekcji najwyższego poziomu (ostatnie wywołanie w kwerendzie) do oceny na kliencie.
Gdy wyrażenia w innej części kwerendy nie można przekonwertować na SQL lub parametr, wyjątek.

**Dlaczego**

Automatyczna ocena zapytań przez klienta umożliwia wykonanie wielu zapytań, nawet jeśli nie można przetłumaczyć ważnych ich części.
To zachowanie może spowodować nieoczekiwane i potencjalnie szkodliwe zachowanie, które może stać się widoczne tylko w produkcji.
Na przykład warunek w `Where()` wywołaniu, którego nie można przetłumaczyć, może spowodować, że wszystkie wiersze z tabeli zostaną przeniesione z serwera bazy danych, a filtr ma zostać zastosowany na kliencie.
Ta sytuacja może łatwo przejść niezauważone, jeśli tabela zawiera tylko kilka wierszy w rozwoju, ale mocno uderzyć, gdy aplikacja przenosi się do produkcji, gdzie tabela może zawierać miliony wierszy.
Ostrzeżenia dotyczące oceny klienta również okazały się zbyt łatwe do zignorowania podczas opracowywania.

Poza tym automatyczna ocena klienta może prowadzić do problemów, w których poprawa tłumaczenia zapytań dla określonych wyrażeń spowodował niezamierzone zmiany podziału między wersjami.

**Środki zaradcze**

Jeśli kwerendy nie można w pełni przetłumaczyć, a następnie przepisać kwerendę `AsEnumerable()`w `ToList()`formularzu, który może być przetłumaczony lub użyć , lub podobne do jawnie przenieść dane z powrotem do klienta, gdzie następnie mogą być dalej przetwarzane przy użyciu LINQ-to-Objects.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>Cele EF Core 3.0 .NET Standard 2.1 zamiast .NET Standard 2.0

[#15498 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> EF Core 3.1 ponownie celuje w .NET Standard 2.0. Spowoduje to przywrócenie obsługi programu .NET Framework.

**Stare zachowanie**

Przed 3.0 EF Core ukierunkowane .NET Standard 2.0 i będzie działać na wszystkich platformach, które obsługują tego standardu, w tym .NET Framework.

**Nowe zachowanie**

Począwszy od 3.0, EF Core cele .NET Standard 2.1 i będzie działać na wszystkich platformach, które obsługują ten standard. Nie obejmuje to programu .NET Framework.

**Dlaczego**

Jest to część strategicznej decyzji w ramach technologii platformy .NET, aby skupić energię na programie .NET Core i innych nowoczesnych platformach platformy .NET, takich jak Xamarin.

**Środki zaradcze**

Użyj EF Core 3.1.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core nie jest już częścią ASP.NET core współdzielonych

[Śledzenie ogłoszeń o problemach#325](https://github.com/aspnet/Announcements/issues/325)

**Stare zachowanie**

Przed ASP.NET Core 3.0, po dodaniu `Microsoft.AspNetCore.App` odwołania `Microsoft.AspNetCore.All`do pakietu lub , to będzie zawierać EF Core i niektórych dostawców danych EF Core, takich jak dostawca programu SQL Server.

**Nowe zachowanie**

Począwszy od 3.0, ASP.NET Core udostępnionej struktury nie obejmuje EF Core lub żadnych dostawców danych EF Core.

**Dlaczego**

Przed tą zmianą uzyskanie EF Core wymagało różnych kroków w zależności od tego, czy aplikacja jest ukierunkowana na ASP.NET Core i SQL Server, czy nie. Ponadto uaktualnienie ASP.NET Core wymusił uaktualnienie EF Core i dostawcy programu SQL Server, co nie zawsze jest pożądane.

Dzięki tej zmianie środowisko uzyskiwania EF Core jest taka sama we wszystkich dostawcach, obsługiwanych implementacjach platformy .NET i typach aplikacji.
Deweloperzy mogą również teraz kontrolować dokładnie, kiedy EF Core i EF Core dostawców danych są uaktualniane.

**Środki zaradcze**

Aby użyć EF Core w aplikacji ASP.NET Core 3.0 lub innej obsługiwanej aplikacji, jawnie dodaj odwołanie do pakietu ef core dostawcy bazy danych, który będzie używany przez aplikację.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>Narzędzie wiersza polecenia EF Core, dotnet ef, nie jest już częścią zestawu .NET Core SDK

[#14016 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Stare zachowanie**

Przed 3.0 `dotnet ef` narzędzie zostało uwzględnione w zestawie .NET Core SDK i było łatwo dostępne do użycia z wiersza polecenia z dowolnego projektu bez konieczności dodatkowych kroków. 

**Nowe zachowanie**

Począwszy od 3.0, .NET SDK `dotnet ef` nie zawiera narzędzia, więc przed jego użyciem należy jawnie zainstalować go jako narzędzie lokalne lub globalne. 

**Dlaczego**

Ta zmiana pozwala nam rozpowszechniać i aktualizować `dotnet ef` jako zwykłe narzędzie interfejsu wiersza polecenia .NET na NuGet, zgodnie z faktem, że EF Core 3.0 jest również zawsze dystrybuowane jako pakiet NuGet.

**Środki zaradcze**

Aby móc zarządzać migracjami lub szkieletem `DbContext` `dotnet-ef` , zainstalować jako narzędzie globalne:

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Można również uzyskać narzędzie lokalne podczas przywracania zależności projektu, który deklaruje go jako zależność narzędzi przy użyciu [pliku manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>Zmieniono nazwę fromSql, ExecuteSql i ExecuteSqlAsync

[#10996 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Stare zachowanie**

Przed EF Core 3.0 te nazwy metod były przeciążone do pracy z normalnym ciągiem lub ciągiem, który powinien być interpolowany do sql i parametrów.

**Nowe zachowanie**

Począwszy od EF Core 3.0, użyj `FromSqlRaw`, `ExecuteSqlRaw`i `ExecuteSqlRawAsync` utworzyć sparametryzowane zapytanie, gdzie parametry są przekazywane oddzielnie od ciągu kwerendy.
Przykład:

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Użyj `FromSqlInterpolated` `ExecuteSqlInterpolated`, `ExecuteSqlInterpolatedAsync` i utworzyć sparametryzowaną kwerendę, w której parametry są przekazywane jako część interpolowanego ciągu zapytania.
Przykład:

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Należy zauważyć, że oba powyższe zapytania będą tworzyć ten sam sparametryzowany SQL z tymi samymi parametrami SQL.

**Dlaczego**

Przeciążenia metody, takie jak ten sprawiają, że bardzo łatwo przypadkowo wywołać metodę ciągu nieprzetworzonego, gdy intencją było wywołanie interpolowanej metody ciągu i na odwrót.
Może to spowodować kwerendy nie są parametryzowane, kiedy powinny być.

**Środki zaradcze**

Przełącz się, aby użyć nowych nazw metod.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>Metoda FromSql używana z procedurą składowaną nie może być

[#15392 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Stare zachowanie**

Przed EF Core 3.0, FromSql metoda próbowała wykryć, czy przekazany SQL może być skomponowany na. To nie oceny klienta, gdy SQL był niekomponowany jak procedura składowana. Poniższa kwerenda działała przez uruchomienie procedury składowanej na serwerze i wykonanie firstordefault po stronie klienta.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Nowe zachowanie**

Począwszy od EF Core 3.0, EF Core nie spróbuje przeanalizować SQL. Więc jeśli są komponowania po FromSqlRaw/FromSqlInterpolated, a następnie EF Core będzie komponować SQL, powodując sub kwerendy. Więc jeśli używasz procedury składowanej ze składem, otrzymasz wyjątek dla nieprawidłowej składni SQL.

**Dlaczego**

EF Core 3.0 nie obsługuje automatycznej oceny klienta, ponieważ był podatny na błędy, jak wyjaśniono [tutaj.](#linq-queries-are-no-longer-evaluated-on-the-client)

**Środki zaradcze**

Jeśli używasz procedury składowanej w FromSqlRaw/FromSqlInterpolated, wiesz, że nie można składać na, więc można dodać __AsEnumerable/AsAsyncEnumerable__ zaraz po FromSql wywołanie metody, aby uniknąć jakiejkolwiek kompozycji po stronie serwera.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>Metody FromSql można określić tylko w katalogach głównych kwerend

[#15704 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**Stare zachowanie**

Przed EF Core 3.0, `FromSql` metoda może być określona w dowolnym miejscu w kwerendzie.

**Nowe zachowanie**

Począwszy `FromSqlRaw` od EF Core 3.0, nowe i `FromSqlInterpolated` metody (które zastępują) `FromSql`mogą być określone `DbSet<>`tylko na korzeniach zapytań, czyli bezpośrednio na . Próba określenia ich w dowolnym miejscu innego spowoduje błąd kompilacji.

**Dlaczego**

Określanie `FromSql` w dowolnym `DbSet` miejscu innym niż na nie ma żadnego znaczenia dodanego lub wartości dodanej i może spowodować niejednoznaczność w niektórych scenariuszach.

**Środki zaradcze**

`FromSql`wywołania powinny być przenoszone bezpośrednio `DbSet` na to, do czego się odnoszą.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>Kwerendy bez śledzenia nie wykonują już rozpoznawania tożsamości

[#13518 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**Stare zachowanie**

Przed EF Core 3.0 tego samego wystąpienia jednostki będzie używany dla każdego wystąpienia jednostki z danego typu i identyfikatora. Odpowiada to zachowaniu zapytań śledzenia. Na przykład ta kwerenda:

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
zwraca to `Category` samo wystąpienie dla każdego, `Product` który jest skojarzony z daną kategorią.

**Nowe zachowanie**

Począwszy od EF Core 3.0, różne wystąpienia jednostek zostaną utworzone, gdy jednostka o danym typie i identyfikatorze zostanie napotkana w różnych miejscach na zwróconym wykresie. Na przykład powyższa kwerenda zwróci `Category` teraz `Product` nowe wystąpienie dla każdego, nawet jeśli dwa produkty są skojarzone z tą samą kategorią.

**Dlaczego**

Rozpoznawanie tożsamości (czyli określanie, że jednostka ma ten sam typ i identyfikator co wcześniej napotkana jednostka) dodaje dodatkową wydajność i obciążenie pamięci. To zwykle jest sprzeczne z dlaczego nie śledzenia kwerendy są używane w pierwszej kolejności. Ponadto podczas rozpoznawania tożsamości czasami może być przydatne, nie jest potrzebne, jeśli jednostki mają być serializowane i wysyłane do klienta, co jest wspólne dla kwerend bez śledzenia.

**Środki zaradcze**

Użyj kwerendy śledzącej, jeśli wymagane jest rozpoznawanie tożsamości.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~Wykonywanie kwerendy jest rejestrowane na poziomie debugowania~~ Przywrócone

[#14523 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Firma We cofnięte tej zmiany, ponieważ nowa konfiguracja w EF Core 3.0 umożliwia poziom dziennika dla dowolnego zdarzenia, które mają być określone przez aplikację. Na przykład, aby przełączyć `Debug`rejestrowanie SQL do , `OnConfiguring` `AddDbContext`jawnie skonfigurować poziom w lub:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Tymczasowe wartości klucza nie są już ustawiane w wystąpieniach encji

[#12378 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Stare zachowanie**

Przed EF Core 3.0, wartości tymczasowe zostały przypisane do wszystkich właściwości klucza, które później mają rzeczywistą wartość generowaną przez bazę danych.
Zazwyczaj te wartości tymczasowe były duże liczby ujemne.

**Nowe zachowanie**

Począwszy od 3.0, EF Core przechowuje wartość klucza tymczasowego jako część informacji śledzenia jednostki i pozostawia właściwość klucza się bez zmian.

**Dlaczego**

Ta zmiana została wnieszona, aby zapobiec tymczasowe wartości klucza błędnie staje `DbContext` się trwałe, gdy `DbContext` jednostka, która została wcześniej śledzona przez niektóre wystąpienie jest przenoszony do innego wystąpienia. 

**Środki zaradcze**

Aplikacje, które przypisują wartości klucza podstawowego do kluczy obcych w celu utworzenia skojarzeń między jednostkami, mogą zależeć od starego zachowania, jeśli klucze podstawowe są generowane przez magazyn i należą do jednostek w `Added` stanie.
Można tego uniknąć poprzez:
* Nie używanie kluczy generowanych przez magazyn.
* Ustawianie właściwości nawigacji w celu utworzenia relacji zamiast ustawiania wartości klucza obcego.
* Uzyskaj rzeczywiste wartości klucza tymczasowego z informacji śledzenia jednostki.
Na przykład `context.Entry(blog).Property(e => e.Id).CurrentValue` zwróci wartość tymczasową, nawet jeśli `blog.Id` sama nie została ustawiona.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges honoruje wartości kluczy wygenerowane przez magazyn

[#14616 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Stare zachowanie**

Przed EF Core 3.0, nieśledzona jednostka znaleziona przez `DetectChanges` będzie śledzona w `Added` stanie i wstawiana jako nowy wiersz, gdy `SaveChanges` jest wywoływana.

**Nowe zachowanie**

Począwszy od EF Core 3.0, jeśli jednostka używa wygenerowanych wartości klucza i ustawiono `Modified` pewną wartość klucza, jednostka będzie śledzona w stanie.
Oznacza to, że zakłada się, że wiersz dla `SaveChanges` jednostki istnieje i zostanie zaktualizowany, gdy zostanie wywołany.
Jeśli wartość klucza nie jest ustawiona lub typ jednostki nie używa wygenerowanych kluczy, `Added` nowa encja będzie nadal śledzona tak, jak w poprzednich wersjach.

**Dlaczego**

Ta zmiana została włączona, aby ułatwić i bardziej spójne do pracy z wykresów jednostki rozłączone podczas korzystania z kluczy generowanych przez magazyn.

**Środki zaradcze**

Ta zmiana może podzielić aplikację, jeśli typ jednostki jest skonfigurowany do używania wygenerowanych kluczy, ale wartości klucza są jawnie ustawiane dla nowych wystąpień.
Poprawka jest jawnie skonfigurować właściwości klucza, aby nie używać wygenerowanych wartości.
Na przykład z płynnym interfejsem API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Lub z adnotacjami danych:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Kaskadowe usunięcia są teraz usuwane natychmiast domyślnie

[#10114 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Stare zachowanie**

Przed 3.0 EF Core stosowane akcje kaskadowe (usuwanie jednostek zależnych, gdy wymagany podmiot zabezpieczeń jest usuwany lub gdy relacja z wymaganym podmiotem jest zerwana) nie stało się, dopóki SaveChanges został wywołany.

**Nowe zachowanie**

Począwszy od 3.0, EF Core stosuje akcje kaskadowe, gdy tylko zostanie wykryty warunek wyzwalania.
Na przykład `context.Remove()` wywołanie usunięcia jednostki głównej spowoduje, że wszystkie śledzone powiązane `Deleted` wymagane zależności również zostaną ustawione natychmiast.

**Dlaczego**

Ta zmiana została wywiązywana w celu poprawy środowiska dla scenariuszy powiązania danych i inspekcji, w których ważne jest, aby zrozumieć, które jednostki zostaną usunięte _przed_ `SaveChanges` wywołaniem.

**Środki zaradcze**

Poprzednie zachowanie można przywrócić za `context.ChangeTracker`pomocą ustawień na .
Przykład:

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>Wczesne ładowanie powiązanych jednostek odbywa się teraz w jednej kwerendzie

[#18022 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Stare zachowanie**

Przed 3.0, chętnie ładowania nawigacji `Include` kolekcji za pośrednictwem operatorów spowodowało wiele zapytań do wygenerowania w relacyjnej bazy danych, po jednym dla każdego typu jednostki pokrewnej.

**Nowe zachowanie**

Począwszy od 3.0, EF Core generuje pojedyncze zapytanie z JOIN w relacyjnych baz danych.

**Dlaczego**

Wydawanie wielu zapytań w celu zaimplementowania pojedynczej kwerendy LINQ spowodowało wiele problemów, w tym negatywną wydajność, ponieważ konieczne było wiele rund w obie strony bazy danych, a problemy z spójnością danych, ponieważ każda kwerenda mogła obserwować inny stan bazy danych.

**Środki zaradcze**

Chociaż technicznie nie jest to zmiana podziału, może mieć znaczący wpływ na wydajność aplikacji, gdy pojedyncze zapytanie zawiera dużą liczbę operatorów `Include` na nawigacji zbierania. [Zobacz ten komentarz,](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) aby uzyskać więcej informacji i przepisywania zapytań w bardziej efektywny sposób.

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict ma czystsze semantyki

[#12661 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Stare zachowanie**

Przed 3.0, `DeleteBehavior.Restrict` utworzone klucze `Restrict` obce w bazie danych z semantyki, ale także zmienił wewnętrzne korekty w sposób nieoczywidycy.

**Nowe zachowanie**

Począwszy od 3.0, zapewnia, `DeleteBehavior.Restrict` że `Restrict` klucze obce są tworzone z semantyki - to znaczy, bez kaskad; rzut na naruszenie ograniczeń - bez wpływu również EF wewnętrzne korekty.

**Dlaczego**

Ta zmiana została wniesienia w celu poprawy doświadczenia podczas korzystania `DeleteBehavior` w intuicyjny sposób, bez nieoczekiwanych skutków ubocznych.

**Środki zaradcze**

Poprzednie zachowanie można przywrócić `DeleteBehavior.ClientNoAction`za pomocą programu .

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Typy zapytań są konsolidowane za pomocą typów encji

[#14194 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Stare zachowanie**

Przed EF Core 3.0 [typy zapytań](xref:core/modeling/keyless-entity-types) były sposobem na zapytanie danych, które nie definiują klucza podstawowego w sposób strukturalny.
Oznacza to, że typ kwerendy był używany do mapowania typów jednostek bez kluczy (bardziej prawdopodobne z widoku, ale prawdopodobnie z tabeli), podczas gdy zwykły typ jednostki był używany, gdy klucz był dostępny (bardziej prawdopodobne z tabeli, ale prawdopodobnie z widoku).

**Nowe zachowanie**

Typ kwerendy staje się teraz tylko typem jednostki bez klucza podstawowego.
Typy jednostek bezkluzywowe mają taką samą funkcjonalność jak typy zapytań w poprzednich wersjach.

**Dlaczego**

Ta zmiana została wydana w celu zmniejszenia zamieszania wokół celu typów kwerend.
W szczególności są one bezkluzywne typy jednostek i są one z natury tylko do odczytu z tego powodu, ale nie powinny być używane tylko dlatego, że typ jednostki musi być tylko do odczytu.
Podobnie są one często mapowane do widoków, ale to tylko dlatego, że widoki często nie definiują kluczy.

**Środki zaradcze**

Następujące części interfejsu API są przestarzałe:
* **`ModelBuilder.Query<>()`**- Zamiast `ModelBuilder.Entity<>().HasNoKey()` tego należy wywołać, aby oznaczyć typ jednostki jako posiadające żadnych kluczy.
To nadal nie będzie skonfigurowany przez konwencję, aby uniknąć błędnej konfiguracji, gdy oczekuje się klucza podstawowego, ale nie pasuje do konwencji.
* **`DbQuery<>`**- Zamiast `DbSet<>` tego należy użyć.
* **`DbContext.Query<>()`**- Zamiast `DbContext.Set<>()` tego należy użyć.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>Interfejs API konfiguracji dla relacji typu należącego uległ zmianie

[Problem śledzenia #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[problem śledzenia #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Stare zachowanie**

Przed EF Core 3.0 konfiguracja relacji należącej `OwnsOne` do `OwnsMany` własnością została wykonana bezpośrednio po lub wywołania. 

**Nowe zachowanie**

Począwszy od EF Core 3.0, istnieje teraz płynny interfejs API, aby skonfigurować właściwość nawigacji do właściciela za pomocą `WithOwner()`.
Przykład:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

Konfiguracja związana z relacją między właścicielem a `WithOwner()` własnością powinna być teraz powiązana po podobnej do sposobu konfigurowania innych relacji.
Podczas gdy konfiguracja dla samego typu należącego do sieci nadal będzie pokutowana łańcuchem po `OwnsOne()/OwnsMany()`.
Przykład:

```csharp
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

Dodatkowo `Entity()`wywołanie `HasOne()`, `Set()` lub z własnością docelowej typu będzie teraz zgłosić wyjątek.

**Dlaczego**

Ta zmiana została wydzielona w celu utworzenia czystszego oddzielenia między konfigurowaniem samego typu należącego a _relacją z_ typem należącym do państwa.
To z kolei usuwa niejednoznaczności i zamieszanie `HasForeignKey`wokół metod takich jak .

**Środki zaradcze**

Zmień konfigurację relacji typu należącego do sieci, aby użyć nowej powierzchni interfejsu API, jak pokazano w powyższym przykładzie.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Jednostki zależne dzielące tabelę z podmiotem głównym są teraz opcjonalne

[#9005 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**Stare zachowanie**

Należy wziąć pod uwagę następujący model:
```csharp
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
Przed EF Core `OrderDetails` 3.0, `Order` jeśli jest własnością lub jawnie `OrderDetails` mapowane do tej samej `Order`tabeli, a następnie wystąpienie było zawsze wymagane podczas dodawania nowego . .


**Nowe zachowanie**

Począwszy od 3.0 EF Core `Order` pozwala `OrderDetails` dodać bez `OrderDetails` i mapuje wszystkie właściwości z wyjątkiem klucza podstawowego do kolumn nullable.
Podczas wykonywania zapytań `OrderDetails` `null` EF Core ustawia, czy którykolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie `null`ma wymaganych właściwości oprócz klucza podstawowego i wszystkie właściwości są .

**Środki zaradcze**

Jeśli model ma udostępnianie tabeli zależne od wszystkich kolumn opcjonalnych, ale `null` nawigacja wskazująca na niego nie powinna `null`być, aplikacja powinna zostać zmodyfikowana do obsługi przypadków, gdy nawigacja jest . Jeśli nie jest to możliwe, wymagana właściwość powinna zostać dodana do typu`null` jednostki lub co najmniej jedna właściwość powinna mieć przypisaną do niej wartość nienabytą.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Wszystkie jednostki współużytkujące tabelę z kolumną tokenu współbieżności muszą mapować ją na właściwość

[#14154 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**Stare zachowanie**

Należy wziąć pod uwagę następujący model:
```csharp
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
Przed EF Core `OrderDetails` 3.0, `Order` jeśli jest własnością lub jawnie mapowane `OrderDetails` do `Version` tej samej tabeli, a następnie aktualizowanie po prostu nie zaktualizuje wartość na kliencie i następna aktualizacja zakończy się niepowodzeniem.


**Nowe zachowanie**

Począwszy od 3.0, EF Core `Version` propaguje nową wartość, jeśli `Order` jest właścicielem `OrderDetails`. W przeciwnym razie wyjątek jest zgłaszany podczas sprawdzania poprawności modelu.

**Dlaczego**

Ta zmiana została wniesienia, aby uniknąć przestarzałej wartości tokenu współbieżności, gdy tylko jedna z jednostek mapowanych do tej samej tabeli jest aktualizowana.

**Środki zaradcze**

Wszystkie jednostki współużytkujące tabelę muszą zawierać właściwość mapowane do kolumny tokenu współbieżności. Jest możliwe, aby utworzyć jeden w stanie cienia:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>Nie można wyszukiwać jednostek będących własnością bez użycia zapytania śledzenia przez właściciela

[#18876 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**Stare zachowanie**

Przed EF Core 3.0, należące do jednostek można wyszukiwać jako inne nawigacji.

```csharp
context.People.Select(p => p.Address);
```

**Nowe zachowanie**

Począwszy od 3.0 EF Core zrzuci, jeśli kwerenda śledzenia projektów jednostki własnością bez właściciela.

**Dlaczego**

Jednostki należące nie mogą być manipulowane bez właściciela, więc w większości przypadków ich wykonywanie zapytań w ten sposób jest błędem.

**Środki zaradcze**

Jeśli jednostka należąca powinna być śledzona do zmodyfikowannia w jakikolwiek sposób później, właściciel powinien zostać uwzględniony w zapytaniu.

W przeciwnym `AsNoTracking()` razie dodaj połączenie:

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Właściwości dziedziczone z typów niezamapowanych są teraz mapowane na jedną kolumnę dla wszystkich typów pochodnych

[#13998 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**Stare zachowanie**

Należy wziąć pod uwagę następujący model:
```csharp
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

Przed EF Core 3.0, `ShippingAddress` właściwość będzie mapowane `BulkOrder` `Order` do oddzielnych kolumn dla i domyślnie.

**Nowe zachowanie**

Począwszy od 3.0, EF Core `ShippingAddress`tworzy tylko jedną kolumnę dla .

**Dlaczego**

Stary behavoir był nieoczekiwany.

**Środki zaradcze**

Właściwość nadal może być jawnie mapowane do oddzielnej kolumny na typy pochodne:

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>Konwencja właściwości klucza obcego nie jest już zgodna z tą samą nazwą co właściwość główna

[#13274 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**Stare zachowanie**

Należy wziąć pod uwagę następujący model:
```csharp
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
Przed EF Core 3.0, `CustomerId` właściwość będzie używana dla klucza obcego przez konwencję.
Jednak jeśli `Order` jest typem własnością, to `CustomerId` również zrobić klucz podstawowy i zwykle nie jest to oczekiwanie.

**Nowe zachowanie**

Począwszy od 3.0, EF Core nie próbuje używać właściwości dla kluczy obcych przez konwencję, jeśli mają taką samą nazwę jak principal właściwości.
Nazwa typu głównego połączona z nazwą właściwości głównej i nazwa nawigacji połączona z wzorcami nazw właściwości głównej są nadal dopasowywać.
Przykład:

```csharp
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

```csharp
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

**Dlaczego**

Ta zmiana została wyjęty, aby uniknąć błędnego definiowania właściwości klucza podstawowego na typie należącym do organizacji.

**Środki zaradcze**

Jeśli właściwość miała być kluczem obcym, a tym samym częścią klucza podstawowego, a następnie jawnie skonfigurować go jako takie.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>Połączenie z bazą danych jest teraz zamknięte, jeśli nie jest już używane przed zakończeniem transakcji TransactionScope

[#14218 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Stare zachowanie**

Przed EF Core 3.0, jeśli kontekst `TransactionScope`otwiera połączenie wewnątrz , połączenie `TransactionScope` pozostaje otwarte, gdy bieżący jest aktywny.

```csharp
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

Począwszy od 3.0, EF Core zamyka połączenie, gdy tylko zostanie to zrobione przy użyciu go.

**Dlaczego**

Ta zmiana pozwala na użycie wielu `TransactionScope`kontekstów w tym samym . Nowe zachowanie jest również zgodne z EF6.

**Środki zaradcze**

Jeśli połączenie musi pozostać otwarte `OpenConnection()` jawne wywołanie zapewni, że EF Core nie zamyka go przedwcześnie:

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Każda właściwość używa niezależnego generowania klucza całkowitej w pamięci

[#6872 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Stare zachowanie**

Przed EF Core 3.0 jeden generator wartości udostępnionej został użyty dla wszystkich właściwości klucza całkowitej w pamięci.

**Nowe zachowanie**

Począwszy od EF Core 3.0, każda właściwość klucza całkowitej pobiera własny generator wartości podczas korzystania z bazy danych w pamięci.
Ponadto jeśli baza danych zostanie usunięta, generowanie kluczy jest resetowane dla wszystkich tabel.

**Dlaczego**

Ta zmiana została wydzielona w celu dostosowania generowania klucza w pamięci bliżej do generowania klucza prawdziwej bazy danych i poprawić możliwość izolowania testów od siebie podczas korzystania z bazy danych w pamięci.

**Środki zaradcze**

Może to spowodować przerwanie aplikacji, która polega na określonych wartościach klucza w pamięci, które mają zostać ustawione.
Zamiast tego należy rozważyć nie poleganie na określonych wartości klucza lub aktualizowanie, aby dopasować nowe zachowanie.

### <a name="backing-fields-are-used-by-default"></a>Pola zapasowe są używane domyślnie

[#12430 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Stare zachowanie**

Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.
Wyjątkiem było wykonanie kwerendy, gdzie pole kopii będzie ustawiane bezpośrednio, jeśli jest znane.

**Nowe zachowanie**

Począwszy od EF Core 3.0, jeśli pole zapasowe dla właściwości jest znane, a następnie EF Core zawsze odczytuje i zapisuje tę właściwość przy użyciu pola zapasowego.
This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.

**Dlaczego**

Ta zmiana została włączona, aby zapobiec EF Core błędnie wyzwalania logiki biznesowej domyślnie podczas wykonywania operacji bazy danych z udziałem jednostek.

**Środki zaradcze**

Zachowanie pre-3.0 można przywrócić poprzez konfigurację trybu `ModelBuilder`dostępu do właściwości na .
Przykład:

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Throw, jeśli znaleziono wiele zgodnych pól podkładowych

[#12523 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Stare zachowanie**

Przed EF Core 3.0, jeśli wiele pól pasuje do reguł znajdowania pola zapasowego właściwości, a następnie jedno pole zostanie wybrany na podstawie pewnej kolejności pierwszeństwa.
Może to spowodować, że niewłaściwe pole będzie używane w niejednoznacznych przypadkach.

**Nowe zachowanie**

Począwszy od EF Core 3.0, jeśli wiele pól są dopasowane do tej samej właściwości, a następnie wyjątek.

**Dlaczego**

Ta zmiana została wyjęna, aby uniknąć dyskretnego używania jednego pola nad drugim, gdy tylko jedno może być poprawne.

**Środki zaradcze**

Właściwości z niejednoznacznymi polami zapasowymi muszą mieć pole do jawnego użycia.
Na przykład przy użyciu płynnego interfejsu API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Nazwy właściwości tylko do pól powinny być zgodne z nazwą pola

**Stare zachowanie**

Przed EF Core 3.0, właściwość może być określona przez wartość ciągu i jeśli nie znaleziono żadnej właściwości o tej nazwie w typie .NET, a następnie EF Core spróbuje dopasować go do pola przy użyciu reguł konwencji.

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Nowe zachowanie**

Począwszy od EF Core 3.0, właściwość tylko do pól musi dokładnie odpowiadać nazwie pola.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Dlaczego**

Ta zmiana została wytyczona w celu uniknięcia używania tego samego pola dla dwóch właściwości o nazwie podobnie, ale także sprawia, że pasujące reguły dla właściwości tylko w polu są takie same, jak w przypadku właściwości mapowanych do właściwości CLR.

**Środki zaradcze**

Właściwości tylko pola muszą mieć taką samą nazwę jak pole, do którego są mapowane.
W przyszłej wersji EF Core po 3.0 planujemy ponownie włączyć jawne skonfigurowanie nazwy pola, która różni się od nazwy właściwości (patrz [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)problem):

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool nie będzie już wywoływać addlogging i AddMemoryCache

[#14756 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Stare zachowanie**

Przed EF Core `AddDbContext` 3.0, wywołanie lub `AddDbContextPool` również zarejestrować rejestrowanie i buforowanie pamięci usług z DI poprzez wywołania [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) i [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nowe zachowanie**

Począwszy od EF Core 3.0 `AddDbContext` i `AddDbContextPool` nie będzie już rejestrować tych usług z iniekcji zależności (DI).

**Dlaczego**

EF Core 3.0 nie wymaga, aby te usługi znajdują się w kontenerze DI aplikacji. Jednak jeśli `ILoggerFactory` jest zarejestrowany w kontenerze DI aplikacji, a następnie będzie nadal używany przez EF Core.

**Środki zaradcze**

Jeśli aplikacja potrzebuje tych usług, a następnie zarejestrować je jawnie z kontenera DI przy użyciu [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) lub [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFramework* dodaje IMemoryCache z limitem rozmiaru

[#12905 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**Stare zachowanie**

Przed EF Core 3.0 metody wywoływania `AddEntityFramework*` również zarejestrować usługi buforowania pamięci z DI bez limitu rozmiaru.

**Nowe zachowanie**

Począwszy od EF Core 3.0, `AddEntityFramework*` zarejestruje usługę IMemoryCache z limitem rozmiaru. Jeśli inne usługi dodane później zależą od IMemoryCache mogą szybko osiągnąć domyślny limit powodujący wyjątki lub obniżoną wydajność.

**Dlaczego**

Przy użyciu IMemoryCache bez limitu może spowodować użycie pamięci niekontrolowane, jeśli występuje błąd w logice buforowania kwerend lub kwerendy są generowane dynamicznie. Posiadanie domyślnego limitu zmniejsza potencjalny atak DoS.

**Środki zaradcze**

W większości `AddEntityFramework*` przypadków wywołanie `AddDbContext` `AddDbContextPool` nie jest konieczne, jeśli lub jest wywoływana, jak również. W związku z tym najlepszym środkiem zaradczym jest usunięcie wywołania. `AddEntityFramework*`

Jeśli aplikacja potrzebuje tych usług, a następnie zarejestrować implementację IMemoryCache jawnie z kontenerem DI wcześniej przy użyciu [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry wykonuje teraz lokalne zmiany wykrywania

[#13552 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Stare zachowanie**

Przed EF Core 3.0 wywołanie `DbContext.Entry` spowoduje, że zmiany mają być wykrywane dla wszystkich śledzonych jednostek.
Dzięki temu stan ujawniony `EntityEntry` w tym kraju był aktualny.

**Nowe zachowanie**

Począwszy od EF Core 3.0, wywołanie `DbContext.Entry` będzie teraz tylko próbować wykryć zmiany w danej jednostce i wszystkie śledzone jednostki główne związane z nim.
Oznacza to, że zmiany w innym miejscu może nie zostały wykryte przez wywołanie tej metody, co może mieć wpływ na stan aplikacji.

Należy zauważyć, że jeśli `ChangeTracker.AutoDetectChangesEnabled` jest ustawiona na `false` to nawet to wykrywanie zmian lokalnych zostanie wyłączone.

Inne metody, które powodują wykrywanie `SaveChanges`zmian — na `DetectChanges` przykład `ChangeTracker.Entries` i --nadal powodują pełne wszystkich śledzonych jednostek.

**Dlaczego**

Ta zmiana została włączona w `context.Entry`celu poprawy domyślnej wydajności używania programu .

**Środki zaradcze**

Wywołanie `ChangeTracker.DetectChanges()` jawnie `Entry` przed wywołaniem, aby upewnić się, że zachowanie pre-3.0.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Klucze tablicy ciągów i bajtów nie są domyślnie generowane przez klienta

[#14617 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Stare zachowanie**

Przed EF Core 3.0 `string` i `byte[]` właściwości klucza może być używany bez jawnie ustawienie wartości innej niż null.
W takim przypadku wartość klucza zostanie wygenerowana na kliencie jako identyfikator `byte[]`GUID, serializowany do bajtów dla .

**Nowe zachowanie**

Począwszy od EF Core 3.0 zostanie zgłoszony wyjątek wskazujący, że nie ustawiono żadnej wartości klucza.

**Dlaczego**

Ta zmiana została wnieszona, ponieważ wartości generowane przez `string` / `byte[]` klienta zazwyczaj nie są przydatne, a domyślne zachowanie utrudniało rozumienie o generowanych wartościach klucza w typowy sposób.

**Środki zaradcze**

Zachowanie pre-3.0 można uzyskać, wyraźnie określając, że właściwości klucza należy użyć wygenerowanych wartości, jeśli nie ma innej wartości innej wartości nie null.
Na przykład z płynnym interfejsem API:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Lub z adnotacjami danych:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory jest teraz usługą o określonym zakresie

[#14698 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Stare zachowanie**

Przed EF Core 3.0, `ILoggerFactory` został zarejestrowany jako usługa singleton.

**Nowe zachowanie**

Począwszy od EF Core 3.0, `ILoggerFactory` jest teraz zarejestrowany jako zakres.

**Dlaczego**

Ta zmiana została wytyczona w `DbContext` celu umożliwienia skojarzenia rejestratora z wystąpieniem, co umożliwia inne funkcje i usuwa niektóre przypadki patologicznych zachowań, takich jak eksplozja wewnętrznych dostawców usług.

**Środki zaradcze**

Ta zmiana nie powinna mieć wpływu na kod aplikacji, chyba że rejestruje i korzysta z usług niestandardowych na wewnętrznym dostawcy usług EF Core.
To nie jest powszechne.
W takich przypadkach większość rzeczy będzie nadal działać, ale `ILoggerFactory` każda usługa singleton, `ILoggerFactory` która była zależna od, będzie musiała zostać zmieniona, aby uzyskać w inny sposób.

Jeśli napotkasz takie sytuacje, zgłoś problem w [monitorze problemów EF Core GitHub,](https://github.com/aspnet/EntityFrameworkCore/issues) aby poinformować nas, jak używasz, `ILoggerFactory` abyśmy mogli lepiej zrozumieć, jak nie złamać tego ponownie w przyszłości.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Serwery proxy z opóźnieniem nie zakładają już, że właściwości nawigacji są w pełni załadowane

[#12780 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Stare zachowanie**

Przed EF Core 3.0, po a `DbContext` został usunięty nie było możliwości dowiedzenia się, czy dana właściwość nawigacji na jednostki uzyskane z tego kontekstu został w pełni załadowany, czy nie.
Zamiast tego serwery proxy załóżmy, że nawigacja referencyjna jest ładowana, jeśli ma wartość niezerową i że nawigacja z kolekcją jest ładowana, jeśli nie jest pusta.
W takich przypadkach próby z opóźnieniem obciążenia byłoby no-op.

**Nowe zachowanie**

Począwszy od EF Core 3.0, serwery proxy śledzić, czy właściwość nawigacji jest ładowany.
Oznacza to, że próba uzyskania dostępu do właściwości nawigacji, która jest ładowana po usunięciu kontekstu zawsze będzie no-op, nawet wtedy, gdy załadowana nawigacja jest pusta lub null.
Z drugiej strony próba uzyskania dostępu do właściwości nawigacji, która nie jest ładowana, zgłosić wyjątek, jeśli kontekst jest usuwany, nawet jeśli właściwość nawigacji jest niepustą kolekcją.
Jeśli taka sytuacja wystąpi, oznacza to, że kod aplikacji próbuje użyć ładowania z opóźnieniem w nieprawidłowym czasie, a aplikacja powinna zostać zmieniona, aby tego nie robić.

**Dlaczego**

Ta zmiana została wniesiena, aby zachowanie spójne i poprawne `DbContext` podczas próby z opóźnieniem obciążenia na disposed wystąpienia.

**Środki zaradcze**

Zaktualizuj kod aplikacji, aby nie podejmować prób ładowania z opóźnieniem z usuniętym kontekstem lub skonfigurować go jako no-op, jak opisano w komunikacie o wyjątku.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Nadmierne tworzenie wewnętrznych usługodawców jest teraz domyślnie błędem

[#10236 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Stare zachowanie**

Przed EF Core 3.0, ostrzeżenie będzie rejestrowane dla aplikacji tworzącej patologiczną liczbę wewnętrznych dostawców usług.

**Nowe zachowanie**

Począwszy od EF Core 3.0, to ostrzeżenie jest teraz brane pod uwagę i błąd i wyjątek. 

**Dlaczego**

Ta zmiana została wprowadzona do kierowania lepszym kodem aplikacji poprzez uwidacznianie tego przypadku patologicznego bardziej jawnie.

**Środki zaradcze**

Najbardziej odpowiednią przyczyną działania w przypadku napotkania tego błędu jest zrozumienie głównej przyczyny i zaprzestanie tworzenia tak wielu wewnętrznych dostawców usług.
Jednak błąd można przekonwertować z powrotem na ostrzeżenie (lub zignorować) za pomocą konfiguracji na . `DbContextOptionsBuilder`
Przykład:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nowe zachowanie hasone/hasmany wywoływane z jednym ciągiem

[#9171 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Stare zachowanie**

Przed EF Core 3.0, wywoływanie `HasOne` kodu lub `HasMany` z jednego ciągu został zinterpretowany w sposób mylący.
Przykład:
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Kod wygląda jak odnosi `Samurai` się do innego typu `Entrance` jednostki przy użyciu właściwości nawigacji, która może być prywatna.

W rzeczywistości ten kod próbuje utworzyć relację `Entrance` do pewnego typu jednostki o nazwie bez właściwości nawigacji.

**Nowe zachowanie**

Począwszy od EF Core 3.0, kod powyżej teraz robi to, co wyglądało to powinno było robić wcześniej.

**Dlaczego**

Stare zachowanie było bardzo mylące, zwłaszcza podczas odczytywania kodu konfiguracji i szukania błędów.

**Środki zaradcze**

Spowoduje to tylko przerwanie aplikacji, które jawnie konfigurują relacje przy użyciu ciągów dla nazw typów i bez jawnego określania właściwości nawigacji.
Nie jest to powszechne.
Poprzednie zachowanie można uzyskać za `null` pośrednictwem jawnie przekazywania nazwy właściwości nawigacji.
Przykład:

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>Typ zwracany dla kilku metod asynchronicznego został zmieniony z Task na ValueTask

[#15184 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Stare zachowanie**

Następujące metody asynchronowe `Task<T>`wcześniej zwróciły:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()`(i klasy pochodne)

**Nowe zachowanie**

Wyżej wymienione metody zwracają `ValueTask<T>` teraz ponad `T` to samo, co poprzednio.

**Dlaczego**

Ta zmiana zmniejsza liczbę alokacji sterty poniesione podczas wywoływania tych metod, poprawiając ogólną wydajność.

**Środki zaradcze**

Aplikacje po prostu oczekujące na powyższe interfejsy API muszą być tylko ponownie skompilowane — nie są konieczne żadne zmiany źródła.
Bardziej złożone użycie (np. przekazywanie `Task` `Task.WhenAny()`zwrócone do) zazwyczaj `ValueTask<T>` wymagają, aby `Task<T>` zwrócony być konwertowane na a, wywołując `AsTask()` na nim.
Należy zauważyć, że to neguje redukcję alokacji, która przynosi tę zmianę.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>Adnotacja relacyjna:TypeMapping jest teraz tylko TypeMapping

[#9913 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Stare zachowanie**

Nazwa adnotacji adnotacji mapowania typów to "Relational:TypeMapping".

**Nowe zachowanie**

Nazwa adnotacji adnotacji mapowania typów jest teraz "TypeMapping".

**Dlaczego**

Mapowania typów są teraz używane dla więcej niż tylko relacyjnych dostawców bazy danych.

**Środki zaradcze**

Spowoduje to tylko przerwanie aplikacji, które uzyskują dostęp do mapowania typów bezpośrednio jako adnotacji, co nie jest powszechne.
Najbardziej odpowiednią akcją do naprawienia jest użycie powierzchni interfejsu API, aby uzyskać dostęp do mapowań typu, a nie bezpośrednio przy użyciu adnotacji.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable na typ pochodny zgłasza wyjątek 

[#11811 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Stare zachowanie**

Przed EF Core 3.0, `ToTable()` wywoływane na typ pochodny będą ignorowane, ponieważ tylko strategia mapowania dziedziczenia był TPH, gdzie to nie jest prawidłowe. 

**Nowe zachowanie**

Począwszy od EF Core 3.0 i w ramach przygotowań do `ToTable()` dodawania obsługi TPT i TPC w nowszej wersji, wywoływane na typ pochodny będzie teraz zgłosić wyjątek, aby uniknąć nieoczekiwanej zmiany mapowania w przyszłości.

**Dlaczego**

Obecnie nie jest prawidłowe mapowanie typu pochodnego do innej tabeli.
Ta zmiana pozwala uniknąć zerwania w przyszłości, gdy staje się ważną rzeczą do zrobienia.

**Środki zaradcze**

Usuń wszelkie próby mapowania typów pochodnych do innych tabel.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex zastąpiony hasindexem 

[#12366 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Stare zachowanie**

Przed EF Core 3.0, pod warunkiem, `ForSqlServerHasIndex().ForSqlServerInclude()` że `INCLUDE`sposób konfigurowania kolumn używanych z .

**Nowe zachowanie**

Począwszy od EF Core 3.0, przy użyciu `Include` w indeksie jest teraz obsługiwany na poziomie relacyjnej.
Użyj witryny `HasIndex().ForSqlServerInclude()`.

**Dlaczego**

Ta zmiana została wprowadzona w celu `Include` konsolidacji interfejsu API dla indeksów w jednym miejscu dla wszystkich dostawców bazy danych.

**Środki zaradcze**

Użyj nowego interfejsu API, jak pokazano powyżej.

### <a name="metadata-api-changes"></a>Zmiany interfejsu API metadanych

[#214 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Nowe zachowanie**

Następujące właściwości zostały przekonwertowane na metody rozszerzenia:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Dlaczego**

Zmiana ta upraszcza implementację wyżej wymienionych interfejsów.

**Środki zaradcze**

Użyj nowych metod rozszerzenia.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Zmiany interfejsu API metadanych specyficznych dla dostawcy

[#214 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Nowe zachowanie**

Metody rozszerzenia specyficzne dla dostawcy zostaną spłaszczone:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Dlaczego**

Zmiana ta upraszcza wdrażanie wyżej wymienionych metod rozszerzenia.

**Środki zaradcze**

Użyj nowych metod rozszerzenia.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core nie wysyła już pragmy do egzekwowania SQLite FK

[#12151 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Stare zachowanie**

Przed EF Core 3.0 EF `PRAGMA foreign_keys = 1` Core będzie wysyłać po otwarciu połączenia z SQLite.

**Nowe zachowanie**

Począwszy od EF Core 3.0, `PRAGMA foreign_keys = 1` EF Core nie wysyła już po otwarciu połączenia z SQLite.

**Dlaczego**

Ta zmiana została włączona, `SQLitePCLRaw.bundle_e_sqlite3` ponieważ EF Core używa domyślnie, co z kolei oznacza, że wymuszanie FK jest domyślnie włączone i nie musi być jawnie włączone przy każdym otwarciu połączenia.

**Środki zaradcze**

Klucze obce są domyślnie włączone w pliku SQLitePCLRaw.bundle_e_sqlite3, który jest domyślnie używany dla EF Core.
W innych przypadkach klucze obce można `Foreign Keys=True` włączyć, określając w ciągu połączenia.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite teraz zależy od SQLitePCLRaw.bundle_e_sqlite3

**Stare zachowanie**

Przed EF Core 3.0, `SQLitePCLRaw.bundle_green`EF Core używane .

**Nowe zachowanie**

Począwszy od EF Core 3.0, EF Core używa `SQLitePCLRaw.bundle_e_sqlite3`.

**Dlaczego**

Ta zmiana została wydzielona tak, że wersja SQLite używane na iOS zgodne z innymi platformami.

**Środki zaradcze**

Aby użyć natywnej wersji SQLite `Microsoft.Data.Sqlite` w systemach iOS, skonfiguruj użycie innego `SQLitePCLRaw` pakietu.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Wartości Guid są teraz przechowywane jako tekst na SQLite

[#15078 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Stare zachowanie**

Wartości guid były wcześniej przechowywane jako wartości blob na SQLite.

**Nowe zachowanie**

Wartości guid są teraz przechowywane jako tekst.

**Dlaczego**

Format binarny Guids nie jest standaryzowany. Przechowywanie wartości jako TEXT sprawia, że baza danych jest bardziej zgodna z innymi technologiami.

**Środki zaradcze**

Istniejące bazy danych można migrować do nowego formatu, wykonując program SQL, podobnie jak następujące polecenie.

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

W EF Core można również kontynuować przy użyciu poprzedniego zachowania, konfigurując konwerter wartości na tych właściwościach.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

Microsoft.Data.Sqlite pozostaje w stanie odczytywać wartości Guid zarówno z kolumny BLOB i TEXT; Jednak ponieważ domyślny format parametrów i stałych uległ zmianie, prawdopodobnie będziesz musiał podjąć działania w przypadku większości scenariuszy dotyczących guidów.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Wartości char są teraz przechowywane jako tekst na SQLite

[#15020 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Stare zachowanie**

Wartości char były wcześniej sored jako wartości INTEGER na SQLite. Na przykład wartość char *A* została przechowywana jako wartość całkowita 65.

**Nowe zachowanie**

Wartości char są teraz przechowywane jako TEKST.

**Dlaczego**

Przechowywanie wartości jako TEKST jest bardziej naturalne i sprawia, że baza danych bardziej zgodne z innymi technologiami.

**Środki zaradcze**

Istniejące bazy danych można migrować do nowego formatu, wykonując program SQL, podobnie jak następujące polecenie.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

W EF Core można również kontynuować przy użyciu poprzedniego zachowania, konfigurując konwerter wartości na tych właściwościach.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

Microsoft.Data.Sqlite również pozostaje w stanie odczytywać wartości znaków z kolumn INTEGER i TEXT, więc niektóre scenariusze nie mogą wymagać żadnych akcji.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>Identyfikatory migracji są teraz generowane przy użyciu kalendarza kultury niezmiennej

[#12978 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Stare zachowanie**

Identyfikatory migracji zostały przypadkowo wygenerowane przy użyciu kalendarza bieżącej kultury.

**Nowe zachowanie**

Identyfikatory migracji są teraz zawsze generowane przy użyciu kalendarza kultury niezmiennej (gregoriańskiego).

**Dlaczego**

Kolejność migracji jest ważna podczas aktualizowania bazy danych lub rozwiązywania konfliktów scalania. Za pomocą kalendarza niezmienne pozwala uniknąć zamawiania problemów, które mogą wynikać z członków zespołu o różnych kalendarzy systemowych.

**Środki zaradcze**

Ta zmiana dotyczy każdego, kto używa kalendarza niegregoriańskiego, w którym rok jest większy niż kalendarz gregoriański (jak tajski kalendarz buddyjski). Istniejące identyfikatory migracji będą musiały zostać zaktualizowane, aby nowe migracje były porządkowane po istniejących migracjach.

Identyfikator migracji można znaleźć w atrybucie Migracja w plikach projektanta migracji.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

Tabela historia migracji również musi zostać zaktualizowana.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging został usunięty

[#16400 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**Stare zachowanie**

Przed EF Core 3.0, `UseRowNumberForPaging` może służyć do generowania SQL dla stronicowania, który jest zgodny z programem SQL Server 2008.

**Nowe zachowanie**

Począwszy od EF Core 3.0, EF będzie generować tylko SQL dla stronicowania, który jest zgodny tylko z nowszych wersji programu SQL Server. 

**Dlaczego**

Wprowadzamy tę zmianę, ponieważ [SQL Server 2008 nie jest już obsługiwanym produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) i aktualizowanie tej funkcji do pracy ze zmianami kwerend wprowadzonych w EF Core 3.0 jest znaczącą pracę.

**Środki zaradcze**

Zaleca się aktualizowanie do nowszej wersji programu SQL Server lub przy użyciu wyższego poziomu zgodności, tak aby wygenerowany SQL jest obsługiwany. Mając na to na myśli, jeśli nie jesteś w stanie tego zrobić, prosimy o [komentarz w sprawie śledzenia problemu](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ze szczegółami. Możemy ponownie przyjrzeć się tej decyzji na podstawie opinii.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>Informacje/metadane rozszerzenia zostały usunięte z IDbContextOptionsExtension

[#16119 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**Stare zachowanie**

`IDbContextOptionsExtension`zawiera metody dostarczania metadanych dotyczących rozszerzenia.

**Nowe zachowanie**

Te metody zostały przeniesione `DbContextOptionsExtensionInfo` do nowej abstrakcyjnej klasy podstawowej, która jest zwracana z nowej `IDbContextOptionsExtension.Info` właściwości.

**Dlaczego**

W wersjach od 2.0 do 3.0 musieliśmy dodać lub zmienić te metody kilka razy.
Podział ich na nową abstrakcyjną klasę podstawową ułatwi wprowadzanie tego rodzaju zmian bez przerywania istniejących rozszerzeń.

**Środki zaradcze**

Aktualizuj rozszerzenia, aby postępować zgodnie z nowym wzorcem.
Przykłady znajdują się w wielu `IDbContextOptionsExtension` implementacjach dla różnych rodzajów rozszerzeń w kodzie źródłowym EF Core.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>Nazwa logqueryPossibleExceptionWithAggregateOperator została zmieniona

[#10985 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**Zmień**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`została zmieniona `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na .

**Dlaczego**

Wyrównuje nazwę tego zdarzenia ostrzegawczego ze wszystkimi innymi zdarzeniami ostrzegawczymi.

**Środki zaradcze**

Użyj nowej nazwy. (Należy zauważyć, że numer identyfikatora zdarzenia nie uległ zmianie).

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Wyjaśnienie interfejsu API dla nazw ograniczeń klucza obcego

[#10730 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Stare zachowanie**

Przed EF Core 3.0 nazwy ograniczeń klucza obcego były określane jako po prostu "nazwa". Przykład:

```csharp
var constraintName = myForeignKey.Name;
```

**Nowe zachowanie**

Począwszy od EF Core 3.0, nazwy ograniczeń klucza obcego są teraz określane jako "nazwa ograniczenia". Przykład:

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**Dlaczego**

Ta zmiana zapewnia spójność nazewnictwa w tym obszarze, a także wyjaśnia, że jest to nazwa ograniczenia klucza obcego, a nie kolumna lub nazwa właściwości, na którym jest zdefiniowany klucz obcy.

**Środki zaradcze**

Użyj nowej nazwy.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator.HasTables/HasTablesAsync zostały upublicznione

[#15997 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**Stare zachowanie**

Przed EF Core 3.0 te metody były chronione.

**Nowe zachowanie**

Począwszy od EF Core 3.0, te metody są publiczne.

**Dlaczego**

Te metody są używane przez EF do określenia, czy baza danych jest tworzona, ale puste. Może to być również przydatne spoza EF przy określaniu, czy należy zastosować migracje.

**Środki zaradcze**

Zmień dostępność wszystkich nadpisań.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Microsoft.EntityFrameworkCore.Design jest teraz pakietem rozwojuzależności

[#11506 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**Stare zachowanie**

Przed EF Core 3.0, Microsoft.EntityFrameworkCore.Design był regularnym pakietem NuGet, którego zestaw może być przywoływane przez projekty, które od niego zależały.

**Nowe zachowanie**

Począwszy od EF Core 3.0, jest pakietem rozwojuzależności. Oznacza to, że zależność nie będzie przepływać przechodnie do innych projektów i że nie można już, domyślnie, odwoływać się do jego zestawu.

**Dlaczego**

Ten pakiet jest przeznaczony tylko do użycia w czasie projektowania. Wdrożone aplikacje nie powinny się do niego odwoływać. Uczynienie pakietu developmentDependency wzmacnia to zalecenie.

**Środki zaradcze**

Jeśli chcesz odwołać się do tego pakietu, aby zastąpić zachowanie EF Core w czasie projektowania, następnie można zaktualizować packagereference metadanych elementu w projekcie.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Jeśli pakiet jest przywoływany przechodnie za pośrednictwem microsoft.EntityFrameworkCore.Tools, należy dodać jawne PackageReference do pakietu, aby zmienić jego metadane. Takie wyraźne odwołanie należy dodać do każdego projektu, w którym potrzebne są typy z pakietu.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL.raw zaktualizowany do wersji 2.0.0

[#14824 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**Stare zachowanie**

Microsoft.EntityFrameworkCore.Sqlite wcześniej zależało od wersji 1.1.12 sqlitePCL.raw.

**Nowe zachowanie**

Zaktualizowaliśmy nasz pakiet tak, aby był zależny od wersji 2.0.0.

**Dlaczego**

Wersja 2.0.0 celów SQLitePCL.raw .NET Standard 2.0. Wcześniej była ukierunkowana na standard .NET Standard 1.1, który wymagał dużego zamknięcia przechodnich paczek do pracy.

**Środki zaradcze**

SQLitePCL.raw wersja 2.0.0 zawiera pewne przełomowe zmiany. Szczegółowe informacje można znaleźć w [informacjach o wersji.](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md)

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite zaktualizowano do wersji 2.0.0

[#14825 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**Stare zachowanie**

Pakiety przestrzenne wcześniej zależały od wersji 1.15.1 NetTopologySuite.

**Nowe zachowanie**

Zaktualizowaliśmy nasz pakiet, aby był zależny od wersji 2.0.0.

**Dlaczego**

Wersja 2.0.0 NetTopologySuite ma na celu rozwiązanie kilku problemów z użytecznością napotkanych przez użytkowników EF Core.

**Środki zaradcze**

NetTopologySuite wersja 2.0.0 zawiera kilka przełomowych zmian. Szczegółowe informacje można znaleźć w [informacjach o wersji.](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001)

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>Microsoft.Data.SqlClient jest używany zamiast System.Data.SqlClient

[#15636 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Stare zachowanie**

Microsoft.EntityFrameWorkCore.SqlServer wcześniej zależało od System.Data.SqlClient.

**Nowe zachowanie**

Zaktualizowaliśmy nasz pakiet, aby był zależny od pliku Microsoft.Data.SqlClient.

**Dlaczego**

Microsoft.Data.SqlClient jest flagowym sterownikiem dostępu do danych dla programu SQL Server w przyszłości, a System.Data.SqlClient nie jest już przedmiotem rozwoju.
Niektóre ważne funkcje, takie jak Zawsze zaszyfrowane, są dostępne tylko w witrynie Microsoft.Data.SqlClient.

**Środki zaradcze**

Jeśli kod ma bezpośrednią zależność od System.Data.SqlClient, należy go zmienić na odwołanie Microsoft.Data.SqlClient zamiast; ponieważ dwa pakiety zachowują bardzo wysoki stopień zgodności interfejsu API, powinna to być tylko prosta zmiana pakietu i obszaru nazw.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Należy skonfigurować wiele niejednoznacznych relacji samonawodujących się 

[#13573 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**Stare zachowanie**

Typ jednostki z wieloma właściwościami nawigacji jednokierunkowej i pasującymi FK został niepoprawnie skonfigurowany jako pojedyncza relacja. Przykład:

```csharp
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

Ten scenariusz jest teraz wykrywany w budynku modelu i wyjątek wskazuje, że model jest niejednoznaczny.

**Dlaczego**

Wynikowy model był niejednoznaczny i prawdopodobnie zwykle będzie błędny w tym przypadku.

**Środki zaradcze**

Użyj pełnej konfiguracji relacji. Przykład:

```csharp
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction.Schema jest null lub pusty ciąg konfiguruje go do domyślnego schematu modelu

[#12757 problemu śledzenia](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Stare zachowanie**

DbFunction skonfigurowany ze schematem jako pusty ciąg został potraktowany jako wbudowana funkcja bez schematu. Na przykład następujący `DatePart` kod będzie `DATEPART` mapować funkcję CLR do wbudowanej funkcji na SqlServer.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Nowe zachowanie**

Wszystkie mapowania DbFunction są uważane za mapowane na funkcje zdefiniowane przez użytkownika. W związku z tym wartość pustego ciągu spowoduje umieszczenie funkcji wewnątrz domyślnego schematu dla modelu. Który może być schemat skonfigurowany jawnie `modelBuilder.HasDefaultSchema()` za `dbo` pośrednictwem płynnego interfejsu API lub w inny sposób.

**Dlaczego**

Wcześniej schemat jest pusty był sposób leczenia tej funkcji jest wbudowany, ale logika ta ma zastosowanie tylko do SqlServer, gdzie wbudowane funkcje nie należą do żadnego schematu.

**Środki zaradcze**

Skonfiguruj tłumaczenie DbFunction ręcznie, aby zamapować je na wbudowaną funkcję.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```

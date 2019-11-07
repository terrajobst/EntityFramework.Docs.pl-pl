---
title: Wcześniejsze wersje Entity Framework-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
uid: ef6/what-is-new/past-releases
ms.openlocfilehash: fada7740453cd9a55a1d0069236efcecbd9aa314
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656139"
---
# <a name="past-releases-of-entity-framework"></a>Wcześniejsze wersje Entity Framework

Pierwsza wersja Entity Framework została wydana w 2008 w ramach .NET Framework 3,5 SP1 i Visual Studio 2008 SP1.

Począwszy od wersji programu EF 4.1, dostarczono ją jako [pakiet NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) — obecnie jednym z najpopularniejszych pakietów w witrynie NuGet.org.

Między wersjami 4,1 i 5,0 pakiet NuGet EntityFramework rozszerzył biblioteki EF, które zostały wysłane jako część .NET Framework.

Począwszy od wersji 6, EF stał się projektem typu open source, a także przeniesiono całkowicie poza pasmem .NET Framework.
Oznacza to, że po dodaniu pakietu NuGet EntityFramework w wersji 6 do aplikacji otrzymujesz kompletną kopię biblioteki EF, która nie jest zależna od bitów EF, które są dostarczane jako część .NET Framework.
Zapewnia to nieco skrócenie tempa opracowywania i dostarczania nowych funkcji.

W czerwcu 2016 opublikowano EF Core 1,0. EF Core jest oparta na nowej bazie kodu i jest zaprojektowana jako bardziej uproszczona i rozszerzalna wersja EF.
Obecnie EF Core jest głównym celem rozwoju zespołu Entity Framework w firmie Microsoft.
Oznacza to, że nie ma żadnych nowych głównych funkcji planowanych dla EF6. Jednak EF6 nadal są obsługiwane jako projekt typu open source i obsługiwany produkt firmy Microsoft.

Poniżej znajduje się lista poprzednich wersji, w odwrotnej kolejności chronologicznej, wraz z informacjami dotyczącymi nowych funkcji wprowadzonych w poszczególnych wersjach.

## <a name="ef-tools-update-in-visual-studio-2017-157"></a>Aktualizacja narzędzi EF w programie Visual Studio 2017 15,7
W maju 2018 firma Microsoft udostępniła zaktualizowaną wersję narzędzi EF w ramach programu Visual Studio 2017 15,7.
Obejmuje to ulepszenia niektórych typowych punktów:

- Poprawki dla kilku błędów ułatwień dostępu do interfejsu użytkownika
- Obejście SQL Server regresji wydajności podczas generowania modeli z istniejących baz danych [#4](https://github.com/aspnet/entityframework6/issues/4)
- Obsługa aktualizowania modeli dla większych modeli w SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Innym ulepszeniem tej nowej wersji narzędzi EF jest zainstalowanie środowiska uruchomieniowego EF 6,2 podczas tworzenia modelu w nowym projekcie. W starszych wersjach programu Visual Studio można używać środowiska uruchomieniowego EF 6,2 (a także dowolnej starszej wersji EF) przez zainstalowanie odpowiedniej wersji pakietu NuGet.

## <a name="ef-620"></a>DR 6.2.0
Środowisko uruchomieniowe EF 6,2 zostało wydane do NuGet w październiku z 2017.
Dziękujemy za dążenie do wysiłków naszej społeczności uczestników oprogramowania typu open source, EF 6,2 zawiera wiele [poprawek błędów](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) i [ulepszeń produktów](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Poniżej przedstawiono krótką listę najważniejszych zmian wpływających na środowisko uruchomieniowe EF 6,2:

- Skrócenie czasu uruchamiania przez załadowanie pierwszego modelu kodu z trwałej pamięci podręcznej [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Interfejs API Fluent do definiowania indeksów [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- Dbfunctions. like () w celu umożliwienia pisania zapytań LINQ, które są tłumaczone na takie jak w programie SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Program migruje. exe obsługuje teraz- [#240](https://github.com/aspnet/EntityFramework6/issues/240) opcji skryptu
- EF6 może teraz korzystać z wartości kluczy generowanych przez sekwencję w SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aktualizowanie listy błędów przejściowych dla strategii wykonywania SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Usterka: Ponowna próba wykonania zapytań lub poleceń SQL kończy się niepowodzeniem z błędem "SqlParameter jest już zawarty w innym SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Usterka: Obliczanie często przeszukiwanych DBQuery. ToString () w debugerze [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="ef-613"></a>EF 6.1.3
Środowisko uruchomieniowe programu EF 6.1.3 zostało wydane w ramach programu NuGet w październiku 2015.
Ta wersja zawiera tylko poprawki do wad o wysokim priorytecie i regresje raportowane w wersji 6.1.2.
Poprawki obejmują:

- Zapytanie: regresja w EF 6.1.2: wprowadzono zewnętrzne zastosowania i bardziej złożone zapytania dla relacji 1:1 i klauzula "let"
- Wystąpił problem TPT podczas ukrywania właściwości klasy bazowej w klasie dziedziczonej
- Proces migracji bazy danych SQL kończy się niepowodzeniem, gdy słowo "go" jest zawarte w tekście
- Utwórz flagę zgodności dla obsługi spłaszczania UnionAll i przecinania
- Zapytanie z wieloma dołączami nie działa w 6.1.2 (praca w 6.1.1)
- "Wystąpił błąd w składni SQL" po uaktualnieniu z EF 6.1.1 do 6.1.2

## <a name="ef-612"></a>DR 6.1.2
Środowisko uruchomieniowe EF 6.1.2 zostało wydane w programie NuGet w grudniu 2014.
Ta wersja dotyczy głównie poprawek błędów. Zaakceptujemy również kilka nieprawidłowych zmian od członków społeczności:
- **Parametry pamięci podręcznej zapytań można skonfigurować z poziomu pliku App/Web. Configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Metody SQLFile i Sqlresources w usłudze dbmigration** umożliwiają uruchomienie skryptu SQL przechowywanego jako plik lub osadzony zasób.

## <a name="ef-611"></a>EF 6.1.1
Środowisko uruchomieniowe EF 6.1.1 zostało wydane w programie NuGet w czerwcu 2014.
Ta wersja zawiera poprawki dotyczące problemów napotkanych przez liczbę osób. Między innymi:
- Projektant: Wystąpił błąd podczas otwierania EF5 EDMX z dokładnością dziesiętną w projektancie EF6
- Logika wykrywania wystąpień domyślnych dla LocalDB nie działa z SQL Server 2014

## <a name="ef-610"></a>DR 6.1.0
Środowisko uruchomieniowe EF 6.1.0 zostało wydane jako pakiet NuGet w marcu 2014.
Ta aktualizacja pomocnicza obejmuje znaczną liczbę nowych funkcji:

- **Konsolidacja narzędzi** zapewnia spójny sposób tworzenia nowego modelu EF. Ta funkcja [rozszerza kreatora Entity Data Model ADO.NET, aby obsługiwał tworzenie modeli Code First](~/ef6/modeling/code-first/workflows/existing-database.md), w tym odtwarzanie z istniejącej bazy danych. Te funkcje były wcześniej dostępne w jakości beta w narzędziach EF.
- **[Obsługa niepowodzeń zatwierdzania transakcji](~/ef6/fundamentals/connection-resiliency/commit-failures.md)** zapewnia CommitFailureHandler, który korzysta z nowo wprowadzonej zdolności do przechwytywania operacji transakcji. CommitFailureHandler umożliwia automatyczne odzyskiwanie z błędów połączeń podczas zatwierdzania transakcji.
- **[Indexattribute](~/ef6/modeling/code-first/data-annotations.md)** umożliwia określenie indeksów przez umieszczenie atrybutu `[Index]` we właściwości (lub właściwościach) w modelu Code First. Code First następnie utworzy odpowiedni indeks w bazie danych.
- **Interfejs API mapowania publicznego** zapewnia dostęp do informacji EF na temat tego, jak właściwości i typy są mapowane na kolumny i tabele w bazie danych. W poprzednich wersjach ten interfejs API był wewnętrzny.
- **[Możliwość konfigurowania interceptorów za pośrednictwem pliku App/Web. config](~/ef6/fundamentals/configuring/config-file.md)** pozwala na dodawanie interceptorów bez konieczności ponownego kompilowania aplikacji.
- **System. Data. Entity. Infrastructure. przechwytując. DatabaseLogger**jest nowym interceptorem, dzięki czemu można łatwo rejestrować wszystkie operacje bazy danych do pliku. W połączeniu z poprzednią funkcją umożliwia to łatwe [przełączenie na rejestrowanie operacji bazy danych dla wdrożonej aplikacji](~/ef6/fundamentals/configuring/config-file.md)bez konieczności ponownego kompilowania.
- Udoskonalono **wykrywanie zmian modelu migracji** , dzięki czemu migracja szkieletowa jest bardziej dokładna. Ulepszono również wydajność procesu wykrywania zmian.
- **Ulepszenia wydajności** , w tym mniejsze operacje bazy danych podczas inicjowania, optymalizacje dla porównania równości o wartości null w zapytaniach LINQ, szybsze generowanie widoku (Tworzenie modelu) w większej liczbie scenariuszy i wydajniejsze materializację śledzone jednostki z wieloma skojarzeniami.

## <a name="ef-602"></a>DR 6.0.2
Środowisko uruchomieniowe EF 6.0.2 zostało wydane w programie NuGet w grudniu 2013.
Ta wersja poprawki jest ograniczona do rozwiązywania problemów, które zostały wprowadzone w wersji EF6 (regresje wydajności/zachowanie od EF5).

## <a name="ef-601"></a>DR 6.0.1
Środowisko uruchomieniowe EF 6.0.1 zostało wydane do programu NuGet w październiku 2013 równocześnie z dodatkiem Dr 6.0.0, ponieważ drugie zostało osadzone w wersji programu Visual Studio, która została wcześniej zablokowana.
Ta wersja poprawki jest ograniczona do rozwiązywania problemów, które zostały wprowadzone w wersji EF6 (regresje wydajności/zachowanie od EF5).
Najbardziej znaczące zmiany umożliwiły rozwiązanie niektórych problemów z wydajnością podczas rozgrzewania modeli EF.
Jest to ważne, ponieważ wydajność rozgrzewania była koncentracją w EF6, a te problemy były negacją niektórych innych zysków z wydajności w EF6.

## <a name="ef-60"></a>EF 6,0
Środowisko uruchomieniowe EF 6.0.0 zostało wydane w ramach programu NuGet w październiku 2013.
Jest to pierwsza wersja, w której kompletne środowisko uruchomieniowe EF jest zawarte w [pakiecie NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/) , który nie zależy od bitów EF, które są częścią .NET Framework.
Przeniesienie pozostałych części środowiska uruchomieniowego do pakietu NuGet wymagało pewnej istotnej zmiany w istniejącym kodzie.
Zobacz sekcję dotyczącą [uaktualniania do Entity Framework 6](upgrading-to-ef6.md) , aby uzyskać szczegółowe informacje na temat czynności ręcznych wymaganych do uaktualnienia.

Ta wersja zawiera wiele nowych funkcji.
Następujące funkcje działają dla modeli utworzonych za pomocą Code First lub programu EF Designer:

- **[Zapytanie asynchroniczne i zapisywanie](~/ef6/fundamentals/async.md)** dodaje obsługę wzorców asynchronicznych opartych na zadaniach, które zostały wprowadzone w programie .NET 4,5.
- **[Odporność połączenia](~/ef6/fundamentals/connection-resiliency/retry-logic.md)** włącza automatyczne odzyskiwanie z niepowodzeń połączeń przejściowych.
- **[Konfiguracja oparta na kodzie](~/ef6/fundamentals/configuring/code-based.md)** umożliwia wykonywanie czynności konfiguracyjnych, które były tradycyjnie wykonywane w pliku konfiguracji — w kodzie.
- W przypadku **[rozpoznawania zależności](~/ef6/fundamentals/configuring/dependency-resolution.md)** wprowadzono obsługę wzorca lokalizatora usługi, a firma Microsoft oferuje pewne funkcje, które można zastąpić implementacjami niestandardowymi.
- **[Przechwycenie/rejestrowanie SQL](~/ef6/fundamentals/logging-and-interception.md)** udostępnia bloki konstrukcyjne niskiego poziomu do przechwycenia operacji EF z prostym rejestrowaniem SQL utworzonym na górze.
- **Udoskonalenia** dotyczące możliwości testowania ułatwiają tworzenie podwójnej precyzji dla DbContext i nieogólnymi w przypadku [korzystania z struktury imitacji](~/ef6/fundamentals/testing/mocking.md) lub napisania wieloznacznego [przeprowadzenia testu](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext można teraz utworzyć przy użyciu DbConnection, który jest już otwarty](~/ef6/fundamentals/connection-management.md)** , co umożliwia scenariusze, które mogą być przydatne, jeśli połączenie może być otwarte podczas tworzenia kontekstu (na przykład udostępniania połączenia między składnikami, w których nie można zagwarantować stan połączenia).
- **[Ulepszona obsługa transakcji](~/ef6/saving/transactions.md)** zapewnia obsługę transakcji poza platformą, a także ulepszone sposoby tworzenia transakcji w ramach struktury.
- **Wyliczenia, przestrzenne i lepsza wydajność na platformie .net 4,0** — przez przeniesienie podstawowych składników, które były w .NET Framework do pakietu NuGet EF, możemy teraz oferować pomoc techniczną dla wyliczenia, typy danych przestrzennych oraz ulepszenia wydajności EF5 na platformie .NET 4,0.
- **Zwiększona wydajność wyliczalnych elementów. zawiera w zapytaniach LINQ**.
- **Ulepszony czas rozgrzewania (generowanie widoku)** , szczególnie w przypadku dużych modeli.
- **Usługa pluralizacja &amp; Singularization**.
- **Niestandardowe implementacje elementu Equals lub GetHashCode** w klasach jednostek są teraz obsługiwane.
- **Nieogólnymi. AddRange/RemoveRange** zapewnia zoptymalizowany sposób dodawania lub usuwania wielu jednostek z zestawu.
- **DbChangeTracker. HasChanges** zapewnia łatwy i wydajny sposób, aby sprawdzić, czy istnieją oczekujące zmiany, które mają być zapisane w bazie danych.
- **SqlCeFunctions** zapewnia programowi SQL Compact odpowiednik funkcji SqlFunctions.

Następujące funkcje mają zastosowanie tylko do Code First:

- **[Niestandardowe konwencje Code First](~/ef6/modeling/code-first/conventions/custom.md)** pozwalają pisać własne konwencje w celu uniknięcia powtarzania konfiguracji. Udostępniamy prosty interfejs API dla uproszczonych Konwencji oraz bardziej złożone bloki konstrukcyjne, aby umożliwić tworzenie bardziej złożonych Konwencji.
- Obsługiwane jest teraz **[mapowanie Code First, aby wstawiać/aktualizować/usuwać procedury składowane](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)** .
- **[Skrypty migracji idempotentne](~/ef6/modeling/code-first/migrations/index.md)** umożliwiają Generowanie skryptu SQL, który może uaktualnić bazę danych w dowolnej wersji do najnowszej wersji.
- **[Tabela historii konfigurowalnych migracji](~/ef6/modeling/code-first/migrations/history-customization.md)** umożliwia dostosowanie definicji tabeli historii migracji. Jest to szczególnie przydatne w przypadku dostawców baz danych, którzy wymagają odpowiednich typów danych itp., aby można było je określić w celu poprawnego działania tabeli historii migracji.
- W **wielu kontekstach na bazę danych** program usuwa poprzednie ograniczenie jednego Code First modelu na bazę danych podczas korzystania z migracji lub podczas Code First automatycznie utworzyć bazę danych.
- **[DbModelBuilder. HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)** jest nowym interfejsem API Code First, który umożliwia skonfigurowanie domyślnego schematu bazy danych dla modelu Code First w jednym miejscu. Wcześniej Code First schemat domyślny został zakodowany do &quot;dbo&quot; i jedynym sposobem skonfigurowania schematu, do którego należy tabela, był za pośrednictwem interfejsu API ToTable.
- **DbModelBuilder. Configurations. AddFromAssembly Metoda** umożliwia łatwe dodawanie wszystkich klas konfiguracyjnych zdefiniowanych w zestawie podczas korzystania z klas konfiguracji z interfejsem API Fluent Code First.
- **[Niestandardowe operacje migracji](https://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)** umożliwiają dodanie dodatkowych operacji do użycia w migracjach opartych na kodzie.
- **Domyślny poziom izolacji transakcji został zmieniony na READ_COMMITTED_SNAPSHOT** dla baz danych utworzonych przy użyciu Code First, co pozwala na większą skalowalność i mniejszą liczbę zakleszczenii.
- **Jednostki i typy złożone mogą teraz być klasami nestedinside**.

## <a name="ef-50"></a>EF 5,0
Środowisko uruchomieniowe EF 5.0.0 zostało wydane jako pakiet NuGet w sierpniu 2012.
W tej wersji wprowadzono nowe funkcje, takie jak obsługa wyliczenia, funkcje z wartościami przechowywanymi w tabeli, typy danych przestrzennych i różne ulepszenia wydajności.

Entity Framework Designer w programie Visual Studio 2012 wprowadza również obsługę wielu diagramów na model, kolorowanie kształtów na powierzchni projektowej i importowanie wsadowe procedur składowanych.

Poniżej znajduje się lista zawartości, która została przedstawiona w odniesieniu do wersji EF 5:

-   [Wpis wydania Dr 5](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nowe funkcje w programie EF5
    -   [Obsługa wyliczenia w Code First](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Obsługa tekstów stałych w programie EF Designer](~/ef6/modeling/designer/data-types/enums.md)
    -   [Typy danych przestrzennych w Code First](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Typy danych przestrzennych w projektancie EF](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Obsługa dostawcy dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funkcje zwracające tabelę](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Wiele diagramów na model](~/ef6/modeling/designer/multiple-diagrams.md)
-   Konfigurowanie modelu
    -   [Tworzenie modelu](~/ef6/modeling/index.md)
    -   [Połączenia i modele](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Zagadnienia dotyczące wydajności](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Praca z programem Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Ustawienia pliku konfiguracji](~/ef6/fundamentals/configuring/config-file.md)
    -   [Słownik](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First do nowej bazy danych (Przewodnik i film wideo)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First do istniejącej bazy danych (Przewodnik i film wideo)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Konwencje](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Adnotacje danych](~/ef6/modeling/code-first/data-annotations.md)
        -   [Interfejs API Fluent — Konfigurowanie/mapowanie właściwości & typów](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Interfejs API Fluent — Konfigurowanie relacji](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [Interfejs API Fluent z VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migracje Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Automatyczne Migracje Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migruj plik exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definiowanie dbsets](~/ef6/modeling/code-first/dbsets.md)
    -   Projektant EF
        -   [Model First (Przewodnik i wideo)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (Przewodnik i wideo)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Typy złożone](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Skojarzenia/relacje](~/ef6/modeling/designer/relationships.md)
        -   [Wzorzec dziedziczenia TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Wzorzec dziedziczenia TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Zapytanie z procedurami składowanymi](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Procedury składowane z wieloma zestawami wyników](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Wstawianie, aktualizowanie & usuwania przy użyciu procedur składowanych](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapowanie jednostki na wiele tabel (podział jednostki)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapowanie wielu jednostek na jedną tabelę (podział tabeli)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definiowanie zapytań](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Szablony generowania kodu](~/ef6/modeling/designer/codegen/index.md)
        -   [Przywracanie do obiektu ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Korzystanie z modelu
    -   [Praca z klasą DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Wykonywanie zapytań/Znajdowanie jednostek](~/ef6/querying/index.md)
    -   [Praca z relacjami](~/ef6/fundamentals/relationships.md)
    -   [Ładowanie powiązanych jednostek](~/ef6/querying/related-data.md)
    -   [Praca z danymi lokalnymi](~/ef6/querying/local-data.md)
    -   [Aplikacje N-warstwowe](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Pierwotne zapytania SQL](~/ef6/querying/raw-sql.md)
    -   [Optymistyczne wzorce współbieżności](~/ef6/saving/concurrency.md)
    -   [Praca z serwerami proxy](~/ef6/fundamentals/proxies.md)
    -   [Automatyczne wykrywanie zmian](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Zapytania bez śledzenia](~/ef6/querying/no-tracking.md)
    -   [Metoda Load](~/ef6/querying/load-method.md)
    -   [Dodaj/Dołącz i Stany jednostek](~/ef6/saving/change-tracking/entity-state.md)
    -   [Praca z wartościami właściwości](~/ef6/saving/change-tracking/property-values.md)
    -   [Powiązanie danych z WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Powiązanie danych z WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
Środowisko uruchomieniowe EF 4.3.1 zostało wydane w programie NuGet w lutym 2012 krótko po EF 4.3.0.
W tej wersji poprawki uwzględniono niektóre poprawki błędów w wersji EF 4,3 i wprowadzono lepszą obsługę LocalDB dla klientów korzystających z programu EF 4,3 z programem Visual Studio 2012.

Poniżej znajduje się lista zawartości, która została umieszczona w szczególności dla wydania EF 4.3.1, większość zawartości dostarczonej dla EF 4,1 nadal dotyczy również EF 4,3:

-   [Wpis w blogu dotyczący wydania EF 4.3.1](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4,3
Środowisko uruchomieniowe EF 4.3.0 zostało wydane w programie NuGet w lutym 2012.
W tej wersji uwzględniono nową funkcję Migracje Code First, która umożliwia przyrostową zmianę bazy danych utworzonej przez Code First w miarę rozwoju modelu Code First.

Poniżej znajduje się lista zawartości, która została umieszczona w odniesieniu do wersji EF 4,3, większość zawartości dostarczonej do EF 4,1 nadal dotyczy również EF 4,3:
-   [Dr 4,3 wydanie wydania](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Przewodnik po migracji na podstawie kodu Dr 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Przewodnik po automatycznej migracji Dr 4,3](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4,2
Środowisko uruchomieniowe EF 4.2.0 zostało wydane w usłudze NuGet w listopadzie 2011.
Ta wersja zawiera poprawki błędów do wersji EF 4.1.1.
Ponieważ w tej wersji uwzględniono tylko poprawki błędów, mogło to być wersja EF 4.1.2, ale przeszedłmy do 4,2, aby umożliwić Przechodzenie przez numery wersji poprawek opartych na dacie, które zostały użyte w wersjach 4.1. x i zastosować standardową [wersję semantyczną](https://semver.org) dla s przechowywanie wersji emantic.

Poniżej znajduje się lista zawartości, która została umieszczona w odniesieniu do wersji EF 4,2, zawartość udostępniona dla EF 4,1 nadal dotyczy również EF 4,2:

-   [Dr 4,2 wydanie wydania](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Przewodnik Code First](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Przewodnik po Database First & modelu](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
Środowisko uruchomieniowe EF 4.1.10715 zostało wydane w programie NuGet w lipcu 2011.
Oprócz błędów poprawki w tej wersji wprowadzono pewne składniki, które ułatwiają pracę z modelem Code First w czasie projektowania.
Te składniki są używane przez Migracje Code First (zawarte w EF 4,3) i dr energia Tools.

Zwróć uwagę na to, że numer wersji 4.1.10715 pakietu jest niewidoczny.
Używamy wersji poprawek opartych na dacie przed podjęciem decyzji o przyjęciu [wersji semantycznej](https://semver.org).
Ta wersja jest uważana za wersję EF 4,1 poprawka 1 (lub dr 4.1.1).

Poniżej znajduje się lista zawartości, która została umieszczona w wersji 4.1.1:

-   [Dr 4.1.1 Release post](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4,1
Środowisko uruchomieniowe EF 4.1.10331 było pierwsze do opublikowania w usłudze NuGet, w kwietniu 2011.
W tej wersji uwzględniono uproszczony interfejs API DbContext i przepływ pracy Code First.

Zobaczysz nie4.1.10331ny numer wersji, który powinien być naprawdę 4,1. Ponadto istnieje wersja 4.1.10311, która powinna mieć 4.1.0-RC ("RC" oznacza "Release Candidate").
Używamy wersji poprawek opartych na dacie przed podjęciem decyzji o przyjęciu [wersji semantycznej](https://semver.org).

Poniżej znajduje się lista zawartości zebranych w wersji 4,1. Większość nadal stosuje się do nowszych wersji Entity Framework:

-   [Dr 4,1 wydanie wydania](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Przewodnik Code First](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Przewodnik po Database First & modelu](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [Federacyjne usługi SQL Azure i Entity Framework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4,0
Ta wersja została uwzględniona w .NET Framework 4 i Visual Studio 2010 w kwietniu 2010.
Ważne nowe funkcje w tej wersji obejmują obsługę POCO, mapowanie kluczy obcych, ładowanie z opóźnieniem, ulepszenia dotyczące możliwości testowania, dostosowywalne generowanie kodu i przepływ pracy Model First.

Mimo że była to druga wersja Entity Framework, jej nazwa to EF 4, aby dostosować ją do wersji .NET Framework, która została dostarczona z.
Po tej wersji rozpoczęto udostępnianie Entity Framework w NuGet i zaakceptowanie wersji semantycznej, ponieważ nie są one już powiązane z wersją .NET Framework.

Należy zauważyć, że niektóre kolejne wersje .NET Framework były dostarczane z ważnymi aktualizacjami uwzględnionych bitów EF.
W rzeczywistości wiele nowych funkcji EF 5,0 zostało wdrożonych jako ulepszenia tych bitów.
Jednak w celu racjonalizacji historii przechowywania wersji EF będziemy nadal odwoływać się do protokołu EF, który jest częścią .NET Framework jako środowiska uruchomieniowego EF 4,0, a wszystkie nowsze wersje składają się z [pakietu NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).

## <a name="ef-35"></a>EF 3,5
Początkowa wersja Entity Framework została uwzględniona w oprogramowaniu .NET 3,5 z dodatkiem Service Pack 1 i Visual Studio 2008 SP1 wydanej w sierpniu z 2008.
W tej wersji udostępniono podstawową obsługę usługi O/RM przy użyciu przepływu pracy Database First.

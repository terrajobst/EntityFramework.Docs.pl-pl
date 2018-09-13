---
title: Przeszłymi wersjami programu Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
ms.openlocfilehash: 4c711bb48938e5c0432881c61766b0bff66498f2
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490147"
---
# <a name="past-releases-of-entity-framework"></a>Przeszłymi wersjami programu Entity Framework

Pierwsza wersja programu Entity Framework został wydany w 2008 roku jako część .NET Framework 3.5 SP1 i Visual Studio 2008 z dodatkiem SP1.

Począwszy od wersji EF4.1 jest dostarczana jako [pakiet NuGet platformy EntityFramework](https://www.nuget.org/packages/EntityFramework/) — obecnie jedną z najbardziej popularnych pakietów w witrynie NuGet.org.

Od wersji 4.1 i 5.0 pakiet NuGet platformy EntityFramework rozszerzone biblioteki EF, z którymi dostarczana jako część programu .NET Framework.   

Począwszy od wersji 6 EF stało się to projekt open source i również przeniesiona całkowicie poza pasmem formularz programu .NET Framework.
Oznacza to, że po dodaniu pakietu NuGet w wersji 6 EntityFramework do aplikacji, otrzymujesz pełną kopię biblioteki EF, która nie zależy od usługi bits EF, które są dostarczane jako część .NET Framework.
Pomogła nieco przyspieszyć tempie rozwoju i dostarczania nowych funkcji.

W czerwcu 2016 roku opublikowaliśmy EF Core 1.0. EF Core opiera się na nowe bazy kodu i został zaprojektowany jako bardziej uproszczone i rozszerzalny wersja programu EF.
Obecnie programem EF Core jest głównym celem tworzenia zespołu Entity Framework w firmie Microsoft.
Oznacza to są planowane żadne nowe główne funkcje dla platformy EF6. Jednak EF6 nadal jest przechowywane jako projekt open source i obsługiwanych produktów firmy Microsoft.

Poniżej przedstawiono listę poprzednich wersjach, w odwrotnej kolejności chronologicznej z informacjami na temat nowych funkcji, które zostały wprowadzone w każdej wersji.

## <a name="ef-613"></a>EF 6.1.3
EF 6.1.3 środowiska wykonawczego został wydany do narzędzia NuGet w października 2015 r.
Ta wersja zawiera tylko poprawki usterek o wysokim priorytecie, regresji zgłoszone 6.1.2 wydania.
Poprawki obejmują:

- Zapytanie: Regresję w programie EF 6.1.2: operatora OUTER APPLY wprowadzone i bardziej złożone zapytania dla relacji 1:1 i klauzuli "let"
- Ukrywanie właściwości klasy podstawowej w klasie dziedziczonej TPT problem
- DbMigration.Sql zakończy się niepowodzeniem, gdy wyraz "Przejdź" znajduje się w tekście
- Utwórz flagi zgodności UnionAll i Intersect Obsługa struktur płaskich
- Zapytanie o obejmuje wiele nie działa w 6.1.2 (Praca w 6.1.1)
- "Masz błąd w składni SQL" wyjątków po uaktualnieniu z programu EF 6.1.1 i 6.1.2

## <a name="ef-612"></a>EF 6.1.2
EF 6.1.2 środowiska uruchomieniowego wydanej z grudnia 2014 r. do narzędzia NuGet.
Ta wersja dotyczy przede wszystkim poprawki błędów. Możemy także akceptowane kilku warte zauważenia zmian z członkami społeczności:
- **Parametry pamięci podręcznej zapytania można konfigurować na podstawie pliku app/web.configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Metody SqlFile i SqlResource na DbMigration** umożliwiają uruchamianie SQL skrypt zapisane w formie pliku lub osadzonego zasobu.

## <a name="ef-611"></a>EF 6.1.1
EF 6.1.1 środowiska uruchomieniowego wydanej w czerwcu 2014 do narzędzia NuGet.
Ta wersja zawiera poprawki dotyczące problemów, których wystąpiło wiele osób. Między innymi:
- Projektant: Błąd podczas otwierania EF5 edmx z precyzja dziesiętna w Projektancie EF6
- Domyślne wystąpienie logikę wykrywania LocalDB nie działa w przypadku programu SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 środowiska uruchomieniowego wydanej w marcu 2014 do narzędzia NuGet.
Ta pomocnicza aktualizacja obejmuje szereg istotnych nowe funkcje:

- **Narzędzia konsolidacji** zapewnia spójny sposób do utworzenia nowego modelu platformy EF. Ta funkcja [rozszerza kreatora ADO.NET Entity Data Model, aby obsługiwać tworzenie modele Code First](~/ef6/modeling/code-first/workflows/existing-database.md), w tym odtwarzanie z istniejącej bazy danych. Funkcje te wcześniej były dostępne w wersji Beta jakość EF Power Tools.
- **[Obsługa błędów zatwierdzania transakcji](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  zapewnia CommitFailureHandler, co sprawia, że korzystanie z możliwości nowo wprowadzonych w celu przechwycenia operacji transakcji. CommitFailureHandler umożliwia automatyczne odzyskiwanie po awarii połączenia, przy jednoczesnym Zatwierdzanie transakcji.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  umożliwia indeksy, aby określić, umieszczając `[Index]` atrybutu na właściwości (lub właściwości) w modelu Code First. Kod najpierw następnie utworzy odpowiedni indeks w bazie danych.
- **Mapowania publicznego interfejsu API** zapewnia dostęp do informacji EF ma na sposobie mapowania właściwości i typy kolumn i tabel w bazie danych. W poprzednich wersjach tego interfejsu API były wewnętrznego.
- **[Możliwość konfigurowania interceptory za pomocą pliku App/Web.config](~/ef6/fundamentals/configuring/config-file.md)**  umożliwia interceptory do dodania bez konieczności ponownego kompilowania aplikacji.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**jest nowy interceptor, która ułatwia wszystkie operacje bazy danych w pliku dziennika. W połączeniu z poprzedniej funkcji, dzięki temu można łatwo [włączyć rejestrowanie operacji bazy danych dla wdrożonej aplikacji](~/ef6/fundamentals/configuring/config-file.md), bez konieczności ponownej kompilacji.
- **Wykrywanie zmiany modelu migracje** został ulepszony, aby były szkieletu migracje bardziej precyzyjne; także udoskonalono wydajności procesu wykrywania zmian.
- **Ulepszenia wydajności** w tym operacje niższych bazy danych podczas inicjowania, optymalizacje dla porównania równości wartości null w zapytaniach LINQ szybciej wyświetlać generowania (Tworzenie modelu) w dalszych scenariuszach i bardziej wydajne materializacja śledzonych jednostek z wielu skojarzeń.

## <a name="ef-602"></a>EF 6.0.2
EF 6.0.2 środowiska wykonawczego został wydany do narzędzia NuGet w grudnia 2013 r.
Ta wersja poprawki nie jest ograniczona do rozwiązywania problemów, które zostały wprowadzone w wersji EF6 (regresji wydajności/zachowanie od EF5).

## <a name="ef-601"></a>EF 6.0.1
EF 6.0.1 środowiska wykonawczego został wydany do narzędzia NuGet w październiku 2013 razem z programem EF 6.0.0, ponieważ zostały one osadzone w wersjach programu Visual Studio, która była zablokowana kilka miesięcy przed.
Ta wersja poprawki nie jest ograniczona do rozwiązywania problemów, które zostały wprowadzone w wersji EF6 (regresji wydajności/zachowanie od EF5).
Najbardziej znaczące zmiany zostały wprowadzone rozwiązać pewne problemy z wydajnością podczas rozgrzewania dla modeli EF.
Jest to ważne w trakcie rozgrzewania wydajności został obszar koncentracji uwagi w EF6 i te problemy zostały Negacja niektóre inne wzrost wydajności w EF6.

## <a name="ef-60"></a>EF 6.0
EF 6.0.0 środowiska wykonawczego został wydany do narzędzia NuGet w październiku 2013.
Jest to pierwsza wersja jest uwzględniana pełną środowiska uruchomieniowego EF, w [pakiet NuGet platformy EntityFramework](https://www.nuget.org/packages/EntityFramework/) która nie zależy od bitów EF, które są częścią programu .NET Framework.
Przenoszenie pozostałe części środowiska uruchomieniowego do pakietu NuGet wymagana liczba przełomowe zmiany dla istniejącego kodu.
Zobacz sekcję dotyczącą [uaktualnianie do programu Entity Framework 6](upgrading-to-ef6.md) więcej informacji na temat ręcznego kroki wymagane do uaktualnienia.

Ta wersja zawiera wiele nowych funkcji.
Następujące funkcje działają w przypadku modeli utworzonych za pomocą Code First i projektancie platformy EF:

- **[Zapytania asynchroniczne i Zapisz](~/ef6/fundamentals/async.md)**  alokowanej dla wzorca asynchronicznego opartego na zadaniach, które zostały wprowadzone w .NET 4.5.
- **[Elastyczność połączenia](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  umożliwia automatyczne odzyskiwanie po awarii przejściowych połączenia.
- **[Konfiguracja na podstawie kodu](~/ef6/fundamentals/configuring/code-based.md)**  zapewnia możliwość wykonywania konfiguracji — tradycyjnie zostało wykonane w pliku konfiguracji — w kodzie.
- **[Rozpoznawanie zależności](~/ef6/fundamentals/configuring/dependency-resolution.md)**  wprowadza obsługę wspólnym lokalizatorze usług wzorzec i firma Microsoft została uwzględniona się niektóre elementy funkcji, którą można zastąpić za pomocą niestandardowych implementacji.
- **[Rejestrowanie przejmowanie/SQL](~/ef6/fundamentals/logging-and-interception.md)**  zapewnia niskiego poziomu bloków konstrukcyjnych przejmowanie EF operacji za pomocą prostego rejestrowania SQL zbudowany na górze.
- **Ulepszenia testowania** ułatwiają tworzenie testu wartości podwójnej precyzji dla typu DbContext i DbSet podczas [za pomocą platformy pozorowania](~/ef6/fundamentals/testing/mocking.md) lub [napisanie własnego testu wartości podwójnej precyzji](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[Kontekst DbContext można teraz tworzyć przy użyciu DbConnection, który jest już otwarty](~/ef6/fundamentals/connection-management.md)**  umożliwiająca scenariuszach, gdzie byłaby pomocne może być otwarte połączenie, podczas tworzenia kontekstu (takie jak udostępnianie połączenia między składnikami gdzie Użytkownik może nie gwarantuje stan połączenia).
- **[Ulepszona obsługa transakcji](~/ef6/saving/transactions.md)**  zapewnia obsługę zewnętrznej framework, jak również ulepszoną sposoby tworzenia transakcji w ramach transakcji.
- **Typy wyliczeniowe, przestrzenne i lepszą wydajność, program .NET 4.0** —, przenosząc podstawowe składniki, które wcześniej w programie .NET Framework do pakietu NuGet programu EF, możemy teraz oferować pomoc techniczną wyliczenia, typów danych przestrzennych i ulepszenia wydajności z EF5 na platformie .NET 4.0.
- **Zwiększono wydajność Enumerable.Contains w zapytaniach LINQ**.
- **Ulepszone ciepło czasu (Generowanie widoku)**, szczególnie w przypadku dużych modeli.
- **Pluralizacja podłączanych &amp; usługi Singularization**.
- **Niestandardowe implementacje Equals i GetHashCode** w jednostce klasy są teraz obsługiwane.
- **DbSet.AddRange/RemoveRange** zapewnia sposób zoptymalizowany, aby dodać lub usunąć wiele jednostek z zestawu.
- **DbChangeTracker.HasChanges** zapewnia prosty i skuteczny sposób sprawdzić, czy są oczekujących zmian do zapisania w bazie danych.
- **SqlCeFunctions** zapewnia SQL Compact odpowiednikiem SqlFunctions.

Następujące funkcje Zastosuj tylko do Code First:

- **[Niestandardowe pierwszy konwencje związane z](~/ef6/modeling/code-first/conventions/custom.md)**  Zezwalaj pisania własnych Konwencji odpowiadającym, aby zapobiec powstawaniu powtarzających się konfiguracji. Firma Microsoft udostępnia prosty interfejs API konwencje uproszczone, a także niektórych bardziej złożonych bloków konstrukcyjnych umożliwia tworzenie bardziej skomplikowanych Konwencji.
- **[Kod pierwszy mapowanie procedur składowanych wstawiania/aktualizowania/usuwania](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  jest teraz obsługiwane.
- **[Skrypty migracji Idempotentne](~/ef6/modeling/code-first/migrations/index.md)**  umożliwiają generowanie skryptu SQL, które można uaktualnić bazę danych w dowolnej wersji do najnowszej wersji.
- **[Tabela migracji można skonfigurować w historii](~/ef6/modeling/code-first/migrations/history-customization.md)**  umożliwia dostosowanie definicji tabeli historii migracji. Jest to szczególnie przydatne dla dostawcy bazy danych wymagają odpowiedni typ danych itp. może być określony dla tabeli migracji historii, aby działać poprawnie.
- **Wiele kontekstów na bazę danych** usunie poprzednie ograniczenie jeden model Code First na bazę danych, korzystając z migracji lub Code First automatycznie utworzyła bazę danych dla Ciebie.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  jest nowy kod pierwszy interfejs API umożliwiający domyślny schemat bazy danych dla modelu Code First, należy skonfigurować w jednym miejscu. Wcześniej Code First schemat domyślny: stałe zakodowany &quot;dbo&quot; i jedynym sposobem, aby skonfigurował schemat, do której będzie należał tabeli za pomocą ToTable interfejsu API.
- **Metoda DbModelBuilder.Configurations.AddFromAssembly** pozwala łatwo dodać wszystkich klas konfiguracji zdefiniowany w zestawie, w przypadku korzystania z klas konfiguracji przy użyciu kodu pierwszy Fluent interfejsu API.
- **[Operacje migracji niestandardowe](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  umożliwia dodawanie dodatkowych operacji do użycia w migracji opartego na kodzie.
- **Domyślny poziom izolacji transakcji jest zmieniana na READ_COMMITTED_SNAPSHOT** dla baz danych utworzonych przy użyciu Code First, co zapewnia lepszą skalowalność i mniej zakleszczenia.
- **Jednostek i typów złożonych można teraz klasy nestedinside**. |

## <a name="ef-50"></a>EF 5.0
EF 5.0.0 środowiska uruchomieniowego wydanej w sierpniu 2012 do narzędzia NuGet.
W tej wersji wprowadzono kilka nowych funkcji, w tym obsługa wyliczenia, zwracających tabelę, typów danych przestrzennych i różne ulepszenia wydajności.

Entity Framework Designer w programie Visual Studio 2012 wprowadza również obsługę wielu — diagramy, na model, Kolorowanie kształtów na projekt powierzchni i batch importu procedur składowanych.

Oto lista zawartości, które razem utworzona Państwu specjalnie dla wersji programów EF 5.

-   [Wpis w wersji 5 EF](http://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nowe funkcje w EF5
    -   [Wyliczenie obsługi w kodzie najpierw](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Obsługa typu wyliczeniowego w Projektancie platformy EF](~/ef6/modeling/designer/data-types/enums.md)
    -   [Typy w kodzie najpierw danych przestrzennych](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Typy danych przestrzennych w Projektancie platformy EF](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Obsługa dostawców dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funkcje zwracające tabelę](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Wiele diagramów na modelu](~/ef6/modeling/designer/multiple-diagrams.md)
-   Konfigurowanie modelu
    -   [Tworzenie modelu](~/ef6/modeling/index.md)
    -   [Połączenia i modeli](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Zagadnienia dotyczące wydajności](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Praca z programem Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Ustawienia pliku konfiguracji](~/ef6/fundamentals/configuring/config-file.md)
    -   [Słownik](~/ef6/resources/glossary.md)
    -   Najpierw kod
        -   [Najpierw kod do nowej bazy danych (wskazówki i wideo)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Najpierw kod istniejącej bazy danych (wskazówki i wideo)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Konwencje](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Adnotacje danych](~/ef6/modeling/code-first/data-annotations.md)
        -   [Interfejs API Fluent — Konfigurowanie/mapowania typów & właściwości](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Interfejs API Fluent — Konfigurowanie relacji](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [Interfejs Fluent API przy użyciu VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migracje Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Migracje automatyczne Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definiowanie DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   Projektancie platformy EF
        -   [Pierwszy model (wskazówki i wideo)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Baza danych pierwszy (wskazówki i wideo)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Typy złożone](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Skojarzenia/relacji](~/ef6/modeling/designer/relationships.md)
        -   [Wzorzec dziedziczenia TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Wzorzec dziedziczenia TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Zapytanie za pomocą procedur składowanych](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Procedury składowane z wielu zestawów wyników](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Wstawianie, aktualizowanie i usuwanie za pomocą procedur składowanych](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Mapuj jednostki z wieloma tabelami (dzielenie jednostki)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Mapowanie wielu jednostek do jednej tabeli (dzielenie tabeli)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definiowanie zapytań](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Szablonów generowania kodu](~/ef6/modeling/designer/codegen/index.md)
        -   [Powracanie do obiektu ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Przy użyciu modelu
    -   [Praca z klasą DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Wykonywanie zapytań wyszukiwania jednostek](~/ef6/querying/index.md)
    -   [Praca z relacjami](~/ef6/fundamentals/relationships.md)
    -   [Trwa ładowanie powiązanych jednostek](~/ef6/querying/related-data.md)
    -   [Praca z danymi lokalnymi](~/ef6/querying/local-data.md)
    -   [N-warstwowych](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Pierwotne zapytania SQL](~/ef6/querying/raw-sql.md)
    -   [Wzorce optymistycznej współbieżności](~/ef6/saving/concurrency.md)
    -   [Praca z serwerów proxy](~/ef6/fundamentals/proxies.md)
    -   [Automatyczne wykrywanie zmian](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Bez śledzenia zapytań](~/ef6/querying/no-tracking.md)
    -   [Metoda Load](~/ef6/querying/load-method.md)
    -   [Dodaj/Dołączanie i Stany jednostki](~/ef6/saving/change-tracking/entity-state.md)
    -   [Praca z wartościami właściwości](~/ef6/saving/change-tracking/property-values.md)
    -   [Dane wiązania przy użyciu platformy WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Danych powiązanych z WinForms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 środowiska wykonawczego został wydany do narzędzia NuGet w lutym 2012 wkrótce po EF 4.3.0.
Ta wersja poprawki zawarte kilka poprawek w wersji 4.3 platformy EF i wprowadzono lepszą obsługę LocalDB dla klientów korzystających z EF 4.3 w programie Visual Studio 2012.

Oto lista zawartości, której umieściliśmy razem specjalnie w celu wydania EF 4.3.1, większość zawartości parametru EF 4.1 nadal obowiązuje ograniczenie do EF 4.3 także.

-   [EF 4.3.1 wersji wpis w blogu](http://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 środowiska uruchomieniowego wydanej w lutym 2012 do narzędzia NuGet.
W tej wersji uwzględnione nową funkcję migracje Code First, która umożliwia bazy danych utworzonej przez rozwiązanie Code First przyrostowo zostać zmienione w miarę rozwoju model Code First.

Oto lista zawartości, które razem utworzona Państwu specjalnie dla wersji 4.3 platformy EF, większość zawartości parametru EF 4.1 nadal obowiązuje ograniczenie do EF 4.3 także:
-   [Wpis w wersji 4.3 platformy EF](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Przewodnik 4.3 migracje oparte na kod programem EF](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Przewodnik 4.3 automatycznej migracji EF](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
EF 4.2.0 środowiska wykonawczego został wydany do narzędzia NuGet w listopad 2011.
Ta wersja zawiera poprawki błędów EF 4.1.1 wydania.
Ponieważ tylko zawarte w tej wersji poprawki błędów, które wystąpiły EF 4.1.2 poprawki wersji, ale firma Microsoft zgody, aby przejść do 4.2, aby umożliwić nam odbiegać od daty na podstawie numerów wersji poprawki zostały użyte podczas 4.1.x zwalnia i przyjmuje [semantycznego Versionsing](https://semver.org) standardowych wersji semantycznej.

Oto lista zawartości, które razem utworzona Państwu specjalnie dla wersji EF 4.2, treść podana dla platformy EF 4.1 nadal mają zastosowanie EF 4.2 także.

-   [Wpis w wersji 4.2 EF](http://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Pierwszy instruktaż dotyczący kodu](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Pierwszym omówieniem modelu i bazy danych](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
EF 4.1.10715 środowiska wykonawczego został wydany do narzędzia NuGet w lipca 2011 roku.
Oprócz poprawek błędów Ta wersja poprawki wprowadzone niektórych składników, aby ułatwić czasu projektowania narzędzi do pracy z modelem Code First.
Te składniki są używane przez migracje Code First (zawarte w wersji 4.3 platformy EF) i programem EF Power Tools.

Można zauważyć, że otrzymano nieoczekiwany wersji numer 4.1.10715 pakietu.
Użyliśmy korzystają z wersji poprawki na podstawie daty, zanim podjęliśmy decyzję o przyjęcie [Semantic Versioning](https://semver.org).
Pomyśl o tej wersji EF 4.1 poprawka 1 (lub EF 4.1.1).

Oto lista zawartości razem utworzona Państwu dla 4.1.1 wersji:

-   [EF 4.1.1 wersji wpisu](http://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
EF 4.1.10331 środowiska uruchomieniowego była pierwszą mają zostać opublikowane w usłudze NuGet, w kwietnia 2011 r.
W tej wersji są uwzględnione uproszczony interfejs API typu DbContext i Code First przepływu pracy.

Zauważysz, numer wersji dziwne 4.1.10331, które naprawdę powinny zostać 4.1. Ponadto istnieje 4.1.10311 wersji, które powinny zostać 4.1.0-rc ("rc" oznacza "Wersja Release candidate").
Użyliśmy korzystają z wersji poprawki na podstawie daty, zanim podjęliśmy decyzję o przyjęcie [Semantic Versioning](https://semver.org).

Oto lista zawartości, które ze sobą umieściliśmy w wersji 4.1. Nadal obowiązuje ograniczenie ilości go do nowszej wersji programu Entity Framework:

-   [Wpis w wersji 4.1 EF](http://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Pierwszy instruktaż dotyczący kodu](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Pierwszym omówieniem modelu i bazy danych](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure Federacji i Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
W tej wersji zostało uwzględnione w .NET Framework 4 i Visual Studio 2010, w kwietniu 2010.
Ważnych nowych funkcji w tej wersji uwzględnione POCO pomocy technicznej, mapowanie klucza obcego, ładowanie z opóźnieniem, ulepszenia testowania, generowanie kodu można dostosowywać i modelu pierwszego przepływu pracy.

Mimo że pochodzi drugie wydanie programu Entity Framework, został o nazwie 4 EF, aby było zgodne z .NET Framework w wersji dostarczanej z programem.
Po tej wersji firma Microsoft pracę, udostępniając dla narzędzia NuGet programu Entity Framework i przyjęte semantycznego przechowywania wersji, ponieważ zostały firma Microsoft nie jest już powiązany z wersji programu .NET Framework.

Należy pamiętać, że niektóre kolejne wersje programu .NET Framework zostały dostarczone z istotne aktualizacje uwzględnione bitów EF.
W rzeczywistości wiele nowych funkcji EF 5.0 zostały zaimplementowane jako ulepszenia na te usługi bits.
Jednak aby racjonalizować z historią przechowywania wersji dla platformy EF, firma Microsoft nadal odwoływać się do bitów EF, które należą do programu .NET Framework jako środowiska uruchomieniowego EF 4.0, gdy wszystkie nowsze wersje składają się z [pakiet NuGet platformy EntityFramework](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
Początkowa wersja platformy Entity Framework została uwzględniona w .NET 3.5 z dodatkiem SP1 i Visual Studio 2008 z dodatkiem SP1, wydanej w sierpniu 2008.
Ta wersja podana podstawowej Obiektowo pomocy technicznej przy użyciu bazy danych pierwszego przepływu pracy.

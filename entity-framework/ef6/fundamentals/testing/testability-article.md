---
title: Testowalność i Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: aec177438004fd255bef85a5e5047cf6b5a6f782
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284047"
---
# <a name="testability-and-entity-framework-40"></a>Testowalność i Entity Framework 4.0
Scott Allen

Data publikacji: Maja 2010

## <a name="introduction"></a>Wprowadzenie

Ten dokument zawiera opis i pokazuje, jak pisać kodu sprawdzalnego działa zgodnie z ADO.NET Entity Framework 4.0 i Visual Studio 2010. W tym dokumencie nie podejmuje próby skupić się na określonych metodologia testowania, takich jak projektowanie oparte na testach (TDD) lub projektowania opartego na zachowanie (BDD). Zamiast tego ten dokument koncentruje się na temat sposobu pisania kodu, który używa programu ADO.NET Entity Framework, ale pozostaje ułatwia odizolować i przetestować w zautomatyzowany sposób. Zapoznamy się często używane wzorce projektowe, które ułatwiają dostęp do scenariuszy testowania w danych i zobacz, jak te wzorce są stosowane, gdy przy użyciu platformy. Również przyjrzymy określonych funkcji framework, aby zobaczyć, jak te funkcje mogą współdziałać w kodu sprawdzalnego działa zgodnie.

## <a name="what-is-testable-code"></a>Co to jest kodu sprawdzalnego działa zgodnie?

Możliwość sprawdzenia oprogramowanie przy użyciu zautomatyzowanych testów jednostek oferuje wiele korzyści pożądane. Wszyscy wie, że dobry test zmniejsza liczbę usterek oprogramowania w aplikacji i zwiększa jakość aplikacji — ale przebiega testów jednostkowych w miejscu wykraczają daleko poza po prostu znajdowaniu usterek.

Zestaw testów jednostkowych dobre umożliwia zespół deweloperów zaoszczędzić czas i zachowuje kontrolę nad tworzone przez nich oprogramowanie. Zespół wprowadzić zmiany w istniejącym kodzie, Refaktoryzacja, ich przeprojektowywania i restrukturyzacji oprogramowania do nowych wymagań i dodawania nowych składników do aplikacji, korzystając z możliwości, wiedząc, zestawu testów, można sprawdzić działanie aplikacji. Testy jednostkowe są częścią cyklu szybkiej opinii w ułatwienia zmian i zachować łatwość oprogramowania w miarę wzrostu złożoności.

Testy jednostkowe jest dostarczany z ceną, jednak. Zespół będzie musiał poświęcić czas na tworzenie i obsługa testów jednostkowych. Nakład pracy wymagany do utworzenia tych testów jest bezpośrednio związana **testowalności** podstawowego oprogramowania. Jak łatwo jest oprogramowanie do testowania? Zespół projektowania oprogramowania za pomocą testowania należy pamiętać, utworzy szybciej niż zespół, Praca z oprogramowania bez sprawdzalnego działa zgodnie skuteczne testy.

Firma Microsoft zaprojektowała ADO.NET Entity Framework 4.0 (EF4) za pomocą testowania na uwadze. Nie oznacza to, że deweloperzy będzie zapisywać testów jednostkowych dla framework sam kod. Zamiast tego cele testowania EF4 ułatwiają tworzenie kodu sprawdzalnego działa zgodnie opartą na strukturze. Zanim przyjrzymy się konkretne przykłady, warto zrozumieć jakość kodu sprawdzalnego działa zgodnie.

### <a name="the-qualities-of-testable-code"></a>Jakość kodu sprawdzalnego działa zgodnie

Kod, który jest łatwy do testów zawsze będzie mieć co najmniej dwóch cech. Łatwo jest pierwszym kodu sprawdzalnego działa zgodnie **obserwować**. Biorąc pod uwagę pewne zestaw danych wejściowych, powinno być łatwe do sprawdzanie danych wyjściowych kodu. Na przykład następujące metody badania jest łatwe, ponieważ metoda bezpośrednio zwraca wynik obliczeń.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Testowanie metody jest trudne, jeśli metoda zapisuje obliczona wartość do gniazda sieciowego, tabeli bazy danych lub plików, podobnie do poniższego kodu. Test musi być wykonania dodatkowej pracy do pobrania wartości.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Po drugie, jest łatwa do kodu sprawdzalnego działa zgodnie **izolowania**. Użyjmy następujący pseudo-kod złe przykład kodu sprawdzalnego działa zgodnie.

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

Metoda ta jest łatwy do obserwowania — firma Microsoft może przekazać ubezpieczeniowej i sprawdź, czy oczekiwany wynik pasuje do wartości zwracanej. Jednak do metody testowej musimy mieć zainstalowane przy użyciu poprawnego schematu bazy danych i konfigurowanie serwera SMTP, w przypadku, gdy metoda próbuje wysłać wiadomość e-mail.

Test jednostkowy tylko chciał sprawdzić, czy logiki obliczeń wewnątrz metody, ale test może zakończyć się niepowodzeniem, ponieważ serwer poczty e-mail jest w trybie offline lub przenieść serwer bazy danych. Oba te błędy są związane z zachowaniem testu będzie chciał sprawdzić, czy. Zachowanie jest trudne do izolowania.

Programistów, którzy Dokładamy wszelkich starań, aby zapisać często kodu sprawdzalnego działa zgodnie Dokładamy wszelkich starań zachować separacji w kodzie są zapisu. Powyższej metody należy skoncentrować się na obliczeń biznesowych i delegować szczegółów implementacji bazy danych i poczty e-mail do innych składników. Robert C. Martin wywołuje to pojedynczy zasady odpowiedzialności. Obiekt powinna hermetyzować pojedynczy, wąskie odpowiedzialność, takich jak obliczenia wartości zasady. Wszystkie inne zadania bazy danych i powiadomień należy odpowiedzialność innego obiektu. Kod napisany w ten sposób jest łatwiejsze do izolowania ponieważ koncentruje się na pojedynczym zadaniem.

Na platformie .NET mamy abstrakcji, potrzebne do działania w przekonaniu pojedynczej odpowiedzialności i uzyskania izolacji. Możemy użyć definicji interfejsu i wymusić kod, aby użyć abstrakcji interfejsu, a nie do konkretnego typu. W dalszej części tego dokumentu zobaczymy, jak metoda, takich jak zły przykład przedstawiony powyżej może współpracować z interfejsów *Szukaj* jak rozmawiamy w bazie danych. W czasie testu jednak możemy użyć zamiast fikcyjnego implementację, która nie skontaktuj się z bazą danych, ale zamiast tego przechowuje dane w pamięci. Ta implementacja fikcyjnego izoluje kodu z niepowiązanych problemów w kod dostępu do danych lub baza danych konfiguracji.

Istnieją dodatkowe korzyści z izolacji. Obliczenia biznesowego w ostatnim metody powinna trwać tylko kilka milisekund, które można wykonać, ale same testy mogą działać przez kilka sekund jako przeskoków kodu wokół sieci i rozmowy na różnych serwerach. Testy jednostkowe, należy uruchomić szybki w celu ułatwienia niewielkich zmian. Testy jednostkowe powinny także powtarzalne oraz wystąpi niepowodzenie, ponieważ składnik niezwiązanych ze sobą na test ma problem. Pisanie kodu, który jest łatwy do obserwowania i do izolowania oznacza, że deweloperzy będzie teraz łatwiejsze pisania testów dla kodu, mogą spędzać mniej czasu oczekiwania na testami do wykonania, a więcej co ważniejsze, mogą spędzać mniej czasu, śledzenie błędów, które nie istnieją.

Miejmy nadzieję można docenić zalety testowania i zrozumieć cechy, które wykazuje kodu sprawdzalnego działa zgodnie. Jesteśmy rozwiązać jak napisać kod, który współdziała z EF4, aby zapisać dane w bazie danych pozostając dostrzegalnych i łatwe izolowanie, ale najpierw będzie zawęzić naszym głównym celem w celu omówienia sprawdzalnego działa zgodnie projektów, aby uzyskać dostęp do danych.

## <a name="design-patterns-for-data-persistence"></a>Wzorce projektowe zapewniające trwałość danych

Obu przykładach zły przedstawiony wcześniej było zbyt wiele obowiązki. Pierwszy przykład zły musiał wykonać obliczenia *i* zapisu do pliku. Drugi przykład zły musiały odczytywać dane z bazy danych *i* wykonywanie obliczeń biznesowych *i* wysyłanie wiadomości e-mail. Projektowanie mniejszych metody, które Rozdziel problemy i delegować odpowiedzialność za inne składniki wprowadzisz dotykowego do pisania kodu sprawdzalnego działa zgodnie. Celem jest tworzyć funkcje za pośrednictwem akcji abstrakcje niewielkiego i skupionego projektu.

Gdy chodzi o trwałości danych mały abstrakcje ukierunkowanych, których firma Microsoft szuka są więc wspólne został zostały opisane jako wzorców projektowych. Książki Martina Fowlera wzorców Enterprise architektury aplikacji była pierwszym pracy do opisania tych wzorców w drukowania. Firma Microsoft udostępni krótki opis tych wzorców w poniższych sekcjach, zanim pokazujemy, jak te ADO.NET Entity Framework implementuje i współdziała z tych wzorców.

### <a name="the-repository-pattern"></a>Wzorzec repozytorium

Fowlera wynika z repozytorium "pośredniczy między domeną i dane warstw mapowania za pomocą interfejsu przypominającego kolekcji do uzyskiwania dostępu do obiektów domeny". Celem wzorca repozytorium jest do izolowania kodu z minutiae dostępu do danych i jak widzieliśmy wcześniej izolacji jest cechę wymaganych do testowania.

Kluczem do izolacji jest, jak repozytorium przedstawia obiektów za pomocą interfejsu przypominającego kolekcji. Logika zapisu do użycia w repozytorium ma nie wiadomo, jak repozytorium zmaterializowania obiektów, których w przypadku żądania. Repozytorium może komunikować się z bazą danych lub po prostu może zwracać obiekty z kolekcji w pamięci. Wszystko, czego Twój kod musi wiedzieć, jest repozytorium pojawia się do zachowania kolekcji i pobierania, dodawanie i usuwanie obiektów z kolekcji.

W istniejących aplikacjach .NET konkretnych repozytorium często dziedziczy interfejs ogólny, jak pokazano poniżej:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Wybierzemy kilka zmian do definicji interfejsu gdy firma Microsoft zapewnia implementację dla EF4, ale podstawowe pojęcia pozostają bez zmian. Kod można użyć konkretnego repozytorium implementacji interfejsu można pobrać jednostki, jego wartość klucza podstawowego, można pobrać kolekcję jednostek na podstawie oceny predykat, lub po prostu pobrać wszystkie dostępne jednostki. Kod można również dodawać i usuwać jednostki za pośrednictwem interfejsu repozytorium.

Biorąc pod uwagę obiektów IRepository pracowników, kod może wykonać następujące czynności.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Ponieważ kod wykorzystuje interfejs (IRepository pracowników), firma Microsoft może dostarczyć kod różne implementacje interfejsu. Jedna implementacja może być implementację wspierane przez EF4 i trwałość obiektów w bazie danych programu Microsoft SQL Server. Inną implementację (po jednym używanych przez firmę Microsoft podczas testowania) może być objęta obiektów listy pracowników w pamięci. Interfejs może pomóc w celu uzyskania izolacji w kodzie.

Zwróć uwagę, IRepository&lt;T&gt; interfejsu nie ujawnia operacja zapisu. Jak zaktualizować istniejące obiekty? Mogą pochodzić między definicje IRepository, które obejmują operacja zapisu i implementacji tych repozytoriów, należy od razu utrwalić obiektu do bazy danych. Jednak w wielu aplikacjach nie chcemy zachować obiekty indywidualnie. Zamiast tego chcemy Ożyw obiektów, być może z różnych repozytoriów, zmodyfikować te obiekty jako część operacji biznesowych i następnie zachować wszystkie obiekty w ramach jednej operacji niepodzielnych. Na szczęście jest wzorzec tego typu zachowania.

### <a name="the-unit-of-work-pattern"></a>Jednostka wzorzec pracy

Fowlera mówi, jednostka pracy będzie "Obsługa listy obiektów wpływ transakcji biznesowych i służy do koordynowania zapisu poza zmiany i rozwiązywanie problemów współbieżności". Jest odpowiedzialny za jednostkę pracy do śledzenia zmian do obiektów, firma Microsoft Ożyw z repozytorium i utrwala wszelkie zmiany, które wprowadziliśmy do obiektów, gdy o jednostce pracy, aby potwierdzić zmiany. Jest również odpowiedzialność za jednostkę pracy do wykonania nowe obiekty, firma Microsoft zostały dodane do wszystkich repozytoriów i wstawianie obiektów bazy danych, a także usunięcie Zarządzaj witryną.

Jeśli nigdy nie wykonano żadnej pracy z zestawami danych ADO.NET następnie będzie już można zapoznać się z jednostką wzorzec pracy. Zestawy danych ADO.NET wcześniej mieli możliwość Śledź nasze aktualizacje, usuwanie i wstawianie elementu DataRow obiektów i może (za pomocą adaptera TableAdapter) uzgadniają wszystkich zmian do bazy danych. Jednak obiektów DataSet modelu podzestaw odłączonej bazy danych. Jednostka wzorzec pracy wykazuje takie samo zachowanie, ale działa z obiektami biznesowych i obiektów domeny, które są odizolowane od kod dostępu do danych i rozpoznaje bazy danych.

Abstrakcja do modelowania jednostek pracy w kodzie .NET może wyglądać następująco:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Dzięki uwidocznieniu działania odwołania repozytorium przy użyciu jednostki pracy, którą firma Microsoft zapewnia pojedynczą jednostkę pracy obiekt ma możliwość śledzenia wszystkich jednostek zmaterializowanego podczas transakcji biznesowych. Implementacja metody zatwierdzania dla rzeczywistego jednostki pracy jest do uzgadniają zmiany w pamięci z bazą danych, gdzie się Magia. 

Biorąc pod uwagę odwołaniem IUnitOfWork, kod można wprowadzać zmiany w obiektach firm pobierane z jednego lub więcej repozytoriów i Zapisz wszystkie zmiany, przy użyciu niepodzielnych operacji zatwierdzania.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Wzorzec obciążenia z opóźnieniem

Fowlera używa nazwy obciążenia z opóźnieniem do opisania "obiekt, który nie zawiera wszystkich danych potrzebujesz, ale wie, jak przygotować". Przezroczysty powolne ładowanie to ważna cecha ma podczas pisania kodu sprawdzalnego działa zgodnie biznesowych i Praca z relacyjnej bazy danych. Na przykład rozważmy poniższy kod.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Jak jest wypełniana kolekcji kart Istnieją dwa możliwe odpowiedzi. Jedną odpowiedź jest, że repozytorium pracowników, po wyświetleniu monitu o pobranie pracownika, wysyła zapytanie do pobrania pracownika wraz z informacjami skojarzone karta godzin pracownika. Relacyjne bazy danych to zwykle wymaga zapytanie z klauzulą JOIN i może skutkować podczas pobierania informacji niż aplikacja potrzebuje. Co zrobić, jeśli aplikacja nigdy nie musi dotykać właściwości kart?

Druga odpowiedź jest można załadować właściwości kart "na żądanie". To powolne ładowanie jest niewidoczne dla logiki biznesowej i niejawne, ponieważ kod nie jest wywoływany specjalne interfejsów API można pobrać informacji o karcie czasu. Kod zakłada, że informacje dotyczące karty czasu jest obecna, gdy są potrzebne. Ładowanie z opóźnieniem, które zazwyczaj polega na przechwytywaniu środowiska uruchomieniowego wywołań metody opisywanego zaangażowanych jest kilka magic. Przechwytujący kodu jest odpowiedzialny za komunikować się z bazą danych i pobierania informacji o karcie czasu przy równoczesnym zachowaniu logika biznesowa może być logiki biznesowej. Ta magic obciążenia z opóźnieniem umożliwia kod firm, aby izolować sam z operacjami pobierania danych i skutkuje więcej kodu sprawdzalnego działa zgodnie.

Wadą ładowane z opóźnieniem jest fakt, że aplikacja *jest* potrzebne informacje karty godz., kod będzie wykonywał żadnych dodatkowych kwerend. To nie jest istotna dla wielu aplikacji, ale dla aplikacji zawierających poufne dane wydajności lub aplikacji liczba obiektów pracowników w pętli i wykonywanie zapytania do pobierania czasu kart podczas każdej iteracji pętli (problem często określane jako N + 1 problem z zapytania), powolne ładowanie jest przeciągania. W tych scenariuszach aplikacji może być eagerly załadować informacji o czasie karty w najbardziej efektywny sposób.

Na szczęście zobaczymy, jak EF4 obsługuje zarówno niejawne obciążeń z opóźnieniem i wydajne eager ładuje możemy przenosić do następnej sekcji i implementacji tych wzorców.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementowanie wzorców z programu Entity Framework

Dobra wiadomość jest wszystkich wzorców projektowych, które firma Microsoft opisanego w ostatniej sekcji są proste do wdrożenia przy użyciu EF4. Aby zademonstrować są użyjemy prostej aplikacji ASP.NET MVC do edytowania i wyświetlania pracowników i ich skojarzone karta godzin. Rozpoczniemy pracę przy użyciu następujących "zwykłe stare CLR obiektów" (POCOs). 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

Te definicje klas będzie się nieznacznie zmieniać omówimy różne podejścia i funkcje EF4, ale celem jest zapewnienie tych klas jako trwałości zakresu (PI) jak to możliwe. Obiekt PI nie wie, *jak*, lub nawet *Jeśli*, stan przechowuje, są przechowywane w bazie danych. PI i POCOs idą ręka w rękę z oprogramowaniem sprawdzalnego działa zgodnie. Obiekty przy użyciu podejścia POCO są mniej ograniczone, bardziej elastyczne i łatwiejsze do testu, ponieważ mogą one działać bez bazy danych istnieje.

Za pomocą POCOs w miejscu możemy utworzyć Entity Data Model (EDM) w programie Visual Studio (patrz rysunek 1). Firma Microsoft nie będzie używać EDM do generowania kodu dla naszych jednostek. Zamiast tego chcemy użyć jednostki, które firma Microsoft lovingly pracowali ręcznie. Firma Microsoft będzie używała tylko EDM Generowanie naszych schemat bazy danych, a następnie podaj metadane EF4 potrzebuje do mapowania obiektów do bazy danych.

![EF test_01](~/ef6/media/eftest-01.jpg)

**Rysunek 1.**

Uwaga: Jeśli chcesz najpierw opracowanie modelu EDM jest możliwe czyszczenie, generowanie kodu POCO z EDM. Można to zrobić za pomocą rozszerzenia programu Visual Studio 2010, dostarczane przez zespół Data programowania. Aby pobrać rozszerzenie, uruchom Menedżera rozszerzeń z menu Narzędzia w programie Visual Studio i znajdowania galerii szablonów w trybie online "POCO" (zobacz rysunek 2). Istnieje kilka szablonów POCO są dostępne na platformie EF. Aby uzyskać więcej informacji na temat korzystania z tego szablonu, zobacz " [Instruktaż: obiektów POCO szablonu programu Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![EF test_02](~/ef6/media/eftest-02.png)

**Rysunek 2**

Z tego POCO punkt początkowy przeanalizujemy dwa różne podejścia do kodu sprawdzalnego działa zgodnie. Pierwszym sposobem czy mogę wywołać podejście EF, ponieważ wykorzystuje abstrakcje z Entity Framework interfejs API, aby zaimplementować jednostek pracy i repozytoriów. W drugiej metody możemy utworzyć własną abstrakcje niestandardowe repozytorium i wtedy wyświetlone zalety i wady każdej metody. Rozpoczniemy od wypróbowania podejście EF.  

### <a name="an-ef-centric-implementation"></a>Implementacja przetwarzających EF

Należy wziąć pod uwagę następujące akcji kontrolera w projekcie ASP.NET MVC. Akcja pobiera obiekt pracowników i zwraca wynik, aby wyświetlić widok szczegółowy pracownika.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Jest kodu sprawdzalnego działa zgodnie? Istnieją co najmniej dwóch testów czy musimy zweryfikować zachowanie akcji. Najpierw chcemy sprawdzić, czy akcja zwraca widok poprawne — łatwe testu. Firma Microsoft będzie również chcieć napisać test, aby sprawdzić działanie powoduje pobranie poprawne pracowników i prosimy o poświęcenie zrobienie tego bez wykonywania kodu, aby w bazie danych. Należy pamiętać, że chcemy izolowanie testowanego kodu. Izolacja będzie upewnij się, że test nie zakończy się niepowodzeniem z powodu usterki w kod dostępu do danych lub baza danych konfiguracji. Jeśli test zakończy się niepowodzeniem, wiemy, że mamy usterkę w logiką kontrolera, a nie w niektórych niższe składnika poziomu systemu.

W celu uzyskania izolacji poprosimy niektóre elementy abstrakcji takich jak interfejsy, które firma Microsoft przedstawiony wcześniej dla repozytoriów i jednostki pracy. Pamiętaj, że wzorzec repozytorium jest przeznaczona do pośredniczy między obiektów domeny i warstwy mapowanie danych. W tym scenariuszu EF4 *jest* dane mapowania warstwy i udostępnia już abstrakcji podobne do repozytorium, o nazwie IObjectSet&lt;T&gt; (z przestrzeni nazw System.Data.Objects). Definicja interfejsu wygląda podobnie do poniższego.

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

IObjectSet&lt;T&gt; spełnia wymagania dotyczące repozytorium, ponieważ jest on podobny kolekcji obiektów (za pośrednictwem interfejsu IEnumerable&lt;T&gt;) i dostarcza metod dodawania i usuwania obiektów z kolekcji symulowane. Metod dołączania i odłączania uwidocznić dodatkowe funkcje interfejsu API EF4. Aby użyć IObjectSet&lt;T&gt; jako interfejs dla repozytoriów potrzebujemy jednostkę pracy abstrakcji można powiązać ze sobą repozytoriów.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Jedną konkretną implementację tego interfejsu będzie się komunikował z programu SQL Server i łatwo utworzyć za pomocą klasy obiektu ObjectContext z EF4. Klasa obiektu ObjectContext jest prawdziwe jednostka pracy w interfejsie API EF4.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

Dzięki temu IObjectSet&lt;T&gt; w życie jest równie proste jak wywołania metody CreateObjectSet obiektu ObjectContext. Za kulisami środowisko użyje metadanych zamieszczone w EDM, aby wygenerować konkretnego obiektu ObjectSet&lt;T&gt;. Używany będzie ze zwracaniem IObjectSet&lt;T&gt; interfejsu, ponieważ ułatwi to zachować testowania w kodzie klienta.

Ta implementacja konkretnych przydaje się w środowisku produkcyjnym, ale musimy skupić się na sposób użyjemy naszego abstrakcji IUnitOfWork aby ułatwić testowanie.

### <a name="the-test-doubles"></a>Badanie wartości podwójnej precyzji

Aby wyizolować akcji kontrolera będziemy potrzebować możliwości przełączania się między rzeczywistych jednostkę pracy (wspierana przez obiekt ObjectContext) i testowej jednostki typu double lub "fałszywych" pracy (wykonywania operacji w pamięci). Typowym podejściem do wykonania tego rodzaju przełączanie jest zezwala kontroler MVC wystąpienia jednostki pracy, ale zamiast tego przebiegu jednostkę pracy do kontrolera jako parametr konstruktora.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Powyższy kod jest przykładem iniekcji zależności. Nie umożliwiamy kontrolera utworzyć jego zależności (jednostka pracy), ale wstrzykiwanie zależności do kontrolera. W projekcie MVC jest często używa się fabryki kontrolerów niestandardowych w połączeniu z odwrócenie kontenera kontrolek (IoC) do automatyzowania iniekcji zależności. Te tematy wykraczają poza zakres tego artykułu, ale możesz przeczytać więcej przez następujące odwołania na końcu tego artykułu.

Jednostka fałszywych wprowadzania pracy, które firma Microsoft można używać do testowania może wyglądać następująco.

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

Zwróć uwagę, że fałszywych jednostkę pracy ujawnia właściwość zatwierdzone. Czasami jest to przydatne dodać funkcje do fałszywych klasę, która ułatwić testowanie. W tym przypadku jest łatwo obserwować, jeśli kod zatwierdzenia jednostkę pracy, zaznaczając właściwość zatwierdzone.

Będziemy również potrzebować fałszywych IObjectSet&lt;T&gt; do przechowywania obiektów, pracowników i karty czasowej w pamięci. Oferujemy pojedynczą implementacją za pomocą typów ogólnych.

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

Ten test podwójnego deleguje większość swojej pracy, aby podstawowy zestaw HashSet&lt;T&gt; obiektu. Uwaga tego IObjectSet&lt;T&gt; wymaga ograniczenie generyczne wymuszanie T jako klasę (typ odwołania) i wymusza także NAS, aby zaimplementować interfejs IQueryable&lt;T&gt;. Ułatwia tworzenie kolekcji w pamięci, są traktowane jako element IQueryable&lt;T&gt; przy użyciu standardowego operatora zapytań LINQ AsQueryable.

### <a name="the-tests"></a>Testy

Testy jednostkowe tradycyjnych użyje klasy jeden test do przechowywania wszystkich testów dla wszystkich akcji w pojedynczy kontroler MVC. Firma Microsoft można pisać testy lub dowolnego typu testu jednostkowego przy użyciu pamięci elementów sztucznych utworzyliśmy. Jednak w tym artykule, w których firma Microsoft zapobiegnie monolityczne testu klasy i zamiast tego pogrupować Nasze testy, aby skoncentrować się na konkretne funkcje.  Na przykład, "Tworzenie nowego pracownika" może być funkcje, które chcemy sprawdzić, więc zostanie użyta klasa jeden test Aby zweryfikować akcji pojedynczy kontroler odpowiedzialnych za tworzenie nowego pracownika.

Brak wspólnego kodu ustawień potrzebnych dla wszystkich tych klas testowych szczegółowe. Na przykład zawsze należy utworzyć naszych repozytoriów w pamięci i fałszywych jednostki pracy. Należy również wystąpienie kontrolera pracowników z fałszywych jednostek pracy, które są wstrzykiwane. Ten typowy kod instalacji zostaną udostępnione różnych klas testowych za pomocą klasy bazowej.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

"Matka obiektu" używamy w klasie bazowej jest jeden wspólny wzorzec do tworzenia danych testowych. Matkę obiektów zawiera metodami factory do tworzenia wystąpienia testów jednostek do użycia w wielu świetlnymi testu.

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

Możemy użyć EmployeeControllerTestBase jako klasa bazowa dla liczby świetlnymi testu (patrz rysunek 3). Każdy warunki początkowe testu przetestuje akcji określonego kontrolera. Na przykład jeden warunki początkowe testu koncentruje się na testowanie Akcja Utwórz używane podczas żądania HTTP GET (Aby wyświetlić widok na potrzeby tworzenia pracownika) i różnych warunków początkowych koncentruje się na Akcja Utwórz używana w żądaniu POST protokołu HTTP (do uwzględnienia informacji przesyłanych przez użytkownikowi utworzenie pracownika). Każdej klasy pochodnej odpowiada tylko ustawienia wymagane w tym kontekście określonego oraz w celu zapewnienia potwierdzenia potrzebne, aby zweryfikować wyniki dla kontekst określonego testu.

![EF test_03](~/ef6/media/eftest-03.png)

**Rysunek 3.**

Styl nazewnictwa Konwencji i testowania przedstawionych w tym miejscu nie jest wymagana dla kodu sprawdzalnego działa zgodnie — wystarczy jedno z podejść. Rysunek 4 przedstawia testów przeprowadzanych codziennie w Resharper mózgów Jet test runner wtyczki dla programu Visual Studio 2010.

![EF test_04](~/ef6/media/eftest-04.png)

**Rysunek 4**

Za pomocą klasy bazowej do obsługi kodu udostępnionego Instalatora testów jednostkowych dla każdej akcji kontrolera są małe i łatwe do zapisu. Testy będą wykonywane szybko (ponieważ będziemy działają w pamięci operacji) i nie powinna zakończyć się niepowodzeniem ze względu na niepowiązanych infrastruktury lub środowiskiem naturalnym (ponieważ mamy już izolowany jednostki w ramach testu).

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

W tych testach klasa bazowa wykonuje większość pracy Instalatora. Należy pamiętać, że tworzy konstruktora klasy bazowej, repozytorium w pamięci, fałszywych jednostkę pracy i wystąpienia klasy EmployeeController. Klasa testowa pochodzi z tej klasy bazowej i koncentruje się na szczegółowe informacje na temat testowania metody Create. W tym przypadku szczegółowe informacje na temat gotować "rozmieścić, działanie i asercja" czynności, pokazywany w jakiejkolwiek jednostce z badania procedury:

-   Utwórz obiekt Nowy_pracownik symulowanie danych przychodzących.
-   Wywołaj akcję Utwórz EmployeeController i przekazać Nowy_pracownik.
-   Sprawdź, czy akcja Utwórz tworzy oczekiwanych wyników (pracownika pojawia się w repozytorium).

Co utworzyliśmy pozwala nam na wszystkie akcje EmployeeController testu. Na przykład podczas pisania testów dla akcji indeksu kontrolera pracowników firma Microsoft może dziedziczyć z klasy bazowej testu do ustanowienia tej samej podstawowej instalacji na potrzeby testów. Klasa bazowa utworzy ponownie repozytorium w pamięci, fałszywych jednostkę pracy i wystąpienia EmployeeController. Testy dla akcji indeksu wystarczy skupić się na wywoływaniu akcji indeksu i testowanie jakość modelu akcji zwraca.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

Tworzymy z substytutami w pamięci nie jest rozróżniana ukierunkowane na testowanie *stanu* oprogramowania. Na przykład podczas testowania Akcja Utwórz, chcemy, aby sprawdzić stan repozytorium, po wykonaniu akcji tworzenia — repozytorium przechowywania nowych pracownika?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Później przyjrzymy interakcji na podstawie badania. Testowanie interakcji na podstawie zostanie wyświetlone pytanie, jeśli testowany kod wywoływany właściwe metody na naszych obiektów i przekazywane poprawnych parametrów. Teraz przejdziemy okładki innego wzorzec projektowy — obciążenia z opóźnieniem.

## <a name="eager-loading-and-lazy-loading"></a>Wczesne ładowanie i powolne ładowanie

W pewnym momencie w sieci web platformy ASP.NET MVC aplikacji, którą firma Microsoft może chcieć wyświetlić informacje dotyczące pracowników i obejmują pracownika skojarzone karty godzin. Na przykład możemy mieć karty czas wyświetlania podsumowania, zawierający nazwisko pracownika oraz łącznej liczbie kart czasu systemu. Istnieje kilka rozwiązań, które możemy wykonać, aby zaimplementować tę funkcję.

### <a name="projection"></a>Rzut

Jedno z podejść łatwy do utworzenia podsumowania jest do konstruowania modelu dedykowanych informacje, które ma być wyświetlany w widoku. W tym scenariuszu modelu może wyglądać następująco.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Należy pamiętać, że EmployeeSummaryViewModel nie jest jednostką — innymi słowy nie jest coś, co jest potrzebne do utrwalenia w bazie danych. Tylko będziemy używać tej klasy do mieszania danych w widoku w silnie typizowany sposób. Model widoku przypomina danych przenieść obiekt (DTO), ponieważ zawiera ona nie zachowanie (nie metody) — tylko właściwości. Właściwości będą przechowywane dane, które trzeba przenieść. Jest łatwy do utworzenia wystąpienia tego modelu widoku za pomocą operatora rzutowania standard firmy LINQ — wybierz operator.

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

Istnieją dwa istotne funkcje do powyższego kodu. Najpierw — kod jest łatwy do testowania, ponieważ jest wciąż łatwe do obserwowania i izolować. Wybierz operator działa równie dobrze względem naszych elementów sztucznych w pamięci, co względem rzeczywistych jednostkę pracy.

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

Druga funkcja istotne jest, jak kod umożliwia EF4 do wygenerowania pojedynczego, wydajny kwerendy można złożyć pracowników i karty z czasu informacji ze sobą. Firma Microsoft załadowane informacje dotyczące pracowników i informacje dotyczące karty czasu do tego samego obiektu bez korzystania z żadnych specjalnych interfejsów API. Kod wyrażone jedynie informacje, których wymaga przy użyciu standardowych operatorów LINQ, które działają względem źródła danych w pamięci, a także zdalnych źródeł danych. EF4 był w stanie dokonać translacji drzew wyrażeń, generowane przez zapytanie LINQ i C\# kompilatora do pojedynczych i wydajne zapytania T-SQL.

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

Brak innym razem, gdy firma Microsoft nie chcesz pracować z modelu widoku lub obiekt DTO, ale z rzeczywistych jednostek. Gdy wiemy, że potrzebujemy pracownika *i* karty godzin pracownika, firma Microsoft eagerly ładowanie powiązanych danych w sposób dyskretny kod i wydajne.

### <a name="explicit-eager-loading"></a>Jawne wczesne ładowanie

Jeśli chcemy eagerly załadować informacji o powiązanych jednostek potrzebujemy mechanizmu logiki biznesowej (lub w tym scenariuszu kontroler akcji logiki) aby wyrazić chęć repozytorium. Obiekt ObjectQuery EF4&lt;T&gt; klasa definiuje metodę Include w taki sposób, aby określić pokrewnych obiektów do pobrania podczas wykonywania kwerendy. Należy pamiętać, EF4 ObjectContext udostępnia jednostek za pomocą konkretnego obiektu ObjectSet&lt;T&gt; klasy, która dziedziczy z ObjectQuery&lt;T&gt;.  Jeśli firma Microsoft była używana w obiekcie ObjectSet&lt;T&gt; odwołań w naszej akcji kontrolera, napiszemy następujący kod, aby określić eager obciążenia informacje dotyczące karty czas dla każdego pracownika.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Jednak ponieważ próbujemy zapewnienie naszego kodu sprawdzalnego działa zgodnie możemy są nie udostępnianie obiektu ObjectSet&lt;T&gt; z poza rzeczywistych jednostką pracy klasy. Zamiast tego Polegamy IObjectSet&lt;T&gt; interfejs, który ułatwia fałszywe, ale IObjectSet&lt;T&gt; nie definiuje metodę Include. Zaletą korzystania z LINQ jest, możemy utworzyć własną operator Include.

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

Zwróć uwagę, ten operator Include jest zdefiniowany jako metody rozszerzenia dla elementu IQueryable&lt;T&gt; zamiast IObjectSet&lt;T&gt;. To daje możliwość korzystania z metody z szerszego zakresu możliwych typów, w tym IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, a obiekt ObjectSet&lt;T&gt;. W przypadku sekwencji źródłowej nie jest oryginalnym ObjectQuery EF4&lt;T&gt;, to nie ma żadnych szkód, wykonywane, i Include operator jest pusta. Jeśli podstawowe sekwencji *jest* ObjectQuery&lt;T&gt; (lub pochodzić od ObjectQuery&lt;T&gt;), EF4 będą Zobacz nasze wymagania w zakresie dodatkowych danych i formułowanie odpowiednie SQL Zapytanie.

Za pomocą tego nowego operatora w miejscu możemy jawne żądanie eager obciążenia informacje dotyczące karty czasu z repozytorium.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Po uruchomieniu testów rzeczywistego obiektu ObjectContext, ten kod tworzy następujące pojedynczego zapytania. Zapytanie zbiera wystarczającą ilość informacji z bazy danych w jednym podróży do zmaterializowania obiektów pracowników i całkowicie wypełnić ich właściwości kart.

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

Mamy znakomitą wiadomość jest całkowicie sprawdzalnego działa zgodnie pozostaje kod wewnątrz metody akcji. Nie potrzebujemy Podaj wszelkie dodatkowe funkcje dla naszych elementów sztucznych na obsługuje operatora Include. Złe wiadomości jest boss Said WE had użycie operatora Include wewnątrz kodu, Chcieliśmy, aby zapewnić trwałość zakresu. Jest to podstawowy przykład typu skutków ubocznych, które będą potrzebne do wzięcia pod uwagę podczas kompilowania kodu sprawdzalnego działa zgodnie. Istnieją terminy, niezbędne, aby umożliwić przeciek wątpliwości trwałości poza abstrakcji repozytorium, aby spełnić cele dotyczące wydajności.

Alternatywa dla wczesne ładowanie jest powolne ładowanie. Powolne ładowanie oznacza, że robimy *nie* naszych firm kod potrzebny do jawnie poinformować o konieczności powiązane dane. Zamiast tego stosujemy nasze jednostek w aplikacji, a jeśli dodatkowe dane potrzebne Entity Framework załaduje dane na żądanie.

### <a name="lazy-loading"></a>Ładowanie z opóźnieniem

To ułatwia Wyobraź sobie scenariusz, w których nie można określić, co będzie potrzebne dane dany fragment logiki biznesowej. Firma Microsoft znać logiki wymaga obiektu pracowników, ale firma Microsoft może gałąź do wykonywania różnych ścieżek, gdzie niektóre z tych ścieżek wymagają informacji o karcie czas od pracownika, a niektóre nie. Scenariusze, takie jak to są idealne dla niejawnego powolne ładowanie ponieważ magiczny sposób wyświetlania danych na zgodnie z potrzebami.

Powolne ładowanie, znany także jako odroczone ładowanie, umieść pewne wymagania na naszych obiekty jednostki. POCOs z nieznajomości trwałości wartość true, nie będzie twarzy wszelkie wymagania z warstwy trwałości, ale nieznajomości trwałości wartość true, jest praktycznie niemożliwe do osiągnięcia.  Zamiast tego mierzymy nieznajomości trwałości w stopniach względnych. Byłoby niefortunne potrzebowaliśmy dziedziczą z klasy bazowej zorientowanej na trwałości lub użyć wyspecjalizowane kolekcji, aby osiągnąć powolne ładowanie w POCOs. Na szczęście EF4 ma płynniejsza rozwiązania.

### <a name="virtually-undetectable"></a>Praktycznie wykryć

Używanie obiektów POCO, EF4 można dynamicznie generować proxy środowiska uruchomieniowego dla jednostki. Te serwery proxy niewidocznie opakować POCOs zmaterializowany i zapewnić Uzyskaj dodatkowe usługi przechwycenie każdej właściwości i ustaw operację do wykonania dodatkowej pracy. Takie usługi jest funkcja ładowania z opóźnieniem, którą firma Microsoft szuka. Inna usługa jest wydajny mechanizm, który może zapisać zmianie wartości właściwości jednostki, program śledzenia zmian. Lista zmian jest używana przez obiekt ObjectContext podczas metodę SaveChanges, aby utrwalić wszystkie jednostki modyfikacji, przy użyciu polecenia aktualizacji.

Dla tych serwerów proxy do pracy jednak potrzebują sposób do podłączenia do właściwości get i ustawiania wykonywanych względem jednostki a serwerami proxy osiągnięcie tego celu przez zastąpienie wirtualnych elementów członkowskich. W związku z tym jeśli chcemy masz niejawnej powolne ładowanie i wydajny śledzenia zmian należy przejść z powrotem do naszych POCO definicje klas i oznacz właściwości jako wirtualny.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Firma Microsoft może nadal wskazuje, że jednostki pracowników jest przede wszystkim trwałości zakresu. Jedynym wymaganiem jest, aby użyć wirtualnych elementów członkowskich i nie ma to wpływu na testowanie kodu. Firma Microsoft nie muszą pochodzić z żadnych specjalnych klasy bazowej lub nawet za pomocą specjalnych kolekcji przeznaczone do ładowania z opóźnieniem. Tak jak pokazano kod, każda klasa implementacji ICollection&lt;T&gt; jest dostępna na potrzeby przechowywania powiązanych jednostek.

Istnieje również jeden drobnej zmiany, które należy wprowadzić w naszych jednostki pracy. Powolne ładowanie jest *poza* domyślnie, pracując bezpośrednio z obiektem ObjectContext. Jest właściwością, możemy ustawić właściwości ContextOptions umożliwia odroczone ładowanie i możemy ustawić tę właściwość w naszym rzeczywistych jednostkę pracy, jeśli chcemy Włącz powolne ładowanie wszędzie.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

Przy użyciu niejawnego ładowania z opóźnieniem, włączone, kod aplikacji może użyć pracownika pracownika związanych oraz kart czas pozostając błogiej świadomości ilości pracy wymaganej dla platformy EF załadować dodatkowe dane.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Powolne ładowanie sprawia, że kod aplikacji jest łatwiejsze do zapisu, a proxy magic kod nie będzie całkowicie sprawdzalnego działa zgodnie. Substytuty w pamięci jednostki pracy po prostu wstępnego ładowania fałszywych jednostki z powiązane dane, gdy potrzebne podczas testu.

W tym momencie będziesz Włącz naszej uwagi od tworzenia repozytoriów za pomocą IObjectSet&lt;T&gt; i przyjrzyj się elementy abstrakcji, aby ukryć wszystkie znaki Framework trwałości.

## <a name="custom-repositories"></a>Niestandardowe repozytoria

Umieszczeniem możemy najpierw jednostki pracy wzorcu projektowym w tym artykule zamieszczone przykładowy kod dla jak może wyglądać jednostkę pracy. Umożliwia ponowne przedstawia tę koncepcję oryginalny, używając pracowników i pracowników karta godzin scenariusza, który możemy masz doświadczenie w pracy z.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Główną różnicą między tej jednostki pracy i jednostki pracy utworzonych w ostatniej sekcji jest jak tej jednostki pracy nie używa żadnych abstrakcje platformy EF4 (nie ma żadnych IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; działa również jako interfejs repozytorium, ale interfejs API udostępnia ona doskonale mogą być niewyrównane naszej aplikacji potrzebom. W tym podejściu nadchodzących firma Microsoft będzie reprezentować repozytoriów przy użyciu niestandardowych IRepository&lt;T&gt; abstrakcji.

Wielu programistów, którzy postępuj zgodnie z projektowania opartego na testach, projektowania opartego na zachowanie i projektowanie metodologii opartego na domenach Preferuj IRepository&lt;T&gt; podejścia z kilku powodów. Najpierw IRepository&lt;T&gt; interfejs reprezentuje "" warstwa przeciwdegradacyjna. Zgodnie z opisem w Eric Evans w książce projektowania opartego na domenie warstwy przeciwdegradacyjnej przechowuje kod domeny od infrastruktury interfejsów API, np. trwałość interfejsu API. Po drugie deweloperzy mogą tworzyć metody do repozytorium, które potrzeb dokładnie aplikacji (jak wykryte podczas pisania testów). Na przykład może być często potrzebujemy zlokalizować pojedynczą jednostkę przy użyciu wartości Identyfikatora, abyśmy mogli dodać metodę FindById interfejsu repozytorium.  Nasze IRepository&lt;T&gt; definicja będzie wyglądać podobnie do poniższego.

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Zwróć uwagę, firma Microsoft będzie ponownie upuść, aby za pomocą element IQueryable&lt;T&gt; interfejsu do udostępnienia kolekcji jednostek. Element IQueryable&lt;T&gt; umożliwia drzew wyrażeń LINQ przepływać do dostawcy EF4 i dostawcy holistycznego widoku zapytania. Drugą opcją będzie zwracać IEnumerable&lt;T&gt;, co oznacza, że dostawcy EF4 LINQ będą widzieć tylko wyrażenia utworzone wewnątrz repozytorium. Wszelkie grupowania, kolejność i projekcji wykonywane poza repozytorium nie będzie składa do polecenia SQL wysyłane do bazy danych, która może obniżyć wydajność. Z drugiej strony, repozytorium, zwracając tylko IEnumerable&lt;T&gt; wyniki będą nigdy nie Cię zaskoczyć, za pomocą nowego polecenia SQL. W obu przypadkach efekt będzie działać, a oba podejścia nadal sprawdzalnego działa zgodnie.

Jest prosta zapewnić pojedynczą implementacją IRepository&lt;T&gt; interfejs, za pomocą typów ogólnych i EF4 API obiektu ObjectContext.

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

IRepository&lt;T&gt; podejście zapewnia nam niektóre dodatkową kontrolę nad naszego zapytania, ponieważ klient musi wywołać metodę, aby uzyskać dostęp do jednostki. Wewnątrz metody możemy to zapewnić dodatkowe czynności kontrolne i operatorów LINQ do wymuszania ograniczeń aplikacji. Zwróć uwagę, że interfejs ma dwa ograniczenia dla parametru typu ogólnego. Pierwszy ograniczeniem jest to klasa zmiany barwy wad, wymagane przez obiekt ObjectSet&lt;T&gt;, i drugi ograniczenie wymusza naszych jednostek do zaimplementowania IEntity — abstrakcję utworzony dla aplikacji. Interfejs IEntity wymusza podmiotów odczytywalną Właściwość Id, a następnie możemy użyć tej właściwości w metodzie FindById. IEntity jest zdefiniowana z następującym kodem.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity może zostać uznana za mały naruszenie nieznajomości trwałości, ponieważ naszych jednostki są wymagane do zaimplementowania interfejsu. Należy pamiętać nieznajomości trwałości dotyczy wady i zalety i dla wielu funkcji FindById będzie przeważają ograniczenia nałożone przez interfejs. Interfejs nie ma wpływu na testowania.

Utworzenie wystąpienia na żywo IRepository&lt;T&gt; wymaga obiektu ObjectContext EF4, dlatego konkretnej jednostki pracy wdrożenia należy zarządzać procesu tworzenia wystąpienia.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a>Przy użyciu niestandardowych repozytorium

Za pomocą naszych niestandardowe repozytorium nie jest znacznie różnią się od przy użyciu repozytorium, w oparciu o IObjectSet&lt;T&gt;. Zamiast bezpośrednio do właściwości z zastosowaniem operatorów LINQ, najpierw musisz wywołać za pomocą jednego z repozytorium metod do pobrania element IQueryable&lt;T&gt; odwołania.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Zwróć uwagę, że niestandardowy operator Include, wcześniej zaimplementowane będzie działać bez zmian. Metoda FindById z repozytorium usuwa zduplikowane logiki z akcji próby pobrania pojedynczej jednostki.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Nie ma znaczące różnic w testowania dwa podejścia, w których firma Microsoft zostały zbadane. Zapewniamy fałszywych implementacje IRepository&lt;T&gt; przez utworzenie klas konkretnych wspierana przez zestaw HashSet&lt;pracowników&gt; — podobnie jak zrobiliśmy w ostatniej sekcji. Jednak niektórzy deweloperzy wolą używać makiety obiektów i testowanie struktur obiektu zamiast tworzenia elementów sztucznych. Zapoznamy się przy użyciu mocks do testowania naszej implementacji i omówiono różnice między mocks i sztucznych elementów w następnej sekcji.

### <a name="testing-with-mocks"></a>Testowanie za pomocą Mocks

Istnieją różne metody do tworzenia wywołań jakie Martina Fowlera "test podwójnego". Test podwójnego (na przykład stunt filmu double) jest obiektem tworzonych "występować w" rzeczywiste, obiekty w środowisku produkcyjnym podczas testów. Repozytoriów w pamięci, którą utworzyliśmy są testu wartości podwójnej precyzji dla repozytoriów, komunikujące się z programu SQL Server. Pokazaliśmy już, jak za pomocą tych podwaja testów podczas testów jednostkowych izolowania kodu i zachować testów przeprowadzanych codziennie przez szybkie.

Badanie wartości podwójnej precyzji, którą utworzyliśmy ma implementacje rzeczywistych, praca. W tle każdego z nich przechowuje kolekcję konkretnych obiektów i będą oni dodawać i Usuń obiekty z tej kolekcji, jak możemy manipulować repozytorium podczas testu. Niektórzy deweloperzy, takich jak tworzenie ich podwaja się test temu — przy użyciu rzeczywistego kodu i implementacje pracy.  Te testu wartości podwójnej precyzji są tak zwany *elementów sztucznych*. Mają one implementacje pracy, ale nie są prawdziwe, do użytku produkcyjnego. Repozytorium fałszywych faktycznie nie zapisu w bazie danych. Serwer SMTP fałszywych faktycznie nie Wyślij wiadomość e-mail za pośrednictwem sieci.

### <a name="mocks-versus-fakes"></a>Mocks w porównaniu z elementów sztucznych

Istnieje inny typ testu double nazywane *testowanie*. Chociaż elementów sztucznych implementacje pracy, mocks są dostarczane z implementacji. Za pomocą framework makiety obiektu możemy konstruowania tych makiety obiektów w czasie wykonywania i używać ich jako wartości podwójnej precyzji testu. W tej sekcji będziemy używać "open source" pozorowanie framework Moq. Poniżej przedstawiono prosty przykład użycia Moq umożliwia dynamiczne tworzenie test podwójnego repozytorium pracownika.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Poprosimy Moq dla IRepository&lt;pracowników&gt; implementacji i tworzy jeden dynamicznie. Firma Microsoft może uzyskać dostęp do obiekt implementujący IRepository&lt;pracowników&gt; , uzyskując dostęp do właściwości obiektu pozorny&lt;T&gt; obiektu. Jest ten obiekt wewnętrzny, które możemy przekazać do naszych kontrolerów, a nie będą wiedzieli, jeśli jest to test podwójnego lub rzeczywistego repozytorium. Firma Microsoft wywoływać metody na obiekt, tak samo, jak firma Microsoft będzie wywoływać metody na obiekt z rzeczywistej implementacji.

Możesz się zastanawiać, co makiety repozytorium zrobić po możemy wywołać metody Add. Ponieważ nie ma żadnej implementacji za makiety obiektu, Dodaj nic nie robi. Brak konkretnego kolekcji w tle, takie jak mieliśmy z substytutami, którą napisaliśmy, więc jest odrzucana pracownika. Jak wygląda wartość zwracaną FindById? W tym przypadku makiety obiektu jest jedyną czynnością, którą wykonania, która jest zwracana wartość domyślną. Ponieważ firma Microsoft jest zwracany typ odwołania (pracownika), wartość zwracana jest wartość null.

Mocks może dźwiękowych bezwartościowe; Istnieją jednak dwa więcej funkcji mocks, które jeszcze nie Omówiliśmy. Po pierwsze Moq framework rejestruje wszystkie wywołania makiety obiektu. W dalszej części kodu możemy zadawać Moq, czy każdy użytkownik wywołał metodę Add, czy każdy użytkownik wywołał metodę FindById. Zobaczymy, pokażemy, jak możemy użyć tej funkcji "czarne pole" Rejestrowanie w testach.

Druga funkcja doskonałe jest, jak możemy użyć Moq program makiety obiektu z *oczekiwania*. Oczekiwanie informuje makiety obiektu, jak reagować na interakcji z danym. Na przykład możemy zaprogramować Oczekiwanie w naszym pozorny i stwierdzenie, aby zwrócić obiekt pracowników, gdy ktoś wywołuje FindById. Środowisko Moq wykorzystuje interfejs API konfiguracji i wyrażeń lambda z programem tego oczekiwania.

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

W tym przykładzie prosimy Moq dynamicznie Tworzenie repozytorium, a następnie będziemy programować repozytorium z oczekiwaniami. Oczekuje informuje makiety obiekt do zwrotu nowy obiekt pracowników z wartością identyfikatora 5, gdy ktoś wywołuje metodę FindById, przekazując wartość 5. Ten test zakończy się pomyślnie, a firma Microsoft nie trzeba tworzyć pełną implementację do IRepository fałszywych&lt;T&gt;.

Spróbujmy ponownie testy, którą napisaliśmy wcześniej i poprawkami, a ich do używania mocks zamiast elementów sztucznych. Podobnie jak wcześniej, użyjemy klasę bazową można skonfigurować wspólne elementy infrastruktury potrzebnych dla wszystkich kontrolera testów.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

Kod ustawienia pozostają takie same. Zamiast korzystać z elementów sztucznych, użyjemy Moq do konstruowania obiektów makiety. Klasa bazowa organizuje makiety jednostka pracy do zwrócenia makiety repozytorium, gdy kod wywołuje właściwość pracowników. Pozostała konfiguracja makiety odbędzie się wewnątrz świetlnymi testów przeznaczonych dla każdego konkretnego scenariusza. Na przykład warunki początkowe testu dla akcji indeksu skonfiguruje makiety repozytorium, aby zwrócić listę pracowników, gdy akcja wywołuje metodę FindAll makiety repozytorium.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

Z wyjątkiem oczekiwania Nasze testy wyglądać podobnie do testów, które Musieliśmy przed. Jednak dzięki możliwości rejestrowania makiety Framework możemy podejście, testowanie pod kątem różnych. Omówimy to Nowa perspektywa w następnej sekcji.

### <a name="state-versus-interaction-testing"></a>Stan i testowanie interakcji

Istnieją różne techniki, które można użyć do testowania oprogramowania za pomocą makiety obiektów. Jednym z podejść jest stanu na podstawie badań, czyli co możemy zostały wykonane w tym dokumencie do tej pory. Stan oparty testowania potwierdzenia sprawia, że o stanie tego oprogramowania. W ostatni test firma Microsoft wywoływane metody akcji kontrolera, a wprowadzone potwierdzenie o modelu, należy go tworzyć. Poniżej przedstawiono kilka przykładów stanu testów:

-   Sprawdź, czy repozytorium zawiera nowy obiekt pracownika po wykonaniu Utwórz.
-   Sprawdź, czy model zawiera listę wszystkich pracowników, po wykonaniu indeksu.
-   Sprawdź, czy po wykonaniu Usuwanie repozytorium nie zawiera danego pracownika.

Innym podejściem zostanie wyświetlony z makiety obiektów jest sprawdzenie *interakcje*. Gdy stan na podstawie testowania potwierdzenia sprawia, że o stanie obiektów, interakcji na podstawie testowania potwierdzenia sprawia, że dotyczące sposobu interakcji obiektów. Na przykład:

-   Sprawdź, czy kontroler wywołuje metody Add z repozytorium, gdy wykonuje Utwórz.
-   Sprawdź, czy kontroler wywołuje metodę FindAll z repozytorium, gdy indeks.
-   Sprawdź, czy kontroler wywołuje jednostki szkoły wywołano metody Commit można zapisać zmian, gdy wykonuje edycji.

Często testowania interakcji wymaga mniejszej ilości danych testu, ponieważ firma Microsoft nie są wywiercenie wewnątrz kolekcji i weryfikowanie liczby. Na przykład gdy wiemy, że akcja szczegóły wywołuje metodę FindById repozytorium przy użyciu poprawnej wartości - następnie akcji jest prawdopodobnie działa prawidłowo. Bez konfigurowania żadnych danych testowych do zwrócenia z FindById, możemy sprawdzić to zachowanie.

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

Tylko ustawienia wymagane w powyższe warunki początkowe testu jest Instalator dostarczonych przez klasę bazową. Gdy firma Microsoft wywołanie akcji kontrolera, Moq zarejestruje interakcje makiety repozytorium. Za pomocą interfejsu API Sprawdź Moq, możemy zadawać Moq Jeśli kontroler wywołana FindById przy użyciu prawidłowego wartość Identyfikatora. Jeśli kontrolera nie wywołała metodę lub wywołano metodę z wartością parametru nieoczekiwany, sprawdź, czy metoda zgłosi wyjątek, a test zakończy się niepowodzeniem.

Oto inny przykład, aby sprawdzić, czy akcja Utwórz wywołuje zatwierdzenia dla bieżącej jednostki pracy.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Jeden zagrożenia z testowaniem interakcji jest tendencja za pośrednictwem Określ interakcje. Możliwość makiety obiektu do rejestrowania i sprawdź, czy każdy interakcja z makiety obiektu nie oznacza testu starać się zweryfikować każdej interakcji. Niektóre interakcje są szczegóły dotyczące implementacji i interakcji należy zweryfikować tylko *wymagane* do zaspokojenia bieżącego testu.

Wybór między mocks lub elementów sztucznych dużym stopniu zależy od systemu, które testujesz i osobistej (lub zespole kart) Preferencje. Obiekty makiety może znacznie zmniejszyć ilość kodu, musisz wdrożyć test wartości podwójnej precyzji, ale nie wszyscy jest komfortowo, jednocześnie programowania oczekiwań i weryfikowanie interakcje.

## <a name="conclusions"></a>Wnioski

W tym dokumencie firma Microsoft została przedstawiona różne podejścia do tworzenia kodu sprawdzalnego działa zgodnie z podczas korzystania z programu Entity Framework ADO.NET dla funkcji trwałości danych. Firma Microsoft mogą korzystać z wbudowanymi abstrakcji takich jak IObjectSet&lt;T&gt;, lub utworzyć własne elementy abstrakcji takich jak IRepository&lt;T&gt;.  W obu przypadkach te elementy abstrakcji konsumentom stale trwały dzięki obsłudze POCO w ADO.NET Entity Framework 4.0, zakresu i wysoce testowalna. Dodatkowe funkcje EF4, takich jak niejawna powolne ładowanie umożliwia usłudze biznesowych kodu do pracy bez konieczności martwienia się o szczegółowe informacje o relacyjnego magazynu danych. Na koniec abstrakcji, tworzonej przez nas są łatwe do makiety lub fałszywe wewnątrz testów jednostkowych i możemy użyć tych testów wartości podwójnej precyzji do osiągnięcia szybkiego uruchamiania, wysoce izolowane i niezawodne testy.

### <a name="additional-resources"></a>Dodatkowe zasoby

-   Robert C. Martin, " [zasady pojedynczej odpowiedzialności](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martina Fowlera [katalog wzorców](http://www.martinfowler.com/eaaCatalog/index.html) z *wzorców architektury aplikacji przedsiębiorstwa*
-   Griffin Caprio " [wstrzykiwanie zależności](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Grupa dyskusyjna danych, " [Instruktaż: testów opartych na tworzenie aplikacji przy użyciu programu Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Grupa dyskusyjna danych, " [wzorców przy użyciu repozytorium i jednostki pracy za pomocą programu Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels " [wprowadzenie BDD](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen " [wprowadzenie do specyfikacji maszyny](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee " [BDD narzędziu MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [projektowania opartego na domenach](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martina Fowlera " [Mocks nie są wycinków](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martina Fowlera " [Test podwójnego](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller, Dyrektor ds " [stanu i testowanie interakcji](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>Biografia

Scott Allen jest członek personelu technicznego w firmie Pluralsight i założycielem OdeToCode.com. 15 lat Wytwarzanie oprogramowania komercyjnego Scott pracował nad rozwiązania związane z 8-bitową urządzeń osadzonych do wysoce skalowalnych aplikacji sieci web platformy ASP.NET. Możesz docierać do Scotta na swoim blogu w OdeToCode lub Matta na Twitterze [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).

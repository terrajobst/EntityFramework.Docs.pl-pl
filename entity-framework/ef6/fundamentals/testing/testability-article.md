---
title: Testowanie i Entity Framework 4,0 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181577"
---
# <a name="testability-and-entity-framework-40"></a>Testowanie i Entity Framework 4,0
Scott

Publikacj Maj 2010

## <a name="introduction"></a>Wprowadzenie

W tym dokumencie opisano i pokazano, jak napisać kod weryfikowalne z ADO.NET Entity Framework 4,0 i Visual Studio 2010. Ten dokument nie próbuje skupić się na określonej metodologii testowania, na przykład w przypadku projektowania opartego na testach (TDD) lub projektu opartego na zachowaniach (BDD). Zamiast tego ten dokument koncentruje się na sposobach pisania kodu, który korzysta z ADO.NET Entity Framework jeszcze łatwo izolować i przetestować w zautomatyzowany sposób. Zapoznajmy się z typowymi wzorcami projektowymi, które ułatwiają testowanie scenariuszy dostępu do danych, i zobacz, jak zastosować te wzorce podczas korzystania z platformy. Poszukajmy również określonych funkcji platformy, aby zobaczyć, jak te funkcje mogą funkcjonować w kodzie weryfikowalne.

## <a name="what-is-testable-code"></a>Co to jest kod weryfikowalne?

Możliwość weryfikowania oprogramowania przy użyciu zautomatyzowanych testów jednostkowych oferuje wiele pożądanych korzyści. Każdy wie, że dobre testy zmniejszają liczbę wad oprogramowania w aplikacji i zwiększają jakość aplikacji, ale ich testy jednostkowe nie przechodzą znacznie poza znalezieniem błędów.

Dobry zestaw testów jednostkowych pozwala zespołowi programistycznemu zaoszczędzić czas i zachować kontrolę nad tworzonym przez nie oprogramowaniem. Zespół może wprowadzać zmiany w istniejącym kodzie, refaktoryzacji, przeprojektowaniu i restrukturyzacji oprogramowania w celu spełnienia nowych wymagań oraz dodawać nowe składniki do aplikacji, jednocześnie wiedząc, że zestaw testów może zweryfikować zachowanie aplikacji. Testy jednostkowe są częścią szybkiego cyklu opinii, aby ułatwić zmianę i zachować łatwość utrzymania oprogramowania w miarę wzrostu złożoności.

Jednak testy jednostkowe są dostarczane z ceną. Zespół musi zainwestować czas, aby utworzyć i zachować testy jednostkowe. Wielkość nakładu pracy wymaganego do utworzenia tych testów jest bezpośrednio związana z **testowaniem** podstawowego oprogramowania. Jak łatwe jest przetestowanie oprogramowania? Zespół, który opracowuje oprogramowanie z myślą o testowaniu, będzie tworzyć skuteczne testy szybciej niż zespół pracujący z oprogramowaniem weryfikowalne.

Firma Microsoft zaprojektowała ADO.NET Entity Framework 4,0 (EF4) z myślą o testowaniu. Nie oznacza to, że deweloperzy będą pisać testy jednostkowe względem samego kodu struktury. Zamiast tego cele testowania dla EF4 ułatwiają tworzenie kodu weryfikowalne, który kompiluje się na podstawie struktury. Przed przystąpieniem do określonych przykładów wartościowa się zrozumienie jakości kodu weryfikowalne.

### <a name="the-qualities-of-testable-code"></a>Jakość kodu weryfikowalne

Kod, który jest łatwy do przetestowania, zawsze wykazuje co najmniej dwie cechy. Najpierw kod weryfikowalne jest łatwy do **obserwowania**. W przypadku niektórych zestawów danych wejściowych powinno być łatwe przestrzeganie danych wyjściowych kodu. Na przykład testowanie następującej metody jest proste, ponieważ metoda bezpośrednio zwraca wynik obliczenia.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Testowanie metody jest trudne, jeśli metoda zapisuje obliczoną wartość w gnieździe sieciowym, tabeli bazy danych lub pliku, takim jak poniższy kod. Test musi wykonać dodatkową prace, aby pobrać wartość.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Po drugie, kod weryfikowalne jest łatwo **odizolowany**. Użyjmy poniższego pseudo kodu jako nieprawidłowego przykładu kodu weryfikowalne.

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

Metoda jest łatwa do obserwowania — możemy przekazać zasady ubezpieczenia i sprawdzić, czy wartość zwracana jest zgodna z oczekiwanym wynikiem. Jednak w celu przetestowania metody należy zainstalować bazę danych z odpowiednim schematem i skonfigurować serwer SMTP na wypadek próby wysłania wiadomości e-mail przez metodę.

Test jednostkowy chce jedynie sprawdzić logikę obliczeń wewnątrz metody, ale test może się nie powieść, ponieważ serwer poczty e-mail jest w trybie offline lub serwer bazy danych został przeniesiony. Obie te błędy nie są związane z zachowaniem, które test chce zweryfikować. Zachowanie jest trudne do odizolowania.

Deweloperzy oprogramowania, którzy dążą do pisania kodu weryfikowalne często dążą do utrzymania rozdzielenia problemów w kodzie, który pisze. Powyższa metoda powinna skupić się na obliczeniach firmy i delegować szczegóły implementacji bazy danych i wiadomości e-mail do innych składników. Robert C. Martin wywołuje tę samą regułę odpowiedzialności. Obiekt powinien hermetyzować jedną z wąskich obowiązków, takich jak obliczanie wartości zasad. Wszystkie inne bazy danych i służbowe powiadomienia powinny być odpowiedzialne za inne obiekty. Kod zapisany w ten sposób jest łatwiejszy do odizolowania, ponieważ koncentruje się na pojedynczym zadaniu.

W programie .NET mamy streszczenia, które muszą przestrzegać jednej zasady odpowiedzialności i uzyskać izolację. Możemy użyć definicji interfejsu i wymusić użycie przez kod abstrakcji interfejsu zamiast konkretnego typu. W dalszej części tego dokumentu zobaczymy, jak metoda, taka jak niewłaściwy przykład przedstawiony powyżej, może współdziałać z interfejsami, które *wyglądają* podobnie do bazy danych. W czasie testu można jednak zastąpić implementację fikcyjną, która nie komunikuje się z bazą danych, ale zamiast tego przechowuje dane w pamięci. Ta implementacja fikcyjna izoluje kod z niepowiązanych problemów w kodzie dostępu do danych lub w konfiguracji bazy danych.

Istnieje dodatkowe korzyści związane z izolacją. Obliczenia biznesowe w ostatniej metodzie powinny trwać tylko kilka milisekund, ale test może być wykonywany przez kilka sekund w miarę przeskoków kodu między siecią i rozmowy z różnymi serwerami. Testy jednostkowe powinny działać szybko, aby ułatwić małym zmianom. Testy jednostkowe powinny również być powtarzane i kończyć się niepowodzeniem, ponieważ wystąpił problem ze składnikiem niezwiązanym z testem. Pisanie kodu, który jest łatwy do obserwowania i wyodrębnienia oznacza, że deweloperzy będą mieli łatwiejszy czas na zapisanie testów dla kodu, poświęcasz mniej czasu na przeprowadzenie testów i co ważniejsze, Poświęcaj mniej czasu na błędy śledzenia błędów, które nie istnieją.

Miejmy nadzieję można dowiedzieć się, jakie są korzyści z testowania i poznać jakości, które weryfikowalne kod. Zamierzamy się dowiedzieć, jak napisać kod, który współpracuje z usługą EF4, aby zapisać dane w bazie danych, a pozostało zauważalne i łatwe do odizolowania, ale najpierw zawężamy nasz skup, aby omówić projekty weryfikowalne na potrzeby dostępu do danych.

## <a name="design-patterns-for-data-persistence"></a>Wzorce projektowe dla trwałości danych

Oba z nieprawidłowych przykładów przedstawionych wcześniej miały zbyt wiele obowiązków. Pierwszy zły przykład wymagał wykonania obliczeń *i* zapisu w pliku. Drugi zły przykład wymagał odczytania danych z bazy danych *i* wykonania obliczeń w firmie *oraz* wysłania wiadomości e-mail. Przez projektowanie mniejszych metod, które dzielą się problemami i delegowanie odpowiedzialności za inne składniki, będziesz mieć wspaniałe podejścia do pisania kodu weryfikowalne. Celem jest tworzenie funkcji przez redagowanie akcji z małych i ukierunkowanych abstrakcji.

Gdy chodzi o trwałość danych, są to popularne i uporządkowane abstrakcje, więc są one takie same, jak w przypadku wzorców projektowych. Wzorce książek Fowleraowych z rozliczeniami w przedsiębiorstwie były pierwszym działaniem opisującym te wzorce na wydruku. Udostępnimy krótkie opisy tych wzorców w poniższych sekcjach, zanim pokażemy, jak te ADO.NET Entity Framework implementują i współdziałają z tymi wzorcami.

### <a name="the-repository-pattern"></a>Wzorzec repozytorium

Fowlera mówi repozytorium "koryguje między warstwami mapowania domeny i danych przy użyciu interfejsu przypominającego gromadzenie do uzyskiwania dostępu do obiektów domeny". Celem wzorca repozytorium jest odizolowanie kodu od minutiae dostępu do danych, a w przypadku wcześniejszej izolacji jest wymagana cecha do testowania.

Klucz odizolowany polega na tym, jak repozytorium uwidacznia obiekty przy użyciu interfejsu podobnej do kolekcji. Logika, którą zapisujesz do korzystania z repozytorium, nie ma znaczenia, w jaki sposób repozytorium będzie zmaterializowania żądane obiekty. Repozytorium może komunikować się z bazą danych lub po prostu zwraca obiekty z kolekcji w pamięci. Każdy kod musi wiedzieć, że repozytorium jest utrzymywane do obsługi kolekcji i można pobrać, dodać i usunąć obiekty z kolekcji.

W istniejących aplikacjach .NET konkretne repozytorium często dziedziczy z interfejsu generycznego, takiego jak następujące:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Wprowadzimy kilka zmian w definicji interfejsu, gdy udostępnimy implementację EF4, ale podstawowa koncepcja pozostaje taka sama. Kod może użyć konkretnego repozytorium implementującego ten interfejs, aby pobrać jednostkę według wartości klucza podstawowego, pobrać kolekcję jednostek na podstawie oceny predykatu lub po prostu pobrać wszystkie dostępne jednostki. Kod może również dodawać i usuwać jednostki za pomocą interfejsu repozytorium.

Mając IRepository obiektów pracowników, kod może wykonać następujące operacje.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Ponieważ kod używa interfejsu (IRepository pracownika), możemy udostępnić kod z różnymi implementacjami interfejsu. Jedną z implementacji może być implementacja EF4 i utrwalanie obiektów w bazie danych Microsoft SQL Server. Inna implementacja (używana podczas testowania) może być obsługiwana przez listę obiektów pracowników w pamięci. Interfejs pomoże uzyskać izolację w kodzie.

Zwróć uwagę, że interfejs IRepository @ no__t-0T @ no__t-1 nie ujawnia operacji zapisywania. Jak aktualizować istniejące obiekty? Mogą występować w definicjach IRepository, które obejmują operację zapisywania, a implementacje tych repozytoriów będą musiały natychmiast utrzymać obiekt w bazie danych. Jednak w wielu aplikacjach nie chcemy, aby obiekty były utrwalane pojedynczo. Zamiast tego chcemy przenieść obiekty do życia, prawdopodobnie z różnych repozytoriów, zmodyfikować te obiekty jako część działania biznesowego, a następnie utrwalać wszystkie obiekty w ramach jednej, niepodzielnej operacji. Na szczęście istnieje wzorzec zezwalający na zachowanie tego typu.

### <a name="the-unit-of-work-pattern"></a>Wzorzec jednostki pracy

Fowlera oznacza, że jednostka pracy będzie obsługiwać listę obiektów, na które ma wpływ transakcja biznesowa, i koordynuje wpisywanie zmian i rozwiązywanie problemów współbieżności. Jest odpowiedzialna za jednostkę pracy, która śledzi zmiany w obiektach, które doprowadzamy do życia z repozytorium, i utrzymuje wszelkie zmiany wprowadzone w obiektach, gdy poinformujemy o jednostce pracy, aby zatwierdzić zmiany. Jest również odpowiedzialna za jednostkę pracy, która zajmie się nowymi obiektami, które zostały dodane do wszystkich repozytoriów i wstawia obiekty do bazy danych, a także do zarządzania usuwaniem.

Jeśli kiedykolwiek dojdziesz do pracy z zestawami danych ADO.NET, zobaczysz, że masz już doświadczenie ze wzorca jednostki pracy. Zestawy danych ADO.NET umożliwiają śledzenie naszych aktualizacji, usunięć i wstawiania obiektów DataRow i mogą być (za pomocą TableAdapter) uzgadniają wszystkie nasze zmiany w bazie danych. Jednak model obiektów DataSet ma odłączony podzestaw źródłowej bazy danych. Wzorzec jednostki pracy ma takie samo zachowanie, ale współpracuje z obiektami biznesowymi i obiektami domen, które są izolowane od kodu dostępu do danych i nie wiedząc bazy danych.

Streszczenie modelu jednostki pracy w kodzie .NET może wyglądać następująco:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Przez ujawnienie odwołań do repozytorium z jednostki pracy możemy zapewnić, że pojedynczy obiekt jednostki pracy ma możliwość śledzenia wszystkich jednostek w ramach transakcji biznesowej. Implementacja metody zatwierdzania dla rzeczywistej jednostki pracy polega na tym, że wszystko jest wykonywane w celu uzgodnienia zmian w pamięci z bazą danych. 

Mając odwołanie IUnitOfWork, kod może wprowadzać zmiany w obiektach biznesowych pobieranych z jednego lub większej liczby repozytoriów i zapisywać wszystkie zmiany przy użyciu niepodzielnej operacji zatwierdzania.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Wzorzec obciążenia z opóźnieniem

Fowlera używa załadowania nazwy z opóźnieniem do opisywania "obiektu, który nie zawiera wszystkich potrzebnych danych, ale wie, jak to zrobić". Przezroczyste ładowanie z opóźnieniem jest ważną funkcją do tworzenia kodu biznesowego weryfikowalne i pracy z relacyjną bazą danych. Na przykład rozważmy poniższy kod.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Jak jest wypełniona kolekcja TimeCards? Istnieją dwie możliwe odpowiedzi. Jedną z odpowiedzi jest to, że repozytorium pracowników, gdy zostanie wyświetlony monit o pobranie pracownika, generuje zapytanie w celu pobrania pracownika wraz z informacjami o karcie czasowej skojarzonym z pracownikami. W relacyjnych bazach danych zwykle wymaga zapytania z klauzulą JOIN i może spowodować pobranie większej ilości informacji niż wymaga aplikacji. Co zrobić, jeśli aplikacja nigdy nie musi dotykać właściwości TimeCards?

Drugą odpowiedzią jest załadowanie właściwości TimeCards "na żądanie". To ładowanie z opóźnieniem jest niejawne i niewidoczne dla logiki biznesowej, ponieważ kod nie wywołuje specjalnych interfejsów API w celu pobrania informacji o karcie czasowej. Kod przyjmuje, że informacje o karcie czasowej są obecne, gdy jest to zajdzie taka potrzeba. Istnieje pewna magiczna Metoda ładowania z opóźnieniem, która zwykle obejmuje przechwycenie wywołania metody przez środowisko uruchomieniowe. Kod przechwytywania jest odpowiedzialny za rozmowę z bazą danych i pobieranie informacji o karcie czasu, pozostawiając logikę biznesową bezpłatnie do logiki biznesowej. Ten opóźniony magiczny ciąż umożliwia kod firmy odizolowanie od operacji pobierania danych i daje większy kod weryfikowalne.

Wadą do obciążenia z opóźnieniem jest to, że gdy *aplikacja wymaga* informacji o karcie czasowej, kod wykona dodatkowe zapytanie. Nie jest to istotne w przypadku wielu aplikacji, ale w przypadku aplikacji i aplikacji wrażliwych na wydajność przez wiele obiektów pracowników i wykonywania zapytania w celu pobrania kart czasu podczas każdej iteracji pętli (problem często określa się jako N + 1) problem z kwerendą), ładowanie z opóźnieniem jest przeciągane. W tych scenariuszach aplikacja może chcieć eagerly dane karty czasu ładowania w najbardziej efektywny sposób.

Na szczęście zobaczymy, jak EF4 obsługuje zarówno niejawne obciążenia z opóźnieniem, jak i wydajne obciążenia eager podczas przechodzenia do następnej sekcji i implementują te wzorce.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementowanie wzorców przy użyciu Entity Framework

Dobra wiadomość polega na tym, że wszystkie wzorce projektowe opisane w ostatniej sekcji są proste do wdrożenia z EF4. Aby udowodnić, że będziemy używać prostej aplikacji ASP.NET MVC do edytowania i wyświetlania pracowników i skojarzonych z nimi informacji o karcie czasowej. Zaczniemy od następującej "zwykłych starych obiektów CLR" (POCOs). 

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

Te definicje klas zmienią się nieco w miarę eksplorowania różnych metod i funkcji EF4, ale celem jest zachowanie tych klas jako trwałości ignorujących (PI), jak to możliwe. Obiekt PI nie wie, *jak*, a nawet *czy*stan, w którym znajduje się w bazie danych. PI i POCOs są dostępne z oprogramowaniem weryfikowalne. Obiekty korzystające z podejścia POCO są mniej ograniczone, bardziej elastyczne i łatwiejsze do testowania, ponieważ mogą działać bez bazy danych.

Korzystając z POCOs w miejscu, możemy utworzyć Entity Data Model (EDM) w programie Visual Studio (patrz rysunek 1). Nie będziemy używać modelu EDM do generowania kodu dla naszych jednostek. Zamiast tego chcemy używać jednostek, które Lovingly łodzi. Będziemy używać modelu EDM tylko do generowania schematu bazy danych i dostarczania metadanych EF4 potrzebnych do mapowania obiektów na bazę danych.

![Dr test_01](~/ef6/media/eftest-01.jpg)

**Rysunek 1**

Uwaga: Jeśli chcesz najpierw opracować model modelu EDM, możliwe jest wygenerowanie czystego, POCOego kodu z modelu EDM. Można to zrobić za pomocą rozszerzenia programu Visual Studio 2010 dostarczonego przez zespół obsługujący programowanie danych. Aby pobrać rozszerzenie, uruchom Menedżera rozszerzeń z menu Narzędzia w programie Visual Studio i Przeszukaj galerię online szablonów dla "POCO" (patrz rysunek 2). Istnieje kilka szablonów POCO dostępnych dla EF. Aby uzyskać więcej informacji na temat używania szablonu, zobacz "[Walkthrough: Szablon POCO dla Entity Framework @ no__t-0 ".

![Dr test_02](~/ef6/media/eftest-02.png)

**Rysunek 2**

Z tego POCOego punktu początkowego będziemy eksplorować dwa różne podejścia do kodu weryfikowalne. Pierwsze podejście wywołuje podejście EF, ponieważ wykorzystuje abstrakcje z interfejsu API Entity Framework do implementowania jednostek pracy i repozytoriów. W drugim podejściu utworzymy własne abstrakcyjne streszczenie repozytorium, a następnie zobaczysz zalety i wady każdego podejścia. Zaczniemy od poznawania metody EF.  

### <a name="an-ef-centric-implementation"></a>Implementacja skoncentrowana na EF

Rozważmy następującą akcję kontrolera z projektu ASP.NET MVC. Akcja pobiera obiekt Employee i zwraca wynik, aby wyświetlić szczegółowy widok pracownika.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Czy kod jest weryfikowalne? Istnieją co najmniej dwa testy wymagające zweryfikowania zachowania działania. Najpierw chcemy sprawdzić, czy akcja zwraca poprawny widok — prosty test. Chcemy również napisać test, aby sprawdzić, czy akcja pobiera odpowiedniego pracownika, i chcemy to zrobić bez wykonywania kodu w celu wykonania zapytania względem bazy danych. Pamiętaj, że chcemy odizolować kod pod testem. Izolacja zapewni, że test nie powiedzie się z powodu błędu w kodzie dostępu do danych lub konfiguracji bazy danych. Jeśli test zakończy się niepowodzeniem, firma Microsoft wie, że mamy usterkę w logice kontrolera, a nie w pewnym składniku systemu niższego poziomu.

Aby uzyskać izolację, będziemy potrzebować pewnych streszczeń, takich jak interfejsy przedstawione wcześniej dla repozytoriów i jednostek pracy. Należy pamiętać, że wzorzec repozytorium jest przeznaczony do korygowania między obiektami domeny a warstwą mapowania danych. W tym scenariuszu EF4 *jest* warstwa mapowania danych i zawiera już abstrakcję podobną do repozytorium o nazwie IObjectSet @ No__t-1T @ no__t-2 (z przestrzeni nazw System. Data. Objects). Definicja interfejsu wygląda następująco.

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

IObjectSet @ no__t-0T @ no__t-1 spełnia wymagania dotyczące repozytorium, ponieważ przypomina kolekcję obiektów (za pośrednictwem interfejsu IEnumerable @ no__t-2T @ no__t-3) i udostępnia metody dodawania i usuwania obiektów z symulowanej kolekcji. Metody dołączania i odłączania uwidaczniają dodatkowe możliwości interfejsu API EF4. Aby użyć IObjectSet @ no__t-0T @ no__t-1 jako interfejsu dla repozytoriów, potrzebujemy abstrakcji jednostek pracy, aby powiązać repozytoria ze sobą.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Jedna konkretna implementacja tego interfejsu będzie komunikować się z SQL Server i będzie łatwa do tworzenia przy użyciu klasy ObjectContext z EF4. Klasa ObjectContext jest rzeczywistą jednostką pracy w interfejsie API EF4.

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

IObjectSet @ no__t-0T @ no__t-1 do życia jest tak proste jak wywołanie metody metody CreateObjectSet obiektu ObjectContext. W tle środowisko Framework będzie używać metadanych dostarczonych w modelu EDM, aby utworzyć konkretny obiekt ObjectSet @ no__t-0T @ no__t-1. Nastąpi powrót do IObjectSet @ no__t-0T @ no__t-1 interfejsu, ponieważ pomoże to w zachowaniu testów w kodzie klienta.

Ta konkretna implementacja jest przydatna w środowisku produkcyjnym, ale musimy skupić się na tym, jak będziemy korzystać z naszego abstrakcji IUnitOfWork w celu ułatwienia testowania.

### <a name="the-test-doubles"></a>Test podwaja się

Aby wyizolować akcję kontrolera, potrzebna jest możliwość przełączenia między rzeczywistą jednostką pracy (za pomocą obiektu ObjectContext) i testu podwójnego lub "fałszywego" jednostki pracy (operacje wykonywane w pamięci). Typowym podejściem do przeprowadzenia tego typu przełączenia jest to, że kontroler MVC nie pozwala na utworzenie wystąpienia jednostki pracy, ale zamiast tego przekazać jednostkę pracy do kontrolera jako parametr konstruktora.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Powyższy kod jest przykładem iniekcji zależności. Nie zezwolimy, aby kontroler utworzył zależność (jednostkę pracy), ale wstrzyknąć zależność do kontrolera. W projekcie MVC wspólne użycie fabryki kontrolerów niestandardowych w połączeniu z kontenerem Inversion of Control (IoC) w celu zautomatyzowania iniekcji zależności. Te tematy wykraczają poza zakres tego artykułu, ale więcej informacji można znaleźć, postępując zgodnie z odwołaniami na końcu tego artykułu.

Fałszywa implementacja pracy, której można użyć do testowania, może wyglądać następująco.

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

Zwróć uwagę na to, że sfałszowana jednostka pracy uwidacznia Właściwość zatwierdzona. Czasami warto dodać do fałszywej klasy funkcje, które ułatwiają testowanie. W takim przypadku można łatwo sprawdzić, czy kod zatwierdza jednostkę pracy, sprawdzając Właściwość zatwierdzona.

Potrzebujemy również fałszywych IObjectSet @ no__t-0T @ no__t-1 do przechowywania obiektów pracowników i godzin w pamięci. Możemy udostępnić jedną implementację przy użyciu typów ogólnych.

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

Ten test powoduje dwukrotne delegowanie większości zadań do bazowego obiektu HashSet — @ no__t-0T @ no__t-1. Należy zauważyć, że IObjectSet @ no__t-0T @ no__t-1 wymaga ograniczenia generycznego wymuszania T jako klasy (typ referencyjny), a także wymusza implementację interfejsu IQueryable @ no__t-2T @ no__t-3. Można łatwo utworzyć kolekcję w pamięci jako interfejs IQueryable @ no__t-0T @ no__t-1 przy użyciu standardowego operatora LINQ AsQueryable.

### <a name="the-tests"></a>Testy

Tradycyjne testy jednostkowe będą używały pojedynczej klasy testowej do przechowywania wszystkich testów dla wszystkich akcji w jednym kontrolerze MVC. Możemy napisać te testy lub dowolnego typu testu jednostkowego, używając w pamięci sztucznie wbudowanej klasy. Jednakże w tym artykule będziemy unikać podejścia do klasy testów monolitycznych, a zamiast tego grupować testy, aby skoncentrować się na określonej funkcji.  Na przykład "Utwórz nowego pracownika" może być funkcją, którą chcemy przetestować, więc użyjemy pojedynczej klasy testowej, aby zweryfikować akcję pojedynczego kontrolera odpowiedzialną za utworzenie nowego pracownika.

Istnieje kilka typowych kodów konfiguracji potrzebnych dla wszystkich tych precyzyjnych klas testowych. Na przykład zawsze musimy utworzyć repozytoria w pamięci i fałszywą jednostkę pracy. Potrzebujemy również wystąpienia kontrolera pracownika z pustą jednostką pracy wstrzykiwaną. Udostępnimy ten wspólny kod instalatora między klasami testów przy użyciu klasy bazowej.

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

"Matki" obiektu używanej w klasie bazowej jest jednym wspólnym wzorcem do tworzenia danych testowych. Obiekt matki zawiera metody fabryki do tworzenia wystąpień jednostek testowych do użycia w wielu zastosowaniach testów.

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

Możemy użyć EmployeeControllerTestBase jako klasy bazowej dla wielu armatur testowych (patrz rysunek 3). Każda Armatura testowa testuje określoną akcję kontrolera. Na przykład jedna Armatura testowa koncentruje się na testowaniu akcji tworzenia używanej podczas żądania HTTP GET (aby wyświetlić widok służący do tworzenia pracownika), a inna Armatura koncentruje się na akcji tworzenia używanej w żądaniu POST protokołu HTTP (Aby uzyskać informacje przesłane przez Użytkownik, aby utworzyć pracownika). Każda klasa pochodna jest odpowiedzialna za konfigurację wymaganą w określonym kontekście i aby zapewnić potwierdzenia, które są konieczne do zweryfikowania wyników dla danego kontekstu testu.

![Dr test_03](~/ef6/media/eftest-03.png)

**Rysunek 3**

Konwencja nazewnictwa i styl testu przedstawiony w tym miejscu nie są wymagane dla kodu weryfikowalne — jest to tylko jedno podejście. Na rysunku 4 przedstawiono testy działające w ramach wtyczki programu Test Runner dla programu Visual Studio 2010.

![Dr test_04](~/ef6/media/eftest-04.png)

**Rysunek 4**

Przy użyciu klasy bazowej do obsługi kodu konfiguracji udostępnionej testy jednostkowe dla każdej akcji kontrolera są małe i łatwe do zapisu. Testy będą wykonywane szybko (ponieważ wykonujemy operacje w pamięci) i nie powinny być przyczyną niepowodzenia ze względu na niepowiązaną infrastrukturę lub obawy dotyczące środowiska (ponieważ wyizolowano jednostkę testową).

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

W tych testach Klasa bazowa wykonuje większość czynności konfiguracyjnych. Należy pamiętać, że Konstruktor klasy bazowej tworzy repozytorium w pamięci, fałszywą jednostkę pracy i wystąpienie klasy EmployeeController. Klasa testowa pochodzi z tej klasy bazowej i koncentruje się na charakterystyce testów metody Create. W takim przypadku określone czynności są przetestowane w dół do kroków "Rozmieść, Act i Assert", które będą widoczne w dowolnej procedurze testowania jednostkowego:

-   Utwórz obiekt newEmployee, aby symulować przychodzące dane.
-   Wywołaj akcję tworzenia EmployeeController i przekaż ją do newEmployee.
-   Upewnij się, że akcja Utwórz powoduje uzyskanie oczekiwanych wyników (pracownik zostanie wyświetlony w repozytorium).

Utworzone przez nas elementy umożliwiają przetestowanie dowolnych akcji EmployeeController. Na przykład podczas pisania testów dla akcji indeksu kontrolera pracownika możemy dziedziczyć z klasy bazowej testu, aby określić tę samą konfigurację podstawową dla naszych testów. Ponownie Klasa bazowa spowoduje utworzenie repozytorium w pamięci, fałszywej jednostki pracy i wystąpienia EmployeeController. Testy akcji index muszą jedynie skupić się na wywoływaniu akcji index i przetestowaniu jakości modelu, który zwraca akcja.

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

Testy tworzone za pomocą elementów sztucznych w pamięci są ukierunkowane na testowanie *stanu* oprogramowania. Na przykład podczas testowania akcji tworzenia chcemy sprawdzić stan repozytorium po wykonaniu akcji tworzenia — czy repozytorium zawiera nowego pracownika?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Później przejdziemy do testowania opartego na interakcji. Testowanie oparte na interakcji będzie pytać, czy testowany kod wywołał odpowiednie metody dla naszych obiektów i przeszedł poprawne parametry. Na razie przejdziemy do drugiego wzorca projektowego — z opóźnieniem.

## <a name="eager-loading-and-lazy-loading"></a>Ładowanie eager i ładowanie z opóźnieniem

W pewnym momencie w aplikacji sieci Web ASP.NET MVC firma Microsoft może chcieć wyświetlić informacje pracownika i dołączyć karty czasu powiązane z pracownikami. Na przykład może być wyświetlany ekran podsumowania karty czasu, który pokazuje nazwisko pracownika i łączną liczbę kart czasu w systemie. Istnieje kilka metod zaimplementowania tej funkcji.

### <a name="projection"></a>Rzut

Jednym z prostych metod tworzenia podsumowania jest konstruowanie modelu przeznaczonego dla informacji, które chcemy wyświetlić w widoku. W tym scenariuszu model może wyglądać podobnie do poniższego.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Zwróć uwagę, że EmployeeSummaryViewModel nie jest jednostką, innymi słowy, nie ma czegoś do utrwalenia w bazie danych. Będziemy używać tej klasy tylko do losowego przechodzenia danych do widoku. Model widoku jest podobny do obiektu transferu danych (DTO), ponieważ nie zawiera żadnego zachowania (brak metod) — tylko właściwości. Właściwości będą zawierać dane, które muszą zostać przeniesione. Można łatwo utworzyć wystąpienie tego modelu widoku przy użyciu standardowego operatora rzutowania LINQ — operator SELECT.

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

Istnieją dwie istotne funkcje w powyższym kodzie. Pierwszy — kod jest łatwy do przetestowania, ponieważ jest nadal łatwo obserwować i izolować. Operator SELECT działa tak samo jak w przypadku elementów sztucznych w pamięci, tak jak w przypadku rzeczywistej jednostki pracy.

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

Druga istotna funkcja to sposób, w jaki kod umożliwia EF4om generowanie pojedynczego, wydajnego zapytania w celu zebrania informacji o pracownikach i kartach czasowych. Informacje o pracownikach i karcie czasu zostały załadowane do tego samego obiektu bez użycia żadnych specjalnych interfejsów API. Kod jedynie wyraził informacje wymagane przy użyciu standardowych operatorów LINQ, które pracują z źródłami danych w pamięci, a także ze zdalnymi źródłami danych. EF4 mógł przetłumaczyć drzewa wyrażeń wygenerowane przez zapytanie LINQ i kompilator C @ no__t-0 do jednego i wydajnego zapytania T-SQL.

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
         )  AS [Project1]
    )  AS [Limit1]
```

Istnieją inne sytuacje, w których nie chcemy współdziałać z modelem widoku ani obiektem DTO, ale z rzeczywistymi obiektami. Gdy wiemy, że potrzebujemy pracownika *i* kart czasu pracownika, możemy eagerly załadować powiązane dane w sposób niezależny i wydajny.

### <a name="explicit-eager-loading"></a>Jawne ładowanie eager

Jeśli chcemy eagerly informacje dotyczące jednostki związanej z ładowaniem, potrzebujemy pewnego mechanizmu logiki biznesowej (lub w tym scenariuszu, logiki akcji kontrolera) do wyrażania chęci do repozytorium. Klasa EF4 ObjectQuery @ no__t-0T @ no__t-1 definiuje metodę include, aby określić powiązane obiekty do pobrania podczas zapytania. Należy pamiętać, że EF4 ObjectContext ujawnia jednostki za pośrednictwem konkretnej klasy ObjectSet @ no__t-0T @ no__t-1, która dziedziczy z ObjectQuery @ no__t-2T @ no__t-3.  Jeśli w naszym działaniu kontrolera użyto obiektu ObjectSet @ no__t-0T @ no__t-1, możemy napisać następujący kod, aby określić eager obciążenie informacjami o karcie czasu dla każdego pracownika.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Jednak ze względu na to, że próbujemy zachować nasz kod weryfikowalne, nie ujawniamy obiektu ObjectSet @ no__t-0T @ no__t-1 spoza rzeczywistej jednostki pracy. Zamiast tego korzystamy z interfejsu IObjectSet @ no__t-0T @ no__t-1, który jest łatwiejszy do sfałszowania, ale IObjectSet @ no__t-2T @ no__t-3 nie definiuje metody include. Estetyki LINQ polega na tym, że możemy utworzyć własny operator include.

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

Zwróć uwagę, że ten operator include został zdefiniowany jako metoda rozszerzająca interfejs IQueryable @ no__t-0T @ no__t-1 zamiast IObjectSet @ no__t-2T @ no__t-3. Dzięki temu możemy korzystać z metody z szerszym zakresem możliwych typów, w tym IQueryable @ no__t-0T @ no__t-1, IObjectSet @ no__t-2T @ no__t-3, ObjectQuery @ no__t-4T @ no__t-5 i ObjectSet @ no__t-6T @ no__t-7. W przypadku, gdy podstawową sekwencją nie jest oryginalny EF4 ObjectQuery @ no__t-0T @ no__t-1, wówczas nie ma żadnej szkody i operator include to no-op. Jeśli podstawową sekwencją *jest* ObjectQuery @ No__t-1T @ no__t-2 (lub pochodna z ObjectQuery @ No__t-3T @ no__t-4), wówczas EF4 zobaczy nasze wymaganie dotyczące dodatkowych danych i formułuje odpowiednie zapytanie SQL.

Przy użyciu tego nowego operatora można jawnie zażądać eager ładowania informacji o karcie czasowej z repozytorium.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

W przypadku uruchomienia względem rzeczywistego obiektu ObjectContext kod generuje następujące pojedyncze zapytanie. Zapytanie zbiera wystarczające informacje z bazy danych w jednej podróży, aby zmaterializowania obiekty pracowników i w pełni wypełnić swoją właściwość TimeCards.

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

Doskonałe wiadomości to kod wewnątrz metody akcji, który pozostaje w pełni weryfikowalne. Nie musimy udostępniać żadnych dodatkowych funkcji dla naszych fałszywych, aby obsługiwać operator include. W przypadku nieprawidłowych wiadomości musimy użyć operatora include wewnątrz kodu, który chciał zachować trwałość ignorujących. Jest to podstawowy przykład typu kompromisów, które należy oszacować podczas kompilowania kodu weryfikowalne. Istnieją przypadki, w których konieczne jest poinformowanie o wycieku, poza abstrakcją repozytorium, aby osiągnąć cele wydajności.

Alternatywą dla ładowania eager jest ładowanie z opóźnieniem. Ładowanie z opóźnieniem oznacza, że nasz kod firmy *nie* jest potrzebny do jawnego ogłaszania wymagania dotyczącego skojarzonych danych. Zamiast tego używamy naszych jednostek w aplikacji, a jeśli potrzebujesz dodatkowych danych Entity Framework załadują dane na żądanie.

### <a name="lazy-loading"></a>Ładowanie z opóźnieniem

Można łatwo przystąpić do scenariusza, w którym nie wiemy, jakie dane będą potrzebne. Firma Microsoft może wiedzieć, że logika wymaga obiektu Employee, ale może odgałęziać się do różnych ścieżek wykonywania, w których niektóre z tych ścieżek wymagają informacji o karcie czasowej od pracownika, a niektóre nie. Takie scenariusze są idealnym rozwiązaniem dla niejawnego ładowania z opóźnieniem, ponieważ dane są wyświetlane w sposób niezbędny.

Ładowanie z opóźnieniem, znane także jako odroczone ładowanie, nakłada pewne wymagania dotyczące obiektów Entity. POCOs z prawdziwym ignorujących trwałości nie wpłynie na żadne wymagania z warstwy trwałości, ale prawdziwe ignorujących trwałości jest praktycznie niemożliwe.  Zamiast tego mierzę trwałość ignorujących w względnych stopniach. Jeśli konieczne jest dziedziczenie z klasy podstawowej zorientowanej na trwałość lub użycie wyspecjalizowanej kolekcji w celu osiągnięcia ładowania z opóźnieniem w POCOs, może to potrwać. Na szczęście EF4 ma mniej niepożądane rozwiązanie.

### <a name="virtually-undetectable"></a>Praktycznie niewykrywalny

W przypadku korzystania z obiektów POCO EF4 może dynamicznie generować serwery proxy środowiska uruchomieniowego dla jednostek. Te serwery proxy w niewidoczny sposób zawijają POCOs z materiałami i oferują dodatkowe usługi, przechwytuje każdą właściwość pobieranie i ustawianie w celu wykonywania dodatkowych czynności. Jedną z tych usług jest funkcja ładowania z opóźnieniem, którą szukamy. Inna usługa to wydajny mechanizm śledzenia zmian, który można rejestrować, gdy program zmienia wartości właściwości jednostki. Lista zmian jest używana przez obiekt ObjectContext w metodzie metody SaveChanges, aby zachować wszelkie zmodyfikowane jednostki za pomocą poleceń UPDATE.

Jednak aby te serwery proxy działały, muszą one mieć możliwość podłączania do właściwości operacji get i Set w jednostce, a serwery proxy osiągają ten cel przez zastępowanie wirtualnych elementów członkowskich. Z tego względu, jeśli chcemy niejawnie załadować z opóźnieniem i wydajnym śledzeniem zmian, musimy wrócić do naszych definicji klas POCO i oznaczyć właściwości jako wirtualne.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Nadal możemy powiedzieć, że jednostka Employee jest w większości ignorujących trwałość. Jedynym wymaganiem jest użycie wirtualnych elementów członkowskich. nie ma to wpływu na testowanie kodu. Nie musimy dziedziczyć z żadnej specjalnej klasy bazowej, a nawet użyć specjalnej kolekcji przeznaczonej do ładowania z opóźnieniem. Jak ilustruje kod, każda klasa implementująca interfejs ICollection @ no__t-0T @ no__t-1 jest dostępna do przechowywania powiązanych jednostek.

Istnieje również jedna niewielka zmiana, którą trzeba wprowadzić w naszej jednostce pracy. Ładowanie z opóźnieniem jest domyślnie *wyłączone* podczas pracy bezpośrednio z obiektem ObjectContext. Istnieje właściwość, którą można ustawić we właściwości ContextOptions, aby umożliwić ładowanie odroczone i można ustawić tę właściwość wewnątrz rzeczywistej jednostki pracy, jeśli chcemy włączyć ładowanie z opóźnieniem wszędzie.

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

Po włączeniu niejawnego ładowania z opóźnieniem kod aplikacji może korzystać z pracownika i kart czasu skojarzonych z pracownikami, a pozostałe blissfullyą wiedzą o pracy wymaganej do załadowania dodatkowych danych przez program Dr.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Ładowanie z opóźnieniem sprawia, że kod aplikacji jest łatwiejszy do zapisu, a przy użyciu Magic proxy kod pozostanie całkowicie weryfikowalne. W pamięci podręcznej jednostki pracy można po prostu wstępnie załadować fałszywe jednostki ze skojarzonymi danymi, jeśli są one odpowiednie podczas testu.

W tym momencie będziemy zachęcać do kompilowania repozytoriów przy użyciu IObjectSet @ no__t-0T @ no__t-1 i przyjrzeć się abstrakcjom w celu ukrycia wszystkich oznak struktury trwałości.

## <a name="custom-repositories"></a>Repozytoria niestandardowe

Po pierwszej prezentowaniu wzorca projektowego jednostki pracy w tym artykule podano przykładowy kod, który może wyglądać jak jednostka pracy. Zacznijmy od tego oryginalnego pomysłu przy użyciu scenariusza pracownika i karty czasu pracownika, z którym pracujemy.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Podstawowa różnica między tą jednostką pracy i jednostką pracy utworzoną w ostatniej sekcji polega na tym, jak ta jednostka pracy nie korzysta z żadnych streszczeń z EF4 Framework (nie ma IObjectSet @ no__t-0T @ no__t-1). IObjectSet @ no__t-0T @ no__t-1 działa dobrze jako interfejs repozytorium, ale interfejs API, który uwidacznia, może nie być idealnie wyrównany do potrzeb aplikacji. W tym nadchodzącym podejściu będziemy reprezentować repozytoria przy użyciu niestandardowego IRepository @ no__t-0T @ no__t-1.

Wielu deweloperów, którzy przestrzegają projektu opartego na testach, projektu opartego na zadziałach i metodologii opartej na warunkach domeny, preferują podejście IRepository @ no__t-0T @ no__t-1 z kilku powodów. Najpierw interfejs IRepository @ no__t-0T @ no__t-1 reprezentuje warstwę "Ochrona przed uszkodzeniem". Zgodnie z opisem przez Eric Evans w swojej organizacji projektowej opartej na domenie, warstwa antywirusowa utrzymuje kod domeny z interfejsów API infrastruktury, takich jak trwałość interfejsu API. Na koniec deweloperzy mogą tworzyć metody w repozytorium, które spełniają dokładne potrzeby aplikacji (jak zostało to wykryte podczas pisania testów). Przykładowo może być często konieczne znalezienie pojedynczej jednostki przy użyciu wartości identyfikatora, aby można było dodać metodę FindById do interfejsu repozytorium.  Nasze IRepository @ no__t-0T @ no__t-1 będą wyglądać następująco.

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

Zwróć uwagę, że będziemy wrócić do korzystania z interfejsu IQueryable @ no__t-0T @ no__t-1 w celu udostępnienia kolekcji jednostek. Interfejs IQueryable @ no__t-0T @ no__t-1 zezwala na przepływowanie drzew wyrażeń LINQ do dostawcy EF4 i nadawanie dostawcy całościowego widoku zapytania. Druga opcja zwróci wartość IEnumerable @ no__t-0T @ no__t-1, co oznacza, że dostawca EF4 LINQ zobaczy tylko wyrażenia wbudowane w repozytorium. Wszystkie grupowanie, porządkowanie i projekcje wykonywane poza repozytorium nie zostaną złożone do polecenia SQL wysłanego do bazy danych, co może obniżyć wydajność. Z drugiej strony repozytorium zwracające tylko wartość IEnumerable @ no__t-0T @ no__t-1 spowoduje, że nowe polecenie SQL nie zostanie nigdy zaskakujące. Obie metody będą działać, a oba podejścia pozostaną weryfikowalne.

Jest to proste, aby zapewnić jedną implementację interfejsu IRepository @ no__t-0T @ no__t-1 przy użyciu typów ogólnych i interfejsu API programu EF4 ObjectContext.

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

IRepository @ no__t-0T @ no__t-1 zapewnia nam dodatkową kontrolę nad naszymi zapytaniami, ponieważ klient musi wywołać metodę, aby uzyskać dostęp do jednostki. Wewnątrz metody możemy udostępnić dodatkowe sprawdzenia i operatory LINQ w celu wymuszenia ograniczeń aplikacji. Zauważ, że interfejs ma dwa ograniczenia dla parametru typu ogólnego. Pierwsze ograniczenie to wady klas wymagane przez obiekt ObjectSet @ no__t-0T @ no__t-1, a drugie ograniczenie wymusza, aby nasze jednostki implementują IEntity — abstrakcję utworzoną dla aplikacji. Interfejs IEntity wymusza, aby jednostki miały Właściwość identyfikatora z możliwością odczytu, a następnie można użyć tej właściwości w metodzie FindById. IEntity jest zdefiniowany przy użyciu następującego kodu.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity może być traktowany jako niewielkie naruszenie trwałości ignorujących, ponieważ nasze jednostki są wymagane do zaimplementowania tego interfejsu. Pamiętaj, że trwałość ignorujących ma wpływ na kompromisy, a w przypadku wielu funkcji FindById będzie przewyższał ograniczenie narzucone przez interfejs. Interfejs nie ma wpływu na testowanie.

Utworzenie wystąpienia elementu Live IRepository @ no__t-0T @ no__t-1 wymaga elementu EF4 ObjectContext, więc konkretną jednostką implementacji pracy powinna zarządzać Tworzenie wystąpienia.

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

### <a name="using-the-custom-repository"></a>Korzystanie z repozytorium niestandardowego

Korzystanie z naszego niestandardowego repozytorium nie różni się znacznie od użycia repozytorium w oparciu o IObjectSet @ no__t-0T @ no__t-1. Zamiast stosować operatory LINQ bezpośrednio do właściwości, najpierw musimy wywoływać metody tego repozytorium, aby uzyskać informacje o interfejsie IQueryable @ no__t-0T @ no__t-1.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Zwróć uwagę, że niestandardowy operator dołączania będzie działał bez zmian. Metoda FindById repozytorium usuwa zduplikowaną logikę z akcji próbujących pobrać pojedynczą jednostkę.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Nie ma znaczącej różnicy w zakresie testowania dwóch rozważanych metod. Możemy zapewnić fałszywe implementacje IRepository @ no__t-0T @ no__t-1 przez utworzenie konkretnych klas, których kopie zapasowe są wykonywane przez HashSet — @ no__t-2Employee @ no__t-3 — podobnie jak w ostatniej sekcji. Jednak niektórzy deweloperzy wolą używać obiektów tworzenia i makietowania struktur obiektów zamiast tworzyć sztuczne. Zobaczmy, jak używać makietów do testowania implementacji i omówienia różnic między fragmentami i elementami sztucznymi w następnej sekcji.

### <a name="testing-with-mocks"></a>Testowanie za pomocą makiet

Istnieją różne podejścia do kompilowania, które Fowlera dzwoni "test Double". Test Double (taki jak stunt Movie Double) jest obiektem, który kompiluje się do "w rzeczywistości" dla rzeczywistych obiektów produkcyjnych podczas testów. Utworzone repozytoria w pamięci są testami podwójne dla repozytoriów, które komunikują się SQL Server. Dowiesz się, jak używać tych testów podczas testów jednostkowych w celu odizolowania kodu i zapewnienia szybkiego uruchamiania testów.

Testy zostały skompilowane z rzeczywistymi, działającymi implementacjami. W tle każdy z nich przechowuje konkretną kolekcję obiektów i dodaje i usuwa obiekty z tej kolekcji podczas manipulowania repozytorium podczas testu. Niektórzy deweloperzy, którzy lubią kompilację testu, w ten sposób podwajają się w ten sposób, korzystając z prawdziwych i wydajnych implementacji  Ten test podwaja te elementy, które wywołujemy *fałszywe*. Mają one pracę z implementacjami, ale nie są one wystarczające do użycia w środowisku produkcyjnym. Fałszywe repozytorium nie zapisuje w bazie danych. Fałszywy serwer SMTP nie wysyła w rzeczywistości wiadomości e-mail za pośrednictwem sieci.

### <a name="mocks-versus-fakes"></a>Elementy w przeciwieństwie do elementów sztucznych

Istnieje inny typ testu podwójnie znany jako *makieta*. Podczas gdy elementy sztuczne mają działające implementacje, makiety nie są implementowane. Dzięki pomocy dotyczącej struktury obiektów makiety tworzymy te obiekty w czasie wykonywania i używają ich jako podwójnego przetestowania. W tej sekcji będziemy korzystać z struktury "Moqing" "open source". Oto prosty przykład użycia MOQ do dynamicznego tworzenia testów dla repozytorium pracowników.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Zaproponujemy MOQ dla implementacji IRepository @ no__t-0Employee @ no__t-1 i tworzy ją dynamicznie. Możemy uzyskać do obiektu implementującego IRepository @ no__t-0Employee @ no__t-1, uzyskując dostęp do właściwości Object obiektu DB@ no__t-2T @ no__t-3. Jest to ten obiekt wewnętrzny, który można przekazać do naszych kontrolerów i nie będzie wiadomo, czy jest to test podwójny, czy rzeczywiste repozytorium. Możemy wywoływać metody na obiekcie tak samo, jak możemy wywołać metody dla obiektu z rzeczywistą implementacją.

Należy zastanawiać się, co będzie miało repozytorium makiety po wywołaniu metody Add. Ponieważ nie istnieje implementacja za obiektem makiety, nic nie robi. Nie ma żadnej konkretnej kolekcji w tle, podobnie jak w przypadku zapisana przez nas sfałszowanych, więc pracownik jest odrzucany. Co z wartością zwracaną z FindById? W takim przypadku obiekt makiety robi tylko to, co może zrobić, co spowoduje zwrócenie wartości domyślnej. Ponieważ zwracamy typ referencyjny (pracownika), wartość zwracana jest wartością null.

Makiety mogą dźwiękować bezwartościowe; Istnieją jednak dwie inne funkcje makiet, o których nie podano informacji. Najpierw platforma MOQ rejestruje wszystkie wywołania wykonane na obiekcie makiety. Później można polecić MOQ, jeśli ktoś wywołał metodę Add, lub jeśli ktoś wywołał metodę FindById. Zobaczymy później, w jaki sposób możemy użyć tej funkcji nagrywania "czarnego pola" w testach.

Druga świetna funkcja polega na tym, jak możemy używać MOQ do programowania obiektu makiety z *oczekiwaniami*. Oczekiwanie nakazuje obiektowi makiety, jak odpowiedzieć na daną interakcję. Na przykład możemy zaprogramować oczekiwanie w naszym makietie i poinstruować go, aby zwracał obiekt Employee, gdy ktoś wywoła FindById. Struktura MOQ używa interfejsu API Instalatora i wyrażeń lambda do zaprogramowania tych oczekiwań.

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

W tym przykładzie poprosił MOQ o dynamiczne skompilowanie repozytorium, a następnie programuje repozytorium z oczekiwaniami. Oczekiwanie nakazuje obiektowi imitacji zwrócenie nowego obiektu pracownika z wartością identyfikatora 5, gdy ktoś wywoła metodę FindById, przekazując wartość 5. Ten test kończy się niepowodzeniem i nie musimy kompilować pełnej implementacji dla fałszywych IRepository @ no__t-0T @ no__t-1.

Ponownie odwiedzamy wcześniej wykonane testy i przeprowadzimy je do korzystania z form zamiast fałszywych. Podobnie jak wcześniej, użyjemy klasy bazowej do skonfigurowania wspólnych części infrastruktury potrzebnej dla wszystkich testów kontrolera.

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

Kod instalatora pozostaje w większości tego samego. Zamiast używać fałszywych, będziemy używać MOQ do konstruowania obiektów makiety. Klasa bazowa jest rozmieszczenia dla jednostki, która będzie zwracać repozytorium, gdy kod wywołuje właściwość Employees. Pozostała część konfiguracji makiety będzie odbywać się w ramach armatury testowej przeznaczonych dla każdego konkretnego scenariusza. Na przykład, armatura testowa dla akcji indeks spowoduje skonfigurowanie repozytorium makiety, aby zwracało listę pracowników, gdy akcja wywoła metodę FindAll repozytorium.

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

Z wyjątkiem oczekiwań, nasze testy wyglądają podobnie jak testy, które wcześniej istniały. Jednak dzięki możliwości rejestrowania struktury makiety możemy dochodzić do testowania z innego kąta. Ta nowa perspektywa zostanie wyświetlona w następnej sekcji.

### <a name="state-versus-interaction-testing"></a>Stan a testowanie interakcji

Istnieją różne techniki, których można użyć do testowania oprogramowania z obiektami makiety. Jednym z metod jest użycie testowania opartego na stanie, co jest gotowe do wykonania w tym dokumencie. Testowanie na podstawie stanu pozwala na potwierdzenie stanu oprogramowania. W ostatnim teście wywołana została metoda działania na kontrolerze i złożyła potwierdzenie dotyczące modelu, który powinien zostać skompilowany. Oto kilka innych przykładów stanu testowania:

-   Sprawdź, czy repozytorium zawiera nowy obiekt Employee po wykonaniu.
-   Sprawdź, czy model zawiera listę wszystkich pracowników po wykonaniu indeksu.
-   Upewnij się, że repozytorium nie zawiera danego pracownika po wykonaniu usuwania.

Inne podejście, które zobaczysz, jest widoczne z obiektami makiety, aby zweryfikować *interakcje*. Podczas testowania opartego na stanie są sprawdzane informacje o stanie obiektów, testowanie na podstawie interakcji sprawia, że obiekty są w interakcji z interakcją. Na przykład:

-   Upewnij się, że kontroler wywołuje metodę Add repozytorium podczas wykonywania tworzenia.
-   Sprawdź, czy kontroler wywołuje metodę FindAll repozytorium, gdy jest wykonywane indeksowanie.
-   Sprawdź, czy kontroler wywołuje metodę zatwierdzania jednostki pracy, aby zapisać zmiany po wykonaniu edycji.

Testowanie interakcji często wymaga mniej danych testowych, ponieważ nie są one poking wewnątrz kolekcji i nie sprawdzają liczby. Na przykład, jeśli wiemy, że akcja Details wywołuje metodę FindById repozytorium z poprawną wartością, to działanie prawdopodobnie działa poprawnie. Możemy sprawdzić to zachowanie bez konfigurowania jakichkolwiek danych testowych do zwrócenia z FindById.

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

Jedyną konfiguracją wymaganą w powyższym zasobie testowym jest instalacja dostarczana przez klasę bazową. Gdy wywołamy akcję kontrolera, MOQ będzie rejestrować interakcje z repozytorium makiety. Za pomocą weryfikowania interfejsu API MOQ można polecić MOQ, Jeśli kontroler wywołał FindById z prawidłową wartością identyfikatora. Jeśli kontroler nie wywołał metody lub wywołaniu metody z nieoczekiwaną wartością parametru, Metoda verify zgłosi wyjątek, a test zakończy się niepowodzeniem.

Oto inny przykład, aby sprawdzić, czy akcja tworzenia wywołuje zatwierdzenie w bieżącej jednostce pracy.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Jedno zagrożenie z testowaniem interakcji to tendencja do określania interakcji. Możliwość rejestrowania i weryfikowania każdej interakcji z obiektem makiety nie oznacza, że test powinien weryfikować każdą interakcję. Niektóre interakcje są szczegółami implementacji i weryfikują interakcje *wymagane* do spełnienia bieżącego testu.

Wybór między makietami lub sztucznymi jest w dużym stopniu zależny od testowanego systemu i preferencji osobistych (lub zespołów). Obiekty makiety mogą znacząco zmniejszyć ilość kodu wymaganego do wdrożenia testów, ale nie każdy z nich jest wygodny dla oczekiwań programistycznych i sprawdza interakcje.

## <a name="conclusions"></a>Wnioski

W tym dokumencie przedstawiono kilka metod tworzenia kodu weryfikowalne przy użyciu Entity Framework ADO.NET w celu zapewnienia trwałości danych. Możemy wykorzystać skompilowane streszczenia, takie jak IObjectSet @ no__t-0T @ no__t-1 lub utworzyć własne streszczenia, takie jak IRepository @ no__t-2T @ no__t-3.  W obu przypadkach wsparcie POCO w ADO.NET Entity Framework 4,0 umożliwia konsumentom tych streszczeń pozostawanie trwałych ignorujących i wysoce weryfikowalne. Dodatkowe funkcje EF4, takie jak niejawne ładowanie z opóźnieniem, umożliwiają działanie kodu usługi biznesowej i aplikacji bez konieczności pojmowania się szczegółami relacyjnego magazynu danych. Na koniec, tworzone streszczenia są łatwe do zawinięcia lub fałszywe wewnątrz testów jednostkowych, a firma Microsoft może używać tych testów w celu szybkiego uruchamiania, wysoce odizolowanych i niezawodnych testów.

### <a name="additional-resources"></a>Dodatkowe zasoby

-   Robert C. Martin, " [Pojedyncza zasada odpowiedzialności](https://www.objectmentor.com/resources/articles/srp.pdf)"
-   Fowlera Martin, [Katalog wzorców](https://www.martinfowler.com/eaaCatalog/index.html) ze *wzorców architektury aplikacji dla przedsiębiorstw*
-   Griffin Caprio, " [iniekcja zależności](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog dotyczący programowania danych "@no__t 0Walkthrough: Programowanie sterowane testami za pomocą Entity Framework 4.0 @ no__t-0 ".
-   Blog dotyczący programowania danych, " [używanie wzorców repozytorium i jednostki pracy z Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Aaron Jensen, " [Wprowadzenie specyfikacji maszyn](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lewandowski, " [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [Projektowanie oparte na domenie](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Fowlera Martin, " [imitacje nie są fragmentami](https://martinfowler.com/articles/mocksArentStubs.html)"
-   Fowlera Martin, " [test Double](https://martinfowler.com/bliki/TestDouble.html)"
-   [Moq](https://code.google.com/p/moq/)

### <a name="biography"></a>Biografii

Scott, który jest członkiem działu technicznego w Pluralsight i założyciel OdeToCode.com. W trakcie komercyjnego programowania oprogramowania Scott pracował nad rozwiązaniami dla wszystkich urządzeń z 8-bitowymi urządzeniami osadzonymi w wysoce skalowalnych aplikacjach sieci Web ASP.NET. Możesz skontaktować się z Scott w swoim blogu w witrynie OdeToCode lub w serwisie Twitter w [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).

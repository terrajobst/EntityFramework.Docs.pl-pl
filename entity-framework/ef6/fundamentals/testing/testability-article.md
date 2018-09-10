---
title: Testowalność i Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 2a2384c7868ae3cf6af4f915c06ae9fdb622634c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251326"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="3fc70-102">Testowalność i Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="3fc70-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="3fc70-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="3fc70-103">Scott Allen</span></span>

<span data-ttu-id="3fc70-104">Data publikacji: Maja 2010</span><span class="sxs-lookup"><span data-stu-id="3fc70-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="3fc70-105">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="3fc70-105">Introduction</span></span>

<span data-ttu-id="3fc70-106">Ten dokument zawiera opis i pokazuje, jak pisać kodu sprawdzalnego działa zgodnie z ADO.NET Entity Framework 4.0 i Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="3fc70-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="3fc70-107">W tym dokumencie nie podejmuje próby skupić się na określonych metodologia testowania, takich jak projektowanie oparte na testach (TDD) lub projektowania opartego na zachowanie (BDD).</span><span class="sxs-lookup"><span data-stu-id="3fc70-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="3fc70-108">Zamiast tego ten dokument koncentruje się na temat sposobu pisania kodu, który używa programu ADO.NET Entity Framework, ale pozostaje ułatwia odizolować i przetestować w zautomatyzowany sposób.</span><span class="sxs-lookup"><span data-stu-id="3fc70-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="3fc70-109">Zapoznamy się często używane wzorce projektowe, które ułatwiają dostęp do scenariuszy testowania w danych i zobacz, jak te wzorce są stosowane, gdy przy użyciu platformy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="3fc70-110">Również przyjrzymy określonych funkcji framework, aby zobaczyć, jak te funkcje mogą współdziałać w kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="3fc70-111">Co to jest kodu sprawdzalnego działa zgodnie?</span><span class="sxs-lookup"><span data-stu-id="3fc70-111">What Is Testable Code?</span></span>

<span data-ttu-id="3fc70-112">Możliwość sprawdzenia oprogramowanie przy użyciu zautomatyzowanych testów jednostek oferuje wiele korzyści pożądane.</span><span class="sxs-lookup"><span data-stu-id="3fc70-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="3fc70-113">Wszyscy wie, że dobry test zmniejsza liczbę usterek oprogramowania w aplikacji i zwiększa jakość aplikacji — ale przebiega testów jednostkowych w miejscu wykraczają daleko poza po prostu znajdowaniu usterek.</span><span class="sxs-lookup"><span data-stu-id="3fc70-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="3fc70-114">Zestaw testów jednostkowych dobre umożliwia zespół deweloperów zaoszczędzić czas i zachowuje kontrolę nad tworzone przez nich oprogramowanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="3fc70-115">Zespół wprowadzić zmiany w istniejącym kodzie, Refaktoryzacja, ich przeprojektowywania i restrukturyzacji oprogramowania do nowych wymagań i dodawania nowych składników do aplikacji, korzystając z możliwości, wiedząc, zestawu testów, można sprawdzić działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="3fc70-116">Testy jednostkowe są częścią cyklu szybkiej opinii w ułatwienia zmian i zachować łatwość oprogramowania w miarę wzrostu złożoności.</span><span class="sxs-lookup"><span data-stu-id="3fc70-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="3fc70-117">Testy jednostkowe jest dostarczany z ceną, jednak.</span><span class="sxs-lookup"><span data-stu-id="3fc70-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="3fc70-118">Zespół będzie musiał poświęcić czas na tworzenie i obsługa testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="3fc70-119">Nakład pracy wymagany do utworzenia tych testów jest bezpośrednio związana **testowalności** podstawowego oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="3fc70-120">Jak łatwo jest oprogramowanie do testowania?</span><span class="sxs-lookup"><span data-stu-id="3fc70-120">How easy is the software to test?</span></span> <span data-ttu-id="3fc70-121">Zespół projektowania oprogramowania za pomocą testowania należy pamiętać, utworzy szybciej niż zespół, Praca z oprogramowania bez sprawdzalnego działa zgodnie skuteczne testy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="3fc70-122">Firma Microsoft zaprojektowała ADO.NET Entity Framework 4.0 (EF4) za pomocą testowania na uwadze.</span><span class="sxs-lookup"><span data-stu-id="3fc70-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="3fc70-123">Nie oznacza to, że deweloperzy będzie zapisywać testów jednostkowych dla framework sam kod.</span><span class="sxs-lookup"><span data-stu-id="3fc70-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="3fc70-124">Zamiast tego cele testowania EF4 ułatwiają tworzenie kodu sprawdzalnego działa zgodnie opartą na strukturze.</span><span class="sxs-lookup"><span data-stu-id="3fc70-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="3fc70-125">Zanim przyjrzymy się konkretne przykłady, warto zrozumieć jakość kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="3fc70-126">Jakość kodu sprawdzalnego działa zgodnie</span><span class="sxs-lookup"><span data-stu-id="3fc70-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="3fc70-127">Kod, który jest łatwy do testów zawsze będzie mieć co najmniej dwóch cech.</span><span class="sxs-lookup"><span data-stu-id="3fc70-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="3fc70-128">Łatwo jest pierwszym kodu sprawdzalnego działa zgodnie **obserwować**.</span><span class="sxs-lookup"><span data-stu-id="3fc70-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="3fc70-129">Biorąc pod uwagę pewne zestaw danych wejściowych, powinno być łatwe do sprawdzanie danych wyjściowych kodu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="3fc70-130">Na przykład następujące metody badania jest łatwe, ponieważ metoda bezpośrednio zwraca wynik obliczeń.</span><span class="sxs-lookup"><span data-stu-id="3fc70-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="3fc70-131">Testowanie metody jest trudne, jeśli metoda zapisuje obliczona wartość do gniazda sieciowego, tabeli bazy danych lub plików, podobnie do poniższego kodu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="3fc70-132">Test musi być wykonania dodatkowej pracy do pobrania wartości.</span><span class="sxs-lookup"><span data-stu-id="3fc70-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="3fc70-133">Po drugie, jest łatwa do kodu sprawdzalnego działa zgodnie **izolowania**.</span><span class="sxs-lookup"><span data-stu-id="3fc70-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="3fc70-134">Użyjmy następujący pseudo-kod złe przykład kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="3fc70-135">Metoda ta jest łatwy do obserwowania — firma Microsoft może przekazać ubezpieczeniowej i sprawdź, czy oczekiwany wynik pasuje do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="3fc70-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="3fc70-136">Jednak do metody testowej musimy mieć zainstalowane przy użyciu poprawnego schematu bazy danych i konfigurowanie serwera SMTP, w przypadku, gdy metoda próbuje wysłać wiadomość e-mail.</span><span class="sxs-lookup"><span data-stu-id="3fc70-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="3fc70-137">Test jednostkowy tylko chciał sprawdzić, czy logiki obliczeń wewnątrz metody, ale test może zakończyć się niepowodzeniem, ponieważ serwer poczty e-mail jest w trybie offline lub przenieść serwer bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="3fc70-138">Oba te błędy są związane z zachowaniem testu będzie chciał sprawdzić, czy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="3fc70-139">Zachowanie jest trudne do izolowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="3fc70-140">Programistów, którzy Dokładamy wszelkich starań, aby zapisać często kodu sprawdzalnego działa zgodnie Dokładamy wszelkich starań zachować separacji w kodzie są zapisu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="3fc70-141">Powyższej metody należy skoncentrować się na obliczeń biznesowych i delegować szczegółów implementacji bazy danych i poczty e-mail do innych składników.</span><span class="sxs-lookup"><span data-stu-id="3fc70-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="3fc70-142">Robert C. Martin wywołuje to pojedynczy zasady odpowiedzialności.</span><span class="sxs-lookup"><span data-stu-id="3fc70-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="3fc70-143">Obiekt powinna hermetyzować pojedynczy, wąskie odpowiedzialność, takich jak obliczenia wartości zasady.</span><span class="sxs-lookup"><span data-stu-id="3fc70-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="3fc70-144">Wszystkie inne zadania bazy danych i powiadomień należy odpowiedzialność innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="3fc70-145">Kod napisany w ten sposób jest łatwiejsze do izolowania ponieważ koncentruje się na pojedynczym zadaniem.</span><span class="sxs-lookup"><span data-stu-id="3fc70-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="3fc70-146">Na platformie .NET mamy abstrakcji, potrzebne do działania w przekonaniu pojedynczej odpowiedzialności i uzyskania izolacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="3fc70-147">Możemy użyć definicji interfejsu i wymusić kod, aby użyć abstrakcji interfejsu, a nie do konkretnego typu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="3fc70-148">W dalszej części tego dokumentu zobaczymy, jak metoda, takich jak zły przykład przedstawiony powyżej może współpracować z interfejsów *Szukaj* jak rozmawiamy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="3fc70-149">W czasie testu jednak możemy użyć zamiast fikcyjnego implementację, która nie skontaktuj się z bazą danych, ale zamiast tego przechowuje dane w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3fc70-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="3fc70-150">Ta implementacja fikcyjnego izoluje kodu z niepowiązanych problemów w kod dostępu do danych lub baza danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="3fc70-151">Istnieją dodatkowe korzyści z izolacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="3fc70-152">Obliczenia biznesowego w ostatnim metody powinna trwać tylko kilka milisekund, które można wykonać, ale same testy mogą działać przez kilka sekund jako przeskoków kodu wokół sieci i rozmowy na różnych serwerach.</span><span class="sxs-lookup"><span data-stu-id="3fc70-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="3fc70-153">Testy jednostkowe, należy uruchomić szybki w celu ułatwienia niewielkich zmian.</span><span class="sxs-lookup"><span data-stu-id="3fc70-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="3fc70-154">Testy jednostkowe powinny także powtarzalne oraz wystąpi niepowodzenie, ponieważ składnik niezwiązanych ze sobą na test ma problem.</span><span class="sxs-lookup"><span data-stu-id="3fc70-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="3fc70-155">Pisanie kodu, który jest łatwy do obserwowania i do izolowania oznacza, że deweloperzy będzie teraz łatwiejsze pisania testów dla kodu, mogą spędzać mniej czasu oczekiwania na testami do wykonania, a więcej co ważniejsze, mogą spędzać mniej czasu, śledzenie błędów, które nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="3fc70-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="3fc70-156">Miejmy nadzieję można docenić zalety testowania i zrozumieć cechy, które wykazuje kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="3fc70-157">Jesteśmy rozwiązać jak napisać kod, który współdziała z EF4, aby zapisać dane w bazie danych pozostając dostrzegalnych i łatwe izolowanie, ale najpierw będzie zawęzić naszym głównym celem w celu omówienia sprawdzalnego działa zgodnie projektów, aby uzyskać dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="3fc70-158">Wzorce projektowe zapewniające trwałość danych</span><span class="sxs-lookup"><span data-stu-id="3fc70-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="3fc70-159">Obu przykładach zły przedstawiony wcześniej było zbyt wiele obowiązki.</span><span class="sxs-lookup"><span data-stu-id="3fc70-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="3fc70-160">Pierwszy przykład zły musiał wykonać obliczenia *i* zapisu do pliku.</span><span class="sxs-lookup"><span data-stu-id="3fc70-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="3fc70-161">Drugi przykład zły musiały odczytywać dane z bazy danych *i* wykonywanie obliczeń biznesowych *i* wysyłanie wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="3fc70-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="3fc70-162">Projektowanie mniejszych metody, które Rozdziel problemy i delegować odpowiedzialność za inne składniki wprowadzisz dotykowego do pisania kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="3fc70-163">Celem jest tworzyć funkcje za pośrednictwem akcji abstrakcje niewielkiego i skupionego projektu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="3fc70-164">Gdy chodzi o trwałości danych mały abstrakcje ukierunkowanych, których firma Microsoft szuka są więc wspólne został zostały opisane jako wzorców projektowych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="3fc70-165">Książki Martina Fowlera wzorców Enterprise architektury aplikacji była pierwszym pracy do opisania tych wzorców w drukowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="3fc70-166">Firma Microsoft udostępni krótki opis tych wzorców w poniższych sekcjach, zanim pokazujemy, jak te ADO.NET Entity Framework implementuje i współdziała z tych wzorców.</span><span class="sxs-lookup"><span data-stu-id="3fc70-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="3fc70-167">Wzorzec repozytorium</span><span class="sxs-lookup"><span data-stu-id="3fc70-167">The Repository Pattern</span></span>

<span data-ttu-id="3fc70-168">Fowlera wynika z repozytorium "pośredniczy między domeną i dane warstw mapowania za pomocą interfejsu przypominającego kolekcji do uzyskiwania dostępu do obiektów domeny".</span><span class="sxs-lookup"><span data-stu-id="3fc70-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="3fc70-169">Celem wzorca repozytorium jest do izolowania kodu z minutiae dostępu do danych i jak widzieliśmy wcześniej izolacji jest cechę wymaganych do testowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="3fc70-170">Kluczem do izolacji jest, jak repozytorium przedstawia obiektów za pomocą interfejsu przypominającego kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="3fc70-171">Logika zapisu do użycia w repozytorium ma nie wiadomo, jak repozytorium zmaterializowania obiektów, których w przypadku żądania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="3fc70-172">Repozytorium może komunikować się z bazą danych lub po prostu może zwracać obiekty z kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3fc70-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="3fc70-173">Wszystko, czego Twój kod musi wiedzieć, jest repozytorium pojawia się do zachowania kolekcji i pobierania, dodawanie i usuwanie obiektów z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="3fc70-174">W istniejących aplikacjach .NET konkretnych repozytorium często dziedziczy interfejs ogólny, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3fc70-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="3fc70-175">Wybierzemy kilka zmian do definicji interfejsu gdy firma Microsoft zapewnia implementację dla EF4, ale podstawowe pojęcia pozostają bez zmian.</span><span class="sxs-lookup"><span data-stu-id="3fc70-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="3fc70-176">Kod można użyć konkretnego repozytorium implementacji interfejsu można pobrać jednostki, jego wartość klucza podstawowego, można pobrać kolekcję jednostek na podstawie oceny predykat, lub po prostu pobrać wszystkie dostępne jednostki.</span><span class="sxs-lookup"><span data-stu-id="3fc70-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="3fc70-177">Kod można również dodawać i usuwać jednostki za pośrednictwem interfejsu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="3fc70-178">Biorąc pod uwagę obiektów IRepository pracowników, kod może wykonać następujące czynności.</span><span class="sxs-lookup"><span data-stu-id="3fc70-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="3fc70-179">Ponieważ kod wykorzystuje interfejs (IRepository pracowników), firma Microsoft może dostarczyć kod różne implementacje interfejsu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="3fc70-180">Jedna implementacja może być implementację wspierane przez EF4 i trwałość obiektów w bazie danych programu Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3fc70-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="3fc70-181">Inną implementację (po jednym używanych przez firmę Microsoft podczas testowania) może być objęta obiektów listy pracowników w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3fc70-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="3fc70-182">Interfejs może pomóc w celu uzyskania izolacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="3fc70-183">Zwróć uwagę, IRepository&lt;T&gt; interfejsu nie ujawnia operacja zapisu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="3fc70-184">Jak zaktualizować istniejące obiekty?</span><span class="sxs-lookup"><span data-stu-id="3fc70-184">How do we update existing objects?</span></span> <span data-ttu-id="3fc70-185">Mogą pochodzić między definicje IRepository, które obejmują operacja zapisu i implementacji tych repozytoriów, należy od razu utrwalić obiektu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="3fc70-186">Jednak w wielu aplikacjach nie chcemy zachować obiekty indywidualnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="3fc70-187">Zamiast tego chcemy Ożyw obiektów, być może z różnych repozytoriów, zmodyfikować te obiekty jako część operacji biznesowych i następnie zachować wszystkie obiekty w ramach jednej operacji niepodzielnych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="3fc70-188">Na szczęście jest wzorzec tego typu zachowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="3fc70-189">Jednostka wzorzec pracy</span><span class="sxs-lookup"><span data-stu-id="3fc70-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="3fc70-190">Fowlera mówi, jednostka pracy będzie "Obsługa listy obiektów wpływ transakcji biznesowych i służy do koordynowania zapisu poza zmiany i rozwiązywanie problemów współbieżności".</span><span class="sxs-lookup"><span data-stu-id="3fc70-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="3fc70-191">Jest odpowiedzialny za jednostkę pracy do śledzenia zmian do obiektów, firma Microsoft Ożyw z repozytorium i utrwala wszelkie zmiany, które wprowadziliśmy do obiektów, gdy o jednostce pracy, aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="3fc70-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="3fc70-192">Jest również odpowiedzialność za jednostkę pracy do wykonania nowe obiekty, firma Microsoft zostały dodane do wszystkich repozytoriów i wstawianie obiektów bazy danych, a także usunięcie Zarządzaj witryną.</span><span class="sxs-lookup"><span data-stu-id="3fc70-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="3fc70-193">Jeśli nigdy nie wykonano żadnej pracy z zestawami danych ADO.NET następnie będzie już można zapoznać się z jednostką wzorzec pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="3fc70-194">Zestawy danych ADO.NET wcześniej mieli możliwość Śledź nasze aktualizacje, usuwanie i wstawianie elementu DataRow obiektów i może (za pomocą adaptera TableAdapter) uzgadniają wszystkich zmian do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="3fc70-195">Jednak obiektów DataSet modelu podzestaw odłączonej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="3fc70-196">Jednostka wzorzec pracy wykazuje takie samo zachowanie, ale działa z obiektami biznesowych i obiektów domeny, które są odizolowane od kod dostępu do danych i rozpoznaje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="3fc70-197">Abstrakcja do modelowania jednostek pracy w kodzie .NET może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3fc70-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="3fc70-198">Dzięki uwidocznieniu działania odwołania repozytorium przy użyciu jednostki pracy, którą firma Microsoft zapewnia pojedynczą jednostkę pracy obiekt ma możliwość śledzenia wszystkich jednostek zmaterializowanego podczas transakcji biznesowych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="3fc70-199">Implementacja metody zatwierdzania dla rzeczywistego jednostki pracy jest do uzgadniają zmiany w pamięci z bazą danych, gdzie się Magia.</span><span class="sxs-lookup"><span data-stu-id="3fc70-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="3fc70-200">Biorąc pod uwagę odwołaniem IUnitOfWork, kod można wprowadzać zmiany w obiektach firm pobierane z jednego lub więcej repozytoriów i Zapisz wszystkie zmiany, przy użyciu niepodzielnych operacji zatwierdzania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="3fc70-201">Wzorzec obciążenia z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="3fc70-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="3fc70-202">Fowlera używa nazwy obciążenia z opóźnieniem do opisania "obiekt, który nie zawiera wszystkich danych potrzebujesz, ale wie, jak przygotować".</span><span class="sxs-lookup"><span data-stu-id="3fc70-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="3fc70-203">Przezroczysty powolne ładowanie to ważna cecha ma podczas pisania kodu sprawdzalnego działa zgodnie biznesowych i Praca z relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="3fc70-204">Na przykład rozważmy poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="3fc70-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="3fc70-205">Jak jest wypełniana kolekcji kart</span><span class="sxs-lookup"><span data-stu-id="3fc70-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="3fc70-206">Istnieją dwa możliwe odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3fc70-206">There are two possible answers.</span></span> <span data-ttu-id="3fc70-207">Jedną odpowiedź jest, że repozytorium pracowników, po wyświetleniu monitu o pobranie pracownika, wysyła zapytanie do pobrania pracownika wraz z informacjami skojarzone karta godzin pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="3fc70-208">Relacyjne bazy danych to zwykle wymaga zapytanie z klauzulą JOIN i może skutkować podczas pobierania informacji niż aplikacja potrzebuje.</span><span class="sxs-lookup"><span data-stu-id="3fc70-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="3fc70-209">Co zrobić, jeśli aplikacja nigdy nie musi dotykać właściwości kart?</span><span class="sxs-lookup"><span data-stu-id="3fc70-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="3fc70-210">Druga odpowiedź jest można załadować właściwości kart "na żądanie".</span><span class="sxs-lookup"><span data-stu-id="3fc70-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="3fc70-211">To powolne ładowanie jest niewidoczne dla logiki biznesowej i niejawne, ponieważ kod nie jest wywoływany specjalne interfejsów API można pobrać informacji o karcie czasu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="3fc70-212">Kod zakłada, że informacje dotyczące karty czasu jest obecna, gdy są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="3fc70-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="3fc70-213">Ładowanie z opóźnieniem, które zazwyczaj polega na przechwytywaniu środowiska uruchomieniowego wywołań metody opisywanego zaangażowanych jest kilka magic.</span><span class="sxs-lookup"><span data-stu-id="3fc70-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="3fc70-214">Przechwytujący kodu jest odpowiedzialny za komunikować się z bazą danych i pobierania informacji o karcie czasu przy równoczesnym zachowaniu logika biznesowa może być logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="3fc70-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="3fc70-215">Ta magic obciążenia z opóźnieniem umożliwia kod firm, aby izolować sam z operacjami pobierania danych i skutkuje więcej kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="3fc70-216">Wadą ładowane z opóźnieniem jest fakt, że aplikacja *jest* potrzebne informacje karty godz., kod będzie wykonywał żadnych dodatkowych kwerend.</span><span class="sxs-lookup"><span data-stu-id="3fc70-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="3fc70-217">To nie jest istotna dla wielu aplikacji, ale dla aplikacji zawierających poufne dane wydajności lub aplikacji liczba obiektów pracowników w pętli i wykonywanie zapytania do pobierania czasu kart podczas każdej iteracji pętli (problem często określane jako N + 1 problem z zapytania), powolne ładowanie jest przeciągania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="3fc70-218">W tych scenariuszach aplikacji może być eagerly załadować informacji o czasie karty w najbardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="3fc70-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="3fc70-219">Na szczęście zobaczymy, jak EF4 obsługuje zarówno niejawne obciążeń z opóźnieniem i wydajne eager ładuje możemy przenosić do następnej sekcji i implementacji tych wzorców.</span><span class="sxs-lookup"><span data-stu-id="3fc70-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="3fc70-220">Implementowanie wzorców z programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3fc70-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="3fc70-221">Dobra wiadomość jest wszystkich wzorców projektowych, które firma Microsoft opisanego w ostatniej sekcji są proste do wdrożenia przy użyciu EF4.</span><span class="sxs-lookup"><span data-stu-id="3fc70-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="3fc70-222">Aby zademonstrować są użyjemy prostej aplikacji ASP.NET MVC do edytowania i wyświetlania pracowników i ich skojarzone karta godzin.</span><span class="sxs-lookup"><span data-stu-id="3fc70-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="3fc70-223">Rozpoczniemy pracę przy użyciu następujących "zwykłe stare CLR obiektów" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="3fc70-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="3fc70-224">Te definicje klas będzie się nieznacznie zmieniać omówimy różne podejścia i funkcje EF4, ale celem jest zapewnienie tych klas jako trwałości zakresu (PI) jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="3fc70-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="3fc70-225">Obiekt PI nie wie, *jak*, lub nawet *Jeśli*, stan przechowuje, są przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="3fc70-226">PI i POCOs idą ręka w rękę z oprogramowaniem sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="3fc70-227">Obiekty przy użyciu podejścia POCO są mniej ograniczone, bardziej elastyczne i łatwiejsze do testu, ponieważ mogą one działać bez bazy danych istnieje.</span><span class="sxs-lookup"><span data-stu-id="3fc70-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="3fc70-228">Za pomocą POCOs w miejscu możemy utworzyć Entity Data Model (EDM) w programie Visual Studio (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="3fc70-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="3fc70-229">Firma Microsoft nie będzie używać EDM do generowania kodu dla naszych jednostek.</span><span class="sxs-lookup"><span data-stu-id="3fc70-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="3fc70-230">Zamiast tego chcemy użyć jednostki, które firma Microsoft lovingly pracowali ręcznie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="3fc70-231">Firma Microsoft będzie używała tylko EDM Generowanie naszych schemat bazy danych, a następnie podaj metadane EF4 potrzebuje do mapowania obiektów do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="3fc70-233">**Rysunek 1.**</span><span class="sxs-lookup"><span data-stu-id="3fc70-233">**Figure 1**</span></span>

<span data-ttu-id="3fc70-234">Uwaga: Jeśli chcesz najpierw opracowanie modelu EDM jest możliwe czyszczenie, generowanie kodu POCO z EDM.</span><span class="sxs-lookup"><span data-stu-id="3fc70-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="3fc70-235">Można to zrobić za pomocą rozszerzenia programu Visual Studio 2010, dostarczane przez zespół Data programowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="3fc70-236">Aby pobrać rozszerzenie, uruchom Menedżera rozszerzeń z menu Narzędzia w programie Visual Studio i znajdowania galerii szablonów w trybie online "POCO" (zobacz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="3fc70-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="3fc70-237">Istnieje kilka szablonów POCO są dostępne na platformie EF.</span><span class="sxs-lookup"><span data-stu-id="3fc70-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="3fc70-238">Aby uzyskać więcej informacji na temat korzystania z tego szablonu, zobacz " [Instruktaż: obiektów POCO szablonu programu Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="3fc70-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="3fc70-240">**Rysunek 2**</span><span class="sxs-lookup"><span data-stu-id="3fc70-240">**Figure 2**</span></span>

<span data-ttu-id="3fc70-241">Z tego POCO punkt początkowy przeanalizujemy dwa różne podejścia do kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="3fc70-242">Pierwszym sposobem czy mogę wywołać podejście EF, ponieważ wykorzystuje abstrakcje z Entity Framework interfejs API, aby zaimplementować jednostek pracy i repozytoriów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="3fc70-243">W drugiej metody możemy utworzyć własną abstrakcje niestandardowe repozytorium i wtedy wyświetlone zalety i wady każdej metody.</span><span class="sxs-lookup"><span data-stu-id="3fc70-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="3fc70-244">Rozpoczniemy od wypróbowania podejście EF.</span><span class="sxs-lookup"><span data-stu-id="3fc70-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="3fc70-245">Implementacja przetwarzających EF</span><span class="sxs-lookup"><span data-stu-id="3fc70-245">An EF Centric Implementation</span></span>

<span data-ttu-id="3fc70-246">Należy wziąć pod uwagę następujące akcji kontrolera w projekcie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3fc70-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="3fc70-247">Akcja pobiera obiekt pracowników i zwraca wynik, aby wyświetlić widok szczegółowy pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="3fc70-248">Jest kodu sprawdzalnego działa zgodnie?</span><span class="sxs-lookup"><span data-stu-id="3fc70-248">Is the code testable?</span></span> <span data-ttu-id="3fc70-249">Istnieją co najmniej dwóch testów czy musimy zweryfikować zachowanie akcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="3fc70-250">Najpierw chcemy sprawdzić, czy akcja zwraca widok poprawne — łatwe testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="3fc70-251">Firma Microsoft będzie również chcieć napisać test, aby sprawdzić działanie powoduje pobranie poprawne pracowników i prosimy o poświęcenie zrobienie tego bez wykonywania kodu, aby w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="3fc70-252">Należy pamiętać, że chcemy izolowanie testowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="3fc70-253">Izolacja będzie upewnij się, że test nie zakończy się niepowodzeniem z powodu usterki w kod dostępu do danych lub baza danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="3fc70-254">Jeśli test zakończy się niepowodzeniem, wiemy, że mamy usterkę w logiką kontrolera, a nie w niektórych niższe składnika poziomu systemu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="3fc70-255">W celu uzyskania izolacji poprosimy niektóre elementy abstrakcji takich jak interfejsy, które firma Microsoft przedstawiony wcześniej dla repozytoriów i jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="3fc70-256">Pamiętaj, że wzorzec repozytorium jest przeznaczona do pośredniczy między obiektów domeny i warstwy mapowanie danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="3fc70-257">W tym scenariuszu EF4 *jest* dane mapowania warstwy i udostępnia już abstrakcji podobne do repozytorium, o nazwie IObjectSet&lt;T&gt; (z przestrzeni nazw System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="3fc70-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="3fc70-258">Definicja interfejsu wygląda podobnie do poniższego.</span><span class="sxs-lookup"><span data-stu-id="3fc70-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="3fc70-259">IObjectSet&lt;T&gt; spełnia wymagania dotyczące repozytorium, ponieważ jest on podobny kolekcji obiektów (za pośrednictwem interfejsu IEnumerable&lt;T&gt;) i dostarcza metod dodawania i usuwania obiektów z kolekcji symulowane.</span><span class="sxs-lookup"><span data-stu-id="3fc70-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="3fc70-260">Metod dołączania i odłączania uwidocznić dodatkowe funkcje interfejsu API EF4.</span><span class="sxs-lookup"><span data-stu-id="3fc70-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="3fc70-261">Aby użyć IObjectSet&lt;T&gt; jako interfejs dla repozytoriów potrzebujemy jednostkę pracy abstrakcji można powiązać ze sobą repozytoriów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="3fc70-262">Jedną konkretną implementację tego interfejsu będzie się komunikował z programu SQL Server i łatwo utworzyć za pomocą klasy obiektu ObjectContext z EF4.</span><span class="sxs-lookup"><span data-stu-id="3fc70-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="3fc70-263">Klasa obiektu ObjectContext jest prawdziwe jednostka pracy w interfejsie API EF4.</span><span class="sxs-lookup"><span data-stu-id="3fc70-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="3fc70-264">Dzięki temu IObjectSet&lt;T&gt; w życie jest równie proste jak wywołania metody CreateObjectSet obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="3fc70-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="3fc70-265">Za kulisami środowisko użyje metadanych zamieszczone w EDM, aby wygenerować konkretnego obiektu ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="3fc70-266">Używany będzie ze zwracaniem IObjectSet&lt;T&gt; interfejsu, ponieważ ułatwi to zachować testowania w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="3fc70-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="3fc70-267">Ta implementacja konkretnych przydaje się w środowisku produkcyjnym, ale musimy skupić się na sposób użyjemy naszego abstrakcji IUnitOfWork aby ułatwić testowanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="3fc70-268">Badanie wartości podwójnej precyzji</span><span class="sxs-lookup"><span data-stu-id="3fc70-268">The Test Doubles</span></span>

<span data-ttu-id="3fc70-269">Aby wyizolować akcji kontrolera będziemy potrzebować możliwości przełączania się między rzeczywistych jednostkę pracy (wspierana przez obiekt ObjectContext) i testowej jednostki typu double lub "fałszywych" pracy (wykonywania operacji w pamięci).</span><span class="sxs-lookup"><span data-stu-id="3fc70-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="3fc70-270">Typowym podejściem do wykonania tego rodzaju przełączanie jest zezwala kontroler MVC wystąpienia jednostki pracy, ale zamiast tego przebiegu jednostkę pracy do kontrolera jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="3fc70-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="3fc70-271">Powyższy kod jest przykładem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="3fc70-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="3fc70-272">Nie umożliwiamy kontrolera utworzyć jego zależności (jednostka pracy), ale wstrzykiwanie zależności do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3fc70-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="3fc70-273">W projekcie MVC jest często używa się fabryki kontrolerów niestandardowych w połączeniu z odwrócenie kontenera kontrolek (IoC) do automatyzowania iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="3fc70-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="3fc70-274">Te tematy wykraczają poza zakres tego artykułu, ale możesz przeczytać więcej przez następujące odwołania na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="3fc70-275">Jednostka fałszywych wprowadzania pracy, które firma Microsoft można używać do testowania może wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="3fc70-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="3fc70-276">Zwróć uwagę, że fałszywych jednostkę pracy ujawnia właściwość zatwierdzone.</span><span class="sxs-lookup"><span data-stu-id="3fc70-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="3fc70-277">Czasami jest to przydatne dodać funkcje do fałszywych klasę, która ułatwić testowanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="3fc70-278">W tym przypadku jest łatwo obserwować, jeśli kod zatwierdzenia jednostkę pracy, zaznaczając właściwość zatwierdzone.</span><span class="sxs-lookup"><span data-stu-id="3fc70-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="3fc70-279">Będziemy również potrzebować fałszywych IObjectSet&lt;T&gt; do przechowywania obiektów, pracowników i karty czasowej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3fc70-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="3fc70-280">Oferujemy pojedynczą implementacją za pomocą typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="3fc70-281">Ten test podwójnego deleguje większość swojej pracy, aby podstawowy zestaw HashSet&lt;T&gt; obiektu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="3fc70-282">Uwaga tego IObjectSet&lt;T&gt; wymaga ograniczenie generyczne wymuszanie T jako klasę (typ odwołania) i wymusza także NAS, aby zaimplementować interfejs IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="3fc70-283">Ułatwia tworzenie kolekcji w pamięci, są traktowane jako element IQueryable&lt;T&gt; przy użyciu standardowego operatora zapytań LINQ AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="3fc70-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="3fc70-284">Testy</span><span class="sxs-lookup"><span data-stu-id="3fc70-284">The Tests</span></span>

<span data-ttu-id="3fc70-285">Testy jednostkowe tradycyjnych użyje klasy jeden test do przechowywania wszystkich testów dla wszystkich akcji w pojedynczy kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="3fc70-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="3fc70-286">Firma Microsoft można pisać testy lub dowolnego typu testu jednostkowego przy użyciu pamięci elementów sztucznych utworzyliśmy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="3fc70-287">Jednak w tym artykule, w których firma Microsoft zapobiegnie monolityczne testu klasy i zamiast tego pogrupować Nasze testy, aby skoncentrować się na konkretne funkcje.</span><span class="sxs-lookup"><span data-stu-id="3fc70-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="3fc70-288">Na przykład, "Tworzenie nowego pracownika" może być funkcje, które chcemy sprawdzić, więc zostanie użyta klasa jeden test Aby zweryfikować akcji pojedynczy kontroler odpowiedzialnych za tworzenie nowego pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="3fc70-289">Brak wspólnego kodu ustawień potrzebnych dla wszystkich tych klas testowych szczegółowe.</span><span class="sxs-lookup"><span data-stu-id="3fc70-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="3fc70-290">Na przykład zawsze należy utworzyć naszych repozytoriów w pamięci i fałszywych jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="3fc70-291">Należy również wystąpienie kontrolera pracowników z fałszywych jednostek pracy, które są wstrzykiwane.</span><span class="sxs-lookup"><span data-stu-id="3fc70-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="3fc70-292">Ten typowy kod instalacji zostaną udostępnione różnych klas testowych za pomocą klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="3fc70-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="3fc70-293">"Matka obiektu" używamy w klasie bazowej jest jeden wspólny wzorzec do tworzenia danych testowych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="3fc70-294">Matkę obiektów zawiera metodami factory do tworzenia wystąpienia testów jednostek do użycia w wielu świetlnymi testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="3fc70-295">Możemy użyć EmployeeControllerTestBase jako klasa bazowa dla liczby świetlnymi testu (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="3fc70-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="3fc70-296">Każdy warunki początkowe testu przetestuje akcji określonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3fc70-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="3fc70-297">Na przykład jeden warunki początkowe testu koncentruje się na testowanie Akcja Utwórz używane podczas żądania HTTP GET (Aby wyświetlić widok na potrzeby tworzenia pracownika) i różnych warunków początkowych koncentruje się na Akcja Utwórz używana w żądaniu POST protokołu HTTP (do uwzględnienia informacji przesyłanych przez użytkownikowi utworzenie pracownika).</span><span class="sxs-lookup"><span data-stu-id="3fc70-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="3fc70-298">Każdej klasy pochodnej odpowiada tylko ustawienia wymagane w tym kontekście określonego oraz w celu zapewnienia potwierdzenia potrzebne, aby zweryfikować wyniki dla kontekst określonego testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="3fc70-300">**Rysunek 3.**</span><span class="sxs-lookup"><span data-stu-id="3fc70-300">**Figure 3**</span></span>

<span data-ttu-id="3fc70-301">Styl nazewnictwa Konwencji i testowania przedstawionych w tym miejscu nie jest wymagana dla kodu sprawdzalnego działa zgodnie — wystarczy jedno z podejść.</span><span class="sxs-lookup"><span data-stu-id="3fc70-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="3fc70-302">Rysunek 4 przedstawia testów przeprowadzanych codziennie w Resharper mózgów Jet test runner wtyczki dla programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="3fc70-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="3fc70-304">**Rysunek 4**</span><span class="sxs-lookup"><span data-stu-id="3fc70-304">**Figure 4**</span></span>

<span data-ttu-id="3fc70-305">Za pomocą klasy bazowej do obsługi kodu udostępnionego Instalatora testów jednostkowych dla każdej akcji kontrolera są małe i łatwe do zapisu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="3fc70-306">Testy będą wykonywane szybko (ponieważ będziemy działają w pamięci operacji) i nie powinna zakończyć się niepowodzeniem ze względu na niepowiązanych infrastruktury lub środowiskiem naturalnym (ponieważ mamy już izolowany jednostki w ramach testu).</span><span class="sxs-lookup"><span data-stu-id="3fc70-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="3fc70-307">W tych testach klasa bazowa wykonuje większość pracy Instalatora.</span><span class="sxs-lookup"><span data-stu-id="3fc70-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="3fc70-308">Należy pamiętać, że tworzy konstruktora klasy bazowej, repozytorium w pamięci, fałszywych jednostkę pracy i wystąpienia klasy EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="3fc70-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="3fc70-309">Klasa testowa pochodzi z tej klasy bazowej i koncentruje się na szczegółowe informacje na temat testowania metody Create.</span><span class="sxs-lookup"><span data-stu-id="3fc70-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="3fc70-310">W tym przypadku szczegółowe informacje na temat gotować "rozmieścić, działanie i asercja" czynności, pokazywany w jakiejkolwiek jednostce z badania procedury:</span><span class="sxs-lookup"><span data-stu-id="3fc70-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="3fc70-311">Utwórz obiekt Nowy_pracownik symulowanie danych przychodzących.</span><span class="sxs-lookup"><span data-stu-id="3fc70-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="3fc70-312">Wywołaj akcję Utwórz EmployeeController i przekazać Nowy_pracownik.</span><span class="sxs-lookup"><span data-stu-id="3fc70-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="3fc70-313">Sprawdź, czy akcja Utwórz tworzy oczekiwanych wyników (pracownika pojawia się w repozytorium).</span><span class="sxs-lookup"><span data-stu-id="3fc70-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="3fc70-314">Co utworzyliśmy pozwala nam na wszystkie akcje EmployeeController testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="3fc70-315">Na przykład podczas pisania testów dla akcji indeksu kontrolera pracowników firma Microsoft może dziedziczyć z klasy bazowej testu do ustanowienia tej samej podstawowej instalacji na potrzeby testów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="3fc70-316">Klasa bazowa utworzy ponownie repozytorium w pamięci, fałszywych jednostkę pracy i wystąpienia EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="3fc70-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="3fc70-317">Testy dla akcji indeksu wystarczy skupić się na wywoływaniu akcji indeksu i testowanie jakość modelu akcji zwraca.</span><span class="sxs-lookup"><span data-stu-id="3fc70-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="3fc70-318">Tworzymy z substytutami w pamięci nie jest rozróżniana ukierunkowane na testowanie *stanu* oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="3fc70-319">Na przykład podczas testowania Akcja Utwórz, chcemy, aby sprawdzić stan repozytorium, po wykonaniu akcji tworzenia — repozytorium przechowywania nowych pracownika?</span><span class="sxs-lookup"><span data-stu-id="3fc70-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="3fc70-320">Później przyjrzymy interakcji na podstawie badania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="3fc70-321">Testowanie interakcji na podstawie zostanie wyświetlone pytanie, jeśli testowany kod wywoływany właściwe metody na naszych obiektów i przekazywane poprawnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="3fc70-322">Teraz przejdziemy okładki innego wzorzec projektowy — obciążenia z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="3fc70-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="3fc70-323">Wczesne ładowanie i powolne ładowanie</span><span class="sxs-lookup"><span data-stu-id="3fc70-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="3fc70-324">W pewnym momencie w sieci web platformy ASP.NET MVC aplikacji, którą firma Microsoft może chcieć wyświetlić informacje dotyczące pracowników i obejmują pracownika skojarzone karty godzin.</span><span class="sxs-lookup"><span data-stu-id="3fc70-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="3fc70-325">Na przykład możemy mieć karty czas wyświetlania podsumowania, zawierający nazwisko pracownika oraz łącznej liczbie kart czasu systemu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="3fc70-326">Istnieje kilka rozwiązań, które możemy wykonać, aby zaimplementować tę funkcję.</span><span class="sxs-lookup"><span data-stu-id="3fc70-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="3fc70-327">Rzut</span><span class="sxs-lookup"><span data-stu-id="3fc70-327">Projection</span></span>

<span data-ttu-id="3fc70-328">Jedno z podejść łatwy do utworzenia podsumowania jest do konstruowania modelu dedykowanych informacje, które ma być wyświetlany w widoku.</span><span class="sxs-lookup"><span data-stu-id="3fc70-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="3fc70-329">W tym scenariuszu modelu może wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="3fc70-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="3fc70-330">Należy pamiętać, że EmployeeSummaryViewModel nie jest jednostką — innymi słowy nie jest coś, co jest potrzebne do utrwalenia w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="3fc70-331">Tylko będziemy używać tej klasy do mieszania danych w widoku w silnie typizowany sposób.</span><span class="sxs-lookup"><span data-stu-id="3fc70-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="3fc70-332">Model widoku przypomina danych przenieść obiekt (DTO), ponieważ zawiera ona nie zachowanie (nie metody) — tylko właściwości.</span><span class="sxs-lookup"><span data-stu-id="3fc70-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="3fc70-333">Właściwości będą przechowywane dane, które trzeba przenieść.</span><span class="sxs-lookup"><span data-stu-id="3fc70-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="3fc70-334">Jest łatwy do utworzenia wystąpienia tego modelu widoku za pomocą operatora rzutowania standard firmy LINQ — wybierz operator.</span><span class="sxs-lookup"><span data-stu-id="3fc70-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="3fc70-335">Istnieją dwa istotne funkcje do powyższego kodu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-335">There are two notable features to the above code.</span></span> <span data-ttu-id="3fc70-336">Najpierw — kod jest łatwy do testowania, ponieważ jest wciąż łatwe do obserwowania i izolować.</span><span class="sxs-lookup"><span data-stu-id="3fc70-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="3fc70-337">Wybierz operator działa równie dobrze względem naszych elementów sztucznych w pamięci, co względem rzeczywistych jednostkę pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="3fc70-338">Druga funkcja istotne jest, jak kod umożliwia EF4 do wygenerowania pojedynczego, wydajny kwerendy można złożyć pracowników i karty z czasu informacji ze sobą.</span><span class="sxs-lookup"><span data-stu-id="3fc70-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="3fc70-339">Firma Microsoft załadowane informacje dotyczące pracowników i informacje dotyczące karty czasu do tego samego obiektu bez korzystania z żadnych specjalnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="3fc70-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="3fc70-340">Kod wyrażone jedynie informacje, których wymaga przy użyciu standardowych operatorów LINQ, które działają względem źródła danych w pamięci, a także zdalnych źródeł danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="3fc70-341">EF4 był w stanie dokonać translacji drzew wyrażeń, generowane przez zapytanie LINQ i C\# kompilatora do pojedynczych i wydajne zapytania T-SQL.</span><span class="sxs-lookup"><span data-stu-id="3fc70-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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

<span data-ttu-id="3fc70-342">Brak innym razem, gdy firma Microsoft nie chcesz pracować z modelu widoku lub obiekt DTO, ale z rzeczywistych jednostek.</span><span class="sxs-lookup"><span data-stu-id="3fc70-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="3fc70-343">Gdy wiemy, że potrzebujemy pracownika *i* karty godzin pracownika, firma Microsoft eagerly ładowanie powiązanych danych w sposób dyskretny kod i wydajne.</span><span class="sxs-lookup"><span data-stu-id="3fc70-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="3fc70-344">Jawne wczesne ładowanie</span><span class="sxs-lookup"><span data-stu-id="3fc70-344">Explicit Eager Loading</span></span>

<span data-ttu-id="3fc70-345">Jeśli chcemy eagerly załadować informacji o powiązanych jednostek potrzebujemy mechanizmu logiki biznesowej (lub w tym scenariuszu kontroler akcji logiki) aby wyrazić chęć repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="3fc70-346">Obiekt ObjectQuery EF4&lt;T&gt; klasa definiuje metodę Include w taki sposób, aby określić pokrewnych obiektów do pobrania podczas wykonywania kwerendy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="3fc70-347">Należy pamiętać, EF4 ObjectContext udostępnia jednostek za pomocą konkretnego obiektu ObjectSet&lt;T&gt; klasy, która dziedziczy z ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="3fc70-348">Jeśli firma Microsoft była używana w obiekcie ObjectSet&lt;T&gt; odwołań w naszej akcji kontrolera, napiszemy następujący kod, aby określić eager obciążenia informacje dotyczące karty czas dla każdego pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="3fc70-349">Jednak ponieważ próbujemy zapewnienie naszego kodu sprawdzalnego działa zgodnie możemy są nie udostępnianie obiektu ObjectSet&lt;T&gt; z poza rzeczywistych jednostką pracy klasy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="3fc70-350">Zamiast tego Polegamy IObjectSet&lt;T&gt; interfejs, który ułatwia fałszywe, ale IObjectSet&lt;T&gt; nie definiuje metodę Include.</span><span class="sxs-lookup"><span data-stu-id="3fc70-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="3fc70-351">Zaletą korzystania z LINQ jest, możemy utworzyć własną operator Include.</span><span class="sxs-lookup"><span data-stu-id="3fc70-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="3fc70-352">Zwróć uwagę, ten operator Include jest zdefiniowany jako metody rozszerzenia dla elementu IQueryable&lt;T&gt; zamiast IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="3fc70-353">To daje możliwość korzystania z metody z szerszego zakresu możliwych typów, w tym IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, a obiekt ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="3fc70-354">W przypadku sekwencji źródłowej nie jest oryginalnym ObjectQuery EF4&lt;T&gt;, to nie ma żadnych szkód, wykonywane, i Include operator jest pusta.</span><span class="sxs-lookup"><span data-stu-id="3fc70-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="3fc70-355">Jeśli podstawowe sekwencji *jest* ObjectQuery&lt;T&gt; (lub pochodzić od ObjectQuery&lt;T&gt;), EF4 będą Zobacz nasze wymagania w zakresie dodatkowych danych i formułowanie odpowiednie SQL Zapytanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="3fc70-356">Za pomocą tego nowego operatora w miejscu możemy jawne żądanie eager obciążenia informacje dotyczące karty czasu z repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="3fc70-357">Po uruchomieniu testów rzeczywistego obiektu ObjectContext, ten kod tworzy następujące pojedynczego zapytania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="3fc70-358">Zapytanie zbiera wystarczającą ilość informacji z bazy danych w jednym podróży do zmaterializowania obiektów pracowników i całkowicie wypełnić ich właściwości kart.</span><span class="sxs-lookup"><span data-stu-id="3fc70-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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

<span data-ttu-id="3fc70-359">Mamy znakomitą wiadomość jest całkowicie sprawdzalnego działa zgodnie pozostaje kod wewnątrz metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="3fc70-360">Nie potrzebujemy Podaj wszelkie dodatkowe funkcje dla naszych elementów sztucznych na obsługuje operatora Include.</span><span class="sxs-lookup"><span data-stu-id="3fc70-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="3fc70-361">Złe wiadomości jest boss Said WE had użycie operatora Include wewnątrz kodu, Chcieliśmy, aby zapewnić trwałość zakresu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="3fc70-362">Jest to podstawowy przykład typu skutków ubocznych, które będą potrzebne do wzięcia pod uwagę podczas kompilowania kodu sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="3fc70-363">Istnieją terminy, niezbędne, aby umożliwić przeciek wątpliwości trwałości poza abstrakcji repozytorium, aby spełnić cele dotyczące wydajności.</span><span class="sxs-lookup"><span data-stu-id="3fc70-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="3fc70-364">Alternatywa dla wczesne ładowanie jest powolne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="3fc70-365">Powolne ładowanie oznacza, że robimy *nie* naszych firm kod potrzebny do jawnie poinformować o konieczności powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="3fc70-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="3fc70-366">Zamiast tego stosujemy nasze jednostek w aplikacji, a jeśli dodatkowe dane potrzebne Entity Framework załaduje dane na żądanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="3fc70-367">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="3fc70-367">Lazy Loading</span></span>

<span data-ttu-id="3fc70-368">To ułatwia Wyobraź sobie scenariusz, w których nie można określić, co będzie potrzebne dane dany fragment logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="3fc70-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="3fc70-369">Firma Microsoft znać logiki wymaga obiektu pracowników, ale firma Microsoft może gałąź do wykonywania różnych ścieżek, gdzie niektóre z tych ścieżek wymagają informacji o karcie czas od pracownika, a niektóre nie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="3fc70-370">Scenariusze, takie jak to są idealne dla niejawnego powolne ładowanie ponieważ magiczny sposób wyświetlania danych na zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="3fc70-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="3fc70-371">Powolne ładowanie, znany także jako odroczone ładowanie, umieść pewne wymagania na naszych obiekty jednostki.</span><span class="sxs-lookup"><span data-stu-id="3fc70-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="3fc70-372">POCOs z nieznajomości trwałości wartość true, nie będzie twarzy wszelkie wymagania z warstwy trwałości, ale nieznajomości trwałości wartość true, jest praktycznie niemożliwe do osiągnięcia.</span><span class="sxs-lookup"><span data-stu-id="3fc70-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="3fc70-373">Zamiast tego mierzymy nieznajomości trwałości w stopniach względnych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="3fc70-374">Byłoby niefortunne potrzebowaliśmy dziedziczą z klasy bazowej zorientowanej na trwałości lub użyć wyspecjalizowane kolekcji, aby osiągnąć powolne ładowanie w POCOs.</span><span class="sxs-lookup"><span data-stu-id="3fc70-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="3fc70-375">Na szczęście EF4 ma płynniejsza rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="3fc70-376">Praktycznie wykryć</span><span class="sxs-lookup"><span data-stu-id="3fc70-376">Virtually Undetectable</span></span>

<span data-ttu-id="3fc70-377">Używanie obiektów POCO, EF4 można dynamicznie generować proxy środowiska uruchomieniowego dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="3fc70-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="3fc70-378">Te serwery proxy niewidocznie opakować POCOs zmaterializowany i zapewnić Uzyskaj dodatkowe usługi przechwycenie każdej właściwości i ustaw operację do wykonania dodatkowej pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="3fc70-379">Takie usługi jest funkcja ładowania z opóźnieniem, którą firma Microsoft szuka.</span><span class="sxs-lookup"><span data-stu-id="3fc70-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="3fc70-380">Inna usługa jest wydajny mechanizm, który może zapisać zmianie wartości właściwości jednostki, program śledzenia zmian.</span><span class="sxs-lookup"><span data-stu-id="3fc70-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="3fc70-381">Lista zmian jest używana przez obiekt ObjectContext podczas metodę SaveChanges, aby utrwalić wszystkie jednostki modyfikacji, przy użyciu polecenia aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="3fc70-382">Dla tych serwerów proxy do pracy jednak potrzebują sposób do podłączenia do właściwości get i ustawiania wykonywanych względem jednostki a serwerami proxy osiągnięcie tego celu przez zastąpienie wirtualnych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="3fc70-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="3fc70-383">W związku z tym jeśli chcemy masz niejawnej powolne ładowanie i wydajny śledzenia zmian należy przejść z powrotem do naszych POCO definicje klas i oznacz właściwości jako wirtualny.</span><span class="sxs-lookup"><span data-stu-id="3fc70-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="3fc70-384">Firma Microsoft może nadal wskazuje, że jednostki pracowników jest przede wszystkim trwałości zakresu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="3fc70-385">Jedynym wymaganiem jest, aby użyć wirtualnych elementów członkowskich i nie ma to wpływu na testowanie kodu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="3fc70-386">Firma Microsoft nie muszą pochodzić z żadnych specjalnych klasy bazowej lub nawet za pomocą specjalnych kolekcji przeznaczone do ładowania z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="3fc70-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="3fc70-387">Tak jak pokazano kod, każda klasa implementacji ICollection&lt;T&gt; jest dostępna na potrzeby przechowywania powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="3fc70-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="3fc70-388">Istnieje również jeden drobnej zmiany, które należy wprowadzić w naszych jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="3fc70-389">Powolne ładowanie jest *poza* domyślnie, pracując bezpośrednio z obiektem ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="3fc70-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="3fc70-390">Jest właściwością, możemy ustawić właściwości ContextOptions umożliwia odroczone ładowanie i możemy ustawić tę właściwość w naszym rzeczywistych jednostkę pracy, jeśli chcemy Włącz powolne ładowanie wszędzie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="3fc70-391">Przy użyciu niejawnego ładowania z opóźnieniem, włączone, kod aplikacji może użyć pracownika pracownika związanych oraz kart czas pozostając błogiej świadomości ilości pracy wymaganej dla platformy EF załadować dodatkowe dane.</span><span class="sxs-lookup"><span data-stu-id="3fc70-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="3fc70-392">Powolne ładowanie sprawia, że kod aplikacji jest łatwiejsze do zapisu, a proxy magic kod nie będzie całkowicie sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="3fc70-393">Substytuty w pamięci jednostki pracy po prostu wstępnego ładowania fałszywych jednostki z powiązane dane, gdy potrzebne podczas testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="3fc70-394">W tym momencie będziesz Włącz naszej uwagi od tworzenia repozytoriów za pomocą IObjectSet&lt;T&gt; i przyjrzyj się elementy abstrakcji, aby ukryć wszystkie znaki Framework trwałości.</span><span class="sxs-lookup"><span data-stu-id="3fc70-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="3fc70-395">Niestandardowe repozytoria</span><span class="sxs-lookup"><span data-stu-id="3fc70-395">Custom Repositories</span></span>

<span data-ttu-id="3fc70-396">Umieszczeniem możemy najpierw jednostki pracy wzorcu projektowym w tym artykule zamieszczone przykładowy kod dla jak może wyglądać jednostkę pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="3fc70-397">Umożliwia ponowne przedstawia tę koncepcję oryginalny, używając pracowników i pracowników karta godzin scenariusza, który możemy masz doświadczenie w pracy z.</span><span class="sxs-lookup"><span data-stu-id="3fc70-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="3fc70-398">Główną różnicą między tej jednostki pracy i jednostki pracy utworzonych w ostatniej sekcji jest jak tej jednostki pracy nie używa żadnych abstrakcje platformy EF4 (nie ma żadnych IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="3fc70-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="3fc70-399">IObjectSet&lt;T&gt; działa również jako interfejs repozytorium, ale interfejs API udostępnia ona doskonale mogą być niewyrównane naszej aplikacji potrzebom.</span><span class="sxs-lookup"><span data-stu-id="3fc70-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="3fc70-400">W tym podejściu nadchodzących firma Microsoft będzie reprezentować repozytoriów przy użyciu niestandardowych IRepository&lt;T&gt; abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="3fc70-401">Wielu programistów, którzy postępuj zgodnie z projektowania opartego na testach, projektowania opartego na zachowanie i projektowanie metodologii opartego na domenach Preferuj IRepository&lt;T&gt; podejścia z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="3fc70-402">Najpierw IRepository&lt;T&gt; interfejs reprezentuje "" warstwa przeciwdegradacyjna.</span><span class="sxs-lookup"><span data-stu-id="3fc70-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="3fc70-403">Zgodnie z opisem w Eric Evans w książce projektowania opartego na domenie warstwy przeciwdegradacyjnej przechowuje kod domeny od infrastruktury interfejsów API, np. trwałość interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="3fc70-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="3fc70-404">Po drugie deweloperzy mogą tworzyć metody do repozytorium, które potrzeb dokładnie aplikacji (jak wykryte podczas pisania testów).</span><span class="sxs-lookup"><span data-stu-id="3fc70-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="3fc70-405">Na przykład może być często potrzebujemy zlokalizować pojedynczą jednostkę przy użyciu wartości Identyfikatora, abyśmy mogli dodać metodę FindById interfejsu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="3fc70-406">Nasze IRepository&lt;T&gt; definicja będzie wyglądać podobnie do poniższego.</span><span class="sxs-lookup"><span data-stu-id="3fc70-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="3fc70-407">Zwróć uwagę, firma Microsoft będzie ponownie upuść, aby za pomocą element IQueryable&lt;T&gt; interfejsu do udostępnienia kolekcji jednostek.</span><span class="sxs-lookup"><span data-stu-id="3fc70-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="3fc70-408">Element IQueryable&lt;T&gt; umożliwia drzew wyrażeń LINQ przepływać do dostawcy EF4 i dostawcy holistycznego widoku zapytania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="3fc70-409">Drugą opcją będzie zwracać IEnumerable&lt;T&gt;, co oznacza, że dostawcy EF4 LINQ będą widzieć tylko wyrażenia utworzone wewnątrz repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="3fc70-410">Wszelkie grupowania, kolejność i projekcji wykonywane poza repozytorium nie będzie składa do polecenia SQL wysyłane do bazy danych, która może obniżyć wydajność.</span><span class="sxs-lookup"><span data-stu-id="3fc70-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="3fc70-411">Z drugiej strony, repozytorium, zwracając tylko IEnumerable&lt;T&gt; wyniki będą nigdy nie Cię zaskoczyć, za pomocą nowego polecenia SQL.</span><span class="sxs-lookup"><span data-stu-id="3fc70-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="3fc70-412">W obu przypadkach efekt będzie działać, a oba podejścia nadal sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="3fc70-413">Jest prosta zapewnić pojedynczą implementacją IRepository&lt;T&gt; interfejs, za pomocą typów ogólnych i EF4 API obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="3fc70-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="3fc70-414">IRepository&lt;T&gt; podejście zapewnia nam niektóre dodatkową kontrolę nad naszego zapytania, ponieważ klient musi wywołać metodę, aby uzyskać dostęp do jednostki.</span><span class="sxs-lookup"><span data-stu-id="3fc70-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="3fc70-415">Wewnątrz metody możemy to zapewnić dodatkowe czynności kontrolne i operatorów LINQ do wymuszania ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="3fc70-416">Zwróć uwagę, że interfejs ma dwa ograniczenia dla parametru typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="3fc70-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="3fc70-417">Pierwszy ograniczeniem jest to klasa zmiany barwy wad, wymagane przez obiekt ObjectSet&lt;T&gt;, i drugi ograniczenie wymusza naszych jednostek do zaimplementowania IEntity — abstrakcję utworzony dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="3fc70-418">Interfejs IEntity wymusza podmiotów odczytywalną Właściwość Id, a następnie możemy użyć tej właściwości w metodzie FindById.</span><span class="sxs-lookup"><span data-stu-id="3fc70-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="3fc70-419">IEntity jest zdefiniowana z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="3fc70-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="3fc70-420">IEntity może zostać uznana za mały naruszenie nieznajomości trwałości, ponieważ naszych jednostki są wymagane do zaimplementowania interfejsu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="3fc70-421">Należy pamiętać nieznajomości trwałości dotyczy wady i zalety i dla wielu funkcji FindById będzie przeważają ograniczenia nałożone przez interfejs.</span><span class="sxs-lookup"><span data-stu-id="3fc70-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="3fc70-422">Interfejs nie ma wpływu na testowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="3fc70-423">Utworzenie wystąpienia na żywo IRepository&lt;T&gt; wymaga obiektu ObjectContext EF4, dlatego konkretnej jednostki pracy wdrożenia należy zarządzać procesu tworzenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="3fc70-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="3fc70-424">Przy użyciu niestandardowych repozytorium</span><span class="sxs-lookup"><span data-stu-id="3fc70-424">Using the Custom Repository</span></span>

<span data-ttu-id="3fc70-425">Za pomocą naszych niestandardowe repozytorium nie jest znacznie różnią się od przy użyciu repozytorium, w oparciu o IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="3fc70-426">Zamiast bezpośrednio do właściwości z zastosowaniem operatorów LINQ, najpierw musisz wywołać za pomocą jednego z repozytorium metod do pobrania element IQueryable&lt;T&gt; odwołania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="3fc70-427">Zwróć uwagę, że niestandardowy operator Include, wcześniej zaimplementowane będzie działać bez zmian.</span><span class="sxs-lookup"><span data-stu-id="3fc70-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="3fc70-428">Metoda FindById z repozytorium usuwa zduplikowane logiki z akcji próby pobrania pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="3fc70-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="3fc70-429">Nie ma znaczące różnic w testowania dwa podejścia, w których firma Microsoft zostały zbadane.</span><span class="sxs-lookup"><span data-stu-id="3fc70-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="3fc70-430">Zapewniamy fałszywych implementacje IRepository&lt;T&gt; przez utworzenie klas konkretnych wspierana przez zestaw HashSet&lt;pracowników&gt; — podobnie jak zrobiliśmy w ostatniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="3fc70-431">Jednak niektórzy deweloperzy wolą używać makiety obiektów i testowanie struktur obiektu zamiast tworzenia elementów sztucznych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="3fc70-432">Zapoznamy się przy użyciu mocks do testowania naszej implementacji i omówiono różnice między mocks i sztucznych elementów w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="3fc70-433">Testowanie za pomocą Mocks</span><span class="sxs-lookup"><span data-stu-id="3fc70-433">Testing with Mocks</span></span>

<span data-ttu-id="3fc70-434">Istnieją różne metody do tworzenia wywołań jakie Martina Fowlera "test podwójnego".</span><span class="sxs-lookup"><span data-stu-id="3fc70-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="3fc70-435">Test podwójnego (na przykład stunt filmu double) jest obiektem tworzonych "występować w" rzeczywiste, obiekty w środowisku produkcyjnym podczas testów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="3fc70-436">Repozytoriów w pamięci, którą utworzyliśmy są testu wartości podwójnej precyzji dla repozytoriów, komunikujące się z programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3fc70-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="3fc70-437">Pokazaliśmy już, jak za pomocą tych podwaja testów podczas testów jednostkowych izolowania kodu i zachować testów przeprowadzanych codziennie przez szybkie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="3fc70-438">Badanie wartości podwójnej precyzji, którą utworzyliśmy ma implementacje rzeczywistych, praca.</span><span class="sxs-lookup"><span data-stu-id="3fc70-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="3fc70-439">W tle każdego z nich przechowuje kolekcję konkretnych obiektów i będą oni dodawać i Usuń obiekty z tej kolekcji, jak możemy manipulować repozytorium podczas testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="3fc70-440">Niektórzy deweloperzy, takich jak tworzenie ich podwaja się test temu — przy użyciu rzeczywistego kodu i implementacje pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="3fc70-441">Te testu wartości podwójnej precyzji są tak zwany *elementów sztucznych*.</span><span class="sxs-lookup"><span data-stu-id="3fc70-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="3fc70-442">Mają one implementacje pracy, ale nie są prawdziwe, do użytku produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="3fc70-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="3fc70-443">Repozytorium fałszywych faktycznie nie zapisu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="3fc70-444">Serwer SMTP fałszywych faktycznie nie Wyślij wiadomość e-mail za pośrednictwem sieci.</span><span class="sxs-lookup"><span data-stu-id="3fc70-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="3fc70-445">Mocks w porównaniu z elementów sztucznych</span><span class="sxs-lookup"><span data-stu-id="3fc70-445">Mocks versus Fakes</span></span>

<span data-ttu-id="3fc70-446">Istnieje inny typ testu double nazywane *testowanie*.</span><span class="sxs-lookup"><span data-stu-id="3fc70-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="3fc70-447">Chociaż elementów sztucznych implementacje pracy, mocks są dostarczane z implementacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="3fc70-448">Za pomocą framework makiety obiektu możemy konstruowania tych makiety obiektów w czasie wykonywania i używać ich jako wartości podwójnej precyzji testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="3fc70-449">W tej sekcji będziemy używać "open source" pozorowanie framework Moq.</span><span class="sxs-lookup"><span data-stu-id="3fc70-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="3fc70-450">Poniżej przedstawiono prosty przykład użycia Moq umożliwia dynamiczne tworzenie test podwójnego repozytorium pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="3fc70-451">Poprosimy Moq dla IRepository&lt;pracowników&gt; implementacji i tworzy jeden dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="3fc70-452">Firma Microsoft może uzyskać dostęp do obiekt implementujący IRepository&lt;pracowników&gt; , uzyskując dostęp do właściwości obiektu pozorny&lt;T&gt; obiektu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="3fc70-453">Jest ten obiekt wewnętrzny, które możemy przekazać do naszych kontrolerów, a nie będą wiedzieli, jeśli jest to test podwójnego lub rzeczywistego repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="3fc70-454">Firma Microsoft wywoływać metody na obiekt, tak samo, jak firma Microsoft będzie wywoływać metody na obiekt z rzeczywistej implementacji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="3fc70-455">Możesz się zastanawiać, co makiety repozytorium zrobić po możemy wywołać metody Add.</span><span class="sxs-lookup"><span data-stu-id="3fc70-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="3fc70-456">Ponieważ nie ma żadnej implementacji za makiety obiektu, Dodaj nic nie robi.</span><span class="sxs-lookup"><span data-stu-id="3fc70-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="3fc70-457">Brak konkretnego kolekcji w tle, takie jak mieliśmy z substytutami, którą napisaliśmy, więc jest odrzucana pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="3fc70-458">Jak wygląda wartość zwracaną FindById?</span><span class="sxs-lookup"><span data-stu-id="3fc70-458">What about the return value of FindById?</span></span> <span data-ttu-id="3fc70-459">W tym przypadku makiety obiektu jest jedyną czynnością, którą wykonania, która jest zwracana wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="3fc70-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="3fc70-460">Ponieważ firma Microsoft jest zwracany typ odwołania (pracownika), wartość zwracana jest wartość null.</span><span class="sxs-lookup"><span data-stu-id="3fc70-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="3fc70-461">Mocks może dźwiękowych bezwartościowe; Istnieją jednak dwa więcej funkcji mocks, które jeszcze nie Omówiliśmy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="3fc70-462">Po pierwsze Moq framework rejestruje wszystkie wywołania makiety obiektu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="3fc70-463">W dalszej części kodu możemy zadawać Moq, czy każdy użytkownik wywołał metodę Add, czy każdy użytkownik wywołał metodę FindById.</span><span class="sxs-lookup"><span data-stu-id="3fc70-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="3fc70-464">Zobaczymy, pokażemy, jak możemy użyć tej funkcji "czarne pole" Rejestrowanie w testach.</span><span class="sxs-lookup"><span data-stu-id="3fc70-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="3fc70-465">Druga funkcja doskonałe jest, jak możemy użyć Moq program makiety obiektu z *oczekiwania*.</span><span class="sxs-lookup"><span data-stu-id="3fc70-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="3fc70-466">Oczekiwanie informuje makiety obiektu, jak reagować na interakcji z danym.</span><span class="sxs-lookup"><span data-stu-id="3fc70-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="3fc70-467">Na przykład możemy zaprogramować Oczekiwanie w naszym pozorny i stwierdzenie, aby zwrócić obiekt pracowników, gdy ktoś wywołuje FindById.</span><span class="sxs-lookup"><span data-stu-id="3fc70-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="3fc70-468">Środowisko Moq wykorzystuje interfejs API konfiguracji i wyrażeń lambda z programem tego oczekiwania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="3fc70-469">W tym przykładzie prosimy Moq dynamicznie Tworzenie repozytorium, a następnie będziemy programować repozytorium z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="3fc70-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="3fc70-470">Oczekuje informuje makiety obiekt do zwrotu nowy obiekt pracowników z wartością identyfikatora 5, gdy ktoś wywołuje metodę FindById, przekazując wartość 5.</span><span class="sxs-lookup"><span data-stu-id="3fc70-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="3fc70-471">Ten test zakończy się pomyślnie, a firma Microsoft nie trzeba tworzyć pełną implementację do IRepository fałszywych&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="3fc70-472">Spróbujmy ponownie testy, którą napisaliśmy wcześniej i poprawkami, a ich do używania mocks zamiast elementów sztucznych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="3fc70-473">Podobnie jak wcześniej, użyjemy klasę bazową można skonfigurować wspólne elementy infrastruktury potrzebnych dla wszystkich kontrolera testów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="3fc70-474">Kod ustawienia pozostają takie same.</span><span class="sxs-lookup"><span data-stu-id="3fc70-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="3fc70-475">Zamiast korzystać z elementów sztucznych, użyjemy Moq do konstruowania obiektów makiety.</span><span class="sxs-lookup"><span data-stu-id="3fc70-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="3fc70-476">Klasa bazowa organizuje makiety jednostka pracy do zwrócenia makiety repozytorium, gdy kod wywołuje właściwość pracowników.</span><span class="sxs-lookup"><span data-stu-id="3fc70-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="3fc70-477">Pozostała konfiguracja makiety odbędzie się wewnątrz świetlnymi testów przeznaczonych dla każdego konkretnego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="3fc70-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="3fc70-478">Na przykład warunki początkowe testu dla akcji indeksu skonfiguruje makiety repozytorium, aby zwrócić listę pracowników, gdy akcja wywołuje metodę FindAll makiety repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="3fc70-479">Z wyjątkiem oczekiwania Nasze testy wyglądać podobnie do testów, które Musieliśmy przed.</span><span class="sxs-lookup"><span data-stu-id="3fc70-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="3fc70-480">Jednak dzięki możliwości rejestrowania makiety Framework możemy podejście, testowanie pod kątem różnych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="3fc70-481">Omówimy to Nowa perspektywa w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="3fc70-482">Stan i testowanie interakcji</span><span class="sxs-lookup"><span data-stu-id="3fc70-482">State versus Interaction Testing</span></span>

<span data-ttu-id="3fc70-483">Istnieją różne techniki, które można użyć do testowania oprogramowania za pomocą makiety obiektów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="3fc70-484">Jednym z podejść jest stanu na podstawie badań, czyli co możemy zostały wykonane w tym dokumencie do tej pory.</span><span class="sxs-lookup"><span data-stu-id="3fc70-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="3fc70-485">Stan oparty testowania potwierdzenia sprawia, że o stanie tego oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="3fc70-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="3fc70-486">W ostatni test firma Microsoft wywoływane metody akcji kontrolera, a wprowadzone potwierdzenie o modelu, należy go tworzyć.</span><span class="sxs-lookup"><span data-stu-id="3fc70-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="3fc70-487">Poniżej przedstawiono kilka przykładów stanu testów:</span><span class="sxs-lookup"><span data-stu-id="3fc70-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="3fc70-488">Sprawdź, czy repozytorium zawiera nowy obiekt pracownika po wykonaniu Utwórz.</span><span class="sxs-lookup"><span data-stu-id="3fc70-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="3fc70-489">Sprawdź, czy model zawiera listę wszystkich pracowników, po wykonaniu indeksu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="3fc70-490">Sprawdź, czy po wykonaniu Usuwanie repozytorium nie zawiera danego pracownika.</span><span class="sxs-lookup"><span data-stu-id="3fc70-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="3fc70-491">Innym podejściem zostanie wyświetlony z makiety obiektów jest sprawdzenie *interakcje*.</span><span class="sxs-lookup"><span data-stu-id="3fc70-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="3fc70-492">Gdy stan na podstawie testowania potwierdzenia sprawia, że o stanie obiektów, interakcji na podstawie testowania potwierdzenia sprawia, że dotyczące sposobu interakcji obiektów.</span><span class="sxs-lookup"><span data-stu-id="3fc70-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="3fc70-493">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3fc70-493">For example:</span></span>

-   <span data-ttu-id="3fc70-494">Sprawdź, czy kontroler wywołuje metody Add z repozytorium, gdy wykonuje Utwórz.</span><span class="sxs-lookup"><span data-stu-id="3fc70-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="3fc70-495">Sprawdź, czy kontroler wywołuje metodę FindAll z repozytorium, gdy indeks.</span><span class="sxs-lookup"><span data-stu-id="3fc70-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="3fc70-496">Sprawdź, czy kontroler wywołuje jednostki szkoły wywołano metody Commit można zapisać zmian, gdy wykonuje edycji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="3fc70-497">Często testowania interakcji wymaga mniejszej ilości danych testu, ponieważ firma Microsoft nie są wywiercenie wewnątrz kolekcji i weryfikowanie liczby.</span><span class="sxs-lookup"><span data-stu-id="3fc70-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="3fc70-498">Na przykład gdy wiemy, że akcja szczegóły wywołuje metodę FindById repozytorium przy użyciu poprawnej wartości - następnie akcji jest prawdopodobnie działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="3fc70-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="3fc70-499">Bez konfigurowania żadnych danych testowych do zwrócenia z FindById, możemy sprawdzić to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="3fc70-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="3fc70-500">Tylko ustawienia wymagane w powyższe warunki początkowe testu jest Instalator dostarczonych przez klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="3fc70-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="3fc70-501">Gdy firma Microsoft wywołanie akcji kontrolera, Moq zarejestruje interakcje makiety repozytorium.</span><span class="sxs-lookup"><span data-stu-id="3fc70-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="3fc70-502">Za pomocą interfejsu API Sprawdź Moq, możemy zadawać Moq Jeśli kontroler wywołana FindById przy użyciu prawidłowego wartość Identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="3fc70-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="3fc70-503">Jeśli kontrolera nie wywołała metodę lub wywołano metodę z wartością parametru nieoczekiwany, sprawdź, czy metoda zgłosi wyjątek, a test zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="3fc70-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="3fc70-504">Oto inny przykład, aby sprawdzić, czy akcja Utwórz wywołuje zatwierdzenia dla bieżącej jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="3fc70-505">Jeden zagrożenia z testowaniem interakcji jest tendencja za pośrednictwem Określ interakcje.</span><span class="sxs-lookup"><span data-stu-id="3fc70-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="3fc70-506">Możliwość makiety obiektu do rejestrowania i sprawdź, czy każdy interakcja z makiety obiektu nie oznacza testu starać się zweryfikować każdej interakcji.</span><span class="sxs-lookup"><span data-stu-id="3fc70-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="3fc70-507">Niektóre interakcje są szczegóły dotyczące implementacji i interakcji należy zweryfikować tylko *wymagane* do zaspokojenia bieżącego testu.</span><span class="sxs-lookup"><span data-stu-id="3fc70-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="3fc70-508">Wybór między mocks lub elementów sztucznych dużym stopniu zależy od systemu, które testujesz i osobistej (lub zespole kart) Preferencje.</span><span class="sxs-lookup"><span data-stu-id="3fc70-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="3fc70-509">Obiekty makiety może znacznie zmniejszyć ilość kodu, musisz wdrożyć test wartości podwójnej precyzji, ale nie wszyscy jest komfortowo, jednocześnie programowania oczekiwań i weryfikowanie interakcje.</span><span class="sxs-lookup"><span data-stu-id="3fc70-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="3fc70-510">Wnioski</span><span class="sxs-lookup"><span data-stu-id="3fc70-510">Conclusions</span></span>

<span data-ttu-id="3fc70-511">W tym dokumencie firma Microsoft została przedstawiona różne podejścia do tworzenia kodu sprawdzalnego działa zgodnie z podczas korzystania z programu Entity Framework ADO.NET dla funkcji trwałości danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="3fc70-512">Firma Microsoft mogą korzystać z wbudowanymi abstrakcji takich jak IObjectSet&lt;T&gt;, lub utworzyć własne elementy abstrakcji takich jak IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="3fc70-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="3fc70-513">W obu przypadkach te elementy abstrakcji konsumentom stale trwały dzięki obsłudze POCO w ADO.NET Entity Framework 4.0, zakresu i wysoce testowalna.</span><span class="sxs-lookup"><span data-stu-id="3fc70-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="3fc70-514">Dodatkowe funkcje EF4, takich jak niejawna powolne ładowanie umożliwia usłudze biznesowych kodu do pracy bez konieczności martwienia się o szczegółowe informacje o relacyjnego magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="3fc70-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="3fc70-515">Na koniec abstrakcji, tworzonej przez nas są łatwe do makiety lub fałszywe wewnątrz testów jednostkowych i możemy użyć tych testów wartości podwójnej precyzji do osiągnięcia szybkiego uruchamiania, wysoce izolowane i niezawodne testy.</span><span class="sxs-lookup"><span data-stu-id="3fc70-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="3fc70-516">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3fc70-516">Additional Resources</span></span>

-   <span data-ttu-id="3fc70-517">Robert C. Martin, " [zasady pojedynczej odpowiedzialności](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="3fc70-518">Martina Fowlera [katalog wzorców](http://www.martinfowler.com/eaaCatalog/index.html) z *wzorców architektury aplikacji przedsiębiorstwa*</span><span class="sxs-lookup"><span data-stu-id="3fc70-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="3fc70-519">Griffin Caprio " [wstrzykiwanie zależności](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="3fc70-520">Grupa dyskusyjna danych, " [Instruktaż: testów opartych na tworzenie aplikacji przy użyciu programu Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="3fc70-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="3fc70-521">Grupa dyskusyjna danych, " [wzorców przy użyciu repozytorium i jednostki pracy za pomocą programu Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="3fc70-522">Dave Astels " [wprowadzenie BDD](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="3fc70-523">Aaron Jensen " [wprowadzenie do specyfikacji maszyny](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="3fc70-524">Eric Lee " [BDD narzędziu MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="3fc70-525">Eric Evans, " [projektowania opartego na domenach](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="3fc70-526">Martina Fowlera " [Mocks nie są wycinków](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="3fc70-527">Martina Fowlera " [Test podwójnego](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="3fc70-528">Jeremy Miller, Dyrektor ds " [stanu i testowanie interakcji](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="3fc70-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="3fc70-529">Moq</span><span class="sxs-lookup"><span data-stu-id="3fc70-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="3fc70-530">Biografia</span><span class="sxs-lookup"><span data-stu-id="3fc70-530">Biography</span></span>

<span data-ttu-id="3fc70-531">Scott Allen jest członek personelu technicznego w firmie Pluralsight i założycielem OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="3fc70-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="3fc70-532">15 lat Wytwarzanie oprogramowania komercyjnego Scott pracował nad rozwiązania związane z 8-bitową urządzeń osadzonych do wysoce skalowalnych aplikacji sieci web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3fc70-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="3fc70-533">Możesz docierać do Scotta na swoim blogu w OdeToCode lub Matta na Twitterze [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="3fc70-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>

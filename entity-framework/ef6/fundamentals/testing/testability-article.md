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
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="0c53e-102">Testowanie i Entity Framework 4,0</span><span class="sxs-lookup"><span data-stu-id="0c53e-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="0c53e-103">Scott</span><span class="sxs-lookup"><span data-stu-id="0c53e-103">Scott Allen</span></span>

<span data-ttu-id="0c53e-104">Opublikowano: 2010 maja</span><span class="sxs-lookup"><span data-stu-id="0c53e-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="0c53e-105">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="0c53e-105">Introduction</span></span>

<span data-ttu-id="0c53e-106">W tym dokumencie opisano i pokazano, jak napisać kod weryfikowalne z ADO.NET Entity Framework 4,0 i Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="0c53e-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="0c53e-107">Ten dokument nie próbuje skupić się na określonej metodologii testowania, na przykład w przypadku projektowania opartego na testach (TDD) lub projektu opartego na zachowaniach (BDD).</span><span class="sxs-lookup"><span data-stu-id="0c53e-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="0c53e-108">Zamiast tego ten dokument koncentruje się na sposobach pisania kodu, który korzysta z ADO.NET Entity Framework jeszcze łatwo izolować i przetestować w zautomatyzowany sposób.</span><span class="sxs-lookup"><span data-stu-id="0c53e-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="0c53e-109">Zapoznajmy się z typowymi wzorcami projektowymi, które ułatwiają testowanie scenariuszy dostępu do danych, i zobacz, jak zastosować te wzorce podczas korzystania z platformy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="0c53e-110">Poszukajmy również określonych funkcji platformy, aby zobaczyć, jak te funkcje mogą funkcjonować w kodzie weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="0c53e-111">Co to jest kod weryfikowalne?</span><span class="sxs-lookup"><span data-stu-id="0c53e-111">What Is Testable Code?</span></span>

<span data-ttu-id="0c53e-112">Możliwość weryfikowania oprogramowania przy użyciu zautomatyzowanych testów jednostkowych oferuje wiele pożądanych korzyści.</span><span class="sxs-lookup"><span data-stu-id="0c53e-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="0c53e-113">Każdy wie, że dobre testy zmniejszają liczbę wad oprogramowania w aplikacji i zwiększają jakość aplikacji, ale ich testy jednostkowe nie przechodzą znacznie poza znalezieniem błędów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="0c53e-114">Dobry zestaw testów jednostkowych pozwala zespołowi programistycznemu zaoszczędzić czas i zachować kontrolę nad tworzonym przez nie oprogramowaniem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="0c53e-115">Zespół może wprowadzać zmiany w istniejącym kodzie, refaktoryzacji, przeprojektowaniu i restrukturyzacji oprogramowania w celu spełnienia nowych wymagań oraz dodawać nowe składniki do aplikacji, jednocześnie wiedząc, że zestaw testów może zweryfikować zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="0c53e-116">Testy jednostkowe są częścią szybkiego cyklu opinii, aby ułatwić zmianę i zachować łatwość utrzymania oprogramowania w miarę wzrostu złożoności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="0c53e-117">Jednak testy jednostkowe są dostarczane z ceną.</span><span class="sxs-lookup"><span data-stu-id="0c53e-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="0c53e-118">Zespół musi zainwestować czas, aby utworzyć i zachować testy jednostkowe.</span><span class="sxs-lookup"><span data-stu-id="0c53e-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="0c53e-119">Wielkość nakładu pracy wymaganego do utworzenia tych testów jest bezpośrednio związana z **testowaniem** podstawowego oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="0c53e-120">Jak łatwe jest przetestowanie oprogramowania?</span><span class="sxs-lookup"><span data-stu-id="0c53e-120">How easy is the software to test?</span></span> <span data-ttu-id="0c53e-121">Zespół, który opracowuje oprogramowanie z myślą o testowaniu, będzie tworzyć skuteczne testy szybciej niż zespół pracujący z oprogramowaniem weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="0c53e-122">Firma Microsoft zaprojektowała ADO.NET Entity Framework 4,0 (EF4) z myślą o testowaniu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="0c53e-123">Nie oznacza to, że deweloperzy będą pisać testy jednostkowe względem samego kodu struktury.</span><span class="sxs-lookup"><span data-stu-id="0c53e-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="0c53e-124">Zamiast tego cele testowania dla EF4 ułatwiają tworzenie kodu weryfikowalne, który kompiluje się na podstawie struktury.</span><span class="sxs-lookup"><span data-stu-id="0c53e-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="0c53e-125">Przed przystąpieniem do określonych przykładów wartościowa się zrozumienie jakości kodu weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="0c53e-126">Jakość kodu weryfikowalne</span><span class="sxs-lookup"><span data-stu-id="0c53e-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="0c53e-127">Kod, który jest łatwy do przetestowania, zawsze wykazuje co najmniej dwie cechy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="0c53e-128">Najpierw kod weryfikowalne jest łatwy do **obserwowania**.</span><span class="sxs-lookup"><span data-stu-id="0c53e-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="0c53e-129">W przypadku niektórych zestawów danych wejściowych powinno być łatwe przestrzeganie danych wyjściowych kodu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="0c53e-130">Na przykład testowanie następującej metody jest proste, ponieważ metoda bezpośrednio zwraca wynik obliczenia.</span><span class="sxs-lookup"><span data-stu-id="0c53e-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="0c53e-131">Testowanie metody jest trudne, jeśli metoda zapisuje obliczoną wartość w gnieździe sieciowym, tabeli bazy danych lub pliku, takim jak poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="0c53e-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="0c53e-132">Test musi wykonać dodatkową prace, aby pobrać wartość.</span><span class="sxs-lookup"><span data-stu-id="0c53e-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="0c53e-133">Po drugie, kod weryfikowalne jest łatwo **odizolowany**.</span><span class="sxs-lookup"><span data-stu-id="0c53e-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="0c53e-134">Użyjmy poniższego pseudo kodu jako nieprawidłowego przykładu kodu weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="0c53e-135">Metoda jest łatwa do obserwowania — możemy przekazać zasady ubezpieczenia i sprawdzić, czy wartość zwracana jest zgodna z oczekiwanym wynikiem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="0c53e-136">Jednak w celu przetestowania metody należy zainstalować bazę danych z odpowiednim schematem i skonfigurować serwer SMTP na wypadek próby wysłania wiadomości e-mail przez metodę.</span><span class="sxs-lookup"><span data-stu-id="0c53e-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="0c53e-137">Test jednostkowy chce jedynie sprawdzić logikę obliczeń wewnątrz metody, ale test może się nie powieść, ponieważ serwer poczty e-mail jest w trybie offline lub serwer bazy danych został przeniesiony.</span><span class="sxs-lookup"><span data-stu-id="0c53e-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="0c53e-138">Obie te błędy nie są związane z zachowaniem, które test chce zweryfikować.</span><span class="sxs-lookup"><span data-stu-id="0c53e-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="0c53e-139">Zachowanie jest trudne do odizolowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="0c53e-140">Deweloperzy oprogramowania, którzy dążą do pisania kodu weryfikowalne często dążą do utrzymania rozdzielenia problemów w kodzie, który pisze.</span><span class="sxs-lookup"><span data-stu-id="0c53e-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="0c53e-141">Powyższa metoda powinna skupić się na obliczeniach firmy i delegować szczegóły implementacji bazy danych i wiadomości e-mail do innych składników.</span><span class="sxs-lookup"><span data-stu-id="0c53e-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="0c53e-142">Robert C. Martin wywołuje tę samą regułę odpowiedzialności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="0c53e-143">Obiekt powinien hermetyzować jedną z wąskich obowiązków, takich jak obliczanie wartości zasad.</span><span class="sxs-lookup"><span data-stu-id="0c53e-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="0c53e-144">Wszystkie inne bazy danych i służbowe powiadomienia powinny być odpowiedzialne za inne obiekty.</span><span class="sxs-lookup"><span data-stu-id="0c53e-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="0c53e-145">Kod zapisany w ten sposób jest łatwiejszy do odizolowania, ponieważ koncentruje się na pojedynczym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="0c53e-146">W programie .NET mamy streszczenia, które muszą przestrzegać jednej zasady odpowiedzialności i uzyskać izolację.</span><span class="sxs-lookup"><span data-stu-id="0c53e-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="0c53e-147">Możemy użyć definicji interfejsu i wymusić użycie przez kod abstrakcji interfejsu zamiast konkretnego typu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="0c53e-148">W dalszej części tego dokumentu zobaczymy, jak metoda, taka jak niewłaściwy przykład przedstawiony powyżej, może współdziałać z interfejsami, które *wyglądają* podobnie do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="0c53e-149">W czasie testu można jednak zastąpić implementację fikcyjną, która nie komunikuje się z bazą danych, ale zamiast tego przechowuje dane w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0c53e-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="0c53e-150">Ta implementacja fikcyjna izoluje kod z niepowiązanych problemów w kodzie dostępu do danych lub w konfiguracji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="0c53e-151">Istnieje dodatkowe korzyści związane z izolacją.</span><span class="sxs-lookup"><span data-stu-id="0c53e-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="0c53e-152">Obliczenia biznesowe w ostatniej metodzie powinny trwać tylko kilka milisekund, ale test może być wykonywany przez kilka sekund w miarę przeskoków kodu między siecią i rozmowy z różnymi serwerami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="0c53e-153">Testy jednostkowe powinny działać szybko, aby ułatwić małym zmianom.</span><span class="sxs-lookup"><span data-stu-id="0c53e-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="0c53e-154">Testy jednostkowe powinny również być powtarzane i kończyć się niepowodzeniem, ponieważ wystąpił problem ze składnikiem niezwiązanym z testem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="0c53e-155">Pisanie kodu, który jest łatwy do obserwowania i wyodrębnienia oznacza, że deweloperzy będą mieli łatwiejszy czas na zapisanie testów dla kodu, poświęcasz mniej czasu na przeprowadzenie testów i co ważniejsze, Poświęcaj mniej czasu na błędy śledzenia błędów, które nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="0c53e-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="0c53e-156">Miejmy nadzieję można dowiedzieć się, jakie są korzyści z testowania i poznać jakości, które weryfikowalne kod.</span><span class="sxs-lookup"><span data-stu-id="0c53e-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="0c53e-157">Zamierzamy się dowiedzieć, jak napisać kod, który współpracuje z usługą EF4, aby zapisać dane w bazie danych, a pozostało zauważalne i łatwe do odizolowania, ale najpierw zawężamy nasz skup, aby omówić projekty weryfikowalne na potrzeby dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="0c53e-158">Wzorce projektowe dla trwałości danych</span><span class="sxs-lookup"><span data-stu-id="0c53e-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="0c53e-159">Oba z nieprawidłowych przykładów przedstawionych wcześniej miały zbyt wiele obowiązków.</span><span class="sxs-lookup"><span data-stu-id="0c53e-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="0c53e-160">Pierwszy zły przykład wymagał wykonania obliczeń *i* zapisu w pliku.</span><span class="sxs-lookup"><span data-stu-id="0c53e-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="0c53e-161">Drugi zły przykład wymagał odczytania danych z bazy danych *i* wykonania obliczeń w firmie *oraz* wysłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="0c53e-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="0c53e-162">Przez projektowanie mniejszych metod, które dzielą się problemami i delegowanie odpowiedzialności za inne składniki, będziesz mieć wspaniałe podejścia do pisania kodu weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="0c53e-163">Celem jest tworzenie funkcji przez redagowanie akcji z małych i ukierunkowanych abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="0c53e-164">Gdy chodzi o trwałość danych, są to popularne i uporządkowane abstrakcje, więc są one takie same, jak w przypadku wzorców projektowych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="0c53e-165">Wzorce książek Fowleraowych z rozliczeniami w przedsiębiorstwie były pierwszym działaniem opisującym te wzorce na wydruku.</span><span class="sxs-lookup"><span data-stu-id="0c53e-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="0c53e-166">Udostępnimy krótkie opisy tych wzorców w poniższych sekcjach, zanim pokażemy, jak te ADO.NET Entity Framework implementują i współdziałają z tymi wzorcami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="0c53e-167">Wzorzec repozytorium</span><span class="sxs-lookup"><span data-stu-id="0c53e-167">The Repository Pattern</span></span>

<span data-ttu-id="0c53e-168">Fowlera mówi repozytorium "koryguje między warstwami mapowania domeny i danych przy użyciu interfejsu przypominającego gromadzenie do uzyskiwania dostępu do obiektów domeny".</span><span class="sxs-lookup"><span data-stu-id="0c53e-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="0c53e-169">Celem wzorca repozytorium jest odizolowanie kodu od minutiae dostępu do danych, a w przypadku wcześniejszej izolacji jest wymagana cecha do testowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="0c53e-170">Klucz odizolowany polega na tym, jak repozytorium uwidacznia obiekty przy użyciu interfejsu podobnej do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="0c53e-171">Logika, którą zapisujesz do korzystania z repozytorium, nie ma znaczenia, w jaki sposób repozytorium będzie zmaterializowania żądane obiekty.</span><span class="sxs-lookup"><span data-stu-id="0c53e-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="0c53e-172">Repozytorium może komunikować się z bazą danych lub po prostu zwraca obiekty z kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0c53e-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="0c53e-173">Każdy kod musi wiedzieć, że repozytorium jest utrzymywane do obsługi kolekcji i można pobrać, dodać i usunąć obiekty z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="0c53e-174">W istniejących aplikacjach .NET konkretne repozytorium często dziedziczy z interfejsu generycznego, takiego jak następujące:</span><span class="sxs-lookup"><span data-stu-id="0c53e-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="0c53e-175">Wprowadzimy kilka zmian w definicji interfejsu, gdy udostępnimy implementację EF4, ale podstawowa koncepcja pozostaje taka sama.</span><span class="sxs-lookup"><span data-stu-id="0c53e-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="0c53e-176">Kod może użyć konkretnego repozytorium implementującego ten interfejs, aby pobrać jednostkę według wartości klucza podstawowego, pobrać kolekcję jednostek na podstawie oceny predykatu lub po prostu pobrać wszystkie dostępne jednostki.</span><span class="sxs-lookup"><span data-stu-id="0c53e-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="0c53e-177">Kod może również dodawać i usuwać jednostki za pomocą interfejsu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="0c53e-178">Mając IRepository obiektów pracowników, kod może wykonać następujące operacje.</span><span class="sxs-lookup"><span data-stu-id="0c53e-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="0c53e-179">Ponieważ kod używa interfejsu (IRepository pracownika), możemy udostępnić kod z różnymi implementacjami interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="0c53e-180">Jedną z implementacji może być implementacja EF4 i utrwalanie obiektów w bazie danych Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0c53e-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="0c53e-181">Inna implementacja (używana podczas testowania) może być obsługiwana przez listę obiektów pracowników w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0c53e-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="0c53e-182">Interfejs pomoże uzyskać izolację w kodzie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="0c53e-183">Zwróć uwagę, że interfejs IRepository&lt;T&gt; nie uwidacznia operacji zapisywania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="0c53e-184">Jak aktualizować istniejące obiekty?</span><span class="sxs-lookup"><span data-stu-id="0c53e-184">How do we update existing objects?</span></span> <span data-ttu-id="0c53e-185">Mogą występować w definicjach IRepository, które obejmują operację zapisywania, a implementacje tych repozytoriów będą musiały natychmiast utrzymać obiekt w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="0c53e-186">Jednak w wielu aplikacjach nie chcemy, aby obiekty były utrwalane pojedynczo.</span><span class="sxs-lookup"><span data-stu-id="0c53e-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="0c53e-187">Zamiast tego chcemy przenieść obiekty do życia, prawdopodobnie z różnych repozytoriów, zmodyfikować te obiekty jako część działania biznesowego, a następnie utrwalać wszystkie obiekty w ramach jednej, niepodzielnej operacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="0c53e-188">Na szczęście istnieje wzorzec zezwalający na zachowanie tego typu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="0c53e-189">Wzorzec jednostki pracy</span><span class="sxs-lookup"><span data-stu-id="0c53e-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="0c53e-190">Fowlera oznacza, że jednostka pracy będzie obsługiwać listę obiektów, na które ma wpływ transakcja biznesowa, i koordynuje wpisywanie zmian i rozwiązywanie problemów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="0c53e-191">Jest odpowiedzialna za jednostkę pracy, która śledzi zmiany w obiektach, które doprowadzamy do życia z repozytorium, i utrzymuje wszelkie zmiany wprowadzone w obiektach, gdy poinformujemy o jednostce pracy, aby zatwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="0c53e-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="0c53e-192">Jest również odpowiedzialna za jednostkę pracy, która zajmie się nowymi obiektami, które zostały dodane do wszystkich repozytoriów i wstawia obiekty do bazy danych, a także do zarządzania usuwaniem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="0c53e-193">Jeśli kiedykolwiek dojdziesz do pracy z zestawami danych ADO.NET, zobaczysz, że masz już doświadczenie ze wzorca jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="0c53e-194">Zestawy danych ADO.NET umożliwiają śledzenie naszych aktualizacji, usunięć i wstawiania obiektów DataRow i mogą być (za pomocą TableAdapter) uzgadniają wszystkie nasze zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="0c53e-195">Jednak model obiektów DataSet ma odłączony podzestaw źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="0c53e-196">Wzorzec jednostki pracy ma takie samo zachowanie, ale współpracuje z obiektami biznesowymi i obiektami domen, które są izolowane od kodu dostępu do danych i nie wiedząc bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="0c53e-197">Streszczenie modelu jednostki pracy w kodzie .NET może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="0c53e-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="0c53e-198">Przez ujawnienie odwołań do repozytorium z jednostki pracy możemy zapewnić, że pojedynczy obiekt jednostki pracy ma możliwość śledzenia wszystkich jednostek w ramach transakcji biznesowej.</span><span class="sxs-lookup"><span data-stu-id="0c53e-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="0c53e-199">Implementacja metody zatwierdzania dla rzeczywistej jednostki pracy polega na tym, że wszystko jest wykonywane w celu uzgodnienia zmian w pamięci z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="0c53e-200">Mając odwołanie IUnitOfWork, kod może wprowadzać zmiany w obiektach biznesowych pobieranych z jednego lub większej liczby repozytoriów i zapisywać wszystkie zmiany przy użyciu niepodzielnej operacji zatwierdzania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="0c53e-201">Wzorzec obciążenia z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="0c53e-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="0c53e-202">Fowlera używa załadowania nazwy z opóźnieniem do opisywania "obiektu, który nie zawiera wszystkich potrzebnych danych, ale wie, jak to zrobić".</span><span class="sxs-lookup"><span data-stu-id="0c53e-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="0c53e-203">Przezroczyste ładowanie z opóźnieniem jest ważną funkcją do tworzenia kodu biznesowego weryfikowalne i pracy z relacyjną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="0c53e-204">Na przykład rozważmy poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="0c53e-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="0c53e-205">Jak jest wypełniona kolekcja TimeCards?</span><span class="sxs-lookup"><span data-stu-id="0c53e-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="0c53e-206">Istnieją dwie możliwe odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0c53e-206">There are two possible answers.</span></span> <span data-ttu-id="0c53e-207">Jedną z odpowiedzi jest to, że repozytorium pracowników, gdy zostanie wyświetlony monit o pobranie pracownika, generuje zapytanie w celu pobrania pracownika wraz z informacjami o karcie czasowej skojarzonym z pracownikami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="0c53e-208">W relacyjnych bazach danych zwykle wymaga zapytania z klauzulą JOIN i może spowodować pobranie większej ilości informacji niż wymaga aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="0c53e-209">Co zrobić, jeśli aplikacja nigdy nie musi dotykać właściwości TimeCards?</span><span class="sxs-lookup"><span data-stu-id="0c53e-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="0c53e-210">Drugą odpowiedzią jest załadowanie właściwości TimeCards "na żądanie".</span><span class="sxs-lookup"><span data-stu-id="0c53e-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="0c53e-211">To ładowanie z opóźnieniem jest niejawne i niewidoczne dla logiki biznesowej, ponieważ kod nie wywołuje specjalnych interfejsów API w celu pobrania informacji o karcie czasowej.</span><span class="sxs-lookup"><span data-stu-id="0c53e-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="0c53e-212">Kod przyjmuje, że informacje o karcie czasowej są obecne, gdy jest to zajdzie taka potrzeba.</span><span class="sxs-lookup"><span data-stu-id="0c53e-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="0c53e-213">Istnieje pewna magiczna Metoda ładowania z opóźnieniem, która zwykle obejmuje przechwycenie wywołania metody przez środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="0c53e-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="0c53e-214">Kod przechwytywania jest odpowiedzialny za rozmowę z bazą danych i pobieranie informacji o karcie czasu, pozostawiając logikę biznesową bezpłatnie do logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="0c53e-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="0c53e-215">Ten opóźniony magiczny ciąż umożliwia kod firmy odizolowanie od operacji pobierania danych i daje większy kod weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="0c53e-216">Wadą do obciążenia z opóźnieniem jest to, że gdy *aplikacja wymaga* informacji o karcie czasowej, kod wykona dodatkowe zapytanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="0c53e-217">Nie jest to istotne w przypadku wielu aplikacji, ale w przypadku aplikacji i aplikacji wrażliwych na wydajność przez wiele obiektów pracowników i wykonywania zapytania w celu pobrania kart czasu podczas każdej iteracji pętli (problem często określa się jako N + 1) problem z kwerendą), ładowanie z opóźnieniem jest przeciągane.</span><span class="sxs-lookup"><span data-stu-id="0c53e-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="0c53e-218">W tych scenariuszach aplikacja może chcieć eagerly dane karty czasu ładowania w najbardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="0c53e-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="0c53e-219">Na szczęście zobaczymy, jak EF4 obsługuje zarówno niejawne obciążenia z opóźnieniem, jak i wydajne obciążenia eager podczas przechodzenia do następnej sekcji i implementują te wzorce.</span><span class="sxs-lookup"><span data-stu-id="0c53e-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="0c53e-220">Implementowanie wzorców przy użyciu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0c53e-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="0c53e-221">Dobra wiadomość polega na tym, że wszystkie wzorce projektowe opisane w ostatniej sekcji są proste do wdrożenia z EF4.</span><span class="sxs-lookup"><span data-stu-id="0c53e-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="0c53e-222">Aby udowodnić, że będziemy używać prostej aplikacji ASP.NET MVC do edytowania i wyświetlania pracowników i skojarzonych z nimi informacji o karcie czasowej.</span><span class="sxs-lookup"><span data-stu-id="0c53e-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="0c53e-223">Zaczniemy od następującej "zwykłych starych obiektów CLR" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="0c53e-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="0c53e-224">Te definicje klas zmienią się nieco w miarę eksplorowania różnych metod i funkcji EF4, ale celem jest zachowanie tych klas jako trwałości ignorujących (PI), jak to możliwe.</span><span class="sxs-lookup"><span data-stu-id="0c53e-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="0c53e-225">Obiekt PI nie wie, *jak*, a nawet *czy*stan, w którym znajduje się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="0c53e-226">PI i POCOs są dostępne z oprogramowaniem weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="0c53e-227">Obiekty korzystające z podejścia POCO są mniej ograniczone, bardziej elastyczne i łatwiejsze do testowania, ponieważ mogą działać bez bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="0c53e-228">Korzystając z POCOs w miejscu, możemy utworzyć Entity Data Model (EDM) w programie Visual Studio (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="0c53e-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="0c53e-229">Nie będziemy używać modelu EDM do generowania kodu dla naszych jednostek.</span><span class="sxs-lookup"><span data-stu-id="0c53e-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="0c53e-230">Zamiast tego chcemy używać jednostek, które Lovingly łodzi.</span><span class="sxs-lookup"><span data-stu-id="0c53e-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="0c53e-231">Będziemy używać modelu EDM tylko do generowania schematu bazy danych i dostarczania metadanych EF4 potrzebnych do mapowania obiektów na bazę danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![Dr test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="0c53e-233">**Rysunek 1**</span><span class="sxs-lookup"><span data-stu-id="0c53e-233">**Figure 1**</span></span>

<span data-ttu-id="0c53e-234">Uwaga: Jeśli chcesz najpierw opracować model modelu EDM, możliwe jest wygenerowanie czystego, POCOego kodu z modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="0c53e-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="0c53e-235">Można to zrobić za pomocą rozszerzenia programu Visual Studio 2010 dostarczonego przez zespół obsługujący programowanie danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="0c53e-236">Aby pobrać rozszerzenie, uruchom Menedżera rozszerzeń z menu Narzędzia w programie Visual Studio i Przeszukaj galerię online szablonów dla "POCO" (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="0c53e-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="0c53e-237">Istnieje kilka szablonów POCO dostępnych dla EF.</span><span class="sxs-lookup"><span data-stu-id="0c53e-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="0c53e-238">Aby uzyskać więcej informacji na temat korzystania z szablonu, zobacz " [Przewodnik: poco Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="0c53e-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![Dr test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="0c53e-240">**Rysunek 2**</span><span class="sxs-lookup"><span data-stu-id="0c53e-240">**Figure 2**</span></span>

<span data-ttu-id="0c53e-241">Z tego POCOego punktu początkowego będziemy eksplorować dwa różne podejścia do kodu weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="0c53e-242">Pierwsze podejście wywołuje podejście EF, ponieważ wykorzystuje abstrakcje z interfejsu API Entity Framework do implementowania jednostek pracy i repozytoriów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="0c53e-243">W drugim podejściu utworzymy własne abstrakcyjne streszczenie repozytorium, a następnie zobaczysz zalety i wady każdego podejścia.</span><span class="sxs-lookup"><span data-stu-id="0c53e-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="0c53e-244">Zaczniemy od poznawania metody EF.</span><span class="sxs-lookup"><span data-stu-id="0c53e-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="0c53e-245">Implementacja skoncentrowana na EF</span><span class="sxs-lookup"><span data-stu-id="0c53e-245">An EF Centric Implementation</span></span>

<span data-ttu-id="0c53e-246">Rozważmy następującą akcję kontrolera z projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0c53e-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="0c53e-247">Akcja pobiera obiekt Employee i zwraca wynik, aby wyświetlić szczegółowy widok pracownika.</span><span class="sxs-lookup"><span data-stu-id="0c53e-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="0c53e-248">Czy kod jest weryfikowalne?</span><span class="sxs-lookup"><span data-stu-id="0c53e-248">Is the code testable?</span></span> <span data-ttu-id="0c53e-249">Istnieją co najmniej dwa testy wymagające zweryfikowania zachowania działania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="0c53e-250">Najpierw chcemy sprawdzić, czy akcja zwraca poprawny widok — prosty test.</span><span class="sxs-lookup"><span data-stu-id="0c53e-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="0c53e-251">Chcemy również napisać test, aby sprawdzić, czy akcja pobiera odpowiedniego pracownika, i chcemy to zrobić bez wykonywania kodu w celu wykonania zapytania względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="0c53e-252">Pamiętaj, że chcemy odizolować kod pod testem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="0c53e-253">Izolacja zapewni, że test nie powiedzie się z powodu błędu w kodzie dostępu do danych lub konfiguracji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="0c53e-254">Jeśli test zakończy się niepowodzeniem, firma Microsoft wie, że mamy usterkę w logice kontrolera, a nie w pewnym składniku systemu niższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="0c53e-255">Aby uzyskać izolację, będziemy potrzebować pewnych streszczeń, takich jak interfejsy przedstawione wcześniej dla repozytoriów i jednostek pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="0c53e-256">Należy pamiętać, że wzorzec repozytorium jest przeznaczony do korygowania między obiektami domeny a warstwą mapowania danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="0c53e-257">W tym scenariuszu EF4 *jest* warstwa mapowania danych i udostępnia już abstrakcję podobną do repozytorium o nazwie IObjectSet&lt;t&gt; (z przestrzeni nazw System. Data. Objects).</span><span class="sxs-lookup"><span data-stu-id="0c53e-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="0c53e-258">Definicja interfejsu wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="0c53e-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="0c53e-259">IObjectSet&lt;T&gt; spełnia wymagania dotyczące repozytorium, ponieważ przypomina kolekcję obiektów (za pośrednictwem interfejsu IEnumerable&lt;T&gt;) i oferuje metody dodawania i usuwania obiektów z symulowanej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="0c53e-260">Metody dołączania i odłączania uwidaczniają dodatkowe możliwości interfejsu API EF4.</span><span class="sxs-lookup"><span data-stu-id="0c53e-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="0c53e-261">Aby użyć IObjectSet&lt;T&gt; jako interfejsu dla repozytoriów, potrzebujemy abstrakcyjnej jednostki, aby powiązać repozytoria ze sobą.</span><span class="sxs-lookup"><span data-stu-id="0c53e-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="0c53e-262">Jedna konkretna implementacja tego interfejsu będzie komunikować się z SQL Server i będzie łatwa do tworzenia przy użyciu klasy ObjectContext z EF4.</span><span class="sxs-lookup"><span data-stu-id="0c53e-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="0c53e-263">Klasa ObjectContext jest rzeczywistą jednostką pracy w interfejsie API EF4.</span><span class="sxs-lookup"><span data-stu-id="0c53e-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="0c53e-264">IObjectSet&lt;T&gt; jest tak proste jak wywołanie metody metody CreateObjectSet obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="0c53e-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="0c53e-265">W tle środowisko Framework będzie używać metadanych dostarczonych w modelu EDM do tworzenia konkretnego obiektu ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="0c53e-266">Dojdziemy do powracania interfejsu IObjectSet&lt;T&gt;, ponieważ pomoże to w zachowaniu testów w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="0c53e-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="0c53e-267">Ta konkretna implementacja jest przydatna w środowisku produkcyjnym, ale musimy skupić się na tym, jak będziemy korzystać z naszego abstrakcji IUnitOfWork w celu ułatwienia testowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="0c53e-268">Test podwaja się</span><span class="sxs-lookup"><span data-stu-id="0c53e-268">The Test Doubles</span></span>

<span data-ttu-id="0c53e-269">Aby wyizolować akcję kontrolera, potrzebna jest możliwość przełączenia między rzeczywistą jednostką pracy (za pomocą obiektu ObjectContext) i testu podwójnego lub "fałszywego" jednostki pracy (operacje wykonywane w pamięci).</span><span class="sxs-lookup"><span data-stu-id="0c53e-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="0c53e-270">Typowym podejściem do przeprowadzenia tego typu przełączenia jest to, że kontroler MVC nie pozwala na utworzenie wystąpienia jednostki pracy, ale zamiast tego przekazać jednostkę pracy do kontrolera jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0c53e-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="0c53e-271">Powyższy kod jest przykładem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="0c53e-272">Nie zezwolimy, aby kontroler utworzył zależność (jednostkę pracy), ale wstrzyknąć zależność do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0c53e-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="0c53e-273">W projekcie MVC wspólne użycie fabryki kontrolerów niestandardowych w połączeniu z kontenerem Inversion of Control (IoC) w celu zautomatyzowania iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="0c53e-274">Te tematy wykraczają poza zakres tego artykułu, ale więcej informacji można znaleźć, postępując zgodnie z odwołaniami na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="0c53e-275">Fałszywa implementacja pracy, której można użyć do testowania, może wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="0c53e-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="0c53e-276">Zwróć uwagę na to, że sfałszowana jednostka pracy uwidacznia Właściwość zatwierdzona.</span><span class="sxs-lookup"><span data-stu-id="0c53e-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="0c53e-277">Czasami warto dodać do fałszywej klasy funkcje, które ułatwiają testowanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="0c53e-278">W takim przypadku można łatwo sprawdzić, czy kod zatwierdza jednostkę pracy, sprawdzając Właściwość zatwierdzona.</span><span class="sxs-lookup"><span data-stu-id="0c53e-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="0c53e-279">Potrzebujemy również fałszywych IObjectSet&lt;T&gt; do przechowywania obiektów pracowników i godzin w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0c53e-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="0c53e-280">Możemy udostępnić jedną implementację przy użyciu typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="0c53e-281">Ten test powoduje dwukrotne delegowanie większości zadań do bazowego obiektu HashSet —&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="0c53e-282">Należy zauważyć, że IObjectSet&lt;T&gt; wymaga ograniczenia generycznego wymuszające T jako klasę (typ referencyjny), a także wymusza implementację interfejsu IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="0c53e-283">Tworzenie kolekcji w pamięci jest łatwo widoczne jako interfejs IQueryable&lt;T&gt; przy użyciu standardowego operatora LINQ AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="0c53e-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="0c53e-284">Testy</span><span class="sxs-lookup"><span data-stu-id="0c53e-284">The Tests</span></span>

<span data-ttu-id="0c53e-285">Tradycyjne testy jednostkowe będą używały pojedynczej klasy testowej do przechowywania wszystkich testów dla wszystkich akcji w jednym kontrolerze MVC.</span><span class="sxs-lookup"><span data-stu-id="0c53e-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="0c53e-286">Możemy napisać te testy lub dowolnego typu testu jednostkowego, używając w pamięci sztucznie wbudowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="0c53e-287">Jednakże w tym artykule będziemy unikać podejścia do klasy testów monolitycznych, a zamiast tego grupować testy, aby skoncentrować się na określonej funkcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="0c53e-288">  Na przykład "Utwórz nowego pracownika" może być funkcją, którą chcemy przetestować, więc użyjemy pojedynczej klasy testowej, aby zweryfikować akcję pojedynczego kontrolera odpowiedzialną za utworzenie nowego pracownika.</span><span class="sxs-lookup"><span data-stu-id="0c53e-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="0c53e-289">Istnieje kilka typowych kodów konfiguracji potrzebnych dla wszystkich tych precyzyjnych klas testowych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="0c53e-290">Na przykład zawsze musimy utworzyć repozytoria w pamięci i fałszywą jednostkę pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="0c53e-291">Potrzebujemy również wystąpienia kontrolera pracownika z pustą jednostką pracy wstrzykiwaną.</span><span class="sxs-lookup"><span data-stu-id="0c53e-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="0c53e-292">Udostępnimy ten wspólny kod instalatora między klasami testów przy użyciu klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="0c53e-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="0c53e-293">"Matki" obiektu używanej w klasie bazowej jest jednym wspólnym wzorcem do tworzenia danych testowych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="0c53e-294">Obiekt matki zawiera metody fabryki do tworzenia wystąpień jednostek testowych do użycia w wielu zastosowaniach testów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="0c53e-295">Możemy użyć EmployeeControllerTestBase jako klasy bazowej dla wielu armatur testowych (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="0c53e-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="0c53e-296">Każda Armatura testowa testuje określoną akcję kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0c53e-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="0c53e-297">Na przykład jedna Armatura testowa koncentruje się na testowaniu akcji tworzenia używanej podczas żądania HTTP GET (aby wyświetlić widok służący do tworzenia pracownika), a inna Armatura koncentruje się na akcji tworzenia używanej w żądaniu POST protokołu HTTP (Aby uzyskać informacje przesłane przez Użytkownik, aby utworzyć pracownika).</span><span class="sxs-lookup"><span data-stu-id="0c53e-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="0c53e-298">Każda klasa pochodna jest odpowiedzialna za konfigurację wymaganą w określonym kontekście i aby zapewnić potwierdzenia, które są konieczne do zweryfikowania wyników dla danego kontekstu testu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![Dr test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="0c53e-300">**Rysunek 3**</span><span class="sxs-lookup"><span data-stu-id="0c53e-300">**Figure 3**</span></span>

<span data-ttu-id="0c53e-301">Konwencja nazewnictwa i styl testu przedstawiony w tym miejscu nie są wymagane dla kodu weryfikowalne — jest to tylko jedno podejście.</span><span class="sxs-lookup"><span data-stu-id="0c53e-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="0c53e-302">Na rysunku 4 przedstawiono testy działające w ramach wtyczki programu Test Runner dla programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="0c53e-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![Dr test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="0c53e-304">**Rysunek 4**</span><span class="sxs-lookup"><span data-stu-id="0c53e-304">**Figure 4**</span></span>

<span data-ttu-id="0c53e-305">Przy użyciu klasy bazowej do obsługi kodu konfiguracji udostępnionej testy jednostkowe dla każdej akcji kontrolera są małe i łatwe do zapisu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="0c53e-306">Testy będą wykonywane szybko (ponieważ wykonujemy operacje w pamięci) i nie powinny być przyczyną niepowodzenia ze względu na niepowiązaną infrastrukturę lub obawy dotyczące środowiska (ponieważ wyizolowano jednostkę testową).</span><span class="sxs-lookup"><span data-stu-id="0c53e-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="0c53e-307">W tych testach Klasa bazowa wykonuje większość czynności konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="0c53e-308">Należy pamiętać, że Konstruktor klasy bazowej tworzy repozytorium w pamięci, fałszywą jednostkę pracy i wystąpienie klasy EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="0c53e-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="0c53e-309">Klasa testowa pochodzi z tej klasy bazowej i koncentruje się na charakterystyce testów metody Create.</span><span class="sxs-lookup"><span data-stu-id="0c53e-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="0c53e-310">W takim przypadku określone czynności są przetestowane w dół do kroków "Rozmieść, Act i Assert", które będą widoczne w dowolnej procedurze testowania jednostkowego:</span><span class="sxs-lookup"><span data-stu-id="0c53e-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="0c53e-311">Utwórz obiekt newEmployee, aby symulować przychodzące dane.</span><span class="sxs-lookup"><span data-stu-id="0c53e-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="0c53e-312">Wywołaj akcję tworzenia EmployeeController i przekaż ją do newEmployee.</span><span class="sxs-lookup"><span data-stu-id="0c53e-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="0c53e-313">Upewnij się, że akcja Utwórz powoduje uzyskanie oczekiwanych wyników (pracownik zostanie wyświetlony w repozytorium).</span><span class="sxs-lookup"><span data-stu-id="0c53e-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="0c53e-314">Utworzone przez nas elementy umożliwiają przetestowanie dowolnych akcji EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="0c53e-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="0c53e-315">Na przykład podczas pisania testów dla akcji indeksu kontrolera pracownika możemy dziedziczyć z klasy bazowej testu, aby określić tę samą konfigurację podstawową dla naszych testów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="0c53e-316">Ponownie Klasa bazowa spowoduje utworzenie repozytorium w pamięci, fałszywej jednostki pracy i wystąpienia EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="0c53e-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="0c53e-317">Testy akcji index muszą jedynie skupić się na wywoływaniu akcji index i przetestowaniu jakości modelu, który zwraca akcja.</span><span class="sxs-lookup"><span data-stu-id="0c53e-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="0c53e-318">Testy tworzone za pomocą elementów sztucznych w pamięci są ukierunkowane na testowanie *stanu* oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="0c53e-319">Na przykład podczas testowania akcji tworzenia chcemy sprawdzić stan repozytorium po wykonaniu akcji tworzenia — czy repozytorium zawiera nowego pracownika?</span><span class="sxs-lookup"><span data-stu-id="0c53e-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="0c53e-320">Później przejdziemy do testowania opartego na interakcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="0c53e-321">Testowanie oparte na interakcji będzie pytać, czy testowany kod wywołał odpowiednie metody dla naszych obiektów i przeszedł poprawne parametry.</span><span class="sxs-lookup"><span data-stu-id="0c53e-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="0c53e-322">Na razie przejdziemy do drugiego wzorca projektowego — z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="0c53e-323">Ładowanie eager i ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="0c53e-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="0c53e-324">W pewnym momencie w aplikacji sieci Web ASP.NET MVC firma Microsoft może chcieć wyświetlić informacje pracownika i dołączyć karty czasu powiązane z pracownikami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="0c53e-325">Na przykład może być wyświetlany ekran podsumowania karty czasu, który pokazuje nazwisko pracownika i łączną liczbę kart czasu w systemie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="0c53e-326">Istnieje kilka metod zaimplementowania tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="0c53e-327">Rzut</span><span class="sxs-lookup"><span data-stu-id="0c53e-327">Projection</span></span>

<span data-ttu-id="0c53e-328">Jednym z prostych metod tworzenia podsumowania jest konstruowanie modelu przeznaczonego dla informacji, które chcemy wyświetlić w widoku.</span><span class="sxs-lookup"><span data-stu-id="0c53e-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="0c53e-329">W tym scenariuszu model może wyglądać podobnie do poniższego.</span><span class="sxs-lookup"><span data-stu-id="0c53e-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="0c53e-330">Zwróć uwagę, że EmployeeSummaryViewModel nie jest jednostką, innymi słowy, nie ma czegoś do utrwalenia w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="0c53e-331">Będziemy używać tej klasy tylko do losowego przechodzenia danych do widoku.</span><span class="sxs-lookup"><span data-stu-id="0c53e-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="0c53e-332">Model widoku jest podobny do obiektu transferu danych (DTO), ponieważ nie zawiera żadnego zachowania (brak metod) — tylko właściwości.</span><span class="sxs-lookup"><span data-stu-id="0c53e-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="0c53e-333">Właściwości będą zawierać dane, które muszą zostać przeniesione.</span><span class="sxs-lookup"><span data-stu-id="0c53e-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="0c53e-334">Można łatwo utworzyć wystąpienie tego modelu widoku przy użyciu standardowego operatora rzutowania LINQ — operator SELECT.</span><span class="sxs-lookup"><span data-stu-id="0c53e-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="0c53e-335">Istnieją dwie istotne funkcje w powyższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-335">There are two notable features to the above code.</span></span> <span data-ttu-id="0c53e-336">Pierwszy — kod jest łatwy do przetestowania, ponieważ jest nadal łatwo obserwować i izolować.</span><span class="sxs-lookup"><span data-stu-id="0c53e-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="0c53e-337">Operator SELECT działa tak samo jak w przypadku elementów sztucznych w pamięci, tak jak w przypadku rzeczywistej jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="0c53e-338">Druga istotna funkcja to sposób, w jaki kod umożliwia EF4om generowanie pojedynczego, wydajnego zapytania w celu zebrania informacji o pracownikach i kartach czasowych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="0c53e-339">Informacje o pracownikach i karcie czasu zostały załadowane do tego samego obiektu bez użycia żadnych specjalnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="0c53e-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="0c53e-340">Kod jedynie wyraził informacje wymagane przy użyciu standardowych operatorów LINQ, które pracują z źródłami danych w pamięci, a także ze zdalnymi źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="0c53e-341">EF4 było w stanie tłumaczyć drzewa wyrażeń wygenerowane przez zapytania LINQ i kompilator języka C\# do pojedynczego i wydajnego zapytania T-SQL.</span><span class="sxs-lookup"><span data-stu-id="0c53e-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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

<span data-ttu-id="0c53e-342">Istnieją inne sytuacje, w których nie chcemy współdziałać z modelem widoku ani obiektem DTO, ale z rzeczywistymi obiektami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="0c53e-343">Gdy wiemy, że potrzebujemy pracownika *i* kart czasu pracownika, możemy eagerly załadować powiązane dane w sposób niezależny i wydajny.</span><span class="sxs-lookup"><span data-stu-id="0c53e-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="0c53e-344">Jawne ładowanie eager</span><span class="sxs-lookup"><span data-stu-id="0c53e-344">Explicit Eager Loading</span></span>

<span data-ttu-id="0c53e-345">Jeśli chcemy eagerly informacje dotyczące jednostki związanej z ładowaniem, potrzebujemy pewnego mechanizmu logiki biznesowej (lub w tym scenariuszu, logiki akcji kontrolera) do wyrażania chęci do repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="0c53e-346">Klasa EF4 ObjectQuery&lt;T&gt; definiuje metodę include, aby określić powiązane obiekty do pobrania podczas zapytania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="0c53e-347">Należy pamiętać, że EF4 ObjectContext ujawnia jednostki za pośrednictwem konkretnej klasy ObjectSet&lt;T&gt;, która dziedziczy po ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="0c53e-348">  Jeśli korzystamy z elementów ObjectSet&lt;T&gt; References w naszej akcji kontrolera, możemy napisać następujący kod, aby określić eager obciążenie informacjami o karcie czasu dla każdego pracownika.</span><span class="sxs-lookup"><span data-stu-id="0c53e-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="0c53e-349">Jednak ze względu na to, że próbujemy zachować nasz kod weryfikowalne, nie ujawniamy elementu ObjectSet&lt;T&gt; spoza rzeczywistej jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="0c53e-350">Zamiast tego korzystamy z interfejsu IObjectSet&lt;T&gt;, który jest łatwiejszy do sfałszowania, ale IObjectSet&lt;T&gt; nie definiuje metody include.</span><span class="sxs-lookup"><span data-stu-id="0c53e-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="0c53e-351">Estetyki LINQ polega na tym, że możemy utworzyć własny operator include.</span><span class="sxs-lookup"><span data-stu-id="0c53e-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="0c53e-352">Zauważ, że ten operator include jest zdefiniowany jako metoda rozszerzająca dla IQueryable&lt;T&gt; zamiast IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="0c53e-353">Dzięki temu można korzystać z metody z szerszym zakresem możliwych typów, w tym IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;i ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="0c53e-354">W przypadku, gdy podstawową sekwencją nie jest oryginalna EF4 ObjectQuery&lt;T&gt;, nie ma żadnej szkody, a operator include to no-op.</span><span class="sxs-lookup"><span data-stu-id="0c53e-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="0c53e-355">Jeśli podstawową sekwencją *jest* ObjectQuery&lt;t&gt; (lub pochodna z ObjectQuery&lt;t&gt;), wówczas EF4 zobaczy nasze wymaganie dotyczące dodatkowych danych i formułuje odpowiednie zapytanie SQL.</span><span class="sxs-lookup"><span data-stu-id="0c53e-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="0c53e-356">Przy użyciu tego nowego operatora można jawnie zażądać eager ładowania informacji o karcie czasowej z repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="0c53e-357">W przypadku uruchomienia względem rzeczywistego obiektu ObjectContext kod generuje następujące pojedyncze zapytanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="0c53e-358">Zapytanie zbiera wystarczające informacje z bazy danych w jednej podróży, aby zmaterializowania obiekty pracowników i w pełni wypełnić swoją właściwość TimeCards.</span><span class="sxs-lookup"><span data-stu-id="0c53e-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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

<span data-ttu-id="0c53e-359">Doskonałe wiadomości to kod wewnątrz metody akcji, który pozostaje w pełni weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="0c53e-360">Nie musimy udostępniać żadnych dodatkowych funkcji dla naszych fałszywych, aby obsługiwać operator include.</span><span class="sxs-lookup"><span data-stu-id="0c53e-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="0c53e-361">W przypadku nieprawidłowych wiadomości musimy użyć operatora include wewnątrz kodu, który chciał zachować trwałość ignorujących.</span><span class="sxs-lookup"><span data-stu-id="0c53e-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="0c53e-362">Jest to podstawowy przykład typu kompromisów, które należy oszacować podczas kompilowania kodu weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="0c53e-363">Istnieją przypadki, w których konieczne jest poinformowanie o wycieku, poza abstrakcją repozytorium, aby osiągnąć cele wydajności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="0c53e-364">Alternatywą dla ładowania eager jest ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="0c53e-365">Ładowanie z opóźnieniem oznacza, że nasz kod firmy *nie* jest potrzebny do jawnego ogłaszania wymagania dotyczącego skojarzonych danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="0c53e-366">Zamiast tego używamy naszych jednostek w aplikacji, a jeśli potrzebujesz dodatkowych danych Entity Framework załadują dane na żądanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="0c53e-367">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="0c53e-367">Lazy Loading</span></span>

<span data-ttu-id="0c53e-368">Można łatwo przystąpić do scenariusza, w którym nie wiemy, jakie dane będą potrzebne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="0c53e-369">Firma Microsoft może wiedzieć, że logika wymaga obiektu Employee, ale może odgałęziać się do różnych ścieżek wykonywania, w których niektóre z tych ścieżek wymagają informacji o karcie czasowej od pracownika, a niektóre nie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="0c53e-370">Takie scenariusze są idealnym rozwiązaniem dla niejawnego ładowania z opóźnieniem, ponieważ dane są wyświetlane w sposób niezbędny.</span><span class="sxs-lookup"><span data-stu-id="0c53e-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="0c53e-371">Ładowanie z opóźnieniem, znane także jako odroczone ładowanie, nakłada pewne wymagania dotyczące obiektów Entity.</span><span class="sxs-lookup"><span data-stu-id="0c53e-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="0c53e-372">POCOs z prawdziwym ignorujących trwałości nie wpłynie na żadne wymagania z warstwy trwałości, ale prawdziwe ignorujących trwałości jest praktycznie niemożliwe.</span><span class="sxs-lookup"><span data-stu-id="0c53e-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="0c53e-373">  Zamiast tego mierzę trwałość ignorujących w względnych stopniach.</span><span class="sxs-lookup"><span data-stu-id="0c53e-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="0c53e-374">Jeśli konieczne jest dziedziczenie z klasy podstawowej zorientowanej na trwałość lub użycie wyspecjalizowanej kolekcji w celu osiągnięcia ładowania z opóźnieniem w POCOs, może to potrwać.</span><span class="sxs-lookup"><span data-stu-id="0c53e-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="0c53e-375">Na szczęście EF4 ma mniej niepożądane rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="0c53e-376">Praktycznie niewykrywalny</span><span class="sxs-lookup"><span data-stu-id="0c53e-376">Virtually Undetectable</span></span>

<span data-ttu-id="0c53e-377">W przypadku korzystania z obiektów POCO EF4 może dynamicznie generować serwery proxy środowiska uruchomieniowego dla jednostek.</span><span class="sxs-lookup"><span data-stu-id="0c53e-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="0c53e-378">Te serwery proxy w niewidoczny sposób zawijają POCOs z materiałami i oferują dodatkowe usługi, przechwytuje każdą właściwość pobieranie i ustawianie w celu wykonywania dodatkowych czynności.</span><span class="sxs-lookup"><span data-stu-id="0c53e-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="0c53e-379">Jedną z tych usług jest funkcja ładowania z opóźnieniem, którą szukamy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="0c53e-380">Inna usługa to wydajny mechanizm śledzenia zmian, który można rejestrować, gdy program zmienia wartości właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="0c53e-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="0c53e-381">Lista zmian jest używana przez obiekt ObjectContext w metodzie metody SaveChanges, aby zachować wszelkie zmodyfikowane jednostki za pomocą poleceń UPDATE.</span><span class="sxs-lookup"><span data-stu-id="0c53e-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="0c53e-382">Jednak aby te serwery proxy działały, muszą one mieć możliwość podłączania do właściwości operacji get i Set w jednostce, a serwery proxy osiągają ten cel przez zastępowanie wirtualnych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="0c53e-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="0c53e-383">Z tego względu, jeśli chcemy niejawnie załadować z opóźnieniem i wydajnym śledzeniem zmian, musimy wrócić do naszych definicji klas POCO i oznaczyć właściwości jako wirtualne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="0c53e-384">Nadal możemy powiedzieć, że jednostka Employee jest w większości ignorujących trwałość.</span><span class="sxs-lookup"><span data-stu-id="0c53e-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="0c53e-385">Jedynym wymaganiem jest użycie wirtualnych elementów członkowskich. nie ma to wpływu na testowanie kodu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="0c53e-386">Nie musimy dziedziczyć z żadnej specjalnej klasy bazowej, a nawet użyć specjalnej kolekcji przeznaczonej do ładowania z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="0c53e-387">Jak ilustruje kod, każda klasa implementująca interfejs ICollection&lt;T&gt; jest dostępna do przechowywania powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="0c53e-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="0c53e-388">Istnieje również jedna niewielka zmiana, którą trzeba wprowadzić w naszej jednostce pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="0c53e-389">Ładowanie z opóźnieniem jest domyślnie *wyłączone* podczas pracy bezpośrednio z obiektem ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="0c53e-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="0c53e-390">Istnieje właściwość, którą można ustawić we właściwości ContextOptions, aby umożliwić ładowanie odroczone i można ustawić tę właściwość wewnątrz rzeczywistej jednostki pracy, jeśli chcemy włączyć ładowanie z opóźnieniem wszędzie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="0c53e-391">Po włączeniu niejawnego ładowania z opóźnieniem kod aplikacji może korzystać z pracownika i kart czasu skojarzonych z pracownikami, a pozostałe blissfullyą wiedzą o pracy wymaganej do załadowania dodatkowych danych przez program Dr.</span><span class="sxs-lookup"><span data-stu-id="0c53e-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="0c53e-392">Ładowanie z opóźnieniem sprawia, że kod aplikacji jest łatwiejszy do zapisu, a przy użyciu Magic proxy kod pozostanie całkowicie weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="0c53e-393">W pamięci podręcznej jednostki pracy można po prostu wstępnie załadować fałszywe jednostki ze skojarzonymi danymi, jeśli są one odpowiednie podczas testu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="0c53e-394">W tym momencie będziemy zachęcać do kompilowania repozytoriów przy użyciu IObjectSet&lt;T&gt; i przyjrzeć się abstrakcjom, aby ukryć wszystkie oznaki struktury trwałości.</span><span class="sxs-lookup"><span data-stu-id="0c53e-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="0c53e-395">Repozytoria niestandardowe</span><span class="sxs-lookup"><span data-stu-id="0c53e-395">Custom Repositories</span></span>

<span data-ttu-id="0c53e-396">Po pierwszej prezentowaniu wzorca projektowego jednostki pracy w tym artykule podano przykładowy kod, który może wyglądać jak jednostka pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="0c53e-397">Zacznijmy od tego oryginalnego pomysłu przy użyciu scenariusza pracownika i karty czasu pracownika, z którym pracujemy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="0c53e-398">Główną różnicą między tą jednostką pracy i jednostką pracy utworzoną w ostatniej sekcji jest to, w jaki sposób ta jednostka pracy nie korzysta z żadnych streszczeń z EF4 Framework (IObjectSet&gt;&lt;T).</span><span class="sxs-lookup"><span data-stu-id="0c53e-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="0c53e-399">IObjectSet&lt;T&gt; działa dobrze jako interfejs repozytorium, ale interfejs API, który uwidacznia, może nie być idealnie wyrównany do potrzeb aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="0c53e-400">W tym nadchodzącym podejściu będziemy reprezentować repozytoria przy użyciu niestandardowych IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="0c53e-401">Wielu deweloperów, którzy przestrzegają projektu opartego na testach, projektu opartego na zadziałach i metod opartych na założeniach, preferują IRepository&lt;T&gt; z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="0c53e-402">Najpierw interfejs IRepository&lt;T&gt; reprezentuje warstwę "Ochrona przed uszkodzeniem".</span><span class="sxs-lookup"><span data-stu-id="0c53e-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="0c53e-403">Zgodnie z opisem przez Eric Evans w swojej organizacji projektowej opartej na domenie, warstwa antywirusowa utrzymuje kod domeny z interfejsów API infrastruktury, takich jak trwałość interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="0c53e-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="0c53e-404">Na koniec deweloperzy mogą tworzyć metody w repozytorium, które spełniają dokładne potrzeby aplikacji (jak zostało to wykryte podczas pisania testów).</span><span class="sxs-lookup"><span data-stu-id="0c53e-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="0c53e-405">Przykładowo może być często konieczne znalezienie pojedynczej jednostki przy użyciu wartości identyfikatora, aby można było dodać metodę FindById do interfejsu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="0c53e-406">  Nasze definicje IRepository&lt;T&gt; będą wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="0c53e-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="0c53e-407">Zwróć uwagę, że powrócimy do korzystania z interfejsu IQueryable&lt;T&gt;, aby uwidocznić kolekcje jednostek.</span><span class="sxs-lookup"><span data-stu-id="0c53e-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="0c53e-408">Interfejs IQueryable&lt;T&gt; umożliwia przekazanie drzew wyrażeń LINQ do dostawcy EF4 i nadanie dostawcy całościowego widoku zapytania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="0c53e-409">Druga opcja zwróci wartość IEnumerable&lt;T&gt;, co oznacza, że dostawca EF4 LINQ zobaczy tylko wyrażenia wbudowane w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="0c53e-410">Wszystkie grupowanie, porządkowanie i projekcje wykonywane poza repozytorium nie zostaną złożone do polecenia SQL wysłanego do bazy danych, co może obniżyć wydajność.</span><span class="sxs-lookup"><span data-stu-id="0c53e-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="0c53e-411">Z drugiej strony repozytorium zwracające tylko interfejs IEnumerable&lt;T&gt; wyniki nigdy nie przestaną być nowe polecenie SQL.</span><span class="sxs-lookup"><span data-stu-id="0c53e-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="0c53e-412">Obie metody będą działać, a oba podejścia pozostaną weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="0c53e-413">W celu zapewnienia pojedynczej implementacji interfejsu IRepository&lt;T&gt; przy użyciu typów ogólnych i interfejsu API EF4 ObjectContext należy zapewnić prostą implementację.</span><span class="sxs-lookup"><span data-stu-id="0c53e-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="0c53e-414">Podejście IRepository&lt;T&gt; daje nam dodatkową kontrolę nad naszymi zapytaniami, ponieważ klient musi wywołać metodę, aby uzyskać dostęp do jednostki.</span><span class="sxs-lookup"><span data-stu-id="0c53e-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="0c53e-415">Wewnątrz metody możemy udostępnić dodatkowe sprawdzenia i operatory LINQ w celu wymuszenia ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="0c53e-416">Zauważ, że interfejs ma dwa ograniczenia dla parametru typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="0c53e-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="0c53e-417">Pierwsze ograniczenie to wady klas wymagane przez obiekt ObjectSet&lt;T&gt;, a drugie ograniczenie wymusza wdrożenie IEntity — abstrakcję utworzoną dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="0c53e-418">Interfejs IEntity wymusza, aby jednostki miały Właściwość identyfikatora z możliwością odczytu, a następnie można użyć tej właściwości w metodzie FindById.</span><span class="sxs-lookup"><span data-stu-id="0c53e-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="0c53e-419">IEntity jest zdefiniowany przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="0c53e-420">IEntity może być traktowany jako niewielkie naruszenie trwałości ignorujących, ponieważ nasze jednostki są wymagane do zaimplementowania tego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="0c53e-421">Pamiętaj, że trwałość ignorujących ma wpływ na kompromisy, a w przypadku wielu funkcji FindById będzie przewyższał ograniczenie narzucone przez interfejs.</span><span class="sxs-lookup"><span data-stu-id="0c53e-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="0c53e-422">Interfejs nie ma wpływu na testowanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="0c53e-423">Utworzenie wystąpienia usługi Live IRepository&lt;T&gt; wymaga EF4 ObjectContext, więc konkretna implementacja pracy powinna zarządzać tworzeniem wystąpień.</span><span class="sxs-lookup"><span data-stu-id="0c53e-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="0c53e-424">Korzystanie z repozytorium niestandardowego</span><span class="sxs-lookup"><span data-stu-id="0c53e-424">Using the Custom Repository</span></span>

<span data-ttu-id="0c53e-425">Korzystanie z naszego niestandardowego repozytorium nie różni się znacznie od użycia repozytorium w oparciu o IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="0c53e-426">Zamiast stosować operatory LINQ bezpośrednio do właściwości, najpierw musimy wywołać jedną z metod tego repozytorium, aby uzyskać odwołanie do&gt; IQueryable&lt;T.</span><span class="sxs-lookup"><span data-stu-id="0c53e-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="0c53e-427">Zwróć uwagę, że niestandardowy operator dołączania będzie działał bez zmian.</span><span class="sxs-lookup"><span data-stu-id="0c53e-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="0c53e-428">Metoda FindById repozytorium usuwa zduplikowaną logikę z akcji próbujących pobrać pojedynczą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="0c53e-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="0c53e-429">Nie ma znaczącej różnicy w zakresie testowania dwóch rozważanych metod.</span><span class="sxs-lookup"><span data-stu-id="0c53e-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="0c53e-430">Możemy zapewnić fałszywe implementacje IRepository&lt;T&gt; przez budowanie konkretnych klas objętych przez HashSet —&lt;pracownika&gt; — podobnie jak w przypadku ostatniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="0c53e-431">Jednak niektórzy deweloperzy wolą używać obiektów tworzenia i makietowania struktur obiektów zamiast tworzyć sztuczne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="0c53e-432">Zobaczmy, jak używać makietów do testowania implementacji i omówienia różnic między fragmentami i elementami sztucznymi w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="0c53e-433">Testowanie za pomocą makiet</span><span class="sxs-lookup"><span data-stu-id="0c53e-433">Testing with Mocks</span></span>

<span data-ttu-id="0c53e-434">Istnieją różne podejścia do kompilowania, które Fowlera dzwoni "test Double".</span><span class="sxs-lookup"><span data-stu-id="0c53e-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="0c53e-435">Test Double (taki jak stunt Movie Double) jest obiektem, który kompiluje się do "w rzeczywistości" dla rzeczywistych obiektów produkcyjnych podczas testów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="0c53e-436">Utworzone repozytoria w pamięci są testami podwójne dla repozytoriów, które komunikują się SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0c53e-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="0c53e-437">Dowiesz się, jak używać tych testów podczas testów jednostkowych w celu odizolowania kodu i zapewnienia szybkiego uruchamiania testów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="0c53e-438">Testy zostały skompilowane z rzeczywistymi, działającymi implementacjami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="0c53e-439">W tle każdy z nich przechowuje konkretną kolekcję obiektów i dodaje i usuwa obiekty z tej kolekcji podczas manipulowania repozytorium podczas testu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="0c53e-440">Niektórzy deweloperzy, którzy lubią kompilację testu, w ten sposób podwajają się w ten sposób, korzystając z prawdziwych i wydajnych implementacji</span><span class="sxs-lookup"><span data-stu-id="0c53e-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="0c53e-441">  Ten test podwaja te elementy, które wywołujemy *fałszywe*.</span><span class="sxs-lookup"><span data-stu-id="0c53e-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="0c53e-442">Mają one pracę z implementacjami, ale nie są one wystarczające do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="0c53e-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="0c53e-443">Fałszywe repozytorium nie zapisuje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="0c53e-444">Fałszywy serwer SMTP nie wysyła w rzeczywistości wiadomości e-mail za pośrednictwem sieci.</span><span class="sxs-lookup"><span data-stu-id="0c53e-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="0c53e-445">Elementy w przeciwieństwie do elementów sztucznych</span><span class="sxs-lookup"><span data-stu-id="0c53e-445">Mocks versus Fakes</span></span>

<span data-ttu-id="0c53e-446">Istnieje inny typ testu podwójnie znany jako *makieta*.</span><span class="sxs-lookup"><span data-stu-id="0c53e-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="0c53e-447">Podczas gdy elementy sztuczne mają działające implementacje, makiety nie są implementowane.</span><span class="sxs-lookup"><span data-stu-id="0c53e-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="0c53e-448">Dzięki pomocy dotyczącej struktury obiektów makiety tworzymy te obiekty w czasie wykonywania i używają ich jako podwójnego przetestowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="0c53e-449">W tej sekcji będziemy korzystać z struktury "Moqing" "open source".</span><span class="sxs-lookup"><span data-stu-id="0c53e-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="0c53e-450">Oto prosty przykład użycia MOQ do dynamicznego tworzenia testów dla repozytorium pracowników.</span><span class="sxs-lookup"><span data-stu-id="0c53e-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="0c53e-451">Zachęcamy do Moqa o&lt;IRepositorye&gt;j przez pracownika, a następnie kompiluje ją dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="0c53e-452">Możemy uzyskać do obiektu implementującego IRepository&lt;pracownika&gt;, uzyskując dostęp do właściwości Object obiektu "makieta&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="0c53e-453">Jest to ten obiekt wewnętrzny, który można przekazać do naszych kontrolerów i nie będzie wiadomo, czy jest to test podwójny, czy rzeczywiste repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="0c53e-454">Możemy wywoływać metody na obiekcie tak samo, jak możemy wywołać metody dla obiektu z rzeczywistą implementacją.</span><span class="sxs-lookup"><span data-stu-id="0c53e-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="0c53e-455">Należy zastanawiać się, co będzie miało repozytorium makiety po wywołaniu metody Add.</span><span class="sxs-lookup"><span data-stu-id="0c53e-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="0c53e-456">Ponieważ nie istnieje implementacja za obiektem makiety, nic nie robi.</span><span class="sxs-lookup"><span data-stu-id="0c53e-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="0c53e-457">Nie ma żadnej konkretnej kolekcji w tle, podobnie jak w przypadku zapisana przez nas sfałszowanych, więc pracownik jest odrzucany.</span><span class="sxs-lookup"><span data-stu-id="0c53e-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="0c53e-458">Co z wartością zwracaną z FindById?</span><span class="sxs-lookup"><span data-stu-id="0c53e-458">What about the return value of FindById?</span></span> <span data-ttu-id="0c53e-459">W takim przypadku obiekt makiety robi tylko to, co może zrobić, co spowoduje zwrócenie wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="0c53e-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="0c53e-460">Ponieważ zwracamy typ referencyjny (pracownika), wartość zwracana jest wartością null.</span><span class="sxs-lookup"><span data-stu-id="0c53e-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="0c53e-461">Makiety mogą dźwiękować bezwartościowe; Istnieją jednak dwie inne funkcje makiet, o których nie podano informacji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="0c53e-462">Najpierw platforma MOQ rejestruje wszystkie wywołania wykonane na obiekcie makiety.</span><span class="sxs-lookup"><span data-stu-id="0c53e-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="0c53e-463">Później można polecić MOQ, jeśli ktoś wywołał metodę Add, lub jeśli ktoś wywołał metodę FindById.</span><span class="sxs-lookup"><span data-stu-id="0c53e-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="0c53e-464">Zobaczymy później, w jaki sposób możemy użyć tej funkcji nagrywania "czarnego pola" w testach.</span><span class="sxs-lookup"><span data-stu-id="0c53e-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="0c53e-465">Druga świetna funkcja polega na tym, jak możemy używać MOQ do programowania obiektu makiety z *oczekiwaniami*.</span><span class="sxs-lookup"><span data-stu-id="0c53e-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="0c53e-466">Oczekiwanie nakazuje obiektowi makiety, jak odpowiedzieć na daną interakcję.</span><span class="sxs-lookup"><span data-stu-id="0c53e-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="0c53e-467">Na przykład możemy zaprogramować oczekiwanie w naszym makietie i poinstruować go, aby zwracał obiekt Employee, gdy ktoś wywoła FindById.</span><span class="sxs-lookup"><span data-stu-id="0c53e-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="0c53e-468">Struktura MOQ używa interfejsu API Instalatora i wyrażeń lambda do zaprogramowania tych oczekiwań.</span><span class="sxs-lookup"><span data-stu-id="0c53e-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="0c53e-469">W tym przykładzie poprosił MOQ o dynamiczne skompilowanie repozytorium, a następnie programuje repozytorium z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="0c53e-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="0c53e-470">Oczekiwanie nakazuje obiektowi imitacji zwrócenie nowego obiektu pracownika z wartością identyfikatora 5, gdy ktoś wywoła metodę FindById, przekazując wartość 5.</span><span class="sxs-lookup"><span data-stu-id="0c53e-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="0c53e-471">Ten test kończy się niepowodzeniem i nie musimy kompilować pełnej implementacji dla fałszywych IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="0c53e-472">Ponownie odwiedzamy wcześniej wykonane testy i przeprowadzimy je do korzystania z form zamiast fałszywych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="0c53e-473">Podobnie jak wcześniej, użyjemy klasy bazowej do skonfigurowania wspólnych części infrastruktury potrzebnej dla wszystkich testów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0c53e-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="0c53e-474">Kod instalatora pozostaje w większości tego samego.</span><span class="sxs-lookup"><span data-stu-id="0c53e-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="0c53e-475">Zamiast używać fałszywych, będziemy używać MOQ do konstruowania obiektów makiety.</span><span class="sxs-lookup"><span data-stu-id="0c53e-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="0c53e-476">Klasa bazowa jest rozmieszczenia dla jednostki, która będzie zwracać repozytorium, gdy kod wywołuje właściwość Employees.</span><span class="sxs-lookup"><span data-stu-id="0c53e-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="0c53e-477">Pozostała część konfiguracji makiety będzie odbywać się w ramach armatury testowej przeznaczonych dla każdego konkretnego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="0c53e-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="0c53e-478">Na przykład, armatura testowa dla akcji indeks spowoduje skonfigurowanie repozytorium makiety, aby zwracało listę pracowników, gdy akcja wywoła metodę FindAll repozytorium.</span><span class="sxs-lookup"><span data-stu-id="0c53e-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="0c53e-479">Z wyjątkiem oczekiwań, nasze testy wyglądają podobnie jak testy, które wcześniej istniały.</span><span class="sxs-lookup"><span data-stu-id="0c53e-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="0c53e-480">Jednak dzięki możliwości rejestrowania struktury makiety możemy dochodzić do testowania z innego kąta.</span><span class="sxs-lookup"><span data-stu-id="0c53e-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="0c53e-481">Ta nowa perspektywa zostanie wyświetlona w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="0c53e-482">Stan a testowanie interakcji</span><span class="sxs-lookup"><span data-stu-id="0c53e-482">State versus Interaction Testing</span></span>

<span data-ttu-id="0c53e-483">Istnieją różne techniki, których można użyć do testowania oprogramowania z obiektami makiety.</span><span class="sxs-lookup"><span data-stu-id="0c53e-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="0c53e-484">Jednym z metod jest użycie testowania opartego na stanie, co jest gotowe do wykonania w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="0c53e-485">Testowanie na podstawie stanu pozwala na potwierdzenie stanu oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="0c53e-486">W ostatnim teście wywołana została metoda działania na kontrolerze i złożyła potwierdzenie dotyczące modelu, który powinien zostać skompilowany.</span><span class="sxs-lookup"><span data-stu-id="0c53e-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="0c53e-487">Oto kilka innych przykładów stanu testowania:</span><span class="sxs-lookup"><span data-stu-id="0c53e-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="0c53e-488">Sprawdź, czy repozytorium zawiera nowy obiekt Employee po wykonaniu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="0c53e-489">Sprawdź, czy model zawiera listę wszystkich pracowników po wykonaniu indeksu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="0c53e-490">Upewnij się, że repozytorium nie zawiera danego pracownika po wykonaniu usuwania.</span><span class="sxs-lookup"><span data-stu-id="0c53e-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="0c53e-491">Inne podejście, które zobaczysz, jest widoczne z obiektami makiety, aby zweryfikować *interakcje*.</span><span class="sxs-lookup"><span data-stu-id="0c53e-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="0c53e-492">Podczas testowania opartego na stanie są sprawdzane informacje o stanie obiektów, testowanie na podstawie interakcji sprawia, że obiekty są w interakcji z interakcją.</span><span class="sxs-lookup"><span data-stu-id="0c53e-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="0c53e-493">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0c53e-493">For example:</span></span>

-   <span data-ttu-id="0c53e-494">Upewnij się, że kontroler wywołuje metodę Add repozytorium podczas wykonywania tworzenia.</span><span class="sxs-lookup"><span data-stu-id="0c53e-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="0c53e-495">Sprawdź, czy kontroler wywołuje metodę FindAll repozytorium, gdy jest wykonywane indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="0c53e-496">Sprawdź, czy kontroler wywołuje metodę zatwierdzania jednostki pracy, aby zapisać zmiany po wykonaniu edycji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="0c53e-497">Testowanie interakcji często wymaga mniej danych testowych, ponieważ nie są one poking wewnątrz kolekcji i nie sprawdzają liczby.</span><span class="sxs-lookup"><span data-stu-id="0c53e-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="0c53e-498">Na przykład, jeśli wiemy, że akcja Details wywołuje metodę FindById repozytorium z poprawną wartością, to działanie prawdopodobnie działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="0c53e-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="0c53e-499">Możemy sprawdzić to zachowanie bez konfigurowania jakichkolwiek danych testowych do zwrócenia z FindById.</span><span class="sxs-lookup"><span data-stu-id="0c53e-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="0c53e-500">Jedyną konfiguracją wymaganą w powyższym zasobie testowym jest instalacja dostarczana przez klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="0c53e-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="0c53e-501">Gdy wywołamy akcję kontrolera, MOQ będzie rejestrować interakcje z repozytorium makiety.</span><span class="sxs-lookup"><span data-stu-id="0c53e-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="0c53e-502">Za pomocą weryfikowania interfejsu API MOQ można polecić MOQ, Jeśli kontroler wywołał FindById z prawidłową wartością identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="0c53e-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="0c53e-503">Jeśli kontroler nie wywołał metody lub wywołaniu metody z nieoczekiwaną wartością parametru, Metoda verify zgłosi wyjątek, a test zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="0c53e-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="0c53e-504">Oto inny przykład, aby sprawdzić, czy akcja tworzenia wywołuje zatwierdzenie w bieżącej jednostce pracy.</span><span class="sxs-lookup"><span data-stu-id="0c53e-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="0c53e-505">Jedno zagrożenie z testowaniem interakcji to tendencja do określania interakcji.</span><span class="sxs-lookup"><span data-stu-id="0c53e-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="0c53e-506">Możliwość rejestrowania i weryfikowania każdej interakcji z obiektem makiety nie oznacza, że test powinien weryfikować każdą interakcję.</span><span class="sxs-lookup"><span data-stu-id="0c53e-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="0c53e-507">Niektóre interakcje są szczegółami implementacji i weryfikują interakcje *wymagane* do spełnienia bieżącego testu.</span><span class="sxs-lookup"><span data-stu-id="0c53e-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="0c53e-508">Wybór między makietami lub sztucznymi jest w dużym stopniu zależny od testowanego systemu i preferencji osobistych (lub zespołów).</span><span class="sxs-lookup"><span data-stu-id="0c53e-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="0c53e-509">Obiekty makiety mogą znacząco zmniejszyć ilość kodu wymaganego do wdrożenia testów, ale nie każdy z nich jest wygodny dla oczekiwań programistycznych i sprawdza interakcje.</span><span class="sxs-lookup"><span data-stu-id="0c53e-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="0c53e-510">Wnioski</span><span class="sxs-lookup"><span data-stu-id="0c53e-510">Conclusions</span></span>

<span data-ttu-id="0c53e-511">W tym dokumencie przedstawiono kilka metod tworzenia kodu weryfikowalne przy użyciu Entity Framework ADO.NET w celu zapewnienia trwałości danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="0c53e-512">Możemy wykorzystać skompilowane streszczenia, takie jak IObjectSet&lt;T&gt;, lub utworzyć nasze streszczenie, takie jak IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="0c53e-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="0c53e-513">  W obu przypadkach wsparcie POCO w ADO.NET Entity Framework 4,0 umożliwia konsumentom tych streszczeń pozostawanie trwałych ignorujących i wysoce weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="0c53e-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="0c53e-514">Dodatkowe funkcje EF4, takie jak niejawne ładowanie z opóźnieniem, umożliwiają działanie kodu usługi biznesowej i aplikacji bez konieczności pojmowania się szczegółami relacyjnego magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="0c53e-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="0c53e-515">Na koniec, tworzone streszczenia są łatwe do zawinięcia lub fałszywe wewnątrz testów jednostkowych, a firma Microsoft może używać tych testów w celu szybkiego uruchamiania, wysoce odizolowanych i niezawodnych testów.</span><span class="sxs-lookup"><span data-stu-id="0c53e-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="0c53e-516">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0c53e-516">Additional Resources</span></span>

-   <span data-ttu-id="0c53e-517">Robert C. Martin, " [Pojedyncza zasada odpowiedzialności](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="0c53e-518">Fowlera Martin, [Katalog wzorców](https://www.martinfowler.com/eaaCatalog/index.html) ze *wzorców architektury aplikacji dla przedsiębiorstw*</span><span class="sxs-lookup"><span data-stu-id="0c53e-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="0c53e-519">Griffin Caprio, " [iniekcja zależności](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="0c53e-520">Blog dotyczący programowania danych, " [Przewodnik: Programowanie sterowane testami za pomocą Entity Framework 4,0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="0c53e-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="0c53e-521">Blog dotyczący programowania danych, " [używanie wzorców repozytorium i jednostki pracy z Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="0c53e-522">Aaron Jensen, " [Wprowadzenie specyfikacji maszyn](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="0c53e-523">Eric Lewandowski, " [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="0c53e-524">Eric Evans, " [Projektowanie oparte na domenie](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="0c53e-525">Fowlera Martin, " [imitacje nie są fragmentami](https://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="0c53e-526">Fowlera Martin, " [test Double](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="0c53e-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="0c53e-527">Moq</span><span class="sxs-lookup"><span data-stu-id="0c53e-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="0c53e-528">Biografii</span><span class="sxs-lookup"><span data-stu-id="0c53e-528">Biography</span></span>

<span data-ttu-id="0c53e-529">Scott, który jest członkiem działu technicznego w Pluralsight i założyciel OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="0c53e-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="0c53e-530">W trakcie komercyjnego programowania oprogramowania Scott pracował nad rozwiązaniami dla wszystkich urządzeń z 8-bitowymi urządzeniami osadzonymi w wysoce skalowalnych aplikacjach sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0c53e-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="0c53e-531">Możesz skontaktować się z Scott w swoim blogu w witrynie OdeToCode lub w serwisie Twitter w [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="0c53e-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>

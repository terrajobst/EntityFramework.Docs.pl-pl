---
title: Asynchroniczne zapytania i Zapisz - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 89c7b9d533d37b4c9e123f37d8ab27c67ba26cc8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668716"
---
# <a name="async-query-and-save"></a><span data-ttu-id="173a9-102">Asynchroniczne zapytania i Zapisz</span><span class="sxs-lookup"><span data-stu-id="173a9-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="173a9-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="173a9-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="173a9-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="173a9-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="173a9-105">EF6 wprowadzono obsługę dla zapytania asynchroniczne, a następnie Zapisz przy użyciu [async i await słowa kluczowe](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) wprowadzone w .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="173a9-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="173a9-106">Gdy nie wszystkie aplikacje mogą korzystać z asynchroniczności, może służyć do poprawy skalowalności krótki czas reakcji i serwera klienta podczas obsługi długotrwałych, sieci lub zadania I/O-powiązane z.</span><span class="sxs-lookup"><span data-stu-id="173a9-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="173a9-107">Kiedy należy używać naprawdę async</span><span class="sxs-lookup"><span data-stu-id="173a9-107">When to really use async</span></span>

<span data-ttu-id="173a9-108">Ten przewodnik ma na celu wprowadzenie pojęcia asynchronicznej w sposób, który ułatwia obserwować różnica między wykonywania programu synchronicznego i asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="173a9-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="173a9-109">W tym przewodniku nie jest przeznaczona do zilustrowanie jednego z kluczowych scenariuszy, których programowanie async oferuje korzyści.</span><span class="sxs-lookup"><span data-stu-id="173a9-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="173a9-110">Programowanie asynchroniczne koncentruje się głównie na zwalnianie bieżącego wątku zarządzanych (wątek uruchamianie kodu platformy .NET), wykonywanie innych zadań, aż do operacji, która nie wymaga żadnych czas obliczeń w z wątków zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="173a9-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="173a9-111">Na przykład podczas gdy aparat bazy danych jest przetwarzanie kwerendy nie ma nic do można przeprowadzić za pomocą kodu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="173a9-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="173a9-112">W aplikacjach klienckich (WinForms, WPF, itd.) bieżący wątek może służyć do zachowania dynamicznego interfejsu użytkownika podczas operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="173a9-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="173a9-113">W aplikacji serwera (ASP.NET itp.), wątek może służyć do przetwarzania innych żądań przychodzących — to może zmniejszyć użycie pamięci i/lub zwiększyć przepływność serwera.</span><span class="sxs-lookup"><span data-stu-id="173a9-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="173a9-114">W większości aplikacji przy użyciu async będzie zawierać żadnych zauważalnego korzyści, a nawet może być szkodliwe.</span><span class="sxs-lookup"><span data-stu-id="173a9-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="173a9-115">Umożliwia testy, profilowanie i zdroworozsądkowe mierzenie wpływu asynchronicznych w konkretnym scenariuszu przed zatwierdzeniem do niego.</span><span class="sxs-lookup"><span data-stu-id="173a9-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="173a9-116">Poniżej przedstawiono niektóre inne zasoby, aby dowiedzieć się więcej na temat async:</span><span class="sxs-lookup"><span data-stu-id="173a9-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="173a9-117">Omówienie Brandon Bray async/await w .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="173a9-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="173a9-118">[Programowanie asynchroniczne](https://msdn.microsoft.com/library/hh191443.aspx) stron w bibliotece MSDN</span><span class="sxs-lookup"><span data-stu-id="173a9-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="173a9-119">[Jak tworzyć ASP.NET sieci Web aplikacji przy użyciu Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (w tym pokaz serwera większą przepływność)</span><span class="sxs-lookup"><span data-stu-id="173a9-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="173a9-120">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="173a9-120">Create the model</span></span>

<span data-ttu-id="173a9-121">Będziemy używać [Code First przepływu pracy](~/ef6/modeling/code-first/workflows/new-database.md) utworzyć nasz model i wygenerować bazę danych, jednak w asynchronicznej funkcji będzie działać z wszystkich modeli EF, w tym te, utworzone za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="173a9-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="173a9-122">Utwórz aplikację Konsolową i wywołać go **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="173a9-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="173a9-123">Dodaj pakiet NuGet platformy EntityFramework</span><span class="sxs-lookup"><span data-stu-id="173a9-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="173a9-124">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **AsyncDemo** projektu</span><span class="sxs-lookup"><span data-stu-id="173a9-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="173a9-125">Wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="173a9-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="173a9-126">W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu</span><span class="sxs-lookup"><span data-stu-id="173a9-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="173a9-127">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="173a9-127">Click **Install**</span></span>
-   <span data-ttu-id="173a9-128">Dodaj **Model.cs** klasy następującą implementacją</span><span class="sxs-lookup"><span data-stu-id="173a9-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="173a9-129">Utwórz program synchroniczne</span><span class="sxs-lookup"><span data-stu-id="173a9-129">Create a synchronous program</span></span>

<span data-ttu-id="173a9-130">Teraz, gdy mamy już modelu platformy EF, umożliwia pisanie kodu w celu zastosowania do wykonania niektórych dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="173a9-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="173a9-131">Zastąp zawartość **Program.cs** następującym kodem</span><span class="sxs-lookup"><span data-stu-id="173a9-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="173a9-132">Ten kod wywołuje **PerformDatabaseOperations** metodę, która zapisuje nową **Blog** do bazy danych, a następnie pobiera wszystkie **blogi** z bazy danych i wysłania ich do **Konsoli**.</span><span class="sxs-lookup"><span data-stu-id="173a9-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="173a9-133">Dzięki temu program zapisuje oferty dnia, aby **konsoli**.</span><span class="sxs-lookup"><span data-stu-id="173a9-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="173a9-134">Ponieważ kod jest synchroniczne, możemy zaobserwować następujący przepływ wykonania, gdy Uruchamiamy program:</span><span class="sxs-lookup"><span data-stu-id="173a9-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="173a9-135">**SaveChanges** rozpoczyna się wypychania nowego **blogu** do bazy danych</span><span class="sxs-lookup"><span data-stu-id="173a9-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="173a9-136">**SaveChanges** kończy</span><span class="sxs-lookup"><span data-stu-id="173a9-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="173a9-137">Zapytanie o wszystkie **blogi** są wysyłane do bazy danych</span><span class="sxs-lookup"><span data-stu-id="173a9-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="173a9-138">Zapytanie zwraca, a wyniki są zapisywane do **konsoli**</span><span class="sxs-lookup"><span data-stu-id="173a9-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="173a9-139">Cytat dnia są zapisywane do **konsoli**</span><span class="sxs-lookup"><span data-stu-id="173a9-139">Quote of the day is written to **Console**</span></span>

![Synchronizacja danych wyjściowych](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="173a9-141">Dzięki czemu asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="173a9-141">Making it asynchronous</span></span>

<span data-ttu-id="173a9-142">Teraz, gdy nasz program działanie, możemy rozpocząć co użycie nowego async i await słów kluczowych.</span><span class="sxs-lookup"><span data-stu-id="173a9-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="173a9-143">Wprowadziliśmy następujące zmiany do pliku Program.cs</span><span class="sxs-lookup"><span data-stu-id="173a9-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="173a9-144">Wiersz 2: Za pomocą instrukcji for **System.Data.Entity** przestrzeni nazw daje nam dostęp do metod rozszerzenia asynchronicznych EF.</span><span class="sxs-lookup"><span data-stu-id="173a9-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="173a9-145">Wiersz 4: Za pomocą instrukcji for **System.Threading.Tasks** przestrzeni nazw pozwala nam korzystać **zadań** typu.</span><span class="sxs-lookup"><span data-stu-id="173a9-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="173a9-146">Wiersz 12 i 18: Firma Microsoft jest przechwytywany jako zadanie, które monitoruje postęp **PerformSomeDatabaseOperations** (wierszem 12) i następnie blokuje wykonywanie programu w tym zadań raz ukończone wszystkie prace dla program odbywa się (wiersz 18).</span><span class="sxs-lookup"><span data-stu-id="173a9-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="173a9-147">Wiersz 25: Udostępniliśmy aktualizacji **PerformSomeDatabaseOperations** być oznaczony jako **async** i zwracają **zadań**.</span><span class="sxs-lookup"><span data-stu-id="173a9-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="173a9-148">Wiersz 35: Teraz możemy wywołanie asynchroniczne wersję SaveChanges i oczekiwaniem na jej ukończenie.</span><span class="sxs-lookup"><span data-stu-id="173a9-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="173a9-149">Wiersz 42: Teraz dzwonimy wersji asynchronicznej tolist — i oczekiwaniem na wynik.</span><span class="sxs-lookup"><span data-stu-id="173a9-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="173a9-150">Pełną listę metod rozszerzenia dostępne w przestrzeni nazw System.Data.Entity można znaleźć klasy QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="173a9-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="173a9-151">*Należy także dodać "za pomocą System.Data.Entity" do sieci za pomocą instrukcji.*</span><span class="sxs-lookup"><span data-stu-id="173a9-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="173a9-152">Teraz, gdy kod jest asynchroniczna, można zaobserwować przepływu wykonywania różnych gdy Uruchamiamy program:</span><span class="sxs-lookup"><span data-stu-id="173a9-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="173a9-153">**SaveChanges** rozpoczyna się wypychania nowego **Blog** w bazie danych *po wysłaniu polecenia do bazy danych co obliczenia, konieczna jest w bieżącym wątku zarządzanych. **PerformDatabaseOperations** metoda zwróci wartość (nawet jeśli nie zostało zakończone, wykonując) i kontynuuje przepływu programu dla metody Main.*</span><span class="sxs-lookup"><span data-stu-id="173a9-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="173a9-154">**Cytat dnia są zapisywane do konsoli**
    *ponieważ nie ma więcej pracy w metody Main, wątków zarządzanych jest zablokowany na czas oczekiwania wywołania do momentu ukończenia operacji bazy danych. Po zakończeniu pozostałą część naszego **PerformDatabaseOperations** zostaną wykonane.*</span><span class="sxs-lookup"><span data-stu-id="173a9-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="173a9-155">**SaveChanges** kończy</span><span class="sxs-lookup"><span data-stu-id="173a9-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="173a9-156">Zapytanie o wszystkie **blogi** są wysyłane do bazy danych *ponownie wątków zarządzanych jest bezpłatna wykonywanie innych zadań, podczas gdy zapytania są przetwarzane w bazie danych. Od wszystkich innych wykonanie zostało ukończone, wątek będzie po prostu zatrzymanie przy wywołaniu oczekiwania jednak.*</span><span class="sxs-lookup"><span data-stu-id="173a9-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="173a9-157">Zapytanie zwraca, a wyniki są zapisywane do **konsoli**</span><span class="sxs-lookup"><span data-stu-id="173a9-157">Query returns and results are written to **Console**</span></span>

![Dane wyjściowe Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="173a9-159">Wnioskiem</span><span class="sxs-lookup"><span data-stu-id="173a9-159">The takeaway</span></span>

<span data-ttu-id="173a9-160">Teraz widzieliśmy, jak łatwo jest zapewnienie użycie metod asynchronicznych EF firmy.</span><span class="sxs-lookup"><span data-stu-id="173a9-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="173a9-161">Mimo że zalety asynchronicznych może nie być jasne, za pomocą aplikacji konsoli proste, strategie tego samego można stosować w sytuacjach, w której długotrwałych lub powiązane z siecią działania mogą w przeciwnym razie zablokują aplikacji lub spowodować dużej liczby wątków do Zwiększ ilość pamięci zajmowaną.</span><span class="sxs-lookup"><span data-stu-id="173a9-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>

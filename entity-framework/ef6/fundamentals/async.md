---
title: Zapytanie asynchroniczne i Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: ae578976ffc88b407ef0aaa0017935005bedd093
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921631"
---
# <a name="async-query-and-save"></a><span data-ttu-id="36a18-102">Zapytanie asynchroniczne i zapisywanie</span><span class="sxs-lookup"><span data-stu-id="36a18-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="36a18-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="36a18-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="36a18-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="36a18-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="36a18-105">EF6 wprowadza obsługę zapytań asynchronicznych i zapisuj przy użyciu [słów kluczowych async i await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , które zostały wprowadzone w programie .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="36a18-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="36a18-106">Chociaż nie wszystkie aplikacje mogą korzystać z usługi asynchroniczności, mogą one służyć do poprawienia czasu reakcji klienta i skalowalności serwera podczas obsługi zadań długotrwałych, sieciowych lub we/wy.</span><span class="sxs-lookup"><span data-stu-id="36a18-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="36a18-107">Kiedy naprawdę używać Async</span><span class="sxs-lookup"><span data-stu-id="36a18-107">When to really use async</span></span>

<span data-ttu-id="36a18-108">Celem tego instruktażu jest wprowadzenie koncepcji asynchronicznych w taki sposób, aby ułatwić przestrzeganie różnic między wykonywaniem asynchronicznych i synchronicznych programów.</span><span class="sxs-lookup"><span data-stu-id="36a18-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="36a18-109">Ten Instruktaż nie jest przeznaczony do zilustrowania żadnego z kluczowych scenariuszy, w których programowanie asynchroniczne zapewnia korzyści.</span><span class="sxs-lookup"><span data-stu-id="36a18-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="36a18-110">Programowanie asynchroniczne polega głównie na zwolnieniu bieżącego wątku zarządzanego (wątek, w którym działa kod .NET), aby wykonać inne czynności podczas oczekiwania na operację, która nie wymaga czasu obliczeń z zarządzanego wątku.</span><span class="sxs-lookup"><span data-stu-id="36a18-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="36a18-111">Na przykład podczas przetwarzania zapytania przez aparat bazy danych nie ma nic do wykonania przez kod platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="36a18-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="36a18-112">W aplikacjach klienckich (WinForms, WPF itp.) bieżący wątek można użyć, aby zapewnić czas odpowiedzi interfejsu użytkownika podczas wykonywania operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="36a18-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="36a18-113">W aplikacjach serwerowych (ASP.NET itp.) wątek może służyć do przetwarzania innych żądań przychodzących — może to zmniejszyć użycie pamięci i/lub zwiększyć przepływność serwera.</span><span class="sxs-lookup"><span data-stu-id="36a18-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="36a18-114">W przypadku większości aplikacji korzystających z funkcji Async nie będą mieć zauważalnych korzyści, a nawet mogą być szkodliwe.</span><span class="sxs-lookup"><span data-stu-id="36a18-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="36a18-115">Korzystaj z testów, profilowania i typowych sensie, aby mierzyć wpływ Async w konkretnym scenariuszu przed jego zatwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="36a18-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="36a18-116">Poniżej przedstawiono kilka zasobów, aby poznać informacje o komunikacji asynchronicznej:</span><span class="sxs-lookup"><span data-stu-id="36a18-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="36a18-117">Brandon Bray — Omówienie Async/await w programie .NET 4,5</span><span class="sxs-lookup"><span data-stu-id="36a18-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="36a18-118">[Asynchroniczne programowanie](https://msdn.microsoft.com/library/hh191443.aspx) stron w bibliotece MSDN</span><span class="sxs-lookup"><span data-stu-id="36a18-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="36a18-119">[Jak kompilować ASP.NET aplikacje sieci Web przy użyciu](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) usługi asynchronicznej (obejmuje demonstrację zwiększonej przepływności serwera)</span><span class="sxs-lookup"><span data-stu-id="36a18-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="36a18-120">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="36a18-120">Create the model</span></span>

<span data-ttu-id="36a18-121">W celu utworzenia naszego modelu i wygenerowania bazy danych będziemy używać [przepływu pracy Code First](~/ef6/modeling/code-first/workflows/new-database.md) , jednak funkcja asynchroniczna będzie współdziałać z wszystkimi modelami EF, w tym tymi utworzonymi przy użyciu programu EF Designer.</span><span class="sxs-lookup"><span data-stu-id="36a18-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="36a18-122">Tworzenie aplikacji konsolowej i wywoływanie jej **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="36a18-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="36a18-123">Dodawanie pakietu NuGet EntityFramework</span><span class="sxs-lookup"><span data-stu-id="36a18-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="36a18-124">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="36a18-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="36a18-125">Wybierz pozycję **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="36a18-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="36a18-126">W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="36a18-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="36a18-127">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="36a18-127">Click **Install**</span></span>
-   <span data-ttu-id="36a18-128">Dodaj klasę **model.cs** z następującą implementacją</span><span class="sxs-lookup"><span data-stu-id="36a18-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="36a18-129">Tworzenie programu synchronicznego</span><span class="sxs-lookup"><span data-stu-id="36a18-129">Create a synchronous program</span></span>

<span data-ttu-id="36a18-130">Teraz, gdy mamy już model EF, Napiszmy kod, który używa go do wykonania pewnego dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="36a18-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="36a18-131">Zastąp zawartość **program.cs** następującym kodem</span><span class="sxs-lookup"><span data-stu-id="36a18-131">Replace the contents of **Program.cs** with the following code</span></span>

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

<span data-ttu-id="36a18-132">Ten kod wywołuje metodę **PerformDatabaseOperations** , która zapisuje nowy **blog** w bazie danych, a następnie pobiera wszystkie **Blogi** z bazy danych i drukuje je do **konsoli**programu.</span><span class="sxs-lookup"><span data-stu-id="36a18-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="36a18-133">Po wykonaniu tej operacji program zapisuje ofertę dnia do **konsoli**programu.</span><span class="sxs-lookup"><span data-stu-id="36a18-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="36a18-134">Ponieważ kod jest synchroniczny, podczas uruchamiania programu można obserwować następujący przepływ wykonywania:</span><span class="sxs-lookup"><span data-stu-id="36a18-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="36a18-135">**Metody SaveChanges** rozpoczyna wypychanie nowego **bloga** do bazy danych</span><span class="sxs-lookup"><span data-stu-id="36a18-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="36a18-136">**Metody SaveChanges**</span><span class="sxs-lookup"><span data-stu-id="36a18-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="36a18-137">Zapytanie dotyczące wszystkich **blogów** jest wysyłane do bazy danych</span><span class="sxs-lookup"><span data-stu-id="36a18-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="36a18-138">Zapytania zwracane i wyniki są zapisywane w **konsoli**</span><span class="sxs-lookup"><span data-stu-id="36a18-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="36a18-139">Cytat dnia jest zapisywana w **konsoli**</span><span class="sxs-lookup"><span data-stu-id="36a18-139">Quote of the day is written to **Console**</span></span>

![Synchronizuj dane wyjściowe](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="36a18-141">Wykonywanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="36a18-141">Making it asynchronous</span></span>

<span data-ttu-id="36a18-142">Teraz, gdy nasz program został uruchomiony, możemy zacząć korzystać z nowych słów kluczowych async i await.</span><span class="sxs-lookup"><span data-stu-id="36a18-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="36a18-143">Wprowadziliśmy następujące zmiany w programie Program.cs</span><span class="sxs-lookup"><span data-stu-id="36a18-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="36a18-144">Wiersz 2: Instrukcja using dla przestrzeni nazw **System. Data. Entity** daje nam dostęp do metod rozszerzenia asynchronicznego EF.</span><span class="sxs-lookup"><span data-stu-id="36a18-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="36a18-145">Wiersz 4: Instrukcja using dla przestrzeni nazw **System. Threading. Tasks** umożliwia nam korzystanie z typu **zadania** .</span><span class="sxs-lookup"><span data-stu-id="36a18-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="36a18-146">Wiersz 12 & 18: Przechwytuje jako zadanie monitorujące postęp **PerformSomeDatabaseOperations** (wiersz 12), a następnie blokując wykonywanie programu w celu wykonania tego zadania po zakończeniu całej pracy dla programu (wiersz 18).</span><span class="sxs-lookup"><span data-stu-id="36a18-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="36a18-147">Wiersz 25: Zaktualizowaliśmy **PerformSomeDatabaseOperations** , aby można było je oznaczyć jako **Async** i zwrócić **zadanie**.</span><span class="sxs-lookup"><span data-stu-id="36a18-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="36a18-148">Wiersz 35: Teraz wywołujemy asynchroniczną wersję metody SaveChanges i oczekuje na jej ukończenie.</span><span class="sxs-lookup"><span data-stu-id="36a18-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="36a18-149">Wiersz 42: Teraz wywołujemy asynchroniczną wersję ToList — i czeka na wynik.</span><span class="sxs-lookup"><span data-stu-id="36a18-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="36a18-150">Aby uzyskać pełną listę dostępnych metod rozszerzenia w przestrzeni nazw System. Data. Entity, zapoznaj się z klasą QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="36a18-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="36a18-151">*Należy również dodać "using System. Data. Entity" do instrukcji using.*</span><span class="sxs-lookup"><span data-stu-id="36a18-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="36a18-152">Teraz, gdy kod jest asynchroniczny, można obserwować inny przepływ wykonywania podczas uruchamiania programu:</span><span class="sxs-lookup"><span data-stu-id="36a18-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1. <span data-ttu-id="36a18-153">**Metody SaveChanges** rozpoczyna wypychanie nowego **bloga** do bazy danych</span><span class="sxs-lookup"><span data-stu-id="36a18-153">**SaveChanges** begins to push the new **Blog** to the database</span></span>  
    <span data-ttu-id="36a18-154">*Po wysłaniu polecenia do bazy danych nie jest wymagany czas obliczeniowy w bieżącym wątku zarządzanym. Metoda **PerformDatabaseOperations** zwraca (nawet jeśli nie została ukończona), a przepływ programu w metodzie Main jest kontynuowany.*</span><span class="sxs-lookup"><span data-stu-id="36a18-154">*Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2. <span data-ttu-id="36a18-155">**Cytat dnia jest zapisywana w konsoli**</span><span class="sxs-lookup"><span data-stu-id="36a18-155">**Quote of the day is written to Console**</span></span>  
    <span data-ttu-id="36a18-156">*Ponieważ nie ma więcej pracy do wykonania w metodzie Main, zarządzany wątek jest blokowany na wywołanie oczekiwania do momentu zakończenia operacji bazy danych. Po zakończeniu zostanie wykonane pozostała część naszych **PerformDatabaseOperations** .*</span><span class="sxs-lookup"><span data-stu-id="36a18-156">*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="36a18-157">**Metody SaveChanges**</span><span class="sxs-lookup"><span data-stu-id="36a18-157">**SaveChanges** completes</span></span>  
4.  <span data-ttu-id="36a18-158">Zapytanie dotyczące wszystkich **blogów** jest wysyłane do bazy danych</span><span class="sxs-lookup"><span data-stu-id="36a18-158">Query for all **Blogs** is sent to the database</span></span>  
    <span data-ttu-id="36a18-159">*Ponownie zarządzany wątek jest bezpłatny do wykonywania innych czynności w czasie przetwarzania zapytania w bazie danych. Ponieważ wszystkie inne wykonania zostały ukończone, wątek zostanie po prostu zatrzymany na wywołaniu oczekiwania.*</span><span class="sxs-lookup"><span data-stu-id="36a18-159">*Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="36a18-160">Zapytania zwracane i wyniki są zapisywane w **konsoli**</span><span class="sxs-lookup"><span data-stu-id="36a18-160">Query returns and results are written to **Console**</span></span>  

![Asynchroniczne dane wyjściowe](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="36a18-162">Wnioskiem</span><span class="sxs-lookup"><span data-stu-id="36a18-162">The takeaway</span></span>

<span data-ttu-id="36a18-163">Teraz wiemy, jak łatwo jest korzystać z metod asynchronicznych EF.</span><span class="sxs-lookup"><span data-stu-id="36a18-163">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="36a18-164">Mimo że zalety Async mogą nie być bardzo widoczne w przypadku prostej aplikacji konsolowej, te same strategie mogą być stosowane w sytuacjach, w których długotrwałe lub powiązane z siecią działania mogą blokować aplikację lub spowodować, że wiele wątków Zwiększ rozmiary pamięci.</span><span class="sxs-lookup"><span data-stu-id="36a18-164">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>

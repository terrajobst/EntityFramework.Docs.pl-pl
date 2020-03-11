---
title: Zapytanie asynchroniczne i Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417981"
---
# <a name="async-query-and-save"></a>Zapytanie asynchroniczne i zapisywanie
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

EF6 wprowadza obsługę zapytań asynchronicznych i zapisuj przy użyciu [słów kluczowych async i await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , które zostały wprowadzone w programie .NET 4,5. Chociaż nie wszystkie aplikacje mogą korzystać z usługi asynchroniczności, mogą one służyć do poprawienia czasu reakcji klienta i skalowalności serwera podczas obsługi zadań długotrwałych, sieciowych lub we/wy.

## <a name="when-to-really-use-async"></a>Kiedy naprawdę używać Async

Celem tego instruktażu jest wprowadzenie koncepcji asynchronicznych w taki sposób, aby ułatwić przestrzeganie różnic między wykonywaniem asynchronicznych i synchronicznych programów. Ten Instruktaż nie jest przeznaczony do zilustrowania żadnego z kluczowych scenariuszy, w których programowanie asynchroniczne zapewnia korzyści.

Programowanie asynchroniczne polega głównie na zwolnieniu bieżącego wątku zarządzanego (wątek, w którym działa kod .NET), aby wykonać inne czynności podczas oczekiwania na operację, która nie wymaga czasu obliczeń z zarządzanego wątku. Na przykład podczas przetwarzania zapytania przez aparat bazy danych nie ma nic do wykonania przez kod platformy .NET.

W aplikacjach klienckich (WinForms, WPF itp.) bieżący wątek można użyć, aby zapewnić czas odpowiedzi interfejsu użytkownika podczas wykonywania operacji asynchronicznej. W aplikacjach serwerowych (ASP.NET itp.) wątek może służyć do przetwarzania innych żądań przychodzących — może to zmniejszyć użycie pamięci i/lub zwiększyć przepływność serwera.

W przypadku większości aplikacji korzystających z funkcji Async nie będą mieć zauważalnych korzyści, a nawet mogą być szkodliwe. Korzystaj z testów, profilowania i typowych sensie, aby mierzyć wpływ Async w konkretnym scenariuszu przed jego zatwierdzeniem.

Poniżej przedstawiono kilka zasobów, aby poznać informacje o komunikacji asynchronicznej:

-   [Brandon Bray — Omówienie Async/await w programie .NET 4,5](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Asynchroniczne programowanie](https://msdn.microsoft.com/library/hh191443.aspx) stron w bibliotece MSDN
-   [Jak kompilować ASP.NET aplikacje sieci Web przy użyciu usługi Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (obejmuje demonstrację zwiększonej przepływności serwera)

## <a name="create-the-model"></a>Tworzenie modelu

W celu utworzenia naszego modelu i wygenerowania bazy danych będziemy używać [przepływu pracy Code First](~/ef6/modeling/code-first/workflows/new-database.md) , jednak funkcja asynchroniczna będzie współdziałać z wszystkimi modelami EF, w tym tymi utworzonymi przy użyciu programu EF Designer.

-   Tworzenie aplikacji konsolowej i wywoływanie jej **AsyncDemo**
-   Dodawanie pakietu NuGet EntityFramework
    -   W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **AsyncDemo**
    -   Wybierz pozycję **Zarządzaj pakietami NuGet...**
    -   W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework**
    -   Kliknij przycisk **Instaluj**
-   Dodaj klasę **model.cs** z następującą implementacją

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

 

## <a name="create-a-synchronous-program"></a>Tworzenie programu synchronicznego

Teraz, gdy mamy już model EF, Napiszmy kod, który używa go do wykonania pewnego dostępu do danych.

-   Zastąp zawartość **program.cs** następującym kodem

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

Ten kod wywołuje metodę **PerformDatabaseOperations** , która zapisuje nowy **blog** w bazie danych, a następnie pobiera wszystkie **Blogi** z bazy danych i drukuje je do **konsoli**programu. Po wykonaniu tej operacji program zapisuje ofertę dnia do **konsoli**programu.

Ponieważ kod jest synchroniczny, podczas uruchamiania programu można obserwować następujący przepływ wykonywania:

1.  **Metody SaveChanges** rozpoczyna wypychanie nowego **bloga** do bazy danych
2.  **Metody SaveChanges**
3.  Zapytanie dotyczące wszystkich **blogów** jest wysyłane do bazy danych
4.  Zapytania zwracane i wyniki są zapisywane w **konsoli**
5.  Cytat dnia jest zapisywana w **konsoli**

![Synchronizuj dane wyjściowe](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Wykonywanie asynchroniczne

Teraz, gdy nasz program został uruchomiony, możemy zacząć korzystać z nowych słów kluczowych async i await. Wprowadziliśmy następujące zmiany w programie Program.cs

1.  Wiersz 2: Instrukcja using dla przestrzeni nazw **System. Data. Entity** daje nam dostęp do metod rozszerzeń asynchronicznych EF.
2.  Wiersz 4: Instrukcja using dla przestrzeni nazw **System. Threading. Tasks** umożliwia nam korzystanie z tego typu **zadania** .
3.  Wiersz 12 & 18: Przechwytywanie jako zadanie monitorujące postęp **PerformSomeDatabaseOperations** (wiersz 12), a następnie blokowanie wykonywania programu dla tego zadania, gdy wszystkie prace dla programu zostaną wykonane (wiersz 18).
4.  Wiersz 25: Zaktualizowaliśmy **PerformSomeDatabaseOperations** , aby można było oznaczyć ją jako **Async** i zwrócić **zadanie**.
5.  Wiersz 35: obecnie wywołujemy asynchroniczną wersję metody SaveChanges i oczekuje na jej ukończenie.
6.  Wiersz 42: obecnie wywołujemy asynchroniczną wersję ToList — i oczekuje na wynik.

Aby uzyskać pełną listę dostępnych metod rozszerzenia w przestrzeni nazw System. Data. Entity, zapoznaj się z klasą QueryableExtensions. *Należy również dodać "using System. Data. Entity" do instrukcji using.*

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

Teraz, gdy kod jest asynchroniczny, można obserwować inny przepływ wykonywania podczas uruchamiania programu:

1. **Metody SaveChanges** rozpoczyna wypychanie nowego **bloga** do bazy danych  
    *Po wysłaniu polecenia do bazy danych nie jest wymagany czas obliczeniowy w bieżącym wątku zarządzanym. Metoda **PerformDatabaseOperations** zwraca (nawet jeśli nie została ukończona), a przepływ programu w metodzie Main jest kontynuowany.*
2. **Cytat dnia jest zapisywana w konsoli**  
    *Ponieważ nie ma więcej pracy do wykonania w metodzie Main, zarządzany wątek jest blokowany na wywołanie oczekiwania do momentu zakończenia operacji bazy danych. Po zakończeniu zostanie wykonane pozostała część naszych **PerformDatabaseOperations** .*
3.  **Metody SaveChanges**  
4.  Zapytanie dotyczące wszystkich **blogów** jest wysyłane do bazy danych  
    *Ponownie zarządzany wątek jest bezpłatny do wykonywania innych czynności w czasie przetwarzania zapytania w bazie danych. Ponieważ wszystkie inne wykonania zostały ukończone, wątek zostanie po prostu zatrzymany na wywołaniu oczekiwania.*
5.  Zapytania zwracane i wyniki są zapisywane w **konsoli**  

![Asynchroniczne dane wyjściowe](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Wnioskiem

Teraz wiemy, jak łatwo jest korzystać z metod asynchronicznych EF. Mimo że zalety Async mogą nie być bardzo widoczne w przypadku prostej aplikacji konsolowej, te same strategie mogą być stosowane w sytuacjach, w których długotrwałe lub powiązane z siecią działania mogą blokować aplikację lub spowodować, że wiele wątków Zwiększ rozmiary pamięci.

---
title: Asynchroniczne zapytania i Zapisz - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 758f8bc3d14fc1f60f14ff14f4251aeed057c518
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994466"
---
# <a name="async-query-and-save"></a>Asynchroniczne zapytania i Zapisz
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

EF6 wprowadzono obsługę dla zapytania asynchroniczne, a następnie Zapisz przy użyciu [async i await słowa kluczowe](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) wprowadzone w .NET 4.5. Gdy nie wszystkie aplikacje mogą korzystać z asynchroniczności, może służyć do poprawy skalowalności krótki czas reakcji i serwera klienta podczas obsługi długotrwałych, sieci lub zadania I/O-powiązane z.

## <a name="when-to-really-use-async"></a>Kiedy należy używać naprawdę async

Ten przewodnik ma na celu wprowadzenie pojęcia asynchronicznej w sposób, który ułatwia obserwować różnica między wykonywania programu synchronicznego i asynchronicznego. W tym przewodniku nie jest przeznaczona do zilustrowanie jednego z kluczowych scenariuszy, których programowanie async oferuje korzyści.

Programowanie asynchroniczne koncentruje się głównie na zwalnianie bieżącego wątku zarządzanych (wątek uruchamianie kodu platformy .NET), wykonywanie innych zadań, aż do operacji, która nie wymaga żadnych czas obliczeń w z wątków zarządzanych. Na przykład podczas gdy aparat bazy danych jest przetwarzanie kwerendy nie ma nic do można przeprowadzić za pomocą kodu platformy .NET.

W aplikacjach klienckich (WinForms, WPF, itd.) bieżący wątek może służyć do zachowania dynamicznego interfejsu użytkownika podczas operacji asynchronicznej. W aplikacji serwera (ASP.NET itp.), wątek może służyć do przetwarzania innych żądań przychodzących — to może zmniejszyć użycie pamięci i/lub zwiększyć przepływność serwera.

W większości aplikacji przy użyciu async będzie zawierać żadnych zauważalnego korzyści, a nawet może być szkodliwe. Umożliwia testy, profilowanie i zdroworozsądkowe mierzenie wpływu asynchronicznych w konkretnym scenariuszu przed zatwierdzeniem do niego.

Poniżej przedstawiono niektóre inne zasoby, aby dowiedzieć się więcej na temat async:

-   [Omówienie Brandon Bray async/await w .NET 4.5](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Programowanie asynchroniczne](https://msdn.microsoft.com/library/hh191443.aspx) stron w bibliotece MSDN
-   [Jak tworzyć ASP.NET sieci Web aplikacji przy użyciu Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (w tym pokaz serwera większą przepływność)

## <a name="create-the-model"></a>Tworzenie modelu

Będziemy używać [Code First przepływu pracy](~/ef6/modeling/code-first/workflows/new-database.md) utworzyć nasz model i wygenerować bazę danych, jednak w asynchronicznej funkcji będzie działać z wszystkich modeli EF, w tym te, utworzone za pomocą projektanta EF.

-   Utwórz aplikację Konsolową i wywołać go **AsyncDemo**
-   Dodaj pakiet NuGet platformy EntityFramework
    -   W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **AsyncDemo** projektu
    -   Wybierz **Zarządzaj pakietami NuGet...**
    -   W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu
    -   Kliknij przycisk **instalacji**
-   Dodaj **Model.cs** klasy następującą implementacją

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

 

## <a name="create-a-synchronous-program"></a>Utwórz program synchroniczne

Teraz, gdy mamy już modelu platformy EF, umożliwia pisanie kodu w celu zastosowania do wykonania niektórych dostęp do danych.

-   Zastąp zawartość **Program.cs** następującym kodem

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

                Console.WriteLine();
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
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

Ten kod wywołuje **PerformDatabaseOperations** metodę, która zapisuje nową **Blog** do bazy danych, a następnie pobiera wszystkie **blogi** z bazy danych i wysłania ich do **Konsoli**. Dzięki temu program zapisuje oferty dnia, aby **konsoli**.

Ponieważ kod jest synchronicznego, możemy zaobserwować następujący przepływ wykonania, gdy Uruchamiamy program:

1.  **SaveChanges** rozpoczyna się wypychania nowego **blogu** do bazy danych
2.  **SaveChanges** kończy
3.  Zapytanie o wszystkie **blogi** są wysyłane do bazy danych
4.  Zapytanie zwraca, a wyniki są zapisywane do **konsoli**
5.  Cytat dnia są zapisywane do **konsoli**

![SyncOutput](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Dzięki czemu asynchroniczne

Teraz, gdy nasz program działanie, możemy rozpocząć co użycie nowego async i await słów kluczowych. Wprowadziliśmy następujące zmiany do pliku Program.cs

1.  Wiersz 2: Za pomocą instrukcji dla **System.Data.Entity** przestrzeni nazw daje nam dostęp do metod rozszerzenia asynchronicznych EF.
2.  Wiersz 4: Za pomocą instrukcji dla **System.Threading.Tasks** przestrzeni nazw pozwala nam korzystać **zadań** typu.
3.  Wiersz 12 i 18: Firma Microsoft jest przechwytywany jako zadanie, które monitoruje postęp **PerformSomeDatabaseOperations** (wierszem 12) i następnie blokuje wykonywanie programu w tym zadań raz ukończone wszystkie prace dla program odbywa się (wiersz 18).
4.  Wiersz 25: Udostępniliśmy aktualizacji **PerformSomeDatabaseOperations** być oznaczony jako **async** i zwracają **zadań**.
5.  Wiersz 35: Firma Microsoft teraz wywołanie asynchroniczne wersję SaveChanges i oczekiwaniem na jej ukończenie.
6.  Wiersz 42: Firma Microsoft jest teraz wywołanie asynchroniczne wersję tolist — i oczekiwaniem na wynik.

Pełną listę metod rozszerzenia dostępne w przestrzeni nazw System.Data.Entity można znaleźć klasy QueryableExtensions. *Należy także dodać "za pomocą System.Data.Entity" do sieci za pomocą instrukcji.*

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

Teraz, gdy kod jest asynchroniczna, można zaobserwować przepływu wykonywania różnych gdy Uruchamiamy program:

1.  **SaveChanges** rozpoczyna się wypychania nowego **Blog** w bazie danych *po wysłaniu polecenia do bazy danych co obliczenia, konieczna jest w bieżącym wątku zarządzanych. **PerformDatabaseOperations** metoda zwróci wartość (nawet jeśli nie zostało zakończone, wykonując) i kontynuuje przepływu programu dla metody Main.*
2.  **Cytat dnia są zapisywane do konsoli**
    *ponieważ nie ma więcej pracy w metody Main, wątków zarządzanych jest zablokowany na czas oczekiwania wywołania do momentu ukończenia operacji bazy danych. Po zakończeniu pozostałą część naszego **PerformDatabaseOperations** * zostaną wykonane.
3.  **SaveChanges** kończy
4.  Zapytanie o wszystkie **blogi** są wysyłane do bazy danych *ponownie wątków zarządzanych jest bezpłatna wykonywanie innych zadań, podczas gdy zapytania są przetwarzane w bazie danych. Od wszystkich innych wykonanie zostało ukończone, wątek będzie po prostu zatrzymanie przy wywołaniu oczekiwania jednak.*
5.  Zapytanie zwraca, a wyniki są zapisywane do **konsoli**

![AsyncOutput](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Wnioskiem

Teraz widzieliśmy, jak łatwo jest zapewnienie użycie metod asynchronicznych EF firmy. Mimo że zalety asynchronicznych może nie być jasne, za pomocą aplikacji konsoli proste, strategie tego samego można stosować w sytuacjach, w której długotrwałych lub powiązane z siecią działania mogą w przeciwnym razie zablokują aplikacji lub spowodować dużej liczby wątków do Zwiększ ilość pamięci zajmowaną.

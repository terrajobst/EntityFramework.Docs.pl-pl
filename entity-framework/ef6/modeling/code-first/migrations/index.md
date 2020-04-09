---
title: Migracje code first - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78418963"
---
# <a name="code-first-migrations"></a>Migracje kodu
Migracje code first to zalecany sposób rozwijania schematu bazy danych aplikacji, jeśli używasz przepływu pracy Code First. Migracje zapewniają zestaw narzędzi, które umożliwiają:

1. Tworzenie początkowej bazy danych, która współpracuje z modelem EF
2. Generowanie migracji w celu śledzenia zmian wprowadzać w modelu EF
2. Dbaj o aktualną bazę danych dzięki tym zmianom

Poniższy instruktaż zapewni omówienie migracje code first w ramach encji. Możesz ukończyć cały instruktaż lub przejść do interesującego Cię tematu. Poruszane są następujące tematy:

## <a name="building-an-initial-model--database"></a>Tworzenie początkowej bazy danych & modelu

Zanim zaczniemy używać migracji, potrzebujemy projektu i modelu Code First do pracy. W tym instruktażu będziemy używać kanonicznego modelu **bloga** i **postu.**

-   Tworzenie nowej aplikacji **MigrationsDemo** Console
-   Dodawanie najnowszej wersji pakietu **NuGet programu EntityFramework** do projektu
    -   **Narzędzia&gt; – Menedżer&gt; pakietów biblioteki – Konsola Menedżera pakietów**
    -   Uruchamianie polecenia **Install-Package EntityFramework**
-   Dodaj plik **Model.cs** z kodem pokazanym poniżej. Ten kod definiuje jedną klasę **bloga,** która składa się z naszego modelu domeny i klasę **BlogContext,** która jest naszym kontekstem EF Code First

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Teraz, gdy mamy model nadszedł czas, aby użyć go do wykonywania dostępu do danych. Zaktualizuj plik **Program.cs** kodem pokazanym poniżej.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Uruchom aplikację, a zobaczysz, że **migrationsCodeDemo.BlogContext** bazy danych jest tworzony dla Ciebie.

    ![Baza danych LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Włączanie migracji

Nadszedł czas, aby wprowadzić kilka zmian w naszym modelu.

-   Wprowadźmy url właściwości do bloga klasy.

``` csharp
    public string Url { get; set; }
```

Jeśli aplikacja miałaby zostać ponownie uruchomiony, otrzymasz InvalidOperationException stwierdzając, że *model kopii kontekstu "BlogContext" uległ zmianie od czasu utworzenia bazy danych. Rozważ użycie migracji code first do zaktualizowania bazy danych (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Jak sugeruje wyjątek, nadszedł czas, aby rozpocząć korzystanie z migracji code first. Pierwszym krokiem jest włączenie migracji dla naszego kontekstu.

-   Uruchamianie polecenia **Włącz migracje** w konsoli Menedżera pakietów

    To polecenie dodało folder **Migracje** do naszego projektu. Ten nowy folder zawiera dwa pliki:

-   **Klasa konfiguracji.** Ta klasa umożliwia skonfigurowanie zachowania migracji dla kontekstu. W tym instruktażu po prostu użyjemy domyślnej konfiguracji.
    *Ponieważ istnieje tylko jeden kontekst Code First w projekcie, Enable-Migrations automatycznie wypełnił typ kontekstu, do który dotyczy ta konfiguracja.*
-   **Początkowa migracjatworzenia**. Ta migracja została wygenerowana, ponieważ mieliśmy już Code First utworzyć bazę danych dla nas, zanim włączono migracje. Kod w tej migracji szkieletu reprezentuje obiekty, które zostały już utworzone w bazie danych. W naszym przypadku jest to tabela **Blog** z **blogid** i **nazwa** kolumny. Nazwa pliku zawiera sygnaturę czasową ułatwiającą zamawianie.
    *Jeśli baza danych nie została jeszcze utworzona, migracja InitialCreate nie zostałaby dodana do projektu. Zamiast tego przy pierwszym wywołaniu Add-Migration kod do tworzenia tych tabel będzie szkieletu do nowej migracji.*

### <a name="multiple-models-targeting-the-same-database"></a>Wiele modeli kierowanych na tę samą bazę danych

Podczas korzystania z wersji wcześniejszych ef6, tylko jeden model Code First może służyć do generowania/zarządzania schematem bazy danych. Jest to wynik pojedynczej ** \_ \_MigrationsHistory** tabeli na bazę danych bez możliwości określenia, które wpisy należą do którego modelu.

Począwszy od **EF6, Configuration** klasa zawiera **ContextKey** właściwości. Działa to jako unikatowy identyfikator dla każdego modelu Code First. Odpowiednia kolumna ** \_ \_w MigrationsHistory** tabeli umożliwia wpisy z wielu modeli do udziału w tabeli. Domyślnie ta właściwość jest ustawiona na w pełni kwalifikowaną nazwę kontekstu.

## <a name="generating--running-migrations"></a>Generowanie & uruchomionych migracji

Migracje code first ma dwa polecenia podstawowe, które zostaną zaznajomieni z.

-   **Add-Migration** spowoduje utworzenie następnej migracji na podstawie zmian wprowadzonych w modelu od czasu utworzenia ostatniej migracji
-   **Update-Database** zastosuje wszelkie oczekujące migracje do bazy danych

Musimy uszłać migrację, aby zadbać o nową właściwość Url, którą dodaliśmy. Polecenie **Add-Migration** pozwala nam nadać tym migracji nazwę, po prostu zadzwońmy do naszego **AddBlogUrl**.

-   Uruchamianie polecenia **Add-Migration AddBlogUrl** w konsoli Menedżera pakietów
-   W folderze **Migracje** mamy teraz nową migrację **AddBlogUrl.** Nazwa pliku migracji jest wstępnie ustalona za pomocą sygnatury czasowej, aby ułatwić zamawianie

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Blogs", "Url", c => c.String());
            }

            public override void Down()
            {
                DropColumn("dbo.Blogs", "Url");
            }
        }
    }
```

Możemy teraz edytować lub dodać do tej migracji, ale wszystko wygląda całkiem nieźle. Użyjmy **update-database,** aby zastosować tę migrację do bazy danych.

-   Uruchamianie polecenia **Update-Database** w konsoli Menedżera pakietów
-   Migracje code first będą porównywać migracje w naszym folderze Migracje z **migracjami,** które zostały zastosowane do bazy danych. Zobaczy, że należy zastosować migrację **AddBlogUrl** i uruchomić ją.

Baza danych **MigrationsDemo.BlogContext** jest teraz aktualizowana w celu uwzględnienia kolumny **Url** w tabeli **Blogi.**

## <a name="customizing-migrations"></a>Dostosowywanie migracji

Do tej pory wygenerowaliśmy i uruchomiliśmy migrację bez wprowadzania żadnych zmian. Teraz przyjrzyjmy się edycji kodu, który jest generowany domyślnie.

-   Nadszedł czas, aby wprowadzić kilka zmian w naszym modelu, dodajmy nową właściwość **Ocena** do klasy **Blog**

``` csharp
    public int Rating { get; set; }
```

-   Dodajmy również nową klasę **Post**

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Dodamy również kolekcję **postów** do klasy **Blog,** aby utworzyć drugi koniec relacji między **blogiem** a **postem**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Użyjemy polecenia **Add-Migration,** aby umożliwić kod pierwsze migracje szkieletu jego najlepsze przypuszczenie na migracji dla nas. Będziemy nazywać tę migrację **AddPostClass**.

-   Uruchom polecenie **Add-Migration AddPostClass** w konsoli Menedżera pakietów.

Code First Migrations zrobił całkiem dobrą robotę rusztowania tych zmian, ale są pewne rzeczy, które możemy chcieć zmienić:

1.  Najpierw dodajmy unikatowy indeks do kolumny **Posts.Title** (Dodawanie w wierszu 22 & 29 w poniższym kodzie).
2.  Dodajemy również kolumnę **Blogs.Rating,** która nie może być nullowa. Jeśli w tabeli są jakieś istniejące dane, zostanie przypisana domyślna wartość CLR typu danych dla nowej kolumny (Ocena jest liczą całkowitą, więc będzie to **0**). Ale chcemy określić domyślną wartość **3,** tak aby istniejące wiersze w tabeli **Blogi** zaczęły się od przyzwoitej oceny.
    (Możesz zobaczyć domyślną wartość określoną w wierszu 24 poniższego kodu)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

Nasza edytowana migracja jest gotowa do użycia, więc użyjmy **update-database,** aby zaktualizować bazę danych. Tym razem określmy flagę **—Verbose,** aby można było zobaczyć sql, że migracje code first jest uruchomiony.

-   Uruchom polecenie **Update-Database —verbose** w konsoli Menedżera pakietów.

## <a name="data-motion--custom-sql"></a>Ruch danych / Niestandardowy SQL

Do tej pory przyjrzeliśmy się operacjom migracji, które nie zmieniają ani nie przenoszą żadnych danych, teraz przyjrzyjmy się czemuś, co musi przenieść niektóre dane. Nie ma jeszcze natywnej obsługi ruchu danych, ale możemy uruchomić niektóre dowolne polecenia SQL w dowolnym momencie naszego skryptu.

-   Dodajmy **Właściwość Post.Abstract** do naszego modelu. Później wstępnie wypełnimy **streszczenie** dla istniejących wpisów przy użyciu tekstu z początku kolumny **Zawartość.**

``` csharp
    public string Abstract { get; set; }
```

Użyjemy polecenia **Add-Migration,** aby umożliwić kod pierwsze migracje szkieletu jego najlepsze przypuszczenie na migracji dla nas.

-   Uruchom polecenie **Add-Migration AddPostAbstract** w konsoli Menedżera pakietów.
-   Wygenerowana migracja zajmuje się zmianami schematu, ale chcemy również wstępnie wypełnić **kolumnę Streszczenie** przy użyciu pierwszych 100 znaków zawartości dla każdego wpisu. Możemy to zrobić, upuszczając do SQL i uruchamiając **update** instrukcji po dodaniu kolumny.
    (Dodanie w wierszu 12 w kodzie poniżej)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

Nasza edytowana migracja wygląda dobrze, więc użyjmy **update-database,** aby zaktualizować bazę danych. Określimy **flagę —Verbose,** aby śmy mogli zobaczyć, że SQL jest uruchamiany w bazie danych.

-   Uruchom polecenie **Update-Database —verbose** w konsoli Menedżera pakietów.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrowanie do określonej wersji (w tym na starszą wersję)

Do tej pory zawsze uaktualniliśmy do najnowszej migracji, ale mogą być chwile, kiedy chcesz uaktualnić / obniżyć do określonej migracji.

Załóżmy, że chcemy przeprowadzić migrację naszej bazy danych do stanu, w jaki był po uruchomieniu naszej migracji **AddBlogUrl.** Możemy użyć **przełącznika —TargetMigration,** aby przejść na starszą wersję do tej migracji.

-   Uruchom polecenie **Update-Database –TargetMigration: AddBlogUrl** w konsoli Menedżera pakietów.

To polecenie uruchomi skrypt Down dla naszych migracji **AddBlogAbstract** i **AddPostClass.**

Jeśli chcesz toczyć całą drogę z powrotem do pustej bazy danych, a następnie można użyć **Update-Database –TargetMigration: $InitialDatabase** polecenia.

## <a name="getting-a-sql-script"></a>Uzyskiwanie skryptu SQL

Jeśli inny deweloper chce tych zmian na swoim komputerze, może po prostu zsynchronizować, gdy sprawdzimy nasze zmiany w kontroli źródła. Po uzyskaniu nowych migracji mogą po prostu uruchomić polecenie Update-Database, aby zmiany były stosowane lokalnie. Jeśli jednak chcemy wypchnąć te zmiany na serwer testowy, a ostatecznie do produkcji, prawdopodobnie chcemy skryptu SQL, który możemy przekazać naszemu DBA.

-   Uruchom polecenie **Aktualizuj bazę danych,** ale tym razem określ flagę **–Script,** aby zmiany były zapisywane w skrypcie, a nie stosowane. Określimy również źródło i migrację docelową, aby wygenerować skrypt. Chcemy, aby skrypt przejść z pustej bazy danych (**$InitialDatabase**) do najnowszej wersji (migracja **AddPostAbstract**).
    *Jeśli migracja docelowa nie zostanie określona, migracje użyją najnowszej migracji jako obiektu docelowego. Jeśli nie określisz migracji źródłowej, migracje użyją bieżącego stanu bazy danych.*
-   Uruchom polecenie **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** w konsoli Menedżera pakietów

Code First Migrations uruchomi potok migracji, ale zamiast faktycznie zastosować zmiany, zapisze je do pliku .sql. Po wygenerowaniu skryptu jest on otwierany w programie Visual Studio, gotowy do wyświetlenia lub zapisania.

### <a name="generating-idempotent-scripts"></a>Generowanie skryptów idempotentnych

Począwszy od EF6, jeśli określisz **—SourceMigration $InitialDatabase,** wygenerowany skrypt będzie "idempotent". Skrypty Idempotentne można uaktualnić bazę danych obecnie w dowolnej wersji do najnowszej wersji (lub określonej wersji, jeśli używasz **-TargetMigration**). Wygenerowany skrypt zawiera logikę, aby sprawdzić ** \_ \_MigrationsHistory** tabeli i tylko zastosować zmiany, które nie zostały wcześniej zastosowane.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Automatyczne uaktualnianie podczas uruchamiania aplikacji (migrateDatabaseToLatestVersion Initializer)

Jeśli wdrażasz aplikację, możesz chcieć, aby automatycznie uaktualniła bazę danych (stosując wszelkie oczekujące migracje) po uruchomieniu aplikacji. Można to zrobić, rejestrującicwizator bazy danych **MigrateDatabaseToLatestVersion.** Inicjator bazy danych po prostu zawiera pewną logikę, która jest używana, aby upewnić się, że baza danych jest poprawnie skonfigurowana. Ta logika jest uruchamiana po raz pierwszy kontekst jest używany w procesie aplikacji (**AppDomain**).

Możemy zaktualizować plik **Program.cs,** jak pokazano poniżej, aby ustawić **migrateDatabaseToLatestVersion inicjatora** blogcontext przed użyciem kontekstu (wiersz 14). Należy również zauważyć, że należy również dodać using instrukcji dla obszaru nazw **System.Data.Entity** (wiersz 5).

*Podczas tworzenia wystąpienia tego inicjatora musimy określić typ kontekstu **(BlogContext)** i konfigurację migracji **(Konfiguracja)**- konfiguracja migracji jest klasą, która została dodana do naszego **folderu Migracje,** gdy włączyliśmy migracje.*

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

                using (var db = new BlogContext())
                {
                    db.Blogs.Add(new Blog { Name = "Another Blog " });
                    db.SaveChanges();

                    foreach (var blog in db.Blogs)
                    {
                        Console.WriteLine(blog.Name);
                    }
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
        }
    }
```

Teraz, gdy nasza aplikacja działa, najpierw sprawdzi, czy baza danych, na które jest kierowana, jest aktualna, i zastosuje wszelkie oczekujące migracje, jeśli nie jest.

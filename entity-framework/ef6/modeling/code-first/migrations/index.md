---
title: Migracje Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: f408ef861a2992783142fa1483d1433ca710399a
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415799"
---
# <a name="code-first-migrations"></a>Migracje Code First
Migracje Code First jest zalecany sposób rozwijać schemat bazy danych aplikacji, jeśli używasz kodu pierwszego przepływu pracy. Migracje udostępniają zestaw narzędzi, które umożliwiają:

1. Tworzenie początkowej bazy danych, która współdziała z modelu platformy EF
2. Generowanie migracji, aby śledzić zmiany wprowadzone do modelu platformy EF
2. Bazy danych na bieżąco z tymi zmianami

Następujące Instruktaż zapewnia przegląd migracje Code First platformy Entity Framework. Możesz ukończenia całego instruktażu lub od razu przejść do tematu, który Cię interesuje. Omówiono następujące tematy:

## <a name="building-an-initial-model--database"></a>Tworzenie początkowej modelu i bazy danych

Zanim zaczniemy, za pomocą migracji potrzebujemy projektu i model Code First chcesz pracować. W tym przewodniku są użyjemy canonical **Blog** i **wpis** modelu.

-   Utwórz nową **MigrationsDemo** aplikacji konsoli
-   Dodaj najnowszą wersję **EntityFramework** pakiet NuGet do projektu
    -   **Narzędzia —&gt; Menedżer pakietów biblioteki —&gt; Konsola Menedżera pakietów**
    -   Uruchom **EntityFramework instalacji pakietu** polecenia
-   Dodaj **Model.cs** pliku z kodem, pokazano poniżej. Ten kod definiuje pojedynczy **Blog** klasę, która sprawia, że nasz model domeny i **BlogContext** klasę, która jest nasz kontekst EF Code First

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

-   Teraz, gdy model, nadszedł czas na jej używać do wykonywania dostęp do danych. Aktualizacja **Program.cs** pliku z kodem, pokazano poniżej.

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

-   Uruchom aplikację i zostanie wyświetlony **MigrationsCodeDemo.BlogContext** baza danych została utworzona dla Ciebie.

    ![Bazy danych LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Włączanie migracji

Nadszedł czas na pewnych zmian więcej w naszym modelu.

-   Umożliwia wprowadzenie właściwość adres Url do klasy blogu.

``` csharp
    public string Url { get; set; }
```

Jeśli zamierzasz uruchomić aplikację ponownie otrzymamy InvalidOperationException, podając *modelu kopii kontekstu "BlogContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First w aktualizacji bazy danych (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Jak sugeruje wyjątek, jest czas, aby rozpocząć korzystanie z migracje Code First. Pierwszym krokiem jest włączanie funkcji migracje dla nasz kontekst.

-   Uruchom **Enable-Migrations** polecenia w konsoli Menedżera pakietów

    To polecenie został dodany **migracje** folderu do naszego projektu. Ten nowy folder zawiera dwa pliki:

-   **Klasa konfiguracji.** Ta klasa pozwala skonfigurować sposób działania migracji w Twoim kontekście. W tym przewodniku po prostu używamy domyślnej konfiguracji.
    *Ponieważ istnieje tylko pojedynczy kontekst Code First w projekcie, Enable-Migrations jest wypełniane automatycznie typ kontekstu, którego dotyczy ta konfiguracja.*
-   **Migracja InitialCreate**. Ta migracja został wygenerowany, ponieważ już mieliśmy Code First utworzyć bazę danych, zanim umożliwiliśmy migracji. Kod w tej migracji szkieletu reprezentuje obiektów, które zostały już utworzone w bazie danych. W tym przypadku, który jest **Blog** tabeli za pomocą **BlogId** i **nazwa** kolumn. Nazwa pliku zawiera sygnaturę czasową, aby pomóc w kolejności.
    *Jeśli baza danych nie został już utworzony tej migracji InitialCreate czy nie zostały dodane do projektu. Zamiast tego po raz pierwszy nazywamy migracji Dodaj kod, aby utworzyć te tabele będą szkieletu do nowej migracji.*

### <a name="multiple-models-targeting-the-same-database"></a>Wiele modeli przeznaczonych dla tej samej bazy danych

W przypadku używania wersji przed EF6, tylko jeden model Code First może służyć do generowania/Zarządzanie schematami bazy danych. Jest to wynikiem pojedynczej  **\_ \_MigrationsHistory** tabeli na bazę danych w sposób identyfikowania zapisy, które należą do modelu.

Począwszy od platformy EF6, **konfiguracji** klasa zawiera **elementu ContextKey** właściwości. Jest to zabezpieczenie jako unikatowy identyfikator dla każdego modelu Code First. W kolumnie  **\_ \_MigrationsHistory** tabeli umożliwia wpisy z wielu modeli, aby udostępnić w tabeli. Ta właściwość jest domyślnie do w pełni kwalifikowanej nazwy kontekstu.

## <a name="generating--running-migrations"></a>Generowanie i uruchamianie migracji

Migracje Code First ma dwa podstawowe polecenia, które ma być zapoznanie się z.

-   **Dodaj migracji** będzie tworzenia szkieletu dalej migracji, w oparciu o zmiany wprowadzone do modelu od chwili utworzenia ostatniej migracji
-   **Update-Database** Zastosuj wszelkie oczekujące migracji do bazy danych

Potrzebujemy do tworzenia szkieletu migracji, aby zadbać o nową właściwość adres Url, które dodaliśmy. **Migracji Dodaj** polecenie umożliwia nam nazwij te migracji, po prostu nazwiemy naszych **AddBlogUrl**.

-   Uruchom **AddBlogUrl migracji Dodaj** polecenia w konsoli Menedżera pakietów
-   W **migracje** folderu w efekcie powstał nowy **AddBlogUrl** migracji. Filename migracji wstępnie ustalony z sygnaturą czasową pomagające w kolejności

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

Firma Microsoft, można teraz edytować lub dodać do tej migracji, ale wszystko wygląda dość dobrze. Użyjmy **Update-Database** dotyczą tej migracji bazy danych.

-   Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów
-   Migracje Code First zostanie porównany migracji w naszym **migracje** folder z tymi, które zostały zastosowane do bazy danych. Zostanie on wyświetlony **AddBlogUrl** migracji musi być zastosowane i uruchom go.

**MigrationsDemo.BlogContext** bazy danych został zaktualizowany do uwzględnienia **adresu Url** kolumny w **blogi** tabeli.

## <a name="customizing-migrations"></a>Dostosowywanie migracji

Do tej pory mamy już i uruchamianie migracji bez wprowadzania żadnych zmian. Teraz Przyjrzyjmy się edycją kodu, który pobiera wygenerowany domyślnie.

-   Nadszedł czas, aby wprowadzić pewne zmiany więcej w naszym modelu, możemy dodać nową **ocena** właściwości **blogu** klasy

``` csharp
    public int Rating { get; set; }
```

-   Możemy również dodać nowy **wpis** klasy

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

-   Dodamy również **wpisy** kolekcji **Blog** klasy w celu utworzenia drugiej stronie relacji między **Blog** i **wpis**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Użyjemy **migracji Dodaj** polecenie, aby umożliwić migracje Code First tworzenia szkieletu jego najbardziej prawdopodobny wybór na migrację dla nas. Zamierzamy wywołania tej migracji **AddPostClass**.

-   Uruchom **AddPostClass migracji Dodaj** polecenia w konsoli Menedżera pakietów.

Migracje Code First jak całkiem Niezłe rozwiązanie zadanie tworzenia szkieletów te zmiany, ale istnieje kilka kwestii, które firma Microsoft może wystąpić potrzeba zmiany:

1.  Najpierw w górę, możemy dodać unikatowego indeksu **Posts.Title** kolumny (Dodawanie w wierszu 22 & 29 w poniższym kodzie).
2.  Dodajemy również dopuszcza **Blogs.Rating** kolumny. W przypadku istniejących danych w tabeli zostaną zostaną przypisane domyślne CLR typu danych dla nowej kolumny (ocena jest liczba całkowita, tak byłoby **0**). Ale chcemy określić wartość domyślną **3** więc w istniejącym wiersze **blogi** tabeli rozpoczyna się od klasyfikacji znośnego.
    (Wartość domyślna określona w wierszu 24 kod poniżej można znaleźć)

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

Nasze edytowanych migracji jest gotowy do Przejdź, dlatego użyjemy **Update-Database** Aby przełączyć bazę danych aktualne. Teraz możemy określić **— pełne** Flaga, dzięki czemu możesz zobaczyć SQL Server, który działa migracje Code First.

-   Uruchom **Update-Database — pełne** polecenia w konsoli Menedżera pakietów.

## <a name="data-motion--custom-sql"></a>Ruchu danych niestandardowymi SQL

Do tej pory mają przyjrzeliśmy się migracji, które operacje, które nie zmieniać i przenieść wszystkie dane teraz możemy przyjrzeć się coś wymagających przenoszenia niektórych danych. Istnieje jeszcze nie natywną obsługę ruchu danych, ale niektóre dowolnych poleceń SQL w dowolnym momencie można uruchomić w naszego skryptu.

-   Dodajmy **Post.Abstract** właściwość nasz model. Później, będziemy do wstępnego wypełniania **abstrakcyjne** dla istniejących wpisów od początku przy użyciu jakiś tekst **zawartości** kolumny.

``` csharp
    public string Abstract { get; set; }
```

Użyjemy **migracji Dodaj** polecenie, aby umożliwić migracje Code First tworzenia szkieletu jego najbardziej prawdopodobny wybór na migrację dla nas.

-   Uruchom **AddPostAbstract migracji Dodaj** polecenia w konsoli Menedżera pakietów.
-   Wygenerowany migracji zajmie się zmiany schematu, ale chcemy również do wstępnego wypełniania **abstrakcyjne** kolumny przy użyciu 100 pierwszych znaków zawartości dla każdego wpisu. Możemy to zrobić przez rozwijanej opuszcza SQL i uruchamianie **aktualizacji** instrukcji po dodaniu kolumny.
    (Dodanie w wierszu 12 w poniższym kodzie)

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

Nasze edytowanych migracji jest dobra, dlatego użyjemy **Update-Database** Aby przełączyć bazę danych aktualne. Firma Microsoft będzie określić **— pełne** Flaga, dzięki czemu możemy zobaczyć SQL są uruchamiane w bazie danych.

-   Uruchom **Update-Database — pełne** polecenia w konsoli Menedżera pakietów.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrowanie do określonej wersji (w tym obniżenia poziomu)

Do tej pory zawsze uaktualniliśmy do najnowszych migracji, ale mogą zaistnieć sytuacje, kiedy zechcesz, uaktualnianie i obniżanie wersji do migracji.

Załóżmy, że chcemy przeprowadzić migrację naszych bazy danych do stanu sprzed po uruchomieniu naszych **AddBlogUrl** migracji. Możemy użyć **— TargetMigration** przełącznika na starszą wersję tej migracji.

-   Uruchom **Update-Database-TargetMigration: AddBlogUrl** polecenia w konsoli Menedżera pakietów.

To polecenie uruchomi skrypt w dół dla naszych **AddBlogAbstract** i **AddPostClass** migracji.

Jeśli chcesz przeprowadzić wdrożenie aż z powrotem do pustej bazy danych, wówczas można użyć **Update-Database-TargetMigration: $InitialDatabase** polecenia.

## <a name="getting-a-sql-script"></a>Pobieranie skryptu SQL

Jeśli inny Deweloper chce tych zmian na swoich maszynach one po prostu synchronizacji po sprawdzenie zmian w systemie kontroli źródła. Po naszej nowej migracji one po prostu uruchomić polecenie Update-Database, aby zmiany zastosowana lokalnie. Jednak jeśli chcemy wdrożyć tych zmian na serwerze testowym i po pewnym czasie produkcji, prawdopodobnie chcemy skrypt SQL, które firma Microsoft może przekazują do naszych DBA.

-   Uruchom **Update-Database** polecenia, ale tym razem określ **— skrypt** Flaga tak, aby zmiany są zapisywane do skryptu zamiast stosowane. Firma Microsoft będzie również określić źródło i cel migracji można wygenerować skryptu dla. Chcemy, aby skrypt, aby przejść od pustej bazy danych (**$InitialDatabase**) do najnowszej wersji (migracja **AddPostAbstract**).
    *Jeśli nie określisz migracji docelowego migracji użyje najnowszych migracji jako element docelowy. Jeśli nie określisz migracje źródła migracje użyje bieżący stan bazy danych.*
-   Uruchom **Update-Database-Script - SourceMigration: $InitialDatabase - TargetMigration: AddPostAbstract** polecenia w konsoli Menedżera pakietów

Migracje Code First uruchomi proces migracji, ale zamiast faktycznie stosowania zmian go będzie zapisać je do pliku SQL dla Ciebie. Wygenerowany skrypt jest otwierany automatycznie w programie Visual Studio gotowe wyświetlić lub zapisać.

### <a name="generating-idempotent-scripts"></a>Generowanie skryptów Idempotentne

Począwszy od platformy EF6, jeśli określisz **— SourceMigration $InitialDatabase** wygenerowany skrypt będzie "idempotentne". Skrypty Idempotentne można uaktualnić bazy danych obecnie w dowolnej wersji do najnowszej wersji (lub określonej wersji, jeśli używasz **— TargetMigration**). Wygenerowany skrypt zawiera logikę do sprawdzenia  **\_ \_MigrationsHistory** tabeli i tylko zastosować zmiany, które nie zostały zastosowane wcześniej.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Automatycznie uaktualnianie przy uruchamianiu aplikacji (inicjatorze MigrateDatabaseToLatestVersion)

Jeśli aplikacja jest wdrażana można ją automatycznie uaktualnić bazy danych (stosując wszystkie oczekujące migracje) po uruchomieniu aplikacji. Można to zrobić, rejestrując **MigrateDatabaseToLatestVersion** inicjatora bazy danych. Inicjator bazy danych zawiera po prostu logikę, który służy do upewnij się, że baza danych jest prawidłowo skonfigurowana. Logika jest uruchamiany po raz pierwszy kontekstu jest używana w ramach procesu aplikacji (**AppDomain**).

Firma Microsoft może aktualizować **Program.cs** pliku, jak pokazano poniżej, aby ustawić **MigrateDatabaseToLatestVersion** inicjator dla BlogContext, zanim użyjemy kontekstu (wiersz 14). Należy pamiętać, że trzeba będzie również dodać za pomocą instrukcji for **System.Data.Entity** przestrzeni nazw (wiersz 5).

*Kiedy tworzymy wystąpienie tego inicjatora, należy określić typ kontekstu (**BlogContext**) i konfiguracji migracji (**konfiguracji**) — Konfiguracja migracji jest klasa, która otrzymała dodany do naszych **migracje** folderu podczas umożliwiliśmy migracji.*

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
                Database.SetInitializer(new MigrateDatabaseToLatestVersion\<BlogContext, Configuration>());

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

Teraz zawsze, gdy nasze działania aplikacji, najpierw będzie sprawdzał, jeśli baza danych jego celem jest element jest aktualny i Zastosuj wszelkie oczekujące migracji, jeśli nie jest.

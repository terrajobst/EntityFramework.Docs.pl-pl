---
title: Migracje Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418963"
---
# <a name="code-first-migrations"></a>Migracje Code First
Migracje Code First jest zalecanym sposobem, aby rozwijać schemat bazy danych aplikacji, jeśli jest używany przepływ pracy Code First. Migracje udostępniają zestaw narzędzi umożliwiających:

1. Utwórz początkową bazę danych, która współpracuje z modelem EF
2. Generowanie migracji w celu śledzenia zmian wprowadzonych w modelu EF
2. Aktualizuj bazę danych o te zmiany

Poniższy przewodnik zawiera omówienie Migracje Code First w Entity Framework. Możesz ukończyć cały przewodnik lub przejść do tematu, który Cię interesuje. Omówione są następujące tematy:

## <a name="building-an-initial-model--database"></a>Kompilowanie wstępnego modelu & bazy danych

Przed rozpoczęciem korzystania z migracji potrzebujemy projektu i modelu Code First do pracy z programem. W tym instruktażu będziemy używać **bloga** i modelu **post** .

-   Utwórz nową aplikację konsolową **MigrationsDemo**
-   Dodaj najnowszą wersję pakietu NuGet **EntityFramework** do projektu
    -   **Tools — Menedżer pakietów&gt; Library —&gt; konsoli Menedżera pakietów**
    -   Uruchom polecenie **install-package EntityFramework**
-   Dodaj plik **model.cs** z kodem pokazanym poniżej. Ten kod definiuje jedną klasę **blogu** , która składa się z naszego modelu domeny i klasy **BlogContext** , która stanowi nasz kontekst Code First

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

-   Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do danych. Zaktualizuj plik **program.cs** za pomocą kodu pokazanego poniżej.

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

-   Uruchom aplikację i zobaczysz, że została utworzona baza danych **MigrationsCodeDemo. BlogContext** .

    ![LocalDB bazy danych](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Włączanie migracji

Jest to czas na wprowadzenie kolejnych zmian w modelu.

-   Wprowadźmy Właściwość adresu URL do klasy bloga.

``` csharp
    public string Url { get; set; }
```

Jeśli chcesz uruchomić aplikację ponownie, otrzymasz InvalidOperationException z informacją o *tym, że model zapasowy kontekstu "BlogContext" został zmieniony od czasu utworzenia bazy danych. Aby zaktualizować bazę danych (http://go.microsoft.com/fwlink/?LinkId=238269), należy rozważyć użycie Migracje Code First* [](https://go.microsoft.com/fwlink/?LinkId=238269) *.*

Z powodu wyjątku sugerujemy czas rozpoczęcia korzystania z Migracje Code First. Pierwszym krokiem jest włączenie migracji w naszym kontekście.

-   Uruchamianie polecenia **enable-migrations** w konsoli Menedżera pakietów

    To polecenie dodało folder **migracji** do naszego projektu. Ten nowy folder zawiera dwa pliki:

-   **Klasa konfiguracji.** Ta klasa umożliwia skonfigurowanie sposobu zachowania migracji dla kontekstu. W tym instruktażu zostanie użyta domyślna konfiguracja.
    *Ponieważ istnieje tylko jeden Code First kontekst w projekcie, Enable-migrations automatycznie wypełnia typ kontekstu, do którego odnosi się ta konfiguracja.*
-   **Migracja InitialCreate**. Ta migracja została wygenerowana, ponieważ Code First utworzyć bazę danych dla nas przed włączeniem migracji. Kod w tej migracji szkieletowej reprezentuje obiekty, które zostały już utworzone w bazie danych. W naszym przypadku jest to tabela **blogów** z **BlogId** i kolumnami **nazw** . Nazwa pliku zawiera sygnaturę czasową ułatwiającą porządkowanie.
    *Jeśli baza danych nie została jeszcze utworzona, migracja InitialCreate nie została dodana do projektu. Zamiast tego podczas pierwszego wywołania metody Add-Migration kod służący do tworzenia tych tabel będzie szkieletem do nowej migracji.*

### <a name="multiple-models-targeting-the-same-database"></a>Wiele modeli przeznaczonych dla tej samej bazy danych

W przypadku korzystania z wersji wcześniejszych niż EF6 tylko jeden model Code First może być używany do generowania schematu bazy danych i zarządzania nią. Jest to wynik pojedynczej **\_\_tabeli MigrationsHistory** na bazę danych bez możliwości zidentyfikowania wpisów należących do tego modelu.

Począwszy od EF6, Klasa **konfiguracji** zawiera właściwość **ContextKey** . Działa jako unikatowy identyfikator dla każdego modelu Code First. Odpowiadająca kolumna w **\_\_tabeli MigrationsHistory** umożliwia zapisów z wielu modeli w celu udostępnienia tabeli. Domyślnie ta właściwość ma ustawioną w pełni kwalifikowaną nazwę kontekstu.

## <a name="generating--running-migrations"></a>Generowanie & wykonywania migracji

Migracje Code First ma dwa podstawowe polecenia, z którymi zamierzasz się zapoznać.

-   W ramach **migracji** do kolejnej migracji w oparciu o zmiany wprowadzone w modelu od momentu utworzenia ostatniej migracji
-   **Aktualizacja — baza danych** zastosuje wszystkie oczekujące migracje do bazy danych

Musimy utworzyć szkielet migracji, aby zachować ostrożność podczas dodawania nowej właściwości adresu URL. Polecenie **Add-Migration** umożliwia nam nadanie nazw migracji nazwy, przywołują nasze **AddBlogUrl**.

-   Uruchamianie polecenia **Add-Migration AddBlogUrl** w konsoli Menedżera pakietów
-   W folderze **migracji** mamy teraz nową migrację **AddBlogUrl** . Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby ułatwić porządkowanie

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

Możemy teraz edytować lub dodawać do tej migracji, ale wszystko wygląda dość dobrze. Aby zastosować tę migrację do bazy danych, użyjmy do bazy **danych Update-Database** .

-   Uruchamianie polecenia **Update-Database** w konsoli Menedżera pakietów
-   Migracje Code First będzie porównać migracje w naszym folderze **migracji** z tymi, które zostały zastosowane do bazy danych programu. Zobaczy on, że należy zastosować migrację **AddBlogUrl** i uruchomić ją.

Baza danych **MigrationsDemo. BlogContext** jest teraz aktualizowana w celu uwzględnienia kolumny **adresu URL** w tabeli **blogów** .

## <a name="customizing-migrations"></a>Dostosowywanie migracji

Do tej pory wygenerowałeś i przeprowadzimy migrację bez wprowadzania żadnych zmian. Teraz przyjrzyjmy się edycji kodu, który jest generowany domyślnie.

-   Aby wprowadzić więcej zmian w modelu, dodajmy nową właściwość **rankingu** do klasy **bloga**

``` csharp
    public int Rating { get; set; }
```

-   Dodajmy również nową klasę **post**

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

-   Dodamy również kolekcję **ogłoszeń** do klasy **blog** , aby utworzyć drugi koniec relacji między **blogiem** i **wpisem**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Użyjemy polecenia **Add-Migration** , aby zapewnić, że migracje Code First szkieletem na potrzeby migracji do nas. Będziemy wywoływać to **AddPostClass**migracji.

-   Uruchom polecenie **Add-Migration AddPostClass** w konsoli Menedżera pakietów.

Migracje Code First było bardzo dobre zadanie tworzenia szkieletu tych zmian, ale istnieje kilka rzeczy, które warto zmienić:

1.  Najpierw Dodaj unikatowy indeks do **wpisów. kolumna tytułu** (dodanie w wierszu 22 & 29 w poniższym kodzie).
2.  Dodawana jest również kolumna " **Blogi** " z nie dopuszczaniem wartości null. Jeśli w tabeli znajdują się jakieś dane, zostanie do niego przypisana domyślna wartość CLR typu danych dla nowej kolumny (Klasyfikacja jest liczbą całkowitą, tak aby była **równa 0**). Ale chcemy określić wartość domyślną równą **3** , aby istniejące wiersze w tabeli **blogów** miały od znośnego klasyfikację.
    (Można zobaczyć wartość domyślną określoną w wierszu 24 poniższego kodu)

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

Twoja edytowana migracja jest gotowa do użycia, więc użyjemy **aktualizacji bazy danych** , aby zapewnić aktualność bazy danych. Ten czas pozwala określić flagę **– verbose** , aby zobaczyć, że program SQL migracje Code First jest uruchomiony.

-   Uruchom polecenie **Update-Database – verbose** w konsoli Menedżera pakietów.

## <a name="data-motion--custom-sql"></a>Ruch danych/niestandardowe SQL

Do tej pory oglądamy operacje migracji, które nie zmieniają ani nie przechodzą żadnych danych, teraz przyjrzyjmy się coś, co będzie wymagało przeniesienia pewnych danych. Nie ma jeszcze natywnej obsługi ruchu danych, ale można uruchamiać w dowolnym momencie dowolne polecenia SQL.

-   Dodajmy do naszego modelu Właściwość **post. abstract** . Później będziemy wstępnie wypełniać **streszczenie** istniejących wpisów przy użyciu pewnego tekstu od początku kolumny **Content (zawartość** ).

``` csharp
    public string Abstract { get; set; }
```

Użyjemy polecenia **Add-Migration** , aby zapewnić, że migracje Code First szkieletem na potrzeby migracji do nas.

-   Uruchom polecenie **Add-Migration AddPostAbstract** w konsoli Menedżera pakietów.
-   Wygenerowane migracje uwzględniają zmiany schematu, ale chcemy również wstępnie wypełnić kolumnę **abstrakcyjną** przy użyciu pierwszych 100 znaków zawartości dla każdego wpisu. Możemy to zrobić przez upuszczenie na SQL i uruchomienie instrukcji **Update** po dodaniu kolumny.
    (Dodawanie w wierszu 12 w poniższym kodzie)

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

Twoja edytowana migracja jest dobra, dlatego użyjemy polecenia **Update-Database** , aby zapewnić aktualność bazy danych. Określimy flagę **– verbose** , aby można było zobaczyć uruchamianie kodu SQL w bazie danych.

-   Uruchom polecenie **Update-Database – verbose** w konsoli Menedżera pakietów.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrowanie do określonej wersji (w tym obniżanie poziomu)

Do tej pory zawsze uaktualniono do najnowszej migracji, ale mogą wystąpić sytuacje, w których uaktualnienie/obniżenie poziomu zostanie przeprowadzone w określonej migracji.

Załóżmy, że chcemy przeprowadzić migrację bazy danych do stanu, w którym znajdowała się po zakończeniu migracji **AddBlogUrl** . Do obniżenia poziomu migracji można użyć przełącznika **– TargetMigration** .

-   Uruchom polecenie **Update-Database – TargetMigration: AddBlogUrl** w konsoli Menedżera pakietów.

To polecenie spowoduje uruchomienie skryptu w dół dla naszych **AddBlogAbstract** i migracji **AddPostClass** .

Jeśli chcesz przywrócić cały sposób do pustej bazy danych, możesz użyć polecenia **Update-Database – TargetMigration: $InitialDatabase** .

## <a name="getting-a-sql-script"></a>Pobieranie skryptu SQL

Jeśli inny deweloper chce zmienić te zmiany na komputerze, może po prostu przeprowadzić synchronizację, gdy sprawdzimy nasze zmiany w kontroli źródła. Po otrzymaniu nowych migracji można po prostu uruchomić polecenie Update-Database, aby zmiany zostały zastosowane lokalnie. Jeśli jednak chcemy wypchnąć te zmiany do serwera testowego i ostatecznie produkcji, prawdopodobnie chcemy, aby skrypt SQL mógł zostać przekazany do naszego DBA.

-   Uruchom polecenie **Update-Database** , ale ten czas należy określić flagę **– Script** , tak aby zmiany były zapisywane do skryptu, a nie do zastosowania. Określimy również migrację źródłową i docelową w celu wygenerowania skryptu. Chcemy, aby skrypt przeszedł z pustej bazy danych ( **$InitialDatabase**) do najnowszej wersji ( **AddPostAbstract**migracji).
    *Jeśli nie określisz docelowej migracji, migracja będzie używać najnowszej migracji jako docelowej. Jeśli nie określisz migracji źródła, migracja będzie używać bieżącego stanu bazy danych.*
-   Uruchom polecenie **Update-Database-Script-SourceMigration: $InitialDatabase-TargetMigration: AddPostAbstract** w konsoli Menedżera pakietów

Migracje Code First uruchomi potok migracji, ale zamiast tego rzeczywiście zastosuje zmiany, zapisze je do pliku SQL. Po wygenerowaniu skryptu zostanie on otwarty w programie Visual Studio, gotowy do wyświetlenia lub zapisania.

### <a name="generating-idempotent-scripts"></a>Generowanie skryptów idempotentne

Rozpoczynając od EF6, jeśli określono wartość **– SourceMigration $InitialDatabase** , wygenerowany skrypt będzie "idempotentne". Skrypty idempotentne mogą uaktualnić bazę danych obecnie w dowolnej wersji do najnowszej wersji (lub określonej wersji, jeśli używasz programu **– TargetMigration**). Wygenerowany skrypt zawiera logikę, aby sprawdzić **\_\_tabeli MigrationsHistory** i zastosować tylko zmiany, które nie zostały wcześniej zastosowane.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Automatyczne uaktualnianie przy uruchamianiu aplikacji (inicjator MigrateDatabaseToLatestVersion)

W przypadku wdrażania aplikacji może być konieczne automatyczne uaktualnienie bazy danych (przez zastosowanie wszelkich oczekujących migracji) podczas uruchamiania aplikacji. Można to zrobić przez zarejestrowanie inicjatora bazy danych **MigrateDatabaseToLatestVersion** . Inicjator bazy danych po prostu zawiera logikę, która jest używana do poprawnego skonfigurowania bazy danych. Ta logika jest uruchamiana przy pierwszym użyciu kontekstu w procesie aplikacji (**AppDomain**).

Można zaktualizować plik **program.cs** , jak pokazano poniżej, aby ustawić inicjator **MigrateDatabaseToLatestVersion** dla BlogContext przed użyciem kontekstu (wiersz 14). Należy pamiętać, że należy również dodać instrukcję using dla przestrzeni nazw **System. Data. Entity** (wiersz 5).

*Gdy utworzymy wystąpienie tego inicjatora, musimy określić typ kontekstu (**BlogContext**) i konfigurację migracji (**konfigurację**) — Konfiguracja migracji jest klasą dodaną do folderu **migracji** po włączeniu migracji.*

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

Teraz za każdym razem, gdy aplikacja jest uruchamiana, najpierw sprawdza, czy baza danych, do której jest przeznaczona, jest aktualna, i zastosuje oczekujące migracje, jeśli nie jest.

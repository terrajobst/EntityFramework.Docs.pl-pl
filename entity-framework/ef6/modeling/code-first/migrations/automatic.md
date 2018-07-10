---
title: Migracje automatyczne Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
caps.latest.revision: 3
ms.openlocfilehash: 1f6ed728271e38d8e65fcf33fd45c74f085d9178
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912683"
---
# <a name="automatic-code-first-migrations"></a>Migracje automatyczne Code First
Automatycznej migracji pozwala użyć migracje Code First bez potrzeby plik kodu dla każdej zmiany wprowadzone w projekcie. Nie wszystkie zmiany można zastosować automatycznie — na przykład zmienia nazwę kolumny korzystają z migracji za pomocą kodu.

> [!NOTE]
> W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy. Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="recommendation-for-team-environments"></a>Zalecenia dotyczące środowiska zespołowe

Możesz rozdzielać migracje w architekturze kodu i automatyczne, ale nie jest to zalecane w scenariuszach tworzenia zespołu. Jeśli jesteś częścią zespołu deweloperów korzystających z kontroli źródła albo należy używać wyłącznie automatycznej migracji lub migracje oparte wyłącznie na kodzie. Biorąc pod uwagę ograniczenia na potrzeby automatycznej migracji firma Microsoft zaleca, za pomocą migracje oparte na kodzie w środowiskach zespołu.

## <a name="building-an-initial-model--database"></a>Tworzenie początkowej modelu i bazy danych

Zanim zaczniemy, za pomocą migracji potrzebujemy projektu i model Code First chcesz pracować. W tym przewodniku są użyjemy canonical **Blog** i **wpis** modelu.

-   Utwórz nową **MigrationsAutomaticDemo** aplikacji konsoli
-   Dodaj najnowszą wersję **EntityFramework** pakiet NuGet do projektu
    -   **Narzędzia —&gt; Menedżer pakietów biblioteki —&gt; Konsola Menedżera pakietów**
    -   Uruchom **EntityFramework instalacji pakietu** polecenia
-   Dodaj **Model.cs** pliku z kodem, pokazano poniżej. Ten kod definiuje pojedynczy **Blog** klasę, która sprawia, że nasz model domeny i **BlogContext** klasę, która jest nasz kontekst EF Code First

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
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

      namespace MigrationsAutomaticDemo
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

-   Uruchom aplikację i zostanie wyświetlony **MigrationsAutomaticCodeDemo.BlogContext** baza danych została utworzona dla Ciebie.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Włączanie migracji

Nadszedł czas na pewnych zmian więcej w naszym modelu.

-   Umożliwia wprowadzenie właściwość adres Url do klasy blogu.

``` csharp
    public string Url { get; set; }
```

Jeśli zamierzasz uruchomić aplikację ponownie otrzymamy InvalidOperationException, podając *modelu kopii kontekstu "BlogContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First w aktualizacji bazy danych (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*

Jak sugeruje wyjątek, jest czas, aby rozpocząć korzystanie z migracje Code First. Ponieważ chcemy użyć migracje automatyczne zamierzamy określ **— EnableAutomaticMigrations** przełącznika.

-   Uruchom **Enable-Migrations — EnableAutomaticMigrations** polecenia w poleceniu ten konsoli Menedżera pakietów została dodana **migracje** folderu do naszego projektu. Ten nowy folder zawiera jeden plik:

-   **Klasa konfiguracji.** Ta klasa pozwala skonfigurować sposób działania migracji w Twoim kontekście. W tym przewodniku po prostu używamy domyślnej konfiguracji.
    *Ponieważ istnieje tylko pojedynczy kontekst Code First w projekcie, Enable-Migrations jest wypełniane automatycznie typ kontekstu, którego dotyczy ta konfiguracja.*

 

## <a name="your-first-automatic-migration"></a>Pierwszy automatycznej migracji

Migracje Code First ma dwa podstawowe polecenia, które ma być zapoznanie się z.

-   **Dodaj migracji** będzie tworzenia szkieletu dalej migracji, w oparciu o zmiany wprowadzone do modelu od chwili utworzenia ostatniej migracji
-   **Update-Database** Zastosuj wszelkie oczekujące migracji do bazy danych

Firma Microsoft zamierza uniknąć za pomocą Dodaj migracji (chyba że to naprawdę musimy) i skoncentrować się na umożliwienie migracje Code First automatycznie obliczyć i zastosować zmiany. Użyjmy **Update-Database** można pobrać migracje Code First, aby wypchnąć zmiany do nasz model (nowy **Blog.Ur**właściwość l) w bazie danych.

-   Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów.

**MigrationsAutomaticDemo.BlogContext** bazy danych został zaktualizowany do uwzględnienia **adresu Url** kolumny w **blogi** tabeli.

 

## <a name="your-second-automatic-migration"></a>Drugi automatycznej migracji

Upewnijmy się, innej zmiany i pozwól migracje Code First automatycznie wypychać zmiany do bazy danych dla nas.

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

Teraz za pomocą **Update-Database** Aby przełączyć bazę danych aktualne. Teraz możemy określić **— pełne** Flaga, dzięki czemu możesz zobaczyć SQL Server, który działa migracje Code First.

-   Uruchom **Update-Database — pełne** polecenia w konsoli Menedżera pakietów.

## <a name="adding-a-code-based-migration"></a>Dodawanie kodu na podstawie migracji

Teraz Przyjrzyjmy się coś, co firma Microsoft może być konieczne użycie oparty na kodzie migracja.

-   Dodajmy **ocena** właściwości **blogu** klasy

``` csharp
    public int Rating { get; set; }
```

Firma Microsoft może po prostu uruchomić **Update-Database** do wypychania tych zmian w bazie danych. Jednak dodajemy dopuszcza **Blogs.Rating** kolumny, w przypadku istniejących danych w tabeli zostaną zostaną przypisane domyślne CLR typu danych dla nowej kolumny (ocena jest liczba całkowita, tak byłoby **0**). Ale chcemy określić wartość domyślną **3** więc w istniejącym wiersze **blogi** tabeli rozpoczyna się od klasyfikacji znośnego.
Użyjemy polecenia Add-migracji można zapisać tej zmiany, które się do migracji za pomocą kodu, dzięki czemu możemy poddać edycji. **Migracji Dodaj** polecenie umożliwia nam nazwij te migracji, po prostu nazwiemy naszych **AddBlogRating**.

-   Uruchom **AddBlogRating migracji Dodaj** polecenia w konsoli Menedżera pakietów.
-   W **migracje** folderu w efekcie powstał nowy **AddBlogRating** migracji. Nazwa pliku migracji jest wstępnie stała z sygnaturą czasową pomagające w kolejności. Umożliwia edytowanie wygenerowany kod, aby określić domyślną wartość 3 dla Blog.Rating (wiersz 10 w poniższym kodzie)

*Migracji ma również plik związany z kodem, który przechwytuje niektórych metadanych. Te metadane umożliwi migracje Code First replikowanie automatycznej migracji, możemy wykonać przed migracją to oparty na kodzie. Jest to ważne, jeśli inny Deweloper chce Uruchom migracje naszych lub gdy nadejdzie czas wdrażania naszej aplikacji.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

Nasze edytowanych migracji jest dobra, dlatego użyjemy **Update-Database** Aby przełączyć bazę danych aktualne.

-   Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów.

## <a name="back-to-automatic-migrations"></a>Powrót do automatycznej migracji

Jesteśmy teraz bezpłatnych wrócić do automatycznej migracji dla prostsze zmian. Migracje Code First zajmie się przeprowadzania migracji automatycznej i oparte na kodzie w odpowiedniej kolejności, na podstawie metadanych, która jest przechowywana w pliku związanym z kodem dla każdego migracji opartego na kodzie.

-   Dodajmy właściwość Post.Abstract naszym modelu

``` csharp
    public string Abstract { get; set; }
```

Teraz możemy użyć **Update-Database** można pobrać migracje Code First wypychania tej zmiany do bazy danych za pomocą automatycznej migracji.

-   Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów.

## <a name="summary"></a>Podsumowanie

W tym przewodniku pokazano, jak użyć migracje automatycznego wypychania modelu zmienia się z bazą danych. Przedstawiono także sposób tworzenia szkieletu i uruchamiania oparte na kodzie migracje między automatycznej migracji, gdy potrzebujesz większej kontroli.

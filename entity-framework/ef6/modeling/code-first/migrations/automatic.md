---
title: Automatyczne Migracje Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419001"
---
# <a name="automatic-code-first-migrations"></a>Automatyczne Migracje Code First
Automatyczne migracje umożliwiają używanie Migracje Code First bez pliku kodu w projekcie dla każdej wprowadzonej zmiany. Nie wszystkie zmiany mogą być stosowane automatycznie — na przykład zmiana nazw kolumn wymaga użycia migracji opartej na kodzie.

> [!NOTE]
> W tym artykule założono, że wiesz, jak używać Migracje Code First w podstawowych scenariuszach. Jeśli tego nie zrobisz, musisz przeczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="recommendation-for-team-environments"></a>Zalecenie dla środowisk zespołu

Można przeplatać migracje automatyczne i oparte na kodzie, ale nie jest to zalecane w scenariuszach deweloperskich zespołu. Jeśli jesteś częścią zespołu deweloperów korzystających z kontroli źródła, należy używać czysto automatycznych migracji lub czystych migracji opartych na kodzie. Uwzględniając ograniczenia migracji automatycznej, zalecamy korzystanie z migracji opartych na kodzie w środowiskach zespołu.

## <a name="building-an-initial-model--database"></a>Kompilowanie wstępnego modelu & bazy danych

Przed rozpoczęciem korzystania z migracji potrzebujemy projektu i modelu Code First do pracy z programem. W tym instruktażu będziemy używać **bloga** i modelu **post** .

-   Utwórz nową aplikację konsolową **MigrationsAutomaticDemo**
-   Dodaj najnowszą wersję pakietu NuGet **EntityFramework** do projektu
    -   **Tools — Menedżer pakietów&gt; Library —&gt; konsoli Menedżera pakietów**
    -   Uruchom polecenie **install-package EntityFramework**
-   Dodaj plik **model.cs** z kodem pokazanym poniżej. Ten kod definiuje jedną klasę **blogu** , która składa się z naszego modelu domeny i klasy **BlogContext** , która stanowi nasz kontekst Code First

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

-   Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do danych. Zaktualizuj plik **program.cs** za pomocą kodu pokazanego poniżej.

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

-   Uruchom aplikację i zobaczysz, że została utworzona baza danych **MigrationsAutomaticCodeDemo. BlogContext** .

    ![LocalDB bazy danych](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Włączanie migracji

Jest to czas na wprowadzenie kolejnych zmian w modelu.

-   Wprowadźmy Właściwość adresu URL do klasy bloga.

``` csharp
    public string Url { get; set; }
```

Jeśli chcesz uruchomić aplikację ponownie, otrzymasz InvalidOperationException z informacją o *tym, że model zapasowy kontekstu "BlogContext" został zmieniony od czasu utworzenia bazy danych. Aby zaktualizować bazę danych (http://go.microsoft.com/fwlink/?LinkId=238269), należy rozważyć użycie Migracje Code First* [](https://go.microsoft.com/fwlink/?LinkId=238269) *.*

Z powodu wyjątku sugerujemy czas rozpoczęcia korzystania z Migracje Code First. Ze względu na to, że chcemy użyć automatycznych migracji, zamierzamy określić przełącznik **– EnableAutomaticMigrations** .

-   Uruchom polecenie **enable-migrations-EnableAutomaticMigrations** w konsoli Menedżera pakietów to polecenie dodało folder **migracji** do projektu. Ten nowy folder zawiera jeden plik:

-   **Klasa konfiguracji.** Ta klasa umożliwia skonfigurowanie sposobu zachowania migracji dla kontekstu. W tym instruktażu zostanie użyta domyślna konfiguracja.
    *Ponieważ istnieje tylko jeden Code First kontekst w projekcie, Enable-migrations automatycznie wypełnia typ kontekstu, do którego odnosi się ta konfiguracja.*

 

## <a name="your-first-automatic-migration"></a>Twoja pierwsza automatyczna migracja

Migracje Code First ma dwa podstawowe polecenia, z którymi zamierzasz się zapoznać.

-   W ramach **migracji** do kolejnej migracji w oparciu o zmiany wprowadzone w modelu od momentu utworzenia ostatniej migracji
-   **Aktualizacja — baza danych** zastosuje wszystkie oczekujące migracje do bazy danych

Będziemy unikać używania dodatku do migracji (chyba że naprawdę to konieczne) i skupić się na umożliwieniu Migracje Code First automatycznego obliczania i stosowania zmian. Użyjmy polecenia **Update-Database** , aby uzyskać migracje Code First w celu wypchnięcia zmian do naszego modelu (Nowa właściwość **bloga**l) do bazy danych.

-   Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.

Baza danych **MigrationsAutomaticDemo. BlogContext** jest teraz aktualizowana w celu uwzględnienia kolumny **adresu URL** w tabeli **blogów** .

 

## <a name="your-second-automatic-migration"></a>Twoja druga automatyczna migracja

Wprowadźmy kolejną zmianę i pozwól, Migracje Code First automatycznie wypychanie zmian do bazy danych.

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

Teraz korzystaj z bazy **danych Update-Database** , aby zapewnić aktualność bazy danych. Ten czas pozwala określić flagę **– verbose** , aby zobaczyć, że program SQL migracje Code First jest uruchomiony.

-   Uruchom polecenie **Update-Database – verbose** w konsoli Menedżera pakietów.

## <a name="adding-a-code-based-migration"></a>Dodawanie migracji opartej na kodzie

Teraz przyjrzyjmy się coś, co będziemy potrzebować do użycia migracji opartej na kodzie dla programu.

-   Dodajmy do klasy **bloga** Właściwość **Rating**

``` csharp
    public int Rating { get; set; }
```

Po prostu można uruchomić z **bazy danych Update-Database** , aby wypchnąć te zmiany do bazy danych. Jednak dodawana jest kolumna " **blogi. Rating** ", która nie dopuszcza wartości null, jeśli w tabeli znajdują się jakieś dane, do których zostanie przypisany domyślny typ danych dla nowej kolumny (Klasyfikacja jest liczbą całkowitą, tak aby była **równa 0**). Ale chcemy określić wartość domyślną równą **3** , aby istniejące wiersze w tabeli **blogów** miały od znośnego klasyfikację.
Użyjmy polecenia Add-Migration, aby napisać tę zmianę do migracji opartej na kodzie, aby można było ją edytować. Polecenie **Add-Migration** umożliwia nam nadanie nazw migracji nazwy, przywołują nasze **AddBlogRating**.

-   Uruchom polecenie **Add-Migration AddBlogRating** w konsoli Menedżera pakietów.
-   W folderze **migracji** mamy teraz nową migrację **AddBlogRating** . Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby pomóc w określeniu kolejności. Zmodyfikujmy wygenerowany kod, aby określić wartość domyślną 3 dla blogu. Klasyfikacja (wiersz 10 w poniższym kodzie)

*Migracja ma także plik związany z kodem, który przechwytuje niektóre metadane. Te metadane umożliwią Migracje Code First replikowania automatycznych migracji wykonanych przed tą migracją opartą na kodzie. Jest to ważne, jeśli inny deweloper chce przeprowadzić migracje lub czas wdrożenia naszej aplikacji.*

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

Twoja edytowana migracja jest dobra, dlatego użyjemy polecenia **Update-Database** , aby zapewnić aktualność bazy danych.

-   Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.

## <a name="back-to-automatic-migrations"></a>Z powrotem do automatycznych migracji

Teraz możemy przełączyć się z powrotem do automatycznej migracji, aby uprościć zmiany. Migracje Code First zadbają o przeprowadzenie migracji automatycznej i opartej na kodzie w odpowiedniej kolejności na podstawie metadanych przechowywanych w pliku związanym z kodem dla każdej migracji opartej na kodzie.

-   Dodajmy do naszego modelu Właściwość post. abstract

``` csharp
    public string Abstract { get; set; }
```

Teraz możemy użyć usługi **Update-Database** , aby uzyskać migracje Code First do wypchnięcia tej zmiany do bazy danych przy użyciu automatycznej migracji.

-   Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.

## <a name="summary"></a>Podsumowanie

W tym instruktażu przedstawiono sposób korzystania z automatycznych migracji w celu wypychania zmian modelu do bazy danych. Pokazano również, jak szkieletować i uruchamiać migracje oparte na kodzie między automatyczną migracją, gdy potrzebna jest większa kontrola.

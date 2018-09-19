---
title: Migracje automatyczne Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283917"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="d35f5-102">Migracje automatyczne Code First</span><span class="sxs-lookup"><span data-stu-id="d35f5-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="d35f5-103">Automatycznej migracji pozwala użyć migracje Code First bez potrzeby plik kodu dla każdej zmiany wprowadzone w projekcie.</span><span class="sxs-lookup"><span data-stu-id="d35f5-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="d35f5-104">Nie wszystkie zmiany można zastosować automatycznie — na przykład zmienia nazwę kolumny korzystają z migracji za pomocą kodu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="d35f5-105">W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="d35f5-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="d35f5-106">Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="d35f5-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="d35f5-107">Zalecenia dotyczące środowiska zespołowe</span><span class="sxs-lookup"><span data-stu-id="d35f5-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="d35f5-108">Możesz rozdzielać migracje w architekturze kodu i automatyczne, ale nie jest to zalecane w scenariuszach tworzenia zespołu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="d35f5-109">Jeśli jesteś częścią zespołu deweloperów korzystających z kontroli źródła albo należy używać wyłącznie automatycznej migracji lub migracje oparte wyłącznie na kodzie.</span><span class="sxs-lookup"><span data-stu-id="d35f5-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="d35f5-110">Biorąc pod uwagę ograniczenia na potrzeby automatycznej migracji firma Microsoft zaleca, za pomocą migracje oparte na kodzie w środowiskach zespołu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="d35f5-111">Tworzenie początkowej modelu i bazy danych</span><span class="sxs-lookup"><span data-stu-id="d35f5-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="d35f5-112">Zanim zaczniemy, za pomocą migracji potrzebujemy projektu i model Code First chcesz pracować.</span><span class="sxs-lookup"><span data-stu-id="d35f5-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="d35f5-113">W tym przewodniku są użyjemy canonical **Blog** i **wpis** modelu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="d35f5-114">Utwórz nową **MigrationsAutomaticDemo** aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="d35f5-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="d35f5-115">Dodaj najnowszą wersję **EntityFramework** pakiet NuGet do projektu</span><span class="sxs-lookup"><span data-stu-id="d35f5-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="d35f5-116">**Narzędzia —&gt; Menedżer pakietów biblioteki —&gt; Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="d35f5-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="d35f5-117">Uruchom **EntityFramework instalacji pakietu** polecenia</span><span class="sxs-lookup"><span data-stu-id="d35f5-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="d35f5-118">Dodaj **Model.cs** pliku z kodem, pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d35f5-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="d35f5-119">Ten kod definiuje pojedynczy **Blog** klasę, która sprawia, że nasz model domeny i **BlogContext** klasę, która jest nasz kontekst EF Code First</span><span class="sxs-lookup"><span data-stu-id="d35f5-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="d35f5-120">Teraz, gdy model, nadszedł czas na jej używać do wykonywania dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="d35f5-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="d35f5-121">Aktualizacja **Program.cs** pliku z kodem, pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d35f5-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="d35f5-122">Uruchom aplikację i zostanie wyświetlony **MigrationsAutomaticCodeDemo.BlogContext** baza danych została utworzona dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="d35f5-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Bazy danych LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="d35f5-124">Włączanie migracji</span><span class="sxs-lookup"><span data-stu-id="d35f5-124">Enabling Migrations</span></span>

<span data-ttu-id="d35f5-125">Nadszedł czas na pewnych zmian więcej w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="d35f5-126">Umożliwia wprowadzenie właściwość adres Url do klasy blogu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="d35f5-127">Jeśli zamierzasz uruchomić aplikację ponownie otrzymamy InvalidOperationException, podając *modelu kopii kontekstu "BlogContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First w aktualizacji bazy danych (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="d35f5-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="d35f5-128">Jak sugeruje wyjątek, jest czas, aby rozpocząć korzystanie z migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="d35f5-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="d35f5-129">Ponieważ chcemy użyć migracje automatyczne zamierzamy określ **— EnableAutomaticMigrations** przełącznika.</span><span class="sxs-lookup"><span data-stu-id="d35f5-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="d35f5-130">Uruchom **Enable-Migrations — EnableAutomaticMigrations** polecenia w poleceniu ten konsoli Menedżera pakietów została dodana **migracje** folderu do naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="d35f5-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="d35f5-131">Ten nowy folder zawiera jeden plik:</span><span class="sxs-lookup"><span data-stu-id="d35f5-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="d35f5-132">**Klasa konfiguracji.**</span><span class="sxs-lookup"><span data-stu-id="d35f5-132">**The Configuration class.**</span></span> <span data-ttu-id="d35f5-133">Ta klasa pozwala skonfigurować sposób działania migracji w Twoim kontekście.</span><span class="sxs-lookup"><span data-stu-id="d35f5-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="d35f5-134">W tym przewodniku po prostu używamy domyślnej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d35f5-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="d35f5-135">*Ponieważ istnieje tylko pojedynczy kontekst Code First w projekcie, Enable-Migrations jest wypełniane automatycznie typ kontekstu, którego dotyczy ta konfiguracja.*</span><span class="sxs-lookup"><span data-stu-id="d35f5-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="d35f5-136">Pierwszy automatycznej migracji</span><span class="sxs-lookup"><span data-stu-id="d35f5-136">Your First Automatic Migration</span></span>

<span data-ttu-id="d35f5-137">Migracje Code First ma dwa podstawowe polecenia, które ma być zapoznanie się z.</span><span class="sxs-lookup"><span data-stu-id="d35f5-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="d35f5-138">**Dodaj migracji** będzie tworzenia szkieletu dalej migracji, w oparciu o zmiany wprowadzone do modelu od chwili utworzenia ostatniej migracji</span><span class="sxs-lookup"><span data-stu-id="d35f5-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="d35f5-139">**Update-Database** Zastosuj wszelkie oczekujące migracji do bazy danych</span><span class="sxs-lookup"><span data-stu-id="d35f5-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="d35f5-140">Firma Microsoft zamierza uniknąć za pomocą Dodaj migracji (chyba że to naprawdę musimy) i skoncentrować się na umożliwienie migracje Code First automatycznie obliczyć i zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="d35f5-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="d35f5-141">Użyjmy **Update-Database** można pobrać migracje Code First, aby wypchnąć zmiany do nasz model (nowy **Blog.Ur**właściwość l) w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d35f5-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="d35f5-142">Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d35f5-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="d35f5-143">**MigrationsAutomaticDemo.BlogContext** bazy danych został zaktualizowany do uwzględnienia **adresu Url** kolumny w **blogi** tabeli.</span><span class="sxs-lookup"><span data-stu-id="d35f5-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="d35f5-144">Drugi automatycznej migracji</span><span class="sxs-lookup"><span data-stu-id="d35f5-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="d35f5-145">Upewnijmy się, innej zmiany i pozwól migracje Code First automatycznie wypychać zmiany do bazy danych dla nas.</span><span class="sxs-lookup"><span data-stu-id="d35f5-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="d35f5-146">Możemy również dodać nowy **wpis** klasy</span><span class="sxs-lookup"><span data-stu-id="d35f5-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="d35f5-147">Dodamy również **wpisy** kolekcji **Blog** klasy w celu utworzenia drugiej stronie relacji między **Blog** i **wpis**</span><span class="sxs-lookup"><span data-stu-id="d35f5-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="d35f5-148">Teraz za pomocą **Update-Database** Aby przełączyć bazę danych aktualne.</span><span class="sxs-lookup"><span data-stu-id="d35f5-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="d35f5-149">Teraz możemy określić **— pełne** Flaga, dzięki czemu możesz zobaczyć SQL Server, który działa migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="d35f5-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="d35f5-150">Uruchom **Update-Database — pełne** polecenia w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d35f5-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="d35f5-151">Dodawanie kodu na podstawie migracji</span><span class="sxs-lookup"><span data-stu-id="d35f5-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="d35f5-152">Teraz Przyjrzyjmy się coś, co firma Microsoft może być konieczne użycie oparty na kodzie migracja.</span><span class="sxs-lookup"><span data-stu-id="d35f5-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="d35f5-153">Dodajmy **ocena** właściwości **blogu** klasy</span><span class="sxs-lookup"><span data-stu-id="d35f5-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="d35f5-154">Firma Microsoft może po prostu uruchomić **Update-Database** do wypychania tych zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d35f5-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="d35f5-155">Jednak dodajemy dopuszcza **Blogs.Rating** kolumny, w przypadku istniejących danych w tabeli zostaną zostaną przypisane domyślne CLR typu danych dla nowej kolumny (ocena jest liczba całkowita, tak byłoby **0**).</span><span class="sxs-lookup"><span data-stu-id="d35f5-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="d35f5-156">Ale chcemy określić wartość domyślną **3** więc w istniejącym wiersze **blogi** tabeli rozpoczyna się od klasyfikacji znośnego.</span><span class="sxs-lookup"><span data-stu-id="d35f5-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="d35f5-157">Użyjemy polecenia Add-migracji można zapisać tej zmiany, które się do migracji za pomocą kodu, dzięki czemu możemy poddać edycji.</span><span class="sxs-lookup"><span data-stu-id="d35f5-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="d35f5-158">**Migracji Dodaj** polecenie umożliwia nam nazwij te migracji, po prostu nazwiemy naszych **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="d35f5-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="d35f5-159">Uruchom **AddBlogRating migracji Dodaj** polecenia w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d35f5-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="d35f5-160">W **migracje** folderu w efekcie powstał nowy **AddBlogRating** migracji.</span><span class="sxs-lookup"><span data-stu-id="d35f5-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="d35f5-161">Nazwa pliku migracji jest wstępnie stała z sygnaturą czasową pomagające w kolejności.</span><span class="sxs-lookup"><span data-stu-id="d35f5-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="d35f5-162">Umożliwia edytowanie wygenerowany kod, aby określić domyślną wartość 3 dla Blog.Rating (wiersz 10 w poniższym kodzie)</span><span class="sxs-lookup"><span data-stu-id="d35f5-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="d35f5-163">*Migracji ma również plik związany z kodem, który przechwytuje niektórych metadanych. Te metadane umożliwi migracje Code First replikowanie automatycznej migracji, możemy wykonać przed migracją to oparty na kodzie. Jest to ważne, jeśli inny Deweloper chce Uruchom migracje naszych lub gdy nadejdzie czas wdrażania naszej aplikacji.*</span><span class="sxs-lookup"><span data-stu-id="d35f5-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="d35f5-164">Nasze edytowanych migracji jest dobra, dlatego użyjemy **Update-Database** Aby przełączyć bazę danych aktualne.</span><span class="sxs-lookup"><span data-stu-id="d35f5-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="d35f5-165">Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d35f5-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="d35f5-166">Powrót do automatycznej migracji</span><span class="sxs-lookup"><span data-stu-id="d35f5-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="d35f5-167">Jesteśmy teraz bezpłatnych wrócić do automatycznej migracji dla prostsze zmian.</span><span class="sxs-lookup"><span data-stu-id="d35f5-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="d35f5-168">Migracje Code First zajmie się przeprowadzania migracji automatycznej i oparte na kodzie w odpowiedniej kolejności, na podstawie metadanych, która jest przechowywana w pliku związanym z kodem dla każdego migracji opartego na kodzie.</span><span class="sxs-lookup"><span data-stu-id="d35f5-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="d35f5-169">Dodajmy właściwość Post.Abstract naszym modelu</span><span class="sxs-lookup"><span data-stu-id="d35f5-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="d35f5-170">Teraz możemy użyć **Update-Database** można pobrać migracje Code First wypychania tej zmiany do bazy danych za pomocą automatycznej migracji.</span><span class="sxs-lookup"><span data-stu-id="d35f5-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="d35f5-171">Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="d35f5-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="d35f5-172">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d35f5-172">Summary</span></span>

<span data-ttu-id="d35f5-173">W tym przewodniku pokazano, jak użyć migracje automatycznego wypychania modelu zmienia się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="d35f5-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="d35f5-174">Przedstawiono także sposób tworzenia szkieletu i uruchamiania oparte na kodzie migracje między automatycznej migracji, gdy potrzebujesz większej kontroli.</span><span class="sxs-lookup"><span data-stu-id="d35f5-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>

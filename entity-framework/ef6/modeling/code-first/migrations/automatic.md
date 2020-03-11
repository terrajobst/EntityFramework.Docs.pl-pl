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
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="37c3d-102">Automatyczne Migracje Code First</span><span class="sxs-lookup"><span data-stu-id="37c3d-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="37c3d-103">Automatyczne migracje umożliwiają używanie Migracje Code First bez pliku kodu w projekcie dla każdej wprowadzonej zmiany.</span><span class="sxs-lookup"><span data-stu-id="37c3d-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="37c3d-104">Nie wszystkie zmiany mogą być stosowane automatycznie — na przykład zmiana nazw kolumn wymaga użycia migracji opartej na kodzie.</span><span class="sxs-lookup"><span data-stu-id="37c3d-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="37c3d-105">W tym artykule założono, że wiesz, jak używać Migracje Code First w podstawowych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="37c3d-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="37c3d-106">Jeśli tego nie zrobisz, musisz przeczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="37c3d-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="37c3d-107">Zalecenie dla środowisk zespołu</span><span class="sxs-lookup"><span data-stu-id="37c3d-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="37c3d-108">Można przeplatać migracje automatyczne i oparte na kodzie, ale nie jest to zalecane w scenariuszach deweloperskich zespołu.</span><span class="sxs-lookup"><span data-stu-id="37c3d-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="37c3d-109">Jeśli jesteś częścią zespołu deweloperów korzystających z kontroli źródła, należy używać czysto automatycznych migracji lub czystych migracji opartych na kodzie.</span><span class="sxs-lookup"><span data-stu-id="37c3d-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="37c3d-110">Uwzględniając ograniczenia migracji automatycznej, zalecamy korzystanie z migracji opartych na kodzie w środowiskach zespołu.</span><span class="sxs-lookup"><span data-stu-id="37c3d-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="37c3d-111">Kompilowanie wstępnego modelu & bazy danych</span><span class="sxs-lookup"><span data-stu-id="37c3d-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="37c3d-112">Przed rozpoczęciem korzystania z migracji potrzebujemy projektu i modelu Code First do pracy z programem.</span><span class="sxs-lookup"><span data-stu-id="37c3d-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="37c3d-113">W tym instruktażu będziemy używać **bloga** i modelu **post** .</span><span class="sxs-lookup"><span data-stu-id="37c3d-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="37c3d-114">Utwórz nową aplikację konsolową **MigrationsAutomaticDemo**</span><span class="sxs-lookup"><span data-stu-id="37c3d-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="37c3d-115">Dodaj najnowszą wersję pakietu NuGet **EntityFramework** do projektu</span><span class="sxs-lookup"><span data-stu-id="37c3d-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="37c3d-116">**Tools — Menedżer pakietów&gt; Library —&gt; konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="37c3d-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="37c3d-117">Uruchom polecenie **install-package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="37c3d-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="37c3d-118">Dodaj plik **model.cs** z kodem pokazanym poniżej.</span><span class="sxs-lookup"><span data-stu-id="37c3d-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="37c3d-119">Ten kod definiuje jedną klasę **blogu** , która składa się z naszego modelu domeny i klasy **BlogContext** , która stanowi nasz kontekst Code First</span><span class="sxs-lookup"><span data-stu-id="37c3d-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="37c3d-120">Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="37c3d-121">Zaktualizuj plik **program.cs** za pomocą kodu pokazanego poniżej.</span><span class="sxs-lookup"><span data-stu-id="37c3d-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="37c3d-122">Uruchom aplikację i zobaczysz, że została utworzona baza danych **MigrationsAutomaticCodeDemo. BlogContext** .</span><span class="sxs-lookup"><span data-stu-id="37c3d-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![LocalDB bazy danych](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="37c3d-124">Włączanie migracji</span><span class="sxs-lookup"><span data-stu-id="37c3d-124">Enabling Migrations</span></span>

<span data-ttu-id="37c3d-125">Jest to czas na wprowadzenie kolejnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="37c3d-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="37c3d-126">Wprowadźmy Właściwość adresu URL do klasy bloga.</span><span class="sxs-lookup"><span data-stu-id="37c3d-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="37c3d-127">Jeśli chcesz uruchomić aplikację ponownie, otrzymasz InvalidOperationException z informacją o *tym, że model zapasowy kontekstu "BlogContext" został zmieniony od czasu utworzenia bazy danych. Aby zaktualizować bazę danych (http://go.microsoft.com/fwlink/?LinkId=238269), należy rozważyć użycie Migracje Code First* [](https://go.microsoft.com/fwlink/?LinkId=238269) *.*</span><span class="sxs-lookup"><span data-stu-id="37c3d-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="37c3d-128">Z powodu wyjątku sugerujemy czas rozpoczęcia korzystania z Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="37c3d-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="37c3d-129">Ze względu na to, że chcemy użyć automatycznych migracji, zamierzamy określić przełącznik **– EnableAutomaticMigrations** .</span><span class="sxs-lookup"><span data-stu-id="37c3d-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="37c3d-130">Uruchom polecenie **enable-migrations-EnableAutomaticMigrations** w konsoli Menedżera pakietów to polecenie dodało folder **migracji** do projektu.</span><span class="sxs-lookup"><span data-stu-id="37c3d-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="37c3d-131">Ten nowy folder zawiera jeden plik:</span><span class="sxs-lookup"><span data-stu-id="37c3d-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="37c3d-132">**Klasa konfiguracji.**</span><span class="sxs-lookup"><span data-stu-id="37c3d-132">**The Configuration class.**</span></span> <span data-ttu-id="37c3d-133">Ta klasa umożliwia skonfigurowanie sposobu zachowania migracji dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="37c3d-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="37c3d-134">W tym instruktażu zostanie użyta domyślna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="37c3d-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="37c3d-135">*Ponieważ istnieje tylko jeden Code First kontekst w projekcie, Enable-migrations automatycznie wypełnia typ kontekstu, do którego odnosi się ta konfiguracja.*</span><span class="sxs-lookup"><span data-stu-id="37c3d-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="37c3d-136">Twoja pierwsza automatyczna migracja</span><span class="sxs-lookup"><span data-stu-id="37c3d-136">Your First Automatic Migration</span></span>

<span data-ttu-id="37c3d-137">Migracje Code First ma dwa podstawowe polecenia, z którymi zamierzasz się zapoznać.</span><span class="sxs-lookup"><span data-stu-id="37c3d-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="37c3d-138">W ramach **migracji** do kolejnej migracji w oparciu o zmiany wprowadzone w modelu od momentu utworzenia ostatniej migracji</span><span class="sxs-lookup"><span data-stu-id="37c3d-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="37c3d-139">**Aktualizacja — baza danych** zastosuje wszystkie oczekujące migracje do bazy danych</span><span class="sxs-lookup"><span data-stu-id="37c3d-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="37c3d-140">Będziemy unikać używania dodatku do migracji (chyba że naprawdę to konieczne) i skupić się na umożliwieniu Migracje Code First automatycznego obliczania i stosowania zmian.</span><span class="sxs-lookup"><span data-stu-id="37c3d-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="37c3d-141">Użyjmy polecenia **Update-Database** , aby uzyskać migracje Code First w celu wypchnięcia zmian do naszego modelu (Nowa właściwość **bloga**l) do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="37c3d-142">Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="37c3d-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="37c3d-143">Baza danych **MigrationsAutomaticDemo. BlogContext** jest teraz aktualizowana w celu uwzględnienia kolumny **adresu URL** w tabeli **blogów** .</span><span class="sxs-lookup"><span data-stu-id="37c3d-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="37c3d-144">Twoja druga automatyczna migracja</span><span class="sxs-lookup"><span data-stu-id="37c3d-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="37c3d-145">Wprowadźmy kolejną zmianę i pozwól, Migracje Code First automatycznie wypychanie zmian do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="37c3d-146">Dodajmy również nową klasę **post**</span><span class="sxs-lookup"><span data-stu-id="37c3d-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="37c3d-147">Dodamy również kolekcję **ogłoszeń** do klasy **blog** , aby utworzyć drugi koniec relacji między **blogiem** i **wpisem**</span><span class="sxs-lookup"><span data-stu-id="37c3d-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="37c3d-148">Teraz korzystaj z bazy **danych Update-Database** , aby zapewnić aktualność bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="37c3d-149">Ten czas pozwala określić flagę **– verbose** , aby zobaczyć, że program SQL migracje Code First jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="37c3d-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="37c3d-150">Uruchom polecenie **Update-Database – verbose** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="37c3d-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="37c3d-151">Dodawanie migracji opartej na kodzie</span><span class="sxs-lookup"><span data-stu-id="37c3d-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="37c3d-152">Teraz przyjrzyjmy się coś, co będziemy potrzebować do użycia migracji opartej na kodzie dla programu.</span><span class="sxs-lookup"><span data-stu-id="37c3d-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="37c3d-153">Dodajmy do klasy **bloga** Właściwość **Rating**</span><span class="sxs-lookup"><span data-stu-id="37c3d-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="37c3d-154">Po prostu można uruchomić z **bazy danych Update-Database** , aby wypchnąć te zmiany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="37c3d-155">Jednak dodawana jest kolumna " **blogi. Rating** ", która nie dopuszcza wartości null, jeśli w tabeli znajdują się jakieś dane, do których zostanie przypisany domyślny typ danych dla nowej kolumny (Klasyfikacja jest liczbą całkowitą, tak aby była **równa 0**).</span><span class="sxs-lookup"><span data-stu-id="37c3d-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="37c3d-156">Ale chcemy określić wartość domyślną równą **3** , aby istniejące wiersze w tabeli **blogów** miały od znośnego klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="37c3d-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="37c3d-157">Użyjmy polecenia Add-Migration, aby napisać tę zmianę do migracji opartej na kodzie, aby można było ją edytować.</span><span class="sxs-lookup"><span data-stu-id="37c3d-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="37c3d-158">Polecenie **Add-Migration** umożliwia nam nadanie nazw migracji nazwy, przywołują nasze **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="37c3d-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="37c3d-159">Uruchom polecenie **Add-Migration AddBlogRating** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="37c3d-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="37c3d-160">W folderze **migracji** mamy teraz nową migrację **AddBlogRating** .</span><span class="sxs-lookup"><span data-stu-id="37c3d-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="37c3d-161">Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby pomóc w określeniu kolejności.</span><span class="sxs-lookup"><span data-stu-id="37c3d-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="37c3d-162">Zmodyfikujmy wygenerowany kod, aby określić wartość domyślną 3 dla blogu. Klasyfikacja (wiersz 10 w poniższym kodzie)</span><span class="sxs-lookup"><span data-stu-id="37c3d-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="37c3d-163">*Migracja ma także plik związany z kodem, który przechwytuje niektóre metadane. Te metadane umożliwią Migracje Code First replikowania automatycznych migracji wykonanych przed tą migracją opartą na kodzie. Jest to ważne, jeśli inny deweloper chce przeprowadzić migracje lub czas wdrożenia naszej aplikacji.*</span><span class="sxs-lookup"><span data-stu-id="37c3d-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="37c3d-164">Twoja edytowana migracja jest dobra, dlatego użyjemy polecenia **Update-Database** , aby zapewnić aktualność bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="37c3d-165">Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="37c3d-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="37c3d-166">Z powrotem do automatycznych migracji</span><span class="sxs-lookup"><span data-stu-id="37c3d-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="37c3d-167">Teraz możemy przełączyć się z powrotem do automatycznej migracji, aby uprościć zmiany.</span><span class="sxs-lookup"><span data-stu-id="37c3d-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="37c3d-168">Migracje Code First zadbają o przeprowadzenie migracji automatycznej i opartej na kodzie w odpowiedniej kolejności na podstawie metadanych przechowywanych w pliku związanym z kodem dla każdej migracji opartej na kodzie.</span><span class="sxs-lookup"><span data-stu-id="37c3d-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="37c3d-169">Dodajmy do naszego modelu Właściwość post. abstract</span><span class="sxs-lookup"><span data-stu-id="37c3d-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="37c3d-170">Teraz możemy użyć usługi **Update-Database** , aby uzyskać migracje Code First do wypchnięcia tej zmiany do bazy danych przy użyciu automatycznej migracji.</span><span class="sxs-lookup"><span data-stu-id="37c3d-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="37c3d-171">Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="37c3d-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="37c3d-172">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="37c3d-172">Summary</span></span>

<span data-ttu-id="37c3d-173">W tym instruktażu przedstawiono sposób korzystania z automatycznych migracji w celu wypychania zmian modelu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="37c3d-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="37c3d-174">Pokazano również, jak szkieletować i uruchamiać migracje oparte na kodzie między automatyczną migracją, gdy potrzebna jest większa kontrola.</span><span class="sxs-lookup"><span data-stu-id="37c3d-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>

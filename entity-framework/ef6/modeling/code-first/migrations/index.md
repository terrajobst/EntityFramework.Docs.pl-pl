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
# <a name="code-first-migrations"></a><span data-ttu-id="3768f-102">Migracje Code First</span><span class="sxs-lookup"><span data-stu-id="3768f-102">Code First Migrations</span></span>
<span data-ttu-id="3768f-103">Migracje Code First jest zalecanym sposobem, aby rozwijać schemat bazy danych aplikacji, jeśli jest używany przepływ pracy Code First.</span><span class="sxs-lookup"><span data-stu-id="3768f-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="3768f-104">Migracje udostępniają zestaw narzędzi umożliwiających:</span><span class="sxs-lookup"><span data-stu-id="3768f-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="3768f-105">Utwórz początkową bazę danych, która współpracuje z modelem EF</span><span class="sxs-lookup"><span data-stu-id="3768f-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="3768f-106">Generowanie migracji w celu śledzenia zmian wprowadzonych w modelu EF</span><span class="sxs-lookup"><span data-stu-id="3768f-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="3768f-107">Aktualizuj bazę danych o te zmiany</span><span class="sxs-lookup"><span data-stu-id="3768f-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="3768f-108">Poniższy przewodnik zawiera omówienie Migracje Code First w Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3768f-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="3768f-109">Możesz ukończyć cały przewodnik lub przejść do tematu, który Cię interesuje.</span><span class="sxs-lookup"><span data-stu-id="3768f-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="3768f-110">Omówione są następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="3768f-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="3768f-111">Kompilowanie wstępnego modelu & bazy danych</span><span class="sxs-lookup"><span data-stu-id="3768f-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="3768f-112">Przed rozpoczęciem korzystania z migracji potrzebujemy projektu i modelu Code First do pracy z programem.</span><span class="sxs-lookup"><span data-stu-id="3768f-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="3768f-113">W tym instruktażu będziemy używać **bloga** i modelu **post** .</span><span class="sxs-lookup"><span data-stu-id="3768f-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="3768f-114">Utwórz nową aplikację konsolową **MigrationsDemo**</span><span class="sxs-lookup"><span data-stu-id="3768f-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="3768f-115">Dodaj najnowszą wersję pakietu NuGet **EntityFramework** do projektu</span><span class="sxs-lookup"><span data-stu-id="3768f-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="3768f-116">**Tools — Menedżer pakietów&gt; Library —&gt; konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="3768f-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="3768f-117">Uruchom polecenie **install-package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="3768f-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="3768f-118">Dodaj plik **model.cs** z kodem pokazanym poniżej.</span><span class="sxs-lookup"><span data-stu-id="3768f-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="3768f-119">Ten kod definiuje jedną klasę **blogu** , która składa się z naszego modelu domeny i klasy **BlogContext** , która stanowi nasz kontekst Code First</span><span class="sxs-lookup"><span data-stu-id="3768f-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="3768f-120">Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="3768f-121">Zaktualizuj plik **program.cs** za pomocą kodu pokazanego poniżej.</span><span class="sxs-lookup"><span data-stu-id="3768f-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="3768f-122">Uruchom aplikację i zobaczysz, że została utworzona baza danych **MigrationsCodeDemo. BlogContext** .</span><span class="sxs-lookup"><span data-stu-id="3768f-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![LocalDB bazy danych](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="3768f-124">Włączanie migracji</span><span class="sxs-lookup"><span data-stu-id="3768f-124">Enabling Migrations</span></span>

<span data-ttu-id="3768f-125">Jest to czas na wprowadzenie kolejnych zmian w modelu.</span><span class="sxs-lookup"><span data-stu-id="3768f-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="3768f-126">Wprowadźmy Właściwość adresu URL do klasy bloga.</span><span class="sxs-lookup"><span data-stu-id="3768f-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="3768f-127">Jeśli chcesz uruchomić aplikację ponownie, otrzymasz InvalidOperationException z informacją o *tym, że model zapasowy kontekstu "BlogContext" został zmieniony od czasu utworzenia bazy danych. Aby zaktualizować bazę danych (http://go.microsoft.com/fwlink/?LinkId=238269), należy rozważyć użycie Migracje Code First* [](https://go.microsoft.com/fwlink/?LinkId=238269) *.*</span><span class="sxs-lookup"><span data-stu-id="3768f-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="3768f-128">Z powodu wyjątku sugerujemy czas rozpoczęcia korzystania z Migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="3768f-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="3768f-129">Pierwszym krokiem jest włączenie migracji w naszym kontekście.</span><span class="sxs-lookup"><span data-stu-id="3768f-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="3768f-130">Uruchamianie polecenia **enable-migrations** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="3768f-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="3768f-131">To polecenie dodało folder **migracji** do naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="3768f-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="3768f-132">Ten nowy folder zawiera dwa pliki:</span><span class="sxs-lookup"><span data-stu-id="3768f-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="3768f-133">**Klasa konfiguracji.**</span><span class="sxs-lookup"><span data-stu-id="3768f-133">**The Configuration class.**</span></span> <span data-ttu-id="3768f-134">Ta klasa umożliwia skonfigurowanie sposobu zachowania migracji dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3768f-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="3768f-135">W tym instruktażu zostanie użyta domyślna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="3768f-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="3768f-136">*Ponieważ istnieje tylko jeden Code First kontekst w projekcie, Enable-migrations automatycznie wypełnia typ kontekstu, do którego odnosi się ta konfiguracja.*</span><span class="sxs-lookup"><span data-stu-id="3768f-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="3768f-137">**Migracja InitialCreate**.</span><span class="sxs-lookup"><span data-stu-id="3768f-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="3768f-138">Ta migracja została wygenerowana, ponieważ Code First utworzyć bazę danych dla nas przed włączeniem migracji.</span><span class="sxs-lookup"><span data-stu-id="3768f-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="3768f-139">Kod w tej migracji szkieletowej reprezentuje obiekty, które zostały już utworzone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="3768f-140">W naszym przypadku jest to tabela **blogów** z **BlogId** i kolumnami **nazw** .</span><span class="sxs-lookup"><span data-stu-id="3768f-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="3768f-141">Nazwa pliku zawiera sygnaturę czasową ułatwiającą porządkowanie.</span><span class="sxs-lookup"><span data-stu-id="3768f-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="3768f-142">*Jeśli baza danych nie została jeszcze utworzona, migracja InitialCreate nie została dodana do projektu. Zamiast tego podczas pierwszego wywołania metody Add-Migration kod służący do tworzenia tych tabel będzie szkieletem do nowej migracji.*</span><span class="sxs-lookup"><span data-stu-id="3768f-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="3768f-143">Wiele modeli przeznaczonych dla tej samej bazy danych</span><span class="sxs-lookup"><span data-stu-id="3768f-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="3768f-144">W przypadku korzystania z wersji wcześniejszych niż EF6 tylko jeden model Code First może być używany do generowania schematu bazy danych i zarządzania nią.</span><span class="sxs-lookup"><span data-stu-id="3768f-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="3768f-145">Jest to wynik pojedynczej **\_\_tabeli MigrationsHistory** na bazę danych bez możliwości zidentyfikowania wpisów należących do tego modelu.</span><span class="sxs-lookup"><span data-stu-id="3768f-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="3768f-146">Począwszy od EF6, Klasa **konfiguracji** zawiera właściwość **ContextKey** .</span><span class="sxs-lookup"><span data-stu-id="3768f-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="3768f-147">Działa jako unikatowy identyfikator dla każdego modelu Code First.</span><span class="sxs-lookup"><span data-stu-id="3768f-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="3768f-148">Odpowiadająca kolumna w **\_\_tabeli MigrationsHistory** umożliwia zapisów z wielu modeli w celu udostępnienia tabeli.</span><span class="sxs-lookup"><span data-stu-id="3768f-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="3768f-149">Domyślnie ta właściwość ma ustawioną w pełni kwalifikowaną nazwę kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3768f-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="3768f-150">Generowanie & wykonywania migracji</span><span class="sxs-lookup"><span data-stu-id="3768f-150">Generating & Running Migrations</span></span>

<span data-ttu-id="3768f-151">Migracje Code First ma dwa podstawowe polecenia, z którymi zamierzasz się zapoznać.</span><span class="sxs-lookup"><span data-stu-id="3768f-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="3768f-152">W ramach **migracji** do kolejnej migracji w oparciu o zmiany wprowadzone w modelu od momentu utworzenia ostatniej migracji</span><span class="sxs-lookup"><span data-stu-id="3768f-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="3768f-153">**Aktualizacja — baza danych** zastosuje wszystkie oczekujące migracje do bazy danych</span><span class="sxs-lookup"><span data-stu-id="3768f-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="3768f-154">Musimy utworzyć szkielet migracji, aby zachować ostrożność podczas dodawania nowej właściwości adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3768f-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="3768f-155">Polecenie **Add-Migration** umożliwia nam nadanie nazw migracji nazwy, przywołują nasze **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="3768f-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="3768f-156">Uruchamianie polecenia **Add-Migration AddBlogUrl** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="3768f-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="3768f-157">W folderze **migracji** mamy teraz nową migrację **AddBlogUrl** .</span><span class="sxs-lookup"><span data-stu-id="3768f-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="3768f-158">Nazwa pliku migracji jest wstępnie naprawiona sygnaturą czasową, aby ułatwić porządkowanie</span><span class="sxs-lookup"><span data-stu-id="3768f-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="3768f-159">Możemy teraz edytować lub dodawać do tej migracji, ale wszystko wygląda dość dobrze.</span><span class="sxs-lookup"><span data-stu-id="3768f-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="3768f-160">Aby zastosować tę migrację do bazy danych, użyjmy do bazy **danych Update-Database** .</span><span class="sxs-lookup"><span data-stu-id="3768f-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="3768f-161">Uruchamianie polecenia **Update-Database** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="3768f-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="3768f-162">Migracje Code First będzie porównać migracje w naszym folderze **migracji** z tymi, które zostały zastosowane do bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="3768f-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="3768f-163">Zobaczy on, że należy zastosować migrację **AddBlogUrl** i uruchomić ją.</span><span class="sxs-lookup"><span data-stu-id="3768f-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="3768f-164">Baza danych **MigrationsDemo. BlogContext** jest teraz aktualizowana w celu uwzględnienia kolumny **adresu URL** w tabeli **blogów** .</span><span class="sxs-lookup"><span data-stu-id="3768f-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="3768f-165">Dostosowywanie migracji</span><span class="sxs-lookup"><span data-stu-id="3768f-165">Customizing Migrations</span></span>

<span data-ttu-id="3768f-166">Do tej pory wygenerowałeś i przeprowadzimy migrację bez wprowadzania żadnych zmian.</span><span class="sxs-lookup"><span data-stu-id="3768f-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="3768f-167">Teraz przyjrzyjmy się edycji kodu, który jest generowany domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3768f-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="3768f-168">Aby wprowadzić więcej zmian w modelu, dodajmy nową właściwość **rankingu** do klasy **bloga**</span><span class="sxs-lookup"><span data-stu-id="3768f-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="3768f-169">Dodajmy również nową klasę **post**</span><span class="sxs-lookup"><span data-stu-id="3768f-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="3768f-170">Dodamy również kolekcję **ogłoszeń** do klasy **blog** , aby utworzyć drugi koniec relacji między **blogiem** i **wpisem**</span><span class="sxs-lookup"><span data-stu-id="3768f-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="3768f-171">Użyjemy polecenia **Add-Migration** , aby zapewnić, że migracje Code First szkieletem na potrzeby migracji do nas.</span><span class="sxs-lookup"><span data-stu-id="3768f-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="3768f-172">Będziemy wywoływać to **AddPostClass**migracji.</span><span class="sxs-lookup"><span data-stu-id="3768f-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="3768f-173">Uruchom polecenie **Add-Migration AddPostClass** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="3768f-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="3768f-174">Migracje Code First było bardzo dobre zadanie tworzenia szkieletu tych zmian, ale istnieje kilka rzeczy, które warto zmienić:</span><span class="sxs-lookup"><span data-stu-id="3768f-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="3768f-175">Najpierw Dodaj unikatowy indeks do **wpisów. kolumna tytułu** (dodanie w wierszu 22 & 29 w poniższym kodzie).</span><span class="sxs-lookup"><span data-stu-id="3768f-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="3768f-176">Dodawana jest również kolumna " **Blogi** " z nie dopuszczaniem wartości null.</span><span class="sxs-lookup"><span data-stu-id="3768f-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="3768f-177">Jeśli w tabeli znajdują się jakieś dane, zostanie do niego przypisana domyślna wartość CLR typu danych dla nowej kolumny (Klasyfikacja jest liczbą całkowitą, tak aby była **równa 0**).</span><span class="sxs-lookup"><span data-stu-id="3768f-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="3768f-178">Ale chcemy określić wartość domyślną równą **3** , aby istniejące wiersze w tabeli **blogów** miały od znośnego klasyfikację.</span><span class="sxs-lookup"><span data-stu-id="3768f-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="3768f-179">(Można zobaczyć wartość domyślną określoną w wierszu 24 poniższego kodu)</span><span class="sxs-lookup"><span data-stu-id="3768f-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="3768f-180">Twoja edytowana migracja jest gotowa do użycia, więc użyjemy **aktualizacji bazy danych** , aby zapewnić aktualność bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="3768f-181">Ten czas pozwala określić flagę **– verbose** , aby zobaczyć, że program SQL migracje Code First jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="3768f-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="3768f-182">Uruchom polecenie **Update-Database – verbose** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="3768f-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="3768f-183">Ruch danych/niestandardowe SQL</span><span class="sxs-lookup"><span data-stu-id="3768f-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="3768f-184">Do tej pory oglądamy operacje migracji, które nie zmieniają ani nie przechodzą żadnych danych, teraz przyjrzyjmy się coś, co będzie wymagało przeniesienia pewnych danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="3768f-185">Nie ma jeszcze natywnej obsługi ruchu danych, ale można uruchamiać w dowolnym momencie dowolne polecenia SQL.</span><span class="sxs-lookup"><span data-stu-id="3768f-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="3768f-186">Dodajmy do naszego modelu Właściwość **post. abstract** .</span><span class="sxs-lookup"><span data-stu-id="3768f-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="3768f-187">Później będziemy wstępnie wypełniać **streszczenie** istniejących wpisów przy użyciu pewnego tekstu od początku kolumny **Content (zawartość** ).</span><span class="sxs-lookup"><span data-stu-id="3768f-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="3768f-188">Użyjemy polecenia **Add-Migration** , aby zapewnić, że migracje Code First szkieletem na potrzeby migracji do nas.</span><span class="sxs-lookup"><span data-stu-id="3768f-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="3768f-189">Uruchom polecenie **Add-Migration AddPostAbstract** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="3768f-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="3768f-190">Wygenerowane migracje uwzględniają zmiany schematu, ale chcemy również wstępnie wypełnić kolumnę **abstrakcyjną** przy użyciu pierwszych 100 znaków zawartości dla każdego wpisu.</span><span class="sxs-lookup"><span data-stu-id="3768f-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="3768f-191">Możemy to zrobić przez upuszczenie na SQL i uruchomienie instrukcji **Update** po dodaniu kolumny.</span><span class="sxs-lookup"><span data-stu-id="3768f-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="3768f-192">(Dodawanie w wierszu 12 w poniższym kodzie)</span><span class="sxs-lookup"><span data-stu-id="3768f-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="3768f-193">Twoja edytowana migracja jest dobra, dlatego użyjemy polecenia **Update-Database** , aby zapewnić aktualność bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="3768f-194">Określimy flagę **– verbose** , aby można było zobaczyć uruchamianie kodu SQL w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="3768f-195">Uruchom polecenie **Update-Database – verbose** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="3768f-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="3768f-196">Migrowanie do określonej wersji (w tym obniżanie poziomu)</span><span class="sxs-lookup"><span data-stu-id="3768f-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="3768f-197">Do tej pory zawsze uaktualniono do najnowszej migracji, ale mogą wystąpić sytuacje, w których uaktualnienie/obniżenie poziomu zostanie przeprowadzone w określonej migracji.</span><span class="sxs-lookup"><span data-stu-id="3768f-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="3768f-198">Załóżmy, że chcemy przeprowadzić migrację bazy danych do stanu, w którym znajdowała się po zakończeniu migracji **AddBlogUrl** .</span><span class="sxs-lookup"><span data-stu-id="3768f-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="3768f-199">Do obniżenia poziomu migracji można użyć przełącznika **– TargetMigration** .</span><span class="sxs-lookup"><span data-stu-id="3768f-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="3768f-200">Uruchom polecenie **Update-Database – TargetMigration: AddBlogUrl** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="3768f-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="3768f-201">To polecenie spowoduje uruchomienie skryptu w dół dla naszych **AddBlogAbstract** i migracji **AddPostClass** .</span><span class="sxs-lookup"><span data-stu-id="3768f-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="3768f-202">Jeśli chcesz przywrócić cały sposób do pustej bazy danych, możesz użyć polecenia **Update-Database – TargetMigration: $InitialDatabase** .</span><span class="sxs-lookup"><span data-stu-id="3768f-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="3768f-203">Pobieranie skryptu SQL</span><span class="sxs-lookup"><span data-stu-id="3768f-203">Getting a SQL Script</span></span>

<span data-ttu-id="3768f-204">Jeśli inny deweloper chce zmienić te zmiany na komputerze, może po prostu przeprowadzić synchronizację, gdy sprawdzimy nasze zmiany w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="3768f-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="3768f-205">Po otrzymaniu nowych migracji można po prostu uruchomić polecenie Update-Database, aby zmiany zostały zastosowane lokalnie.</span><span class="sxs-lookup"><span data-stu-id="3768f-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="3768f-206">Jeśli jednak chcemy wypchnąć te zmiany do serwera testowego i ostatecznie produkcji, prawdopodobnie chcemy, aby skrypt SQL mógł zostać przekazany do naszego DBA.</span><span class="sxs-lookup"><span data-stu-id="3768f-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="3768f-207">Uruchom polecenie **Update-Database** , ale ten czas należy określić flagę **– Script** , tak aby zmiany były zapisywane do skryptu, a nie do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="3768f-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="3768f-208">Określimy również migrację źródłową i docelową w celu wygenerowania skryptu.</span><span class="sxs-lookup"><span data-stu-id="3768f-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="3768f-209">Chcemy, aby skrypt przeszedł z pustej bazy danych ( **$InitialDatabase**) do najnowszej wersji ( **AddPostAbstract**migracji).</span><span class="sxs-lookup"><span data-stu-id="3768f-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="3768f-210">*Jeśli nie określisz docelowej migracji, migracja będzie używać najnowszej migracji jako docelowej. Jeśli nie określisz migracji źródła, migracja będzie używać bieżącego stanu bazy danych.*</span><span class="sxs-lookup"><span data-stu-id="3768f-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="3768f-211">Uruchom polecenie **Update-Database-Script-SourceMigration: $InitialDatabase-TargetMigration: AddPostAbstract** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="3768f-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="3768f-212">Migracje Code First uruchomi potok migracji, ale zamiast tego rzeczywiście zastosuje zmiany, zapisze je do pliku SQL.</span><span class="sxs-lookup"><span data-stu-id="3768f-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="3768f-213">Po wygenerowaniu skryptu zostanie on otwarty w programie Visual Studio, gotowy do wyświetlenia lub zapisania.</span><span class="sxs-lookup"><span data-stu-id="3768f-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="3768f-214">Generowanie skryptów idempotentne</span><span class="sxs-lookup"><span data-stu-id="3768f-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="3768f-215">Rozpoczynając od EF6, jeśli określono wartość **– SourceMigration $InitialDatabase** , wygenerowany skrypt będzie "idempotentne".</span><span class="sxs-lookup"><span data-stu-id="3768f-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="3768f-216">Skrypty idempotentne mogą uaktualnić bazę danych obecnie w dowolnej wersji do najnowszej wersji (lub określonej wersji, jeśli używasz programu **– TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="3768f-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="3768f-217">Wygenerowany skrypt zawiera logikę, aby sprawdzić **\_\_tabeli MigrationsHistory** i zastosować tylko zmiany, które nie zostały wcześniej zastosowane.</span><span class="sxs-lookup"><span data-stu-id="3768f-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="3768f-218">Automatyczne uaktualnianie przy uruchamianiu aplikacji (inicjator MigrateDatabaseToLatestVersion)</span><span class="sxs-lookup"><span data-stu-id="3768f-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="3768f-219">W przypadku wdrażania aplikacji może być konieczne automatyczne uaktualnienie bazy danych (przez zastosowanie wszelkich oczekujących migracji) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3768f-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="3768f-220">Można to zrobić przez zarejestrowanie inicjatora bazy danych **MigrateDatabaseToLatestVersion** .</span><span class="sxs-lookup"><span data-stu-id="3768f-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="3768f-221">Inicjator bazy danych po prostu zawiera logikę, która jest używana do poprawnego skonfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3768f-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="3768f-222">Ta logika jest uruchamiana przy pierwszym użyciu kontekstu w procesie aplikacji (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="3768f-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="3768f-223">Można zaktualizować plik **program.cs** , jak pokazano poniżej, aby ustawić inicjator **MigrateDatabaseToLatestVersion** dla BlogContext przed użyciem kontekstu (wiersz 14).</span><span class="sxs-lookup"><span data-stu-id="3768f-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="3768f-224">Należy pamiętać, że należy również dodać instrukcję using dla przestrzeni nazw **System. Data. Entity** (wiersz 5).</span><span class="sxs-lookup"><span data-stu-id="3768f-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="3768f-225">*Gdy utworzymy wystąpienie tego inicjatora, musimy określić typ kontekstu (**BlogContext**) i konfigurację migracji (**konfigurację**) — Konfiguracja migracji jest klasą dodaną do folderu **migracji** po włączeniu migracji.*</span><span class="sxs-lookup"><span data-stu-id="3768f-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="3768f-226">Teraz za każdym razem, gdy aplikacja jest uruchamiana, najpierw sprawdza, czy baza danych, do której jest przeznaczona, jest aktualna, i zastosuje oczekujące migracje, jeśli nie jest.</span><span class="sxs-lookup"><span data-stu-id="3768f-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>

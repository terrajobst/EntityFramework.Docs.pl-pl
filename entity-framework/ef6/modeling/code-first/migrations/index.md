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
# <a name="code-first-migrations"></a><span data-ttu-id="43655-102">Migracje kodu</span><span class="sxs-lookup"><span data-stu-id="43655-102">Code First Migrations</span></span>
<span data-ttu-id="43655-103">Migracje code first to zalecany sposób rozwijania schematu bazy danych aplikacji, jeśli używasz przepływu pracy Code First.</span><span class="sxs-lookup"><span data-stu-id="43655-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="43655-104">Migracje zapewniają zestaw narzędzi, które umożliwiają:</span><span class="sxs-lookup"><span data-stu-id="43655-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="43655-105">Tworzenie początkowej bazy danych, która współpracuje z modelem EF</span><span class="sxs-lookup"><span data-stu-id="43655-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="43655-106">Generowanie migracji w celu śledzenia zmian wprowadzać w modelu EF</span><span class="sxs-lookup"><span data-stu-id="43655-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="43655-107">Dbaj o aktualną bazę danych dzięki tym zmianom</span><span class="sxs-lookup"><span data-stu-id="43655-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="43655-108">Poniższy instruktaż zapewni omówienie migracje code first w ramach encji.</span><span class="sxs-lookup"><span data-stu-id="43655-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="43655-109">Możesz ukończyć cały instruktaż lub przejść do interesującego Cię tematu.</span><span class="sxs-lookup"><span data-stu-id="43655-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="43655-110">Poruszane są następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="43655-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="43655-111">Tworzenie początkowej bazy danych & modelu</span><span class="sxs-lookup"><span data-stu-id="43655-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="43655-112">Zanim zaczniemy używać migracji, potrzebujemy projektu i modelu Code First do pracy.</span><span class="sxs-lookup"><span data-stu-id="43655-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="43655-113">W tym instruktażu będziemy używać kanonicznego modelu **bloga** i **postu.**</span><span class="sxs-lookup"><span data-stu-id="43655-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="43655-114">Tworzenie nowej aplikacji **MigrationsDemo** Console</span><span class="sxs-lookup"><span data-stu-id="43655-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="43655-115">Dodawanie najnowszej wersji pakietu **NuGet programu EntityFramework** do projektu</span><span class="sxs-lookup"><span data-stu-id="43655-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="43655-116">**Narzędzia&gt; – Menedżer&gt; pakietów biblioteki – Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="43655-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="43655-117">Uruchamianie polecenia **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="43655-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="43655-118">Dodaj plik **Model.cs** z kodem pokazanym poniżej.</span><span class="sxs-lookup"><span data-stu-id="43655-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="43655-119">Ten kod definiuje jedną klasę **bloga,** która składa się z naszego modelu domeny i klasę **BlogContext,** która jest naszym kontekstem EF Code First</span><span class="sxs-lookup"><span data-stu-id="43655-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="43655-120">Teraz, gdy mamy model nadszedł czas, aby użyć go do wykonywania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="43655-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="43655-121">Zaktualizuj plik **Program.cs** kodem pokazanym poniżej.</span><span class="sxs-lookup"><span data-stu-id="43655-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="43655-122">Uruchom aplikację, a zobaczysz, że **migrationsCodeDemo.BlogContext** bazy danych jest tworzony dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="43655-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![Baza danych LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="43655-124">Włączanie migracji</span><span class="sxs-lookup"><span data-stu-id="43655-124">Enabling Migrations</span></span>

<span data-ttu-id="43655-125">Nadszedł czas, aby wprowadzić kilka zmian w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="43655-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="43655-126">Wprowadźmy url właściwości do bloga klasy.</span><span class="sxs-lookup"><span data-stu-id="43655-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="43655-127">Jeśli aplikacja miałaby zostać ponownie uruchomiony, otrzymasz InvalidOperationException stwierdzając, że *model kopii kontekstu "BlogContext" uległ zmianie od czasu utworzenia bazy danych. Rozważ użycie migracji code first do zaktualizowania bazy danych (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="43655-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="43655-128">Jak sugeruje wyjątek, nadszedł czas, aby rozpocząć korzystanie z migracji code first.</span><span class="sxs-lookup"><span data-stu-id="43655-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="43655-129">Pierwszym krokiem jest włączenie migracji dla naszego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="43655-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="43655-130">Uruchamianie polecenia **Włącz migracje** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="43655-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="43655-131">To polecenie dodało folder **Migracje** do naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="43655-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="43655-132">Ten nowy folder zawiera dwa pliki:</span><span class="sxs-lookup"><span data-stu-id="43655-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="43655-133">**Klasa konfiguracji.**</span><span class="sxs-lookup"><span data-stu-id="43655-133">**The Configuration class.**</span></span> <span data-ttu-id="43655-134">Ta klasa umożliwia skonfigurowanie zachowania migracji dla kontekstu.</span><span class="sxs-lookup"><span data-stu-id="43655-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="43655-135">W tym instruktażu po prostu użyjemy domyślnej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43655-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="43655-136">*Ponieważ istnieje tylko jeden kontekst Code First w projekcie, Enable-Migrations automatycznie wypełnił typ kontekstu, do który dotyczy ta konfiguracja.*</span><span class="sxs-lookup"><span data-stu-id="43655-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="43655-137">**Początkowa migracjatworzenia**.</span><span class="sxs-lookup"><span data-stu-id="43655-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="43655-138">Ta migracja została wygenerowana, ponieważ mieliśmy już Code First utworzyć bazę danych dla nas, zanim włączono migracje.</span><span class="sxs-lookup"><span data-stu-id="43655-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="43655-139">Kod w tej migracji szkieletu reprezentuje obiekty, które zostały już utworzone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43655-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="43655-140">W naszym przypadku jest to tabela **Blog** z **blogid** i **nazwa** kolumny.</span><span class="sxs-lookup"><span data-stu-id="43655-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="43655-141">Nazwa pliku zawiera sygnaturę czasową ułatwiającą zamawianie.</span><span class="sxs-lookup"><span data-stu-id="43655-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="43655-142">*Jeśli baza danych nie została jeszcze utworzona, migracja InitialCreate nie zostałaby dodana do projektu. Zamiast tego przy pierwszym wywołaniu Add-Migration kod do tworzenia tych tabel będzie szkieletu do nowej migracji.*</span><span class="sxs-lookup"><span data-stu-id="43655-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="43655-143">Wiele modeli kierowanych na tę samą bazę danych</span><span class="sxs-lookup"><span data-stu-id="43655-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="43655-144">Podczas korzystania z wersji wcześniejszych ef6, tylko jeden model Code First może służyć do generowania/zarządzania schematem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43655-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="43655-145">Jest to wynik pojedynczej \*\* \_ \_MigrationsHistory\*\* tabeli na bazę danych bez możliwości określenia, które wpisy należą do którego modelu.</span><span class="sxs-lookup"><span data-stu-id="43655-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="43655-146">Począwszy od **EF6, Configuration** klasa zawiera **ContextKey** właściwości.</span><span class="sxs-lookup"><span data-stu-id="43655-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="43655-147">Działa to jako unikatowy identyfikator dla każdego modelu Code First.</span><span class="sxs-lookup"><span data-stu-id="43655-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="43655-148">Odpowiednia kolumna \*\* \_ \_w MigrationsHistory\*\* tabeli umożliwia wpisy z wielu modeli do udziału w tabeli.</span><span class="sxs-lookup"><span data-stu-id="43655-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="43655-149">Domyślnie ta właściwość jest ustawiona na w pełni kwalifikowaną nazwę kontekstu.</span><span class="sxs-lookup"><span data-stu-id="43655-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="43655-150">Generowanie & uruchomionych migracji</span><span class="sxs-lookup"><span data-stu-id="43655-150">Generating & Running Migrations</span></span>

<span data-ttu-id="43655-151">Migracje code first ma dwa polecenia podstawowe, które zostaną zaznajomieni z.</span><span class="sxs-lookup"><span data-stu-id="43655-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="43655-152">**Add-Migration** spowoduje utworzenie następnej migracji na podstawie zmian wprowadzonych w modelu od czasu utworzenia ostatniej migracji</span><span class="sxs-lookup"><span data-stu-id="43655-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="43655-153">**Update-Database** zastosuje wszelkie oczekujące migracje do bazy danych</span><span class="sxs-lookup"><span data-stu-id="43655-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="43655-154">Musimy uszłać migrację, aby zadbać o nową właściwość Url, którą dodaliśmy.</span><span class="sxs-lookup"><span data-stu-id="43655-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="43655-155">Polecenie **Add-Migration** pozwala nam nadać tym migracji nazwę, po prostu zadzwońmy do naszego **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="43655-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="43655-156">Uruchamianie polecenia **Add-Migration AddBlogUrl** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="43655-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="43655-157">W folderze **Migracje** mamy teraz nową migrację **AddBlogUrl.**</span><span class="sxs-lookup"><span data-stu-id="43655-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="43655-158">Nazwa pliku migracji jest wstępnie ustalona za pomocą sygnatury czasowej, aby ułatwić zamawianie</span><span class="sxs-lookup"><span data-stu-id="43655-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="43655-159">Możemy teraz edytować lub dodać do tej migracji, ale wszystko wygląda całkiem nieźle.</span><span class="sxs-lookup"><span data-stu-id="43655-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="43655-160">Użyjmy **update-database,** aby zastosować tę migrację do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43655-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="43655-161">Uruchamianie polecenia **Update-Database** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="43655-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="43655-162">Migracje code first będą porównywać migracje w naszym folderze Migracje z **migracjami,** które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43655-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="43655-163">Zobaczy, że należy zastosować migrację **AddBlogUrl** i uruchomić ją.</span><span class="sxs-lookup"><span data-stu-id="43655-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="43655-164">Baza danych **MigrationsDemo.BlogContext** jest teraz aktualizowana w celu uwzględnienia kolumny **Url** w tabeli **Blogi.**</span><span class="sxs-lookup"><span data-stu-id="43655-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="43655-165">Dostosowywanie migracji</span><span class="sxs-lookup"><span data-stu-id="43655-165">Customizing Migrations</span></span>

<span data-ttu-id="43655-166">Do tej pory wygenerowaliśmy i uruchomiliśmy migrację bez wprowadzania żadnych zmian.</span><span class="sxs-lookup"><span data-stu-id="43655-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="43655-167">Teraz przyjrzyjmy się edycji kodu, który jest generowany domyślnie.</span><span class="sxs-lookup"><span data-stu-id="43655-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="43655-168">Nadszedł czas, aby wprowadzić kilka zmian w naszym modelu, dodajmy nową właściwość **Ocena** do klasy **Blog**</span><span class="sxs-lookup"><span data-stu-id="43655-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="43655-169">Dodajmy również nową klasę **Post**</span><span class="sxs-lookup"><span data-stu-id="43655-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="43655-170">Dodamy również kolekcję **postów** do klasy **Blog,** aby utworzyć drugi koniec relacji między **blogiem** a **postem**</span><span class="sxs-lookup"><span data-stu-id="43655-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="43655-171">Użyjemy polecenia **Add-Migration,** aby umożliwić kod pierwsze migracje szkieletu jego najlepsze przypuszczenie na migracji dla nas.</span><span class="sxs-lookup"><span data-stu-id="43655-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="43655-172">Będziemy nazywać tę migrację **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="43655-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="43655-173">Uruchom polecenie **Add-Migration AddPostClass** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="43655-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="43655-174">Code First Migrations zrobił całkiem dobrą robotę rusztowania tych zmian, ale są pewne rzeczy, które możemy chcieć zmienić:</span><span class="sxs-lookup"><span data-stu-id="43655-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="43655-175">Najpierw dodajmy unikatowy indeks do kolumny **Posts.Title** (Dodawanie w wierszu 22 & 29 w poniższym kodzie).</span><span class="sxs-lookup"><span data-stu-id="43655-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="43655-176">Dodajemy również kolumnę **Blogs.Rating,** która nie może być nullowa.</span><span class="sxs-lookup"><span data-stu-id="43655-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="43655-177">Jeśli w tabeli są jakieś istniejące dane, zostanie przypisana domyślna wartość CLR typu danych dla nowej kolumny (Ocena jest liczą całkowitą, więc będzie to **0**).</span><span class="sxs-lookup"><span data-stu-id="43655-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="43655-178">Ale chcemy określić domyślną wartość **3,** tak aby istniejące wiersze w tabeli **Blogi** zaczęły się od przyzwoitej oceny.</span><span class="sxs-lookup"><span data-stu-id="43655-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="43655-179">(Możesz zobaczyć domyślną wartość określoną w wierszu 24 poniższego kodu)</span><span class="sxs-lookup"><span data-stu-id="43655-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="43655-180">Nasza edytowana migracja jest gotowa do użycia, więc użyjmy **update-database,** aby zaktualizować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43655-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="43655-181">Tym razem określmy flagę **—Verbose,** aby można było zobaczyć sql, że migracje code first jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="43655-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="43655-182">Uruchom polecenie **Update-Database —verbose** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="43655-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="43655-183">Ruch danych / Niestandardowy SQL</span><span class="sxs-lookup"><span data-stu-id="43655-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="43655-184">Do tej pory przyjrzeliśmy się operacjom migracji, które nie zmieniają ani nie przenoszą żadnych danych, teraz przyjrzyjmy się czemuś, co musi przenieść niektóre dane.</span><span class="sxs-lookup"><span data-stu-id="43655-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="43655-185">Nie ma jeszcze natywnej obsługi ruchu danych, ale możemy uruchomić niektóre dowolne polecenia SQL w dowolnym momencie naszego skryptu.</span><span class="sxs-lookup"><span data-stu-id="43655-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="43655-186">Dodajmy **Właściwość Post.Abstract** do naszego modelu.</span><span class="sxs-lookup"><span data-stu-id="43655-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="43655-187">Później wstępnie wypełnimy **streszczenie** dla istniejących wpisów przy użyciu tekstu z początku kolumny **Zawartość.**</span><span class="sxs-lookup"><span data-stu-id="43655-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="43655-188">Użyjemy polecenia **Add-Migration,** aby umożliwić kod pierwsze migracje szkieletu jego najlepsze przypuszczenie na migracji dla nas.</span><span class="sxs-lookup"><span data-stu-id="43655-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="43655-189">Uruchom polecenie **Add-Migration AddPostAbstract** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="43655-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="43655-190">Wygenerowana migracja zajmuje się zmianami schematu, ale chcemy również wstępnie wypełnić **kolumnę Streszczenie** przy użyciu pierwszych 100 znaków zawartości dla każdego wpisu.</span><span class="sxs-lookup"><span data-stu-id="43655-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="43655-191">Możemy to zrobić, upuszczając do SQL i uruchamiając **update** instrukcji po dodaniu kolumny.</span><span class="sxs-lookup"><span data-stu-id="43655-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="43655-192">(Dodanie w wierszu 12 w kodzie poniżej)</span><span class="sxs-lookup"><span data-stu-id="43655-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="43655-193">Nasza edytowana migracja wygląda dobrze, więc użyjmy **update-database,** aby zaktualizować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="43655-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="43655-194">Określimy **flagę —Verbose,** aby śmy mogli zobaczyć, że SQL jest uruchamiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="43655-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="43655-195">Uruchom polecenie **Update-Database —verbose** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="43655-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="43655-196">Migrowanie do określonej wersji (w tym na starszą wersję)</span><span class="sxs-lookup"><span data-stu-id="43655-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="43655-197">Do tej pory zawsze uaktualniliśmy do najnowszej migracji, ale mogą być chwile, kiedy chcesz uaktualnić / obniżyć do określonej migracji.</span><span class="sxs-lookup"><span data-stu-id="43655-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="43655-198">Załóżmy, że chcemy przeprowadzić migrację naszej bazy danych do stanu, w jaki był po uruchomieniu naszej migracji **AddBlogUrl.**</span><span class="sxs-lookup"><span data-stu-id="43655-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="43655-199">Możemy użyć **przełącznika —TargetMigration,** aby przejść na starszą wersję do tej migracji.</span><span class="sxs-lookup"><span data-stu-id="43655-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="43655-200">Uruchom polecenie **Update-Database –TargetMigration: AddBlogUrl** w konsoli Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="43655-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="43655-201">To polecenie uruchomi skrypt Down dla naszych migracji **AddBlogAbstract** i **AddPostClass.**</span><span class="sxs-lookup"><span data-stu-id="43655-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="43655-202">Jeśli chcesz toczyć całą drogę z powrotem do pustej bazy danych, a następnie można użyć **Update-Database –TargetMigration: $InitialDatabase** polecenia.</span><span class="sxs-lookup"><span data-stu-id="43655-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="43655-203">Uzyskiwanie skryptu SQL</span><span class="sxs-lookup"><span data-stu-id="43655-203">Getting a SQL Script</span></span>

<span data-ttu-id="43655-204">Jeśli inny deweloper chce tych zmian na swoim komputerze, może po prostu zsynchronizować, gdy sprawdzimy nasze zmiany w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="43655-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="43655-205">Po uzyskaniu nowych migracji mogą po prostu uruchomić polecenie Update-Database, aby zmiany były stosowane lokalnie.</span><span class="sxs-lookup"><span data-stu-id="43655-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="43655-206">Jeśli jednak chcemy wypchnąć te zmiany na serwer testowy, a ostatecznie do produkcji, prawdopodobnie chcemy skryptu SQL, który możemy przekazać naszemu DBA.</span><span class="sxs-lookup"><span data-stu-id="43655-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="43655-207">Uruchom polecenie **Aktualizuj bazę danych,** ale tym razem określ flagę **–Script,** aby zmiany były zapisywane w skrypcie, a nie stosowane.</span><span class="sxs-lookup"><span data-stu-id="43655-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="43655-208">Określimy również źródło i migrację docelową, aby wygenerować skrypt.</span><span class="sxs-lookup"><span data-stu-id="43655-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="43655-209">Chcemy, aby skrypt przejść z pustej bazy danych (**$InitialDatabase**) do najnowszej wersji (migracja **AddPostAbstract**).</span><span class="sxs-lookup"><span data-stu-id="43655-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="43655-210">*Jeśli migracja docelowa nie zostanie określona, migracje użyją najnowszej migracji jako obiektu docelowego. Jeśli nie określisz migracji źródłowej, migracje użyją bieżącego stanu bazy danych.*</span><span class="sxs-lookup"><span data-stu-id="43655-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="43655-211">Uruchom polecenie **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** w konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="43655-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="43655-212">Code First Migrations uruchomi potok migracji, ale zamiast faktycznie zastosować zmiany, zapisze je do pliku .sql.</span><span class="sxs-lookup"><span data-stu-id="43655-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="43655-213">Po wygenerowaniu skryptu jest on otwierany w programie Visual Studio, gotowy do wyświetlenia lub zapisania.</span><span class="sxs-lookup"><span data-stu-id="43655-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="43655-214">Generowanie skryptów idempotentnych</span><span class="sxs-lookup"><span data-stu-id="43655-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="43655-215">Począwszy od EF6, jeśli określisz **—SourceMigration $InitialDatabase,** wygenerowany skrypt będzie "idempotent".</span><span class="sxs-lookup"><span data-stu-id="43655-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="43655-216">Skrypty Idempotentne można uaktualnić bazę danych obecnie w dowolnej wersji do najnowszej wersji (lub określonej wersji, jeśli używasz **-TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="43655-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="43655-217">Wygenerowany skrypt zawiera logikę, aby sprawdzić \*\* \_ \_MigrationsHistory\*\* tabeli i tylko zastosować zmiany, które nie zostały wcześniej zastosowane.</span><span class="sxs-lookup"><span data-stu-id="43655-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="43655-218">Automatyczne uaktualnianie podczas uruchamiania aplikacji (migrateDatabaseToLatestVersion Initializer)</span><span class="sxs-lookup"><span data-stu-id="43655-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="43655-219">Jeśli wdrażasz aplikację, możesz chcieć, aby automatycznie uaktualniła bazę danych (stosując wszelkie oczekujące migracje) po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43655-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="43655-220">Można to zrobić, rejestrującicwizator bazy danych **MigrateDatabaseToLatestVersion.**</span><span class="sxs-lookup"><span data-stu-id="43655-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="43655-221">Inicjator bazy danych po prostu zawiera pewną logikę, która jest używana, aby upewnić się, że baza danych jest poprawnie skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="43655-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="43655-222">Ta logika jest uruchamiana po raz pierwszy kontekst jest używany w procesie aplikacji (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="43655-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="43655-223">Możemy zaktualizować plik **Program.cs,** jak pokazano poniżej, aby ustawić **migrateDatabaseToLatestVersion inicjatora** blogcontext przed użyciem kontekstu (wiersz 14).</span><span class="sxs-lookup"><span data-stu-id="43655-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="43655-224">Należy również zauważyć, że należy również dodać using instrukcji dla obszaru nazw **System.Data.Entity** (wiersz 5).</span><span class="sxs-lookup"><span data-stu-id="43655-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="43655-225">*Podczas tworzenia wystąpienia tego inicjatora musimy określić typ kontekstu **(BlogContext)** i konfigurację migracji **(Konfiguracja)**- konfiguracja migracji jest klasą, która została dodana do naszego **folderu Migracje,** gdy włączyliśmy migracje.*</span><span class="sxs-lookup"><span data-stu-id="43655-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="43655-226">Teraz, gdy nasza aplikacja działa, najpierw sprawdzi, czy baza danych, na które jest kierowana, jest aktualna, i zastosuje wszelkie oczekujące migracje, jeśli nie jest.</span><span class="sxs-lookup"><span data-stu-id="43655-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>

---
title: Powiązanie danych przy użyciu platformy WPF — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 5bd4a9b98a12de41e4ec37c2cc7dbdc537210893
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490236"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="e95d8-102">Powiązanie danych przy użyciu platformy WPF</span><span class="sxs-lookup"><span data-stu-id="e95d8-102">Databinding with WPF</span></span>
<span data-ttu-id="e95d8-103">Ten przewodnik krok po kroku pokazano, jak powiązać POCO typy kontrolek WPF w postaci "elementy główne szczegóły".</span><span class="sxs-lookup"><span data-stu-id="e95d8-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="e95d8-104">Aplikacja używa interfejsów API programu Entity Framework do wypełniania obiekty z danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="e95d8-105">Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: **kategorii** (jednostki\\głównego) i **produktu** (zależne\\szczegółów).</span><span class="sxs-lookup"><span data-stu-id="e95d8-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="e95d8-106">Następnie narzędzi programu Visual Studio są używane do powiązania typy zdefiniowane w modelu, który ma kontrolek WPF.</span><span class="sxs-lookup"><span data-stu-id="e95d8-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="e95d8-107">Struktura powiązanie danych WPF umożliwia nawigacji między powiązane obiekty: Wybieranie wierszy w widoku głównego powoduje, że widok szczegółów aktualizacji za pomocą odpowiednich danych podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="e95d8-108">Zrzuty ekranu i zamieszczone w tym przewodniku są pobierane z programu Visual Studio 2013, ale możesz ukończyć ten przewodnik przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e95d8-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="e95d8-109">Użyj opcji "Object" do tworzenia źródeł danych w WPF</span><span class="sxs-lookup"><span data-stu-id="e95d8-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="e95d8-110">Za pomocą poprzedniej wersji programu Entity Framework użyliśmy zalecamy używanie **bazy danych** podczas tworzenia nowego źródła danych oparty na modelu utworzonych za pomocą projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="e95d8-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="e95d8-111">Taka konfiguracja była ponieważ projektant wygeneruje kontekstu, który pochodzi od obiektu ObjectContext i klas jednostek, które pochodną EntityObject.</span><span class="sxs-lookup"><span data-stu-id="e95d8-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="e95d8-112">Przy użyciu opcji bazy danych umożliwia pisanie kodu najlepsze do interakcji z tym powierzchni interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e95d8-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="e95d8-113">Projektant EF dla programu Visual Studio 2012 i Visual Studio 2013 generowanie kontekstu, który pochodzi od typu DbContext wraz z prostego klas obiektów POCO.</span><span class="sxs-lookup"><span data-stu-id="e95d8-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="e95d8-114">Za pomocą programu Visual Studio 2010, zaleca się zamianę do szablonu generowania kodu, który używa typu DbContext, zgodnie z opisem w dalszej części tego przewodnika.</span><span class="sxs-lookup"><span data-stu-id="e95d8-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="e95d8-115">Korzystając z powierzchni interfejsu API typu DbContext należy użyć **obiektu** opcji podczas tworzenia nowego źródła danych, jak pokazano w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="e95d8-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="e95d8-116">Jeśli to konieczne, możesz [powrócić do generowania kodu na podstawie obiektu ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) dla modeli utworzonych za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="e95d8-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e95d8-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e95d8-117">Pre-Requisites</span></span>

<span data-ttu-id="e95d8-118">Musisz mieć program Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010 należy zainstalować w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="e95d8-119">Jeśli używasz programu Visual Studio 2010 należy zainstalować NuGet.</span><span class="sxs-lookup"><span data-stu-id="e95d8-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="e95d8-120">Aby uzyskać więcej informacji, zobacz [Instalowanie systemu NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="e95d8-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="e95d8-121">Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e95d8-121">Create the Application</span></span>

-   <span data-ttu-id="e95d8-122">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e95d8-122">Open Visual Studio</span></span>
-   <span data-ttu-id="e95d8-123">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e95d8-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="e95d8-124">Wybierz **Windows** w okienku po lewej stronie i **WPFApplication** w okienku po prawej stronie</span><span class="sxs-lookup"><span data-stu-id="e95d8-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="e95d8-125">Wprowadź **WPFwithEFSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="e95d8-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="e95d8-126">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="e95d8-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="e95d8-127">Instalowanie pakietu NuGet programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e95d8-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="e95d8-128">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **WinFormswithEFSample** projektu</span><span class="sxs-lookup"><span data-stu-id="e95d8-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="e95d8-129">Wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="e95d8-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="e95d8-130">W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu</span><span class="sxs-lookup"><span data-stu-id="e95d8-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="e95d8-131">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="e95d8-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="e95d8-132">Oprócz zestawu EntityFramework dodawany jest również odwołania do System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="e95d8-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="e95d8-133">Jeśli projekt zawiera odwołanie do System.Data.Entity, następnie zostanie on usunięty po zainstalowaniu pakietu platformy EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="e95d8-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="e95d8-134">Zestaw System.Data.Entity nie jest już używany w przypadku aplikacji platformy Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e95d8-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="e95d8-135">Zdefiniuj Model</span><span class="sxs-lookup"><span data-stu-id="e95d8-135">Define a Model</span></span>

<span data-ttu-id="e95d8-136">W tym przewodniku, możesz wybrać do zaimplementowania model przy użyciu Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="e95d8-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="e95d8-137">Wykonaj jedną z dwóch poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="e95d8-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="e95d8-138">Opcja 1: Definiowanie modelu za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="e95d8-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="e95d8-139">W tej sekcji przedstawiono sposób tworzenia modelu i jego skojarzonej bazy danych za pomocą funkcji Code First.</span><span class="sxs-lookup"><span data-stu-id="e95d8-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="e95d8-140">Przejdź do następnej sekcji (**opcja 2: Definiowanie model przy użyciu pierwszej bazy danych)** Jeśli zostanie wykorzystany raczej Database First odwrócić inżynier modelu z bazy danych za pomocą projektanta EF</span><span class="sxs-lookup"><span data-stu-id="e95d8-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="e95d8-141">Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).</span><span class="sxs-lookup"><span data-stu-id="e95d8-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="e95d8-142">Dodaj nową klasę do **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="e95d8-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="e95d8-143">Kliknij prawym przyciskiem myszy nazwę projektu</span><span class="sxs-lookup"><span data-stu-id="e95d8-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="e95d8-144">Wybierz **Dodaj**, następnie **nowy element**</span><span class="sxs-lookup"><span data-stu-id="e95d8-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="e95d8-145">Wybierz **klasy** i wprowadź **produktu** nazwy klasy</span><span class="sxs-lookup"><span data-stu-id="e95d8-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="e95d8-146">Zastąp **produktu** klasy definicji z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="e95d8-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="e95d8-147">**Produktów** właściwość **kategorii** klasy i **kategorii** właściwość **produktu** klasy są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="e95d8-148">Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="e95d8-149">Oprócz definiowania jednostek, musisz zdefiniować klasę, która pochodzi od typu DbContext i udostępnia DbSet&lt;TEntity&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="e95d8-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="e95d8-150">DbSet&lt;TEntity&gt; właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="e95d8-151">Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="e95d8-152">Dodaj nową **ProductContext** klasy do projektu przy użyciu następujących definicji:</span><span class="sxs-lookup"><span data-stu-id="e95d8-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="e95d8-153">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="e95d8-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="e95d8-154">Opcja 2: Definiowanie model przy użyciu pierwszej bazy danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="e95d8-155">W tej sekcji pokazano, jak użyć pierwszej bazy danych do odtworzenia modelu z bazy danych za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="e95d8-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="e95d8-156">Jeśli należy wykonać czynności opisane w poprzedniej sekcji (**opcja 1: Definiowanie modelu za pomocą funkcji Code First)**, Pomiń tę sekcję i przejdź bezpośrednio do **powolne ładowanie** sekcji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="e95d8-157">Utwórz istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-157">Create an Existing Database</span></span>

<span data-ttu-id="e95d8-158">Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="e95d8-159">Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="e95d8-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="e95d8-160">Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.</span><span class="sxs-lookup"><span data-stu-id="e95d8-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="e95d8-161">Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="e95d8-162">Rozpocznijmy i wygenerować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="e95d8-163">**Widok —&gt; Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="e95d8-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="e95d8-164">Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="e95d8-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="e95d8-165">Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera przed, musisz wybrać programu Microsoft SQL Server jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Zmień źródło danych](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="e95d8-167">Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany, a następnie wprowadź **produktów** jako nazwa bazy danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Dodaj połączenie LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Dodaj połączenie Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="e95d8-170">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="e95d8-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Tworzenie bazy danych](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="e95d8-172">Nowe bazy danych będą teraz wyświetlane w Eksploratorze serwera, kliknij prawym przyciskiem myszy na nim i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="e95d8-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="e95d8-173">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="e95d8-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="e95d8-174">Model odtwarzania</span><span class="sxs-lookup"><span data-stu-id="e95d8-174">Reverse Engineer Model</span></span>

<span data-ttu-id="e95d8-175">Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="e95d8-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="e95d8-176">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="e95d8-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="e95d8-177">Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="e95d8-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="e95d8-178">Wprowadź **ProductModel** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="e95d8-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="e95d8-179">Spowoduje to uruchomienie **Kreator modelu Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="e95d8-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="e95d8-180">Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="e95d8-180">Select **Generate from Database** and click **Next**</span></span>

    ![Wybierz zawartość modelu](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="e95d8-182">Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, wprowadź **ProductContext** jako nazwa parametrów połączenia i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="e95d8-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Wybierz połączenie](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="e95d8-184">Kliknij pole wyboru obok "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"</span><span class="sxs-lookup"><span data-stu-id="e95d8-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Wybierz obiekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="e95d8-186">Po zakończeniu procesu odtwarzania nowy model jest dodawane do projektu i otworzona w celu wyświetlania w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e95d8-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="e95d8-187">Dodano również pliku App.config do projektu przy użyciu szczegółów połączenia dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="e95d8-188">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="e95d8-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="e95d8-189">Jeśli pracujesz w programie Visual Studio 2010 należy zaktualizować projektancie platformy EF używać EF6 generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="e95d8-190">Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="e95d8-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="e95d8-191">Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**</span><span class="sxs-lookup"><span data-stu-id="e95d8-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="e95d8-192">Wybierz **EF Generator DbContext dla języka C w wersji 6.x\#,** wprowadź **ProductsModel** jako nazwę i kliknij przycisk Dodaj</span><span class="sxs-lookup"><span data-stu-id="e95d8-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="e95d8-193">Aktualizowanie generowania kodu dla powiązania danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-193">Updating code generation for data binding</span></span>

<span data-ttu-id="e95d8-194">EF generuje kod z modelu przy użyciu szablonów T4.</span><span class="sxs-lookup"><span data-stu-id="e95d8-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="e95d8-195">Szablonów dostarczanych z programem Visual Studio lub pobrać z galerii programu Visual Studio są przeznaczone do użytku ogólnego przeznaczenia.</span><span class="sxs-lookup"><span data-stu-id="e95d8-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="e95d8-196">To oznacza, że jednostki generowane w te szablony mają prosty interfejs ICollection&lt;T&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="e95d8-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="e95d8-197">Jednak podczas ustalania danych powiązania, za pomocą WPF jest pożądane, aby użyć **observablecollection —** dla właściwości kolekcji, tak że WPF można śledzenie pracy do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="e95d8-198">W tym celu firma Microsoft będzie modyfikować szablony do użycia z observablecollection —.</span><span class="sxs-lookup"><span data-stu-id="e95d8-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="e95d8-199">Otwórz **Eksploratora rozwiązań** i Znajdź **ProductModel.edmx** pliku</span><span class="sxs-lookup"><span data-stu-id="e95d8-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="e95d8-200">Znajdź **ProductModel.tt** pliku, który będzie można zagnieździć w pliku ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="e95d8-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Szablon modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="e95d8-202">Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e95d8-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="e95d8-203">Znajdź i Zamień dwa wystąpienia "**ICollection**"z"**observablecollection —**".</span><span class="sxs-lookup"><span data-stu-id="e95d8-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="e95d8-204">Są to znajduje się mniej więcej w wierszach 296 i 484.</span><span class="sxs-lookup"><span data-stu-id="e95d8-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="e95d8-205">Znajdź i Zamień pierwsze wystąpienie ciągu "**hashset —**"z"**observablecollection —**".</span><span class="sxs-lookup"><span data-stu-id="e95d8-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="e95d8-206">To wystąpienie znajduje się mniej więcej w wierszu 50.</span><span class="sxs-lookup"><span data-stu-id="e95d8-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="e95d8-207">**Nie** zastąpić drugie wystąpienie hashset — znaleziono w dalszej części kodu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="e95d8-208">Znajdź i Zamień tylko wystąpienie "**System.Collections.Generic**"z"**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="e95d8-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="e95d8-209">To znajduje się mniej więcej w wierszu 424.</span><span class="sxs-lookup"><span data-stu-id="e95d8-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="e95d8-210">Zapisz plik ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="e95d8-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="e95d8-211">Powinno to spowodować kodu dla jednostek do ponownego wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="e95d8-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="e95d8-212">Jeśli kod nie jest generowany ponownie automatycznie, kliknij prawym przyciskiem myszy ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".</span><span class="sxs-lookup"><span data-stu-id="e95d8-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="e95d8-213">Jeśli teraz otworzysz plik Category.cs (która jest zagnieżdżony w ProductModel.tt), wówczas powinien zostać wyświetlony, że kolekcja produktów ma typ **observablecollection —&lt;produktu&gt;**.</span><span class="sxs-lookup"><span data-stu-id="e95d8-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="e95d8-214">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="e95d8-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="e95d8-215">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="e95d8-215">Lazy Loading</span></span>

<span data-ttu-id="e95d8-216">**Produktów** właściwość **kategorii** klasy i **kategorii** właściwość **produktu** klasy są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="e95d8-217">Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="e95d8-218">EF daje możliwość ładowanie powiązanych jednostek z bazy danych automatycznie po raz pierwszy możesz uzyskać dostęp do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e95d8-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="e95d8-219">W przypadku tego typu (nazywane powolne ładowanie) podczas ładowania należy pamiętać, że po raz pierwszy możesz uzyskać dostęp do każdej właściwości nawigacji oddzielnego zapytania będą wykonywane względem bazy danych, jeśli zawartość nie jest już w kontekście.</span><span class="sxs-lookup"><span data-stu-id="e95d8-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="e95d8-220">Korzystając z typów jednostki POCO, EF realizuje powolne ładowanie przez tworzenie wystąpień typów pochodnych serwera proxy podczas wykonywania, a następnie zastępowanie właściwości wirtualnych w Twoich zajęciach, które można dodać punktów zaczepienia ładowania.</span><span class="sxs-lookup"><span data-stu-id="e95d8-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="e95d8-221">Aby uzyskać powolne ładowanie powiązanych obiektów, należy zadeklarować nawigacji metody pobierające właściwości jako **publicznych** i **wirtualnego** (**Overridable** w języku Visual Basic), i możesz klasy nie może być **zapieczętowanego** (**NotOverridable** w języku Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="e95d8-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="e95d8-222">Przy użyciu bazy danych właściwości nawigacji pierwszy zostaną zastosowane automatycznie wirtualnego, aby umożliwić ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="e95d8-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="e95d8-223">W sekcji Code First Wybraliśmy Tworzenie właściwości nawigacji wirtualnego z tego samego powodu</span><span class="sxs-lookup"><span data-stu-id="e95d8-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="e95d8-224">Wiązanie obiektu z kontrolkami</span><span class="sxs-lookup"><span data-stu-id="e95d8-224">Bind Object to Controls</span></span>

<span data-ttu-id="e95d8-225">Dodawanie klas, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WPF.</span><span class="sxs-lookup"><span data-stu-id="e95d8-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="e95d8-226">Kliknij dwukrotnie **MainWindow.xaml** w Eksploratorze rozwiązań, aby otworzyć formularz główny</span><span class="sxs-lookup"><span data-stu-id="e95d8-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="e95d8-227">W menu głównym wybierz **projekt —&gt; Dodaj nowe źródło danych...**</span><span class="sxs-lookup"><span data-stu-id="e95d8-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="e95d8-228">(w programie Visual Studio 2010, musisz wybrać **dane —&gt; Dodaj nowe źródło danych...** )</span><span class="sxs-lookup"><span data-stu-id="e95d8-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="e95d8-229">W polu Wybierz Typewindow źródła danych wybierz **obiektu** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="e95d8-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="e95d8-230">Wybierz obiekty danych okna dialogowego rozwijania może **WPFwithEFSample** dwa razy i wybierz pozycję **kategorii**</span><span class="sxs-lookup"><span data-stu-id="e95d8-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="e95d8-231">*Nie ma potrzeby wybrać **produktu** źródła danych, ponieważ można je uzyskać do niego za pośrednictwem **produktu**przez właściwość **kategorii** źródła danych*</span><span class="sxs-lookup"><span data-stu-id="e95d8-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Wybierz obiekty danych](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="e95d8-233">Kliknij przycisk **Zakończ.**</span><span class="sxs-lookup"><span data-stu-id="e95d8-233">Click **Finish.**</span></span>
-   <span data-ttu-id="e95d8-234">Okno źródeł danych zostanie otwarty obok okna MainWindow.xaml *okna źródła danych nie są wyświetlane, wybierz opcję **widok —&gt; inne Windows -&gt; źródeł danych***</span><span class="sxs-lookup"><span data-stu-id="e95d8-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="e95d8-235">Naciśnij ikonę pinezki, więc okna źródeł danych, automatycznie ukrywać.</span><span class="sxs-lookup"><span data-stu-id="e95d8-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="e95d8-236">Może być konieczne przycisk Odśwież okno zostało już widoczne.</span><span class="sxs-lookup"><span data-stu-id="e95d8-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Źródła danych](~/ef6/media/datasources.png)

-   <span data-ttu-id="e95d8-238">Wybierz pozycję \*\* kategorii \*\* źródła danych i przeciągnij je na formularzu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-238">Select the \*\*Category \*\*data source and drag it on the form.</span></span>

<span data-ttu-id="e95d8-239">Następujące wystąpił podczas możemy przeciągnąć tego źródła:</span><span class="sxs-lookup"><span data-stu-id="e95d8-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="e95d8-240">**CategoryViewSource** zasobów i \*\* categoryDataGrid \*\* kontroli zostały dodane do XAML.</span><span class="sxs-lookup"><span data-stu-id="e95d8-240">The **categoryViewSource** resource and the\*\* categoryDataGrid\*\* control were added to XAML.</span></span> <span data-ttu-id="e95d8-241">Aby uzyskać więcej informacji na temat DataViewSources zobacz http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="e95d8-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="e95d8-242">Właściwość DataContext nadrzędnego elementu siatki "{staticresource — **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="e95d8-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="e95d8-243">**CategoryViewSource** zasobu służy jako źródło powiązania zewnętrzne\\nadrzędnego elementu siatki.</span><span class="sxs-lookup"><span data-stu-id="e95d8-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="e95d8-244">Wewnętrzny elementów siatki, wartość elementu DataContext następnie jest Dziedzicz po elemencie nadrzędnym siatki (właściwości ItemsSource categoryDataGrid jest ustawiona na "{powiązania}").</span><span class="sxs-lookup"><span data-stu-id="e95d8-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="e95d8-245">Dodawanie szczegółów siatki</span><span class="sxs-lookup"><span data-stu-id="e95d8-245">Adding a Details Grid</span></span>

<span data-ttu-id="e95d8-246">Skoro mamy już siatki, aby wyświetlić kategorie Przyjrzyjmy dodać siatkę szczegóły, aby wyświetlić skojarzone produkty.</span><span class="sxs-lookup"><span data-stu-id="e95d8-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="e95d8-247">Wybierz pozycję \*\* produktów \*\* właściwości w obszarze \*\* kategorii \*\* źródła danych i przeciągnij je na formularzu.</span><span class="sxs-lookup"><span data-stu-id="e95d8-247">Select the \*\*Products \*\*property from under the \*\*Category \*\*data source and drag it on the form.</span></span>
    -   <span data-ttu-id="e95d8-248">**CategoryProductsViewSource** zasobów i **productDataGrid** siatki są dodawane do XAML</span><span class="sxs-lookup"><span data-stu-id="e95d8-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="e95d8-249">Ścieżka powiązania dla tego zasobu jest równa produktów</span><span class="sxs-lookup"><span data-stu-id="e95d8-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="e95d8-250">Framework powiązanie danych WPF zapewnia tylko produktów związanych z wybraną kategorię wyświetlane w **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="e95d8-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="e95d8-251">Z przybornika przeciągnij **przycisk** się do formularza.</span><span class="sxs-lookup"><span data-stu-id="e95d8-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="e95d8-252">Ustaw **nazwa** właściwości **buttonSave** i **zawartości** właściwości **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="e95d8-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="e95d8-253">Formularz powinien wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="e95d8-253">The form should look similar to this:</span></span>

![Projektant](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="e95d8-255">Dodaj kod, który obsługuje interakcji z danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="e95d8-256">Nadszedł czas, aby dodać niektóre procedury obsługi zdarzeń do głównego okna.</span><span class="sxs-lookup"><span data-stu-id="e95d8-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="e95d8-257">W oknie XAML kliknij  **&lt;okna** elementu, po wybraniu tej opcji okna głównego</span><span class="sxs-lookup"><span data-stu-id="e95d8-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="e95d8-258">W **właściwości** wybierz okno **zdarzenia** w prawym górnym rogu, następnie kliknij dwukrotnie przycisk po prawej stronie pola tekstowego **Loaded** etykiety</span><span class="sxs-lookup"><span data-stu-id="e95d8-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Główne okno właściwości](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="e95d8-260">Również dodać **kliknij** zdarzenie **Zapisz** przycisku przez dwukrotne kliknięcie przycisku Zapisz w projektancie.</span><span class="sxs-lookup"><span data-stu-id="e95d8-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="e95d8-261">Wybranie tej opcji powoduje możesz kodu formularza, firma Microsoft będzie teraz edytować kod, aby użyć ProductContext przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="e95d8-262">Zaktualizuj kod dla okna głównego, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="e95d8-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="e95d8-263">Ten kod deklaruje wystąpienie długotrwałych **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="e95d8-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="e95d8-264">**ProductContext** obiekt jest używany do wykonywania zapytań i zapisać dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e95d8-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="e95d8-265">**Dispose**() na **ProductContext** wystąpienia jest następnie wywoływana z zastąpione **OnClosing** metody.</span><span class="sxs-lookup"><span data-stu-id="e95d8-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="e95d8-266">Komentarze w kodzie zawierają szczegółowe informacje o dany kod realizuje.</span><span class="sxs-lookup"><span data-stu-id="e95d8-266">The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="e95d8-267">Testowanie aplikacji WPF</span><span class="sxs-lookup"><span data-stu-id="e95d8-267">Test the WPF Application</span></span>

-   <span data-ttu-id="e95d8-268">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="e95d8-268">Compile and run the application.</span></span> <span data-ttu-id="e95d8-269">Jeśli użyto Code First, a następnie zostanie wyświetlony **WPFwithEFSample.ProductContext** baza danych została utworzona dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="e95d8-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="e95d8-270">Wprowadź nazwę kategorii w górnym siatki produktu nazwy i w dół siatki *wypełniać kolumny Identyfikatora, ponieważ klucz podstawowy jest generowany przez bazę danych*</span><span class="sxs-lookup"><span data-stu-id="e95d8-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Okno główne z nowych kategorii i produktami](~/ef6/media/screen1.png)

-   <span data-ttu-id="e95d8-272">Naciśnij klawisz **Zapisz** przycisk, aby zapisać dane w bazie danych</span><span class="sxs-lookup"><span data-stu-id="e95d8-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="e95d8-273">Po wywołaniu przez DbContext **SaveChanges**(), identyfikatory są wypełniane przy użyciu wartości z bazy danych, wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="e95d8-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="e95d8-274">Ponieważ firma Microsoft o nazwie **Odśwież**() po **SaveChanges**() **DataGrid** formanty są aktualizowane na nowe wartości.</span><span class="sxs-lookup"><span data-stu-id="e95d8-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Okno główne z identyfikatorami wypełnione](~/ef6/media/screen2.png)

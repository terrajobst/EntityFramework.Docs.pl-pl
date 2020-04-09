---
title: Powiązanie danych za pomocą WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639144"
---
> [!IMPORTANT]
> <span data-ttu-id="96e77-102">**Ten dokument jest prawidłowy tylko dla programu WPF w programie .NET Framework**</span><span class="sxs-lookup"><span data-stu-id="96e77-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="96e77-103">W tym dokumencie opisano powiązanie danych dla WPF WPF w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="96e77-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="96e77-104">W przypadku nowych projektów .NET Core zaleca się użycie [ef core](/ef/core) zamiast entity framework 6.</span><span class="sxs-lookup"><span data-stu-id="96e77-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="96e77-105">Dokumentacja wiązania danych w EF Core jest śledzona w [#778 problem](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span><span class="sxs-lookup"><span data-stu-id="96e77-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="96e77-106">Powiązanie danych za pomocą WPF</span><span class="sxs-lookup"><span data-stu-id="96e77-106">Databinding with WPF</span></span>
<span data-ttu-id="96e77-107">W tym przewodniku krok po kroku pokazano, jak powiązać typy POCO do formantów WPF w formularzu "master-detail".</span><span class="sxs-lookup"><span data-stu-id="96e77-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="96e77-108">Aplikacja używa interfejsów API frameworka do wypełniania obiektów danymi z bazy danych, śledzenia zmian i utrwalania danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="96e77-109">Model definiuje dwa typy, które uczestniczą w **Category** relacji jeden\\do wielu: Kategoria (główny główny) i **Produkt** (szczegóły zależne).\\</span><span class="sxs-lookup"><span data-stu-id="96e77-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="96e77-110">Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu do formantów WPF.</span><span class="sxs-lookup"><span data-stu-id="96e77-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="96e77-111">Struktura powiązania danych WPF umożliwia nawigację między powiązanymi obiektami: wybranie wierszy w widoku głównym powoduje, że widok szczegółów do aktualizacji z odpowiednimi danymi podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="96e77-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="96e77-112">Zrzuty ekranu i listy kodów w tym instruktażu są pobierane z programu Visual Studio 2013, ale można ukończyć ten przewodnik za pomocą programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="96e77-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="96e77-113">Użyj opcji "Obiekt" do tworzenia źródeł danych WPF</span><span class="sxs-lookup"><span data-stu-id="96e77-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="96e77-114">W poprzedniej wersji entity framework uzyliśmy przy użyciu **opcji bazy danych** podczas tworzenia nowego źródła danych na podstawie modelu utworzonego za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="96e77-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="96e77-115">Stało się tak, ponieważ projektant będzie generować kontekst, który pochodzi z ObjectContext i klasy jednostek, które pochodzą z EntityObject.</span><span class="sxs-lookup"><span data-stu-id="96e77-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="96e77-116">Za pomocą opcji bazy danych pomoże napisać najlepszy kod do interakcji z tej powierzchni interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="96e77-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="96e77-117">Projektanci EF dla programu Visual Studio 2012 i Visual Studio 2013 generują kontekst, który wywodzi się z DbContext wraz z prostymi klasami jednostek POCO.</span><span class="sxs-lookup"><span data-stu-id="96e77-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="96e77-118">W programie Visual Studio 2010 zaleca się zamianę na szablon generowania kodu, który używa DbContext, jak opisano w dalszej części tego przewodnika.</span><span class="sxs-lookup"><span data-stu-id="96e77-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="96e77-119">Podczas korzystania z powierzchni interfejsu API DbContext należy użyć **opcji Obiekt** podczas tworzenia nowego źródła danych, jak pokazano w tym instruktażu.</span><span class="sxs-lookup"><span data-stu-id="96e77-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="96e77-120">W razie potrzeby można [przywrócić do generowania kodu opartego na ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) dla modeli utworzonych za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="96e77-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="96e77-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="96e77-121">Pre-Requisites</span></span>

<span data-ttu-id="96e77-122">Aby ukończyć ten przewodnik, musisz mieć zainstalowany program Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="96e77-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="96e77-123">Jeśli używasz programu Visual Studio 2010, należy również zainstalować NuGet.</span><span class="sxs-lookup"><span data-stu-id="96e77-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="96e77-124">Aby uzyskać więcej informacji, zobacz [Instalowanie nuget](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="96e77-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="96e77-125">Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="96e77-125">Create the Application</span></span>

-   <span data-ttu-id="96e77-126">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96e77-126">Open Visual Studio</span></span>
-   <span data-ttu-id="96e77-127">**Plik&gt; —&gt; nowy — projekt....**</span><span class="sxs-lookup"><span data-stu-id="96e77-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="96e77-128">Wybieranie **systemu Windows** w lewym okienku i **WPFApplication** w prawym okienku</span><span class="sxs-lookup"><span data-stu-id="96e77-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="96e77-129">Wprowadź **WPFwithEFSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="96e77-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="96e77-130">Wybierz **przycisk OK**</span><span class="sxs-lookup"><span data-stu-id="96e77-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="96e77-131">Instalowanie pakietu NuGet framework encji</span><span class="sxs-lookup"><span data-stu-id="96e77-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="96e77-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="96e77-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="96e77-133">Wybierz **pozycję Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="96e77-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="96e77-134">W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **Online** i wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="96e77-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="96e77-135">Kliknij **pozycję Zainstaluj**</span><span class="sxs-lookup"><span data-stu-id="96e77-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="96e77-136">Oprócz zestawu EntityFramework dodano również odwołanie do System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="96e77-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="96e77-137">Jeśli projekt ma odwołanie do System.Data.Entity, zostanie on usunięty po zainstalowaniu pakietu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="96e77-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="96e77-138">Zestaw System.Data.Entity nie jest już używany dla aplikacji Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="96e77-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="96e77-139">Definiowanie modelu</span><span class="sxs-lookup"><span data-stu-id="96e77-139">Define a Model</span></span>

<span data-ttu-id="96e77-140">W tym instruktażu można wybrać do zaimplementowania modelu przy użyciu code first lub projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="96e77-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="96e77-141">Ukończ jedną z dwóch poniższych sekcji.</span><span class="sxs-lookup"><span data-stu-id="96e77-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="96e77-142">Opcja 1: Najpierw zdefiniuj model przy użyciu kodu</span><span class="sxs-lookup"><span data-stu-id="96e77-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="96e77-143">W tej sekcji pokazano, jak utworzyć model i skojarzoną z nim bazę danych przy użyciu kodu najpierw.</span><span class="sxs-lookup"><span data-stu-id="96e77-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="96e77-144">Przejdź do następnej sekcji **(Opcja 2: Zdefiniuj model przy użyciu bazy danych najpierw),** jeśli wolisz użyć bazy danych najpierw do odtworzenia modelu z bazy danych przy użyciu projektanta EF</span><span class="sxs-lookup"><span data-stu-id="96e77-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="96e77-145">Podczas korzystania z code first rozwoju zwykle rozpocząć od pisania .NET Framework klas, które definiują model koncepcyjny (domena).</span><span class="sxs-lookup"><span data-stu-id="96e77-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="96e77-146">Dodaj nową klasę do **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="96e77-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="96e77-147">Kliknij prawym przyciskiem myszy nazwę projektu</span><span class="sxs-lookup"><span data-stu-id="96e77-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="96e77-148">Wybierz pozycję **Dodaj**, a następnie **pozycję Nowy element**</span><span class="sxs-lookup"><span data-stu-id="96e77-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="96e77-149">Wybierz **klasę** i wprowadź **produkt** dla nazwy klasy</span><span class="sxs-lookup"><span data-stu-id="96e77-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="96e77-150">Zastąp definicję klasy **produktu** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="96e77-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="96e77-151">**Właściwość Produkty** w **Category** klasy i **Category** właściwości na **Product** klasy są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="96e77-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="96e77-152">W entity framework właściwości nawigacji umożliwiają poruszanie się po relacji między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="96e77-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="96e77-153">Oprócz definiowania jednostek, należy zdefiniować klasę, która pochodzi z DbContext&lt;i udostępnia&gt; właściwości DbSet TEntity.</span><span class="sxs-lookup"><span data-stu-id="96e77-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="96e77-154">Właściwości DbSet&lt;TEntity&gt; niech kontekst wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="96e77-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="96e77-155">Wystąpienie typu pochodnego DbContext zarządza obiektami jednostki w czasie wykonywania, który obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="96e77-156">Dodaj nową klasę **ProductContext** do projektu z następującą definicją:</span><span class="sxs-lookup"><span data-stu-id="96e77-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="96e77-157">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="96e77-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="96e77-158">Opcja 2: Najpierw zdefiniuj model przy użyciu bazy danych</span><span class="sxs-lookup"><span data-stu-id="96e77-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="96e77-159">W tej sekcji pokazano, jak używać najpierw bazy danych do inżynierii wstecznej modelu z bazy danych przy użyciu projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="96e77-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="96e77-160">Jeśli poprzednia sekcja została ukończona (**Opcja 1: Zdefiniuj model przy użyciu najpierw kodu),** pomiń tę sekcję i przejdź bezpośrednio do sekcji **Ładowanie z opóźnieniem.**</span><span class="sxs-lookup"><span data-stu-id="96e77-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="96e77-161">Tworzenie istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="96e77-161">Create an Existing Database</span></span>

<span data-ttu-id="96e77-162">Zazwyczaj podczas kierowania istniejącej bazy danych zostanie już utworzona, ale w tym instruktażu musimy utworzyć bazę danych, aby uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="96e77-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="96e77-163">Serwer bazy danych zainstalowany w programie Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="96e77-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="96e77-164">Jeśli używasz programu Visual Studio 2010, będziesz tworzyć bazę danych PROGRAMU SQL Express.</span><span class="sxs-lookup"><span data-stu-id="96e77-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="96e77-165">Jeśli używasz programu Visual Studio 2012, a następnie będziesz tworzyć bazy danych [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="96e77-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="96e77-166">Przejdźmy dalej i wygenerujmy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="96e77-167">**Widok&gt; — Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="96e77-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="96e77-168">Kliknij prawym przyciskiem myszy na **Połączenia danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="96e77-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="96e77-169">Jeśli nie masz połączenia z bazą danych z Eksploratora serwera, zanim będziesz musiał wybrać program Microsoft SQL Server jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="96e77-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Zmienianie źródła danych](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="96e77-171">Połącz się z localdb lub SQL Express, w zależności od tego, który z nich został zainstalowany, i wprowadź **produkty** jako nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="96e77-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Dodawanie lokalnej bazy danych połączeń](~/ef6/media/addconnectionlocaldb.png)

    ![Dodaj ekspres połączenia](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="96e77-174">Wybierz **przycisk OK,** a pojawi się pytanie, czy chcesz utworzyć nową bazę danych, wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="96e77-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Create Database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="96e77-176">Nowa baza danych pojawi się teraz w Eksploratorze serwera, kliknij ją prawym przyciskiem myszy i wybierz **pozycję Nowa kwerenda**</span><span class="sxs-lookup"><span data-stu-id="96e77-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="96e77-177">Skopiuj następujący SQL do nowej kwerendy, a następnie kliknij prawym przyciskiem myszy kwerendę i wybierz polecenie **Wykonaj**</span><span class="sxs-lookup"><span data-stu-id="96e77-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="96e77-178">Model inżyniera odwrotnego</span><span class="sxs-lookup"><span data-stu-id="96e77-178">Reverse Engineer Model</span></span>

<span data-ttu-id="96e77-179">Mamy zamiar korzystać z Entity Framework Designer, który jest dołączony do programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="96e77-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="96e77-180">**Projekt&gt; — dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="96e77-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="96e77-181">Wybierz **pozycję Dane** z lewego menu, a następnie ADO.NET modelu danych **encji**</span><span class="sxs-lookup"><span data-stu-id="96e77-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="96e77-182">Wprowadź **ProductModel** jako nazwę i kliknij **przycisk OK**</span><span class="sxs-lookup"><span data-stu-id="96e77-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="96e77-183">Spowoduje to uruchomienie **Kreatora modeli danych encji**</span><span class="sxs-lookup"><span data-stu-id="96e77-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="96e77-184">Wybierz **pozycję Generuj z bazy danych** i kliknij przycisk **Dalej**</span><span class="sxs-lookup"><span data-stu-id="96e77-184">Select **Generate from Database** and click **Next**</span></span>

    ![Wybierz zawartość modelu](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="96e77-186">Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **ProductContext** jako nazwę ciągu połączenia i kliknij przycisk **Dalej**</span><span class="sxs-lookup"><span data-stu-id="96e77-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Wybierz połączenie](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="96e77-188">Kliknij pole wyboru obok pozycji "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"</span><span class="sxs-lookup"><span data-stu-id="96e77-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Wybierz swoje obiekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="96e77-190">Po zakończeniu procesu inżynierii odwrotnej nowy model jest dodawany do projektu i otwiera się do wyświetlania w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="96e77-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="96e77-191">Plik App.config został również dodany do projektu ze szczegółami połączenia dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="96e77-192">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="96e77-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="96e77-193">Jeśli pracujesz w programie Visual Studio 2010, należy zaktualizować projektanta EF do generowania kodu EF6.</span><span class="sxs-lookup"><span data-stu-id="96e77-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="96e77-194">Kliknij prawym przyciskiem myszy puste miejsce modelu w projektancie EF i wybierz pozycję **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="96e77-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="96e77-195">Wybierz **szablony online** z lewego menu i wyszukaj **DbContext**</span><span class="sxs-lookup"><span data-stu-id="96e77-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="96e77-196">Wybierz **EF 6.x DbContext\#Generator dla C ,** wprowadź **ProductsModel** jako nazwę i kliknij przycisk Dodaj</span><span class="sxs-lookup"><span data-stu-id="96e77-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="96e77-197">Aktualizowanie generowania kodu dla powiązania danych</span><span class="sxs-lookup"><span data-stu-id="96e77-197">Updating code generation for data binding</span></span>

<span data-ttu-id="96e77-198">EF generuje kod z modelu przy użyciu szablonów T4.</span><span class="sxs-lookup"><span data-stu-id="96e77-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="96e77-199">Szablony dostarczane z programem Visual Studio lub pobrane z galerii programu Visual Studio są przeznaczone do użytku ogólnego.</span><span class="sxs-lookup"><span data-stu-id="96e77-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="96e77-200">Oznacza to, że jednostki generowane z tych szablonów mają proste właściwości ICollection&lt;T.&gt;</span><span class="sxs-lookup"><span data-stu-id="96e77-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="96e77-201">Jednak podczas wykonywania powiązania danych przy użyciu WPF WPF jest pożądane, aby użyć **ObservableCollection** dla właściwości kolekcji, dzięki czemu WPF można śledzić zmiany wprowadzone do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="96e77-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="96e77-202">W tym celu będziemy modyfikować szablony do korzystania z ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="96e77-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="96e77-203">Otwórz **Eksploratora rozwiązań** i znajdź plik **ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="96e77-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="96e77-204">Znajdź plik **ProductModel.tt,** który będzie zagnieżdżony pod plikiem ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="96e77-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Szablon modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="96e77-206">Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96e77-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="96e77-207">Znajdź i wymień dwa wystąpienia "**ICollection**" na "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="96e77-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="96e77-208">Znajdują się one w przybliżeniu na liniach 296 i 484.</span><span class="sxs-lookup"><span data-stu-id="96e77-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="96e77-209">Znajdź i zastąp pierwsze wystąpienie**hashsetu**na "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="96e77-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="96e77-210">To wystąpienie znajduje się w przybliżeniu na linii 50.</span><span class="sxs-lookup"><span data-stu-id="96e77-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="96e77-211">**Nie** należy zastępować drugiego wystąpienia zestawu hashset znalezionego w dalszej części kodu.</span><span class="sxs-lookup"><span data-stu-id="96e77-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="96e77-212">Znajdź i zastąp jedyne wystąpienie "**System.Collections.Generic**" na "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="96e77-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="96e77-213">Znajduje się on w przybliżeniu na linii 424.</span><span class="sxs-lookup"><span data-stu-id="96e77-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="96e77-214">Zapisz plik ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="96e77-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="96e77-215">Powinno to spowodować, że kod dla jednostek do ponownego wygenerowanie.</span><span class="sxs-lookup"><span data-stu-id="96e77-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="96e77-216">Jeśli kod nie zostanie automatycznie wygenerowany, kliknij prawym przyciskiem myszy ProductModel.tt i wybierz "Uruchom narzędzie niestandardowe".</span><span class="sxs-lookup"><span data-stu-id="96e77-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="96e77-217">Jeśli teraz otworzysz plik Category.cs (który jest zagnieżdżony w obszarze ProductModel.tt), powinieneś zobaczyć, że kolekcja Products ma typ **&lt;ObservableCollection Product&gt;**.</span><span class="sxs-lookup"><span data-stu-id="96e77-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="96e77-218">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="96e77-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="96e77-219">Z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="96e77-219">Lazy Loading</span></span>

<span data-ttu-id="96e77-220">**Właściwość Produkty** w **Category** klasy i **Category** właściwości na **Product** klasy są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="96e77-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="96e77-221">W entity framework właściwości nawigacji umożliwiają poruszanie się po relacji między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="96e77-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="96e77-222">EF umożliwia automatyczne ładowanie powiązanych jednostek z bazy danych przy pierwszym wejściu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="96e77-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="96e77-223">W przypadku tego typu ładowania (nazywanego ładowaniem z opóźnieniem) należy pamiętać, że przy pierwszym dostępie do każdej właściwości nawigacji zostanie wykonana oddzielna kwerenda względem bazy danych, jeśli zawartość nie znajduje się jeszcze w kontekście.</span><span class="sxs-lookup"><span data-stu-id="96e77-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="96e77-224">Podczas korzystania z typów jednostek POCO EF osiąga ładowanie z opóźnieniem, tworząc wystąpienia pochodnych typów proxy w czasie wykonywania, a następnie zastępując właściwości wirtualne w klasach, aby dodać hak ładowania.</span><span class="sxs-lookup"><span data-stu-id="96e77-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="96e77-225">Aby uzyskać opóźnienie ładowania powiązanych obiektów, należy zadeklarować metodyciwości nawigacji jako **publiczne** i **wirtualne** **(możliwe do zastąpienia** w języku Visual Basic), a klasa nie może być **zapieczętowana** **(NotOverridable** w języku Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="96e77-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="96e77-226">Podczas korzystania z bazy danych pierwsze właściwości nawigacji są automatycznie wirtualne, aby włączyć ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="96e77-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="96e77-227">W sekcji Code First zdecydowaliśmy się uczynić właściwości nawigacji wirtualnymi z tego samego powodu</span><span class="sxs-lookup"><span data-stu-id="96e77-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="96e77-228">Obiekt powiązania z formanty</span><span class="sxs-lookup"><span data-stu-id="96e77-228">Bind Object to Controls</span></span>

<span data-ttu-id="96e77-229">Dodaj klasy, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WPF.</span><span class="sxs-lookup"><span data-stu-id="96e77-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="96e77-230">Kliknij dwukrotnie plik **MainWindow.xaml** w Eksploratorze rozwiązań, aby otworzyć formularz główny</span><span class="sxs-lookup"><span data-stu-id="96e77-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="96e77-231">Z menu głównego wybierz **Project -&gt; Dodaj nowe źródło danych ...**</span><span class="sxs-lookup"><span data-stu-id="96e77-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="96e77-232">(w programie Visual Studio 2010 należy wybrać **opcję Dane —&gt; Dodaj nowe źródło danych...**)</span><span class="sxs-lookup"><span data-stu-id="96e77-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="96e77-233">W obszarze Wybieranie okna typem źródła danych wybierz pozycję **Obiekt** i kliknij przycisk **Dalej**</span><span class="sxs-lookup"><span data-stu-id="96e77-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="96e77-234">W oknie dialogowym Wybieranie obiektów danych rozwiń **wpfwithefsample** dwa razy i wybierz **kategorię**</span><span class="sxs-lookup"><span data-stu-id="96e77-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="96e77-235">*Nie ma potrzeby wybierania źródła danych **produktu,** ponieważ do niego przejdziemy za pośrednictwem właściwości **Product**'s w źródle danych **kategoria***</span><span class="sxs-lookup"><span data-stu-id="96e77-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Wybieranie obiektów danych](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="96e77-237">Kliknij **pozycję Zakończ.**</span><span class="sxs-lookup"><span data-stu-id="96e77-237">Click **Finish.**</span></span>
-   <span data-ttu-id="96e77-238">Okno Źródła danych jest otwierane obok okna MainWindow.xaml \*Jeśli okno Źródła danych nie jest wyświetlane, wybierz pozycję Widok — **&gt; Inne źródła&gt; danych systemu Windows** \*</span><span class="sxs-lookup"><span data-stu-id="96e77-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="96e77-239">Naciśnij ikonę pinezki, aby okno Źródła danych nie było automatycznie ukrywane.</span><span class="sxs-lookup"><span data-stu-id="96e77-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="96e77-240">Może być konieczne naciśnięcie przycisku odświeżania, jeśli okno było już widoczne.</span><span class="sxs-lookup"><span data-stu-id="96e77-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Źródła danych](~/ef6/media/datasources.png)

-   <span data-ttu-id="96e77-242">Zaznacz źródło danych **Kategoria** i przeciągnij je w formularzu.</span><span class="sxs-lookup"><span data-stu-id="96e77-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="96e77-243">Następujące zdarzenia miały miejsce, gdy przeciągliśmy to źródło:</span><span class="sxs-lookup"><span data-stu-id="96e77-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="96e77-244">Do XAML dodano zasób **CategoryViewSource** i formant **categoryDataGrid**</span><span class="sxs-lookup"><span data-stu-id="96e77-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="96e77-245">Właściwość DataContext w nadrzędnym elemencie Siatki została ustawiona na "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="96e77-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="96e77-246">Zasób **CategoryViewSource** służy jako źródło\\powiązania dla zewnętrznego elementu siatki nadrzędnej.</span><span class="sxs-lookup"><span data-stu-id="96e77-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="96e77-247">Wewnętrzne elementy siatki dziedziczą następnie wartość DataContext z nadrzędnej siatki (właściwość CategoryDataGrid's ItemsSource jest ustawiona na "{Binding}")</span><span class="sxs-lookup"><span data-stu-id="96e77-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="96e77-248">Dodawanie siatki szczegółów</span><span class="sxs-lookup"><span data-stu-id="96e77-248">Adding a Details Grid</span></span>

<span data-ttu-id="96e77-249">Teraz, gdy mamy siatkę do wyświetlania kategorii dodajmy siatkę szczegółów, aby wyświetlić skojarzone produkty.</span><span class="sxs-lookup"><span data-stu-id="96e77-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="96e77-250">Wybierz **Products** właściwość spod źródła danych **kategoria** i przeciągnij go w formularzu.</span><span class="sxs-lookup"><span data-stu-id="96e77-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="96e77-251">Do xaml są dodawane zasoby **CategoryProductsViewSource** i **siatka productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="96e77-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="96e77-252">Ścieżka powiązania dla tego zasobu jest ustawiona na Produkty</span><span class="sxs-lookup"><span data-stu-id="96e77-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="96e77-253">Struktura wiązania danych WPF zapewnia, że tylko produkty związane z wybraną kategorią są wyświetlane w **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="96e77-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="96e77-254">W przyborniku przeciągnij **przycisk** do formularza.</span><span class="sxs-lookup"><span data-stu-id="96e77-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="96e77-255">Ustaw **właściwość Nazwa** na **buttonSave** i **Content,** właściwość **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="96e77-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="96e77-256">Formularz powinien wyglądać podobnie do tego:</span><span class="sxs-lookup"><span data-stu-id="96e77-256">The form should look similar to this:</span></span>

![Projektant](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="96e77-258">Dodaj kod, który obsługuje interakcję z danymi</span><span class="sxs-lookup"><span data-stu-id="96e77-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="96e77-259">Nadszedł czas, aby dodać niektóre programy obsługi zdarzeń do okna głównego.</span><span class="sxs-lookup"><span data-stu-id="96e77-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="96e77-260">W oknie XAML kliknij element \*\* &lt;Okno,\*\* w tym oknie wybiera okno główne</span><span class="sxs-lookup"><span data-stu-id="96e77-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="96e77-261">W oknie **Właściwości** wybierz pozycję **Zdarzenia** w prawym górnym rogu, a następnie kliknij dwukrotnie pole tekstowe po prawej stronie etykiety **Loaded**</span><span class="sxs-lookup"><span data-stu-id="96e77-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Właściwości okna głównego](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="96e77-263">Dodaj również zdarzenie **Kliknij** dla przycisku **Zapisz,** klikając dwukrotnie przycisk Zapisz w projektancie.</span><span class="sxs-lookup"><span data-stu-id="96e77-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="96e77-264">Spowoduje to, że do kodu za formularzem, będziemy teraz edytować kod, aby użyć ProductContext do wykonywania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="96e77-265">Zaktualizuj kod mainwindow, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="96e77-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="96e77-266">Kod deklaruje długotrwałe wystąpienie **ProduktuContext**.</span><span class="sxs-lookup"><span data-stu-id="96e77-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="96e77-267">**Obiekt ProductContext** służy do wykonywania zapytań i zapisywania danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="96e77-268">**Dispose()** w **ProductContext wystąpienie** jest następnie wywoływana z overridden **OnClosing** metody.</span><span class="sxs-lookup"><span data-stu-id="96e77-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="96e77-269">Komentarze do kodu zawierają szczegółowe informacje o tym, co robi kod.</span><span class="sxs-lookup"><span data-stu-id="96e77-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="96e77-270">Testowanie aplikacji WPF</span><span class="sxs-lookup"><span data-stu-id="96e77-270">Test the WPF Application</span></span>

-   <span data-ttu-id="96e77-271">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="96e77-271">Compile and run the application.</span></span> <span data-ttu-id="96e77-272">Jeśli użyto najpierw kodu, wtedy zobaczysz, że **wpfwithEFSample.ProductContext** bazy danych jest tworzony dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="96e77-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="96e77-273">Wprowadź nazwę kategorii w górnej siatce i nazwy produktów w dolnej siatce *Nie wprowadzaj niczego w kolumnach identyfikatorów, ponieważ klucz podstawowy jest generowany przez bazę danych*</span><span class="sxs-lookup"><span data-stu-id="96e77-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Okno główne z nowymi kategoriami i produktami](~/ef6/media/screen1.png)

-   <span data-ttu-id="96e77-275">Naciśnij przycisk **Zapisz,** aby zapisać dane w bazie danych</span><span class="sxs-lookup"><span data-stu-id="96e77-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="96e77-276">Po wywołaniu **SaveChanges()** programu DbContext identyfikatory są wypełniane wartościami generowanymi przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="96e77-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="96e77-277">Ponieważ po **savechanges()** nazwaliśmy **refresh()** formanty **DataGrid** są również aktualizowane o nowe wartości.</span><span class="sxs-lookup"><span data-stu-id="96e77-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Okno główne z wypełnionymi identyfikatorami](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="96e77-279">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="96e77-279">Additional Resources</span></span>

<span data-ttu-id="96e77-280">Aby dowiedzieć się więcej na temat powiązania danych z kolekcjami przy użyciu WPF, zobacz [ten temat](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) w dokumentacji WPF.</span><span class="sxs-lookup"><span data-stu-id="96e77-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  

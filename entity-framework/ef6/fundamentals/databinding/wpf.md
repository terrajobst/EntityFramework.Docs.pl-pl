---
title: Wiązanie danych przy użyciu WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416597"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="d9ea7-102">Wiązanie danych z WPF</span><span class="sxs-lookup"><span data-stu-id="d9ea7-102">Databinding with WPF</span></span>
<span data-ttu-id="d9ea7-103">W tym przewodniku krok po kroku pokazano, jak powiązać typy POCO z kontrolkami WPF w formularzu "wzorzec-szczegóły".</span><span class="sxs-lookup"><span data-stu-id="d9ea7-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="d9ea7-104">Aplikacja używa Entity Framework interfejsów API do wypełniania obiektów danymi z bazy danych, śledzenia zmian i utrwalania danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="d9ea7-105">Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: **Kategoria** (główna\\główna) i **produkt** (zależne\\szczegóły).</span><span class="sxs-lookup"><span data-stu-id="d9ea7-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="d9ea7-106">Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu z kontrolkami WPF.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="d9ea7-107">Struktura powiązań danych WPF umożliwia nawigowanie między obiektami pokrewnymi: wybranie wierszy w widoku wzorca powoduje aktualizację widoku szczegółów z odpowiednimi danymi podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="d9ea7-108">Zrzuty ekranu i listy kodu w tym instruktażu są pobierane z Visual Studio 2013 ale można wykonać ten Instruktaż w programie Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="d9ea7-109">Użyj opcji "Object" do tworzenia źródeł danych WPF</span><span class="sxs-lookup"><span data-stu-id="d9ea7-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="d9ea7-110">W przypadku wcześniejszej wersji Entity Framework zalecamy użycie opcji **bazy danych** podczas tworzenia nowego źródła danych na podstawie modelu utworzonego za pomocą narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="d9ea7-111">To dlatego, że projektant wygeneruje kontekst pochodzący z klas ObjectContext i Entity Classes, które pochodzą z obiektu EntityObject.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="d9ea7-112">Użycie opcji baza danych pomoże Ci napisać najlepszy kod do współdziałania z tą powierzchnią interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="d9ea7-113">Projektanci programu EF dla programu Visual Studio 2012 i Visual Studio 2013 generują kontekst, który pochodzi od DbContext wraz z prostymi klasami jednostek POCO.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="d9ea7-114">W programie Visual Studio 2010 zalecamy zamianę na szablon generowania kodu, który używa DbContext, zgodnie z opisem w dalszej części tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="d9ea7-115">Korzystając z powierzchni interfejsu API DbContext, należy użyć opcji **Object** podczas tworzenia nowego źródła danych, jak pokazano w tym instruktażu.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="d9ea7-116">W razie potrzeby można [wrócić do generowania kodu opartego na obiekcie ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) dla modeli utworzonych za pomocą programu EF Designer.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d9ea7-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d9ea7-117">Pre-Requisites</span></span>

<span data-ttu-id="d9ea7-118">Aby ukończyć ten przewodnik, musisz mieć zainstalowany Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d9ea7-119">W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="d9ea7-120">Aby uzyskać więcej informacji, zobacz [Instalowanie programu NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="d9ea7-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="d9ea7-121">Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d9ea7-121">Create the Application</span></span>

-   <span data-ttu-id="d9ea7-122">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-122">Open Visual Studio</span></span>
-   <span data-ttu-id="d9ea7-123">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="d9ea7-124">Wybierz pozycję **Windows** w lewym okienku i **WPFApplication** w okienku po prawej stronie</span><span class="sxs-lookup"><span data-stu-id="d9ea7-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="d9ea7-125">Wprowadź **WPFwithEFSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="d9ea7-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="d9ea7-126">Wybierz **przycisk OK**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="d9ea7-127">Zainstaluj pakiet NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d9ea7-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="d9ea7-128">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="d9ea7-129">Wybierz pozycję **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="d9ea7-130">W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="d9ea7-131">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="d9ea7-132">Oprócz zestawu EntityFramework dodawane jest również odwołanie do elementu System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="d9ea7-133">Jeśli projekt zawiera odwołanie do System. Data. Entity, zostanie usunięte po zainstalowaniu pakietu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="d9ea7-134">Zestaw system. Data. Entity nie jest już używany w przypadku aplikacji Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="d9ea7-135">Zdefiniuj model</span><span class="sxs-lookup"><span data-stu-id="d9ea7-135">Define a Model</span></span>

<span data-ttu-id="d9ea7-136">W tym instruktażu możesz wybrać wdrożenie modelu przy użyciu Code First lub programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="d9ea7-137">Wykonaj jedną z dwóch poniższych sekcji.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="d9ea7-138">Opcja 1: definiowanie modelu przy użyciu Code First</span><span class="sxs-lookup"><span data-stu-id="d9ea7-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="d9ea7-139">W tej sekcji pokazano, jak utworzyć model i skojarzoną z nim bazę danych przy użyciu Code First.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="d9ea7-140">Przejdź do następnej sekcji (**Opcja 2: zdefiniuj model przy użyciu Database First)** , jeśli wolisz używać Database First do odtwarzania modelu z bazy danych przy użyciu narzędzia Dr Designer</span><span class="sxs-lookup"><span data-stu-id="d9ea7-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="d9ea7-141">W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny).</span><span class="sxs-lookup"><span data-stu-id="d9ea7-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="d9ea7-142">Dodaj nową klasę do **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="d9ea7-143">Kliknij prawym przyciskiem myszy nazwę projektu</span><span class="sxs-lookup"><span data-stu-id="d9ea7-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="d9ea7-144">Wybierz pozycję **Dodaj**, a następnie pozycję **nowy element**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="d9ea7-145">Wybierz **klasę** i wprowadź  **produktu** dla nazwy klasy</span><span class="sxs-lookup"><span data-stu-id="d9ea7-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="d9ea7-146">Zastąp definicję klasy  **produktu** następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="d9ea7-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="d9ea7-147">Właściwości **Products** klasy **Category** i **Category** klasy **Product** są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="d9ea7-148">W Entity Framework właściwości nawigacji umożliwiają nawigowanie po relacjach między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="d9ea7-149">Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z DbContext i uwidacznia Nieogólnymi&lt;&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="d9ea7-150">Nieogólnymi&lt;&gt; właściwości umożliwiają kontekstowi znać, które typy mają być uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="d9ea7-151">Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="d9ea7-152">Dodaj nową klasę **ProductContext** do projektu z następującą definicją:</span><span class="sxs-lookup"><span data-stu-id="d9ea7-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="d9ea7-153">Kompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="d9ea7-154">Opcja 2: definiowanie modelu przy użyciu Database First</span><span class="sxs-lookup"><span data-stu-id="d9ea7-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="d9ea7-155">W tej sekcji pokazano, jak używać programu Database First, aby odtworzyć model z bazy danych przy użyciu narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="d9ea7-156">Jeśli poprzednia sekcja została ukończona (**Opcja 1: zdefiniuj model przy użyciu Code First)** , Pomiń tę sekcję i przejdź bezpośrednio do sekcji **ładowanie z opóźnieniem** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="d9ea7-157">Tworzenie istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="d9ea7-157">Create an Existing Database</span></span>

<span data-ttu-id="d9ea7-158">Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="d9ea7-159">Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d9ea7-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d9ea7-160">Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="d9ea7-161">Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="d9ea7-162">Przyjrzyjmy się i wygenerujemy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="d9ea7-163">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d9ea7-164">Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d9ea7-165">Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="d9ea7-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Zmień źródło danych](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="d9ea7-167">Połącz się z usługą LocalDB lub SQL Express, w zależności od tego, który został zainstalowany, i wprowadź **produkty** jako nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="d9ea7-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Dodaj LocalDB połączenia](~/ef6/media/addconnectionlocaldb.png)

    ![Dodaj połączenie ekspresowe](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="d9ea7-170">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Create Database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="d9ea7-172">Nowa baza danych będzie teraz wyświetlana w Eksplorator serwera, kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="d9ea7-173">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="d9ea7-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="d9ea7-174">Odtwórz Model</span><span class="sxs-lookup"><span data-stu-id="d9ea7-174">Reverse Engineer Model</span></span>

<span data-ttu-id="d9ea7-175">Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="d9ea7-176">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="d9ea7-177">Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="d9ea7-178">Wprowadź **ProductModel** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="d9ea7-179">Spowoduje to uruchomienie **kreatora Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="d9ea7-180">Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-180">Select **Generate from Database** and click **Next**</span></span>

    ![Wybierz zawartość modelu](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="d9ea7-182">Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **ProductContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Wybierz połączenie](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="d9ea7-184">Kliknij pole wyboru obok pozycji "tabele", aby zaimportować wszystkie tabele, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Wybieranie obiektów](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="d9ea7-186">Po zakończeniu procesu odtwarzania nowy model zostanie dodany do projektu i otwarty do wyświetlania w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="d9ea7-187">Plik App. config został również dodany do projektu i zawiera szczegółowe informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="d9ea7-188">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d9ea7-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="d9ea7-189">Jeśli pracujesz w programie Visual Studio 2010, musisz zaktualizować projektanta EF, aby korzystał z generowania kodu EF6.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="d9ea7-190">Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="d9ea7-191">Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="d9ea7-192">Wybierz opcję **Dr 6. x DbContext generator dla C\#,** wprowadź **ProductsModel** jako nazwę, a następnie kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="d9ea7-193">Aktualizowanie generowania kodu dla powiązania danych</span><span class="sxs-lookup"><span data-stu-id="d9ea7-193">Updating code generation for data binding</span></span>

<span data-ttu-id="d9ea7-194">EF generuje kod z modelu przy użyciu szablonów T4.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="d9ea7-195">Szablony dostarczane z programem Visual Studio lub pobrane z galerii programu Visual Studio są przeznaczone do ogólnego użycia.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="d9ea7-196">Oznacza to, że jednostki wygenerowane na podstawie tych szablonów mają proste właściwości ICollection&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="d9ea7-197">Jednak podczas tworzenia powiązań danych przy użyciu platformy WPF pożądane jest użycie **ObservableCollection** dla właściwości kolekcji, dzięki czemu WPF może śledzić zmiany wprowadzone do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="d9ea7-198">W tym celu będziemy modyfikować szablony, aby używać ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="d9ea7-199">Otwórz **Eksplorator rozwiązań** i Znajdź plik **ProductModel. edmx**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="d9ea7-200">Znajdź plik **ProductModel.tt** , który zostanie zagnieżdżony w pliku ProductModel. edmx</span><span class="sxs-lookup"><span data-stu-id="d9ea7-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Szablon modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="d9ea7-202">Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="d9ea7-203">Znajdź i Zamień dwa wystąpienia elementu "**ICollection**" na "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="d9ea7-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="d9ea7-204">Znajdują się one w przybliżeniu w wierszach 296 i 484.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="d9ea7-205">Znajdź i Zamień pierwsze wystąpienie elementu "**HashSet —** " na "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="d9ea7-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="d9ea7-206">To wystąpienie znajduje się około w wierszu 50.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="d9ea7-207">**Nie** zamieniaj drugiego wystąpienia HashSet — znalezionego w dalszej części kodu.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="d9ea7-208">Znajdź i Zamień jedynym wystąpieniem elementu "**System. Collections. Generic**" na "**System. Collections. ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="d9ea7-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="d9ea7-209">Ta lokalizacja znajduje się około w wierszu 424.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="d9ea7-210">Zapisz plik ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="d9ea7-211">Powinno to spowodować, że kod dla jednostek zostanie ponownie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="d9ea7-212">Jeśli kod nie zostanie wygenerowany automatycznie, kliknij prawym przyciskiem myszy pozycję ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".</span><span class="sxs-lookup"><span data-stu-id="d9ea7-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="d9ea7-213">Jeśli teraz otworzysz plik Category.cs (który jest zagnieżdżony w obszarze ProductModel.tt), powinna zostać wyświetlona, że kolekcja Products ma typ **ObservableCollection&lt;produktu&gt;** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="d9ea7-214">Kompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="d9ea7-215">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="d9ea7-215">Lazy Loading</span></span>

<span data-ttu-id="d9ea7-216">Właściwości **Products** klasy **Category** i **Category** klasy **Product** są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="d9ea7-217">W Entity Framework właściwości nawigacji umożliwiają nawigowanie po relacjach między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="d9ea7-218">EF oferuje opcję ładowania powiązanych jednostek z bazy danych automatycznie przy pierwszym dostępie do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="d9ea7-219">W przypadku tego typu ładowania (nazywanego ładowaniem opóźnionym) należy pamiętać, że podczas pierwszego uzyskiwania dostępu do każdej właściwości nawigacji oddzielne zapytanie zostanie wykonane względem bazy danych, jeśli zawartość nie jest jeszcze w kontekście.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="d9ea7-220">W przypadku korzystania z typów jednostek POCO EF osiąga opóźnione ładowanie przez utworzenie wystąpień pochodnych typów proxy podczas wykonywania, a następnie Zastępowanie właściwości wirtualnych w klasach, aby dodać punkt zaczepienia ładowania.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="d9ea7-221">Aby uzyskać opóźnione ładowanie pokrewnych obiektów, należy zadeklarować metody do pobierania właściwości nawigacji jako **publiczne** i **wirtualne** (Zastąp w Visual Basic), a Klasa nie może być **zapieczętowana** **(** **NotOverridable** w Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="d9ea7-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="d9ea7-222">Przy użyciu Database First właściwości nawigacji są automatycznie wprowadzane do wirtualnego, aby umożliwić ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="d9ea7-223">W sekcji Code First wybrano, aby właściwości nawigacji były wirtualne z tego samego powodu</span><span class="sxs-lookup"><span data-stu-id="d9ea7-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="d9ea7-224">Powiąż obiekt z kontrolkami</span><span class="sxs-lookup"><span data-stu-id="d9ea7-224">Bind Object to Controls</span></span>

<span data-ttu-id="d9ea7-225">Dodaj klasy, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WPF.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="d9ea7-226">Kliknij dwukrotnie pozycję **MainWindow. XAML** w Eksplorator rozwiązań, aby otworzyć formularz główny.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="d9ea7-227">Z menu głównego wybierz pozycję **projekt&gt; Dodaj nowe źródło danych...**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="d9ea7-228">(w programie Visual Studio 2010 musisz wybrać pozycję **dane&gt; Dodaj nowe źródło danych...** )</span><span class="sxs-lookup"><span data-stu-id="d9ea7-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="d9ea7-229">W oknie Wybieranie źródła danych Typewindow wybierz **obiekt** , a następnie kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="d9ea7-230">W oknie dialogowym Wybieranie obiektów danych unfold **WPFwithEFSample** dwa razy i wybierz **kategorię**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="d9ea7-231">*Nie ma potrzeby wybierania źródła danych **produktu** , ponieważ zostanie on przejdziemy do niego za pomocą właściwości **produktu**w źródle danych **kategorii** .*</span><span class="sxs-lookup"><span data-stu-id="d9ea7-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Wybierz obiekty danych](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="d9ea7-233">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-233">Click **Finish.**</span></span>
-   <span data-ttu-id="d9ea7-234">Okno źródła danych zostanie otwarte obok okna MainWindow. XAML *, jeśli okno źródła danych nie jest wyświetlane, wybierz pozycję **Wyświetl&gt; inne źródła danych&gt; Windows***</span><span class="sxs-lookup"><span data-stu-id="d9ea7-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="d9ea7-235">Naciśnij ikonę pinezki, aby okno źródła danych nie było ukrywane.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="d9ea7-236">Może być konieczne kliknięcie przycisku Odśwież, jeśli okno było już widoczne.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Źródła danych](~/ef6/media/datasources.png)

-   <span data-ttu-id="d9ea7-238">Wybierz **kategorię** źródło danych i przeciągnij ją na formularz.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="d9ea7-239">Podczas przeciągania tego źródła wystąpił następujący:</span><span class="sxs-lookup"><span data-stu-id="d9ea7-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="d9ea7-240">Dodano zasób **categoryViewSource** i kontrolkę **categoryDataGrid** do języka XAML</span><span class="sxs-lookup"><span data-stu-id="d9ea7-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="d9ea7-241">Właściwość DataContext elementu nadrzędnej siatki ma ustawioną wartość "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="d9ea7-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="d9ea7-242"> Zasób **categoryViewSource** służy jako źródło powiązań dla zewnętrznego elementu siatki\\nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-242"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="d9ea7-243">Elementy wewnętrznej siatki dziedziczą wartość DataContext z siatki nadrzędnej (Właściwość ItemsSource categoryDataGrid jest ustawiona na "{Binding}")</span><span class="sxs-lookup"><span data-stu-id="d9ea7-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="d9ea7-244">Dodawanie siatki szczegółów</span><span class="sxs-lookup"><span data-stu-id="d9ea7-244">Adding a Details Grid</span></span>

<span data-ttu-id="d9ea7-245">Teraz, gdy mamy siatkę do wyświetlania kategorii dodajemy siatkę szczegółów, aby wyświetlić skojarzone produkty.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="d9ea7-246">Wybierz właściwość **Products (produkty** ) w obszarze Źródło danych **kategorii** i przeciągnij ją na formularz.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="d9ea7-247">Zasób **categoryProductsViewSource** i siatka **productDataGrid** są dodawane do języka XAML</span><span class="sxs-lookup"><span data-stu-id="d9ea7-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="d9ea7-248">Ścieżka powiązania dla tego zasobu jest ustawiona na produkty</span><span class="sxs-lookup"><span data-stu-id="d9ea7-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="d9ea7-249">Struktura powiązań danych WPF zapewnia, że tylko produkty powiązane z wybraną kategorią będą widoczne w **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="d9ea7-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="d9ea7-250">Z przybornika przeciągnij **przycisk** do formularza.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="d9ea7-251">Ustaw właściwość **name** na **buttonSave** oraz Właściwość **Content** , która ma zostać **zapisana**.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="d9ea7-252">Formularz powinien wyglądać podobnie do tego:</span><span class="sxs-lookup"><span data-stu-id="d9ea7-252">The form should look similar to this:</span></span>

![Projektant](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="d9ea7-254">Dodawanie kodu, który obsługuje interakcję z danymi</span><span class="sxs-lookup"><span data-stu-id="d9ea7-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="d9ea7-255">Czas na dodanie obsługi zdarzeń do okna głównego.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="d9ea7-256">W oknie XAML kliknij element **okna&lt;** , zaznaczając Okno główne</span><span class="sxs-lookup"><span data-stu-id="d9ea7-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="d9ea7-257">W oknie **Właściwości** wybierz pozycję **zdarzenia** w prawym górnym rogu, a następnie kliknij dwukrotnie pole tekstowe po prawej stronie **załadowanej** etykiety.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Właściwości okna głównego](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="d9ea7-259">Dodaj również zdarzenie **kliknięcia** dla przycisku **Zapisz** przez dwukrotne kliknięcie przycisku Zapisz w projektancie.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="d9ea7-260">Spowoduje to przeprowadzenie pracy z kodem związanym z formularzem. teraz edytujemy kod, aby użyć ProductContext do zapewnienia dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="d9ea7-261">Zaktualizuj kod dla MainWindow, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="d9ea7-262">Kod deklaruje długotrwałe wystąpienie **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="d9ea7-263">Obiekt **ProductContext** jest używany do wykonywania zapytań i zapisywania danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="d9ea7-264">Metoda **Dispose ()** w wystąpieniu **ProductContext** jest następnie wywoływana z przesłoniętej metody **onzamykającej** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="d9ea7-265"> Komentarze do kodu zawierają szczegółowe informacje o tym, co robi kod.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-265"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="d9ea7-266">Testowanie aplikacji WPF</span><span class="sxs-lookup"><span data-stu-id="d9ea7-266">Test the WPF Application</span></span>

-   <span data-ttu-id="d9ea7-267">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-267">Compile and run the application.</span></span> <span data-ttu-id="d9ea7-268">Jeśli użyto Code First, zostanie wyświetlona baza danych **WPFwithEFSample. ProductContext** .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="d9ea7-269">Wprowadź nazwę kategorii w górnej siatce i nazwy produktów w dolnej siatce *nie wprowadzaj niczego w kolumnach identyfikatorów, ponieważ klucz podstawowy jest generowany przez bazę danych* .</span><span class="sxs-lookup"><span data-stu-id="d9ea7-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Okno główne z nowymi kategoriami i produktami](~/ef6/media/screen1.png)

-   <span data-ttu-id="d9ea7-271">Naciśnij przycisk **Save (Zapisz** ), aby zapisać dane w bazie danych</span><span class="sxs-lookup"><span data-stu-id="d9ea7-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="d9ea7-272">Po wywołaniu elementu DbContext **metody SaveChanges ()** identyfikatory są wypełniane wartościami wygenerowanymi przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="d9ea7-273">Ze względu na to, że wywołano **odświeżanie ()** po **metody SaveChanges ()** , formanty **DataGrid** są aktualizowane również przy użyciu nowych wartości.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Okno główne z wypełnionymi identyfikatorami](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="d9ea7-275">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d9ea7-275">Additional Resources</span></span>

<span data-ttu-id="d9ea7-276">Aby dowiedzieć się więcej na temat powiązania danych z kolekcjami przy użyciu platformy WPF, zobacz [ten temat](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) w dokumentacji WPF.</span><span class="sxs-lookup"><span data-stu-id="d9ea7-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  

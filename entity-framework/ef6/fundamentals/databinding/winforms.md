---
title: Wiązanie danych z WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 7ceb8e85fe3d8f5ab9a5e58ef9c84599585d8f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994532"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="93611-102">Wiązanie danych z WinForms</span><span class="sxs-lookup"><span data-stu-id="93611-102">Databinding with WinForms</span></span>
<span data-ttu-id="93611-103">Ten przewodnik krok po kroku pokazano, jak powiązać POCO typy formantów w formularzach systemu Windows (WinForms) w postaci "elementy główne szczegóły".</span><span class="sxs-lookup"><span data-stu-id="93611-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="93611-104">Aplikacja używa platformy Entity Framework do wypełniania obiekty z danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="93611-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="93611-105">Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: kategorii (jednostki\\głównego) i produktów (zależne\\szczegółów).</span><span class="sxs-lookup"><span data-stu-id="93611-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="93611-106">Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu do kontrolki WinForms.</span><span class="sxs-lookup"><span data-stu-id="93611-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="93611-107">Struktura powiązanie danych formularzy WinForm umożliwia nawigacji między powiązane obiekty: Wybieranie wierszy w widoku głównego powoduje, że widok szczegółów aktualizacji za pomocą odpowiednich danych podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="93611-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="93611-108">Zrzuty ekranu i zamieszczone w tym przewodniku są pobierane z programu Visual Studio 2013, ale możesz ukończyć ten przewodnik przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="93611-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="93611-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="93611-109">Pre-Requisites</span></span>

<span data-ttu-id="93611-110">Musisz mieć program Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010 należy zainstalować w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="93611-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="93611-111">Jeśli używasz programu Visual Studio 2010 należy zainstalować NuGet.</span><span class="sxs-lookup"><span data-stu-id="93611-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="93611-112">Aby uzyskać więcej informacji, zobacz [Instalowanie systemu NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="93611-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="93611-113">Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="93611-113">Create the Application</span></span>

-   <span data-ttu-id="93611-114">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93611-114">Open Visual Studio</span></span>
-   <span data-ttu-id="93611-115">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="93611-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="93611-116">Wybierz **Windows** w okienku po lewej stronie i **Windows FormsApplication** w okienku po prawej stronie</span><span class="sxs-lookup"><span data-stu-id="93611-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="93611-117">Wprowadź **WinFormswithEFSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="93611-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="93611-118">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="93611-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="93611-119">Instalowanie pakietu NuGet programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="93611-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="93611-120">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **WinFormswithEFSample** projektu</span><span class="sxs-lookup"><span data-stu-id="93611-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="93611-121">Wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="93611-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="93611-122">W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu</span><span class="sxs-lookup"><span data-stu-id="93611-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="93611-123">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="93611-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="93611-124">Oprócz zestawu EntityFramework dodawany jest również odwołania do System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="93611-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="93611-125">Jeśli projekt zawiera odwołanie do System.Data.Entity, następnie zostanie on usunięty po zainstalowaniu pakietu platformy EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="93611-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="93611-126">Zestaw System.Data.Entity nie jest już używany w przypadku aplikacji platformy Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="93611-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="93611-127">Implementowanie IListSource dla kolekcji</span><span class="sxs-lookup"><span data-stu-id="93611-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="93611-128">Właściwości kolekcji musi implementować interfejsu IListSource umożliwiające powiązanie dwukierunkowe danych za pomocą sortowania, korzystając z Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="93611-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="93611-129">W tym celu użyjemy rozszerzenie observablecollection — Dodawanie funkcji IListSource.</span><span class="sxs-lookup"><span data-stu-id="93611-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="93611-130">Dodaj **ObservableListSource** klasy do projektu:</span><span class="sxs-lookup"><span data-stu-id="93611-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="93611-131">Kliknij prawym przyciskiem myszy nazwę projektu</span><span class="sxs-lookup"><span data-stu-id="93611-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="93611-132">Wybierz **Add -&gt; nowy element**</span><span class="sxs-lookup"><span data-stu-id="93611-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="93611-133">Wybierz **klasy** i wprowadź **ObservableListSource** nazwy klasy</span><span class="sxs-lookup"><span data-stu-id="93611-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="93611-134">Zastąp kod wygenerowany przez domyślną, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="93611-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="93611-135">*Ta klasa umożliwia dwukierunkowe danych powiązania, a także sortowanie. Klasa pochodzi z observablecollection —&lt;T&gt; i dodaje Jawna implementacja interfejsu IListSource. Metoda GetList() IListSource zaimplementowano by zwrócić implementacje IBindingList, która pozostaje zsynchronizowany z observablecollection —. Implementacja IBindingList generowane przez ToBindingList obsługuje sortowanie. Metoda rozszerzenie ToBindingList jest zdefiniowany w zestawie platformy EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="93611-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="93611-136">Zdefiniuj Model</span><span class="sxs-lookup"><span data-stu-id="93611-136">Define a Model</span></span>

<span data-ttu-id="93611-137">W tym przewodniku, możesz wybrać do zaimplementowania model przy użyciu Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="93611-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="93611-138">Wykonaj jedną z dwóch poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="93611-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="93611-139">Opcja 1: Definiowanie modelu za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="93611-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="93611-140">W tej sekcji przedstawiono sposób tworzenia modelu i jego skojarzonej bazy danych za pomocą funkcji Code First.</span><span class="sxs-lookup"><span data-stu-id="93611-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="93611-141">Przejdź do następnej sekcji (**opcja 2: Definiowanie model przy użyciu pierwszej bazy danych)** Jeśli zostanie wykorzystany raczej Database First odwrócić inżynier modelu z bazy danych za pomocą projektanta EF</span><span class="sxs-lookup"><span data-stu-id="93611-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="93611-142">Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).</span><span class="sxs-lookup"><span data-stu-id="93611-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="93611-143">Dodaj nową **produktu** klasy do projektu</span><span class="sxs-lookup"><span data-stu-id="93611-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="93611-144">Zastąp kod wygenerowany przez domyślną, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="93611-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="93611-145">Dodaj **kategorii** klasy do projektu.</span><span class="sxs-lookup"><span data-stu-id="93611-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="93611-146">Zastąp kod wygenerowany przez domyślną, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="93611-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="93611-147">Oprócz Definiowanie jednostek, musisz zdefiniować klasę, która pochodzi od klasy **DbContext** i udostępnia **DbSet&lt;TEntity&gt;**  właściwości.</span><span class="sxs-lookup"><span data-stu-id="93611-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="93611-148">**DbSet** właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="93611-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="93611-149">**DbContext** i **DbSet** typy są definiowane w zestawie platformy EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="93611-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="93611-150">Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="93611-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="93611-151">Dodaj nową **ProductContext** klasy do projektu.</span><span class="sxs-lookup"><span data-stu-id="93611-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="93611-152">Zastąp kod wygenerowany przez domyślną, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="93611-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="93611-153">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="93611-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="93611-154">Opcja 2: Definiowanie model przy użyciu pierwszej bazy danych</span><span class="sxs-lookup"><span data-stu-id="93611-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="93611-155">W tej sekcji pokazano, jak użyć pierwszej bazy danych do odtworzenia modelu z bazy danych za pomocą projektanta EF.</span><span class="sxs-lookup"><span data-stu-id="93611-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="93611-156">Jeśli należy wykonać czynności opisane w poprzedniej sekcji (**opcja 1: Definiowanie modelu za pomocą funkcji Code First)**, Pomiń tę sekcję i przejdź bezpośrednio do **powolne ładowanie** sekcji.</span><span class="sxs-lookup"><span data-stu-id="93611-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="93611-157">Utwórz istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="93611-157">Create an Existing Database</span></span>

<span data-ttu-id="93611-158">Zazwyczaj przeznaczonych do istniejącej bazy danych, zostanie już utworzony, ale w tym przewodniku należy utworzyć bazy danych w celu uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="93611-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="93611-159">Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="93611-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="93611-160">Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.</span><span class="sxs-lookup"><span data-stu-id="93611-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="93611-161">Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="93611-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="93611-162">Rozpocznijmy i wygenerować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="93611-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="93611-163">**Widok —&gt; Eksploratora serwera**</span><span class="sxs-lookup"><span data-stu-id="93611-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="93611-164">Kliknij prawym przyciskiem myszy **połączeń danych -&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="93611-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="93611-165">Jeśli nie jest połączona z bazą danych za pomocą Eksploratora serwera przed, musisz wybrać programu Microsoft SQL Server jako źródło danych</span><span class="sxs-lookup"><span data-stu-id="93611-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="93611-167">Łączenie się z LocalDB lub SQL Express, w zależności od tego, który z nich został zainstalowany, a następnie wprowadź **produktów** jako nazwa bazy danych</span><span class="sxs-lookup"><span data-stu-id="93611-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="93611-170">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="93611-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="93611-172">Nowe bazy danych będą teraz wyświetlane w Eksploratorze serwera, kliknij prawym przyciskiem myszy na nim i wybierz **nowe zapytanie**</span><span class="sxs-lookup"><span data-stu-id="93611-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="93611-173">Skopiuj następujące instrukcje SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy, zapytania i wybierz pozycję **wykonania**</span><span class="sxs-lookup"><span data-stu-id="93611-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="93611-174">Model odtwarzania</span><span class="sxs-lookup"><span data-stu-id="93611-174">Reverse Engineer Model</span></span>

<span data-ttu-id="93611-175">Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="93611-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="93611-176">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="93611-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="93611-177">Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="93611-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="93611-178">Wprowadź **ProductModel** jako nazwę i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="93611-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="93611-179">Spowoduje to uruchomienie **Kreator modelu Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="93611-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="93611-180">Wybierz **Generuj z bazy danych** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="93611-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="93611-182">Wybierz połączenie do bazy danych utworzonej w pierwszej sekcji, wprowadź **ProductContext** jako nazwa parametrów połączenia i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="93611-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="93611-184">Kliknij pole wyboru obok "Tabele", aby zaimportować wszystkie tabele i kliknij przycisk "Zakończ"</span><span class="sxs-lookup"><span data-stu-id="93611-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="93611-186">Po zakończeniu procesu odtwarzania nowy model jest dodawane do projektu i otworzona w celu wyświetlania w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="93611-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="93611-187">Dodano również pliku App.config do projektu przy użyciu szczegółów połączenia dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="93611-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="93611-188">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="93611-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="93611-189">Jeśli pracujesz w programie Visual Studio 2010 należy zaktualizować projektancie platformy EF używać EF6 generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="93611-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="93611-190">Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="93611-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="93611-191">Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**</span><span class="sxs-lookup"><span data-stu-id="93611-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="93611-192">Wybierz **EF Generator DbContext dla języka C w wersji 6.x\#,** wprowadź **ProductsModel** jako nazwę i kliknij przycisk Dodaj</span><span class="sxs-lookup"><span data-stu-id="93611-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="93611-193">Aktualizowanie generowania kodu dla powiązania danych</span><span class="sxs-lookup"><span data-stu-id="93611-193">Updating code generation for data binding</span></span>

<span data-ttu-id="93611-194">EF generuje kod z modelu przy użyciu szablonów T4.</span><span class="sxs-lookup"><span data-stu-id="93611-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="93611-195">Szablonów dostarczanych z programem Visual Studio lub pobrać z galerii programu Visual Studio są przeznaczone do użytku ogólnego przeznaczenia.</span><span class="sxs-lookup"><span data-stu-id="93611-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="93611-196">To oznacza, że jednostki generowane w te szablony mają prosty interfejs ICollection&lt;T&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="93611-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="93611-197">Jednak podczas wiązania z danymi jest wskazane właściwości kolekcji, które implementują IListSource.</span><span class="sxs-lookup"><span data-stu-id="93611-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="93611-198">Dlatego utworzyliśmy klasy ObservableListSource powyżej i teraz zamierzamy zmodyfikować szablony Aby użyć tej klasy.</span><span class="sxs-lookup"><span data-stu-id="93611-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="93611-199">Otwórz **Eksploratora rozwiązań** i Znajdź **ProductModel.edmx** pliku</span><span class="sxs-lookup"><span data-stu-id="93611-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="93611-200">Znajdź **ProductModel.tt** pliku, który będzie można zagnieździć w pliku ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="93611-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="93611-202">Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93611-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="93611-203">Znajdź i Zamień dwa wystąpienia "**ICollection**"z"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="93611-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="93611-204">Te znajdują się w przybliżeniu wiersze 296 i 484.</span><span class="sxs-lookup"><span data-stu-id="93611-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="93611-205">Znajdź i Zamień pierwsze wystąpienie ciągu "**hashset —**"z"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="93611-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="93611-206">To wystąpienie znajduje się w wierszu około 50.</span><span class="sxs-lookup"><span data-stu-id="93611-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="93611-207">**Nie** zastąpić drugie wystąpienie hashset — znaleziono w dalszej części kodu.</span><span class="sxs-lookup"><span data-stu-id="93611-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="93611-208">Zapisz plik ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="93611-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="93611-209">Powinno to spowodować kodu dla jednostek do ponownego wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="93611-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="93611-210">Jeśli kod nie jest generowany ponownie automatycznie, kliknij prawym przyciskiem myszy ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".</span><span class="sxs-lookup"><span data-stu-id="93611-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="93611-211">Jeśli teraz otworzysz plik Category.cs (która jest zagnieżdżony w ProductModel.tt), wówczas powinien zostać wyświetlony, że kolekcja produktów ma typ **ObservableListSource&lt;produktu&gt;**.</span><span class="sxs-lookup"><span data-stu-id="93611-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="93611-212">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="93611-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="93611-213">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="93611-213">Lazy Loading</span></span>

<span data-ttu-id="93611-214">**Produktów** właściwość **kategorii** klasy i **kategorii** właściwość **produktu** klasy są właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="93611-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="93611-215">Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="93611-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="93611-216">EF daje możliwość ładowanie powiązanych jednostek z bazy danych automatycznie po raz pierwszy możesz uzyskać dostęp do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="93611-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="93611-217">W przypadku tego typu (nazywane powolne ładowanie) podczas ładowania należy pamiętać, że po raz pierwszy możesz uzyskać dostęp do każdej właściwości nawigacji oddzielnego zapytania będą wykonywane względem bazy danych, jeśli zawartość nie jest już w kontekście.</span><span class="sxs-lookup"><span data-stu-id="93611-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="93611-218">Korzystając z typów jednostki POCO, EF realizuje powolne ładowanie przez tworzenie wystąpień typów pochodnych serwera proxy podczas wykonywania, a następnie zastępowanie właściwości wirtualnych w Twoich zajęciach, które można dodać punktów zaczepienia ładowania.</span><span class="sxs-lookup"><span data-stu-id="93611-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="93611-219">Aby uzyskać powolne ładowanie powiązanych obiektów, należy zadeklarować nawigacji metody pobierające właściwości jako **publicznych** i **wirtualnego** (**Overridable** w języku Visual Basic), i możesz klasy nie może być **zapieczętowanego** (**NotOverridable** w języku Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="93611-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="93611-220">Przy użyciu bazy danych właściwości nawigacji pierwszy zostaną zastosowane automatycznie wirtualnego, aby umożliwić ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="93611-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="93611-221">W sekcji Code First Wybraliśmy Tworzenie właściwości nawigacji wirtualnego z tego samego powodu</span><span class="sxs-lookup"><span data-stu-id="93611-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="93611-222">Wiązanie obiektu z kontrolkami</span><span class="sxs-lookup"><span data-stu-id="93611-222">Bind Object to Controls</span></span>

<span data-ttu-id="93611-223">Dodawanie klas, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji formularzy WinForms.</span><span class="sxs-lookup"><span data-stu-id="93611-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="93611-224">W menu głównym wybierz **projekt —&gt; Dodaj nowe źródło danych...**</span><span class="sxs-lookup"><span data-stu-id="93611-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="93611-225">(w programie Visual Studio 2010, musisz wybrać **dane —&gt; Dodaj nowe źródło danych...** )</span><span class="sxs-lookup"><span data-stu-id="93611-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="93611-226">Wybierz typ źródła danych okna wybierz **obiektu** i kliknij przycisk **dalej**</span><span class="sxs-lookup"><span data-stu-id="93611-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="93611-227">Wybierz obiekty danych okna dialogowego rozwijania może **WinFormswithEFSample** dwa razy i wybierz pozycję **kategorii** nie ma potrzeby wybierz źródło danych produktu, ponieważ można je uzyskać do niego w produkcie Właściwość w źródle danych kategorii.</span><span class="sxs-lookup"><span data-stu-id="93611-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![DataSource](~/ef6/media/datasource.png)

-   <span data-ttu-id="93611-229">Kliknij przycisk **Zakończ.** 
     *Okna źródła danych nie są wyświetlane, wybierz opcję *** Widok —&gt; inne Windows -&gt; źródeł danych**</span><span class="sxs-lookup"><span data-stu-id="93611-229">Click **Finish.**
*If the Data Sources window is not showing up, select***View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="93611-230">Naciśnij ikonę pinezki, więc okna źródeł danych, automatycznie ukrywać.</span><span class="sxs-lookup"><span data-stu-id="93611-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="93611-231">Może być konieczne przycisk Odśwież okno zostało już widoczne.</span><span class="sxs-lookup"><span data-stu-id="93611-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSource2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="93611-233">W Eksploratorze rozwiązań kliknij dwukrotnie **Form1.cs** plik, aby otworzyć formularz główny w projektancie.</span><span class="sxs-lookup"><span data-stu-id="93611-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="93611-234">Wybierz **kategorii** źródła danych i przeciągnij je na formularzu.</span><span class="sxs-lookup"><span data-stu-id="93611-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="93611-235">Domyślnie nowe DataGridView (**categoryDataGridView**) i formanty nawigacji na pasku narzędzi są dodawane do projektanta.</span><span class="sxs-lookup"><span data-stu-id="93611-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="93611-236">Te kontrolki są powiązane z BindingSource (**categoryBindingSource**) i powiązanie Navigator (**categoryBindingNavigator**) składników, które są również tworzone.</span><span class="sxs-lookup"><span data-stu-id="93611-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="93611-237">Edytuj kolumny w **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="93611-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="93611-238">Chcemy ustawić **CategoryId** kolumny tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="93611-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="93611-239">Wartość **CategoryId** właściwości został wygenerowany przez bazę danych, po oszczędzamy danych.</span><span class="sxs-lookup"><span data-stu-id="93611-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="93611-240">Kliknij prawym przyciskiem myszy formant DataGridView i wybierz pozycję Edytuj kolumny...</span><span class="sxs-lookup"><span data-stu-id="93611-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="93611-241">Wybierz kolumnę CategoryId i wartość True tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="93611-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="93611-242">Naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="93611-242">Press OK</span></span>
-   <span data-ttu-id="93611-243">Wybierz produkty z kategorii źródła danych i przeciągnij go na formularzu.</span><span class="sxs-lookup"><span data-stu-id="93611-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="93611-244">ProductDataGridView i productBindingSource są dodawane do formularza.</span><span class="sxs-lookup"><span data-stu-id="93611-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="93611-245">Edytuj kolumny na productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="93611-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="93611-246">Chcemy ukryć kolumny CategoryId i kategorii i ustawić ProductId tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="93611-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="93611-247">Wartość właściwości ProductId jest generowany przez bazę danych, po oszczędzamy danych.</span><span class="sxs-lookup"><span data-stu-id="93611-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="93611-248">Kliknij prawym przyciskiem myszy formant DataGridView i wybierz **Edytowanie kolumn...** .</span><span class="sxs-lookup"><span data-stu-id="93611-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="93611-249">Wybierz **ProductId** kolumny oraz zestawu **tylko do odczytu** do **True**.</span><span class="sxs-lookup"><span data-stu-id="93611-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="93611-250">Wybierz **CategoryId** kolumny i naciśnij klawisz **Usuń** przycisku.</span><span class="sxs-lookup"><span data-stu-id="93611-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="93611-251">Wykonaj te czynności z **kategorii** kolumny.</span><span class="sxs-lookup"><span data-stu-id="93611-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="93611-252">Naciśnij klawisz **OK**.</span><span class="sxs-lookup"><span data-stu-id="93611-252">Press **OK**.</span></span>

    <span data-ttu-id="93611-253">Do tej pory firma Microsoft skojarzony naszych formantów DataGridView BindingSource składniki w projektancie.</span><span class="sxs-lookup"><span data-stu-id="93611-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="93611-254">W następnej sekcji dodamy kod kodu można ustawić categoryBindingSource.DataSource do kolekcji jednostek, które obecnie są śledzone przez DbContext.</span><span class="sxs-lookup"><span data-stu-id="93611-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="93611-255">Gdy przeciągnąć i porzucony produktów z kategorii, WinForms trwało Dbamy konfigurowania właściwości productsBindingSource.DataSource categoryBindingSource i productsBindingSource.DataMember właściwości produktów.</span><span class="sxs-lookup"><span data-stu-id="93611-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="93611-256">Ze względu na to powiązanie tylko produkty należące do aktualnie wybranej kategorii będą wyświetlane w productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="93611-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="93611-257">Włącz **Zapisz** przycisk na pasku nawigacji, klikając prawym przyciskiem myszy i wybierając **włączone**.</span><span class="sxs-lookup"><span data-stu-id="93611-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Formularz Form1 projektanta](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="93611-259">Dodaj program obsługi zdarzeń zapisywania przycisku przez dwukrotne kliknięcie przycisku.</span><span class="sxs-lookup"><span data-stu-id="93611-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="93611-260">Spowoduje to dodanie obsługi zdarzeń i wyświetlić kod związany z formularza.</span><span class="sxs-lookup"><span data-stu-id="93611-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="93611-261">Kod **categoryBindingNavigatorSaveItem\_kliknij** programu obsługi zdarzeń zostanie dodana w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="93611-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="93611-262">Dodaj kod, który obsługuje interakcji z danych</span><span class="sxs-lookup"><span data-stu-id="93611-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="93611-263">Teraz dodamy kod, aby użyć ProductContext przeprowadzić dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="93611-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="93611-264">Aktualizowanie kodu dla okna głównego formularza, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="93611-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="93611-265">Ten kod deklaruje długotrwałych wystąpienie ProductContext.</span><span class="sxs-lookup"><span data-stu-id="93611-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="93611-266">Obiekt ProductContext jest używany do wykonywania zapytań i zapisać dane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="93611-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="93611-267">Metoda Dispose() w wystąpieniu ProductContext jest następnie wywoływana z przeciążonej OnClosing.</span><span class="sxs-lookup"><span data-stu-id="93611-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="93611-268">Komentarze w kodzie zawierają szczegółowe informacje o dany kod realizuje.</span><span class="sxs-lookup"><span data-stu-id="93611-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="93611-269">Testowanie aplikacji Windows Forms</span><span class="sxs-lookup"><span data-stu-id="93611-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="93611-270">Kompiluj i uruchom aplikację i przetestowania funkcji.</span><span class="sxs-lookup"><span data-stu-id="93611-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="93611-272">Po zapisaniu wygenerowane klucze są wyświetlane na ekranie.</span><span class="sxs-lookup"><span data-stu-id="93611-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="93611-274">Jeśli została użyta Code First, to będzie również informacja, że **WinFormswithEFSample.ProductContext** baza danych została utworzona dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="93611-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)

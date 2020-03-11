---
title: Wiązanie danych z WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419604"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="5704e-102">Wiązanie danych z WinForms</span><span class="sxs-lookup"><span data-stu-id="5704e-102">Databinding with WinForms</span></span>
<span data-ttu-id="5704e-103">W tym przewodniku krok po kroku pokazano, jak powiązać typy POCO z kontrolkami formularzy okien (WinForms) w formularzu "wzorzec-szczegóły".</span><span class="sxs-lookup"><span data-stu-id="5704e-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="5704e-104">Aplikacja używa Entity Framework do wypełniania obiektów danymi z bazy danych, śledzenia zmian i utrwalania danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="5704e-105">Model definiuje dwa typy, które uczestniczą w relacji jeden do wielu: Kategoria (główna\\główna) i produkt (zależne\\szczegóły).</span><span class="sxs-lookup"><span data-stu-id="5704e-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="5704e-106">Następnie narzędzia programu Visual Studio są używane do powiązania typów zdefiniowanych w modelu z kontrolkami WinForms.</span><span class="sxs-lookup"><span data-stu-id="5704e-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="5704e-107">Środowisko WinForms — powiązania danych umożliwia nawigowanie między obiektami pokrewnymi: wybranie wierszy w widoku wzorca powoduje aktualizację widoku szczegółów z odpowiednimi danymi podrzędnymi.</span><span class="sxs-lookup"><span data-stu-id="5704e-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="5704e-108">Zrzuty ekranu i listy kodu w tym instruktażu są pobierane z Visual Studio 2013 ale można wykonać ten Instruktaż w programie Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5704e-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="5704e-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5704e-109">Pre-Requisites</span></span>

<span data-ttu-id="5704e-110">Aby ukończyć ten przewodnik, musisz mieć zainstalowany Visual Studio 2013, Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5704e-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="5704e-111">W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="5704e-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="5704e-112">Aby uzyskać więcej informacji, zobacz [Instalowanie programu NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="5704e-112">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="5704e-113">Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5704e-113">Create the Application</span></span>

-   <span data-ttu-id="5704e-114">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5704e-114">Open Visual Studio</span></span>
-   <span data-ttu-id="5704e-115">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="5704e-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="5704e-116">Wybierz pozycję **Windows** w lewym okienku i **FormsApplication Windows** w okienku po prawej stronie</span><span class="sxs-lookup"><span data-stu-id="5704e-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="5704e-117">Wprowadź **WinFormswithEFSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="5704e-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="5704e-118">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="5704e-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="5704e-119">Zainstaluj pakiet NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5704e-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="5704e-120">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="5704e-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="5704e-121">Wybierz pozycję **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="5704e-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="5704e-122">W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="5704e-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="5704e-123">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="5704e-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="5704e-124">Oprócz zestawu EntityFramework dodawane jest również odwołanie do elementu System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="5704e-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="5704e-125">Jeśli projekt zawiera odwołanie do System. Data. Entity, zostanie usunięte po zainstalowaniu pakietu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="5704e-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="5704e-126">Zestaw system. Data. Entity nie jest już używany w przypadku aplikacji Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5704e-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="5704e-127">Implementowanie elementu IListSource dla kolekcji</span><span class="sxs-lookup"><span data-stu-id="5704e-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="5704e-128">Właściwości kolekcji muszą implementować interfejs IListSource, aby umożliwić dwukierunkowe wiązanie danych z sortowaniem podczas korzystania z Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="5704e-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="5704e-129">Aby to zrobić, możemy zwiększyć ObservableCollection, aby dodać funkcje IListSource.</span><span class="sxs-lookup"><span data-stu-id="5704e-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="5704e-130">Dodaj klasę **ObservableListSource** do projektu:</span><span class="sxs-lookup"><span data-stu-id="5704e-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="5704e-131">Kliknij prawym przyciskiem myszy nazwę projektu</span><span class="sxs-lookup"><span data-stu-id="5704e-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="5704e-132">Wybierz pozycję **Dodaj-&gt; nowy element**</span><span class="sxs-lookup"><span data-stu-id="5704e-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="5704e-133">Wybierz **klasę** i wprowadź **ObservableListSource** dla nazwy klasy</span><span class="sxs-lookup"><span data-stu-id="5704e-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="5704e-134">Zastąp kod wygenerowany domyślnie następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5704e-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="5704e-135">*Ta klasa umożliwia dwukierunkowe powiązanie danych oraz sortowanie. Klasa pochodzi z ObservableCollection&lt;T&gt; i dodaje jawną implementację IListSource. Metoda GetList () IListSource jest zaimplementowana w celu zwrócenia implementacji IBindingList, która pozostaje w synchronizacji z ObservableCollection. Implementacja IBindingList wygenerowana przez ToBindingList obsługuje sortowanie. Metoda rozszerzenia ToBindingList jest zdefiniowana w zestawie EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="5704e-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

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
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="5704e-136">Zdefiniuj model</span><span class="sxs-lookup"><span data-stu-id="5704e-136">Define a Model</span></span>

<span data-ttu-id="5704e-137">W tym instruktażu możesz wybrać wdrożenie modelu przy użyciu Code First lub programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="5704e-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="5704e-138">Wykonaj jedną z dwóch poniższych sekcji.</span><span class="sxs-lookup"><span data-stu-id="5704e-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="5704e-139">Opcja 1: definiowanie modelu przy użyciu Code First</span><span class="sxs-lookup"><span data-stu-id="5704e-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="5704e-140">W tej sekcji pokazano, jak utworzyć model i skojarzoną z nim bazę danych przy użyciu Code First.</span><span class="sxs-lookup"><span data-stu-id="5704e-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="5704e-141">Przejdź do następnej sekcji (**Opcja 2: zdefiniuj model przy użyciu Database First)** , jeśli wolisz używać Database First do odtwarzania modelu z bazy danych przy użyciu narzędzia Dr Designer</span><span class="sxs-lookup"><span data-stu-id="5704e-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="5704e-142">W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny).</span><span class="sxs-lookup"><span data-stu-id="5704e-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="5704e-143">Dodaj nową klasę **produktu** do projektu</span><span class="sxs-lookup"><span data-stu-id="5704e-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="5704e-144">Zastąp kod wygenerowany domyślnie następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5704e-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="5704e-145">Dodaj klasę **kategorii** do projektu.</span><span class="sxs-lookup"><span data-stu-id="5704e-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="5704e-146">Zastąp kod wygenerowany domyślnie następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5704e-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="5704e-147">Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z **DbContext** i uwidacznia **nieogólnymi&lt;&gt;** właściwości.</span><span class="sxs-lookup"><span data-stu-id="5704e-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="5704e-148">Właściwości **nieogólnymi** umożliwiają kontekstowi znać typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="5704e-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="5704e-149">Typy **DbContext** i **nieogólnymi** są zdefiniowane w zestawie EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="5704e-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="5704e-150">Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="5704e-151">Dodaj nową klasę **ProductContext** do projektu.</span><span class="sxs-lookup"><span data-stu-id="5704e-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="5704e-152">Zastąp kod wygenerowany domyślnie następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5704e-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="5704e-153">Kompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="5704e-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="5704e-154">Opcja 2: definiowanie modelu przy użyciu Database First</span><span class="sxs-lookup"><span data-stu-id="5704e-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="5704e-155">W tej sekcji pokazano, jak używać programu Database First, aby odtworzyć model z bazy danych przy użyciu narzędzia Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="5704e-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="5704e-156">Jeśli poprzednia sekcja została ukończona (**Opcja 1: zdefiniuj model przy użyciu Code First)** , Pomiń tę sekcję i przejdź bezpośrednio do sekcji **ładowanie z opóźnieniem** .</span><span class="sxs-lookup"><span data-stu-id="5704e-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="5704e-157">Tworzenie istniejącej bazy danych</span><span class="sxs-lookup"><span data-stu-id="5704e-157">Create an Existing Database</span></span>

<span data-ttu-id="5704e-158">Zwykle w przypadku określania docelowej istniejącej bazy danych zostanie ona już utworzona, ale w tym instruktażu musimy utworzyć bazę danych do uzyskania dostępu.</span><span class="sxs-lookup"><span data-stu-id="5704e-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="5704e-159">Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5704e-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="5704e-160">Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.</span><span class="sxs-lookup"><span data-stu-id="5704e-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="5704e-161">Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="5704e-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="5704e-162">Przyjrzyjmy się i wygenerujemy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="5704e-163">**Widok-&gt; Eksplorator serwera**</span><span class="sxs-lookup"><span data-stu-id="5704e-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="5704e-164">Kliknij prawym przyciskiem myszy pozycję **połączenia danych —&gt; Dodaj połączenie...**</span><span class="sxs-lookup"><span data-stu-id="5704e-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="5704e-165">Jeśli nie masz połączenia z bazą danych Eksplorator serwera przed wybraniem Microsoft SQL Server jako źródła danych</span><span class="sxs-lookup"><span data-stu-id="5704e-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Zmień źródło danych](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="5704e-167">Połącz się z usługą LocalDB lub SQL Express, w zależności od tego, który został zainstalowany, i wprowadź **produkty** jako nazwę bazy danych</span><span class="sxs-lookup"><span data-stu-id="5704e-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Dodaj LocalDB połączenia](~/ef6/media/addconnectionlocaldb.png)

    ![Dodaj połączenie ekspresowe](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="5704e-170">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="5704e-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Create Database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="5704e-172">Nowa baza danych będzie teraz wyświetlana w Eksplorator serwera, kliknij ją prawym przyciskiem myszy, a następnie wybierz pozycję **nowe zapytanie** .</span><span class="sxs-lookup"><span data-stu-id="5704e-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="5704e-173">Skopiuj następujące SQL do nowego zapytania, a następnie kliknij prawym przyciskiem myszy zapytanie i wybierz polecenie **Execute (wykonaj** ).</span><span class="sxs-lookup"><span data-stu-id="5704e-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="5704e-174">Odtwórz Model</span><span class="sxs-lookup"><span data-stu-id="5704e-174">Reverse Engineer Model</span></span>

<span data-ttu-id="5704e-175">Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="5704e-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="5704e-176">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="5704e-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="5704e-177">Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="5704e-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="5704e-178">Wprowadź **ProductModel** jako nazwę, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="5704e-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="5704e-179">Spowoduje to uruchomienie **kreatora Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="5704e-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="5704e-180">Wybierz pozycję **Generuj z bazy danych** i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="5704e-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="5704e-182">Wybierz połączenie z bazą danych utworzoną w pierwszej sekcji, wprowadź **ProductContext** jako nazwę parametrów połączenia i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="5704e-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Wybierz połączenie](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="5704e-184">Kliknij pole wyboru obok pozycji "tabele", aby zaimportować wszystkie tabele, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="5704e-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Wybieranie obiektów](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="5704e-186">Po zakończeniu procesu odtwarzania nowy model zostanie dodany do projektu i otwarty do wyświetlania w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="5704e-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="5704e-187">Plik App. config został również dodany do projektu i zawiera szczegółowe informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="5704e-188">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="5704e-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="5704e-189">Jeśli pracujesz w programie Visual Studio 2010, musisz zaktualizować projektanta EF, aby korzystał z generowania kodu EF6.</span><span class="sxs-lookup"><span data-stu-id="5704e-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="5704e-190">Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="5704e-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="5704e-191">Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**</span><span class="sxs-lookup"><span data-stu-id="5704e-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="5704e-192">Wybierz opcję **Dr 6. x DbContext generator dla C\#,** wprowadź **ProductsModel** jako nazwę, a następnie kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="5704e-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="5704e-193">Aktualizowanie generowania kodu dla powiązania danych</span><span class="sxs-lookup"><span data-stu-id="5704e-193">Updating code generation for data binding</span></span>

<span data-ttu-id="5704e-194">EF generuje kod z modelu przy użyciu szablonów T4.</span><span class="sxs-lookup"><span data-stu-id="5704e-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="5704e-195">Szablony dostarczane z programem Visual Studio lub pobrane z galerii programu Visual Studio są przeznaczone do ogólnego użycia.</span><span class="sxs-lookup"><span data-stu-id="5704e-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="5704e-196">Oznacza to, że jednostki wygenerowane na podstawie tych szablonów mają proste właściwości ICollection&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="5704e-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="5704e-197">Jednak podczas tworzenia powiązania danych wskazane jest posiadanie właściwości kolekcji, które implementują IListSource.</span><span class="sxs-lookup"><span data-stu-id="5704e-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="5704e-198">Dlatego utworzyliśmy ObservableListSource klasy powyżej i teraz zmodyfikujemy szablony, aby użyć tej klasy.</span><span class="sxs-lookup"><span data-stu-id="5704e-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="5704e-199">Otwórz **Eksplorator rozwiązań** i Znajdź plik **ProductModel. edmx**</span><span class="sxs-lookup"><span data-stu-id="5704e-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="5704e-200">Znajdź plik **ProductModel.tt** , który zostanie zagnieżdżony w pliku ProductModel. edmx</span><span class="sxs-lookup"><span data-stu-id="5704e-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Szablon modelu produktu](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="5704e-202">Kliknij dwukrotnie plik ProductModel.tt, aby otworzyć go w edytorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5704e-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="5704e-203">Znajdź i Zamień dwa wystąpienia elementu "**ICollection**" na "**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="5704e-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="5704e-204">Znajdują się one w około wierszach 296 i 484.</span><span class="sxs-lookup"><span data-stu-id="5704e-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="5704e-205">Znajdź i Zamień pierwsze wystąpienie elementu "**HashSet —** " na "**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="5704e-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="5704e-206">To wystąpienie znajduje się w około wiersz 50.</span><span class="sxs-lookup"><span data-stu-id="5704e-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="5704e-207">**Nie** zamieniaj drugiego wystąpienia HashSet — znalezionego w dalszej części kodu.</span><span class="sxs-lookup"><span data-stu-id="5704e-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="5704e-208">Zapisz plik ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="5704e-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="5704e-209">Powinno to spowodować, że kod dla jednostek zostanie ponownie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="5704e-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="5704e-210">Jeśli kod nie zostanie wygenerowany automatycznie, kliknij prawym przyciskiem myszy pozycję ProductModel.tt i wybierz polecenie "Uruchom narzędzie niestandardowe".</span><span class="sxs-lookup"><span data-stu-id="5704e-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="5704e-211">Jeśli teraz otworzysz plik Category.cs (który jest zagnieżdżony w obszarze ProductModel.tt), powinna zostać wyświetlona, że kolekcja Products ma typ **ObservableListSource&lt;produktu&gt;** .</span><span class="sxs-lookup"><span data-stu-id="5704e-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="5704e-212">Kompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="5704e-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="5704e-213">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="5704e-213">Lazy Loading</span></span>

<span data-ttu-id="5704e-214">Właściwości **Products** klasy **Category** i **Category** klasy **Product** są właściwościami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="5704e-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="5704e-215">W Entity Framework właściwości nawigacji umożliwiają nawigowanie po relacjach między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="5704e-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="5704e-216">EF oferuje opcję ładowania powiązanych jednostek z bazy danych automatycznie przy pierwszym dostępie do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="5704e-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="5704e-217">W przypadku tego typu ładowania (nazywanego ładowaniem opóźnionym) należy pamiętać, że podczas pierwszego uzyskiwania dostępu do każdej właściwości nawigacji oddzielne zapytanie zostanie wykonane względem bazy danych, jeśli zawartość nie jest jeszcze w kontekście.</span><span class="sxs-lookup"><span data-stu-id="5704e-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="5704e-218">W przypadku korzystania z typów jednostek POCO EF osiąga opóźnione ładowanie przez utworzenie wystąpień pochodnych typów proxy podczas wykonywania, a następnie Zastępowanie właściwości wirtualnych w klasach, aby dodać punkt zaczepienia ładowania.</span><span class="sxs-lookup"><span data-stu-id="5704e-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="5704e-219">Aby uzyskać opóźnione ładowanie pokrewnych obiektów, należy zadeklarować metody do pobierania właściwości nawigacji jako **publiczne** i **wirtualne** (Zastąp w Visual Basic), a Klasa nie może być **zapieczętowana** **(** **NotOverridable** w Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="5704e-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="5704e-220">Przy użyciu Database First właściwości nawigacji są automatycznie wprowadzane do wirtualnego, aby umożliwić ładowanie z opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="5704e-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="5704e-221">W sekcji Code First wybrano, aby właściwości nawigacji były wirtualne z tego samego powodu</span><span class="sxs-lookup"><span data-stu-id="5704e-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="5704e-222">Powiąż obiekt z kontrolkami</span><span class="sxs-lookup"><span data-stu-id="5704e-222">Bind Object to Controls</span></span>

<span data-ttu-id="5704e-223">Dodaj klasy, które są zdefiniowane w modelu jako źródła danych dla tej aplikacji WinForms.</span><span class="sxs-lookup"><span data-stu-id="5704e-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="5704e-224">Z menu głównego wybierz pozycję **projekt&gt; Dodaj nowe źródło danych...**</span><span class="sxs-lookup"><span data-stu-id="5704e-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="5704e-225">(w programie Visual Studio 2010 musisz wybrać pozycję **dane&gt; Dodaj nowe źródło danych...** )</span><span class="sxs-lookup"><span data-stu-id="5704e-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="5704e-226">W oknie Wybierz typ źródła danych wybierz pozycję **obiekt** i kliknij przycisk **dalej** .</span><span class="sxs-lookup"><span data-stu-id="5704e-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="5704e-227">W oknie dialogowym Wybieranie obiektów danych unfold **WinFormswithEFSample** dwa razy i wybierz **kategorię** nie ma potrzeby wybierania źródła danych produktu, ponieważ zostanie on przechodzący przez właściwość produktu w źródle danych kategorii.</span><span class="sxs-lookup"><span data-stu-id="5704e-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Źródło danych](~/ef6/media/datasource.png)

-   <span data-ttu-id="5704e-229">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="5704e-229">Click **Finish.**</span></span>
    <span data-ttu-id="5704e-230">Jeśli okno źródła danych nie jest wyświetlane, wybierz pozycję **wyświetl&gt; inne&gt; źródła danych systemu Windows**</span><span class="sxs-lookup"><span data-stu-id="5704e-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="5704e-231">Naciśnij ikonę pinezki, aby okno źródła danych nie było ukrywane.</span><span class="sxs-lookup"><span data-stu-id="5704e-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="5704e-232">Może być konieczne kliknięcie przycisku Odśwież, jeśli okno było już widoczne.</span><span class="sxs-lookup"><span data-stu-id="5704e-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Źródło danych 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="5704e-234">W Eksplorator rozwiązań kliknij dwukrotnie plik **Form1.cs** , aby otworzyć formularz główny w projektancie.</span><span class="sxs-lookup"><span data-stu-id="5704e-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="5704e-235">Wybierz **kategorię** źródło danych i przeciągnij ją na formularz.</span><span class="sxs-lookup"><span data-stu-id="5704e-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="5704e-236">Domyślnie do projektanta dodawane są nowe kontrolki paska narzędzi DataGridView (**categoryDataGridView**) i nawigacyjnego.</span><span class="sxs-lookup"><span data-stu-id="5704e-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="5704e-237">Te kontrolki są powiązane z utworzonymi również składnikami BindingSource (**categoryBindingSource**) i nawigatora powiązań (**categoryBindingNavigator**).</span><span class="sxs-lookup"><span data-stu-id="5704e-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="5704e-238">Edytuj kolumny w **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="5704e-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="5704e-239">Chcemy ustawić kolumnę **IDkategorii** na tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="5704e-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="5704e-240">Wartość właściwości **IDkategorii** jest generowana przez bazę danych po zapisaniu danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="5704e-241">Kliknij prawym przyciskiem myszy formant DataGridView i wybierz polecenie Edytuj kolumny...</span><span class="sxs-lookup"><span data-stu-id="5704e-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="5704e-242">Wybierz kolumnę IDKategorii i ustaw wartość tylko do true</span><span class="sxs-lookup"><span data-stu-id="5704e-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="5704e-243">Naciśnij przycisk OK</span><span class="sxs-lookup"><span data-stu-id="5704e-243">Press OK</span></span>
-   <span data-ttu-id="5704e-244">Wybierz pozycję produkty z kategorii źródło danych i przeciągnij ją w formularzu.</span><span class="sxs-lookup"><span data-stu-id="5704e-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="5704e-245">ProductDataGridView i productBindingSource są dodawane do formularza.</span><span class="sxs-lookup"><span data-stu-id="5704e-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="5704e-246">Edytuj kolumny w productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="5704e-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="5704e-247">Chcemy ukryć IDKategorii i kolumny kategorii i ustawić ProductId jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="5704e-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="5704e-248">Wartość właściwości ProductId jest generowana przez bazę danych po zapisaniu danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="5704e-249">Kliknij prawym przyciskiem myszy formant DataGridView i wybierz polecenie **Edytuj kolumny...** .</span><span class="sxs-lookup"><span data-stu-id="5704e-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="5704e-250">Wybierz kolumnę **ProductID** i dla opcji **ReadOnly** Ustaw **wartość true**.</span><span class="sxs-lookup"><span data-stu-id="5704e-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="5704e-251">Wybierz kolumnę **IDkategorii** i naciśnij przycisk **Usuń** .</span><span class="sxs-lookup"><span data-stu-id="5704e-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="5704e-252">Wykonaj te same czynności z kolumną **Category** .</span><span class="sxs-lookup"><span data-stu-id="5704e-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="5704e-253">Naciśnij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="5704e-253">Press **OK**.</span></span>

    <span data-ttu-id="5704e-254">Do tej pory skojarzenie formantów DataGridView ze składnikami BindingSource w projektancie.</span><span class="sxs-lookup"><span data-stu-id="5704e-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="5704e-255">W następnej sekcji dodamy kod do kodu w celu ustawienia categoryBindingSource. DataSource do kolekcji jednostek, które są aktualnie śledzone przez DbContext.</span><span class="sxs-lookup"><span data-stu-id="5704e-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="5704e-256">Podczas przeciągania i upuszczania produktów z poziomu kategorii WinForms brały pod uwagę skonfigurowanie właściwości productsBindingSource. DataSource na wartość categoryBindingSource i productsBindingSource. DataMember dla produktów.</span><span class="sxs-lookup"><span data-stu-id="5704e-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="5704e-257">Ze względu na to powiązanie tylko produkty należące do aktualnie wybranej kategorii będą wyświetlane w productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="5704e-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="5704e-258">Włącz przycisk **Zapisz** na pasku narzędzi nawigacji, klikając prawym przyciskiem myszy i wybierając pozycję **włączone**.</span><span class="sxs-lookup"><span data-stu-id="5704e-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Projektant formularza 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="5704e-260">Dodaj program obsługi zdarzeń dla przycisku Zapisz przez dwukrotne kliknięcie przycisku.</span><span class="sxs-lookup"><span data-stu-id="5704e-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="5704e-261">Spowoduje to dodanie obsługi zdarzeń i przełączenie do kodu powiązanego z formularzem.</span><span class="sxs-lookup"><span data-stu-id="5704e-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="5704e-262">Kod elementu **categoryBindingNavigatorSaveItem\_kliknij** procedurę obsługi zdarzeń, która zostanie dodana w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="5704e-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="5704e-263">Dodawanie kodu, który obsługuje interakcję z danymi</span><span class="sxs-lookup"><span data-stu-id="5704e-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="5704e-264">Teraz dodamy kod do korzystania z ProductContext w celu uzyskania dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="5704e-265">Zaktualizuj kod dla głównego okna formularza, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="5704e-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="5704e-266">Kod deklaruje długotrwałe wystąpienie ProductContext.</span><span class="sxs-lookup"><span data-stu-id="5704e-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="5704e-267">Obiekt ProductContext jest używany do wykonywania zapytań i zapisywania danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5704e-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="5704e-268">Metoda Dispose () w wystąpieniu ProductContext jest następnie wywoływana z przesłoniętej metody onzamykającej.</span><span class="sxs-lookup"><span data-stu-id="5704e-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="5704e-269">Komentarze do kodu zawierają szczegółowe informacje o tym, co robi kod.</span><span class="sxs-lookup"><span data-stu-id="5704e-269">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="5704e-270">Testowanie aplikacji Windows Forms</span><span class="sxs-lookup"><span data-stu-id="5704e-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="5704e-271">Skompiluj i uruchom aplikację, a następnie możesz przetestować jej działanie.</span><span class="sxs-lookup"><span data-stu-id="5704e-271">Compile and run the application and you can test out the functionality.</span></span>

    ![Formularz 1 przed zapisaniem](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="5704e-273">Po zapisaniu kluczy generowanych przez magazyn są wyświetlane na ekranie.</span><span class="sxs-lookup"><span data-stu-id="5704e-273">After saving the store generated keys are shown on the screen.</span></span>

    ![Formularz 1 po zapisaniu](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="5704e-275">Jeśli używasz Code First, zobaczysz również, że została utworzona baza danych **WinFormswithEFSample. ProductContext** .</span><span class="sxs-lookup"><span data-stu-id="5704e-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Eksplorator obiektów serwera](~/ef6/media/serverobjexplorer.png)

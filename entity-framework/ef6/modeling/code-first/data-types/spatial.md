---
title: Przestrzenny Code First EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182649"
---
# <a name="spatial---code-first"></a><span data-ttu-id="cf6dd-102">Code First przestrzenne</span><span class="sxs-lookup"><span data-stu-id="cf6dd-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="cf6dd-103">**EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="cf6dd-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="cf6dd-105">Film wideo i przewodnik krok po kroku przedstawiają sposób mapowania typów przestrzennych przy użyciu Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="cf6dd-106">Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="cf6dd-107">Ten Instruktaż będzie używać Code First do tworzenia nowej bazy danych, ale można również użyć [Code First do istniejącej bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="cf6dd-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="cf6dd-108">Obsługa typów przestrzennych została wprowadzona w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="cf6dd-109">Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzenny, wyliczenia i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="cf6dd-110">Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="cf6dd-111">Aby można było używać typów danych przestrzennych, należy również użyć dostawcy Entity Framework z obsługą przestrzenną.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="cf6dd-112">Aby uzyskać więcej informacji, zobacz [Obsługa dostawcy dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="cf6dd-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="cf6dd-113">Istnieją dwa główne typy danych przestrzennych: Geografia i geometria.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="cf6dd-114">Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne i Długość geograficzna).</span><span class="sxs-lookup"><span data-stu-id="cf6dd-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="cf6dd-115">Typ danych geometrii reprezentuje układ współrzędnych Euclidean (płaski).</span><span class="sxs-lookup"><span data-stu-id="cf6dd-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="cf6dd-116">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="cf6dd-116">Watch the video</span></span>
<span data-ttu-id="cf6dd-117">W tym filmie wideo pokazano, jak mapować typy przestrzenne za pomocą Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="cf6dd-118">Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="cf6dd-119">**Przedstawione przez**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="cf6dd-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="cf6dd-120">**Film wideo**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="cf6dd-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="cf6dd-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cf6dd-121">Pre-Requisites</span></span>

<span data-ttu-id="cf6dd-122">Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cf6dd-123">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="cf6dd-123">Set up the Project</span></span>

1.  <span data-ttu-id="cf6dd-124">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="cf6dd-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="cf6dd-125">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .</span><span class="sxs-lookup"><span data-stu-id="cf6dd-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="cf6dd-126">W lewym okienku kliknij pozycję **Visual C @ no__t-1**, a następnie wybierz szablon **konsoli**</span><span class="sxs-lookup"><span data-stu-id="cf6dd-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="cf6dd-127">Wprowadź **SpatialCodeFirst** jako nazwę projektu, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="cf6dd-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="cf6dd-128">Zdefiniuj nowy model przy użyciu Code First</span><span class="sxs-lookup"><span data-stu-id="cf6dd-128">Define a New Model using Code First</span></span>

<span data-ttu-id="cf6dd-129">W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny).</span><span class="sxs-lookup"><span data-stu-id="cf6dd-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="cf6dd-130">Poniższy kod definiuje klasę University.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-130">The code below defines the University class.</span></span>

<span data-ttu-id="cf6dd-131">Uniwersytet ma właściwość Location typu DbGeography.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="cf6dd-132">Aby użyć typu DbGeography, należy dodać odwołanie do zestawu System. Data. Entity, a także dodać instrukcję system. Data. przestrzenny using.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="cf6dd-133">Otwórz plik Program.cs i wklej następujące instrukcje using na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="cf6dd-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="cf6dd-134">Dodaj następującą definicję klasy University do pliku Program.cs.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="cf6dd-135">Zdefiniuj typ pochodny DbContext</span><span class="sxs-lookup"><span data-stu-id="cf6dd-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="cf6dd-136">Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z DbContext i uwidacznia Nieogólnymi @ no__t-0TEntity @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="cf6dd-137">Właściwości Nieogólnymi @ no__t-0TEntity @ no__t-1 pozwalają kontekstowi znać, które typy mają być uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="cf6dd-138">Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="cf6dd-139">Typy DbContext i Nieogólnymi są zdefiniowane w zestawie EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="cf6dd-140">Dodamy odwołanie do tej biblioteki DLL przy użyciu pakietu NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="cf6dd-141">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="cf6dd-142">Wybierz pozycję **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="cf6dd-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="cf6dd-143">W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="cf6dd-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="cf6dd-144">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="cf6dd-144">Click **Install**</span></span>

<span data-ttu-id="cf6dd-145">Należy zauważyć, że oprócz zestawu EntityFramework dodawane jest również odwołanie do zestawu System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="cf6dd-146">W górnej części pliku Program.cs Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="cf6dd-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="cf6dd-147">W Program.cs Dodaj definicję kontekstu.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="cf6dd-148">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="cf6dd-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="cf6dd-149">Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="cf6dd-150">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="cf6dd-151">Kod dodaje dwa nowe obiekty University do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="cf6dd-152">Właściwości przestrzenne są inicjowane przy użyciu metody DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="cf6dd-153">Punkt geografii reprezentowany jako WellKnownText jest przenoszona do metody.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="cf6dd-154">Następnie kod zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-154">The code then saves the data.</span></span> <span data-ttu-id="cf6dd-155">Następnie zapytanie LINQ, które zwraca obiekt University, gdzie jego lokalizacja znajduje się najbliżej określonej lokalizacji, jest konstruowany i wykonywany.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="cf6dd-156">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-156">Compile and run the application.</span></span> <span data-ttu-id="cf6dd-157">Program tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cf6dd-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="cf6dd-158">Wyświetlanie wygenerowanej bazy danych</span><span class="sxs-lookup"><span data-stu-id="cf6dd-158">View the Generated Database</span></span>

<span data-ttu-id="cf6dd-159">Po pierwszym uruchomieniu aplikacji Entity Framework tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="cf6dd-160">Ze względu na to, że zainstalowano program Visual Studio 2012, baza danych zostanie utworzona w wystąpieniu LocalDB.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="cf6dd-161">Domyślnie Entity Framework nazw bazy danych po w pełni kwalifikowana nazwa kontekstu pochodnego (w tym przykładzie jest to **SpatialCodeFirst. UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="cf6dd-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="cf6dd-162">Kolejne użycie istniejącej bazy danych będzie możliwe.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="cf6dd-163">Należy pamiętać, że jeśli wprowadzisz zmiany w modelu po utworzeniu bazy danych, należy użyć Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="cf6dd-164">Zapoznaj się z tematem [Code First do nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) , aby zapoznać się z przykładem użycia migracji.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="cf6dd-165">Aby wyświetlić bazę danych i dane, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="cf6dd-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="cf6dd-166">W menu głównym programu Visual Studio 2012 wybierz pozycję **wyświetl** - @ no__t-2 **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="cf6dd-167">Jeśli LocalDB nie znajduje się na liście serwerów, kliknij prawym przyciskiem myszy na **SQL Server** i wybierz polecenie **Dodaj SQL Server** Użyj domyślnego **uwierzytelniania systemu Windows** , aby nawiązać połączenie z wystąpieniem LocalDB</span><span class="sxs-lookup"><span data-stu-id="cf6dd-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="cf6dd-168">Rozwiń węzeł LocalDB</span><span class="sxs-lookup"><span data-stu-id="cf6dd-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="cf6dd-169">Unfold folder **baz danych** w celu wyświetlenia nowej bazy danych i przechodzenia do tabeli **uniwersytetów**</span><span class="sxs-lookup"><span data-stu-id="cf6dd-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="cf6dd-170">Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**</span><span class="sxs-lookup"><span data-stu-id="cf6dd-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="cf6dd-171">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="cf6dd-171">Summary</span></span>

<span data-ttu-id="cf6dd-172">W tym instruktażu przedstawiono sposób używania typów przestrzennych z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="cf6dd-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 

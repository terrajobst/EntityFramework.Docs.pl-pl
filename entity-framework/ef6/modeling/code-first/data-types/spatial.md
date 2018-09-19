---
title: Przestrzenne — najpierw - Code EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: b8f2485a7002dcb4079f4045432f3224123fdb2f
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283605"
---
# <a name="spatial---code-first"></a><span data-ttu-id="76924-102">Przestrzenne - Code pierwszy</span><span class="sxs-lookup"><span data-stu-id="76924-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="76924-103">**EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="76924-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="76924-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="76924-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="76924-105">Przewodnik krok po kroku i wideo pokazuje, jak do mapowania typów przestrzennych z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="76924-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="76924-106">Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="76924-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="76924-107">W tym przewodniku będzie używać Code First, aby utworzyć nową bazę danych, ale można również użyć [Code First istniejącą bazę danych](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="76924-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="76924-108">Obsługa przestrzenne typu została wprowadzona w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="76924-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="76924-109">Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzennych, wyliczeń i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="76924-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="76924-110">Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.</span><span class="sxs-lookup"><span data-stu-id="76924-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="76924-111">Używanie typów danych przestrzennych, należy również użyć dostawcy środowiska Entity Framework, który obsługuje przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="76924-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="76924-112">Zobacz [Obsługa dostawców dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="76924-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="76924-113">Istnieją dwa typy danych przestrzennych głównego: geometry i położenia geograficznego.</span><span class="sxs-lookup"><span data-stu-id="76924-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="76924-114">Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne GPS koordynuje).</span><span class="sxs-lookup"><span data-stu-id="76924-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="76924-115">Typ danych Geometria reprezentuje euklidesowa współrzędnych (płaski).</span><span class="sxs-lookup"><span data-stu-id="76924-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="76924-116">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="76924-116">Watch the video</span></span>
<span data-ttu-id="76924-117">To wideo pokazuje, jak do mapowania typów przestrzennych z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="76924-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="76924-118">Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="76924-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="76924-119">**Osoba prezentująca**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="76924-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="76924-120">**Film wideo**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="76924-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="76924-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="76924-121">Pre-Requisites</span></span>

<span data-ttu-id="76924-122">Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="76924-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="76924-123">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="76924-123">Set up the Project</span></span>

1.  <span data-ttu-id="76924-124">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="76924-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="76924-125">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**</span><span class="sxs-lookup"><span data-stu-id="76924-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="76924-126">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu</span><span class="sxs-lookup"><span data-stu-id="76924-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="76924-127">Wprowadź **SpatialCodeFirst** jako nazwę projektu i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="76924-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="76924-128">Definiowanie nowego modelu za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="76924-128">Define a New Model using Code First</span></span>

<span data-ttu-id="76924-129">Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).</span><span class="sxs-lookup"><span data-stu-id="76924-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="76924-130">Poniższy kod definiuje klasę University.</span><span class="sxs-lookup"><span data-stu-id="76924-130">The code below defines the University class.</span></span>

<span data-ttu-id="76924-131">Uniwersytecie ma właściwość lokalizacji, typu DbGeography.</span><span class="sxs-lookup"><span data-stu-id="76924-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="76924-132">Aby użyć typu DbGeography, należy dodać odwołanie do zestawu System.Data.Entity, a także dodać System.Data.Spatial za pomocą instrukcji.</span><span class="sxs-lookup"><span data-stu-id="76924-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="76924-133">Otwórz plik Program.cs i wklej następujące instrukcje using na początku pliku:</span><span class="sxs-lookup"><span data-stu-id="76924-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="76924-134">Dodaj następującą definicję klasy University do pliku Program.cs.</span><span class="sxs-lookup"><span data-stu-id="76924-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="76924-135">Definiowanie typu pochodnego typu DbContext</span><span class="sxs-lookup"><span data-stu-id="76924-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="76924-136">Oprócz definiowania jednostek, musisz zdefiniować klasę, która pochodzi od typu DbContext i udostępnia DbSet&lt;TEntity&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="76924-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="76924-137">DbSet&lt;TEntity&gt; właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="76924-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="76924-138">Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="76924-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="76924-139">Typy DbContext i DbSet są definiowane w zestawie platformy EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="76924-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="76924-140">Firma Microsoft doda odwołanie do tej biblioteki DLL przy użyciu pakietu EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="76924-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="76924-141">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="76924-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="76924-142">Wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="76924-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="76924-143">W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu.</span><span class="sxs-lookup"><span data-stu-id="76924-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="76924-144">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="76924-144">Click **Install**</span></span>

<span data-ttu-id="76924-145">Należy zauważyć, że oprócz zestawu platformy EntityFramework, odwołanie do zestawu System.ComponentModel.DataAnnotations jest także dodawane.</span><span class="sxs-lookup"><span data-stu-id="76924-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="76924-146">W górnej części pliku Program.cs Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="76924-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="76924-147">W pliku Program.cs Dodaj definicję kontekstu.</span><span class="sxs-lookup"><span data-stu-id="76924-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="76924-148">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="76924-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="76924-149">Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main.</span><span class="sxs-lookup"><span data-stu-id="76924-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="76924-150">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="76924-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="76924-151">Ten kod dodaje dwa nowe obiekty University do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="76924-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="76924-152">Właściwości przestrzenne są inicjowane za pomocą metody DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="76924-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="76924-153">Punkt lokalizacji geograficznej, reprezentowane jako WellKnownText jest przekazywany do metody.</span><span class="sxs-lookup"><span data-stu-id="76924-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="76924-154">Kod następnie zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="76924-154">The code then saves the data.</span></span> <span data-ttu-id="76924-155">Następnie zapytania LINQ, która zwraca obiekt University, gdzie jego lokalizacja jest najbardziej zbliżony do określonej lokalizacji jest tworzony i wykonywane.</span><span class="sxs-lookup"><span data-stu-id="76924-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="76924-156">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="76924-156">Compile and run the application.</span></span> <span data-ttu-id="76924-157">Program generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="76924-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="76924-158">Wyświetlanie wygenerowanych bazy danych</span><span class="sxs-lookup"><span data-stu-id="76924-158">View the Generated Database</span></span>

<span data-ttu-id="76924-159">Po uruchomieniu aplikacji po raz pierwszy, platformy Entity Framework tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="76924-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="76924-160">Dysponujemy programu Visual Studio 2012, baza danych zostanie utworzony w wystąpieniu programu LocalDB.</span><span class="sxs-lookup"><span data-stu-id="76924-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="76924-161">Domyślnie platforma Entity Framework nazwy bazy danych po w pełni kwalifikowanej nazwie pochodnej kontekstu (w tym przykładzie, który jest **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="76924-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="76924-162">Kolejne razy, zostanie użyta istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="76924-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="76924-163">Należy pamiętać, że jeśli wprowadzisz zmiany do modelu, po utworzeniu bazy danych, należy użyć migracje Code First do zaktualizowania schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="76924-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="76924-164">Zobacz [Code First dla nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) na przykład za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="76924-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="76924-165">Aby wyświetlić i bazy danych, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="76924-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="76924-166">W menu głównym programu Visual Studio 2012, wybierz **widoku**  - &gt; **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="76924-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="76924-167">Jeśli LocalDB, nie ma na liście serwerów, kliknij prawym przyciskiem myszy **programu SQL Server** i wybierz **dodawania serwera SQL** Użyj domyślnej **uwierzytelniania Windows** połączyć się z wystąpienia LocalDB</span><span class="sxs-lookup"><span data-stu-id="76924-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="76924-168">Rozwiń węzeł LocalDB</span><span class="sxs-lookup"><span data-stu-id="76924-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="76924-169">Unfold **baz danych** folder, aby zobaczyć nową bazę danych, a następnie przejdź do **uniwersytety** tabeli</span><span class="sxs-lookup"><span data-stu-id="76924-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="76924-170">Aby wyświetlić dane, kliknij prawym przyciskiem myszy w tabeli i wybrać **widoku danych**</span><span class="sxs-lookup"><span data-stu-id="76924-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="76924-171">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="76924-171">Summary</span></span>

<span data-ttu-id="76924-172">W tym przewodniku zobaczyliśmy, jak za pomocą typów przestrzennych programu Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="76924-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 

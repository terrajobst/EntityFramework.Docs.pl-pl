---
title: Przestrzenne EF6 - projektancie platformy EF -
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 67cc6c007a4477b650e7c4875de8fac36a9ba2be
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283761"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="27095-102">Przestrzenne - projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="27095-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="27095-103">**EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="27095-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="27095-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="27095-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="27095-105">Przewodnik krok po kroku i wideo pokazuje, jak do mapowania typów przestrzennych z programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="27095-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="27095-106">Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="27095-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="27095-107">W tym przewodniku użyje pierwszego modelu do utworzenia nowej bazy danych, ale może również służyć projektancie platformy EF z [Database First](~/ef6/modeling/designer/workflows/database-first.md) przepływu pracy do mapowania istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="27095-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="27095-108">Obsługa przestrzenne typu została wprowadzona w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="27095-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="27095-109">Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzennych, wyliczeń i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="27095-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="27095-110">Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.</span><span class="sxs-lookup"><span data-stu-id="27095-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="27095-111">Używanie typów danych przestrzennych, należy również użyć dostawcy środowiska Entity Framework, który obsługuje przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="27095-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="27095-112">Zobacz [Obsługa dostawców dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="27095-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="27095-113">Istnieją dwa typy danych przestrzennych głównego: geometry i położenia geograficznego.</span><span class="sxs-lookup"><span data-stu-id="27095-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="27095-114">Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne GPS koordynuje).</span><span class="sxs-lookup"><span data-stu-id="27095-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="27095-115">Typ danych Geometria reprezentuje euklidesowa współrzędnych (płaski).</span><span class="sxs-lookup"><span data-stu-id="27095-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="27095-116">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="27095-116">Watch the video</span></span>
<span data-ttu-id="27095-117">To wideo pokazuje, jak do mapowania typów przestrzennych z programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="27095-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="27095-118">Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="27095-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="27095-119">**Osoba prezentująca**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="27095-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="27095-120">**Film wideo**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="27095-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="27095-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="27095-121">Pre-Requisites</span></span>

<span data-ttu-id="27095-122">Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="27095-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="27095-123">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="27095-123">Set up the Project</span></span>

1.  <span data-ttu-id="27095-124">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="27095-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="27095-125">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**</span><span class="sxs-lookup"><span data-stu-id="27095-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="27095-126">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu</span><span class="sxs-lookup"><span data-stu-id="27095-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="27095-127">Wprowadź **SpatialEFDesigner** jako nazwę projektu i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="27095-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="27095-128">Utwórz nowy Model przy użyciu projektanta EF</span><span class="sxs-lookup"><span data-stu-id="27095-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="27095-129">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**</span><span class="sxs-lookup"><span data-stu-id="27095-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="27095-130">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów</span><span class="sxs-lookup"><span data-stu-id="27095-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="27095-131">Wprowadź **UniversityModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="27095-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="27095-132">Na stronie Kreator modelu Entity Data Model wybierz **pusty Model** w oknie dialogowym Wybierz zawartość modelu</span><span class="sxs-lookup"><span data-stu-id="27095-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="27095-133">Kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="27095-133">Click **Finish**</span></span>

<span data-ttu-id="27095-134">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="27095-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="27095-135">Kreator wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="27095-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="27095-136">Generuje plik EnumTestModel.edmx, który definiuje modelu koncepcyjnego, model magazynu i mapowanie między nimi.</span><span class="sxs-lookup"><span data-stu-id="27095-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="27095-137">Ustawia właściwość metadanych artefaktów przetwarzania pliku edmx osadzania w zestawie danych wyjściowych, więc pliki metadanych wygenerowanych Pobierz osadzone w zestawie.</span><span class="sxs-lookup"><span data-stu-id="27095-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="27095-138">Dodanie odwołania do następujących zestawów: platformy EntityFramework, System.ComponentModel.DataAnnotations i System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="27095-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="27095-139">Tworzy pliki UniversityModel.tt i UniversityModel.Context.tt i dodaje je w pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="27095-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="27095-140">Te pliki szablonów T4 generowania kodu, który definiuje typu DbContext pochodnych i POCO typów, które mapują jednostek w modelu edmx</span><span class="sxs-lookup"><span data-stu-id="27095-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="27095-141">Dodaj nowy typ jednostki</span><span class="sxs-lookup"><span data-stu-id="27095-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="27095-142">Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wybierz opcję **Add -&gt; jednostki**, pojawi się okno dialogowe nowej jednostki</span><span class="sxs-lookup"><span data-stu-id="27095-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="27095-143">Określ **University** dla typu nazwy i określ **UniversityID** dla danej nazwy właściwości klucza, pozostaw typ jako **Int32**</span><span class="sxs-lookup"><span data-stu-id="27095-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="27095-144">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="27095-144">Click **OK**</span></span>
4.  <span data-ttu-id="27095-145">Kliknij prawym przyciskiem myszy jednostkę, a następnie wybierz pozycję **Dodaj nowy -&gt; właściwość skalarną**</span><span class="sxs-lookup"><span data-stu-id="27095-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="27095-146">Zmień nazwę nowej właściwości **nazwy**</span><span class="sxs-lookup"><span data-stu-id="27095-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="27095-147">Dodawanie innej skalarne właściwości i zmień jej nazwę na **lokalizacji** Otwórz okno właściwości, a następnie zmień typ nowej właściwości **lokalizacji geograficznej**</span><span class="sxs-lookup"><span data-stu-id="27095-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="27095-148">Zapisz model i skompiluj projekt</span><span class="sxs-lookup"><span data-stu-id="27095-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="27095-149">W przypadku tworzenia, ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów.</span><span class="sxs-lookup"><span data-stu-id="27095-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="27095-150">Można zignorować te ostrzeżenia, ponieważ po Wybierzmy wygenerować bazę danych z modelu, błędy znikną.</span><span class="sxs-lookup"><span data-stu-id="27095-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="27095-151">Generuj bazę danych z modelu</span><span class="sxs-lookup"><span data-stu-id="27095-151">Generate Database from Model</span></span>

<span data-ttu-id="27095-152">Teraz możemy wygenerować bazę danych, która jest oparta na modelu.</span><span class="sxs-lookup"><span data-stu-id="27095-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="27095-153">Kliknij prawym przyciskiem myszy pusty obszar w Projektancie jednostki powierzchni i wybierz **Generuj z bazy danych z modelu**</span><span class="sxs-lookup"><span data-stu-id="27095-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="27095-154">Wybierz połączenie danych dialogowym Generuj Kreatora bazy danych jest wyświetlany, kliknij przycisk **nowe połączenie** przycisk Określ **(localdb)\\mssqllocaldb** dla nazwy serwera i  **Uniwersytet** bazy danych, a następnie kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="27095-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="27095-155">Okno z pytaniem, jeśli chcesz utworzyć nową bazę danych będą się pojawiać, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="27095-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="27095-156">Kliknij przycisk **dalej** i Kreatora tworzenia bazy danych generuje języka definicji danych (DDL) dla tworzenia bazy danych wygenerowanej języka DDL są wyświetlane w Podsumowanie i okna dialogowego pole Uwaga dotycząca ustawień, który DDL nie zawiera definicji dla tabelę, która mapuje do typu wyliczenia</span><span class="sxs-lookup"><span data-stu-id="27095-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="27095-157">Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie wykonuje skrypt języka DDL.</span><span class="sxs-lookup"><span data-stu-id="27095-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="27095-158">Kreator tworzenia bazy danych wykonuje następujące czynności: Otwiera **UniversityModel.edmx.sql** w Edytor T-SQL generuje sekcje schematu i mapowania magazynu EDMX pliku informacji o parametrach połączenia usług AD DS do pliku App.config</span><span class="sxs-lookup"><span data-stu-id="27095-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="27095-159">Kliknij prawym przyciskiem myszy w edytorze języka T-SQL i wybierz pozycję **Execute** nawiązać połączenie z serwerem w oknie dialogowym wyświetlony, podaj informacje o połączeniu z kroku 2, a następnie kliknij polecenie **Connect**</span><span class="sxs-lookup"><span data-stu-id="27095-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="27095-160">Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **odświeżania**</span><span class="sxs-lookup"><span data-stu-id="27095-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="27095-161">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="27095-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="27095-162">Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main.</span><span class="sxs-lookup"><span data-stu-id="27095-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="27095-163">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="27095-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="27095-164">Ten kod dodaje dwa nowe obiekty University do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="27095-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="27095-165">Właściwości przestrzenne są inicjowane za pomocą metody DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="27095-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="27095-166">Punkt lokalizacji geograficznej, reprezentowane jako WellKnownText jest przekazywany do metody.</span><span class="sxs-lookup"><span data-stu-id="27095-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="27095-167">Kod następnie zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="27095-167">The code then saves the data.</span></span> <span data-ttu-id="27095-168">Następnie zapytania LINQ, która zwraca obiekt University, gdzie jego lokalizacja jest najbardziej zbliżony do określonej lokalizacji jest tworzony i wykonywane.</span><span class="sxs-lookup"><span data-stu-id="27095-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

<span data-ttu-id="27095-169">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="27095-169">Compile and run the application.</span></span> <span data-ttu-id="27095-170">Program generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="27095-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="27095-171">Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **Odśwież**.</span><span class="sxs-lookup"><span data-stu-id="27095-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="27095-172">Kliknięcie prawym przyciskiem myszy tabelę i wybierz pozycję **dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="27095-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="27095-173">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="27095-173">Summary</span></span>

<span data-ttu-id="27095-174">W tym przewodniku zobaczyliśmy, jak do mapowania typów przestrzennych, za pomocą programu Entity Framework Designer oraz jak używać typów przestrzennych w kodzie.</span><span class="sxs-lookup"><span data-stu-id="27095-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 

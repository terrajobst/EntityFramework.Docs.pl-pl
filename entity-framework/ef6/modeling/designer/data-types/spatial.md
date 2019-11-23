---
title: Projektant przestrzenny-EF — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182504"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="212f2-102">Projektant przestrzenny-EF</span><span class="sxs-lookup"><span data-stu-id="212f2-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="212f2-103">**EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="212f2-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="212f2-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="212f2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="212f2-105">Film wideo i przewodnik krok po kroku przedstawiają sposób mapowania typów przestrzennych przy użyciu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="212f2-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="212f2-106">Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="212f2-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="212f2-107">Ten Instruktaż będzie używać Model First do tworzenia nowej bazy danych, ale można również użyć programu Dr Designer z przepływem pracy [Database First](~/ef6/modeling/designer/workflows/database-first.md) , aby mapować do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="212f2-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="212f2-108">Obsługa typów przestrzennych została wprowadzona w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="212f2-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="212f2-109">Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzenny, wyliczenia i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="212f2-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="212f2-110">Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="212f2-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="212f2-111">Aby można było używać typów danych przestrzennych, należy również użyć dostawcy Entity Framework z obsługą przestrzenną.</span><span class="sxs-lookup"><span data-stu-id="212f2-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="212f2-112">Aby uzyskać więcej informacji, zobacz [Obsługa dostawcy dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="212f2-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="212f2-113">Istnieją dwa główne typy danych przestrzennych: Geografia i geometria.</span><span class="sxs-lookup"><span data-stu-id="212f2-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="212f2-114">Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne i Długość geograficzna).</span><span class="sxs-lookup"><span data-stu-id="212f2-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="212f2-115">Typ danych geometrii reprezentuje układ współrzędnych Euclidean (płaski).</span><span class="sxs-lookup"><span data-stu-id="212f2-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="212f2-116">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="212f2-116">Watch the video</span></span>
<span data-ttu-id="212f2-117">W tym filmie wideo przedstawiono sposób mapowania typów przestrzennych przy użyciu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="212f2-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="212f2-118">Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.</span><span class="sxs-lookup"><span data-stu-id="212f2-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="212f2-119">**Przedstawione przez**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="212f2-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="212f2-120">**Wideo**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (zip)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="212f2-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="212f2-121">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="212f2-121">Pre-Requisites</span></span>

<span data-ttu-id="212f2-122">Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="212f2-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="212f2-123">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="212f2-123">Set up the Project</span></span>

1.  <span data-ttu-id="212f2-124">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="212f2-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="212f2-125">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .</span><span class="sxs-lookup"><span data-stu-id="212f2-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="212f2-126">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli**</span><span class="sxs-lookup"><span data-stu-id="212f2-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="212f2-127">Wprowadź **SpatialEFDesigner** jako nazwę projektu, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="212f2-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="212f2-128">Tworzenie nowego modelu przy użyciu narzędzia Dr Designer</span><span class="sxs-lookup"><span data-stu-id="212f2-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="212f2-129">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="212f2-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="212f2-130">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="212f2-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="212f2-131">W polu Nazwa pliku wprowadź **UniversityModel. edmx** , a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="212f2-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="212f2-132">Na stronie kreatora Entity Data Model wybierz pozycję **pusty model** w oknie dialogowym Wybierz zawartość modelu</span><span class="sxs-lookup"><span data-stu-id="212f2-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="212f2-133">Kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="212f2-133">Click **Finish**</span></span>

<span data-ttu-id="212f2-134">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="212f2-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="212f2-135">Kreator wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="212f2-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="212f2-136">Generuje plik EnumTestModel. edmx, który definiuje model koncepcyjny, model magazynu i mapowanie między nimi.</span><span class="sxs-lookup"><span data-stu-id="212f2-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="212f2-137">Ustawia właściwość przetwarzania artefaktu metadanych pliku. edmx na osadzony w zestawie danych wyjściowych, aby wygenerowane pliki metadanych zostały osadzone w zestawie.</span><span class="sxs-lookup"><span data-stu-id="212f2-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="212f2-138">Dodaje odwołanie do następujących zestawów: EntityFramework, system. ComponentModel. DataAnnotations i system. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="212f2-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="212f2-139">Tworzy pliki UniversityModel.tt i UniversityModel.Context.tt i dodaje je do pliku. edmx.</span><span class="sxs-lookup"><span data-stu-id="212f2-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="212f2-140">Te pliki szablonów T4 generują kod, który definiuje typ pochodny DbContext i typy POCO, które są mapowane na jednostki w modelu. edmx</span><span class="sxs-lookup"><span data-stu-id="212f2-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="212f2-141">Dodaj nowy typ jednostki</span><span class="sxs-lookup"><span data-stu-id="212f2-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="212f2-142">Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, a następnie wybierz polecenie **dodaj&gt; jednostki**, pojawi się okno dialogowe Nowa jednostka</span><span class="sxs-lookup"><span data-stu-id="212f2-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="212f2-143">Określ nazwę typu dla **Uniwersytetu** i określ **UniversityID** dla nazwy właściwości klucza, pozostaw typ jako **Int32**</span><span class="sxs-lookup"><span data-stu-id="212f2-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="212f2-144">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="212f2-144">Click **OK**</span></span>
4.  <span data-ttu-id="212f2-145">Kliknij prawym przyciskiem myszy jednostkę i wybierz polecenie **Dodaj nową-&gt; Właściwość skalarna**</span><span class="sxs-lookup"><span data-stu-id="212f2-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="212f2-146">Zmień nazwę nowej właściwości na **nazwę**</span><span class="sxs-lookup"><span data-stu-id="212f2-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="212f2-147">Dodaj kolejną Właściwość skalarną i zmień jej nazwę na **lokalizację** Otwórz okno właściwości i Zmień typ nowej właściwości na **Geografia**</span><span class="sxs-lookup"><span data-stu-id="212f2-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="212f2-148">Zapisz model i skompiluj projekt</span><span class="sxs-lookup"><span data-stu-id="212f2-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="212f2-149">Podczas kompilowania w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="212f2-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="212f2-150">Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.</span><span class="sxs-lookup"><span data-stu-id="212f2-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="212f2-151">Generuj bazę danych na podstawie modelu</span><span class="sxs-lookup"><span data-stu-id="212f2-151">Generate Database from Model</span></span>

<span data-ttu-id="212f2-152">Teraz możemy wygenerować bazę danych opartą na modelu.</span><span class="sxs-lookup"><span data-stu-id="212f2-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="212f2-153">Kliknij prawym przyciskiem myszy puste miejsce na Entity Designer powierzchni i wybierz polecenie **Generuj bazę danych na podstawie modelu**</span><span class="sxs-lookup"><span data-stu-id="212f2-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="212f2-154">Zostanie wyświetlone okno dialogowe Wybieranie połączenia danych w Kreatorze generowania bazy danych kliknij przycisk **nowe połączenie** , określ **(LocalDB)\\mssqllocaldb** dla nazwy serwera i **University** dla bazy danych, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="212f2-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="212f2-155">Zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz utworzyć nową bazę danych, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="212f2-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="212f2-156">Kliknij przycisk **dalej** , aby Kreator tworzenia bazy danych wygenerował język definicji danych (DDL) służący do tworzenia bazy danych, wygenerowany kod DDL jest wyświetlany w oknie dialogowym Podsumowanie i ustawienia Zanotuj, że kod DDL nie zawiera definicji tabeli, która jest mapowana na typ wyliczenia</span><span class="sxs-lookup"><span data-stu-id="212f2-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="212f2-157">Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie powoduje wykonania skryptu DDL.</span><span class="sxs-lookup"><span data-stu-id="212f2-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="212f2-158">Kreator tworzenia bazy danych wykonuje następujące czynności: otwiera plik **UniversityModel. edmx. SQL** w edytorze T-SQL generuje schemat magazynu i sekcje mapowania pliku edmx dodaje informacje o parametrach połączenia do pliku App. config</span><span class="sxs-lookup"><span data-stu-id="212f2-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="212f2-159">Kliknij prawym przyciskiem myszy w edytorze T-SQL i wybierz polecenie **Wykonaj** okno dialogowe łączenie z serwerem, wprowadź informacje o połączeniu z kroku 2 i kliknij pozycję **Połącz** .</span><span class="sxs-lookup"><span data-stu-id="212f2-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="212f2-160">Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="212f2-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="212f2-161">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="212f2-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="212f2-162">Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main.</span><span class="sxs-lookup"><span data-stu-id="212f2-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="212f2-163">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="212f2-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="212f2-164">Kod dodaje dwa nowe obiekty University do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="212f2-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="212f2-165">Właściwości przestrzenne są inicjowane przy użyciu metody DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="212f2-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="212f2-166">Punkt geografii reprezentowany jako WellKnownText jest przenoszona do metody.</span><span class="sxs-lookup"><span data-stu-id="212f2-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="212f2-167">Następnie kod zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="212f2-167">The code then saves the data.</span></span> <span data-ttu-id="212f2-168">Następnie zapytanie LINQ, które zwraca obiekt University, gdzie jego lokalizacja znajduje się najbliżej określonej lokalizacji, jest konstruowany i wykonywany.</span><span class="sxs-lookup"><span data-stu-id="212f2-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="212f2-169">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="212f2-169">Compile and run the application.</span></span> <span data-ttu-id="212f2-170">Program tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="212f2-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="212f2-171">Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież**.</span><span class="sxs-lookup"><span data-stu-id="212f2-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="212f2-172">Następnie kliknij prawym przyciskiem myszy w tabeli i wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="212f2-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="212f2-173">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="212f2-173">Summary</span></span>

<span data-ttu-id="212f2-174">W tym instruktażu przedstawiono sposób mapowania typów przestrzennych przy użyciu Entity Framework Designer i sposobu używania typów przestrzennych w kodzie.</span><span class="sxs-lookup"><span data-stu-id="212f2-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 

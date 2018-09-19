---
title: Obsługa Enum - projektancie platformy EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 331182c4311565c94cf072eb9b9ad372ac76180a
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283943"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="47df4-102">Pomoc techniczna dla wyliczenia — projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="47df4-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="47df4-103">**EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="47df4-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="47df4-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="47df4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="47df4-105">W tym przewodniku krok po kroku i wideo pokazuje, jak w typach wyliczeniowych za pomocą programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="47df4-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="47df4-106">Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.</span><span class="sxs-lookup"><span data-stu-id="47df4-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="47df4-107">W tym przewodniku użyje pierwszego modelu do utworzenia nowej bazy danych, ale może również służyć projektancie platformy EF z [Database First](~/ef6/modeling/designer/workflows/database-first.md) przepływu pracy do mapowania istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="47df4-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="47df4-108">Obsługa typu wyliczeniowego została wprowadzona w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="47df4-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="47df4-109">Aby korzystać z nowych funkcji, takich jak typy wyliczeniowe, typy danych przestrzennych i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="47df4-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="47df4-110">Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.</span><span class="sxs-lookup"><span data-stu-id="47df4-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="47df4-111">W programie Entity Framework, wyliczenie może mieć następujące typy bazowe: **bajtów**, **Int16**, **Int32**, **Int64** , lub **SByte**.</span><span class="sxs-lookup"><span data-stu-id="47df4-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="47df4-112">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="47df4-112">Watch the Video</span></span>
<span data-ttu-id="47df4-113">To wideo pokazuje, jak w typach wyliczeniowych za pomocą programu Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="47df4-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="47df4-114">Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.</span><span class="sxs-lookup"><span data-stu-id="47df4-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="47df4-115">**Osoba prezentująca**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="47df4-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="47df4-116">**Film wideo**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="47df4-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="47df4-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="47df4-117">Pre-Requisites</span></span>

<span data-ttu-id="47df4-118">Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="47df4-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="47df4-119">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="47df4-119">Set up the Project</span></span>

1.  <span data-ttu-id="47df4-120">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="47df4-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="47df4-121">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**</span><span class="sxs-lookup"><span data-stu-id="47df4-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="47df4-122">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu</span><span class="sxs-lookup"><span data-stu-id="47df4-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="47df4-123">Wprowadź **EnumEFDesigner** jako nazwę projektu i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="47df4-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="47df4-124">Utwórz nowy Model przy użyciu projektanta EF</span><span class="sxs-lookup"><span data-stu-id="47df4-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="47df4-125">Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wskaż opcję **Dodaj**, a następnie kliknij przycisk **nowy element**</span><span class="sxs-lookup"><span data-stu-id="47df4-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="47df4-126">Wybierz **danych** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablonów</span><span class="sxs-lookup"><span data-stu-id="47df4-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="47df4-127">Wprowadź **EnumTestModel.edmx** nazwę pliku, a następnie kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="47df4-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="47df4-128">Na stronie Kreator modelu Entity Data Model wybierz **pusty Model** w oknie dialogowym Wybierz zawartość modelu</span><span class="sxs-lookup"><span data-stu-id="47df4-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="47df4-129">Kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="47df4-129">Click **Finish**</span></span>

<span data-ttu-id="47df4-130">Zostanie wyświetlona Projektancie jednostki, zapewniającą powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="47df4-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="47df4-131">Kreator wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="47df4-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="47df4-132">Generuje plik EnumTestModel.edmx, który definiuje modelu koncepcyjnego, model magazynu i mapowanie między nimi.</span><span class="sxs-lookup"><span data-stu-id="47df4-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="47df4-133">Ustawia właściwość metadanych artefaktów przetwarzania pliku edmx osadzania w zestawie danych wyjściowych, więc pliki metadanych wygenerowanych Pobierz osadzone w zestawie.</span><span class="sxs-lookup"><span data-stu-id="47df4-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="47df4-134">Dodanie odwołania do następujących zestawów: platformy EntityFramework, System.ComponentModel.DataAnnotations i System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="47df4-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="47df4-135">Tworzy pliki EnumTestModel.tt i EnumTestModel.Context.tt i dodaje je w pliku edmx.</span><span class="sxs-lookup"><span data-stu-id="47df4-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="47df4-136">Te pliki szablonów T4 wygenerować kod, który definiuje typu DbContext pochodnych i POCO typów, które mapują jednostek w modelu edmx.</span><span class="sxs-lookup"><span data-stu-id="47df4-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="47df4-137">Dodaj nowy typ jednostki</span><span class="sxs-lookup"><span data-stu-id="47df4-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="47df4-138">Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wybierz opcję **Add -&gt; jednostki**, pojawi się okno dialogowe nowej jednostki</span><span class="sxs-lookup"><span data-stu-id="47df4-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="47df4-139">Określ **działu** dla typu nazwy i określ **DepartmentID** dla danej nazwy właściwości klucza, pozostaw typ jako **Int32**</span><span class="sxs-lookup"><span data-stu-id="47df4-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="47df4-140">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="47df4-140">Click **OK**</span></span>
4.  <span data-ttu-id="47df4-141">Kliknij prawym przyciskiem myszy jednostkę, a następnie wybierz pozycję **Dodaj nowy -&gt; właściwość skalarną**</span><span class="sxs-lookup"><span data-stu-id="47df4-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="47df4-142">Zmień nazwę nowej właściwości **nazwy**</span><span class="sxs-lookup"><span data-stu-id="47df4-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="47df4-143">Zmień typ nowej właściwości **Int32** (domyślnie przez nową właściwość jest typu String) Aby zmienić typ, Otwórz okno właściwości, a następnie zmień typ właściwości **Int32**</span><span class="sxs-lookup"><span data-stu-id="47df4-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="47df4-144">Dodawanie innej skalarne właściwości i zmień jej nazwę na **budżetu**, Zmień typ na **dziesiętna**</span><span class="sxs-lookup"><span data-stu-id="47df4-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="47df4-145">Dodaj typ wyliczeniowy</span><span class="sxs-lookup"><span data-stu-id="47df4-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="47df4-146">W Entity Framework Designer kliknij prawym przyciskiem myszy nazwę właściwości, wybierz **przekonwertować wyliczenia**</span><span class="sxs-lookup"><span data-stu-id="47df4-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Konwertuj na wyliczenie](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="47df4-148">W **Dodaj wyliczenie** okna dialogowego wpisz **DepartmentNames** dla nazwy typu wyliczenia, Zmień typ podstawowy do **Int32**, a następnie dodaj następujące elementy członkowskie do typu: angielski, Matematyczne i oszczędnościami, jakie daje</span><span class="sxs-lookup"><span data-stu-id="47df4-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Dodaj typ wyliczeniowy](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="47df4-150">Naciśnij klawisz **OK**</span><span class="sxs-lookup"><span data-stu-id="47df4-150">Press **OK**</span></span>
4.  <span data-ttu-id="47df4-151">Zapisz model i skompiluj projekt</span><span class="sxs-lookup"><span data-stu-id="47df4-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="47df4-152">W przypadku tworzenia, ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów.</span><span class="sxs-lookup"><span data-stu-id="47df4-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="47df4-153">Można zignorować te ostrzeżenia, ponieważ po Wybierzmy wygenerować bazę danych z modelu, błędy znikną.</span><span class="sxs-lookup"><span data-stu-id="47df4-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="47df4-154">Jeśli przyjrzymy się w oknie właściwości, zauważysz, że typ właściwości Nazwa została zmieniona na **DepartmentNames** i typ wyliczeniowy nowo dodanych została dodana do listy typów.</span><span class="sxs-lookup"><span data-stu-id="47df4-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="47df4-155">Po przełączeniu do okna przeglądarki modelu pojawi się, czy typ został również dodany do węzła typach wyliczeniowych.</span><span class="sxs-lookup"><span data-stu-id="47df4-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Przeglądarka modeli](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="47df4-157">Można również dodać nowe typy wyliczenia z tego okna, klikając prawym przyciskiem myszy i wybierając **Dodaj typ wyliczeniowy**.</span><span class="sxs-lookup"><span data-stu-id="47df4-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="47df4-158">Po utworzeniu typu pojawi się na liście typów i będzie można skojarzyć z właściwością</span><span class="sxs-lookup"><span data-stu-id="47df4-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="47df4-159">Generuj bazę danych z modelu</span><span class="sxs-lookup"><span data-stu-id="47df4-159">Generate Database from Model</span></span>

<span data-ttu-id="47df4-160">Teraz możemy wygenerować bazę danych, która jest oparta na modelu.</span><span class="sxs-lookup"><span data-stu-id="47df4-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="47df4-161">Kliknij prawym przyciskiem myszy pusty obszar w Projektancie jednostki powierzchni i wybierz **Generuj z bazy danych z modelu**</span><span class="sxs-lookup"><span data-stu-id="47df4-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="47df4-162">Wybierz połączenie danych dialogowym Generuj Kreatora bazy danych jest wyświetlany, kliknij przycisk **nowe połączenie** przycisk Określ **(localdb)\\mssqllocaldb** dla nazwy serwera i  **EnumTest** bazy danych, a następnie kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="47df4-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="47df4-163">Okno z pytaniem, jeśli chcesz utworzyć nową bazę danych będą się pojawiać, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="47df4-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="47df4-164">Kliknij przycisk **dalej** i Kreatora tworzenia bazy danych generuje języka definicji danych (DDL) dla tworzenia bazy danych wygenerowanej języka DDL są wyświetlane w Podsumowanie i okna dialogowego pole Uwaga dotycząca ustawień, który DDL nie zawiera definicji dla tabelę, która mapuje do typu wyliczenia</span><span class="sxs-lookup"><span data-stu-id="47df4-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="47df4-165">Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie wykonuje skrypt języka DDL.</span><span class="sxs-lookup"><span data-stu-id="47df4-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="47df4-166">Kreator tworzenia bazy danych wykonuje następujące czynności: Otwiera **EnumTest.edmx.sql** w Edytor T-SQL generuje sekcje schematu i mapowania magazynu EDMX pliku informacji o parametrach połączenia usług AD DS do pliku App.config</span><span class="sxs-lookup"><span data-stu-id="47df4-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="47df4-167">Kliknij prawym przyciskiem myszy w edytorze języka T-SQL i wybierz pozycję **Execute** nawiązać połączenie z serwerem w oknie dialogowym wyświetlony, podaj informacje o połączeniu z kroku 2, a następnie kliknij polecenie **Connect**</span><span class="sxs-lookup"><span data-stu-id="47df4-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="47df4-168">Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **odświeżania**</span><span class="sxs-lookup"><span data-stu-id="47df4-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="47df4-169">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="47df4-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="47df4-170">Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main.</span><span class="sxs-lookup"><span data-stu-id="47df4-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="47df4-171">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="47df4-171">Add the following code into the Main function.</span></span> <span data-ttu-id="47df4-172">Ten kod dodaje nowy obiekt działu do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="47df4-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="47df4-173">Następnie zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="47df4-173">It then saves the data.</span></span> <span data-ttu-id="47df4-174">Kod wykonuje też zapytanie LINQ, które zwraca działu, gdzie nazwa jest DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="47df4-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="47df4-175">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="47df4-175">Compile and run the application.</span></span> <span data-ttu-id="47df4-176">Program generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="47df4-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="47df4-177">Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksploratorze obiektów SQL Server, a następnie wybierz **Odśwież**.</span><span class="sxs-lookup"><span data-stu-id="47df4-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="47df4-178">Kliknięcie prawym przyciskiem myszy tabelę i wybierz pozycję **dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="47df4-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="47df4-179">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="47df4-179">Summary</span></span>

<span data-ttu-id="47df4-180">W tym przewodniku zobaczyliśmy, jak do mapowania typów wyliczenia przy użyciu programu Entity Framework Designer oraz jak używać wyliczenia w kodzie.</span><span class="sxs-lookup"><span data-stu-id="47df4-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 

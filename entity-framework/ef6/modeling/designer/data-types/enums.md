---
title: Enum support-Dr Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182517"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="9341d-102">Obsługa Wyliczenie-Dr Designer</span><span class="sxs-lookup"><span data-stu-id="9341d-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="9341d-103">**EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="9341d-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="9341d-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="9341d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9341d-105">Ten film wideo i przewodnik krok po kroku pokazują, jak używać typów wyliczeniowych z Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9341d-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="9341d-106">Pokazano również, jak używać wyliczeń w zapytaniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="9341d-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="9341d-107">Ten Instruktaż będzie używać Model First do tworzenia nowej bazy danych, ale można również użyć programu Dr Designer z przepływem pracy [Database First](~/ef6/modeling/designer/workflows/database-first.md) , aby mapować do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9341d-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="9341d-108">Obsługa wyliczenia została wprowadzona w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="9341d-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="9341d-109">Aby korzystać z nowych funkcji, takich jak wyliczenia, typy danych przestrzennych i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="9341d-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="9341d-110">Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="9341d-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="9341d-111">W Entity Framework Wyliczenie może mieć następujące typy podstawowe: **Byte**, **Int16**, **Int32**, **Int64** **lub**parzystość.</span><span class="sxs-lookup"><span data-stu-id="9341d-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="9341d-112">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="9341d-112">Watch the Video</span></span>
<span data-ttu-id="9341d-113">W tym filmie wideo pokazano, jak używać typów wyliczeniowych z Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9341d-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="9341d-114">Pokazano również, jak używać wyliczeń w zapytaniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="9341d-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="9341d-115">**Przedstawione przez**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="9341d-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="9341d-116">**Film wideo**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="9341d-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="9341d-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9341d-117">Pre-Requisites</span></span>

<span data-ttu-id="9341d-118">Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="9341d-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9341d-119">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="9341d-119">Set up the Project</span></span>

1.  <span data-ttu-id="9341d-120">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9341d-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="9341d-121">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .</span><span class="sxs-lookup"><span data-stu-id="9341d-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="9341d-122">W lewym okienku kliknij pozycję **Visual C @ no__t-1**, a następnie wybierz szablon **konsoli**</span><span class="sxs-lookup"><span data-stu-id="9341d-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="9341d-123">Wprowadź **EnumEFDesigner** jako nazwę projektu, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="9341d-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="9341d-124">Tworzenie nowego modelu przy użyciu narzędzia Dr Designer</span><span class="sxs-lookup"><span data-stu-id="9341d-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="9341d-125">Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wskaż polecenie **Dodaj**, a następnie kliknij pozycję **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="9341d-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="9341d-126">Wybierz pozycję **dane** z menu po lewej stronie, a następnie wybierz pozycję **ADO.NET Entity Data Model** w okienku szablony.</span><span class="sxs-lookup"><span data-stu-id="9341d-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="9341d-127">W polu Nazwa pliku wprowadź **EnumTestModel. edmx** , a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="9341d-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="9341d-128">Na stronie kreatora Entity Data Model wybierz pozycję **pusty model** w oknie dialogowym Wybierz zawartość modelu</span><span class="sxs-lookup"><span data-stu-id="9341d-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="9341d-129">Kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="9341d-129">Click **Finish**</span></span>

<span data-ttu-id="9341d-130">Zostanie wyświetlona Entity Designer, która zapewnia powierzchnię projektową do edycji modelu.</span><span class="sxs-lookup"><span data-stu-id="9341d-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="9341d-131">Kreator wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9341d-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="9341d-132">Generuje plik EnumTestModel. edmx, który definiuje model koncepcyjny, model magazynu i mapowanie między nimi.</span><span class="sxs-lookup"><span data-stu-id="9341d-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="9341d-133">Ustawia właściwość przetwarzania artefaktu metadanych pliku. edmx na osadzony w zestawie danych wyjściowych, aby wygenerowane pliki metadanych zostały osadzone w zestawie.</span><span class="sxs-lookup"><span data-stu-id="9341d-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="9341d-134">Dodaje odwołanie do następujących zestawów: EntityFramework, system. ComponentModel. DataAnnotations i system. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="9341d-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="9341d-135">Tworzy pliki EnumTestModel.tt i EnumTestModel.Context.tt i dodaje je do pliku. edmx.</span><span class="sxs-lookup"><span data-stu-id="9341d-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="9341d-136">Te pliki szablonów T4 generują kod, który definiuje typ pochodny DbContext i typy POCO, które są mapowane na jednostki w modelu. edmx.</span><span class="sxs-lookup"><span data-stu-id="9341d-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="9341d-137">Dodaj nowy typ jednostki</span><span class="sxs-lookup"><span data-stu-id="9341d-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="9341d-138">Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, a następnie wybierz polecenie **Dodaj-&gt; jednostki**, pojawi się okno dialogowe Nowa jednostka</span><span class="sxs-lookup"><span data-stu-id="9341d-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="9341d-139">Określ **dział** dla nazwy typu i określ **DepartmentID** dla nazwy właściwości klucza, pozostaw typ jako **Int32**</span><span class="sxs-lookup"><span data-stu-id="9341d-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="9341d-140">Kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="9341d-140">Click **OK**</span></span>
4.  <span data-ttu-id="9341d-141">Kliknij prawym przyciskiem myszy jednostkę i wybierz polecenie **Dodaj nową-&gt; Właściwość skalarna**</span><span class="sxs-lookup"><span data-stu-id="9341d-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="9341d-142">Zmień nazwę nowej właściwości na **nazwę**</span><span class="sxs-lookup"><span data-stu-id="9341d-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="9341d-143">Zmień typ nowej właściwości na **Int32** (domyślnie Nowa właściwość jest typu String) Aby zmienić typ, Otwórz okno właściwości i Zmień Właściwość Type na **Int32**</span><span class="sxs-lookup"><span data-stu-id="9341d-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="9341d-144">Dodaj kolejną Właściwość skalarną i zmień jej nazwę na **budżet**, Zmień typ na **dziesiętny**</span><span class="sxs-lookup"><span data-stu-id="9341d-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="9341d-145">Dodawanie typu wyliczeniowego</span><span class="sxs-lookup"><span data-stu-id="9341d-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="9341d-146">W Entity Framework Designer kliknij prawym przyciskiem myszy Właściwość Name, wybierz pozycję **Konwertuj na Wyliczenie** .</span><span class="sxs-lookup"><span data-stu-id="9341d-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Konwertuj na Wyliczenie](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="9341d-148">W oknie dialogowym **Dodawanie wyliczenia** wpisz **DepartmentNames** dla nazwy typu wyliczenia, Zmień typ podstawowy na **Int32**, a następnie Dodaj do typu następujące elementy członkowskie: Angielski, Math i ekonomia</span><span class="sxs-lookup"><span data-stu-id="9341d-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Dodaj typ wyliczenia](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="9341d-150">Naciśnij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="9341d-150">Press **OK**</span></span>
4.  <span data-ttu-id="9341d-151">Zapisz model i skompiluj projekt</span><span class="sxs-lookup"><span data-stu-id="9341d-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="9341d-152">Podczas kompilowania w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń.</span><span class="sxs-lookup"><span data-stu-id="9341d-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="9341d-153">Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.</span><span class="sxs-lookup"><span data-stu-id="9341d-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="9341d-154">Jeśli zobaczysz okno Właściwości, zobaczysz, że typ właściwości Nazwa została zmieniona na **DepartmentNames** , a nowo dodany typ wyliczeniowy został dodany do listy typów.</span><span class="sxs-lookup"><span data-stu-id="9341d-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="9341d-155">Po przełączeniu do okna przeglądarki modelu zobaczysz, że typ został również dodany do węzła typy wyliczeniowe.</span><span class="sxs-lookup"><span data-stu-id="9341d-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Przeglądarka modeli](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="9341d-157">Możesz również dodać nowe typy wyliczeniowe z tego okna, klikając prawym przyciskiem myszy i wybierając pozycję **Dodaj typ wyliczeniowy**.</span><span class="sxs-lookup"><span data-stu-id="9341d-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="9341d-158">Po utworzeniu typu zostanie on wyświetlony na liście typów i będzie można skojarzyć z właściwością</span><span class="sxs-lookup"><span data-stu-id="9341d-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="9341d-159">Generuj bazę danych na podstawie modelu</span><span class="sxs-lookup"><span data-stu-id="9341d-159">Generate Database from Model</span></span>

<span data-ttu-id="9341d-160">Teraz możemy wygenerować bazę danych opartą na modelu.</span><span class="sxs-lookup"><span data-stu-id="9341d-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="9341d-161">Kliknij prawym przyciskiem myszy puste miejsce na Entity Designer powierzchni i wybierz polecenie **Generuj bazę danych na podstawie modelu**</span><span class="sxs-lookup"><span data-stu-id="9341d-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="9341d-162">Zostanie wyświetlone okno dialogowe Wybieranie połączenia danych w Kreatorze generowania bazy danych. kliknij przycisk **nowe połączenie** , określ **(LocalDB) \\mssqllocaldb** jako nazwę serwera i **EnumTest** dla bazy danych, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="9341d-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="9341d-163">Zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz utworzyć nową bazę danych, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="9341d-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="9341d-164">Kliknij przycisk **dalej** , aby Kreator tworzenia bazy danych wygenerował język definicji danych (DDL) służący do tworzenia bazy danych, wygenerowany kod DDL jest wyświetlany w oknie dialogowym Podsumowanie i ustawienia Zanotuj, że kod DDL nie zawiera definicji tabeli mapowanej na typ wyliczenia</span><span class="sxs-lookup"><span data-stu-id="9341d-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="9341d-165">Kliknij przycisk **Zakończ** kliknięcie przycisku Zakończ nie powoduje wykonania skryptu DDL.</span><span class="sxs-lookup"><span data-stu-id="9341d-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="9341d-166">Kreator tworzenia bazy danych wykonuje następujące czynności: Otwiera plik **EnumTest. edmx. SQL** w edytorze T-SQL generuje schemat magazynu i sekcje mapowania pliku edmx dodaje informacje o parametrach połączenia do pliku App. config</span><span class="sxs-lookup"><span data-stu-id="9341d-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="9341d-167">Kliknij prawym przyciskiem myszy w edytorze T-SQL i wybierz polecenie **Wykonaj** okno dialogowe łączenie z serwerem, wprowadź informacje o połączeniu z kroku 2 i kliknij pozycję **Połącz** .</span><span class="sxs-lookup"><span data-stu-id="9341d-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="9341d-168">Aby wyświetlić wygenerowany schemat, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież** .</span><span class="sxs-lookup"><span data-stu-id="9341d-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="9341d-169">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="9341d-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="9341d-170">Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main.</span><span class="sxs-lookup"><span data-stu-id="9341d-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="9341d-171">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="9341d-171">Add the following code into the Main function.</span></span> <span data-ttu-id="9341d-172">Kod dodaje nowy obiekt działu do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="9341d-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="9341d-173">Następnie zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="9341d-173">It then saves the data.</span></span> <span data-ttu-id="9341d-174">Kod wykonuje również zapytanie LINQ, które zwraca dział, w którym nazwa jest DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="9341d-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="9341d-175">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="9341d-175">Compile and run the application.</span></span> <span data-ttu-id="9341d-176">Program tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="9341d-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="9341d-177">Aby wyświetlić dane w bazie danych, kliknij prawym przyciskiem myszy nazwę bazy danych w Eksplorator obiektów SQL Server i wybierz polecenie **Odśwież**.</span><span class="sxs-lookup"><span data-stu-id="9341d-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="9341d-178">Następnie kliknij prawym przyciskiem myszy w tabeli i wybierz polecenie **Wyświetl dane**.</span><span class="sxs-lookup"><span data-stu-id="9341d-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="9341d-179">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9341d-179">Summary</span></span>

<span data-ttu-id="9341d-180">W tym instruktażu przedstawiono sposób mapowania typów wyliczeniowych przy użyciu Entity Framework Designer i sposobu używania wyliczeń w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9341d-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 

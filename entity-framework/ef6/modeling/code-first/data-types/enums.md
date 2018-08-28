---
title: Pomoc techniczna dla wyliczenia — najpierw - Code EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 529370ef7aba3975a36cbfdf5c439ee87b563c7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997237"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="fbdfa-102">Pomoc techniczna dla wyliczenia — kod najpierw</span><span class="sxs-lookup"><span data-stu-id="fbdfa-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="fbdfa-103">**EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="fbdfa-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="fbdfa-105">W tym przewodniku krok po kroku i wideo pokazuje, jak za pomocą typach wyliczeniowych Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="fbdfa-106">Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="fbdfa-107">W tym przewodniku będzie używać Code First, aby utworzyć nową bazę danych, ale można również użyć [Code First, aby zmapować istniejącą bazę danych](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="fbdfa-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="fbdfa-108">Obsługa typu wyliczeniowego została wprowadzona w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="fbdfa-109">Aby korzystać z nowych funkcji, takich jak typy wyliczeniowe, typy danych przestrzennych i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="fbdfa-110">Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="fbdfa-111">W programie Entity Framework, wyliczenie może mieć następujące typy bazowe: **bajtów**, **Int16**, **Int32**, **Int64** , lub **SByte**.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="fbdfa-112">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="fbdfa-112">Watch the video</span></span>
<span data-ttu-id="fbdfa-113">To wideo pokazuje, jak za pomocą typach wyliczeniowych Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="fbdfa-114">Ilustruje też sposób używania typów wyliczeniowych w zapytaniu programu LINQ.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="fbdfa-115">**Osoba prezentująca**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="fbdfa-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="fbdfa-116">**Film wideo**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="fbdfa-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="fbdfa-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fbdfa-117">Pre-Requisites</span></span>

<span data-ttu-id="fbdfa-118">Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="fbdfa-119">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="fbdfa-119">Set up the Project</span></span>

1.  <span data-ttu-id="fbdfa-120">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="fbdfa-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="fbdfa-121">Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**</span><span class="sxs-lookup"><span data-stu-id="fbdfa-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="fbdfa-122">W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu</span><span class="sxs-lookup"><span data-stu-id="fbdfa-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="fbdfa-123">Wprowadź **EnumCodeFirst** jako nazwę projektu i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="fbdfa-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="fbdfa-124">Definiowanie nowego modelu za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="fbdfa-124">Define a New Model using Code First</span></span>

<span data-ttu-id="fbdfa-125">Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).</span><span class="sxs-lookup"><span data-stu-id="fbdfa-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="fbdfa-126">Poniższy kod definiuje klasę działu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-126">The code below defines the Department class.</span></span>

<span data-ttu-id="fbdfa-127">Ten kod definiuje również wyliczenie DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="fbdfa-128">Domyślnie jest wyliczenie **int** typu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="fbdfa-129">Właściwość Nazwa klasy działu jest typu DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="fbdfa-130">Otwórz plik Program.cs i wklej poniższe definicje klas.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="fbdfa-131">Definiowanie typu pochodnego typu DbContext</span><span class="sxs-lookup"><span data-stu-id="fbdfa-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="fbdfa-132">Oprócz definiowania jednostek, musisz zdefiniować klasę, która pochodzi od typu DbContext i udostępnia DbSet&lt;TEntity&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="fbdfa-133">DbSet&lt;TEntity&gt; właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="fbdfa-134">Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="fbdfa-135">Typy DbContext i DbSet są definiowane w zestawie platformy EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="fbdfa-136">Firma Microsoft doda odwołanie do tej biblioteki DLL przy użyciu pakietu EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="fbdfa-137">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="fbdfa-138">Wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="fbdfa-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="fbdfa-139">W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="fbdfa-140">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="fbdfa-140">Click **Install**</span></span>

<span data-ttu-id="fbdfa-141">Należy pamiętać, że oprócz zestawu platformy EntityFramework, odwołania do zestawów System.ComponentModel.DataAnnotations i System.Data.Entity są dodawane także.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="fbdfa-142">W górnej części pliku Program.cs Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="fbdfa-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="fbdfa-143">W pliku Program.cs Dodaj definicję kontekstu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="fbdfa-144">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="fbdfa-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="fbdfa-145">Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="fbdfa-146">Dodaj następujący kod do funkcji Main.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-146">Add the following code into the Main function.</span></span> <span data-ttu-id="fbdfa-147">Ten kod dodaje nowy obiekt działu do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="fbdfa-148">Następnie zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-148">It then saves the data.</span></span> <span data-ttu-id="fbdfa-149">Kod wykonuje też zapytanie LINQ, które zwraca działu, gdzie nazwa jest DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="fbdfa-150">Skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-150">Compile and run the application.</span></span> <span data-ttu-id="fbdfa-151">Program generuje następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="fbdfa-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="fbdfa-152">Wyświetlanie wygenerowanych bazy danych</span><span class="sxs-lookup"><span data-stu-id="fbdfa-152">View the Generated Database</span></span>

<span data-ttu-id="fbdfa-153">Po uruchomieniu aplikacji po raz pierwszy, platformy Entity Framework tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="fbdfa-154">Dysponujemy programu Visual Studio 2012, baza danych zostanie utworzony w wystąpieniu programu LocalDB.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="fbdfa-155">Domyślnie platforma Entity Framework nazwy bazy danych po w pełni kwalifikowanej nazwie pochodnej kontekstu (w tym przykładzie, który jest **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="fbdfa-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="fbdfa-156">Kolejne razy, zostanie użyta istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="fbdfa-157">Należy pamiętać, że jeśli wprowadzisz zmiany do modelu, po utworzeniu bazy danych, należy użyć migracje Code First do zaktualizowania schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="fbdfa-158">Zobacz [Code First dla nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) na przykład za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="fbdfa-159">Aby wyświetlić i bazy danych, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="fbdfa-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="fbdfa-160">W menu głównym programu Visual Studio 2012, wybierz **widoku**  - &gt; **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="fbdfa-161">Jeśli LocalDB, nie ma na liście serwerów, kliknij prawym przyciskiem myszy **programu SQL Server** i wybierz **dodawania serwera SQL** Użyj domyślnej **uwierzytelniania Windows** połączyć się z Wystąpienia LocalDB</span><span class="sxs-lookup"><span data-stu-id="fbdfa-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="fbdfa-162">Rozwiń węzeł LocalDB</span><span class="sxs-lookup"><span data-stu-id="fbdfa-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="fbdfa-163">Unfold **baz danych** folder, aby zobaczyć nową bazę danych, a następnie przejdź do **działu** tabeli należy pamiętać, że Code First nie tworzy tabelę, która mapuje dane na typ wyliczeniowy</span><span class="sxs-lookup"><span data-stu-id="fbdfa-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="fbdfa-164">Aby wyświetlić dane, kliknij prawym przyciskiem myszy w tabeli i wybrać **widoku danych**</span><span class="sxs-lookup"><span data-stu-id="fbdfa-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="fbdfa-165">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="fbdfa-165">Summary</span></span>

<span data-ttu-id="fbdfa-166">W tym przewodniku zobaczyliśmy, jak za pomocą typach wyliczeniowych Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="fbdfa-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 

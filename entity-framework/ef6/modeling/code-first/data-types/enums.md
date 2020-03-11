---
title: Obsługa wyliczenia-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419109"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="ffa29-102">Obsługa wyliczenia — Code First</span><span class="sxs-lookup"><span data-stu-id="ffa29-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="ffa29-103">**EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="ffa29-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="ffa29-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="ffa29-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ffa29-105">Ten film wideo i przewodnik krok po kroku pokazują, jak używać typów wyliczeniowych z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="ffa29-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="ffa29-106">Pokazano również, jak używać wyliczeń w zapytaniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="ffa29-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="ffa29-107">Ten Instruktaż będzie używać Code First do tworzenia nowej bazy danych, ale można również użyć [Code First do mapowania istniejącej bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="ffa29-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="ffa29-108">Obsługa wyliczenia została wprowadzona w Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="ffa29-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="ffa29-109">Aby korzystać z nowych funkcji, takich jak wyliczenia, typy danych przestrzennych i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="ffa29-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="ffa29-110">Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="ffa29-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="ffa29-111">W Entity Framework Wyliczenie może mieć następujące typy podstawowe: **Byte**, **Int16**, **Int32**, **Int64** **lub**bajty.</span><span class="sxs-lookup"><span data-stu-id="ffa29-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="ffa29-112">Obejrzyj film</span><span class="sxs-lookup"><span data-stu-id="ffa29-112">Watch the video</span></span>
<span data-ttu-id="ffa29-113">W tym filmie wideo pokazano, jak używać typów wyliczeniowych z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="ffa29-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="ffa29-114">Pokazano również, jak używać wyliczeń w zapytaniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="ffa29-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="ffa29-115">**Przedstawione przez**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="ffa29-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="ffa29-116">**Wideo**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="ffa29-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ffa29-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ffa29-117">Pre-Requisites</span></span>

<span data-ttu-id="ffa29-118">Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="ffa29-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="ffa29-119">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="ffa29-119">Set up the Project</span></span>

1.  <span data-ttu-id="ffa29-120">Otwórz program Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ffa29-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="ffa29-121">W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .</span><span class="sxs-lookup"><span data-stu-id="ffa29-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="ffa29-122">W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli**</span><span class="sxs-lookup"><span data-stu-id="ffa29-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="ffa29-123">Wprowadź **EnumCodeFirst** jako nazwę projektu, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="ffa29-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="ffa29-124">Zdefiniuj nowy model przy użyciu Code First</span><span class="sxs-lookup"><span data-stu-id="ffa29-124">Define a New Model using Code First</span></span>

<span data-ttu-id="ffa29-125">W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny).</span><span class="sxs-lookup"><span data-stu-id="ffa29-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="ffa29-126">Poniższy kod definiuje klasę działu.</span><span class="sxs-lookup"><span data-stu-id="ffa29-126">The code below defines the Department class.</span></span>

<span data-ttu-id="ffa29-127">Kod definiuje również Wyliczenie DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="ffa29-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="ffa29-128">Domyślnie Wyliczenie jest typu **int** .</span><span class="sxs-lookup"><span data-stu-id="ffa29-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="ffa29-129">Właściwość Name klasy Department ma typ DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="ffa29-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="ffa29-130">Otwórz plik Program.cs i wklej następujące definicje klas.</span><span class="sxs-lookup"><span data-stu-id="ffa29-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="ffa29-131">Zdefiniuj typ pochodny DbContext</span><span class="sxs-lookup"><span data-stu-id="ffa29-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="ffa29-132">Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z DbContext i uwidacznia Nieogólnymi&lt;&gt; właściwości.</span><span class="sxs-lookup"><span data-stu-id="ffa29-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="ffa29-133">Nieogólnymi&lt;&gt; właściwości umożliwiają kontekstowi znać, które typy mają być uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="ffa29-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="ffa29-134">Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ffa29-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="ffa29-135">Typy DbContext i Nieogólnymi są zdefiniowane w zestawie EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="ffa29-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="ffa29-136">Dodamy odwołanie do tej biblioteki DLL przy użyciu pakietu NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="ffa29-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="ffa29-137">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="ffa29-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="ffa29-138">Wybierz pozycję **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="ffa29-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="ffa29-139">W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="ffa29-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="ffa29-140">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="ffa29-140">Click **Install**</span></span>

<span data-ttu-id="ffa29-141">Należy pamiętać, że oprócz zestawu EntityFramework odwołania do zestawów System. ComponentModel. DataAnnotations i system. Data. Entity są również dodawane.</span><span class="sxs-lookup"><span data-stu-id="ffa29-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="ffa29-142">W górnej części pliku Program.cs Dodaj następującą instrukcję using:</span><span class="sxs-lookup"><span data-stu-id="ffa29-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="ffa29-143">W Program.cs Dodaj definicję kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ffa29-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="ffa29-144">Utrwalanie i pobieranie danych</span><span class="sxs-lookup"><span data-stu-id="ffa29-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="ffa29-145">Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main.</span><span class="sxs-lookup"><span data-stu-id="ffa29-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="ffa29-146">Dodaj następujący kod do funkcji main.</span><span class="sxs-lookup"><span data-stu-id="ffa29-146">Add the following code into the Main function.</span></span> <span data-ttu-id="ffa29-147">Kod dodaje nowy obiekt działu do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="ffa29-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="ffa29-148">Następnie zapisuje dane.</span><span class="sxs-lookup"><span data-stu-id="ffa29-148">It then saves the data.</span></span> <span data-ttu-id="ffa29-149">Kod wykonuje również zapytanie LINQ, które zwraca dział, w którym nazwa jest DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="ffa29-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="ffa29-150">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ffa29-150">Compile and run the application.</span></span> <span data-ttu-id="ffa29-151">Program tworzy następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="ffa29-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="ffa29-152">Wyświetlanie wygenerowanej bazy danych</span><span class="sxs-lookup"><span data-stu-id="ffa29-152">View the Generated Database</span></span>

<span data-ttu-id="ffa29-153">Po pierwszym uruchomieniu aplikacji Entity Framework tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="ffa29-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="ffa29-154">Ze względu na to, że zainstalowano program Visual Studio 2012, baza danych zostanie utworzona w wystąpieniu LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ffa29-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="ffa29-155">Domyślnie Entity Framework nazw bazy danych po w pełni kwalifikowana nazwa kontekstu pochodnego (w tym przykładzie to **EnumCodeFirst. EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="ffa29-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="ffa29-156">Kolejne użycie istniejącej bazy danych będzie możliwe.</span><span class="sxs-lookup"><span data-stu-id="ffa29-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="ffa29-157">Należy pamiętać, że jeśli wprowadzisz zmiany w modelu po utworzeniu bazy danych, należy użyć Migracje Code First, aby zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ffa29-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="ffa29-158">Zapoznaj się z tematem [Code First do nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) , aby zapoznać się z przykładem użycia migracji.</span><span class="sxs-lookup"><span data-stu-id="ffa29-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="ffa29-159">Aby wyświetlić bazę danych i dane, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ffa29-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="ffa29-160">W menu głównym programu Visual Studio 2012 wybierz pozycję **wyświetl** -&gt; **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ffa29-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="ffa29-161">Jeśli LocalDB nie znajduje się na liście serwerów, kliknij prawym przyciskiem myszy na **SQL Server** i wybierz polecenie **Dodaj SQL Server** Użyj domyślnego **uwierzytelniania systemu Windows** , aby nawiązać połączenie z wystąpieniem LocalDB</span><span class="sxs-lookup"><span data-stu-id="ffa29-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="ffa29-162">Rozwiń węzeł LocalDB</span><span class="sxs-lookup"><span data-stu-id="ffa29-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="ffa29-163">Unfold folder **Databases** , aby wyświetlić nową bazę danych, i przejdź do uwagi do tabeli **działu** , że Code First nie utworzy tabeli, która jest mapowana na typ wyliczenia</span><span class="sxs-lookup"><span data-stu-id="ffa29-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="ffa29-164">Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**</span><span class="sxs-lookup"><span data-stu-id="ffa29-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="ffa29-165">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ffa29-165">Summary</span></span>

<span data-ttu-id="ffa29-166">W tym instruktażu przedstawiono sposób używania typów wyliczeniowych z Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="ffa29-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 

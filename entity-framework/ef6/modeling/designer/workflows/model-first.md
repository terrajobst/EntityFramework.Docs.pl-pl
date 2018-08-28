---
title: Najpierw — model EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: c21592b27fa752532f5ede5923d0bd751f0bf372
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998118"
---
# <a name="model-first"></a><span data-ttu-id="06b62-102">Najpierw modelu</span><span class="sxs-lookup"><span data-stu-id="06b62-102">Model First</span></span>
<span data-ttu-id="06b62-103">W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwoju pierwszego modelu używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06b62-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="06b62-104">Model umożliwia najpierw utworzyć nowy model przy użyciu programu Entity Framework Designer, a następnie wygenerować schemat bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="06b62-105">Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06b62-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="06b62-106">Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="06b62-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="06b62-107">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="06b62-107">Watch the video</span></span>
<span data-ttu-id="06b62-108">W tym przewodniku krok po kroku i wideo zawierają wprowadzenie do rozwoju pierwszego modelu używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06b62-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="06b62-109">Model umożliwia najpierw utworzyć nowy model przy użyciu programu Entity Framework Designer, a następnie wygenerować schemat bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="06b62-110">Model jest przechowywany w pliku EDMX (z rozszerzeniem edmx) i można wyświetlać i edytować w Projektancie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06b62-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="06b62-111">Klasy, które możesz korzystać z aplikacji są generowane automatycznie z pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="06b62-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="06b62-112">**Osoba prezentująca**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="06b62-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="06b62-113">**Film wideo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="06b62-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="06b62-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="06b62-114">Pre-Requisites</span></span>

<span data-ttu-id="06b62-115">Musisz mieć program Visual Studio 2010 lub Visual Studio 2012 są zainstalowane w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="06b62-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="06b62-116">Jeśli używasz programu Visual Studio 2010, należy również mieć [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="06b62-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="06b62-117">1. Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="06b62-117">1. Create the Application</span></span>

<span data-ttu-id="06b62-118">Aby zachować ich prostotę zamierzamy utworzyć aplikację konsoli podstawowe, która używa pierwszego modelu do dostępu do danych:</span><span class="sxs-lookup"><span data-stu-id="06b62-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="06b62-119">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06b62-119">Open Visual Studio</span></span>
-   <span data-ttu-id="06b62-120">**Plik —&gt; New -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="06b62-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="06b62-121">Wybierz **Windows** menu po lewej stronie i **aplikacji konsoli**</span><span class="sxs-lookup"><span data-stu-id="06b62-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="06b62-122">Wprowadź **ModelFirstSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="06b62-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="06b62-123">Wybierz **OK**</span><span class="sxs-lookup"><span data-stu-id="06b62-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="06b62-124">2. Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="06b62-124">2. Create Model</span></span>

<span data-ttu-id="06b62-125">Zamierzamy korzystania z programu Entity Framework Designer, który wchodzi w skład programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="06b62-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="06b62-126">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="06b62-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="06b62-127">Wybierz **danych** menu po lewej stronie i następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="06b62-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="06b62-128">Wprowadź **BloggingModel** jako nazwę i kliknij przycisk **OK**, zostanie uruchomiony Kreator modelu Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="06b62-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="06b62-129">Wybierz **pusty Model** i kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="06b62-129">Select **Empty Model** and click **Finish**</span></span>

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

<span data-ttu-id="06b62-131">Entity Framework Designer jest otwierany przy użyciu modelu puste.</span><span class="sxs-lookup"><span data-stu-id="06b62-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="06b62-132">Można teraz rozpocząć dodawanie jednostek, właściwości i skojarzenia do modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="06b62-133">Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **właściwości**</span><span class="sxs-lookup"><span data-stu-id="06b62-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="06b62-134">Zmiana okna właściwości **nazwa kontenera jednostki** do **BloggingContext**
    *jest to nazwa pochodnej kontekst, który zostanie wygenerowany dla Ciebie kontekstu reprezentuje sesję z bazą danych, dzięki czemu nam zapytania i Zapisz dane*</span><span class="sxs-lookup"><span data-stu-id="06b62-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="06b62-135">Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Dodaj nowy -&gt; jednostki...**</span><span class="sxs-lookup"><span data-stu-id="06b62-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="06b62-136">Wprowadź **Blog** jako nazwę podmiotu i **BlogId** jako nazwę klucza i kliknij przycisk **OK**</span><span class="sxs-lookup"><span data-stu-id="06b62-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="06b62-138">Kliknij prawym przyciskiem myszy na nową jednostkę na projekt powierzchni i wybierz **Dodaj nowy -&gt; właściwość skalarną**, wprowadź **nazwa** jako nazwę właściwości.</span><span class="sxs-lookup"><span data-stu-id="06b62-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="06b62-139">Powtórz ten proces, aby dodać **adresu Url** właściwości.</span><span class="sxs-lookup"><span data-stu-id="06b62-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="06b62-140">Kliknij prawym przyciskiem myszy **adresu Url** właściwość projekt powierzchni i wybierz **właściwości**, zmiana okna właściwości **Nullable** ustawienie **True** 
     *Dzięki temu można zapisać w blogu do bazy danych bez przypisywania go na adres Url*</span><span class="sxs-lookup"><span data-stu-id="06b62-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="06b62-141">Przy użyciu technik, możesz po prostu dzięki modelom uczenia, Dodaj **wpis** jednostki o **PostId** właściwości klucza</span><span class="sxs-lookup"><span data-stu-id="06b62-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="06b62-142">Dodaj **tytuł** i **zawartości** właściwości skalarne można **wpis** jednostki</span><span class="sxs-lookup"><span data-stu-id="06b62-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="06b62-143">Skoro już mamy kilka jednostek, nadszedł czas na dodawanie skojarzenia (lub relacji) między nimi.</span><span class="sxs-lookup"><span data-stu-id="06b62-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="06b62-144">Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Dodaj nowy -&gt; skojarzenie...**</span><span class="sxs-lookup"><span data-stu-id="06b62-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="06b62-145">Wprowadzić jeden z końców relacji wskazują **Blog** z wartością liczebności równą **jeden** i innych punktu końcowego do **wpis** z wartością liczebności równą **wiele** 
     *Oznacza to, że blogu zawiera wiele wpisów i wpis należy do jednego Blog*</span><span class="sxs-lookup"><span data-stu-id="06b62-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="06b62-146">Upewnij się, **dodawania właściwości klucza obcego do jednostki "Post"** pole jest zaznaczone, a kliknij **OK**</span><span class="sxs-lookup"><span data-stu-id="06b62-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="06b62-148">W efekcie powstał prosty model, który możemy Generuj z bazy danych i umożliwia odczytywanie i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="06b62-150">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="06b62-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="06b62-151">Jeśli pracujesz w programie Visual Studio 2010 istnieją pewne dodatkowe czynności, które należy wykonać uaktualnienie do najnowszej wersji programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06b62-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="06b62-152">Uaktualnienie jest ważne, ponieważ daje ona dostęp do ulepszone powierzchni interfejsu API, który jest znacznie łatwiejsze do użycia, a także najnowsze poprawki.</span><span class="sxs-lookup"><span data-stu-id="06b62-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="06b62-153">Najpierw up, musimy pobrać najnowszą wersję programu Entity Framework z NuGet.</span><span class="sxs-lookup"><span data-stu-id="06b62-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="06b62-154">**Project —&gt; Zarządzaj pakietami NuGet... ** 
     *Jeśli nie masz **Zarządzaj pakietami NuGet... ** opcji, należy zainstalować [najnowszej wersji pakietu NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="06b62-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="06b62-155">Wybierz **Online** kartę</span><span class="sxs-lookup"><span data-stu-id="06b62-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="06b62-156">Wybierz **EntityFramework** pakietu</span><span class="sxs-lookup"><span data-stu-id="06b62-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="06b62-157">Kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="06b62-157">Click **Install**</span></span>

<span data-ttu-id="06b62-158">Następnie należy zamienić nasz model, aby wygenerować kod, który korzysta z interfejsu API typu DbContext, który został wprowadzony w nowszych wersjach programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06b62-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="06b62-159">Kliknij prawym przyciskiem myszy na puste miejsce modelu w Projektancie platformy EF, a następnie wybierz pozycję **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="06b62-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="06b62-160">Wybierz **szablonów Online** z menu po lewej stronie i wyszukaj **DbContext**</span><span class="sxs-lookup"><span data-stu-id="06b62-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="06b62-161">Wybierz EF **5.x Generator DbContext dla języka C\#**, wprowadź **BloggingModel** jako nazwę i kliknij przycisk **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="06b62-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="06b62-163">3. Generowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="06b62-163">3. Generating the Database</span></span>

<span data-ttu-id="06b62-164">Biorąc pod uwagę nasz model, platformy Entity Framework można obliczyć schemat bazy danych, które pomogą nam do przechowywania i pobierania danych przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="06b62-165">Serwer bazy danych, który został zainstalowany przy użyciu programu Visual Studio różni się zależnie od wersji programu Visual Studio zostały zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="06b62-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="06b62-166">Jeśli używasz programu Visual Studio 2010 zostanie utworzona baza danych programu SQL Express.</span><span class="sxs-lookup"><span data-stu-id="06b62-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="06b62-167">Jeśli używasz programu Visual Studio 2012, a następnie zostanie utworzona [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="06b62-168">Rozpocznijmy i wygenerować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="06b62-169">Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Generuj z bazy danych z modelu...**</span><span class="sxs-lookup"><span data-stu-id="06b62-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="06b62-170">Kliknij przycisk **nowe połączenie...**</span><span class="sxs-lookup"><span data-stu-id="06b62-170">Click **New Connection…**</span></span> <span data-ttu-id="06b62-171">a następnie określ LocalDB lub SQL Express, w zależności od instalowanej wersji programu Visual Studio, którego używasz, wprowadź **ModelFirst.Blogging** jako nazwa bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="06b62-174">Wybierz **OK** i uzyskasz, jeśli chcesz utworzyć nową bazę danych, wybierz opcję **tak**</span><span class="sxs-lookup"><span data-stu-id="06b62-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="06b62-175">Wybierz **dalej** i Entity Framework Designer obliczy skryptu, aby utworzyć schemat bazy danych</span><span class="sxs-lookup"><span data-stu-id="06b62-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="06b62-176">Gdy skrypt zostanie wyświetlona, kliknij przycisk **Zakończ** i skrypt zostanie dodany do projektu i otwarte</span><span class="sxs-lookup"><span data-stu-id="06b62-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="06b62-177">Kliknij prawym przyciskiem myszy skrypt i wybierz pozycję **Execute**, zostanie wyświetlony monit, aby określić bazę danych, aby nawiązać połączenie, podaj LocalDB lub SQL Server Express, w zależności od instalowanej wersji programu Visual Studio, którego używasz</span><span class="sxs-lookup"><span data-stu-id="06b62-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="06b62-178">4. Odczytywanie i zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="06b62-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="06b62-179">Teraz, gdy model, nadszedł czas na potrzeby dostępu do niektórych danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="06b62-180">Klasy użyjemy na potrzeby dostępu do danych są generowane automatycznie na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="06b62-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="06b62-181">*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 BloggingModel.tt i BloggingModel.Context.tt plików będzie bezpośrednio w ramach projektu, a nie zagnieżdżony w pliku EDMX.*</span><span class="sxs-lookup"><span data-stu-id="06b62-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClasses](~/ef6/media/generatedclasses.png)

<span data-ttu-id="06b62-183">Implementuje metody Main w pliku Program.cs, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="06b62-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="06b62-184">Ten kod tworzy nowe wystąpienie nasz kontekst i używa go do wstawiania nowego bloga.</span><span class="sxs-lookup"><span data-stu-id="06b62-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="06b62-185">Następnie używa zapytania LINQ, aby pobrać wszystkie blogi z bazy danych uporządkowana w kolejności alfabetycznej według tytułu.</span><span class="sxs-lookup"><span data-stu-id="06b62-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="06b62-186">Możesz teraz uruchomić aplikację i ją przetestować.</span><span class="sxs-lookup"><span data-stu-id="06b62-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="06b62-187">5. Obsługa zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="06b62-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="06b62-188">Teraz nadszedł czas, aby wprowadzić pewne zmiany na nasz model, jeśli możemy wprowadzić te zmiany, należy również zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="06b62-189">Rozpoczniemy przez dodanie nowego obiektu użytkownika w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="06b62-190">Dodaj nową **użytkownika** nazwa jednostki za pomocą **Username** jako nazwę klucza i **ciąg** jako typ właściwości klucza</span><span class="sxs-lookup"><span data-stu-id="06b62-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="06b62-192">Kliknij prawym przyciskiem myszy **Username** właściwość projekt powierzchni i wybierz **właściwości**, we właściwościach okna zmiany **MaxLength** ustawienie **50 ** 
     *To ogranicza dane, które mogą być przechowywane w nazwa użytkownika używana do 50 znaków*</span><span class="sxs-lookup"><span data-stu-id="06b62-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="06b62-193">Dodaj **DisplayName** właściwości skalarne **użytkownika** jednostki</span><span class="sxs-lookup"><span data-stu-id="06b62-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="06b62-194">W efekcie powstał aktualizowanego modelu i jesteśmy gotowi zaktualizować bazę danych, aby obsłużyć nasz nowy typ jednostki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="06b62-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="06b62-195">Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Generuj z bazy danych z modelu...** , Platformy entity Framework obliczy skrypt, aby ponownie utworzyć schemat oparty na aktualizowanego modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="06b62-196">Kliknij przycisk **Zakończ**</span><span class="sxs-lookup"><span data-stu-id="06b62-196">Click **Finish**</span></span>
-   <span data-ttu-id="06b62-197">Może odbierać ostrzeżeń o zastąpienie istniejącego skryptu języka DDL i mapowania i magazynu części modelu, kliknij przycisk **tak** dla obu tych ostrzeżeń</span><span class="sxs-lookup"><span data-stu-id="06b62-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="06b62-198">Zaktualizowano skrypt SQL w celu utworzenia bazy danych jest otwarty</span><span class="sxs-lookup"><span data-stu-id="06b62-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="06b62-199">*Skrypt, który jest generowany będzie porzucić wszystkie istniejące tabele i następnie ponownie Utwórz schemat od podstaw. To może działać na potrzeby lokalnego programowania, ale nie jest wygodną wypychania zmian do bazy danych, która została już wdrożona. Jeśli potrzebujesz opublikować zmiany w bazie danych, która została już wdrożona, konieczne będzie edytowanie skryptu lub użyj narzędzia do porównywania schematu do obliczania skryptów migracji.*</span><span class="sxs-lookup"><span data-stu-id="06b62-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="06b62-200">Kliknij prawym przyciskiem myszy skrypt i wybierz pozycję **Execute**, zostanie wyświetlony monit, aby określić bazę danych, aby nawiązać połączenie, podaj LocalDB lub SQL Server Express, w zależności od instalowanej wersji programu Visual Studio, którego używasz</span><span class="sxs-lookup"><span data-stu-id="06b62-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="06b62-201">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="06b62-201">Summary</span></span>

<span data-ttu-id="06b62-202">W tym przewodniku, który przyjrzeliśmy się pierwszy Model programowania, które umożliwiło nam Tworzenie modelu w Projektancie platformy EF, a następnie wygenerować bazę danych z tego modelu.</span><span class="sxs-lookup"><span data-stu-id="06b62-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="06b62-203">Następnie użyliśmy modelu do odczytu i zapisu pewne dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06b62-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="06b62-204">Na koniec mamy zaktualizowany modelu i tworzony ponownie schematu bazy danych, zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="06b62-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>

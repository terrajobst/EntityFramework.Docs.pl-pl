---
title: Model First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182442"
---
# <a name="model-first"></a><span data-ttu-id="cac50-102">Model First</span><span class="sxs-lookup"><span data-stu-id="cac50-102">Model First</span></span>
<span data-ttu-id="cac50-103">Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Model First opracowywania przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cac50-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="cac50-104">Model First umożliwia utworzenie nowego modelu przy użyciu Entity Framework Designer, a następnie wygenerowanie schematu bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="cac50-105">Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="cac50-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="cac50-106">Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="cac50-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="cac50-107">Obejrzyj wideo</span><span class="sxs-lookup"><span data-stu-id="cac50-107">Watch the video</span></span>
<span data-ttu-id="cac50-108">Ten film wideo i przewodnik krok po kroku zawierają wprowadzenie do Model First opracowywania przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cac50-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="cac50-109">Model First umożliwia utworzenie nowego modelu przy użyciu Entity Framework Designer, a następnie wygenerowanie schematu bazy danych z modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="cac50-110">Model jest przechowywany w pliku EDMX (rozszerzenie EDMX) i można go przeglądać i edytować w Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="cac50-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="cac50-111">Klasy, z którymi można korzystać w aplikacji, są generowane automatycznie na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="cac50-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="cac50-112">**Przedstawione przez**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="cac50-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="cac50-113">**Wideo**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="cac50-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="cac50-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cac50-114">Pre-Requisites</span></span>

<span data-ttu-id="cac50-115">Aby ukończyć ten przewodnik, musisz mieć zainstalowany program Visual Studio 2010 lub Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="cac50-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="cac50-116">W przypadku korzystania z programu Visual Studio 2010 należy również zainstalować pakiet [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="cac50-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="cac50-117">1. Utwórz aplikację</span><span class="sxs-lookup"><span data-stu-id="cac50-117">1. Create the Application</span></span>

<span data-ttu-id="cac50-118">Aby zachować prostotę, możemy utworzyć podstawową aplikację konsolową, która używa Model First do uzyskiwania dostępu do danych:</span><span class="sxs-lookup"><span data-stu-id="cac50-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="cac50-119">Otwórz program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cac50-119">Open Visual Studio</span></span>
-   <span data-ttu-id="cac50-120">**Plik —&gt; nowy&gt; projekt...**</span><span class="sxs-lookup"><span data-stu-id="cac50-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="cac50-121">Wybierz pozycję **Windows** z menu po lewej stronie i **aplikacji konsolowej**</span><span class="sxs-lookup"><span data-stu-id="cac50-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="cac50-122">Wprowadź **ModelFirstSample** jako nazwę</span><span class="sxs-lookup"><span data-stu-id="cac50-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="cac50-123">Wybierz **przycisk OK**</span><span class="sxs-lookup"><span data-stu-id="cac50-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="cac50-124">2. Utwórz model</span><span class="sxs-lookup"><span data-stu-id="cac50-124">2. Create Model</span></span>

<span data-ttu-id="cac50-125">Będziemy używać Entity Framework Designer, które są dołączone jako część programu Visual Studio, aby utworzyć nasz model.</span><span class="sxs-lookup"><span data-stu-id="cac50-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="cac50-126">**Projekt —&gt; Dodaj nowy element...**</span><span class="sxs-lookup"><span data-stu-id="cac50-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="cac50-127">Wybierz pozycję **dane** z menu po lewej stronie, a następnie **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="cac50-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="cac50-128">Wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **OK**. spowoduje to uruchomienie Kreatora Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="cac50-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="cac50-129">Wybierz pozycję **pusty model** i kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="cac50-129">Select **Empty Model** and click **Finish**</span></span>

    ![Utwórz pusty model](~/ef6/media/createemptymodel.png)

<span data-ttu-id="cac50-131">Entity Framework Designer jest otwarty z pustym modelem.</span><span class="sxs-lookup"><span data-stu-id="cac50-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="cac50-132">Teraz możemy rozpocząć dodawanie jednostek, właściwości i skojarzeń do modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="cac50-133">Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Właściwości**</span><span class="sxs-lookup"><span data-stu-id="cac50-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="cac50-134">W okno Właściwości zmienić **nazwę kontenera jednostki** na **BloggingContext**
    *jest to nazwa kontekstu pochodnego, który zostanie wygenerowany dla Ciebie, kontekst reprezentuje sesję z bazą danych, umożliwiając nam wykonywanie zapytań i zapisywanie danych*</span><span class="sxs-lookup"><span data-stu-id="cac50-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="cac50-135">Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Dodaj nową&gt; jednostki...**</span><span class="sxs-lookup"><span data-stu-id="cac50-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="cac50-136">Wprowadź **blog** jako nazwę jednostki i **BlogId** jako nazwę klucza, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="cac50-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Dodaj jednostkę blogu](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="cac50-138">Kliknij prawym przyciskiem myszy nową jednostkę na powierzchni projektowej i wybierz polecenie **Dodaj nową&gt; Właściwość skalarna**, wprowadź **nazwę** jako nazwę właściwości.</span><span class="sxs-lookup"><span data-stu-id="cac50-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="cac50-139">Powtórz ten proces, aby dodać właściwość **adresu URL** .</span><span class="sxs-lookup"><span data-stu-id="cac50-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="cac50-140">Kliknij prawym przyciskiem myszy Właściwość **adres URL** na powierzchni projektowej i wybierz polecenie **właściwości**, w okno właściwości zmienić ustawienie **wartości null** na **true** ,
    *to umożliwia zapisanie blogu w bazie danych bez przypisywania go do adresu URL*</span><span class="sxs-lookup"><span data-stu-id="cac50-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="cac50-141">Korzystając z technik, które właśnie uczysz, Dodaj jednostkę **post** z właściwością klucza **PostId**</span><span class="sxs-lookup"><span data-stu-id="cac50-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="cac50-142">Dodawanie właściwości skalarnych **tytułu** i **zawartości** do jednostki **post**</span><span class="sxs-lookup"><span data-stu-id="cac50-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="cac50-143">Teraz, gdy mamy kilka jednostek, czas na dodanie skojarzenia (lub relacji) między nimi.</span><span class="sxs-lookup"><span data-stu-id="cac50-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="cac50-144">Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Dodaj nowe&gt; skojarzenie...**</span><span class="sxs-lookup"><span data-stu-id="cac50-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="cac50-145">Utwórz jeden koniec punktu relacji do **blogu** z liczebność **jednego** i drugiego punktu końcowego, aby **ogłosić** z liczebność **wielu**
    *oznacza to, że blog zawiera wiele ogłoszeń i wpis należy do jednego bloga*</span><span class="sxs-lookup"><span data-stu-id="cac50-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="cac50-146">Upewnij się, że pole **Dodaj właściwości klucza obcego do elementu "post"** jest zaznaczone, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="cac50-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Dodaj skojarzenie MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="cac50-148">Mamy teraz prosty model umożliwiający wygenerowanie bazy danych i używanie jej do odczytu i zapisu danych.</span><span class="sxs-lookup"><span data-stu-id="cac50-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Model początkowy](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="cac50-150">Dodatkowe kroki w programie Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="cac50-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="cac50-151">Jeśli pracujesz w programie Visual Studio 2010, należy wykonać kilka dodatkowych kroków, aby przeprowadzić uaktualnienie do najnowszej wersji programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cac50-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="cac50-152">Aktualizacja jest ważna, ponieważ zapewnia dostęp do ulepszonej powierzchni interfejsu API, która jest znacznie łatwiejsza w użyciu, a także najnowszych poprawek błędów.</span><span class="sxs-lookup"><span data-stu-id="cac50-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="cac50-153">Najpierw należy uzyskać najnowszą wersję Entity Framework z narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="cac50-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="cac50-154">**Projekt —&gt; zarządzać pakietami NuGet...** 
    , \*Jeśli nie masz opcji **Zarządzaj pakietami NuGet...** należy zainstalować [najnowszą wersję programu NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) \*</span><span class="sxs-lookup"><span data-stu-id="cac50-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="cac50-155">Wybierz kartę **online**</span><span class="sxs-lookup"><span data-stu-id="cac50-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="cac50-156">Wybierz pakiet **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="cac50-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="cac50-157">Kliknij przycisk **Instaluj**</span><span class="sxs-lookup"><span data-stu-id="cac50-157">Click **Install**</span></span>

<span data-ttu-id="cac50-158">Następnie musimy wymienić nasz model, aby wygenerować kod, który korzysta z interfejsu API DbContext, który został wprowadzony w nowszych wersjach Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cac50-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="cac50-159">Kliknij prawym przyciskiem myszy pusty punkt w modelu w programie Dr Designer i wybierz polecenie **Dodaj element generowania kodu...**</span><span class="sxs-lookup"><span data-stu-id="cac50-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="cac50-160">Wybierz pozycję **Szablony online** z menu po lewej stronie i Wyszukaj w usłudze **DbContext**</span><span class="sxs-lookup"><span data-stu-id="cac50-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="cac50-161">Wybierz pozycję Dr **5. x DbContext generator dla C\#** , wprowadź **BloggingModel** jako nazwę, a następnie kliknij przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="cac50-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Szablon DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="cac50-163">3. Generowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="cac50-163">3. Generating the Database</span></span>

<span data-ttu-id="cac50-164">Mając nasz model, Entity Framework może obliczyć schemat bazy danych, który umożliwi nam przechowywanie i pobieranie danych przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="cac50-165">Serwer bazy danych zainstalowany przy użyciu programu Visual Studio różni się w zależności od zainstalowanej wersji programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cac50-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="cac50-166">Jeśli używasz programu Visual Studio 2010, tworzysz bazę danych SQL Express.</span><span class="sxs-lookup"><span data-stu-id="cac50-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="cac50-167">Jeśli używasz programu Visual Studio 2012, zostanie utworzona baza danych [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="cac50-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="cac50-168">Przyjrzyjmy się i wygenerujemy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="cac50-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="cac50-169">Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Generuj bazę danych na podstawie modelu...**</span><span class="sxs-lookup"><span data-stu-id="cac50-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="cac50-170">Kliknij pozycję **nowe połączenie...**</span><span class="sxs-lookup"><span data-stu-id="cac50-170">Click **New Connection…**</span></span> <span data-ttu-id="cac50-171">i określ LocalDB lub SQL Express, w zależności od używanej wersji programu Visual Studio, wprowadź **ModelFirst. blog** jako nazwę bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cac50-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDB połączenie MF](~/ef6/media/localdbconnectionmf.png)

    ![Połączenie SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="cac50-174">Wybierz pozycję **OK** . pojawi się monit z prośbą o utworzenie nowej bazy danych, a następnie wybierz pozycję **tak** .</span><span class="sxs-lookup"><span data-stu-id="cac50-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="cac50-175">Wybierz pozycję **dalej** , a Entity Framework Designer będzie obliczać skrypt, aby utworzyć schemat bazy danych</span><span class="sxs-lookup"><span data-stu-id="cac50-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="cac50-176">Po wyświetleniu skryptu kliknij przycisk **Zakończ** , a skrypt zostanie dodany do projektu i otwarty</span><span class="sxs-lookup"><span data-stu-id="cac50-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="cac50-177">Kliknij prawym przyciskiem myszy skrypt i wybierz polecenie **Wykonaj**, zostanie wyświetlony monit o określenie bazy danych, z którą chcesz nawiązać połączenie, określ LocalDB lub SQL Server Express, w zależności od używanej wersji programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cac50-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="cac50-178">4. odczytywanie & zapisywania danych</span><span class="sxs-lookup"><span data-stu-id="cac50-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="cac50-179">Teraz, gdy mamy już model, który jest używany do uzyskiwania dostępu do niektórych danych.</span><span class="sxs-lookup"><span data-stu-id="cac50-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="cac50-180">Klasy, które będą używane do uzyskiwania dostępu do danych są automatycznie generowane na podstawie pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="cac50-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="cac50-181">*Ten zrzut ekranu pochodzi z programu Visual Studio 2012, jeśli używasz programu Visual Studio 2010 pliki BloggingModel.tt i BloggingModel.Context.tt będą znajdować się bezpośrednio w projekcie, a nie zagnieżdżone w pliku EDMX.*</span><span class="sxs-lookup"><span data-stu-id="cac50-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Wygenerowane klasy](~/ef6/media/generatedclasses.png)

<span data-ttu-id="cac50-183">Zaimplementuj metodę Main w Program.cs, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="cac50-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="cac50-184">Ten kod tworzy nowe wystąpienie naszego kontekstu, a następnie używa go do wstawienia nowego bloga.</span><span class="sxs-lookup"><span data-stu-id="cac50-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="cac50-185">Następnie używa zapytania LINQ do pobrania wszystkich blogów z bazy danych uporządkowane alfabetycznie według tytułu.</span><span class="sxs-lookup"><span data-stu-id="cac50-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="cac50-186">Teraz możesz uruchomić aplikację i przetestować ją.</span><span class="sxs-lookup"><span data-stu-id="cac50-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="cac50-187">5. postępowanie z zmianami modelu</span><span class="sxs-lookup"><span data-stu-id="cac50-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="cac50-188">Teraz można wprowadzić pewne zmiany w modelu, gdy wprowadzimy te zmiany, należy również zaktualizować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cac50-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="cac50-189">Zaczniemy od dodania nowej jednostki użytkownika do naszego modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="cac50-190">Dodaj nową nazwę jednostki **użytkownika** z nazwą **użytkownika** jako nazwę klucza i **ciąg** jako typ właściwości klucza</span><span class="sxs-lookup"><span data-stu-id="cac50-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Dodaj jednostkę użytkownika](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="cac50-192">Kliknij prawym przyciskiem myszy Właściwość **username** na powierzchni projektowej i wybierz polecenie **właściwości**, w okno właściwości zmienić ustawienie **MaxLength** na **50**
    *to ogranicza dane, które mogą być przechowywane w nazwie użytkownika do 50 znaków*</span><span class="sxs-lookup"><span data-stu-id="cac50-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="cac50-193">Dodawanie właściwości skalarnej **DisplayName** do jednostki **użytkownika**</span><span class="sxs-lookup"><span data-stu-id="cac50-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="cac50-194">Mamy już zaktualizowany model i jesteśmy gotowi do zaktualizowania bazy danych, aby pomieścić nasz nowy typ jednostki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cac50-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="cac50-195">Kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Generuj bazę danych na podstawie modelu...** , Entity Framework obliczy skrypt, aby odtworzyć schemat w oparciu o zaktualizowany model.</span><span class="sxs-lookup"><span data-stu-id="cac50-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="cac50-196">Kliknij przycisk **Zakończ** .</span><span class="sxs-lookup"><span data-stu-id="cac50-196">Click **Finish**</span></span>
-   <span data-ttu-id="cac50-197">Można otrzymywać ostrzeżenia o zastępowaniu istniejącego skryptu DDL oraz o mapowaniu i magazynowaniu elementów modelu, a następnie kliknąć przycisk **tak** dla obu tych ostrzeżeń</span><span class="sxs-lookup"><span data-stu-id="cac50-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="cac50-198">Zaktualizowany skrypt SQL służący do tworzenia bazy danych jest otwarty dla Ciebie</span><span class="sxs-lookup"><span data-stu-id="cac50-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="cac50-199">*Wygenerowany skrypt spowoduje porzucenie wszystkich istniejących tabel, a następnie ponowne utworzenie schematu od podstaw. Może to współdziałać z programowaniem lokalnym, ale nie jest zdolny do wypychania zmian w bazie danych, która została już wdrożona. Jeśli musisz opublikować zmiany w bazie danych, która została już wdrożona, musisz edytować skrypt lub użyć narzędzia do porównywania schematów, aby obliczyć skrypt migracji.*</span><span class="sxs-lookup"><span data-stu-id="cac50-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="cac50-200">Kliknij prawym przyciskiem myszy skrypt i wybierz polecenie **Wykonaj**, zostanie wyświetlony monit o określenie bazy danych, z którą chcesz nawiązać połączenie, określ LocalDB lub SQL Server Express, w zależności od używanej wersji programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cac50-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="cac50-201">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="cac50-201">Summary</span></span>

<span data-ttu-id="cac50-202">W tym instruktażu oglądamy Model First opracowywanie, które umożliwiają utworzenie modelu w programie Dr Designer, a następnie wygenerowanie bazy danych z tego modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="cac50-203">Następnie korzystamy z modelu, aby odczytywać i zapisywać dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cac50-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="cac50-204">Na koniec Zaktualizowaliśmy model, a następnie ponownie utworzony schemat bazy danych w celu dopasowania go do modelu.</span><span class="sxs-lookup"><span data-stu-id="cac50-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>

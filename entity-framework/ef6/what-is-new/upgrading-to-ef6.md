---
title: Uaktualnianie do programu Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
caps.latest.revision: 3
ms.openlocfilehash: d22f0d686e6b8e3922d37245386af7723bf08ba1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949267"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="588ad-102">Uaktualnianie do programu Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="588ad-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="588ad-103">W poprzednich wersjach programu EF kod został podzielony między podstawowe biblioteki (przede wszystkim System.Data.Entity.dll) dostarczana jako część programu .NET Framework i poza pasmem (OOB) biblioteki (przede wszystkim EntityFramework.dll) dostarczana z pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="588ad-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="588ad-104">Programy EF6 przyjmuje kod z bibliotekami podstawowych i dołącza go do biblioteki OOB.</span><span class="sxs-lookup"><span data-stu-id="588ad-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="588ad-105">Jest to niezbędne w celu umożliwienia EF ma zostać wykonane "open source" i aby można było podlegać ewolucji w różnym tempie z .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="588ad-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="588ad-106">Skutkiem tego jest aplikacji będzie konieczne można odbudować przeniesionych typów.</span><span class="sxs-lookup"><span data-stu-id="588ad-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="588ad-107">Powinna to być proste dla aplikacji, które korzystają z typu DbContext jako wysłane w programie EF 4.1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="588ad-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="588ad-108">Trochę więcej pracy jest wymagane dla aplikacji, które korzystają z obiektu ObjectContext, ale nadal nie jest ciężko.</span><span class="sxs-lookup"><span data-stu-id="588ad-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="588ad-109">Poniżej przedstawiono listę kontrolną czynności, które należy wykonać, aby uaktualnić istniejącą aplikację platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="588ad-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="588ad-110">1. Zainstaluj pakiet NuGet platformy EF6</span><span class="sxs-lookup"><span data-stu-id="588ad-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="588ad-111">Należy uaktualnić do nowego środowiska uruchomieniowego platformy Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="588ad-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="588ad-112">Kliknij prawym przyciskiem myszy na projekt i wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="588ad-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="588ad-113">W obszarze **Online** wybierz opcję **EntityFramework** i kliknij przycisk **instalacji**</span><span class="sxs-lookup"><span data-stu-id="588ad-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="588ad-114">Jeśli poprzednią wersję został zainstalowany pakiet NuGet platformy EntityFramework to spowoduje uaktualnienie go do platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="588ad-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="588ad-115">Alternatywnie można uruchom następujące polecenie z poziomu konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="588ad-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="588ad-116">2. Upewnij się, czy odwołania do zestawów do System.Data.Entity.dll zostały usunięte</span><span class="sxs-lookup"><span data-stu-id="588ad-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="588ad-117">Instalowanie pakietu EF6 NuGet należy automatycznie usunąć wszystkie odwołania do System.Data.Entity z projektu.</span><span class="sxs-lookup"><span data-stu-id="588ad-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="588ad-118">3. Zamień wszystkie modele EF projektanta (EDMX) umożliwia generowanie kodu w wersji 6.x EF</span><span class="sxs-lookup"><span data-stu-id="588ad-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="588ad-119">Jeśli masz wszystkie modele utworzone za pomocą projektanta EF, należy zaktualizować szablonów generowania kodu, aby wygenerować kod zgodny EF6.</span><span class="sxs-lookup"><span data-stu-id="588ad-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="588ad-120">Brak dostępnych obecnie tylko EF 6.x DbContext Generator szablonów dla programu Visual Studio 2012 i 2013.</span><span class="sxs-lookup"><span data-stu-id="588ad-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="588ad-121">Usuwanie istniejących szablonów generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="588ad-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="588ad-122">Te pliki będą zwykle miały nazwy nadane  **\<edmx_file_name\>.tt** i  **\<edmx_file_name\>. Context.TT** i można zagnieździć w pliku edmx w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="588ad-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="588ad-123">Szablony można wybrać w Eksploratorze rozwiązań, a następnie naciśnij klawisz **Del** klawisz, aby je usunąć.</span><span class="sxs-lookup"><span data-stu-id="588ad-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="588ad-124">W projektów witryny sieci Web szablonów będzie nie być zagnieżdżony w pliku edmx, ale wymienione obok niej w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="588ad-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="588ad-125">W projektach VB.NET, musisz włączyć "Pokaż wszystkie pliki" można było wyświetlić pliki zagnieżdżonych szablonów.</span><span class="sxs-lookup"><span data-stu-id="588ad-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="588ad-126">Dodaj odpowiedni szablon generowanie kodu w wersji 6.x EF.</span><span class="sxs-lookup"><span data-stu-id="588ad-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="588ad-127">Otwieranie modelu w Projektancie platformy EF, kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Dodaj element generowanie kodu...**</span><span class="sxs-lookup"><span data-stu-id="588ad-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="588ad-128">Jeśli używasz interfejsu API typu DbContext (zalecane) następnie **EF 6.x DbContext Generator** będą dostępne w ramach **danych** kartę.</span><span class="sxs-lookup"><span data-stu-id="588ad-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="588ad-129">Jeśli używasz programu Visual Studio 2012 należy zainstalować narzędzia EF 6, aby ten szablon.</span><span class="sxs-lookup"><span data-stu-id="588ad-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="588ad-130">Zobacz [uzyskać Entity Framework](~/ef6/fundamentals/install.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="588ad-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="588ad-131">Jeśli korzystasz z interfejsu API obiektu ObjectContext to będzie konieczne wybranie **Online** kartę i wyszukaj **EF 6.x EntityObject Generator**.</span><span class="sxs-lookup"><span data-stu-id="588ad-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="588ad-132">Jeśli jakiekolwiek dostosowania są stosowane do szablonów generowania kodu, należy ponownie zastosować je do zaktualizowanych szablonów.</span><span class="sxs-lookup"><span data-stu-id="588ad-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="588ad-133">4. Aktualizowanie przestrzeni nazw dla wszystkich typów EF core używane</span><span class="sxs-lookup"><span data-stu-id="588ad-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="588ad-134">Przestrzenie nazw dla typu DbContext i Code First nie uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="588ad-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="588ad-135">To oznacza dla wielu aplikacji, które używają programu EF 4.1 lub nowszym będzie trzeba wprowadzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="588ad-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="588ad-136">Typów, takich jak ObjectContext, które były wcześniej System.Data.Entity.dll zostały przeniesione do nowej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="588ad-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="588ad-137">Oznacza to, że może być konieczne zaktualizowanie swoje *przy użyciu* lub *importu* dyrektywy algorytmie EF6.</span><span class="sxs-lookup"><span data-stu-id="588ad-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="588ad-138">Ogólną zasadą zmian przestrzeni nazw jest, że dowolnego typu w System.Data.* jest przenoszony do System.Data.Entity.Core.*.</span><span class="sxs-lookup"><span data-stu-id="588ad-138">The general rule for namespace changes is that any type in System.Data.* is moved to System.Data.Entity.Core.*.</span></span> <span data-ttu-id="588ad-139">Innymi słowy, wystarczy wstawić **Entity.Core.**</span><span class="sxs-lookup"><span data-stu-id="588ad-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="588ad-140">Po dane systemowe.</span><span class="sxs-lookup"><span data-stu-id="588ad-140">after System.Data.</span></span> <span data-ttu-id="588ad-141">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="588ad-141">For example:</span></span>

- <span data-ttu-id="588ad-142">System.Data.EntityException = > dane systemowe. **Entity.Core.** EntityException</span><span class="sxs-lookup"><span data-stu-id="588ad-142">System.Data.EntityException => System.Data.**Entity.Core.** EntityException</span></span>  
- <span data-ttu-id="588ad-143">System.Data.Objects.ObjectContext = > dane systemowe. **Entity.Core.** Objects.ObjectContext</span><span class="sxs-lookup"><span data-stu-id="588ad-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core.** Objects.ObjectContext</span></span>  
- <span data-ttu-id="588ad-144">System.Data.Objects.DataClasses.RelationshipManager = > dane systemowe. **Entity.Core.** Objects.DataClasses.RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="588ad-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core.** Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="588ad-145">Te typy są w *Core* przestrzenie nazw, ponieważ nie są używane bezpośrednio w przypadku większości aplikacji na podstawie typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="588ad-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="588ad-146">Niektóre typy, które były częścią System.Data.Entity.dll są nadal używane najczęściej i bezpośrednio do aplikacji opartych na DbContext i dlatego nie zostały przeniesione do *Core* przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="588ad-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="588ad-147">Są to:</span><span class="sxs-lookup"><span data-stu-id="588ad-147">These are:</span></span>

- <span data-ttu-id="588ad-148">System.Data.EntityState = > dane systemowe. **Jednostki.** EntityState</span><span class="sxs-lookup"><span data-stu-id="588ad-148">System.Data.EntityState => System.Data.**Entity.** EntityState</span></span>  
- <span data-ttu-id="588ad-149">System.Data.Objects.DataClasses.EdmFunctionAttribute = > dane systemowe. **Entity.DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="588ad-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="588ad-150">Ta klasa została zmieniona; Klasa o starej nazwie nadal istnieje i działa, ale jej oznaczone jako przestarzałe.</span><span class="sxs-lookup"><span data-stu-id="588ad-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="588ad-151">System.Data.Objects.EntityFunctions = > dane systemowe. **Entity.DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="588ad-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="588ad-152">Ta klasa została zmieniona; Klasa o starej nazwie nadal istnieje i działa, ale teraz oznaczone jako przestarzałe).</span><span class="sxs-lookup"><span data-stu-id="588ad-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="588ad-153">Przestrzenne klasy (na przykład DbGeography, DbGeometry) zostały przeniesione z System.Data.Spatial = > dane systemowe. **Jednostki.** Przestrzenne</span><span class="sxs-lookup"><span data-stu-id="588ad-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity.** Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="588ad-154">Niektóre typy w przestrzeni nazw System.Data znajdują się w System.Data.dll, który nie jest zestawem EF.</span><span class="sxs-lookup"><span data-stu-id="588ad-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="588ad-155">Te typy nie zostały przeniesione, i dlatego ich przestrzenie nazw pozostają niezmienione.</span><span class="sxs-lookup"><span data-stu-id="588ad-155">These types have not moved and so their namespaces remain unchanged.</span></span>

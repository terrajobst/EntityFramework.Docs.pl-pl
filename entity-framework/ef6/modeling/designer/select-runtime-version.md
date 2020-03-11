---
title: Wybieranie wersji środowiska uruchomieniowego Entity Framework dla modeli programu EF Designer — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418148"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="aee92-102">Wybieranie wersji środowiska uruchomieniowego Entity Framework dla modeli programu EF Designer</span><span class="sxs-lookup"><span data-stu-id="aee92-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="aee92-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="aee92-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="aee92-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="aee92-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="aee92-105">Począwszy od EF6, dodano następujący ekran do projektanta EF, aby umożliwić wybranie wersji środowiska uruchomieniowego, która ma być docelowa podczas tworzenia modelu.</span><span class="sxs-lookup"><span data-stu-id="aee92-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="aee92-106">Ekran pojawi się, gdy Najnowsza wersja Entity Framework nie jest jeszcze zainstalowana w projekcie.</span><span class="sxs-lookup"><span data-stu-id="aee92-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="aee92-107">Jeśli Najnowsza wersja jest już zainstalowana, zostanie użyta domyślnie.</span><span class="sxs-lookup"><span data-stu-id="aee92-107">If the latest version is already installed it will just be used by default.</span></span>

![Wyświetla](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="aee92-109">Element docelowy EF6. x</span><span class="sxs-lookup"><span data-stu-id="aee92-109">Targeting EF6.x</span></span>

<span data-ttu-id="aee92-110">Aby dodać środowisko uruchomieniowe EF6 do projektu, możesz wybrać EF6 z ekranu "Wybierz swoją wersję".</span><span class="sxs-lookup"><span data-stu-id="aee92-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="aee92-111">Po dodaniu EF6u ten ekran zostanie zatrzymany w bieżącym projekcie.</span><span class="sxs-lookup"><span data-stu-id="aee92-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="aee92-112">EF6 zostanie wyłączona, jeśli masz już zainstalowaną starszą wersję EF (ponieważ nie można określić wielu wersji środowiska uruchomieniowego z tego samego projektu).</span><span class="sxs-lookup"><span data-stu-id="aee92-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="aee92-113">Jeśli opcja EF6 nie jest włączona w tym miejscu, wykonaj następujące kroki, aby uaktualnić projekt do EF6:</span><span class="sxs-lookup"><span data-stu-id="aee92-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="aee92-114">Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz pozycję **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="aee92-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="aee92-115">Wybierz **aktualizacje**</span><span class="sxs-lookup"><span data-stu-id="aee92-115">Select **Updates**</span></span>
3.  <span data-ttu-id="aee92-116">Wybierz pozycję **EntityFramework** (Upewnij się, że zamierzasz ją zaktualizować do wybranej wersji)</span><span class="sxs-lookup"><span data-stu-id="aee92-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="aee92-117">Kliknij pozycję **Update** (Aktualizuj)</span><span class="sxs-lookup"><span data-stu-id="aee92-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="aee92-118">Element docelowy EF5. x</span><span class="sxs-lookup"><span data-stu-id="aee92-118">Targeting EF5.x</span></span>

<span data-ttu-id="aee92-119">Aby dodać środowisko uruchomieniowe EF5 do projektu, możesz wybrać EF5 z ekranu "Wybierz swoją wersję".</span><span class="sxs-lookup"><span data-stu-id="aee92-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="aee92-120">Po dodaniu EF5 nadal zobaczysz ekran z opcją EF6 wyłączona.</span><span class="sxs-lookup"><span data-stu-id="aee92-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="aee92-121">Jeśli masz już zainstalowaną wersję EF4. x środowiska uruchomieniowego, zobaczysz tę wersję EF podaną na ekranie, a nie EF5.</span><span class="sxs-lookup"><span data-stu-id="aee92-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="aee92-122">W tej sytuacji można przeprowadzić uaktualnienie do EF5, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="aee92-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="aee92-123">Wybieranie **narzędzi-&gt; Library Package Manager —&gt; konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="aee92-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="aee92-124">Uruchom **install-package EntityFramework — wersja 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="aee92-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="aee92-125">Element docelowy EF4. x</span><span class="sxs-lookup"><span data-stu-id="aee92-125">Targeting EF4.x</span></span>

<span data-ttu-id="aee92-126">Środowisko uruchomieniowe EF4. x można zainstalować w projekcie, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="aee92-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="aee92-127">Wybieranie **narzędzi-&gt; Library Package Manager —&gt; konsoli Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="aee92-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="aee92-128">Uruchom **install-package EntityFramework — wersja 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="aee92-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>

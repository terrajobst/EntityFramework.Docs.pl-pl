---
title: Wybieranie jednostki wersję środowiska uruchomieniowego EF projektanta modeli - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998245"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="964b4-102">Wybieranie jednostki wersję środowiska uruchomieniowego EF projektanta modeli</span><span class="sxs-lookup"><span data-stu-id="964b4-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="964b4-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="964b4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="964b4-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="964b4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="964b4-105">Począwszy od platformy EF6 dodano następujący ekran projektancie platformy EF, aby możliwe było wybrać wersję środowiska uruchomieniowego, który ma pod kątem podczas jego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="964b4-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="964b4-106">Ekran pojawi się na najnowszą wersję programu Entity Framework nie jest już zainstalowany w projekcie.</span><span class="sxs-lookup"><span data-stu-id="964b4-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="964b4-107">Jeśli już jest zainstalowana najnowsza wersja będzie używany tylko domyślnie.</span><span class="sxs-lookup"><span data-stu-id="964b4-107">If the latest version is already installed it will just be used by default.</span></span>

![Ekran](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="964b4-109">Określanie wartości docelowej EF6.x</span><span class="sxs-lookup"><span data-stu-id="964b4-109">Targeting EF6.x</span></span>

<span data-ttu-id="964b4-110">Możesz wybrać platformy EF6 z ekranu wybierz wersję usługi, aby dodać środowiska uruchomieniowego platformy EF6 do projektu.</span><span class="sxs-lookup"><span data-stu-id="964b4-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="964b4-111">Po dodaniu EF6 należy zatrzymać, widzisz ten ekran w bieżącym projekcie.</span><span class="sxs-lookup"><span data-stu-id="964b4-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="964b4-112">EF6 zostanie wyłączona, jeśli masz już starszą wersję programu EF zainstalowany (ponieważ nie może wskazać wiele wersji środowiska uruchomieniowego, w tym samym projekcie).</span><span class="sxs-lookup"><span data-stu-id="964b4-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="964b4-113">Jeśli opcja EF6 nie jest włączona w tym miejscu, wykonaj następujące kroki, aby uaktualnić projekt do EF6:</span><span class="sxs-lookup"><span data-stu-id="964b4-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="964b4-114">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="964b4-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="964b4-115">Wybierz **aktualizacji**</span><span class="sxs-lookup"><span data-stu-id="964b4-115">Select **Updates**</span></span>
3.  <span data-ttu-id="964b4-116">Wybierz **EntityFramework** (Upewnij się, będzie ona konieczność jej zaktualizowania do wersji mają)</span><span class="sxs-lookup"><span data-stu-id="964b4-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="964b4-117">Kliknij przycisk **aktualizacji**</span><span class="sxs-lookup"><span data-stu-id="964b4-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="964b4-118">Określanie wartości docelowej EF5.x</span><span class="sxs-lookup"><span data-stu-id="964b4-118">Targeting EF5.x</span></span>

<span data-ttu-id="964b4-119">Możesz wybrać EF5 z ekranu wybierz wersję usługi, aby dodać środowiska uruchomieniowego EF5 do projektu.</span><span class="sxs-lookup"><span data-stu-id="964b4-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="964b4-120">Po dodaniu EF5 nadal zobaczysz ekran z opcją EF6 wyłączone.</span><span class="sxs-lookup"><span data-stu-id="964b4-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="964b4-121">Jeśli masz EF4.x wersję środowiska uruchomieniowego już zainstalowanego zostanie wyświetlone tej wersji na liście ekranu, a nie EF5 EF.</span><span class="sxs-lookup"><span data-stu-id="964b4-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="964b4-122">W takiej sytuacji można przeprowadzić uaktualnienie do EF5 wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="964b4-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="964b4-123">Wybierz **Tools -&gt; Menedżer pakietów biblioteki -&gt; Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="964b4-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="964b4-124">Uruchom **EntityFramework instalacji pakietu-wersja 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="964b4-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="964b4-125">Określanie wartości docelowej EF4.x</span><span class="sxs-lookup"><span data-stu-id="964b4-125">Targeting EF4.x</span></span>

<span data-ttu-id="964b4-126">Można zainstalować środowisko uruchomieniowe EF4.x do projektu wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="964b4-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="964b4-127">Wybierz **Tools -&gt; Menedżer pakietów biblioteki -&gt; Konsola Menedżera pakietów**</span><span class="sxs-lookup"><span data-stu-id="964b4-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="964b4-128">Uruchom **EntityFramework instalacji pakietu — w wersji 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="964b4-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>

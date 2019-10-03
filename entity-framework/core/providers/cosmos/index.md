---
title: Dostawca Azure Cosmos DB — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 96686256bb93f5828bb21fed167eb57812806390
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813543"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="903e8-102">Dostawca Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="903e8-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="903e8-103">Ten dostawca jest nowy w EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="903e8-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="903e8-104">Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="903e8-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="903e8-105">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="903e8-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="903e8-106">Zdecydowanie zalecamy zapoznanie się z [dokumentacją Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) przed przeczytaniem tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="903e8-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="903e8-107">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="903e8-107">Install</span></span>

<span data-ttu-id="903e8-108">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="903e8-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="903e8-109">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="903e8-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="903e8-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="903e8-110">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="903e8-111">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="903e8-111">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="903e8-112">Przykład tego artykułu można wyświetlić [w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="903e8-112">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="903e8-113">Podobnie jak w przypadku innych dostawców, pierwszym krokiem jest `UseCosmos`wywołanie:</span><span class="sxs-lookup"><span data-stu-id="903e8-113">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="903e8-114">Punkt końcowy i klucz są stałe tutaj dla uproszczenia, ale w aplikacji produkcyjnej powinny być [przechowywane securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="903e8-114">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="903e8-115">W tym przykładzie `Order` jest prostą jednostką z odwołaniem do [typu](../../modeling/owned-entities.md) `StreetAddress`będącego właścicielem.</span><span class="sxs-lookup"><span data-stu-id="903e8-115">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="903e8-116">Zapisywanie i quering danych jest zgodna z normalnym wzorcem EF:</span><span class="sxs-lookup"><span data-stu-id="903e8-116">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="903e8-117">Wywołanie `EnsureCreated` jest niezbędne do utworzenia wymaganych kolekcji i wstawienia [danych inicjatora](../../modeling/data-seeding.md) , jeśli są obecne w modelu.</span><span class="sxs-lookup"><span data-stu-id="903e8-117">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="903e8-118">Jednak `EnsureCreated` powinien być wywoływany tylko podczas wdrażania, nie normalnego działania, ponieważ może to powodować problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="903e8-118">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="903e8-119">Dostosowanie modelu specyficznego dla Cosmos</span><span class="sxs-lookup"><span data-stu-id="903e8-119">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="903e8-120">Domyślnie wszystkie typy jednostek są mapowane do tego samego kontenera o nazwie po kontekście pochodnym (`"OrderContext"` w tym przypadku).</span><span class="sxs-lookup"><span data-stu-id="903e8-120">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="903e8-121">Aby zmienić domyślną nazwę kontenera, użyj `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="903e8-121">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="903e8-122">Aby zmapować typ jednostki na inne użycie `ToContainer`kontenera:</span><span class="sxs-lookup"><span data-stu-id="903e8-122">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="903e8-123">Aby zidentyfikować typ jednostki, którą dany element reprezentuje EF Core dodaje wartość rozróżniacza nawet wtedy, gdy nie ma żadnych pochodnych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="903e8-123">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="903e8-124">[Można zmienić](../../modeling/inheritance.md)nazwę i wartość rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="903e8-124">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="903e8-125">Jednostki osadzone</span><span class="sxs-lookup"><span data-stu-id="903e8-125">Embedded Entities</span></span>

<span data-ttu-id="903e8-126">Dla jednostek należących do Cosmos są osadzone w tym samym elemencie co właściciel.</span><span class="sxs-lookup"><span data-stu-id="903e8-126">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="903e8-127">Aby zmienić nazwę właściwości, użyj `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="903e8-127">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="903e8-128">W przypadku tej konfiguracji kolejność z powyższego przykładu jest przechowywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="903e8-128">With this configuration the order from the example above is stored like this:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="903e8-129">Kolekcje obiektów będących własnością są również osadzone.</span><span class="sxs-lookup"><span data-stu-id="903e8-129">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="903e8-130">W następnym przykładzie użyjemy `Distributor` klasy z `StreetAddress`kolekcją:</span><span class="sxs-lookup"><span data-stu-id="903e8-130">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="903e8-131">Jednostki posiadane nie muszą podawać jawnych wartości klucza do zapisania:</span><span class="sxs-lookup"><span data-stu-id="903e8-131">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="903e8-132">Zostaną one utrwalone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="903e8-132">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

<span data-ttu-id="903e8-133">Wewnętrznie EF Core zawsze muszą mieć unikatowe wartości klucza dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="903e8-133">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="903e8-134">Klucz podstawowy utworzony domyślnie dla kolekcji posiadanych typów składa się z właściwości klucza obcego wskazujących właściciela i `int` Właściwość odpowiadającą indeksowi w tablicy JSON.</span><span class="sxs-lookup"><span data-stu-id="903e8-134">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="903e8-135">Aby można było użyć tego interfejsu API wprowadzania wartości:</span><span class="sxs-lookup"><span data-stu-id="903e8-135">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="903e8-136">W razie potrzeby można zmienić domyślny klucz podstawowy dla typów jednostek posiadanych, ale następnie należy podać wartości kluczy.</span><span class="sxs-lookup"><span data-stu-id="903e8-136">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="903e8-137">Praca z odłączonymi jednostkami</span><span class="sxs-lookup"><span data-stu-id="903e8-137">Working with Disconnected Entities</span></span>

<span data-ttu-id="903e8-138">Każdy element musi mieć `id` wartość unikatową dla danego klucza partycji.</span><span class="sxs-lookup"><span data-stu-id="903e8-138">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="903e8-139">Domyślnie EF Core generuje wartość przez połączenie rozróżniacza i wartości klucza podstawowego przy użyciu znaku "|" jako ogranicznika.</span><span class="sxs-lookup"><span data-stu-id="903e8-139">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="903e8-140">Wartości klucza są generowane tylko wtedy, gdy jednostka przejdzie `Added` do stanu.</span><span class="sxs-lookup"><span data-stu-id="903e8-140">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="903e8-141">Może to spowodować problem podczas [dołączania jednostek](../../saving/disconnected-entities.md) , jeśli nie mają `id` właściwości w typie .NET do przechowywania wartości.</span><span class="sxs-lookup"><span data-stu-id="903e8-141">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="903e8-142">Aby obejść to ograniczenie, można utworzyć i ustawić `id` wartość ręcznie lub oznaczyć jednostkę jako pierwszą dodaną, a następnie zmienić jej stan na żądany:</span><span class="sxs-lookup"><span data-stu-id="903e8-142">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="903e8-143">Jest to otrzymany kod JSON:</span><span class="sxs-lookup"><span data-stu-id="903e8-143">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```

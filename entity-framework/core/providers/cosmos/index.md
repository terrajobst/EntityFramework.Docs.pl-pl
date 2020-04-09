---
title: Dostawca bazy danych usługi Azure Cosmos — EF Core
description: Dokumentacja dla dostawcy bazy danych, która umożliwia program Entity Framework Core do użycia z interfejsem API SQL usługi Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 74284bf78f404e376436a1ef5d5933186c85ae49
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417320"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="ba4a8-103">Ef Core Dostawca bazy danych usługi Azure Cosmos</span><span class="sxs-lookup"><span data-stu-id="ba4a8-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="ba4a8-104">Ten dostawca jest nowy w EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="ba4a8-105">Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z usługi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="ba4a8-106">Dostawca jest utrzymywany w ramach [projektu core entity framework](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="ba4a8-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="ba4a8-107">Zdecydowanie zaleca się zapoznanie się z [dokumentacją usługi Azure Cosmos DB](/azure/cosmos-db/introduction) przed przeczytaniem tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="ba4a8-108">Ten dostawca działa tylko z interfejsem API SQL usługi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="ba4a8-109">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="ba4a8-109">Install</span></span>

<span data-ttu-id="ba4a8-110">Zainstaluj [pakiet Microsoft.EntityFrameworkCore.Cosmos NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="ba4a8-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="ba4a8-111">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="ba4a8-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[<span data-ttu-id="ba4a8-112">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba4a8-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="ba4a8-113">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="ba4a8-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="ba4a8-114">Możesz wyświetlić ten przykład artykułu [na GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="ba4a8-114">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="ba4a8-115">Podobnie jak w przypadku innych dostawców pierwszym krokiem jest wywołanie [UseCosmos:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="ba4a8-116">Punkt końcowy i klucz są zakodowane w tym miejscu dla uproszczenia, ale w aplikacji produkcyjnej powinny one być [bezpiecznie przechowywane.](/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securely](/aspnet/core/security/app-secrets#secret-manager).</span></span>

<span data-ttu-id="ba4a8-117">W tym `Order` przykładzie jest prosta jednostka z odwołaniem do [typu](../../modeling/owned-entities.md) `StreetAddress`własnością .</span><span class="sxs-lookup"><span data-stu-id="ba4a8-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="ba4a8-118">Zapisywanie i quering danych następuje normalny wzorzec EF:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="ba4a8-119">Wywołanie [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) jest konieczne do utworzenia wymaganych kontenerów i wstawieniu [danych źródłowych,](../../modeling/data-seeding.md) jeśli są obecne w modelu.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="ba4a8-120">Jednak `EnsureCreatedAsync` powinny być wywoływane tylko podczas wdrażania, a nie normalnej pracy, ponieważ może to spowodować problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="ba4a8-121">Dostosowywanie modelu specyficznego dla kosmosu</span><span class="sxs-lookup"><span data-stu-id="ba4a8-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="ba4a8-122">Domyślnie wszystkie typy jednostek są mapowane do tego`"OrderContext"` samego kontenera, nazwany po kontekście pochodnym (w tym przypadku).</span><span class="sxs-lookup"><span data-stu-id="ba4a8-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="ba4a8-123">Aby zmienić domyślną nazwę kontenera, użyj [hasdefaultcontainer:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="ba4a8-124">Aby zamapować typ jednostki na inny kontener, użyj [Funkcji DoKontainera:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="ba4a8-125">Aby zidentyfikować typ jednostki, który reprezentuje ef core dodaje wartość rozróżniacza, nawet jeśli nie ma żadnych typów jednostek pochodnych.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="ba4a8-126">Nazwę i wartość dyskryminatora [można zmienić](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="ba4a8-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="ba4a8-127">Jeśli żaden inny typ jednostki nie będzie kiedykolwiek przechowywany w tym samym kontenerze, dyskryminator może zostać usunięty, wywołując [hasnodiscriminator:](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="ba4a8-128">Klawisze partycji</span><span class="sxs-lookup"><span data-stu-id="ba4a8-128">Partition keys</span></span>

<span data-ttu-id="ba4a8-129">Domyślnie EF Core utworzy kontenery `"__partitionKey"` z kluczem partycji ustawionym na bez podaniu żadnej wartości dla niego podczas wstawiania elementów.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="ba4a8-130">Jednak aby w pełni wykorzystać możliwości wydajności usługi Azure Cosmos, należy użyć [starannie wybranego klucza partycji.](/azure/cosmos-db/partition-data)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="ba4a8-131">Można go skonfigurować, wywołując [HasPartitionKey:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="ba4a8-132">Właściwość klucza partycji może być dowolnego typu, o ile jest [konwertowana na ciąg](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="ba4a8-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="ba4a8-133">Po skonfigurowaniu właściwość klucza partycji powinna mieć zawsze wartość niezerową.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="ba4a8-134">Podczas wystawiania kwerendy warunek można dodać, aby uczynić go single-partycji.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="ba4a8-135">Encje osadzone</span><span class="sxs-lookup"><span data-stu-id="ba4a8-135">Embedded entities</span></span>

<span data-ttu-id="ba4a8-136">Dla jednostek należących do usługi Cosmos są osadzone w tym samym elemencie co właściciel.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="ba4a8-137">Aby zmienić nazwę właściwości, użyj [właściwości ToJsonProperty:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)</span><span class="sxs-lookup"><span data-stu-id="ba4a8-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="ba4a8-138">W tej konfiguracji kolejność z powyższego przykładu jest przechowywana w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-138">With this configuration the order from the example above is stored like this:</span></span>

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="ba4a8-139">Osadzone są również kolekcje jednostek będących własnością.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="ba4a8-140">W następnym przykładzie użyjemy `Distributor` klasy z `StreetAddress`kolekcją:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="ba4a8-141">Jednostki będące własnością nie muszą podawać jawnych wartości kluczy, które mają być przechowywane:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="ba4a8-142">Będą one kontynuowane w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-142">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
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

<span data-ttu-id="ba4a8-143">Wewnętrznie EF Core zawsze musi mieć unikatowe wartości klucza dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="ba4a8-144">Klucz podstawowy utworzony domyślnie dla kolekcji typów posiadanych składa się z właściwości `int` klucza obcego wskazującego właściciela i właściwości odpowiadającej indeksowi w tablicy JSON.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="ba4a8-145">Aby pobrać te wartości, można użyć interfejsu API wejścia:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="ba4a8-146">W razie potrzeby można zmienić domyślny klucz podstawowy dla typów jednostek należących do jednostki, ale wartości klucza powinny być jawnie podane.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="ba4a8-147">Praca z odłączonych encjami</span><span class="sxs-lookup"><span data-stu-id="ba4a8-147">Working with disconnected entities</span></span>

<span data-ttu-id="ba4a8-148">Każdy element musi `id` mieć wartość, która jest unikatowa dla danego klucza partycji.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="ba4a8-149">Domyślnie EF Core generuje wartość, konkadując dyskryminujący i podstawowe wartości klucza, używając '|' jako ogranicznika.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="ba4a8-150">Wartości klucza są generowane tylko wtedy, gdy jednostka wchodzi w `Added` stan.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="ba4a8-151">Może to stanowić problem podczas [dołączania jednostek,](../../saving/disconnected-entities.md) `id` jeśli nie mają właściwości typu .NET do przechowywania wartości.</span><span class="sxs-lookup"><span data-stu-id="ba4a8-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="ba4a8-152">Aby obejść to ograniczenie można `id` utworzyć i ustawić wartość ręcznie lub oznaczyć jednostkę jako dodaną, a następnie zmieniając ją na żądany stan:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="ba4a8-153">Jest to wynikowy JSON:</span><span class="sxs-lookup"><span data-stu-id="ba4a8-153">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```

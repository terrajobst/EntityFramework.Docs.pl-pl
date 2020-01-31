---
title: Dostawca Azure Cosmos DB — EF Core
description: Dokumentacja dostawcy bazy danych, która umożliwia Entity Framework Core do użycia z interfejsem API SQL Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 7451ce6e8d5d7078b3f56a6865aa7698e6fc63ca
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888125"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="0499d-103">Dostawca Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="0499d-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="0499d-104">Ten dostawca jest nowy w EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="0499d-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="0499d-105">Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0499d-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="0499d-106">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="0499d-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="0499d-107">Zdecydowanie zalecamy zapoznanie się z [dokumentacją Azure Cosmos DB](/azure/cosmos-db/introduction) przed przeczytaniem tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0499d-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="0499d-108">Ten dostawca współpracuje tylko z interfejsem API SQL Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0499d-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="0499d-109">Instalacja programu</span><span class="sxs-lookup"><span data-stu-id="0499d-109">Install</span></span>

<span data-ttu-id="0499d-110">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="0499d-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="0499d-111">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0499d-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="0499d-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0499d-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="0499d-113">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="0499d-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="0499d-114">Przykład tego artykułu można wyświetlić [w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="0499d-114">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="0499d-115">Podobnie jak w przypadku innych dostawców, pierwszym krokiem jest wywołanie [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span><span class="sxs-lookup"><span data-stu-id="0499d-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="0499d-116">Punkt końcowy i klucz są stałe tutaj dla uproszczenia, ale w aplikacji produkcyjnej powinny być [bezpiecznie przechowywane](/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="0499d-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securely](/aspnet/core/security/app-secrets#secret-manager).</span></span>

<span data-ttu-id="0499d-117">W tym przykładzie `Order` jest prostą jednostką z odwołaniem do [typu należącego](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="0499d-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="0499d-118">Zapisywanie i quering danych jest zgodna z normalnym wzorcem EF:</span><span class="sxs-lookup"><span data-stu-id="0499d-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="0499d-119">Wywołanie [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) jest niezbędne do utworzenia wymaganych kontenerów i wstawienia [danych inicjatora](../../modeling/data-seeding.md) , jeśli istnieją one w modelu.</span><span class="sxs-lookup"><span data-stu-id="0499d-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="0499d-120">Jednakże `EnsureCreatedAsync` powinny być wywoływane tylko podczas wdrażania, a nie normalnego działania, ponieważ może to spowodować problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="0499d-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="0499d-121">Dostosowanie modelu specyficznego dla Cosmos</span><span class="sxs-lookup"><span data-stu-id="0499d-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="0499d-122">Domyślnie wszystkie typy jednostek są mapowane do tego samego kontenera o nazwie po kontekście pochodnym (w tym przypadku`"OrderContext"` w tym przypadku).</span><span class="sxs-lookup"><span data-stu-id="0499d-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="0499d-123">Aby zmienić domyślną nazwę kontenera, użyj [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span><span class="sxs-lookup"><span data-stu-id="0499d-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="0499d-124">Aby zmapować typ jednostki na inny kontener, użyj [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span><span class="sxs-lookup"><span data-stu-id="0499d-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="0499d-125">Aby zidentyfikować typ jednostki, którą dany element reprezentuje EF Core dodaje wartość rozróżniacza nawet wtedy, gdy nie ma żadnych pochodnych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="0499d-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="0499d-126">[Można zmienić](../../modeling/inheritance.md)nazwę i wartość rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="0499d-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="0499d-127">Jeśli żaden inny typ jednostki nie zostanie kiedykolwiek zapisany w tym samym kontenerze, można usunąć rozróżniacz przez wywołanie [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span><span class="sxs-lookup"><span data-stu-id="0499d-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="0499d-128">Klucze partycji</span><span class="sxs-lookup"><span data-stu-id="0499d-128">Partition keys</span></span>

<span data-ttu-id="0499d-129">Domyślnie EF Core utworzy kontenery z ustawionym kluczem partycji na `"__partitionKey"` bez podawania żadnej wartości przy wstawianiu elementów.</span><span class="sxs-lookup"><span data-stu-id="0499d-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="0499d-130">Aby w pełni wykorzystać możliwości wydajności usługi Azure Cosmos, należy użyć [starannie wybranego klucza partycji](/azure/cosmos-db/partition-data) .</span><span class="sxs-lookup"><span data-stu-id="0499d-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="0499d-131">Można go skonfigurować przez wywołanie [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span><span class="sxs-lookup"><span data-stu-id="0499d-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="0499d-132">Właściwość klucza partycji może być dowolnego typu, o ile jest [konwertowana na ciąg](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="0499d-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="0499d-133">Po skonfigurowaniu właściwości klucza partycji zawsze powinna istnieć wartość inna niż null.</span><span class="sxs-lookup"><span data-stu-id="0499d-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="0499d-134">Podczas wystawiania zapytania można dodać warunek, aby udostępnić go pojedynczej partycji.</span><span class="sxs-lookup"><span data-stu-id="0499d-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="0499d-135">Jednostki osadzone</span><span class="sxs-lookup"><span data-stu-id="0499d-135">Embedded entities</span></span>

<span data-ttu-id="0499d-136">Dla jednostek należących do Cosmos są osadzone w tym samym elemencie co właściciel.</span><span class="sxs-lookup"><span data-stu-id="0499d-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="0499d-137">Aby zmienić nazwę właściwości, użyj [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span><span class="sxs-lookup"><span data-stu-id="0499d-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="0499d-138">W przypadku tej konfiguracji kolejność z powyższego przykładu jest przechowywana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0499d-138">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="0499d-139">Kolekcje obiektów będących własnością są również osadzone.</span><span class="sxs-lookup"><span data-stu-id="0499d-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="0499d-140">W następnym przykładzie użyjemy klasy `Distributor` z kolekcją `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="0499d-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="0499d-141">Jednostki posiadane nie muszą podawać jawnych wartości klucza do zapisania:</span><span class="sxs-lookup"><span data-stu-id="0499d-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="0499d-142">Zostaną one utrwalone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="0499d-142">They will be persisted in this way:</span></span>

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

<span data-ttu-id="0499d-143">Wewnętrznie EF Core zawsze muszą mieć unikatowe wartości klucza dla wszystkich śledzonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="0499d-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="0499d-144">Klucz podstawowy utworzony domyślnie dla kolekcji posiadanych typów składa się z właściwości klucza obcego wskazujących właściciela i Właściwość `int` odpowiadającą indeksowi w tablicy JSON.</span><span class="sxs-lookup"><span data-stu-id="0499d-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="0499d-145">Aby można było użyć tego interfejsu API wprowadzania wartości:</span><span class="sxs-lookup"><span data-stu-id="0499d-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="0499d-146">W razie potrzeby można zmienić domyślny klucz podstawowy dla typów jednostek posiadanych, ale następnie należy podać wartości kluczy.</span><span class="sxs-lookup"><span data-stu-id="0499d-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="0499d-147">Praca z odłączonymi jednostkami</span><span class="sxs-lookup"><span data-stu-id="0499d-147">Working with disconnected entities</span></span>

<span data-ttu-id="0499d-148">Każdy element musi mieć `id` wartość, która jest unikatowa dla danego klucza partycji.</span><span class="sxs-lookup"><span data-stu-id="0499d-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="0499d-149">Domyślnie EF Core generuje wartość przez połączenie rozróżniacza i wartości klucza podstawowego przy użyciu znaku "|" jako ogranicznika.</span><span class="sxs-lookup"><span data-stu-id="0499d-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="0499d-150">Wartości klucza są generowane tylko wtedy, gdy jednostka przejdzie do stanu `Added`.</span><span class="sxs-lookup"><span data-stu-id="0499d-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="0499d-151">Może to spowodować problem podczas [dołączania jednostek](../../saving/disconnected-entities.md) , jeśli nie mają właściwości `id` w typie .NET do przechowywania wartości.</span><span class="sxs-lookup"><span data-stu-id="0499d-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="0499d-152">Aby obejść to ograniczenie, można utworzyć i ustawić wartość `id` ręcznie lub oznaczyć jednostkę jako dodaną jako pierwszą, a następnie zmienić jej stan na żądany:</span><span class="sxs-lookup"><span data-stu-id="0499d-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="0499d-153">Jest to otrzymany kod JSON:</span><span class="sxs-lookup"><span data-stu-id="0499d-153">This is the resulting JSON:</span></span>

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

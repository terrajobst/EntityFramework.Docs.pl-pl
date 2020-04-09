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
# <a name="ef-core-azure-cosmos-db-provider"></a>Ef Core Dostawca bazy danych usługi Azure Cosmos

> [!NOTE]
> Ten dostawca jest nowy w EF Core 3.0.

Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z usługi Azure Cosmos DB. Dostawca jest utrzymywany w ramach [projektu core entity framework](https://github.com/aspnet/EntityFrameworkCore).

Zdecydowanie zaleca się zapoznanie się z [dokumentacją usługi Azure Cosmos DB](/azure/cosmos-db/introduction) przed przeczytaniem tej sekcji.

> [!NOTE]
> Ten dostawca działa tylko z interfejsem API SQL usługi Azure Cosmos DB.

## <a name="install"></a>Instalowanie

Zainstaluj [pakiet Microsoft.EntityFrameworkCore.Cosmos NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Rozpoczęcie pracy

> [!TIP]  
> Możesz wyświetlić ten przykład artykułu [na GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Podobnie jak w przypadku innych dostawców pierwszym krokiem jest wywołanie [UseCosmos:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos)

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Punkt końcowy i klucz są zakodowane w tym miejscu dla uproszczenia, ale w aplikacji produkcyjnej powinny one być [bezpiecznie przechowywane.](/aspnet/core/security/app-secrets#secret-manager)

W tym `Order` przykładzie jest prosta jednostka z odwołaniem do [typu](../../modeling/owned-entities.md) `StreetAddress`własnością .

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Zapisywanie i quering danych następuje normalny wzorzec EF:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Wywołanie [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) jest konieczne do utworzenia wymaganych kontenerów i wstawieniu [danych źródłowych,](../../modeling/data-seeding.md) jeśli są obecne w modelu. Jednak `EnsureCreatedAsync` powinny być wywoływane tylko podczas wdrażania, a nie normalnej pracy, ponieważ może to spowodować problemy z wydajnością.

## <a name="cosmos-specific-model-customization"></a>Dostosowywanie modelu specyficznego dla kosmosu

Domyślnie wszystkie typy jednostek są mapowane do tego`"OrderContext"` samego kontenera, nazwany po kontekście pochodnym (w tym przypadku). Aby zmienić domyślną nazwę kontenera, użyj [hasdefaultcontainer:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer)

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Aby zamapować typ jednostki na inny kontener, użyj [Funkcji DoKontainera:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer)

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Aby zidentyfikować typ jednostki, który reprezentuje ef core dodaje wartość rozróżniacza, nawet jeśli nie ma żadnych typów jednostek pochodnych. Nazwę i wartość dyskryminatora [można zmienić](../../modeling/inheritance.md).

Jeśli żaden inny typ jednostki nie będzie kiedykolwiek przechowywany w tym samym kontenerze, dyskryminator może zostać usunięty, wywołując [hasnodiscriminator:](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator)

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Klawisze partycji

Domyślnie EF Core utworzy kontenery `"__partitionKey"` z kluczem partycji ustawionym na bez podaniu żadnej wartości dla niego podczas wstawiania elementów. Jednak aby w pełni wykorzystać możliwości wydajności usługi Azure Cosmos, należy użyć [starannie wybranego klucza partycji.](/azure/cosmos-db/partition-data) Można go skonfigurować, wywołując [HasPartitionKey:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey)

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>Właściwość klucza partycji może być dowolnego typu, o ile jest [konwertowana na ciąg](xref:core/modeling/value-conversions).

Po skonfigurowaniu właściwość klucza partycji powinna mieć zawsze wartość niezerową. Podczas wystawiania kwerendy warunek można dodać, aby uczynić go single-partycji.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Encje osadzone

Dla jednostek należących do usługi Cosmos są osadzone w tym samym elemencie co właściciel. Aby zmienić nazwę właściwości, użyj [właściwości ToJsonProperty:](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty)

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

W tej konfiguracji kolejność z powyższego przykładu jest przechowywana w ten sposób:

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

Osadzone są również kolekcje jednostek będących własnością. W następnym przykładzie użyjemy `Distributor` klasy z `StreetAddress`kolekcją:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Jednostki będące własnością nie muszą podawać jawnych wartości kluczy, które mają być przechowywane:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Będą one kontynuowane w ten sposób:

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

Wewnętrznie EF Core zawsze musi mieć unikatowe wartości klucza dla wszystkich śledzonych jednostek. Klucz podstawowy utworzony domyślnie dla kolekcji typów posiadanych składa się z właściwości `int` klucza obcego wskazującego właściciela i właściwości odpowiadającej indeksowi w tablicy JSON. Aby pobrać te wartości, można użyć interfejsu API wejścia:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> W razie potrzeby można zmienić domyślny klucz podstawowy dla typów jednostek należących do jednostki, ale wartości klucza powinny być jawnie podane.

## <a name="working-with-disconnected-entities"></a>Praca z odłączonych encjami

Każdy element musi `id` mieć wartość, która jest unikatowa dla danego klucza partycji. Domyślnie EF Core generuje wartość, konkadując dyskryminujący i podstawowe wartości klucza, używając '|' jako ogranicznika. Wartości klucza są generowane tylko wtedy, gdy jednostka wchodzi w `Added` stan. Może to stanowić problem podczas [dołączania jednostek,](../../saving/disconnected-entities.md) `id` jeśli nie mają właściwości typu .NET do przechowywania wartości.

Aby obejść to ograniczenie można `id` utworzyć i ustawić wartość ręcznie lub oznaczyć jednostkę jako dodaną, a następnie zmieniając ją na żądany stan:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Jest to wynikowy JSON:

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

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
# <a name="ef-core-azure-cosmos-db-provider"></a>Dostawca Azure Cosmos DB EF Core

> [!NOTE]
> Ten dostawca jest nowy w EF Core 3,0.

Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Azure Cosmos DB. Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Zdecydowanie zalecamy zapoznanie się z [dokumentacją Azure Cosmos DB](/azure/cosmos-db/introduction) przed przeczytaniem tej sekcji.

> [!NOTE]
> Ten dostawca współpracuje tylko z interfejsem API SQL Azure Cosmos DB.

## <a name="install"></a>Instalacja programu

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Wprowadzenie

> [!TIP]  
> Przykład tego artykułu można wyświetlić [w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Podobnie jak w przypadku innych dostawców, pierwszym krokiem jest wywołanie [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Punkt końcowy i klucz są stałe tutaj dla uproszczenia, ale w aplikacji produkcyjnej powinny być [bezpiecznie przechowywane](/aspnet/core/security/app-secrets#secret-manager).

W tym przykładzie `Order` jest prostą jednostką z odwołaniem do [typu należącego](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Zapisywanie i quering danych jest zgodna z normalnym wzorcem EF:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Wywołanie [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) jest niezbędne do utworzenia wymaganych kontenerów i wstawienia [danych inicjatora](../../modeling/data-seeding.md) , jeśli istnieją one w modelu. Jednakże `EnsureCreatedAsync` powinny być wywoływane tylko podczas wdrażania, a nie normalnego działania, ponieważ może to spowodować problemy z wydajnością.

## <a name="cosmos-specific-model-customization"></a>Dostosowanie modelu specyficznego dla Cosmos

Domyślnie wszystkie typy jednostek są mapowane do tego samego kontenera o nazwie po kontekście pochodnym (w tym przypadku`"OrderContext"` w tym przypadku). Aby zmienić domyślną nazwę kontenera, użyj [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Aby zmapować typ jednostki na inny kontener, użyj [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Aby zidentyfikować typ jednostki, którą dany element reprezentuje EF Core dodaje wartość rozróżniacza nawet wtedy, gdy nie ma żadnych pochodnych typów jednostek. [Można zmienić](../../modeling/inheritance.md)nazwę i wartość rozróżniacza.

Jeśli żaden inny typ jednostki nie zostanie kiedykolwiek zapisany w tym samym kontenerze, można usunąć rozróżniacz przez wywołanie [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Klucze partycji

Domyślnie EF Core utworzy kontenery z ustawionym kluczem partycji na `"__partitionKey"` bez podawania żadnej wartości przy wstawianiu elementów. Aby w pełni wykorzystać możliwości wydajności usługi Azure Cosmos, należy użyć [starannie wybranego klucza partycji](/azure/cosmos-db/partition-data) . Można go skonfigurować przez wywołanie [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>Właściwość klucza partycji może być dowolnego typu, o ile jest [konwertowana na ciąg](xref:core/modeling/value-conversions).

Po skonfigurowaniu właściwości klucza partycji zawsze powinna istnieć wartość inna niż null. Podczas wystawiania zapytania można dodać warunek, aby udostępnić go pojedynczej partycji.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Jednostki osadzone

Dla jednostek należących do Cosmos są osadzone w tym samym elemencie co właściciel. Aby zmienić nazwę właściwości, użyj [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

W przypadku tej konfiguracji kolejność z powyższego przykładu jest przechowywana w następujący sposób:

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

Kolekcje obiektów będących własnością są również osadzone. W następnym przykładzie użyjemy klasy `Distributor` z kolekcją `StreetAddress`:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Jednostki posiadane nie muszą podawać jawnych wartości klucza do zapisania:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Zostaną one utrwalone w następujący sposób:

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

Wewnętrznie EF Core zawsze muszą mieć unikatowe wartości klucza dla wszystkich śledzonych jednostek. Klucz podstawowy utworzony domyślnie dla kolekcji posiadanych typów składa się z właściwości klucza obcego wskazujących właściciela i Właściwość `int` odpowiadającą indeksowi w tablicy JSON. Aby można było użyć tego interfejsu API wprowadzania wartości:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> W razie potrzeby można zmienić domyślny klucz podstawowy dla typów jednostek posiadanych, ale następnie należy podać wartości kluczy.

## <a name="working-with-disconnected-entities"></a>Praca z odłączonymi jednostkami

Każdy element musi mieć `id` wartość, która jest unikatowa dla danego klucza partycji. Domyślnie EF Core generuje wartość przez połączenie rozróżniacza i wartości klucza podstawowego przy użyciu znaku "|" jako ogranicznika. Wartości klucza są generowane tylko wtedy, gdy jednostka przejdzie do stanu `Added`. Może to spowodować problem podczas [dołączania jednostek](../../saving/disconnected-entities.md) , jeśli nie mają właściwości `id` w typie .NET do przechowywania wartości.

Aby obejść to ograniczenie, można utworzyć i ustawić wartość `id` ręcznie lub oznaczyć jednostkę jako dodaną jako pierwszą, a następnie zmienić jej stan na żądany:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Jest to otrzymany kod JSON:

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

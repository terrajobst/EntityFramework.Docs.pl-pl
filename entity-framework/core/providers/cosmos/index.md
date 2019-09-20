---
title: Dostawca Azure Cosmos DB — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: c753bb71089c91cbb26b970cddd118645fb18d56
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150820"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Dostawca Azure Cosmos DB EF Core

>[!NOTE]
> Ten dostawca jest nowy w EF Core 3,0.

Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Azure Cosmos DB. Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Zdecydowanie zalecamy zapoznanie się z [dokumentacją Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) przed przeczytaniem tej sekcji.

## <a name="install"></a>Zainstaluj

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a>Rozpocznij

> [!TIP]  
> Przykład tego artykułu można wyświetlić [w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Podobnie jak w przypadku innych dostawców, pierwszym krokiem jest `UseCosmos`wywołanie:[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Punkt końcowy i klucz są stałe tutaj dla uproszczenia, ale w aplikacji produkcyjnej powinny być [przechowywane securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)

W tym przykładzie `Order` jest prostą jednostką z odwołaniem do [typu](../../modeling/owned-entities.md) `StreetAddress`będącego właścicielem.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Zapisywanie i quering danych jest zgodna z normalnym wzorcem EF:[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Wywołanie `EnsureCreated` jest niezbędne do utworzenia wymaganych kolekcji i wstawienia [danych inicjatora](../../modeling/data-seeding.md) , jeśli są obecne w modelu. Jednak `EnsureCreated` powinien być wywoływany tylko podczas wdrażania, nie normalnego działania, ponieważ może to powodować problemy z wydajnością.

## <a name="cosmos-specific-model-customization"></a>Dostosowanie modelu specyficznego dla Cosmos

Domyślnie wszystkie typy jednostek są mapowane do tego samego kontenera o nazwie po kontekście pochodnym (`"OrderContext"` w tym przypadku). Aby zmienić domyślną nazwę kontenera, użyj `HasDefaultContainer`:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Aby zmapować typ jednostki na inne użycie `ToContainer`kontenera:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Aby zidentyfikować typ jednostki, którą dany element reprezentuje EF Core dodaje wartość rozróżniacza nawet wtedy, gdy nie ma żadnych pochodnych typów jednostek. [Można zmienić](../../modeling/inheritance.md)nazwę i wartość rozróżniacza.

## <a name="embedded-entities"></a>Jednostki osadzone

Dla jednostek należących do Cosmos są osadzone w tym samym elemencie co właściciel. Aby zmienić nazwę właściwości, użyj `ToJsonProperty`:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

W przypadku tej konfiguracji kolejność z powyższego przykładu jest przechowywana w następujący sposób:

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

Kolekcje obiektów będących własnością są również osadzone. W następnym przykładzie użyjemy `Distributor` klasy z `StreetAddress`kolekcją:

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

Wewnętrznie EF Core zawsze muszą mieć unikatowe wartości klucza dla wszystkich śledzonych jednostek. Klucz podstawowy utworzony domyślnie dla kolekcji posiadanych typów składa się z właściwości klucza obcego wskazujących właściciela i `int` Właściwość odpowiadającą indeksowi w tablicy JSON. Aby można było użyć tego interfejsu API wprowadzania wartości:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> W razie potrzeby można zmienić domyślny klucz podstawowy dla typów jednostek posiadanych, ale następnie należy podać wartości kluczy.

## <a name="working-with-disconnected-entities"></a>Praca z odłączonymi jednostkami

Każdy element musi mieć `id` wartość unikatową dla danego klucza partycji. Domyślnie EF Core generuje wartość przez połączenie rozróżniacza i wartości klucza podstawowego przy użyciu znaku "|" jako ogranicznika. Wartości klucza są generowane tylko wtedy, gdy jednostka przejdzie `Added` do stanu. Może to spowodować problem podczas [dołączania jednostek](../../saving/disconnected-entities.md) , jeśli nie mają `id` właściwości w typie .NET do przechowywania wartości.

Aby obejść to ograniczenie, można utworzyć i ustawić `id` wartość ręcznie lub oznaczyć jednostkę jako pierwszą dodaną, a następnie zmienić jej stan na żądany:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Jest to otrzymany kod JSON:

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
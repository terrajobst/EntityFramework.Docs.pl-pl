---
title: Rozmieszczanie danych — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 0b11b6b3104b74e09c60c9c455e22f164df493c7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655767"
---
# <a name="data-seeding"></a>Wstępne wypełnianie danych

Rozmieszczanie danych to proces wypełniania bazy danych z początkowym zestawem danych.

Można to zrobić na kilka sposobów w EF Core:

* Modelowanie danych inicjatora
* Dostosowanie migracji ręcznej
* Niestandardowa logika inicjalizacji

## <a name="model-seed-data"></a>Modelowanie danych inicjatora

> [!NOTE]
> Ta funkcja jest nowa w EF Core 2,1.

W przeciwieństwie do EF6, w EF Core, umieszczania danych można kojarzyć z typem jednostki w ramach konfiguracji modelu. Następnie EF Core [migracji](xref:core/managing-schemas/migrations/index) mogą automatycznie obliczyć operacje wstawiania, aktualizowania lub usuwania, które należy zastosować podczas uaktualniania bazy danych do nowej wersji modelu.

> [!NOTE]
> Migracje uwzględniają tylko zmiany modelu podczas określania, jaka operacja powinna zostać wykonana w celu uzyskania danych inicjatora w żądanym stanie. W rezultacie wszelkie zmiany danych wykonanych poza migracją mogą zostać utracone lub przyczyną błędu.

Przykładowo spowoduje to skonfigurowanie danych inicjatora dla `Blog` w `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Aby dodać jednostki, które mają relację, należy określić wartości klucza obcego:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Jeśli typ jednostki ma wszystkie właściwości w stanie cienia, można użyć anonimowej klasy do podania wartości:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Typy jednostek będących własnością mogą być umieszczane w podobny sposób:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) , aby uzyskać więcej kontekstu.

Po dodaniu danych do modelu [migracja](xref:core/managing-schemas/migrations/index) powinna zostać użyta do zastosowania zmian.

> [!TIP]
> Jeśli konieczne jest zastosowanie migracji w ramach automatycznego wdrożenia, można [utworzyć skrypt SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , który będzie można wyświetlić przed wykonaniem.

Alternatywnie możesz użyć `context.Database.EnsureCreated()`, aby utworzyć nową bazę danych zawierającą dane inicjatora, na przykład bazę danych testowej lub użycie dostawcy w pamięci lub dowolnej niepowiązanej bazy danych. Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` nie spowoduje zaktualizowania schematu ani odsadzenia danych w bazie danych. W przypadku relacyjnych baz danych nie należy wywoływać `EnsureCreated()`, jeśli planujesz używać migracji.

### <a name="limitations-of-model-seed-data"></a>Ograniczenia dotyczące danych inicjatora modelu

Ten typ danych inicjatora jest zarządzany przez migracje, a skrypt, aby zaktualizować dane, które już istnieją w bazie danych, musi zostać wygenerowany bez łączenia się z bazą danych. Powoduje to nakładanie pewnych ograniczeń:

* Wartość klucza podstawowego należy określić nawet wtedy, gdy jest zazwyczaj generowana przez bazę danych. Będzie on używany do wykrywania zmian danych między migracjami.
* Poprzednio umieszczone dane zostaną usunięte, jeśli klucz podstawowy zostanie zmieniony w dowolny sposób.

W związku z tym ta funkcja jest najbardziej przydatna w przypadku danych statycznych, które nie są zmieniane poza migracje i nie zależą od innych elementów w bazie danych, na przykład kodów ZIP.

Jeśli scenariusz zawiera dowolne z poniższych, zaleca się użycie niestandardowej logiki inicjalizacji opisanej w ostatniej sekcji:

* Dane tymczasowe do testowania
* Dane, które są zależne od stanu bazy danych
* Dane wymagające generowania wartości kluczy przez bazę danych, w tym jednostki, które używają alternatywnych kluczy jako tożsamości
* Dane wymagające przekształcenia niestandardowego (które nie są obsługiwane przez [konwersje wartości](xref:core/modeling/value-conversions)), takie jak niektóre skróty haseł
* Dane wymagające wywołań zewnętrznego interfejsu API, takie jak ASP.NET Core role tożsamości i tworzenie użytkowników

## <a name="manual-migration-customization"></a>Dostosowanie migracji ręcznej

Po dodaniu migracji zmiany w danych określonych za pomocą `HasData` są przekształcane na wywołania `InsertData()`, `UpdateData()`i `DeleteData()`. Jednym ze sposobów obejścia niektórych ograniczeń `HasData` jest ręczne dodanie tych wywołań lub [operacji niestandardowych](xref:core/managing-schemas/migrations/operations) do migracji.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Niestandardowa logika inicjalizacji

Prostą i wydajną metodą wykonywania operacji umieszczania danych jest użycie [`DbContext.SaveChanges()`](xref:core/saving/index) przed rozpoczęciem wykonywania głównej logiki aplikacji.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Kod inicjujący nie powinien być częścią normalnego wykonywania aplikacji, ponieważ może to powodować problemy współbieżności, gdy działa wiele wystąpień, a także wymaga aplikacji z uprawnieniami do modyfikowania schematu bazy danych.

W zależności od ograniczeń wdrożenia kod inicjalizacji można wykonać na różne sposoby:

* Uruchamianie aplikacji inicjującej lokalnie
* Wdrażanie aplikacji inicjującej za pomocą głównej aplikacji, wywoływanie procedury inicjowania i wyłączanie lub usuwanie aplikacji inicjującej.

Zwykle może to być zautomatyzowane przy użyciu [profilów publikacji](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).

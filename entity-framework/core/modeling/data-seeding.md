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
# <a name="data-seeding"></a><span data-ttu-id="e3c9f-102">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="e3c9f-102">Data Seeding</span></span>

<span data-ttu-id="e3c9f-103">Rozmieszczanie danych to proces wypełniania bazy danych z początkowym zestawem danych.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="e3c9f-104">Można to zrobić na kilka sposobów w EF Core:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-104">There are several ways this can be accomplished in EF Core:</span></span>

* <span data-ttu-id="e3c9f-105">Modelowanie danych inicjatora</span><span class="sxs-lookup"><span data-stu-id="e3c9f-105">Model seed data</span></span>
* <span data-ttu-id="e3c9f-106">Dostosowanie migracji ręcznej</span><span class="sxs-lookup"><span data-stu-id="e3c9f-106">Manual migration customization</span></span>
* <span data-ttu-id="e3c9f-107">Niestandardowa logika inicjalizacji</span><span class="sxs-lookup"><span data-stu-id="e3c9f-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="e3c9f-108">Modelowanie danych inicjatora</span><span class="sxs-lookup"><span data-stu-id="e3c9f-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="e3c9f-109">Ta funkcja jest nowa w EF Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e3c9f-110">W przeciwieństwie do EF6, w EF Core, umieszczania danych można kojarzyć z typem jednostki w ramach konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="e3c9f-111">Następnie EF Core [migracji](xref:core/managing-schemas/migrations/index) mogą automatycznie obliczyć operacje wstawiania, aktualizowania lub usuwania, które należy zastosować podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="e3c9f-112">Migracje uwzględniają tylko zmiany modelu podczas określania, jaka operacja powinna zostać wykonana w celu uzyskania danych inicjatora w żądanym stanie.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="e3c9f-113">W rezultacie wszelkie zmiany danych wykonanych poza migracją mogą zostać utracone lub przyczyną błędu.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="e3c9f-114">Przykładowo spowoduje to skonfigurowanie danych inicjatora dla `Blog` w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="e3c9f-115">Aby dodać jednostki, które mają relację, należy określić wartości klucza obcego:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="e3c9f-116">Jeśli typ jednostki ma wszystkie właściwości w stanie cienia, można użyć anonimowej klasy do podania wartości:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="e3c9f-117">Typy jednostek będących własnością mogą być umieszczane w podobny sposób:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="e3c9f-118">Zobacz [pełny przykładowy projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) , aby uzyskać więcej kontekstu.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="e3c9f-119">Po dodaniu danych do modelu [migracja](xref:core/managing-schemas/migrations/index) powinna zostać użyta do zastosowania zmian.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="e3c9f-120">Jeśli konieczne jest zastosowanie migracji w ramach automatycznego wdrożenia, można [utworzyć skrypt SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , który będzie można wyświetlić przed wykonaniem.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="e3c9f-121">Alternatywnie możesz użyć `context.Database.EnsureCreated()`, aby utworzyć nową bazę danych zawierającą dane inicjatora, na przykład bazę danych testowej lub użycie dostawcy w pamięci lub dowolnej niepowiązanej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="e3c9f-122">Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` nie spowoduje zaktualizowania schematu ani odsadzenia danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="e3c9f-123">W przypadku relacyjnych baz danych nie należy wywoływać `EnsureCreated()`, jeśli planujesz używać migracji.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="e3c9f-124">Ograniczenia dotyczące danych inicjatora modelu</span><span class="sxs-lookup"><span data-stu-id="e3c9f-124">Limitations of model seed data</span></span>

<span data-ttu-id="e3c9f-125">Ten typ danych inicjatora jest zarządzany przez migracje, a skrypt, aby zaktualizować dane, które już istnieją w bazie danych, musi zostać wygenerowany bez łączenia się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="e3c9f-126">Powoduje to nakładanie pewnych ograniczeń:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-126">This imposes some restrictions:</span></span>

* <span data-ttu-id="e3c9f-127">Wartość klucza podstawowego należy określić nawet wtedy, gdy jest zazwyczaj generowana przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="e3c9f-128">Będzie on używany do wykrywania zmian danych między migracjami.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="e3c9f-129">Poprzednio umieszczone dane zostaną usunięte, jeśli klucz podstawowy zostanie zmieniony w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="e3c9f-130">W związku z tym ta funkcja jest najbardziej przydatna w przypadku danych statycznych, które nie są zmieniane poza migracje i nie zależą od innych elementów w bazie danych, na przykład kodów ZIP.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="e3c9f-131">Jeśli scenariusz zawiera dowolne z poniższych, zaleca się użycie niestandardowej logiki inicjalizacji opisanej w ostatniej sekcji:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>

* <span data-ttu-id="e3c9f-132">Dane tymczasowe do testowania</span><span class="sxs-lookup"><span data-stu-id="e3c9f-132">Temporary data for testing</span></span>
* <span data-ttu-id="e3c9f-133">Dane, które są zależne od stanu bazy danych</span><span class="sxs-lookup"><span data-stu-id="e3c9f-133">Data that depends on database state</span></span>
* <span data-ttu-id="e3c9f-134">Dane wymagające generowania wartości kluczy przez bazę danych, w tym jednostki, które używają alternatywnych kluczy jako tożsamości</span><span class="sxs-lookup"><span data-stu-id="e3c9f-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="e3c9f-135">Dane wymagające przekształcenia niestandardowego (które nie są obsługiwane przez [konwersje wartości](xref:core/modeling/value-conversions)), takie jak niektóre skróty haseł</span><span class="sxs-lookup"><span data-stu-id="e3c9f-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="e3c9f-136">Dane wymagające wywołań zewnętrznego interfejsu API, takie jak ASP.NET Core role tożsamości i tworzenie użytkowników</span><span class="sxs-lookup"><span data-stu-id="e3c9f-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="e3c9f-137">Dostosowanie migracji ręcznej</span><span class="sxs-lookup"><span data-stu-id="e3c9f-137">Manual migration customization</span></span>

<span data-ttu-id="e3c9f-138">Po dodaniu migracji zmiany w danych określonych za pomocą `HasData` są przekształcane na wywołania `InsertData()`, `UpdateData()`i `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="e3c9f-139">Jednym ze sposobów obejścia niektórych ograniczeń `HasData` jest ręczne dodanie tych wywołań lub [operacji niestandardowych](xref:core/managing-schemas/migrations/operations) do migracji.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="e3c9f-140">Niestandardowa logika inicjalizacji</span><span class="sxs-lookup"><span data-stu-id="e3c9f-140">Custom initialization logic</span></span>

<span data-ttu-id="e3c9f-141">Prostą i wydajną metodą wykonywania operacji umieszczania danych jest użycie [`DbContext.SaveChanges()`](xref:core/saving/index) przed rozpoczęciem wykonywania głównej logiki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="e3c9f-142">Kod inicjujący nie powinien być częścią normalnego wykonywania aplikacji, ponieważ może to powodować problemy współbieżności, gdy działa wiele wystąpień, a także wymaga aplikacji z uprawnieniami do modyfikowania schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="e3c9f-143">W zależności od ograniczeń wdrożenia kod inicjalizacji można wykonać na różne sposoby:</span><span class="sxs-lookup"><span data-stu-id="e3c9f-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>

* <span data-ttu-id="e3c9f-144">Uruchamianie aplikacji inicjującej lokalnie</span><span class="sxs-lookup"><span data-stu-id="e3c9f-144">Running the initialization app locally</span></span>
* <span data-ttu-id="e3c9f-145">Wdrażanie aplikacji inicjującej za pomocą głównej aplikacji, wywoływanie procedury inicjowania i wyłączanie lub usuwanie aplikacji inicjującej.</span><span class="sxs-lookup"><span data-stu-id="e3c9f-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="e3c9f-146">Zwykle może to być zautomatyzowane przy użyciu [profilów publikacji](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="e3c9f-146">This can usually be automated by using [publish profiles](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>

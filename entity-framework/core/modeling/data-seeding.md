---
title: Wstępne wypełnianie danych — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857432"
---
# <a name="data-seeding"></a><span data-ttu-id="58b10-102">Wstępne wypełnianie danych</span><span class="sxs-lookup"><span data-stu-id="58b10-102">Data Seeding</span></span>

<span data-ttu-id="58b10-103">Wstępne wypełnianie danych polega na wypełnianie bazy danych za pomocą początkowego zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="58b10-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="58b10-104">Istnieje kilka sposobów, które można to zrobić w programie EF Core:</span><span class="sxs-lookup"><span data-stu-id="58b10-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="58b10-105">Modelowanie danych inicjatora</span><span class="sxs-lookup"><span data-stu-id="58b10-105">Model seed data</span></span>
* <span data-ttu-id="58b10-106">Dostosowywanie ręcznej migracji</span><span class="sxs-lookup"><span data-stu-id="58b10-106">Manual migration customization</span></span>
* <span data-ttu-id="58b10-107">Logikę niestandardową inicjalizację</span><span class="sxs-lookup"><span data-stu-id="58b10-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="58b10-108">Modelowanie danych inicjatora</span><span class="sxs-lookup"><span data-stu-id="58b10-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="58b10-109">Ta funkcja jest nowa na platformie EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="58b10-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="58b10-110">W odróżnieniu od w EF6 w programie EF Core wstępne wypełnianie danych może być skojarzony z typem jednostki jako część konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="58b10-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="58b10-111">Następnie programu EF Core [migracje](xref:core/managing-schemas/migrations/index) można automatycznie obliczyć co Wstawianie, aktualizowanie lub usuwanie potrzebę operacji mają być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.</span><span class="sxs-lookup"><span data-stu-id="58b10-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="58b10-112">Podczas określania, jakie operacja powinna być wykonana pobierać dane inicjatora do żądanego stanu, migracje analizuje tylko zmiany modelu.</span><span class="sxs-lookup"><span data-stu-id="58b10-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="58b10-113">Dlatego wszelkie zmiany w danych poza migracje mogą zostać utracone lub nie powodują wystąpienie błędu.</span><span class="sxs-lookup"><span data-stu-id="58b10-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="58b10-114">Na przykład dane zostaną skonfigurowane `Blog` w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="58b10-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="58b10-115">Aby dodać jednostki, które mają relację wartości klucza obcego muszą być określone:</span><span class="sxs-lookup"><span data-stu-id="58b10-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="58b10-116">Jeśli typ jednostki ma wszystkie właściwości w stanie w tle klasa anonimowa może służyć do Podaj wartości:</span><span class="sxs-lookup"><span data-stu-id="58b10-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="58b10-117">Należące do jednostki, który może zostać rozpoczęta typów w podobny sposób:</span><span class="sxs-lookup"><span data-stu-id="58b10-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="58b10-118">Zobacz [pełny przykład projektu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) Aby uzyskać dodatkowy kontekst.</span><span class="sxs-lookup"><span data-stu-id="58b10-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="58b10-119">Po dodaniu danych do modelu, [migracje](xref:core/managing-schemas/migrations/index) powinien być używany do zastosowania zmian.</span><span class="sxs-lookup"><span data-stu-id="58b10-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="58b10-120">Jeśli konieczne jest zastosowanie migracji w ramach zautomatyzowanego wdrażania możesz [Utwórz skrypt SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) można je przeglądać przed wykonaniem.</span><span class="sxs-lookup"><span data-stu-id="58b10-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="58b10-121">Alternatywnie, można użyć `context.Database.EnsureCreated()` do utworzenia nowej bazy danych zawierającej dane inicjatora, na przykład w przypadku bazy danych testów lub za pomocą dostawcy w pamięci lub dowolnej-relation bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58b10-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="58b10-122">Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` zaktualizuje żadnego schematu ani inicjatora w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="58b10-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="58b10-123">Relacyjne bazy danych nie należy wywoływać `EnsureCreated()` Jeśli planujesz użyć migracje.</span><span class="sxs-lookup"><span data-stu-id="58b10-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="58b10-124">Inicjatora danych tego typu jest zarządzana przez migracje i skrypt do aktualizowania danych, który jest już w bazie danych musi zostać wygenerowane bez połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="58b10-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="58b10-125">To nakłada pewne ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="58b10-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="58b10-126">Wartość klucza podstawowego nie trzeba określać, nawet jeśli zwykle jest generowany przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="58b10-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="58b10-127">Będzie służyć do wykrywania zmian danych między migracji.</span><span class="sxs-lookup"><span data-stu-id="58b10-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="58b10-128">Uprzednio wprowadzonych danych zostanie usunięty, zmiana klucza podstawowego w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="58b10-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="58b10-129">W związku z tym ta funkcja jest najbardziej przydatny w przypadku danych statycznych, który nie ma powinna się zmienić poza migracje i nie zależy od niczego więcej w bazie danych, na przykład kody pocztowe.</span><span class="sxs-lookup"><span data-stu-id="58b10-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="58b10-130">Jeśli scenariusz zawiera następujące zalecane jest używana logika inicjowania niestandardowego opisanego w ostatniej sekcji:</span><span class="sxs-lookup"><span data-stu-id="58b10-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="58b10-131">Dane tymczasowe na potrzeby testowania</span><span class="sxs-lookup"><span data-stu-id="58b10-131">Temporary data for testing</span></span>
* <span data-ttu-id="58b10-132">Dane, które jest zależny od stanu bazy danych</span><span class="sxs-lookup"><span data-stu-id="58b10-132">Data that depends on database state</span></span>
* <span data-ttu-id="58b10-133">Dane, które wymaga wartości klucza, zostanie wygenerowany przez bazę danych, w tym jednostki używające klucze alternatywne jako tożsamość</span><span class="sxs-lookup"><span data-stu-id="58b10-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="58b10-134">Dane, które wymaga niestandardowej transformacji (nie jest obsługiwany przez [wartość konwersje](xref:core/modeling/value-conversions)), takie jak niektóre tworzenia skrótów haseł</span><span class="sxs-lookup"><span data-stu-id="58b10-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="58b10-135">Dane, które wymaga wywołania funkcji API zewnętrznych, takich jak tworzenie ról i użytkowników tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58b10-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="58b10-136">Dostosowywanie ręcznej migracji</span><span class="sxs-lookup"><span data-stu-id="58b10-136">Manual migration customization</span></span>

<span data-ttu-id="58b10-137">Po dodaniu zmiany w danych określony za pomocą migracji `HasData` są przekształcane do wywołania `InsertData()`, `UpdateData()`, i `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="58b10-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="58b10-138">Jednym ze sposobów obejścia niektóre ograniczenia `HasData` jest, aby ręcznie dodać te wywołania lub [operacje niestandardowe](xref:core/managing-schemas/migrations/operations) migracji zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="58b10-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="58b10-139">Logikę niestandardową inicjalizację</span><span class="sxs-lookup"><span data-stu-id="58b10-139">Custom initialization logic</span></span>

<span data-ttu-id="58b10-140">Prosty, zaawansowany sposób wykonania wstępne wypełnianie danych jest użycie [ `DbContext.SaveChanges()` ](xref:core/saving/index) przed głównej aplikacji logiki rozpoczyna wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="58b10-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="58b10-141">Rozmieszczania kod nie powinien być częścią wykonywania zwykła aplikacja, ponieważ może to spowodować problemy ze współbieżnością, gdy wiele wystąpień działają, a także wymagałoby aplikacji mających uprawnienia do modyfikowania schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58b10-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="58b10-142">W zależności od ograniczeń wdrożenia kod inicjujący mogą być wykonywane na różne sposoby:</span><span class="sxs-lookup"><span data-stu-id="58b10-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="58b10-143">Uruchamianie aplikacji inicjowania lokalnie</span><span class="sxs-lookup"><span data-stu-id="58b10-143">Running the initialization app locally</span></span>
* <span data-ttu-id="58b10-144">Wdrażanie aplikacji inicjowanie przy użyciu głównej aplikacji wywoływanie procedury inicjowania i wyłączenie lub usunięcie aplikacji inicjowania.</span><span class="sxs-lookup"><span data-stu-id="58b10-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="58b10-145">Zazwyczaj można to zautomatyzować za pomocą [profilów publikowania](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="58b10-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>

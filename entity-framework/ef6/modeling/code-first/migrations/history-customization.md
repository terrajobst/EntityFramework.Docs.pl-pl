---
title: Dostosowywanie tabeli historii migracje - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: 6644bf2b0ac703a9f3a779b17b31d79d40cc5b69
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489215"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="5f00c-102">Dostosowywanie tabeli historii migracji</span><span class="sxs-lookup"><span data-stu-id="5f00c-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="5f00c-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5f00c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="5f00c-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="5f00c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="5f00c-105">W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="5f00c-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="5f00c-106">Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="5f00c-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="5f00c-107">Co to jest tabela historii migracji?</span><span class="sxs-lookup"><span data-stu-id="5f00c-107">What is Migrations History Table?</span></span>

<span data-ttu-id="5f00c-108">Migracji historii jest tabela używany przez migracje Code First do przechowywania informacji o migracji do niej zastosować.</span><span class="sxs-lookup"><span data-stu-id="5f00c-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="5f00c-109">Domyślnie nazwa tabeli w bazie danych jest \_ \_MigrationHistory i jest tworzony podczas stosowania pierwszy migracji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5f00c-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="5f00c-110">W Entity Framework 5 Ta tabela została tabeli systemowej, jeśli aplikacja używa bazy danych programu Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="5f00c-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="5f00c-111">Jednakże, to została zmieniona w Entity Framework 6, a tabela historii migracji nie jest już oznaczone tabeli systemowej.</span><span class="sxs-lookup"><span data-stu-id="5f00c-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="5f00c-112">Dlaczego warto dostosować tabeli historii migracji?</span><span class="sxs-lookup"><span data-stu-id="5f00c-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="5f00c-113">Migracji tabeli historii mają być używane wyłącznie przez migracje Code First i zmieniając go ręcznie, może przerwać migracji.</span><span class="sxs-lookup"><span data-stu-id="5f00c-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="5f00c-114">Jednak czasem domyślnej konfiguracji nie nadaje się i tabeli musi być dostosowane, na przykład:</span><span class="sxs-lookup"><span data-stu-id="5f00c-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="5f00c-115">Należy zmienić nazwy i/lub aspekty kolumn, aby umożliwić 3<sup>usług pulpitu zdalnego</sup> migracje dostawcy</span><span class="sxs-lookup"><span data-stu-id="5f00c-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="5f00c-116">Aby zmienić nazwę tabeli</span><span class="sxs-lookup"><span data-stu-id="5f00c-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="5f00c-117">Należy użyć innego niż domyślny schemat \_ \_MigrationHistory tabeli</span><span class="sxs-lookup"><span data-stu-id="5f00c-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="5f00c-118">Potrzeba przechowywania dodatkowych danych dla danej wersji kontekstu i w związku z tym należy dodać dodatkową kolumnę do tabeli</span><span class="sxs-lookup"><span data-stu-id="5f00c-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="5f00c-119">Wyrazy środki ostrożności</span><span class="sxs-lookup"><span data-stu-id="5f00c-119">Words of precaution</span></span>

<span data-ttu-id="5f00c-120">Zmiana tabeli migracji historii jest skuteczna, ale należy uważać, aby nie nadużywać.</span><span class="sxs-lookup"><span data-stu-id="5f00c-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="5f00c-121">Środowiska uruchomieniowego EF aktualnie nie sprawdza, czy tabela historii dostosowane migracji jest zgodne ze środowiskiem uruchomieniowym.</span><span class="sxs-lookup"><span data-stu-id="5f00c-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="5f00c-122">Jeśli nie jest aplikacja może przerwać w czasie wykonywania lub nieprzewidywalne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="5f00c-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="5f00c-123">Jest to szczególnie ważne, jeśli używasz wielu kontekstach na bazę danych w przypadku wielu kontekstach można użyć tej samej tabeli historii migracji do przechowywania informacji o migracji.</span><span class="sxs-lookup"><span data-stu-id="5f00c-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="5f00c-124">Jak dostosowywać tabelę historii migracji?</span><span class="sxs-lookup"><span data-stu-id="5f00c-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="5f00c-125">Przed rozpoczęciem musisz wiedzieć, można dostosować w tabeli historii migracji tylko w przypadku, przed zainstalowaniem pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="5f00c-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="5f00c-126">Teraz w kodzie.</span><span class="sxs-lookup"><span data-stu-id="5f00c-126">Now, to the code.</span></span>

<span data-ttu-id="5f00c-127">Najpierw należy utworzyć klasę pochodną klasy System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="5f00c-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="5f00c-128">Klasa HistoryContext jest pochodną klasy DbContext, więc konfigurowania migracji tabeli historii jest bardzo podobny do konfigurowania EF modeli za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="5f00c-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="5f00c-129">Wystarczy zastąpić metodę OnModelCreating i skonfigurować tabelę za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="5f00c-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="5f00c-130">Zwykle po skonfigurowaniu EF modele nie trzeba wywołać podstawowej. OnModelCreating() z przeciążonej OnModelCreating od DbContext.OnModelCreating() ma pustą treść.</span><span class="sxs-lookup"><span data-stu-id="5f00c-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="5f00c-131">Nie jest tak, podczas konfigurowania migracji tabeli historii.</span><span class="sxs-lookup"><span data-stu-id="5f00c-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="5f00c-132">W tym przypadku pierwsze należy w przesłonięcia OnModelCreating() jest faktycznie wywołać podstawowej. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="5f00c-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="5f00c-133">Skonfiguruje migracji tabeli historii w sposób domyślny, który następnie dostosować w metodzie nadrzędnych.</span><span class="sxs-lookup"><span data-stu-id="5f00c-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="5f00c-134">Załóżmy, że chcesz zmienić nazwę tabeli historii migracje i umieść je ze schematem niestandardowej o nazwie "admin".</span><span class="sxs-lookup"><span data-stu-id="5f00c-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="5f00c-135">Oprócz administratora bazy danych chcesz, możesz zmienić nazwy kolumny MigrationId migracji\_identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="5f00c-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="5f00c-136">Można to osiągnąć, tworząc następujące klasy pochodne HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="5f00c-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="5f00c-137">Gdy Twoje niestandardowe HistoryContext jest gotowy należy wprowadzić EF informację, rejestrując go za pomocą [konfiguracja na podstawie kodu](http://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="5f00c-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](http://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="5f00c-138">To wszystko właściwie.</span><span class="sxs-lookup"><span data-stu-id="5f00c-138">That’s pretty much it.</span></span> <span data-ttu-id="5f00c-139">Teraz możesz przejść do konsoli Menedżera pakietów Enable-Migrations Dodaj migracji, a na koniec Aktualizuj bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5f00c-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="5f00c-140">Powinno to spowodować dodanie do tabeli historii migracje skonfigurowana zgodnie ze szczegółami, które określiłeś w klasie pochodnej HistoryContext bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5f00c-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Baza danych](~/ef6/media/database.png)

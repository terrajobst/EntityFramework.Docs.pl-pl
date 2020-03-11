---
title: Dostosowywanie tabeli historii migracji — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418980"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="64372-102">Dostosowywanie tabeli historii migracji</span><span class="sxs-lookup"><span data-stu-id="64372-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="64372-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="64372-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="64372-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="64372-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="64372-105">W tym artykule założono, że wiesz, jak używać Migracje Code First w podstawowych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="64372-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="64372-106">Jeśli tego nie zrobisz, musisz przeczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="64372-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="64372-107">Co to jest tabela historii migracji?</span><span class="sxs-lookup"><span data-stu-id="64372-107">What is Migrations History Table?</span></span>

<span data-ttu-id="64372-108">Tabela historii migracji jest tabelą używaną przez Migracje Code First do przechowywania szczegółowych informacji dotyczących migracji zastosowanych do bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="64372-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="64372-109">Domyślnie nazwa tabeli w bazie danych jest \_\_MigrationHistory i jest tworzona podczas stosowania pierwszej migracji do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="64372-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="64372-110">W Entity Framework 5 Ta tabela była tabelą systemową, jeśli aplikacja używa bazy danych programu Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64372-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="64372-111">Ta zmiana została zmieniona w Entity Framework 6, ale tabela historii migracji nie jest już oznaczona jako tabela systemowa.</span><span class="sxs-lookup"><span data-stu-id="64372-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="64372-112">Dlaczego warto dostosować tabelę historii migracji?</span><span class="sxs-lookup"><span data-stu-id="64372-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="64372-113">Tabela historii migracji powinna być używana wyłącznie przez Migracje Code First i zmiana ta będzie mogła spowodować przerwanie migracji.</span><span class="sxs-lookup"><span data-stu-id="64372-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="64372-114">Czasami jednak konfiguracja domyślna nie jest odpowiednia, a tabela musi być dostosowywana, na przykład:</span><span class="sxs-lookup"><span data-stu-id="64372-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="64372-115">Konieczna jest zmiana nazw i/lub aspektów kolumn w celu włączenia 3 dostawcy migracji na stronie<sup>usług pulpitu zdalnego</sup></span><span class="sxs-lookup"><span data-stu-id="64372-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="64372-116">Chcesz zmienić nazwę tabeli</span><span class="sxs-lookup"><span data-stu-id="64372-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="64372-117">Musisz użyć schematu innego niż domyślny dla \_tabeli \_MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="64372-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="64372-118">Należy przechowywać dodatkowe dane dla danej wersji kontekstu, w związku z czym należy dodać do tabeli dodatkową kolumnę</span><span class="sxs-lookup"><span data-stu-id="64372-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="64372-119">Słowa zapobiegawcze</span><span class="sxs-lookup"><span data-stu-id="64372-119">Words of precaution</span></span>

<span data-ttu-id="64372-120">Zmiana tabeli historii migracji jest zaawansowana, ale należy zachować ostrożność, aby jej nie nadużywać.</span><span class="sxs-lookup"><span data-stu-id="64372-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="64372-121">Środowisko uruchomieniowe EF obecnie nie sprawdza, czy dostosowana tabela historii migracji jest zgodna ze środowiskiem uruchomieniowym.</span><span class="sxs-lookup"><span data-stu-id="64372-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="64372-122">Jeśli nie jest to aplikacja może zostać przerwana w czasie wykonywania lub zachowywać się w sposób nieprzewidywalny.</span><span class="sxs-lookup"><span data-stu-id="64372-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="64372-123">Jest to jeszcze ważniejsze, jeśli używasz wielu kontekstów dla bazy danych, w tym przypadku wiele kontekstów może używać tej samej tabeli historii migracji do przechowywania informacji na temat migracji.</span><span class="sxs-lookup"><span data-stu-id="64372-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="64372-124">Jak dostosować tabelę historii migracji?</span><span class="sxs-lookup"><span data-stu-id="64372-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="64372-125">Przed rozpoczęciem należy się dowiedzieć, że można dostosować tabelę historii migracji tylko przed zastosowaniem pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="64372-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="64372-126">Teraz do kodu.</span><span class="sxs-lookup"><span data-stu-id="64372-126">Now, to the code.</span></span>

<span data-ttu-id="64372-127">Najpierw należy utworzyć klasę pochodzącą od klasy System. Data. Entity. migrations. history. HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="64372-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="64372-128">Klasa HistoryContext jest pochodną klasy DbContext, dlatego skonfigurowanie tabeli historii migracji jest bardzo podobne do konfigurowania modeli EF przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="64372-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="64372-129">Wystarczy przesłonić metodę OnModelCreating i użyć interfejsu API Fluent, aby skonfigurować tabelę.</span><span class="sxs-lookup"><span data-stu-id="64372-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="64372-130">Zwykle podczas konfigurowania modeli EF nie trzeba wywoływać bazy. OnModelCreating () z zastąpionej metody OnModelCreating, ponieważ DbContext. OnModelCreating () ma pustą treść.</span><span class="sxs-lookup"><span data-stu-id="64372-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="64372-131">Nie jest to przypadek podczas konfigurowania tabeli historii migracji.</span><span class="sxs-lookup"><span data-stu-id="64372-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="64372-132">W tym przypadku najpierw należy wykonać zastępowanie OnModelCreating (). OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="64372-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="64372-133">Spowoduje to skonfigurowanie tabeli historii migracji w sposób domyślny, który następnie jest dostrajany w metodzie przesłaniania.</span><span class="sxs-lookup"><span data-stu-id="64372-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="64372-134">Załóżmy, że chcesz zmienić nazwę tabeli historii migracji i umieścić ją w schemacie niestandardowym o nazwie "admin".</span><span class="sxs-lookup"><span data-stu-id="64372-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="64372-135">Ponadto administrator DBA o zmianę nazwy kolumny MigrationId na\_migracji.</span><span class="sxs-lookup"><span data-stu-id="64372-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="64372-136"> Można to osiągnąć przez utworzenie następującej klasy pochodnej z HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="64372-136"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="64372-137">Gdy niestandardowy HistoryContext jest gotowy, musisz go zakodować przez zarejestrowanie go za pośrednictwem [konfiguracji opartej na kodzie](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="64372-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="64372-138">Jest to dość duże.</span><span class="sxs-lookup"><span data-stu-id="64372-138">That’s pretty much it.</span></span> <span data-ttu-id="64372-139">Teraz możesz przejść do konsoli Menedżera pakietów, włączyć-migracji, dodać migrację i wreszcie Update-Database.</span><span class="sxs-lookup"><span data-stu-id="64372-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="64372-140">Powinno to spowodować dodanie do bazy danych tabeli historii migracji, skonfigurowanej zgodnie ze szczegółami określonymi w klasie pochodnej HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="64372-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Baza danych](~/ef6/media/database.png)

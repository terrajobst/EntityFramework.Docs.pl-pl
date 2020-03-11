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
# <a name="customizing-the-migrations-history-table"></a>Dostosowywanie tabeli historii migracji
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

> [!NOTE]
> W tym artykule założono, że wiesz, jak używać Migracje Code First w podstawowych scenariuszach. Jeśli tego nie zrobisz, musisz przeczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="what-is-migrations-history-table"></a>Co to jest tabela historii migracji?

Tabela historii migracji jest tabelą używaną przez Migracje Code First do przechowywania szczegółowych informacji dotyczących migracji zastosowanych do bazy danych programu. Domyślnie nazwa tabeli w bazie danych jest \_\_MigrationHistory i jest tworzona podczas stosowania pierwszej migracji do bazy danych. W Entity Framework 5 Ta tabela była tabelą systemową, jeśli aplikacja używa bazy danych programu Microsoft SQL Server. Ta zmiana została zmieniona w Entity Framework 6, ale tabela historii migracji nie jest już oznaczona jako tabela systemowa.

## <a name="why-customize-migrations-history-table"></a>Dlaczego warto dostosować tabelę historii migracji?

Tabela historii migracji powinna być używana wyłącznie przez Migracje Code First i zmiana ta będzie mogła spowodować przerwanie migracji. Czasami jednak konfiguracja domyślna nie jest odpowiednia, a tabela musi być dostosowywana, na przykład:

-   Konieczna jest zmiana nazw i/lub aspektów kolumn w celu włączenia 3 dostawcy migracji na stronie<sup>usług pulpitu zdalnego</sup>
-   Chcesz zmienić nazwę tabeli
-   Musisz użyć schematu innego niż domyślny dla \_tabeli \_MigrationHistory
-   Należy przechowywać dodatkowe dane dla danej wersji kontekstu, w związku z czym należy dodać do tabeli dodatkową kolumnę

## <a name="words-of-precaution"></a>Słowa zapobiegawcze

Zmiana tabeli historii migracji jest zaawansowana, ale należy zachować ostrożność, aby jej nie nadużywać. Środowisko uruchomieniowe EF obecnie nie sprawdza, czy dostosowana tabela historii migracji jest zgodna ze środowiskiem uruchomieniowym. Jeśli nie jest to aplikacja może zostać przerwana w czasie wykonywania lub zachowywać się w sposób nieprzewidywalny. Jest to jeszcze ważniejsze, jeśli używasz wielu kontekstów dla bazy danych, w tym przypadku wiele kontekstów może używać tej samej tabeli historii migracji do przechowywania informacji na temat migracji.

## <a name="how-to-customize-migrations-history-table"></a>Jak dostosować tabelę historii migracji?

Przed rozpoczęciem należy się dowiedzieć, że można dostosować tabelę historii migracji tylko przed zastosowaniem pierwszej migracji. Teraz do kodu.

Najpierw należy utworzyć klasę pochodzącą od klasy System. Data. Entity. migrations. history. HistoryContext. Klasa HistoryContext jest pochodną klasy DbContext, dlatego skonfigurowanie tabeli historii migracji jest bardzo podobne do konfigurowania modeli EF przy użyciu interfejsu API Fluent. Wystarczy przesłonić metodę OnModelCreating i użyć interfejsu API Fluent, aby skonfigurować tabelę.

>[!NOTE]
> Zwykle podczas konfigurowania modeli EF nie trzeba wywoływać bazy. OnModelCreating () z zastąpionej metody OnModelCreating, ponieważ DbContext. OnModelCreating () ma pustą treść. Nie jest to przypadek podczas konfigurowania tabeli historii migracji. W tym przypadku najpierw należy wykonać zastępowanie OnModelCreating (). OnModelCreating(). Spowoduje to skonfigurowanie tabeli historii migracji w sposób domyślny, który następnie jest dostrajany w metodzie przesłaniania.

Załóżmy, że chcesz zmienić nazwę tabeli historii migracji i umieścić ją w schemacie niestandardowym o nazwie "admin". Ponadto administrator DBA o zmianę nazwy kolumny MigrationId na\_migracji.  Można to osiągnąć przez utworzenie następującej klasy pochodnej z HistoryContext:

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

Gdy niestandardowy HistoryContext jest gotowy, musisz go zakodować przez zarejestrowanie go za pośrednictwem [konfiguracji opartej na kodzie](https://msdn.com/data/jj680699):

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

Jest to dość duże. Teraz możesz przejść do konsoli Menedżera pakietów, włączyć-migracji, dodać migrację i wreszcie Update-Database. Powinno to spowodować dodanie do bazy danych tabeli historii migracji, skonfigurowanej zgodnie ze szczegółami określonymi w klasie pochodnej HistoryContext.

![Baza danych](~/ef6/media/database.png)

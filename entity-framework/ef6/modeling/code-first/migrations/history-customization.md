---
title: Dostosowywanie tabeli historii migracje - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
caps.latest.revision: 3
ms.openlocfilehash: 3805d99be6d37d037096f5c5fb69fd30197c1e91
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914038"
---
# <a name="customizing-the-migrations-history-table"></a>Dostosowywanie tabeli historii migracji
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

> [!NOTE]
> W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy. Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="what-is-migrations-history-table"></a>Co to jest tabela historii migracji?

Migracji historii jest tabela używany przez migracje Code First do przechowywania informacji o migracji do niej zastosować. Domyślnie nazwa tabeli w bazie danych jest \_ \_MigrationHistory i jest tworzony podczas stosowania pierwszy migracji bazy danych. W Entity Framework 5 Ta tabela została tabeli systemowej, jeśli aplikacja używa bazy danych programu Microsoft Sql Server. Jednakże, to została zmieniona w Entity Framework 6, a tabela historii migracji nie jest już oznaczone tabeli systemowej.

## <a name="why-customize-migrations-history-table"></a>Dlaczego warto dostosować tabeli historii migracji?

Migracji tabeli historii mają być używane wyłącznie przez migracje Code First i zmieniając go ręcznie, może przerwać migracji. Jednak czasem domyślnej konfiguracji nie nadaje się i tabeli musi być dostosowane, na przykład:

-   Należy zmienić nazwy i/lub aspekty kolumn, aby umożliwić 3<sup>usług pulpitu zdalnego</sup> migracje dostawcy
-   Aby zmienić nazwę tabeli
-   Należy użyć innego niż domyślny schemat \_ \_MigrationHistory tabeli
-   Potrzeba przechowywania dodatkowych danych dla danej wersji kontekstu i w związku z tym należy dodać dodatkową kolumnę do tabeli

## <a name="words-of-precaution"></a>Wyrazy środki ostrożności

Zmiana tabeli migracji historii jest skuteczna, ale należy uważać, aby nie nadużywać. Środowiska uruchomieniowego EF aktualnie nie sprawdza, czy tabela historii dostosowane migracji jest zgodne ze środowiskiem uruchomieniowym. Jeśli nie jest aplikacja może przerwać w czasie wykonywania lub nieprzewidywalne zachowanie. Jest to szczególnie ważne, jeśli używasz wielu kontekstach na bazę danych w przypadku wielu kontekstach można użyć tej samej tabeli historii migracji do przechowywania informacji o migracji.

## <a name="how-to-customize-migrations-history-table"></a>Jak dostosowywać tabelę historii migracji?

Przed rozpoczęciem musisz wiedzieć, można dostosować w tabeli historii migracji tylko w przypadku, przed zainstalowaniem pierwszej migracji. Teraz w kodzie.

Najpierw należy utworzyć klasę pochodną klasy System.Data.Entity.Migrations.History.HistoryContext. Klasa HistoryContext jest pochodną klasy DbContext, więc konfigurowania migracji tabeli historii jest bardzo podobny do konfigurowania EF modeli za pomocą interfejsu API fluent. Wystarczy zastąpić metodę OnModelCreating i skonfigurować tabelę za pomocą interfejsu API fluent.

>[!NOTE]
> Zwykle po skonfigurowaniu EF modele nie trzeba wywołać podstawowej. OnModelCreating() z przeciążonej OnModelCreating od DbContext.OnModelCreating() ma pustą treść. Nie jest tak, podczas konfigurowania migracji tabeli historii. W tym przypadku pierwsze należy w przesłonięcia OnModelCreating() jest faktycznie wywołać podstawowej. OnModelCreating(). Skonfiguruje migracji tabeli historii w sposób domyślny, który następnie dostosować w metodzie nadrzędnych.

Załóżmy, że chcesz zmienić nazwę tabeli historii migracje i umieść je ze schematem niestandardowej o nazwie "admin". Oprócz administratora bazy danych chcesz, możesz zmienić nazwy kolumny MigrationId migracji\_identyfikatora.  Można to osiągnąć, tworząc następujące klasy pochodne HistoryContext:

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

Gdy Twoje niestandardowe HistoryContext jest gotowy należy wprowadzić EF informację, rejestrując go za pomocą [konfiguracja na podstawie kodu](http://msdn.com/data/jj680699):

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

To wszystko właściwie. Teraz możesz przejść do konsoli Menedżera pakietów Enable-Migrations Dodaj migracji, a na koniec Aktualizuj bazy danych. Powinno to spowodować dodanie do tabeli historii migracje skonfigurowana zgodnie ze szczegółami, które określiłeś w klasie pochodnej HistoryContext bazy danych.

![Baza danych](~/ef6/media/database.png)

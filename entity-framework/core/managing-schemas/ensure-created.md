---
title: Tworzenie i upuszczanie interfejsów API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688632"
---
# <a name="create-and-drop-apis"></a>Tworzenie i upuszczanie interfejsów API

Metody EnsureCreated i EnsureDeleted udostępniają uproszczone zamiast [migracje](migrations/index.md) zarządzania schemat bazy danych. Te metody są przydatne w scenariuszach danych jest przejściowy i można było porzucić po zmianie schematu. Na przykład podczas tworzenia prototypów w testach lub pamięciach podręcznych.

Niektórzy dostawcy (zwłaszcza tymi nierelacyjnych) nie obsługuje migracji. W przypadku tych dostawców EnsureCreated często jest najprostszym sposobem zainicjować schemat bazy danych.

> [!WARNING]
> EnsureCreated i migracje nie działają dobrze ze sobą. Jeśli używasz migracji, nie należy używać EnsureCreated zainicjować schematu.

Przechodzenie ze EnsureCreated do migracji nie jest bezproblemowe. Najprostszym sposobem, aby to zrobił jest porzucenia bazy danych i ponownego utworzenia go za pomocą migracji. Jeśli przewidujesz używanie migracji w przyszłości, najlepiej zamiast EnsureCreated, po prostu zacznij z migracji.

## <a name="ensuredeleted"></a>EnsureDeleted

Metoda EnsureDeleted spowoduje porzucenie bazy danych, jeśli taki istnieje. Jeśli nie masz odpowiednich uprawnień, jest zgłaszany wyjątek.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated spowoduje utworzenie bazy danych, jeśli nie istnieje i zainicjować schemat bazy danych. Jeśli istnieje żadnych tabel (w tym tabel dla innej klasy DbContext) schematu nie można zainicjować.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Ponadto dostępnych asynchronicznych wersji tych metod.

## <a name="sql-script"></a>Skrypt SQL

Aby uzyskać SQL używane przez EnsureCreated, można użyć metody GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Wiele klas typu DbContext

EnsureCreated działa tylko w przypadku, gdy żadna tabela znajdują się w bazie danych. Jeśli to konieczne, można napisać własne sprawdzenie, czy schemat musi zostać zainicjowany i usługa bazowego IRelationalDatabaseCreator zainicjować schematu.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```

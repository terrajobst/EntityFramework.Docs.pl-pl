---
title: Tworzenie i upuszczanie interfejsów API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285642"
---
# <a name="create-and-drop-apis"></a>Tworzenie i upuszczanie interfejsów API

Metody EnsureCreated i EnsureDeleted udostępniają uproszczone zamiast [migracje](migrations/index.md) zarządzania schemat bazy danych. Może to być przydatne w sytuacjach, gdy dane są przejściowy i można było porzucić po zmianie schematu. Na przykład podczas tworzenia prototypów w testach lub pamięciach podręcznych.

Niektórzy dostawcy (zwłaszcza tymi nierelacyjnych) nie obsługuje migracji. W tym przypadku EnsureCreated często jest najprostszym sposobem zainicjować schemat bazy danych.

> [!WARNING]
> EnsureCreated i migracje nie działają dobrze ze sobą. Jeśli używasz migracji, nie należy używać EnsureCreated zainicjować schematu.

Przechodzenie ze EnsureCreated do migracji nie jest bezproblemowe. Jest simpelest sposób osiągnięcia tego celu porzucenia bazy danych i ponownego utworzenia go za pomocą migracji. Jeśli przewidujesz używanie migracji w przyszłości, najlepiej zamiast EnsureCreated, po prostu zacznij z migracji.

## <a name="ensuredeleted"></a>EnsureDeleted

Metoda EnsureDeleted spowoduje porzucenie bazy danych, jeśli taki istnieje. Wyjątek jest generowany, jeśli nie masz odpowiednie uprawnienia.

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

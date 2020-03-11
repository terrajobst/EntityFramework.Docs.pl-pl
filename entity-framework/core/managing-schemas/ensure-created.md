---
title: Tworzenie i usuwanie interfejsów API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416871"
---
# <a name="create-and-drop-apis"></a>Tworzenie i upuszczanie interfejsów API

Metody EnsureCreated i EnsureDeleted zapewniają uproszczoną alternatywę dla [migracji](migrations/index.md) w celu zarządzania schematem bazy danych. Te metody są przydatne w scenariuszach, gdy dane są przejściowe i mogą być porzucane, gdy zmienia się schemat. Na przykład podczas tworzenia prototypów, w testach lub w lokalnych pamięciach podręcznych.

Niektórzy dostawcy (szczególnie nierelacyjne) nie obsługują migracji. W przypadku tych dostawców EnsureCreated jest często najprostszym sposobem na zainicjowanie schematu bazy danych.

> [!WARNING]
> EnsureCreated i migracje nie współpracują ze sobą. W przypadku korzystania z migracji nie należy używać EnsureCreated, aby zainicjować schemat.

Przejście z EnsureCreated do migracji nie jest bezproblemowe. Najprostszym sposobem, aby to zrobić, jest usunięcie bazy danych i jej ponowne utworzenie przy użyciu migracji. Jeśli zamierzasz korzystać z migracji w przyszłości, najlepiej zacząć od migracji zamiast korzystać z EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Metoda EnsureDeleted usunie bazę danych, jeśli istnieje. Jeśli nie masz odpowiednich uprawnień, zostanie zgłoszony wyjątek.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated utworzy bazę danych, jeśli nie istnieje, i zainicjuje schemat bazy danych. Jeśli istnieją tabele (w tym tabele dla innej klasy DbContext), schemat nie zostanie zainicjowany.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Wersje asynchroniczne tych metod są również dostępne.

## <a name="sql-script"></a>Skrypt SQL

Aby uzyskać kod SQL używany przez EnsureCreated, można użyć metody GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Wiele klas DbContext

EnsureCreated działa tylko wtedy, gdy żadna tabela nie znajduje się w bazie danych. W razie potrzeby można napisać własne sprawdzenie, aby sprawdzić, czy schemat musi być zainicjowany, i użyć bazowej usługi IRelationalDatabaseCreator do zainicjowania schematu.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```

---
title: Dostawca bazy danych Microsoft SQL Server — tabele zoptymalizowane pod kątem pamięci — EF Core
description: Jak używać tabel zoptymalizowanych pod kątem pamięci z dostawcą bazy danych Entity Framework Core SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824647"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Obsługa tabel zoptymalizowanych pod kątem pamięci w SQL Server EF Core dostawcy bazy danych

[Tabele zoptymalizowane pod kątem pamięci](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) są funkcją SQL Server, w której cała tabela znajduje się w pamięci. Druga kopia danych tabeli jest utrzymywana na dysku, ale tylko w celach trwałości. Dane w tabelach zoptymalizowanych pod kątem pamięci są odczytywane tylko z dysku podczas odzyskiwania bazy danych. Na przykład po ponownym uruchomieniu serwera.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurowanie tabeli zoptymalizowanej pod kątem pamięci

Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci. W przypadku korzystania z EF Core do tworzenia i konserwowania bazy danych opartej na modelu (z [migracją](xref:core/managing-schemas/migrations/index) lub [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]

---
title: "Pisanie dostawca bazy danych — podstawowe EF"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a>Pisanie dostawcy bazy danych

Informacje na temat pisania dostawcy bazy danych programu Entity Framework Core, zobacz [takim razie chcesz zapisać dostawcę EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) przez [Arthur Vickers](https://github.com/ajcvickers).

Kod EF Core podstawowy jest typu open source i zawiera kilka dostawcy bazy danych, które mogą służyć jako odwołanie. Kod źródłowy można znaleźć w https://github.com/aspnet/EntityFramework.

## <a name="the-providers-beware-label"></a>Etykieta Uważaj dostawców

Gdy rozpoczniesz pracę nad dostawcę obserwować [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) etykiety na naszych GitHub problemy i żądania ściągnięcia. Etykieta możemy służy do identyfikowania zmiany, które mogą mieć wpływ na dostawcy modułów zapisujących.

## <a name="suggested-naming-of-third-party-providers"></a>Sugerowane nazwy dostawców innych firm

Zaleca się przy użyciu następujących nazewnictwa dla pakietów NuGet. Jest to zgodne z nazwami paczek przez zespół EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Na przykład:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`

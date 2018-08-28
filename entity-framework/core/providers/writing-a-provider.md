---
title: Pisanie dostawcy bazy danych — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993669"
---
# <a name="writing-a-database-provider"></a>Pisanie dostawcy bazy danych

Informacje na temat pisania dostawcy bazy danych programu Entity Framework Core, zobacz [więc chcesz napisać dostawcy programu EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) przez [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Te wpisy nie zostały zaktualizowane od czasu programu EF Core 1.1 i od tego czasu miały miejsce znaczące zmiany [681 problem](https://github.com/aspnet/EntityFramework.Docs/issues/681) śledzenie aktualizacji tej dokumentacji.

Codebase programu EF Core jest "open source" i zawiera kilka dostawcy baz danych, które mogą służyć jako odwołanie. Można znaleźć kodu źródłowego w https://github.com/aspnet/EntityFrameworkCore. Może to być również przydatne Spójrz na kod dla często używanych dostawców innych firm, takich jak [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), i [programu SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). W szczególności te projekty są Instalatora, aby rozszerzyć i uruchom testy funkcjonalne, które możemy opublikować dla narzędzia NuGet. Zdecydowanie zaleca się tego rodzaju instalacji.

## <a name="keeping-up-to-date-with-provider-changes"></a>Aktualizowanie zmian w dostawcy

Po wydaniu wersji 2.1, zaczynając od pracy, firma Microsoft opracowała [dziennika zmian](provider-log.md) , może być konieczne odpowiednie zmiany w kodzie dostawcy. Ta wartość jest przeznaczona do pomocy podczas aktualizowania istniejącego dostawcy do pracy z nową wersję programu EF Core.

Przed 2.1, użyliśmy [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiety na naszych problemy usługi GitHub i żądań ściągnięcia dla podobnych celów. Firma Microsoft będzie continiue na potrzeby tych lables na problemy stanowić wskazówkę, które elementy robocze w danej wersji może również wymagać pracy w dostawcy. A `providers-beware` etykiety zazwyczaj oznacza, że implementacja elementu roboczego może spowodować przerwanie dostawców, podczas gdy `providers-fyi` etykiety zazwyczaj oznacza, że dostawcy nie będą działać, ale kod może być konieczne, należy zmienić mimo to, na przykład, aby włączyć nowe funkcje.

## <a name="suggested-naming-of-third-party-providers"></a>Sugerowane nazewnictwa dostawców innych firm

Zaleca się przy użyciu następujących nazewnictwa dla pakietów NuGet. Jest to zgodne z nazwami pakiety dostarczone przez zespół programu EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Na przykład:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`

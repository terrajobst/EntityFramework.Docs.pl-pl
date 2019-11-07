---
title: Pisanie dostawcy bazy danych — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d52a8581772cc5405e94966fa7ebdff4128c252
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654776"
---
# <a name="writing-a-database-provider"></a>Tworzenie dostawcy bazy danych

Aby uzyskać informacje na temat pisania dostawcy bazy danych Entity Framework Core, zobacz, [czy chcesz napisać dostawcę EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) za pomocą [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Te wpisy nie zostały zaktualizowane od czasu EF Core 1,1 i wprowadzono znaczące zmiany od momentu, gdy [problem 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) śledzi aktualizacje tej dokumentacji.

Baza kodu EF Core jest typu open source i zawiera kilku dostawców baz danych, których można użyć jako odwołania. Kod źródłowy można znaleźć w <https://github.com/aspnet/EntityFrameworkCore>. Pomocne może być również przeszukanie kodu dla często używanych dostawców innych firm, takich jak [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)i [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). W szczególności te projekty są skonfigurowane do rozszerania i uruchamiania testów funkcjonalnych publikowanych na platformie NuGet. Ten rodzaj instalacji jest zdecydowanie zalecany.

## <a name="keeping-up-to-date-with-provider-changes"></a>Utrzymywanie Aktualności przy użyciu zmian dostawcy

Począwszy od wersji 2,1, został utworzony [Dziennik zmian](provider-log.md) , które mogą wymagać odpowiednich zmian w kodzie dostawcy. Jest to przydatne w przypadku aktualizowania istniejącego dostawcy do pracy z nową wersją EF Core.

Przed 2,1mi użyto [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiet w naszych problemach z usługą GitHub i żądaniami ściągnięcia w podobny sposób. Continiue się na korzystanie z tych poszukiwaniu w przypadku problemów, aby wskazać, które elementy robocze w danej wersji mogą również wymagać pracy w ramach dostawców. Etykieta `providers-beware` zwykle oznacza, że implementacja elementu pracy może przerwać dostawców, natomiast etykieta `providers-fyi` zazwyczaj oznacza, że dostawcy nie będą przerywane, ale może być konieczne zmodyfikowanie kodu, na przykład w celu włączenia nowych funkcji.

## <a name="suggested-naming-of-third-party-providers"></a>Sugerowane nazewnictwo dostawców innych firm

Zalecamy używanie następujących nazw dla pakietów NuGet. Jest to zgodne z nazwami pakietów dostarczonych przez zespół EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Na przykład:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`

---
title: Dostawca bazy danych programu SQLite — ograniczenia — EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417775"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Ograniczenia dostawcy bazy danych EF Core SQLite

Dostawca oprogramowania SQLite ma wiele ograniczeń migracji. Większość z tych ograniczeń jest wynikiem ograniczeń w źródłowym aparacie bazy danych programu SQLite i nie są specyficzne dla EF.

## <a name="modeling-limitations"></a>Ograniczenia modelowania

Wspólna Biblioteka relacyjna (współdzielona przez Entity Framework dostawcy relacyjnej bazy danych) definiuje interfejsy API dla koncepcji modelowania wspólnych dla większości aparatów relacyjnej bazy danych. Niektóre z tych pojęć nie są obsługiwane przez dostawcę oprogramowania SQLite.

* Schematy
* Sekwencje
* Kolumny obliczane

## <a name="query-limitations"></a>Ograniczenia zapytania

SQLite nie obsługuje natywnie następujących typów danych. EF Core mogą odczytywać i zapisywać wartości tych typów, a także wykonywać zapytania o równość (`where e.Property == value`). Inne operacje, takie jak porównywanie i porządkowanie, będą wymagały oceny klienta.

* DateTimeOffset
* Wartość dziesiętna
* TimeSpan
* UInt64

Zamiast `DateTimeOffset`zalecamy użycie wartości typu DateTime. W przypadku obsługi wielu stref czasowych zaleca się przekonwertowanie wartości na czas UTC przed zapisaniem, a następnie przekonwertowanie z powrotem do odpowiedniej strefy czasowej.

Typ `Decimal` zapewnia wysoki poziom precyzji. Jeśli nie potrzebujesz tego poziomu dokładności, zalecamy użycie funkcji Double. Możesz użyć [konwertera wartości](../../modeling/value-conversions.md) , aby dalej używać miejsc dziesiętnych w klasach.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Ograniczenia migracji

Aparat bazy danych programu SQLite nie obsługuje wielu operacji schematu, które są obsługiwane przez większość innych relacyjnych baz danych. W przypadku próby zastosowania jednej z nieobsługiwanych operacji do bazy danych programu SQLite zostanie wygenerowany `NotSupportedException`.

| Operacja            | Obsługiwane? | Wymaga wersji |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| Indeksowanie          | ✔          | 1.0              |
| Utwórz          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| Nazwa Rename          | ✔          | 1.0              |
| EnsureSchema         | ✔ (No-op)  | 2.0              |
| DropSchema           | ✔ (No-op)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Aktualizacja               | ✔          | 2.0              |
| Usuń               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Obejście ograniczeń dla migracji

Niektóre z tych ograniczeń można obejść, ręcznie pisząc kod w migracjach, aby wykonać ponowną kompilację tabeli. Odbudowanie tabeli obejmuje zmianę nazwy istniejącej tabeli, utworzenie nowej tabeli, skopiowanie danych do nowej tabeli i usunięcie starej tabeli. Aby wykonać niektóre z tych kroków, należy użyć metody `Sql(string)`.

Aby uzyskać więcej informacji, zobacz artykuł [Tworzenie innych rodzajów zmian schematu tabeli](https://sqlite.org/lang_altertable.html#otheralter) w dokumentacji oprogramowania SQLite.

W przyszłości, EF może obsługiwać niektóre z tych operacji przy użyciu metody odbudowywania tabeli w obszarze okładki. [Tę funkcję można śledzić w naszym projekcie usługi GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).

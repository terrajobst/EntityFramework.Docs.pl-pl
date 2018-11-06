---
title: Wstępne wypełnianie danych — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 791f7afff36aac52fe2ffdc16ab580db22011b99
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028099"
---
# <a name="data-seeding"></a>Wstępne wypełnianie danych

Wstępne wypełnianie danych polega na wypełnianie bazy danych za pomocą początkowego zestawu danych.

Istnieje kilka sposobów, które można to zrobić w programie EF Core:
* Modelowanie danych inicjatora
* Dostosowywanie ręcznej migracji
* Logikę niestandardową inicjalizację

## <a name="model-seed-data"></a>Modelowanie danych inicjatora

> [!NOTE]
> Ta funkcja jest nowa na platformie EF Core 2.1.

W odróżnieniu od w EF6 w programie EF Core wstępne wypełnianie danych może być skojarzony z typem jednostki jako część konfiguracji modelu. Następnie programu EF Core [migracje](xref:core/managing-schemas/migrations/index) można automatycznie obliczyć co Wstawianie, aktualizowanie lub usuwanie potrzebę operacji mają być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.

> [!NOTE]
> Podczas określania, jakie operacja powinna być wykonana pobierać dane inicjatora do żądanego stanu, migracje analizuje tylko zmiany modelu. Dlatego wszelkie zmiany w danych poza migracje mogą zostać utracone lub nie powodują wystąpienie błędu.

Na przykład dane zostaną skonfigurowane `Blog` w `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Aby dodać jednostki, które mają relację wartości klucza obcego muszą być określone:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Jeśli typ jednostki ma wszystkie właściwości w stanie w tle klasa anonimowa może służyć do Podaj wartości:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Należące do jednostki, który może zostać rozpoczęta typów w podobny sposób:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Zobacz [pełny przykład projektu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) Aby uzyskać dodatkowy kontekst.

Po dodaniu danych do modelu, [migracje](xref:core/managing-schemas/migrations/index) powinien być używany do zastosowania zmian.

> [!TIP]
> Jeśli konieczne jest zastosowanie migracji w ramach zautomatyzowanego wdrażania możesz [Utwórz skrypt SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) można je przeglądać przed wykonaniem.

Alternatywnie, można użyć `context.Database.EnsureCreated()` do utworzenia nowej bazy danych zawierającej dane inicjatora, na przykład w przypadku bazy danych testów lub za pomocą dostawcy w pamięci lub dowolnej-relation bazy danych. Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` spowoduje zaktualizowanie żadnego schematu ani w bazie danych inicjatora. Relacyjne bazy danych nie należy wywoływać `EnsureCreated()` Jeśli planujesz użyć migracje.

Inicjatora danych tego typu jest zarządzana przez migracje i skrypt do aktualizowania danych, który jest już w bazie danych musi zostać wygenerowane bez połączenia z bazą danych. To nakłada pewne ograniczenia:
* Wartość klucza podstawowego nie trzeba określać, nawet jeśli zwykle jest generowany przez bazę danych. Będzie służyć do wykrywania zmian danych między migracji.
* Uprzednio wprowadzonych danych zostanie usunięty, zmiana klucza podstawowego w dowolny sposób.

W związku z tym ta funkcja jest najbardziej przydatny w przypadku danych statycznych, który nie ma powinna się zmienić poza migracje i nie zależy od niczego więcej w bazie danych, na przykład kody pocztowe.

Jeśli scenariusz zawiera następujące zalecane jest używana logika inicjowania niestandardowego opisanego w ostatniej sekcji:
* Dane tymczasowe na potrzeby testowania
* Dane, które jest zależny od stanu bazy danych
* Dane, które wymaga wartości klucza, zostanie wygenerowany przez bazę danych, w tym jednostki używające klucze alternatywne jako tożsamość
* Dane, które wymaga niestandardowej transformacji (nie jest obsługiwany przez [wartość konwersje](xref:core/modeling/value-conversions)), takie jak niektóre tworzenia skrótów haseł
* Dane, które wymaga wywołania funkcji API zewnętrznych, takich jak tworzenie ról i użytkowników tożsamości platformy ASP.NET Core

## <a name="manual-migration-customization"></a>Dostosowywanie ręcznej migracji

Po dodaniu zmiany w danych określony za pomocą migracji `HasData` są przekształcane do wywołania `InsertData()`, `UpdateData()`, i `DeleteData()`. Jednym ze sposobów obejścia niektóre ograniczenia `HasData` jest, aby ręcznie dodać te wywołania lub [operacje niestandardowe](xref:core/managing-schemas/migrations/operations) migracji zamiast tego.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Logikę niestandardową inicjalizację

Prosty, zaawansowany sposób wykonania wstępne wypełnianie danych jest użycie [ `DbContext.SaveChanges()` ](xref:core/saving/index) przed głównej aplikacji logiki rozpoczyna wykonywanie.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Rozmieszczania kod nie powinien być częścią wykonywania zwykła aplikacja, ponieważ może to spowodować problemy ze współbieżnością, gdy wiele wystąpień działają, a także wymagałoby aplikacji mających uprawnienia do modyfikowania schematu bazy danych.

W zależności od ograniczeń wdrożenia kod inicjujący mogą być wykonywane na różne sposoby:
* Uruchamianie aplikacji inicjowania lokalnie
* Wdrażanie aplikacji inicjowanie przy użyciu głównej aplikacji wywoływanie procedury inicjowania i wyłączenie lub usunięcie aplikacji inicjowania.

Zazwyczaj można to zautomatyzować za pomocą [profilów publikowania](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).

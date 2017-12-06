---
title: "Maksymalna długość - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a>Maksymalna długość

Konfigurowanie maksymalnej długości udostępnia wskazówkę dla magazynu danych o typie danych odpowiednie dla danej właściwości. Maksymalna długość ma zastosowanie tylko do typów danych w tablicy, takich jak `string` i `byte[]`.

> [!NOTE]  
> Entity Framework nie wszystkich sprawdzania poprawności maksymalną długość przed przekazaniem do dostawcy. Jest magazyn dostawcy lub danych, aby sprawdzić, czy odpowiednie. Na przykład gdy przeznaczonych dla programu SQL Server, co przekracza maksymalną długość spowodują wyjątek jako typ danych kolumny źródłowej nie zezwala na nadmiarowe dane mają być przechowywane.

## <a name="conventions"></a>Konwencje

Konwencja jest pozostawiany do dostawcy bazy danych, aby wybrać odpowiedni typ danych właściwości. Dla właściwości, które mają długość dostawcy bazy danych będzie zazwyczaj wybierz typ danych, umożliwiający najdłuższym długość danych. Na przykład Microsoft SQL Server będzie używać `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` Jeśli kolumna jest używana jako klucz).

## <a name="data-annotations"></a>Adnotacji danych

Aby skonfigurować maksymalną długość dla właściwości, można użyć adnotacji danych. W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` używany typ danych.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować maksymalną długość dla właściwości, można użyć interfejsu API Fluent. W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` używany typ danych.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

---
title: Dziedziczenie (relacyjna baza danych) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655630"
---
# <a name="inheritance-relational-database"></a>Dziedziczenie (relacyjna baza danych)

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.

> [!NOTE]  
> Obecnie tylko wzorzec tabeli dla hierarchii (TPH) jest implementowany w EF Core. Inne typowe wzorce, takie jak Table-per-Type (TPT) i typu tabeli (TPC), nie są jeszcze dostępne.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją dziedziczenie zostanie zmapowane przy użyciu wzorca tabela-na-hierarchia (TPH). TPH używa pojedynczej tabeli do przechowywania danych dla wszystkich typów w hierarchii. Kolumna rozróżniacza służy do identyfikowania, który typ reprezentuje każdy wiersz.

EF Core zostanie skonfigurowana tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy zostaną jawnie dołączone do modelu (zobacz [dziedziczenie](../inheritance.md) , aby uzyskać więcej informacji).

Poniżej znajduje się przykład przedstawiający prosty scenariusz dziedziczenia oraz dane przechowywane w tabeli relacyjnej bazy danych przy użyciu wzorca TPH. Kolumna *rozróżniacza* określa, który typ *blogu* jest przechowywany w każdym wierszu.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> W przypadku korzystania z mapowania TPH kolumny bazy danych są automatycznie wprowadzane do wartości null.

## <a name="data-annotations"></a>Adnotacje danych

Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować nazwę i typ kolumny dyskryminatora oraz wartości, które są używane do identyfikowania poszczególnych typów w hierarchii.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Konfigurowanie właściwości rozróżniacza

W powyższych przykładach rozróżniacz jest tworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) w jednostce podstawowej hierarchii. Ponieważ jest to właściwość w modelu, można ją skonfigurować tak samo jak inne właściwości. Na przykład, aby ustawić maksymalną długość w przypadku użycia domyślnego rozróżniacza Konwencji przez Konwencję:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Rozróżniacz może być również mapowany do rzeczywistej właściwości CLR w jednostce. Na przykład:

```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Łącząc te dwa elementy jednocześnie, możliwe jest zamapowanie rozróżniacza na rzeczywistą Właściwość i skonfigurowanie go:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```

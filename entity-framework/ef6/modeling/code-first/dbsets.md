---
title: Definiowanie dbsets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419095"
---
# <a name="defining-dbsets"></a>Definiowanie dbsets
Podczas tworzenia przy użyciu przepływu pracy Code First definiujesz pochodny DbContext, który reprezentuje sesję z bazą danych i uwidacznia Nieogólnymi dla każdego typu w modelu. W tym temacie opisano różne sposoby definiowania właściwości Nieogólnymi.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext z właściwościami Nieogólnymi  

Typowy przypadek przedstawiony w Code First przykłady ma mieć DbContext z publicznymi właściwościami Nieogólnymi dla typów jednostek modelu. Na przykład:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Gdy jest używany w trybie Code First, spowoduje to skonfigurowanie blogów i wpisów jako typów jednostek, a także skonfigurowanie innych typów dostępnych z tych obiektów. Dodatkowo DbContext automatycznie wywoła metodę ustawiającą dla każdej z tych właściwości, aby ustawić wystąpienie odpowiedniego Nieogólnymi.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext z właściwościami IDbSet  

Istnieją sytuacje, takie jak podczas tworzenia imitacji lub sztucznych, gdzie bardziej przydatne jest zadeklarowanie właściwości zestawu przy użyciu interfejsu. W takich przypadkach można użyć interfejsu IDbSet zamiast Nieogólnymi. Na przykład:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Ten kontekst działa w taki sam sposób, jak kontekst, który używa klasy Nieogólnymi dla właściwości zestawu.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext z właściwościami zestawu tylko do odczytu  

Jeśli nie chcesz ujawniać publicznych metod ustawiających właściwości Nieogólnymi lub IDbSet, możesz zamiast tego utworzyć właściwości tylko do odczytu i samodzielnie utworzyć wystąpienia zestawu. Na przykład:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

Należy zauważyć, że DbContext buforuje wystąpienie elementu Nieogólnymi zwracanego z metody Set, tak aby każda z tych właściwości zwracała to samo wystąpienie przy każdym wywołaniu.  

Odnajdywanie typów jednostek dla Code First działa w taki sam sposób jak w przypadku właściwości z publicznymi metodami pobierającymi i metodami ustawiającymi.  

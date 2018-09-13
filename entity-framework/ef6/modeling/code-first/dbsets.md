---
title: Definiowanie DbSets - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489001"
---
# <a name="defining-dbsets"></a>Definiowanie DbSets
Podczas programowania z użyciem kodu pierwszego przepływu pracy należy zdefiniować pochodnego typu DbContext, który reprezentuje sesję z bazą danych i udostępnia DbSet dla każdego typu w modelu. W tym temacie omówiono różne sposoby, można zdefiniować właściwości DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>Kontekst DbContext z właściwościami DbSet  

Często spotykana pokazano w przykładach Code First jest typu DbContext za pomocą właściwości publiczne automatyczne DbSet typów jednostek w modelu. Na przykład:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

W przypadku użycia w trybie Code First, to zostanie skonfigurowany jako typy jednostek, jak również konfigurowanie innych typów, które są osiągalne z tych blogów i wpisów. Ponadto DbContext będzie automatycznie wywoływać metody ustawiającej dla każdej z tych właściwości można ustawić wystąpienie DbSet odpowiednie.  

## <a name="dbcontext-with-idbset-properties"></a>Kontekst DbContext z właściwościami IDbSet  

Istnieją sytuacje, na przykład w przypadku tworzenia mocks lub elementów sztucznych, gdzie jest bardziej użyteczny zadeklarować właściwości zestawu przy użyciu interfejsu. W takich przypadkach IDbSet interfejs może służyć zamiast DbSet. Na przykład:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Ten kontekst działa w taki sam sposób, jak kontekst, który używa klasy DbSet dla jego właściwości zestawu.  

## <a name="dbcontext-with-read-only-set-properties"></a>Kontekst DbContext za pomocą właściwości tylko do odczytu zestawu  

Jeśli nie chcesz ujawniać publicznej metody ustawiające dla właściwości DbSet lub IDbSet zamiast tego można utworzyć właściwości tylko do odczytu i samodzielnie utworzyć wystąpień zestawu. Na przykład:  

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

Należy pamiętać, że DbContext buforuje wystąpienie DbSet zwrócony z metody Set, tak, aby każda z tych właściwości zwróci to samo wystąpienie, za każdym razem, gdy jest wywoływana.  

Odnajdywanie typów jednostek dla Code First działa tak samo jak postaci, w jakiej jest dla właściwości publicznej metod pobierających i ustawiających.  

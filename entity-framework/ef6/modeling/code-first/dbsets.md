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
# <a name="defining-dbsets"></a><span data-ttu-id="43342-102">Definiowanie DbSets</span><span class="sxs-lookup"><span data-stu-id="43342-102">Defining DbSets</span></span>
<span data-ttu-id="43342-103">Podczas programowania z użyciem kodu pierwszego przepływu pracy należy zdefiniować pochodnego typu DbContext, który reprezentuje sesję z bazą danych i udostępnia DbSet dla każdego typu w modelu.</span><span class="sxs-lookup"><span data-stu-id="43342-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="43342-104">W tym temacie omówiono różne sposoby, można zdefiniować właściwości DbSet.</span><span class="sxs-lookup"><span data-stu-id="43342-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="43342-105">Kontekst DbContext z właściwościami DbSet</span><span class="sxs-lookup"><span data-stu-id="43342-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="43342-106">Często spotykana pokazano w przykładach Code First jest typu DbContext za pomocą właściwości publiczne automatyczne DbSet typów jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="43342-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="43342-107">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="43342-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="43342-108">W przypadku użycia w trybie Code First, to zostanie skonfigurowany jako typy jednostek, jak również konfigurowanie innych typów, które są osiągalne z tych blogów i wpisów.</span><span class="sxs-lookup"><span data-stu-id="43342-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="43342-109">Ponadto DbContext będzie automatycznie wywoływać metody ustawiającej dla każdej z tych właściwości można ustawić wystąpienie DbSet odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="43342-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="43342-110">Kontekst DbContext z właściwościami IDbSet</span><span class="sxs-lookup"><span data-stu-id="43342-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="43342-111">Istnieją sytuacje, na przykład w przypadku tworzenia mocks lub elementów sztucznych, gdzie jest bardziej użyteczny zadeklarować właściwości zestawu przy użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="43342-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="43342-112">W takich przypadkach IDbSet interfejs może służyć zamiast DbSet.</span><span class="sxs-lookup"><span data-stu-id="43342-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="43342-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="43342-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="43342-114">Ten kontekst działa w taki sam sposób, jak kontekst, który używa klasy DbSet dla jego właściwości zestawu.</span><span class="sxs-lookup"><span data-stu-id="43342-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="43342-115">Kontekst DbContext za pomocą właściwości tylko do odczytu zestawu</span><span class="sxs-lookup"><span data-stu-id="43342-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="43342-116">Jeśli nie chcesz ujawniać publicznej metody ustawiające dla właściwości DbSet lub IDbSet zamiast tego można utworzyć właściwości tylko do odczytu i samodzielnie utworzyć wystąpień zestawu.</span><span class="sxs-lookup"><span data-stu-id="43342-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="43342-117">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="43342-117">For example:</span></span>  

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

<span data-ttu-id="43342-118">Należy pamiętać, że DbContext buforuje wystąpienie DbSet zwrócony z metody Set, tak, aby każda z tych właściwości zwróci to samo wystąpienie, za każdym razem, gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="43342-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="43342-119">Odnajdywanie typów jednostek dla Code First działa tak samo jak postaci, w jakiej jest dla właściwości publicznej metod pobierających i ustawiających.</span><span class="sxs-lookup"><span data-stu-id="43342-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  

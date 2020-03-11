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
# <a name="defining-dbsets"></a><span data-ttu-id="62a37-102">Definiowanie dbsets</span><span class="sxs-lookup"><span data-stu-id="62a37-102">Defining DbSets</span></span>
<span data-ttu-id="62a37-103">Podczas tworzenia przy użyciu przepływu pracy Code First definiujesz pochodny DbContext, który reprezentuje sesję z bazą danych i uwidacznia Nieogólnymi dla każdego typu w modelu.</span><span class="sxs-lookup"><span data-stu-id="62a37-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="62a37-104">W tym temacie opisano różne sposoby definiowania właściwości Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="62a37-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="62a37-105">DbContext z właściwościami Nieogólnymi</span><span class="sxs-lookup"><span data-stu-id="62a37-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="62a37-106">Typowy przypadek przedstawiony w Code First przykłady ma mieć DbContext z publicznymi właściwościami Nieogólnymi dla typów jednostek modelu.</span><span class="sxs-lookup"><span data-stu-id="62a37-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="62a37-107">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="62a37-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="62a37-108">Gdy jest używany w trybie Code First, spowoduje to skonfigurowanie blogów i wpisów jako typów jednostek, a także skonfigurowanie innych typów dostępnych z tych obiektów.</span><span class="sxs-lookup"><span data-stu-id="62a37-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="62a37-109">Dodatkowo DbContext automatycznie wywoła metodę ustawiającą dla każdej z tych właściwości, aby ustawić wystąpienie odpowiedniego Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="62a37-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="62a37-110">DbContext z właściwościami IDbSet</span><span class="sxs-lookup"><span data-stu-id="62a37-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="62a37-111">Istnieją sytuacje, takie jak podczas tworzenia imitacji lub sztucznych, gdzie bardziej przydatne jest zadeklarowanie właściwości zestawu przy użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="62a37-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="62a37-112">W takich przypadkach można użyć interfejsu IDbSet zamiast Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="62a37-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="62a37-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="62a37-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="62a37-114">Ten kontekst działa w taki sam sposób, jak kontekst, który używa klasy Nieogólnymi dla właściwości zestawu.</span><span class="sxs-lookup"><span data-stu-id="62a37-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="62a37-115">DbContext z właściwościami zestawu tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="62a37-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="62a37-116">Jeśli nie chcesz ujawniać publicznych metod ustawiających właściwości Nieogólnymi lub IDbSet, możesz zamiast tego utworzyć właściwości tylko do odczytu i samodzielnie utworzyć wystąpienia zestawu.</span><span class="sxs-lookup"><span data-stu-id="62a37-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="62a37-117">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="62a37-117">For example:</span></span>  

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

<span data-ttu-id="62a37-118">Należy zauważyć, że DbContext buforuje wystąpienie elementu Nieogólnymi zwracanego z metody Set, tak aby każda z tych właściwości zwracała to samo wystąpienie przy każdym wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="62a37-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="62a37-119">Odnajdywanie typów jednostek dla Code First działa w taki sam sposób jak w przypadku właściwości z publicznymi metodami pobierającymi i metodami ustawiającymi.</span><span class="sxs-lookup"><span data-stu-id="62a37-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  

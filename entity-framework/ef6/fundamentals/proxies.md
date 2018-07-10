---
title: Praca z serwerów proxy - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
caps.latest.revision: 3
ms.openlocfilehash: 4632e246d28a3cd53dabe5ac76e44f4538739abc
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912065"
---
# <a name="working-with-proxies"></a><span data-ttu-id="3b59d-102">Praca z serwerów proxy</span><span class="sxs-lookup"><span data-stu-id="3b59d-102">Working with proxies</span></span>
<span data-ttu-id="3b59d-103">Podczas tworzenia wystąpień typów jednostki POCO, platformy Entity Framework często tworzy wystąpienia typu pochodnego dynamicznie generowanym, który działa jako serwer proxy dla tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="3b59d-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="3b59d-104">Ten serwer proxy zastępuje niektóre właściwości wirtualnego jednostka do wstawienia punkty zaczepienia do operacji wykonywanych automatycznie, gdy uzyskano dostęp do właściwości.</span><span class="sxs-lookup"><span data-stu-id="3b59d-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="3b59d-105">Na przykład ten mechanizm jest używany do obsługi powolne ładowanie relacji.</span><span class="sxs-lookup"><span data-stu-id="3b59d-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="3b59d-106">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="3b59d-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="3b59d-107">Wyłączanie tworzenia serwera proxy</span><span class="sxs-lookup"><span data-stu-id="3b59d-107">Disabling proxy creation</span></span>  

<span data-ttu-id="3b59d-108">Czasami przydatne jest zapobiec tworzenia wystąpień serwera proxy programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3b59d-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="3b59d-109">Na przykład serializacji wystąpień bez serwera proxy jest znacznie prostsze niż serializacji wystąpień serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="3b59d-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="3b59d-110">Tworzenie serwera proxy można wyłączyć, usuwając zaznaczenie flagi ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="3b59d-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="3b59d-111">Jest jednym miejscu, można to zrobić w Konstruktorze typu kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3b59d-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="3b59d-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b59d-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="3b59d-113">Należy pamiętać, że EF nie utworzy serwerów proxy dla typów w przypadku, gdy nie ma nic do serwera proxy w celu.</span><span class="sxs-lookup"><span data-stu-id="3b59d-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="3b59d-114">Oznacza to, czy można również uniknąć serwery proxy, przez umieszczenie typy, które są zapieczętowane i/lub mieć nie właściwości wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="3b59d-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="3b59d-115">Jawne utworzenie wystąpienia serwera proxy</span><span class="sxs-lookup"><span data-stu-id="3b59d-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="3b59d-116">Nie będzie można utworzyć wystąpienia serwera proxy, jeśli tworzone jest wystąpienie jednostki za pomocą nowego operatora.</span><span class="sxs-lookup"><span data-stu-id="3b59d-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="3b59d-117">To może nie być problemem, ale jeśli musisz utworzyć wystąpienie serwera proxy (na przykład, dzięki czemu będzie działać z opóźnieniem ładowania lub serwer proxy sprawdzaniu spójności) następnie możesz to zrobić za pomocą metody Create DbSet.</span><span class="sxs-lookup"><span data-stu-id="3b59d-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="3b59d-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b59d-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="3b59d-119">Jeśli chcesz utworzyć wystąpienie Typ pochodny jednostki można ogólnego wersję Utwórz.</span><span class="sxs-lookup"><span data-stu-id="3b59d-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="3b59d-120">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b59d-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="3b59d-121">Zauważ, że metody Create, nie dodać lub dołączyć utworzonej jednostki do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="3b59d-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="3b59d-122">Należy pamiętać, że metody Create stworzy wystąpienia typu jednostki, sam typ serwera proxy dla jednostki podczas tworzenia będzie mieć żadnej wartości, ponieważ nie będzie robić niczego.</span><span class="sxs-lookup"><span data-stu-id="3b59d-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="3b59d-123">Na przykład jeśli typ jednostki jest zapieczętowany i/lub nie ma wirtualnej właściwości, a następnie utwórz będzie po prostu utworzyć wystąpienia typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="3b59d-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="3b59d-124">Pobieranie typu rzeczywistego obiektu z typem serwera proxy</span><span class="sxs-lookup"><span data-stu-id="3b59d-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="3b59d-125">Typy serwerów proxy mają nazwy, które wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="3b59d-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="3b59d-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="3b59d-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="3b59d-127">Można znaleźć typu jednostki dla tego typu proxy przy użyciu metody Element GetObjectType od obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="3b59d-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="3b59d-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3b59d-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="3b59d-129">Należy zauważyć, że jeśli typ jest przekazywany do Element GetObjectType jest wystąpieniem typu jednostki, który nie jest typ serwera proxy następnie typu jednostki, nadal jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="3b59d-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="3b59d-130">Oznacza to, że zawsze można używać tej metody można pobrać typu rzeczywistego jednostki bez żadnych innych sprawdzania, aby sprawdzić, czy typ jest typ serwera proxy, czy nie.</span><span class="sxs-lookup"><span data-stu-id="3b59d-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  

---
title: Praca z serwerami proxy — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419340"
---
# <a name="working-with-proxies"></a><span data-ttu-id="16e16-102">Praca z serwerami proxy</span><span class="sxs-lookup"><span data-stu-id="16e16-102">Working with proxies</span></span>
<span data-ttu-id="16e16-103">Podczas tworzenia wystąpień typów jednostek POCO, Entity Framework często tworzy wystąpienia dynamicznie generowanego typu pochodnego, który działa jako serwer proxy dla jednostki.</span><span class="sxs-lookup"><span data-stu-id="16e16-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="16e16-104">Ten serwer proxy zastępuje pewne właściwości wirtualne jednostki, aby wstawić punkty zaczepienia do wykonywania akcji automatycznie podczas uzyskiwania dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="16e16-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="16e16-105">Na przykład ten mechanizm służy do obsługi ładowania opóźnionego relacji.</span><span class="sxs-lookup"><span data-stu-id="16e16-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="16e16-106">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="16e16-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="16e16-107">Wyłączanie tworzenia serwera proxy</span><span class="sxs-lookup"><span data-stu-id="16e16-107">Disabling proxy creation</span></span>  

<span data-ttu-id="16e16-108">Czasami warto uniemożliwić Entity Framework tworzenia wystąpień serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="16e16-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="16e16-109">Na przykład Serializacja wystąpień poza serwerem proxy jest znacznie łatwiejsza niż Serializacja wystąpień serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="16e16-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="16e16-110">Tworzenie serwera proxy można wyłączyć, czyszcząc flagę ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="16e16-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="16e16-111">Jedno miejsce można wykonać w konstruktorze kontekstu.</span><span class="sxs-lookup"><span data-stu-id="16e16-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="16e16-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="16e16-112">For example:</span></span>  

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

<span data-ttu-id="16e16-113">Należy pamiętać, że EF nie utworzy serwerów proxy dla typów, w których nie ma niczego do wykonania na serwerze proxy.</span><span class="sxs-lookup"><span data-stu-id="16e16-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="16e16-114">Oznacza to, że można również uniknąć używania typów, które są zapieczętowane i/lub nie mają właściwości wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="16e16-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="16e16-115">Jawne Tworzenie wystąpienia serwera proxy</span><span class="sxs-lookup"><span data-stu-id="16e16-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="16e16-116">Wystąpienie serwera proxy nie zostanie utworzone w przypadku utworzenia wystąpienia jednostki przy użyciu operatora new.</span><span class="sxs-lookup"><span data-stu-id="16e16-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="16e16-117">Może to nie być problem, ale jeśli musisz utworzyć wystąpienie serwera proxy (na przykład, aby ładowanie z opóźnieniem lub śledzenie zmian serwera proxy zadziałało), możesz to zrobić za pomocą metody Create elementu Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="16e16-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="16e16-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="16e16-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="16e16-119">Ogólna wersja metody Create może zostać użyta, jeśli chcesz utworzyć wystąpienie typu jednostki pochodnej.</span><span class="sxs-lookup"><span data-stu-id="16e16-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="16e16-120">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="16e16-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="16e16-121">Należy pamiętać, że metoda Create nie dodaje lub nie dołącza utworzonej jednostki do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="16e16-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="16e16-122">Należy pamiętać, że metoda Create utworzy wystąpienie samego typu jednostki, jeśli Tworzenie typu obiektu pośredniczącego dla jednostki nie będzie miało żadnej wartości, ponieważ nie zajmie to niczego.</span><span class="sxs-lookup"><span data-stu-id="16e16-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="16e16-123">Na przykład, jeśli typ jednostki jest zapieczętowany i/lub nie ma żadnych właściwości wirtualnych, utworzenie spowoduje jedynie utworzenie wystąpienia typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="16e16-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="16e16-124">Pobieranie rzeczywistego typu jednostki z typu serwera proxy</span><span class="sxs-lookup"><span data-stu-id="16e16-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="16e16-125">Typy serwerów proxy mają nazwy, które wyglądają następująco:</span><span class="sxs-lookup"><span data-stu-id="16e16-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="16e16-126">System. Data. Entity. DynamicProxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="16e16-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="16e16-127">Typ jednostki dla tego typu serwera proxy można znaleźć za pomocą metody GetObjectType z obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="16e16-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="16e16-128">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="16e16-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="16e16-129">Należy pamiętać, że jeśli typ przesłany do metody GetObjectType jest wystąpieniem typu jednostki, który nie jest typem serwera proxy, zwracany jest typ jednostki.</span><span class="sxs-lookup"><span data-stu-id="16e16-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="16e16-130">Oznacza to, że zawsze można użyć tej metody do uzyskania rzeczywistego typu jednostki bez żadnych innych kontroli, aby sprawdzić, czy typ jest typem proxy.</span><span class="sxs-lookup"><span data-stu-id="16e16-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  

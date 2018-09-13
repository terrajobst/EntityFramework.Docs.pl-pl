---
title: Praca z serwerów proxy - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489820"
---
# <a name="working-with-proxies"></a>Praca z serwerów proxy
Podczas tworzenia wystąpień typów jednostki POCO, platformy Entity Framework często tworzy wystąpienia typu pochodnego dynamicznie generowanym, który działa jako serwer proxy dla tej jednostki. Ten serwer proxy zastępuje niektóre właściwości wirtualnego jednostka do wstawienia punkty zaczepienia do operacji wykonywanych automatycznie, gdy uzyskano dostęp do właściwości. Na przykład ten mechanizm jest używany do obsługi powolne ładowanie relacji. Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.  

## <a name="disabling-proxy-creation"></a>Wyłączanie tworzenia serwera proxy  

Czasami przydatne jest zapobiec tworzenia wystąpień serwera proxy programu Entity Framework. Na przykład serializacji wystąpień bez serwera proxy jest znacznie prostsze niż serializacji wystąpień serwera proxy. Tworzenie serwera proxy można wyłączyć, usuwając zaznaczenie flagi ProxyCreationEnabled. Jest jednym miejscu, można to zrobić w Konstruktorze typu kontekstu. Na przykład:  

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

Należy pamiętać, że EF nie utworzy serwerów proxy dla typów w przypadku, gdy nie ma nic do serwera proxy w celu. Oznacza to, czy można również uniknąć serwery proxy, przez umieszczenie typy, które są zapieczętowane i/lub mieć nie właściwości wirtualnego.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Jawne utworzenie wystąpienia serwera proxy  

Nie będzie można utworzyć wystąpienia serwera proxy, jeśli tworzone jest wystąpienie jednostki za pomocą nowego operatora. To może nie być problemem, ale jeśli musisz utworzyć wystąpienie serwera proxy (na przykład, dzięki czemu będzie działać z opóźnieniem ładowania lub serwer proxy sprawdzaniu spójności) następnie możesz to zrobić za pomocą metody Create DbSet. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Jeśli chcesz utworzyć wystąpienie Typ pochodny jednostki można ogólnego wersję Utwórz. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Zauważ, że metody Create, nie dodać lub dołączyć utworzonej jednostki do kontekstu.  

Należy pamiętać, że metody Create stworzy wystąpienia typu jednostki, sam typ serwera proxy dla jednostki podczas tworzenia będzie mieć żadnej wartości, ponieważ nie będzie robić niczego. Na przykład jeśli typ jednostki jest zapieczętowany i/lub nie ma wirtualnej właściwości, a następnie utwórz będzie po prostu utworzyć wystąpienia typu jednostki.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Pobieranie typu rzeczywistego obiektu z typem serwera proxy  

Typy serwerów proxy mają nazwy, które wyglądać mniej więcej tak:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Można znaleźć typu jednostki dla tego typu proxy przy użyciu metody Element GetObjectType od obiektu ObjectContext. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Należy zauważyć, że jeśli typ jest przekazywany do Element GetObjectType jest wystąpieniem typu jednostki, który nie jest typ serwera proxy następnie typu jednostki, nadal jest zwracana. Oznacza to, że zawsze można używać tej metody można pobrać typu rzeczywistego jednostki bez żadnych innych sprawdzania, aby sprawdzić, czy typ jest typ serwera proxy, czy nie.  

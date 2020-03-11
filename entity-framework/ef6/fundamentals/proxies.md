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
# <a name="working-with-proxies"></a>Praca z serwerami proxy
Podczas tworzenia wystąpień typów jednostek POCO, Entity Framework często tworzy wystąpienia dynamicznie generowanego typu pochodnego, który działa jako serwer proxy dla jednostki. Ten serwer proxy zastępuje pewne właściwości wirtualne jednostki, aby wstawić punkty zaczepienia do wykonywania akcji automatycznie podczas uzyskiwania dostępu do właściwości. Na przykład ten mechanizm służy do obsługi ładowania opóźnionego relacji. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

## <a name="disabling-proxy-creation"></a>Wyłączanie tworzenia serwera proxy  

Czasami warto uniemożliwić Entity Framework tworzenia wystąpień serwera proxy. Na przykład Serializacja wystąpień poza serwerem proxy jest znacznie łatwiejsza niż Serializacja wystąpień serwera proxy. Tworzenie serwera proxy można wyłączyć, czyszcząc flagę ProxyCreationEnabled. Jedno miejsce można wykonać w konstruktorze kontekstu. Na przykład:  

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

Należy pamiętać, że EF nie utworzy serwerów proxy dla typów, w których nie ma niczego do wykonania na serwerze proxy. Oznacza to, że można również uniknąć używania typów, które są zapieczętowane i/lub nie mają właściwości wirtualnych.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Jawne Tworzenie wystąpienia serwera proxy  

Wystąpienie serwera proxy nie zostanie utworzone w przypadku utworzenia wystąpienia jednostki przy użyciu operatora new. Może to nie być problem, ale jeśli musisz utworzyć wystąpienie serwera proxy (na przykład, aby ładowanie z opóźnieniem lub śledzenie zmian serwera proxy zadziałało), możesz to zrobić za pomocą metody Create elementu Nieogólnymi. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Ogólna wersja metody Create może zostać użyta, jeśli chcesz utworzyć wystąpienie typu jednostki pochodnej. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Należy pamiętać, że metoda Create nie dodaje lub nie dołącza utworzonej jednostki do kontekstu.  

Należy pamiętać, że metoda Create utworzy wystąpienie samego typu jednostki, jeśli Tworzenie typu obiektu pośredniczącego dla jednostki nie będzie miało żadnej wartości, ponieważ nie zajmie to niczego. Na przykład, jeśli typ jednostki jest zapieczętowany i/lub nie ma żadnych właściwości wirtualnych, utworzenie spowoduje jedynie utworzenie wystąpienia typu jednostki.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Pobieranie rzeczywistego typu jednostki z typu serwera proxy  

Typy serwerów proxy mają nazwy, które wyglądają następująco:  

System. Data. Entity. DynamicProxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Typ jednostki dla tego typu serwera proxy można znaleźć za pomocą metody GetObjectType z obiektu ObjectContext. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Należy pamiętać, że jeśli typ przesłany do metody GetObjectType jest wystąpieniem typu jednostki, który nie jest typem serwera proxy, zwracany jest typ jednostki. Oznacza to, że zawsze można użyć tej metody do uzyskania rzeczywistego typu jednostki bez żadnych innych kontroli, aby sprawdzić, czy typ jest typem proxy.  

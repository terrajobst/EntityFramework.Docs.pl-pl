---
title: Parametry połączenia i modeli — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490751"
---
# <a name="connection-strings-and-models"></a>Parametry połączenia i modeli
W tym temacie opisano, jak Entity Framework umożliwia odnalezienie połączenie bazy danych i jak można ją zmienić. Modele utworzone przy użyciu Code First i projektancie platformy EF zostały omówione w tym temacie.  

Zazwyczaj aplikacja programu Entity Framework używa klasy pochodzącej od typu DbContext. Ta klasa pochodna wywoła jednym z konstruktorów w klasie bazowej DbContext do sterowania:  

- Jak kontekst połączy się z bazą danych — oznacza to, jak ciąg połączenia jest znaleziono użyć  
- Kontekst użyje obliczania modelu za pomocą funkcji Code First czy załadować model utworzony za pomocą projektanta EF  
- Dodatkowe opcje zaawansowane  

Następujące fragmenty pokazano niektóre sposoby konstruktorów typu DbContext może być używana.  

## <a name="use-code-first-with-connection-by-convention"></a>Za pomocą Code First połączenia zgodnie z Konwencją  

W przypadku dowolnej innej konfiguracji nie zostało wykonane w aplikacji, wywołując konstruktora bez parametrów na DbContext spowoduje, że DbContext do pracy w trybie Code First przy użyciu połączenia z bazą danych utworzonych przez Konwencję. Na przykład:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

W tym przykładzie DbContext używa w przestrzeni nazw kwalifikowana nazwa Twojej class—Demo.EF.BloggingContext—as pochodnej kontekstu nazwy bazy danych i tworzy ciąg połączenia dla tej bazy danych przy użyciu programu SQL Express lub LocalDB. Jeśli obie są zainstalowane, będzie używany program SQL Express.  

Visual Studio 2010 zawiera programu SQL Express, domyślnie i programu Visual Studio 2012 i nowszych obejmują LocalDB. Podczas instalacji pakietu EntityFramework NuGet sprawdza, czy serwer bazy danych, która jest dostępna. Pakiet NuGet zostanie następnie zaktualizuj plik konfiguracji ustawienia domyślnego serwera bazy danych, który używa Code First, podczas tworzenia połączenia zgodnie z Konwencją. Jeśli program SQL Express jest uruchomiona, będzie używany. Jeśli program SQL Express nie jest dostępna następnie LocalDB zostanie zarejestrowana jako domyślny zamiast tego. Nie zmian do pliku konfiguracji, jeśli zawiera on już ustawienie domyślna fabryka połączenia.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Za pomocą Code First połączenia, Konwencji i określoną nazwę bazy danych  

Jeśli nie wykonano dowolnej innej konfiguracji w aplikacji, następnie wywoływania konstruktora ciągu na DbContext, nazwą bazy danych, którego chcesz użyć spowoduje, że DbContext do pracy w trybie Code First przy użyciu połączenia z bazą danych utworzony zgodnie z Konwencją do bazy danych tę nazwę. Na przykład:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

W tym przykładzie DbContext używa "BloggingDatabase" jako nazwy bazy danych i tworzy ciąg połączenia dla tej bazy danych przy użyciu programu SQL Express (zainstalowany za pomocą programu Visual Studio 2010) lub LocalDB (zainstalowany za pomocą programu Visual Studio 2012). Jeśli obie są zainstalowane, będzie używany program SQL Express.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Za pomocą Code First parametrów połączenia w pliku app.config/web.config  

Można umieścić ciąg połączenia w pliku app.config lub web.config. Na przykład:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Jest to prosty sposób sprawdzić DbContext do korzystania z serwera bazy danych innych niż SQL Express lub LocalDB — w powyższym przykładzie określa bazę danych programu SQL Server Compact Edition.  

Jeśli nazwa ciągu połączenia jest zgodna z nazwą kontekstu (z lub bez kwalifikacji przestrzeni nazw) następnie go będą mogli odnaleźć DbContext stosowania konstruktora bez parametrów. Nazwa parametrów połączenia jest inna niż Nazwa kontekstu można określić typu DbContext, aby używać tego połączenia w trybie Code First, przekazując nazwę parametrów połączenia do konstruktora typu DbContext. Na przykład:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternatywnie można użyć formy "nazwa =\<nazwa parametrów połączenia\>" dla ciągu przekazany do konstruktora typu DbContext. Na przykład:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Ten formularz, sprawia, że jawne, oczekiwany ciąg połączenia, można znaleźć w pliku konfiguracji. Jeśli nie można odnaleźć parametrów połączenia o podanej nazwie, zostanie zgłoszony wyjątek.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Pierwszy Model/bazy danych przy użyciu parametrów połączenia w pliku app.config/web.config  

Modele utworzone za pomocą projektanta EF różnią się od Code First w tym modelu już istnieje i nie jest generowany z kodu, gdy aplikacja zostanie uruchomiona. Ten model jest zwykle istnieje jako plik EDMX w projekcie.  

Projektant doda EF parametry połączenia do pliku app.config lub web.config. Parametry połączenia są specjalne, ponieważ zawiera informacje o sposobach znajdowania informacji w pliku EDMX. Na przykład:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

W Projektancie platformy EF także wygeneruje kod, który zawiera informacje dla kontekstu DbContext dla tego połączenia, przekazując nazwę parametrów połączenia do konstruktora typu DbContext. Na przykład:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

Kontekst DbContext wie, można załadować istniejącego modelu (a nie za pomocą Code First go obliczyć z kodu), ponieważ parametry połączenia są parametry połączenia platformy EF zawierającego szczegółowe informacje o modelu do użycia.  

## <a name="other-dbcontext-constructor-options"></a>Inne opcje konstruktora typu DbContext  

Klasy DbContext zawiera inne konstruktory i wzorce użycia, które umożliwiają niektórych bardziej zaawansowanych scenariuszy. Niektóre z nich to:  

- Klasa DbModelBuilder umożliwia tworzenie model Code First bez tworzenia wystąpienia wystąpienie typu DbContext. Z tego powodu jest obiektem DbModel. Możesz następnie przekazać ten obiekt DbModel do jednego z konstruktorów typu DbContext, gdy jesteś gotowy do utworzenia wystąpienia typu DbContext.  
- Pełny ciąg połączenia można przekazać do typu DbContext, zamiast tylko nazwy ciągu bazy danych lub połączenie. Domyślnie ten ciąg połączenia jest używana z dostawcą System.Data.SqlClient; Można to zmienić, ustawiając z inną implementacją IConnectionFactory na kontekście. Database.DefaultConnectionFactory.  
- Można użyć istniejącego obiektu DbConnection, przekazując go do konstruktora typu DbContext. Jeśli obiekt połączenia jest wystąpieniem EntityConnection, określony w połączeniu model nie będą używane zamiast obliczania modelu przy użyciu najpierw kod. Jeśli obiekt jest wystąpieniem innego typu — na przykład element SqlConnection — a następnie kontekst użyje go w trybie Code First.  
- Istniejący obiekt ObjectContext można przekazać do konstruktora typu DbContext do utworzenia typu DbContext zawijania istniejącego kontekstu. Może to służyć do istniejących aplikacji, które używają obiektu ObjectContext, ale którym chcesz korzystać z zalet DbContext w niektórych części aplikacji.  

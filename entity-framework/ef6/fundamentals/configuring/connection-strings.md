---
title: Parametry połączenia i modele — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419298"
---
# <a name="connection-strings-and-models"></a>Parametry połączenia i modele
W tym temacie opisano, w jaki sposób Entity Framework wykrywają używane połączenie z bazą danych i jak można je zmienić. Modele utworzone przy użyciu Code First i programu Dr Designer zostały omówione w tym temacie.  

Zazwyczaj aplikacja Entity Framework używa klasy pochodnej DbContext. Ta klasa pochodna wywoła jeden z konstruktorów w klasie Base DbContext, aby kontrolować:  

- Jak kontekst będzie łączyć się z bazą danych — to znaczy, jak znaleziono/użyto parametrów połączenia  
- Czy kontekst będzie używać obliczeń dla modelu przy użyciu Code First lub załadowania modelu utworzonego za pomocą narzędzia Dr Designer  
- Dodatkowe opcje zaawansowane  

Poniższe fragmenty pokazują, jak można użyć konstruktorów DbContext.  

## <a name="use-code-first-with-connection-by-convention"></a>Używanie Code First z połączeniem według Konwencji  

Jeśli nie wykonano żadnej innej konfiguracji w aplikacji, wywołanie konstruktora bez parametrów w DbContext spowoduje uruchomienie DbContext w trybie Code First przy użyciu połączenia bazy danych utworzonego zgodnie z Konwencją. Na przykład:  

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

W tym przykładzie DbContext używa kwalifikowanej nazwy klasy kontekstu pochodnego — demonstracyjne. EF. BloggingContext — jako nazwy bazy danych i tworzy parametry połączenia dla tej bazy danych przy użyciu programu SQL Express lub LocalDB. W przypadku zainstalowania obu tych programów zostanie użyta opcja SQL Express.  

Program Visual Studio 2010 zawiera domyślnie program SQL Express, a program Visual Studio 2012 i nowsze zawierają LocalDB. Podczas instalacji pakiet NuGet EntityFramework sprawdza, który serwer bazy danych jest dostępny. Pakiet NuGet zaktualizuje plik konfiguracyjny przez ustawienie domyślnego serwera bazy danych, który Code First używa podczas tworzenia połączenia według Konwencji. Jeśli program SQL Express jest uruchomiony, będzie używany. Jeśli program SQL Express nie jest dostępny, LocalDB zostanie zarejestrowany jako domyślny. Nie wprowadzono żadnych zmian w pliku konfiguracji, jeśli zawiera on już ustawienie domyślnej fabryki połączeń.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Użyj Code First z połączeniem według Konwencji i określonej nazwy bazy danych  

Jeśli nie wykonano żadnej innej konfiguracji w aplikacji, wywołanie konstruktora String w DbContext z nazwą bazy danych, której chcesz użyć, spowoduje uruchomienie DbContext w trybie Code First przy użyciu połączenia z bazą danych utworzonego w ramach Konwencji do bazy danych programu Ta nazwa. Na przykład:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

W tym przykładzie DbContext wykorzystuje "BloggingDatabase" jako nazwę bazy danych i tworzy parametry połączenia dla tej bazy danych przy użyciu programu SQL Express (zainstalowanego z programem Visual Studio 2010) lub LocalDB (instalowany z programem Visual Studio 2012). W przypadku zainstalowania obu tych programów zostanie użyta opcja SQL Express.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Użyj Code First z parametrami połączenia w pliku App. config/Web. config  

Możesz zdecydować się na umieszczenie parametrów połączenia w pliku App. config lub Web. config. Na przykład:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Jest to prosty sposób, aby poinformować DbContext o korzystaniu z serwera bazy danych innego niż SQL Express lub LocalDB — powyższy przykład określa bazę danych SQL Server Compact Edition.  

Jeśli nazwa parametrów połączenia jest zgodna z nazwą kontekstu (z kwalifikacją lub bez), zostanie ona znaleziona przez DbContext, gdy używany jest Konstruktor bez parametrów. Jeśli nazwa parametrów połączenia różni się od nazwy kontekstu, można powiedzieć DbContext, aby użyć tego połączenia w trybie Code First, przekazując nazwę ciągu połączenia do konstruktora DbContext. Na przykład:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternatywnie można użyć formularza "name =\<nazwa parametrów połączenia\>" dla ciągu przesłanego do konstruktora DbContext. Na przykład:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Ten formularz określa, że oczekiwano, że parametry połączenia będą znajdować się w pliku konfiguracyjnym. Jeśli nie zostanie znaleziony ciąg połączenia o podaną nazwę, zostanie zgłoszony wyjątek.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Baza danych/Model First z parametrami połączenia w pliku App. config/Web. config  

Modele utworzone przy użyciu narzędzia Dr Designer różnią się od Code First w tym, że model już istnieje i nie jest generowany na podstawie kodu podczas uruchamiania aplikacji. Model zazwyczaj istnieje jako plik EDMX w projekcie.  

Projektant doda parametry połączenia EF do pliku App. config lub Web. config. Te parametry połączenia mają specjalne informacje o sposobach znajdowania informacji w pliku EDMX. Na przykład:  

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

Program Dr Designer generuje również kod, który informuje DbContext o konieczności użycia tego połączenia, przekazując nazwę ciągu połączenia do konstruktora DbContext. Na przykład:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext wie, aby załadować istniejący model (zamiast używać Code First do obliczenia go z kodu), ponieważ parametry połączenia to parametry połączenia EF zawierające szczegółowe informacje o modelu do użycia.  

## <a name="other-dbcontext-constructor-options"></a>Inne opcje konstruktora DbContext  

Klasa DbContext zawiera inne konstruktory i wzorce użycia, które umożliwiają bardziej zaawansowane scenariusze. Niektóre z nich są następujące:  

- Klasy DbModelBuilder można użyć do kompilowania modelu Code First bez wystąpienia wystąpienia DbContext. Wynikiem tego jest obiekt dbmodel. Następnie można przekazać ten obiekt dbmodel do jednego z konstruktorów DbContext, gdy wszystko jest gotowe do utworzenia wystąpienia DbContext.  
- Można przekazać pełne parametry połączenia do DbContext zamiast tylko nazwę bazy danych lub parametrów połączenia. Domyślnie te parametry połączenia są używane z dostawcą system. Data. SqlClient; można to zmienić, ustawiając inną implementację IConnectionFactory w kontekście. Database. DefaultConnectionFactory.  
- Istniejący obiekt DbConnection można użyć przez przekazanie go do konstruktora DbContext. Jeśli obiekt połączenia jest wystąpieniem elementu EntityConnection, zostanie użyty model określony w połączeniu zamiast obliczania modelu przy użyciu Code First. Jeśli obiekt jest wystąpieniem innego typu — na przykład sqlconnecting — kontekst będzie używać go dla trybu Code First.  
- Można przekazać istniejący obiekt ObjectContext do konstruktora DbContext, aby utworzyć DbContext zawija istniejący kontekst. Ta funkcja może być używana w przypadku istniejących aplikacji, które używają obiektu ObjectContext, ale chcą korzystać z DbContext w niektórych częściach aplikacji.  

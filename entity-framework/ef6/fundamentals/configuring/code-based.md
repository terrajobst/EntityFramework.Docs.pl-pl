---
title: Konfiguracja oparta na kodu - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325356"
---
# <a name="code-based-configuration"></a>Konfiguracja na podstawie kodu
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Konfigurację dla aplikacji platformy Entity Framework można określić w pliku konfiguracji (app.config/web.config) lub za pomocą kodu. Który jest znany jako Konfiguracja na podstawie kodu.  

Konfiguracja w pliku konfiguracji jest opisana w [oddzielny artykuł](config-file.md). Plik konfiguracji, który ma pierwszeństwo przed konfiguracja na podstawie kodu. Innymi słowy Jeśli opcja konfiguracji jest ustawiona, zarówno kod i w pliku konfiguracji, następnie ustawienie w pliku konfiguracji jest używany.  

## <a name="using-dbconfiguration"></a>Za pomocą DbConfiguration  

Konfiguracja oparta na kod w EF6 i nowszych można uzyskać, tworząc podklasa System.Data.Entity.Config.DbConfiguration. Poniższe wskazówki dotyczą sytuacji, gdy Tworzenie podklasy DbConfiguration:  

- Utwórz tylko jedną klasę DbConfiguration dla aplikacji. Ta klasa określa ustawienia dla całego domeny aplikacji.  
- Umieść klasy DbConfiguration z tego samego zestawu jako swojej klasy DbContext. (Zobacz *przenoszenie DbConfiguration* sekcji, jeśli chcesz to zmienić.)  
- Nadaj swojej klasy DbConfiguration publicznego konstruktora bez parametrów.  
- Ustawianie opcji konfiguracji, wywołując DbConfiguration metody chronionej z w ramach tego konstruktora.  

Następujące wskazówki umożliwia EF wykrywanie i używanie konfigurację automatycznie, zarówno narzędzi, który ma dostęp do modelu i uruchamiania aplikacji.  

## <a name="example"></a>Przykład  

Klasy pochodzącej od DbConfiguration może wyglądać następująco:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

Ta klasa konfiguruje EF, aby użyć strategii wykonywania SQL Azure — do automatycznego ponawiania próby wykonania operacji bazy danych nie powiodło się — i Użyj lokalnej bazy danych dla baz danych, które są tworzone zgodnie z Konwencją z Code First.  

## <a name="moving-dbconfiguration"></a>Przenoszenie DbConfiguration  

Istnieją przypadki, w którym nie jest możliwe do umieszczenia na klasę DbConfiguration z tego samego zestawu jako swojej klasy DbContext. Na przykład masz dwie klasy DbContext, każdy w różnych zestawach. Dostępne są dwie opcje do obsługi tego.  

Pierwszym z nich jest określenie wystąpienia DbConfiguration do użycia przy użyciu pliku konfiguracji. Aby to zrobić, należy ustawić atrybut codeConfigurationType sekcji platformy entityFramework. Na przykład:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Wartość codeConfigurationType musi być zestawu i przestrzeni nazw kwalifikowana nazwa klasy DbConfiguration.  

Drugą opcją jest umieścić DbConfigurationTypeAttribute na klasie kontekstu. Na przykład:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Wartość przekazywana do atrybutu mogą być typu DbConfiguration — jak pokazano powyżej — lub zestawu i przestrzeni nazw kwalifikowana ciąg nazwy typu. Na przykład:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Jawne ustawianie DbConfiguration  

Istnieją sytuacje, w którym konfiguracji może być wymagane przed dowolnego typu DbContext został już użyty. Przykłady to między innymi:  

- Za pomocą DbModelBuilder w celu zbudowania modelu bez kontekstu  
- Przy użyciu innych framework/narzędzie kodu korzystającej z typu DbContext, gdzie tego kontekstu jest używana, zanim kontekst aplikacji jest używana  

W takich sytuacjach EF jest nie można automatycznie wykryć konfiguracji i zamiast tego należy wykonać jedną z następujących czynności:  

- Ustaw typ DbConfiguration w pliku konfiguracji, zgodnie z opisem w *przenoszenie DbConfiguration* powyższej sekcji
- Wywołanie metody statycznej DbConfiguration.SetConfiguration podczas uruchamiania aplikacji  

## <a name="overriding-dbconfiguration"></a>Zastępowanie DbConfiguration  

Istnieją sytuacje, w których trzeba zastąpić konfigurację w DbConfiguration. Nie jest to zazwyczaj wykonywane przez deweloperów aplikacji, ale raczej przez dostawców innych firm i wtyczek, które nie mogą używać klasy pochodnej DbConfiguration.  

W tym celu EntityFramework umożliwia program obsługi zdarzeń do zarejestrowania, które można zmodyfikować istniejącą konfigurację, po prostu, zanim zostanie zablokowane w dół.  Zapewnia także metody sugar specjalnie w celu zastąpienia dowolnej usługi zwrócony przez EF wspólnym lokalizatorze usług. Jest to, jak jest przeznaczony do użycia:  

- Przy uruchamianiu aplikacji (przed użyciem EF) wtyczki lub dostawcy należy zarejestrować metody obsługi zdarzeń dla tego zdarzenia. (Zwróć uwagę, że to musi się zdarzyć, zanim aplikacja używa EF).  
- Program obsługi zdarzeń wywołuje ReplaceService dla każdej usługi, który ma zostać zastąpione.  

Na przykład repalce IDbConnectionFactory i DbProviderService należy zarejestrować program obsługi podobnie do następującej:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

W kodzie powyżej MyProviderServices i MyConnectionFactory reprezentują usługi implementacji usługi.  

Można również dodać dodatkową zależność programów obsługi, aby uzyskać ten sam efekt.  

Należy pamiętać, że DbProviderFactory można również opakować w ten sposób, ale spowoduje to więc mają wpływ tylko na EF i nie używa DbProviderFactory poza EF. Z tego powodu należy prawdopodobnie opakować DbProviderFactory, jak mają przed w dalszym ciągu.  

Należy również mieć na uwadze usług, które uruchamiasz zewnętrznie do Twojej aplikacji — na przykład podczas uruchamiania migracji z konsoli Menedżera pakietów. Po uruchomieniu migracji z konsoli, spróbuje ona znaleźć Twoje DbConfiguration. Niezależnie od tego czy pobierze opakowana usługi zależy jednak gdzie on zarejestrowany program obsługi zdarzeń. Jeśli jest zarejestrowany w ramach konstrukcji swoje DbConfiguration kod powinien zostać wykonany, a powinien pobrać opakowane usługi. Zazwyczaj nie będzie to mieć miejsce, i oznacza to, że narzędzia nie będą otrzymywać opakowana usługi.  

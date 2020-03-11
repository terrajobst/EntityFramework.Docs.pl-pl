---
title: Konfiguracja oparta na kodzie — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417870"
---
# <a name="code-based-configuration"></a>Konfiguracja oparta na kodzie
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Konfigurację aplikacji Entity Framework można określić w pliku konfiguracji (App. config/Web. config) lub za pomocą kodu. Ta ostatnia jest znana jako Konfiguracja oparta na kodzie.  

Konfiguracja w pliku konfiguracji jest opisana w [osobnym artykule](config-file.md). Plik konfiguracji ma pierwszeństwo przed konfiguracją opartą na kodzie. Innymi słowy, jeśli opcja konfiguracji jest ustawiona w kodzie i w pliku konfiguracji, zostanie użyta wartość ustawienia w pliku konfiguracji.  

## <a name="using-dbconfiguration"></a>Korzystanie z usługi dbconfiguration  

Konfiguracja oparta na kodzie w EF6 i powyżej zostanie osiągnięta przez utworzenie podklasy system. Data. Entity. config. dbconfiguration. Podczas podklasy dbconfiguration należy przestrzegać następujących wytycznych:  

- Utwórz tylko jedną klasę dbconfiguration dla swojej aplikacji. Ta klasa określa ustawienia całej domeny aplikacji.  
- Umieść klasę dbconfiguration w tym samym zestawie, co Klasa DbContext. (Zapoznaj się z sekcją *przeniesienie dbconfiguration* , jeśli chcesz ją zmienić).  
- Nadaj klasie dbconfiguration konstruktorowi publiczny bez parametrów.  
- Ustawianie opcji konfiguracji przez wywoływanie metod chronionych dbconfiguration z poziomu tego konstruktora.  

Zgodnie z poniższymi wskazówkami firma Dr może automatycznie odnajdywać i korzystać z konfiguracji w ramach obu narzędzi, które muszą uzyskać dostęp do modelu i kiedy aplikacja jest uruchomiona.  

## <a name="example"></a>Przykład  

Klasa pochodna klasy dbconfiguration może wyglądać następująco:  

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

Ta klasa konfiguruje EF do korzystania z strategii wykonywania SQL Azure — do automatycznego ponawiania próby wykonania nieudanych operacji w bazie danych — oraz do korzystania z lokalnej bazy danych dla baz danych utworzonych przy użyciu konwencji z Code First.  

## <a name="moving-dbconfiguration"></a>Przeniesienie dbconfiguration  

Istnieją przypadki, w których nie można umieścić klasy dbconfiguration w tym samym zestawie, co Klasa DbContext. Można na przykład mieć dwie klasy DbContext, każdy w różnych zestawach. Dostępne są dwie opcje obsługi tego elementu.  

Pierwsza opcja polega na użyciu pliku konfiguracji, aby określić wystąpienie dbconfiguration, które ma być używane. W tym celu należy ustawić atrybut codeConfigurationType sekcji entityFramework. Na przykład:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Wartość codeConfigurationType musi być zestawu i kwalifikowana nazwa przestrzeni nazw klasy dbconfiguration.  

Druga opcja polega na umieszczeniu DbConfigurationTypeAttribute w klasie kontekstowej. Na przykład:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Wartość przekazana do atrybutu może być typu dbconfiguration — jak pokazano powyżej — lub zestawu i przestrzeni nazw kwalifikowana nazwa typu. Na przykład:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Jawne ustawienie dbconfiguration  

Istnieją sytuacje, w których może być wymagana konfiguracja przed użyciem dowolnego typu DbContext. Przykłady takich elementów obejmują:  

- Tworzenie modelu bez kontekstu przy użyciu DbModelBuilder  
- Korzystanie z innego kodu platformy/narzędzia, który korzysta z kontekstu DbContext, w którym ten kontekst jest używany przed użyciem kontekstu aplikacji  

W takich sytuacjach EF nie jest możliwe automatyczne odnajdowanie konfiguracji i należy wykonać jedną z następujących czynności:  

- Ustaw typ dbconfiguration w pliku konfiguracyjnym, zgodnie z opisem w sekcji *przeniesienie dbconfiguration* powyżej
- Wywołaj metodę static dbconfiguration. SetConfiguration podczas uruchamiania aplikacji  

## <a name="overriding-dbconfiguration"></a>Zastępowanie dbconfiguration  

Istnieją sytuacje, w których należy zastąpić zestaw konfiguracyjny w konfiguracji dbconfiguration. Nie jest to zwykle wykonywane przez deweloperów aplikacji, ale raczej od dostawców i wtyczek innych firm, które nie mogą używać pochodnej klasy dbconfiguration.  

W tym celu EntityFramework umożliwia rejestrację programu obsługi zdarzeń, który może zmodyfikować istniejącą konfigurację tuż przed zablokowaniem.  Zapewnia również metodę cukru, w odniesieniu do zamiany każdej usługi zwróconej przez Lokalizator usługi EF. Jest to sposób przeznaczony do użycia:  

- Podczas uruchamiania aplikacji (przed użyciem EF) wtyczka lub dostawca powinna zarejestrować metodę obsługi zdarzeń dla tego zdarzenia. (Należy pamiętać, że musi się to zdarzyć przed zastosowaniem EF przez aplikację.)  
- Program obsługi zdarzeń wywołuje ReplaceService dla każdej usługi, która musi zostać zastąpiona.  

Na przykład, aby zastąpić IDbConnectionFactory i DbProviderService, należy zarejestrować procedurę obsługi w następujący sposób:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

W kodzie powyżej MyProviderServices i MyConnectionFactory reprezentuje Twoje wdrożenia usługi.  

Możesz również dodać dodatkowe programy obsługi zależności, aby uzyskać ten sam efekt.  

Należy zauważyć, że w ten sposób można również zawijać DbProviderFactory, ale tak samo będzie miało wpływ tylko na EF i nie używa DbProviderFactory poza EF. Z tego powodu prawdopodobnie zechcesz nadal zawijać DbProviderFactory jak wcześniej.  

Należy również pamiętać o usługach, które są uruchamiane zewnętrznie do aplikacji — na przykład podczas przeprowadzania migracji z konsoli Menedżera pakietów. Po uruchomieniu migracji z konsoli, podejmie próbę znalezienia dbconfiguration. Jednak niezależnie od tego, czy usługa zostanie zarejestrowana, zależy od tego, gdzie zarejestrowano procedurę obsługi zdarzeń. Jeśli jest ona zarejestrowana w ramach konstruowania dbconfiguration, kod powinien zostać wykonany i usługa powinna zostać opakowana. Zwykle nie jest to przypadek i oznacza to, że narzędzia nie będą korzystać z opakowanej usługi.  

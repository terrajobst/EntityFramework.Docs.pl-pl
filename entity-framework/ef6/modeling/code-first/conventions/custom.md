---
title: Niestandardowe Konwencje Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419228"
---
# <a name="custom-code-first-conventions"></a>Niestandardowe Konwencje Code First
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

W przypadku korzystania z Code First model jest obliczany na podstawie klas przy użyciu zestawu Konwencji. Domyślne [konwencje Code First](~/ef6/modeling/code-first/conventions/built-in.md) określają, jak, która właściwość jest kluczem podstawowym jednostki, nazwą tabeli, do której jest mapowany obiekt, i co określa precyzję i skalę kolumny dziesiętnej domyślnie.

Czasami te konwencje domyślne nie są idealnym rozwiązaniem dla modelu i należy je obejść przez skonfigurowanie wielu pojedynczych jednostek przy użyciu adnotacji danych lub interfejsu API Fluent. Niestandardowe Konwencje Code First umożliwiają definiowanie własnych Konwencji, które zapewniają wartości domyślne konfiguracji dla modelu. W tym instruktażu zapoznajemy różne typy niestandardowych Konwencji oraz sposób tworzenia każdego z nich.


## <a name="model-based-conventions"></a>Konwencje oparte na modelu

Ta strona obejmuje interfejs API DbModelBuilder dla Konwencji niestandardowych. Ten interfejs API powinien być wystarczający do tworzenia większości Konwencji niestandardowych. Istnieje również możliwość utworzenia Konwencji opartych na modelu, które manipulują ostatnim modelem po jego utworzeniu — do obsługi zaawansowanych scenariuszy. Aby uzyskać więcej informacji, zobacz [konwencje oparte na modelu](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Nasz model

Zacznijmy od zdefiniowania prostego modelu, który będzie używany z naszymi konwencjami. Dodaj następujące klasy do projektu.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Wprowadzenie do Konwencji niestandardowych

Napiszmy Konwencję, która konfiguruje dowolny klucz o nazwie jako klucz podstawowy dla tego typu jednostki.

Konwencje są włączane w konstruktorze modelu, do którego można uzyskać dostęp poprzez zastępowanie OnModelCreating w kontekście. Zaktualizuj klasę ProductContext w następujący sposób:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Teraz Każda właściwość w naszym modelu o nazwie Key zostanie skonfigurowana jako klucz podstawowy każdego podmiotu.

Możemy również bardziej szczegółowo wprowadzać konwencje przez filtrowanie według typu właściwości, którą zamierzamy skonfigurować:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Spowoduje to skonfigurowanie wszystkich właściwości o nazwie klucz jako klucz podstawowy swojej jednostki, ale tylko wtedy, gdy są one liczbami całkowitymi.

Ciekawą funkcją metody IsKey jest to, że jest to dodatek. Oznacza to, że jeśli wywołasz IsKey na wielu właściwościach i staną się one częścią klucza złożonego. Jednym z tych warunków jest to, że w przypadku określenia wielu właściwości klucza należy również określić zamówienie dla tych właściwości. Można to zrobić przez wywołanie metody HasColumnOrder podobnej do poniższego:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Ten kod skonfiguruje typy w naszym modelu w taki sposób, aby zawierał klucz złożony składający się z kolumny klucza int i kolumny Nazwa ciągu. Jeśli przeglądasz model w projektancie, będzie to wyglądać następująco:

![Klucz złożony](~/ef6/media/compositekey.png)

Innym przykładem Konwencji właściwości jest skonfigurowanie wszystkich właściwości DateTime w modelu do mapowania na typ datetime2 w SQL Server zamiast DateTime. Można to osiągnąć przy użyciu następujących czynności:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Klasy Konwencji

Innym sposobem definiowania Konwencji jest użycie klasy Konwencji do hermetyzacji Konwencji. Korzystając z klasy Konwencji, należy utworzyć typ, który dziedziczy z klasy Konwencji w przestrzeni nazw System. Data. Entity. ModelConfiguration. Conventions.

Możemy utworzyć klasę Konwencji z Konwencją datetime2, którą wykazałeś wcześniej, wykonując następujące czynności:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Aby poinformować Dr o konieczności użycia tej Konwencji, należy dodać ją do kolekcji Konwencji w OnModelCreating, co w przypadku, gdy użytkownik wykonał następujące czynności z przewodnikiem:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Jak widać, dodajemy wystąpienie naszej Konwencji do kolekcji Konwencji. Dziedziczenie z Konwencji zapewnia wygodny sposób grupowania i udostępniania Konwencji dla zespołów lub projektów. Można na przykład mieć bibliotekę klas ze wspólnym zestawem Konwencji używanym przez wszystkie projekty organizacji.

 

## <a name="custom-attributes"></a>Atrybuty niestandardowe

Innym doskonałym zastosowaniem Konwencji jest włączenie nowych atrybutów do użycia podczas konfigurowania modelu. Aby to zilustrować, Utwórzmy atrybut, którego możemy użyć do oznaczania właściwości ciągu jako innych niż Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Teraz Utwórzmy Konwencję, aby zastosować ten atrybut do naszego modelu:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

W tej konwencji można dodać atrybut niebędący znakiem Unicode do dowolnej właściwości ciągu, co oznacza, że kolumna w bazie danych będzie przechowywana jako varchar zamiast nvarchar.

Jedną z elementów, które należy zwrócić uwagę, jest to, że jeśli umieścisz atrybut niezgodny ze standardem Unicode dla elementu innego niż właściwość String, zostanie zgłoszony wyjątek. Jest to spowodowane tym, że nie można skonfigurować elementu isunicode dla dowolnego typu innego niż ciąg. W takim przypadku można uczynić swoją Konwencję bardziej szczegółowo, aby odfiltrować wszystkie elementy, które nie są ciągami.

Chociaż powyższa Konwencja dotyczy definiowania atrybutów niestandardowych, istnieje inny interfejs API, który może być dużo łatwiejszy do użycia, szczególnie w przypadku, gdy chcesz użyć właściwości z klasy Attribute.

Na potrzeby tego przykładu będziemy aktualizować nasz atrybut i zmieniać go na atrybut isunicode, więc wygląda następująco:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Po tym, możemy ustawić bool dla naszego atrybutu, aby określić, czy właściwość powinna być w formacie Unicode. Możemy to zrobić w ramach Konwencji, aby uzyskać dostęp do ClrPropertyowej klasy konfiguracji w następujący sposób:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Jest to bardzo proste, ale istnieje bardziej zwięzły sposób osiągnięcia tego przy użyciu metody interfejsu API Konwencji. Metoda HAVING ma parametr typu Func&lt;PropertyInfo, T&gt;, który akceptuje PropertyInfo tak samo jak Metoda WHERE, ale oczekiwano zwrócenia obiektu. Jeśli zwracany obiekt ma wartość null, właściwość nie zostanie skonfigurowana, co oznacza, że właściwości można odfiltrować w taki sam sposób, jak w przypadku, gdy jest to inna metoda, która również przechwytuje zwracany obiekt i przekazuje go do metody Configure. Działa to w następujący sposób:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Atrybuty niestandardowe nie są jedynym powodem korzystania z metody HAVING. jest to przydatne wszędzie tam, gdzie trzeba się dowiedzieć, na czym polega filtrowanie podczas konfigurowania typów lub właściwości.

 

## <a name="configuring-types"></a>Konfigurowanie typów

Dotychczas wszystkie nasze konwencje zostały przeznaczone dla właściwości, ale istnieje inny obszar interfejsu API Konwencji do konfigurowania typów w modelu. Środowisko jest podobne do obowiązujących Konwencji, ale opcje w obszarze Konfiguracja będą znajdować się w jednostkach, a nie na poziomie właściwości.

Jedną z elementów, które mogą być w rzeczywistości użyteczne w przypadku zmiany konwencji nazewnictwa tabel, w celu zamapowania na istniejący schemat, który różni się od wartości domyślnej EF lub utworzyć nową bazę danych z inną konwencją nazewnictwa. Aby to zrobić, najpierw musimy przyjąć metodę, która może akceptować elementelement dla typu w naszym modelu i zwracać informacje o nazwie tabeli dla tego typu:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Ta metoda przyjmuje typ i zwraca ciąg, który używa małych liter z podkreśleniami zamiast CamelCase. W naszym modelu oznacza to, że Klasa ProductCategory zostanie zmapowana do tabeli o nazwie Product\_Category zamiast ProductCategories.

Po zastosowaniu tej metody możemy ją wywoływać w Konwencji podobnej do tej:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Ta konwencja umożliwia skonfigurowanie każdego typu w naszym modelu do mapowania na nazwę tabeli zwracaną z naszej metody gettablename. Ta konwencja jest równoznaczna z wywołaniem metody ToTable dla każdej jednostki w modelu przy użyciu interfejsu API Fluent.

Jednym z nich jest to, że po wywołaniu ToTable EF przyjmuje ciąg, który podano jako dokładną nazwę tabeli, bez żadnego z pluralizacja, które normalnie zwykle podczas określania nazw tabel. Z tego względu nazwa tabeli z naszej Konwencji to produkt\_kategorii, a nie kategorii\_produktu. Możemy rozwiązać ten problem w naszej Konwencji, wykonując wywołanie do usługi pluralizacja Service wypróbujemy.

W poniższym kodzie zostanie użyta funkcja [rozpoznawania zależności](~/ef6/fundamentals/configuring/dependency-resolution.md) dodana w Ef6 do pobrania usługi pluralizacja, która mogłaby zostać użyta, i pluralize naszej nazwy tabeli.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> Ogólna wersja GetService jest metodą rozszerzającą w przestrzeni nazw System. Data. Entity. Infrastructure. DependencyResolution, dlatego należy dodać instrukcję using do kontekstu, aby można było jej używać.

### <a name="totable-and-inheritance"></a>ToTable i dziedziczenie

Innym ważnym aspektem ToTable jest to, że jeśli jawnie mapujesz typ do danej tabeli, możesz zmienić strategię mapowania, która będzie używana przez EF. Jeśli wywołasz ToTable dla każdego typu w hierarchii dziedziczenia, przekazanie nazwy typu jako nazwy tabeli jak wspomniano powyżej, zmienisz domyślną strategię mapowania tabeli na hierarchię (TPH) na typ tabeli (TPT). Najlepszym sposobem opisywania tej metody jest whith konkretnego przykładu:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Domyślnie zarówno pracownik, jak i Menedżer są zamapowane na tę samą tabelę (pracownicy) w bazie danych. Tabela będzie zawierać zarówno pracowników, jak i menedżerów z kolumną rozróżniacza, która informuje, jakiego typu wystąpienie jest przechowywane w każdym wierszu. Jest to mapowanie TPH, ponieważ istnieje jedna tabela dla hierarchii. Jeśli jednak wywołasz ToTable na obu Classe, wówczas każdy typ będzie mapowany do własnej tabeli, znanej również jako TPT, ponieważ każdy typ ma własną tabelę.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Powyższy kod zostanie zmapowany na strukturę tabeli, która wygląda następująco:

![Przykład TPT](~/ef6/media/tptexample.jpg)

Można to uniknąć i zachować domyślne mapowanie TPH na kilka sposobów:

1.  Wywołaj ToTable z tą samą nazwą tabeli dla każdego typu w hierarchii.
2.  Wywołaj ToTable tylko w klasie podstawowej hierarchii, w naszym przykładzie, który byłby pracownikiem.

 

## <a name="execution-order"></a>Kolejność wykonywania

Konwencje działają w ostatnim systemie WINS, tak samo jak w przypadku interfejsu API Fluent. Oznacza to, że jeśli piszesz dwie konwencje, które skonfigurują tę samą opcję tej samej właściwości, to ostatni z nich do wykonania usługi WINS. Przykładowo w kodzie poniżej maksymalnej długości wszystkich ciągów jest ustawiona na 500, ale następnie skonfigurujemy wszystkie właściwości o nazwie name w modelu, aby mieć maksymalną długość 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Ze względu na to, że Konwencja ustawiająca maksymalną długość na 250 jest późniejsza niż ta, która ustawia wszystkie ciągi na 500, wszystkie właściwości o nazwie name w naszym modelu będą mieć wartość MaxLength 250, natomiast wszystkie inne ciągi, takie jak opisy, byłyby 500. Stosowanie Konwencji w ten sposób oznacza, że można dostarczyć ogólną Konwencję dla typów lub właściwości w modelu, a następnie overide je dla podzestawów, które różnią się.

Interfejs API Fluent i adnotacje danych mogą również służyć do przesłania Konwencji w określonych przypadkach. W naszym przykładzie powyżej, jeśli użyto interfejsu API Fluent do ustawienia maksymalnej długości właściwości, możemy ją umieścić przed lub po Konwencji, ponieważ bardziej szczegółowy interfejs API Fluent będzie omawiać bardziej ogólną Konwencję konfiguracyjną.

 

## <a name="built-in-conventions"></a>Konwencje wbudowane

Ponieważ konwencje niestandardowe mogą mieć wpływ domyślne konwencje Code First, może być przydatne do dodawania Konwencji do uruchamiania przed lub po innej konwencji. W tym celu można użyć metod addbefore i addafter w ramach kolekcji Konwencji w pochodnym kontekście DbContext. Poniższy kod dodaje utworzoną wcześniej klasę Konwencji tak, aby była uruchamiana przed wbudowaną Konwencją odnajdowania kluczy.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Jest to najbardziej używany podczas dodawania Konwencji, które muszą zostać uruchomione przed lub po wbudowaną konwencje, Lista wbudowanych Konwencji można znaleźć tutaj: [System. Data. Entity. ModelConfiguration. Conventions](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).

Można również usunąć konwencje, które nie mają być stosowane do modelu. Aby usunąć Konwencję, użyj metody Remove. Oto przykład usunięcia PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```

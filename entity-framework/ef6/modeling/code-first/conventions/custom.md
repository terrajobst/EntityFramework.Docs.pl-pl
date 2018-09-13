---
title: Niestandardowy kod Konwencji pierwsze - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489847"
---
# <a name="custom-code-first-conventions"></a>Konwencje pierwszy kod niestandardowy
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Przy użyciu najpierw kod modelu jest obliczana z klas przy użyciu zestawu Konwencji. Wartość domyślna [pierwszy konwencje związane z](~/ef6/modeling/code-first/conventions/built-in.md) określić elementy, takie jak, których właściwość staje się klucz podstawowy jednostki, nazwa tabeli mapuje jednostki i jakie dokładności i skali dziesiętna kolumna ma domyślnie.

Czasami te domyślnych Konwencji nie są idealne dla modelu, a trzeba pracować wokół nich przez skonfigurowanie wielu pojedynczych jednostek przy użyciu adnotacji danych lub interfejsu API Fluent. Niestandardowe pierwszy konwencje związane z umożliwiają definiowanie własnych Konwencji odpowiadającym, które zapewniają domyślne wartości dla modelu. W tym przewodniku omówimy różnego rodzaju konwencje niestandardowych oraz sposób tworzenia każdego z nich.


## <a name="model-based-conventions"></a>Konwencje opartych na modelu

Ta strona obejmuje interfejs API DbModelBuilder konwencje niestandardowych. Ten interfejs API powinny być wystarczające do tworzenia większość konwencje niestandardowych. Jednak istnieje także możliwość tworzenia opartych na modelu Konwencji — konwencje, które manipulują końcowego modelu, po jego utworzeniu — Obsługa zaawansowanych scenariuszy. Aby uzyskać więcej informacji, zobacz [opartych na modelu Konwencji](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Nasz Model

Zacznijmy od definiowania prosty model, który możemy użyć za pomocą naszych Konwencji. Dodaj następujące klasy do projektu.

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

 

## <a name="introducing-custom-conventions"></a>Wprowadzenie do niestandardowych Konwencji

Napiszmy Konwencji, który konfiguruje żadnej właściwości o nazwie klucza jako klucza podstawowego dla tego typu jednostki.

Konwencje są włączone w Konstruktorze modelu, którego dostęp można uzyskać poprzez zastąpienie OnModelCreating w kontekście. Aktualizacja klasy ProductContext w następujący sposób:

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

Teraz, będą wszystkich właściwości w naszym modelu o nazwie klucza skonfigurowany jako klucz podstawowy jednostki, niezależnie od jej części.

Również zapytała Konwencji naszego dokładniejszą przez filtrowanie według typu właściwości, który będziemy do skonfigurowania:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

To spowoduje skonfigurowanie wszystkich właściwości o nazwie klucza jako podstawowego klucza ich jednostki, ale tylko wtedy, gdy są one liczbą całkowitą.

Funkcją interesujące metody IsKey jest dodatek. Co oznacza, że jeśli wywołujesz IsKey na wiele właściwości, a wszystkie staną się częścią klucza złożonego. Jedno zastrzeżenie: to jest, czy podczas określania wielu właściwości klucza należy także określić, zamówienie tych właściwości. Można to zrobić, wywołując HasColumnOrder metody, takie jak poniżej:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Ten kod konfiguruje typy w naszym modelu ma klucz złożony składający się z nazwy kolumny klucza int i ciąg. Jeśli firma Microsoft umożliwia wyświetlenie modelu w Projektancie wyglądała następująco:

![Klucz złożony](~/ef6/media/compositekey.png)

Inny przykład Konwencji właściwość to skonfigurować wszystkie właściwości daty/godziny w swój model do mapowania typu datetime2 w programie SQL Server zamiast daty/godziny. Można to osiągnąć przy użyciu następujących czynności:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Klasy Konwencji

Innym sposobem definiowania Konwencji jest użycie klasy Konwencji do hermetyzacji z Konwencji. Korzystając z klasy Konwencji, a następnie utworzyć typ, który dziedziczy z klasy Konwencji w przestrzeni nazw System.Data.Entity.ModelConfiguration.Conventions.

Możemy utworzyć klasę Konwencji z Konwencją datetime2, który wcześniej pokazaliśmy, wykonując następujące czynności:

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

Aby poinformować EF, użyj tej Konwencji, dodaj go do kolekcji konwencje w OnModelCreating, który jeśli wykonywano wraz z tym przewodnikiem będzie wyglądać następująco:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Jak widać, że wystąpienie Konwencji naszego możemy dodać do kolekcji Konwencji. Dziedziczenie z Konwencji oferuje wygodny sposób grupowania i udostępnianie konwencje przez zespoły lub projekty. Na przykład można mieć biblioteki klas w języku wspólny zbiór konwencji, że projekty wszystkie Twojej organizacji użyj.

 

## <a name="custom-attributes"></a>Atrybuty niestandardowe

Innym zastosowaniem doskonałe Konwencji jest aby włączyć nowe atrybuty, które będą używane podczas konfigurowania modelu. Na przykład Utwórz atrybut, który możemy użyć, aby oznaczyć właściwości ciągu jako innego niż Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Teraz Utwórzmy Konwencji zastosowaniu tego atrybutu w naszym modelu:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Z niniejszej Konwencji firma Microsoft można dodać atrybutu NonUnicode do żadnego z naszych właściwości ciągów, które oznacza, że kolumna w bazie danych będą przechowywane jako varchar zamiast nvarchar.

Jedną z rzeczy uwag dotyczących niniejszej Konwencji jest to, że jeśli atrybut NonUnicode zostanie umieszczony na coś innego niż właściwość ciągu, a następnie go spowoduje zgłoszenie wyjątku. Dzieje się tak, ponieważ nie można skonfigurować IsUnicode na dowolnego typu innego niż ciąg. Jeśli tak się stanie, następnie można wprowadzić swoje Konwencji bardziej szczegółowe tak, aby go odfiltrowuje wszystkie elementy, które nie jest ciąg.

Chociaż powyżej Konwencji działa w przypadku definiowania atrybutów niestandardowych, które ma innego interfejsu API, które mogą być znacznie łatwiejsze do użycia, szczególnie gdy zachodzi potrzeba użycia właściwości z klasy atrybutów.

W tym przykładzie użyjemy aktualizacja naszych atrybutu i zmień ją na atrybut IsUnicode, więc wygląda następująco:

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

Gdy będziemy już mieć, firma Microsoft można ustawić typu wartość logiczna na naszych atrybut Konwencji stwierdzić, czy właściwość powinna być Unicode. Firma Microsoft może zrobić w Konwencji, które mamy już uzyskując ClrProperty klasy konfiguracji następująco:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Jest to dość proste, ale jest bardziej zwięzły sposób realizacji tego celu za pomocą Having metoda konwencje interfejsu API. Having metoda ma parametr typu Func&lt;PropertyInfo, T&gt; który akceptuje PropertyInfo taka sama jak Where metody, ale oczekuje się, aby zwrócić obiekt. Jeśli zwracany obiekt ma wartość null, a następnie właściwość nie zostanie skonfigurowany, co oznacza można odfiltrować właściwości z nią tak samo jak miejsca, ale różni się w tym będzie również przechwytywania zwróconego obiektu i przekazać go do metody konfiguracji. Działa to podobnie do poniższych:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Atrybuty niestandardowe nie są Jedyny przypadek, kiedy używać Having metody przydaje się dowolnego miejsca, wymagających przeglądanie informacji o coś, co możesz filtrowania podczas konfigurowania właściwości lub typów.

 

## <a name="configuring-types"></a>Konfigurowanie typów

Do tej pory wszystkie nasze konwencje zostały dla właściwości, ale istnieje inny obszar konwencje interfejsu API na temat konfigurowania typów w modelu. Proces jest podobny do Konwencji, które zauważono pory, ale opcje konfigurowania wewnątrz będzie znajdować się w jednostce zamiast właściwości poziomu.

Jedną z rzeczy, które konwencje poziomu typu mogą być bardzo przydatne podczas ulegnie zmianie tabeli konwencji nazewnictwa, aby mapować do istniejącego schematu, która różni się od domyślnej EF lub Utwórz nową bazę danych z różnych konwencji nazewnictwa. W tym celu najpierw należy metodę, która może zaakceptować TypeInfo dla typu w naszym modelu i zwraca nazwę tabeli dla tego typu, co należy:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Ta metoda przyjmuje typ i zwraca ciąg, który używa małą znakami podkreślenia zamiast CamelCase. W naszym modelu oznacza to, że klasa ProductCategory zostaną zmapowane do tabeli o nazwie produktu\_kategorii zamiast oddzielały kategorie Productcategory.

Gdy będziemy już mieć tej metody można nazywamy je w Konwencji następująco:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Ta konwencja konfiguruje każdy typ w naszym modelu do mapowania nazwy tabeli, który jest zwracany z metody naszych GetTableName. Ta konwencja jest odpowiednikiem wywołania metody ToTable dla każdej jednostki w modelu używając interfejsu API Fluent.

Jedno należy zwrócić uwagę na to jest, że po wywołaniu ToTable EF potrwa ciąg, który jest udostępniany jako nazwę tabeli dokładnie, bez jakichkolwiek pluralizacja, który normalnie jak podczas określania nazwy tabeli. To dlatego nazwa tabeli z Konwencji naszego produktu\_kategorii zamiast produktu\_kategorii. Możemy rozwiązać, w Konwencji naszego poprzez wywołanie usługi pluralizacja określić główną przyczynę.

W poniższym kodzie użyto [rozpoznawania zależności](~/ef6/fundamentals/configuring/dependency-resolution.md) funkcja, dodany do programu EF6 można pobrać usługi pluralizacja, który był używany EF i naszych Nazwa tabeli w liczbie mnogiej.

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
> Ogólny wersję GetService jest metodą rozszerzenia w przestrzeni nazw System.Data.Entity.Infrastructure.DependencyResolution, musisz dodać za pomocą instrukcji do kontekstu w taki sposób, aby można było go używać.

### <a name="totable-and-inheritance"></a>ToTable i dziedziczenie

Innym ważnym aspektem ToTable jest to, że jeśli jawnie mapujesz typ danej tabeli, a następnie można zmienić strategię mapowania, która platforma EF użyje. Jeśli wywołasz ToTable dla każdego typu w hierarchii dziedziczenia, przekazując nazwę typu jako nazwę tabeli, tak jak opisano powyżej, następnie zostanie zmieniony strategii mapowania Tabela wg hierarchii (TPH) domyślny Tabela wg typu (TPT). Najlepszym sposobem, aby opisać to jest whith konkretny przykład:

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

Domyślnie pracownika i Menedżera są mapowane na tej samej tabeli (pracownicy) w bazie danych. Tabela będzie zawierać zarówno pracowników i menedżerów, a kolumna dyskryminatora, który poinformuje, jakiego typu wystąpienia są przechowywane w każdym wierszu. Jest to mapowanie TPH, ponieważ ma jedną tabelę dla hierarchii. Jednak jeśli wywołasz ToTable na obu classe następnie każdego typu będzie zamiast tego można zamapować na własną tabelę, znany także jako TPT, ponieważ każdy typ ma własną tabelę.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Powyższy kod będzie zmapowana do struktury tabeli, która wygląda podobnie do poniższego:

![Przykład Tpt](~/ef6/media/tptexample.jpg)

Można tego uniknąć, a obsługa TPH domyślnego mapowania na kilka sposobów:

1.  Wywołaj ToTable z taką samą nazwę tabeli, dla każdego typu w hierarchii.
2.  Wywołaj ToTable tylko w klasie bazowej, hierarchii, w tym przykładzie, która byłaby pracownika.

 

## <a name="execution-order"></a>Kolejność wykonywania

Konwencje działają w sposób ostatniego wins, taki sam jak interfejs Fluent API. Oznacza to, że jeśli piszesz dwóch Konwencjach skonfigurowanych na tej samej opcji tej właściwości, a następnie ostatni z nich do wykonania usługi wins. Na przykład w poniższym kodzie maksymalna długość wszystkich ciągów ma wartość 500, ale możemy następnie skonfiguruj wszystkie właściwości w modelu, który ma mieć maksymalną długość równą 250 o nazwie Name.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Ponieważ Konwencji, aby ustawić maksymalną długość do 250 po ten, który ustawia wszystkie ciągi na 500, wszystkie właściwości o nazwie Name w naszym modelu będą mieć MaxLength 250 podczas innych ciągów, takich jak opisy, będzie wynosić 500. Za pomocą Konwencji w ten sposób oznacza, że może zapewnić Konwencję ogólnych dla typów lub właściwości w modelu i następnie zastąpić te je dla podzbiorów, które różnią się.

Fluent API i adnotacje danych może również zastąpić Konwencji w szczególnych przypadkach. W naszym powyższym przykładzie Jeśli firma Microsoft gdyby użyto Fluent API, aby ustawić maksymalną długość właściwości następnie może mieć testujemy go przed lub po Konwencji, ponieważ dokładniejszą Fluent API wygra nad bardziej ogólnych Konwencji konfiguracji.

 

## <a name="built-in-conventions"></a>Konwencje wbudowane

Ponieważ konwencje niestandardowe mogą mieć wpływ domyślnych Konwencji Code First, może być przydatne do dodania konwencje do uruchomienia przed lub po innym Konwencji. W tym celu można użyć AddBefore i AddAfter metod zbierania konwencje na Twoje pochodnego typu DbContext. Poniższy kod zwiększałoby klasy Konwencji utworzony wcześniej tak, aby było uruchamiane przed wbudowanej w Konwencji klucza odnajdywania.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Ma to być najbardziej przydatne podczas dodawania konwencje, które mają zostać uruchomione przed lub po wbudowanej Konwencji, lista wbudowanej konwencje można znaleźć tutaj: [Namespace System.Data.Entity.ModelConfiguration.Conventions](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Można również usunąć konwencje, które mają być stosowane do modelu. Aby usunąć z Konwencją, należy użyć metody Remove. Oto przykład usuwania PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```

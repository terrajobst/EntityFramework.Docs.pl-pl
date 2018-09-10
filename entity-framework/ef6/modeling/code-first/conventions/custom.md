---
title: Niestandardowy kod Konwencji pierwsze - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: a0e8080037cf86640275f498ed159c847ff5c057
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251066"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="cebf0-102">Konwencje pierwszy kod niestandardowy</span><span class="sxs-lookup"><span data-stu-id="cebf0-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="cebf0-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="cebf0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="cebf0-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="cebf0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="cebf0-105">Przy użyciu najpierw kod modelu jest obliczana z klas przy użyciu zestawu Konwencji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="cebf0-106">Wartość domyślna [pierwszy konwencje związane z](~/ef6/modeling/code-first/conventions/built-in.md) określić elementy, takie jak, których właściwość staje się klucz podstawowy jednostki, nazwa tabeli mapuje jednostki i jakie dokładności i skali dziesiętna kolumna ma domyślnie.</span><span class="sxs-lookup"><span data-stu-id="cebf0-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="cebf0-107">Czasami te domyślnych Konwencji nie są idealne dla modelu, a trzeba pracować wokół nich przez skonfigurowanie wielu pojedynczych jednostek przy użyciu adnotacji danych lub interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="cebf0-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="cebf0-108">Niestandardowe pierwszy konwencje związane z umożliwiają definiowanie własnych Konwencji odpowiadającym, które zapewniają domyślne wartości dla modelu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="cebf0-109">W tym przewodniku omówimy różnego rodzaju konwencje niestandardowych oraz sposób tworzenia każdego z nich.</span><span class="sxs-lookup"><span data-stu-id="cebf0-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="cebf0-110">Konwencje opartych na modelu</span><span class="sxs-lookup"><span data-stu-id="cebf0-110">Model-Based Conventions</span></span>

<span data-ttu-id="cebf0-111">Ta strona obejmuje interfejs API DbModelBuilder konwencje niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="cebf0-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="cebf0-112">Ten interfejs API powinny być wystarczające do tworzenia większość konwencje niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="cebf0-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="cebf0-113">Jednak istnieje także możliwość tworzenia opartych na modelu Konwencji — konwencje, które manipulują końcowego modelu, po jego utworzeniu — Obsługa zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="cebf0-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="cebf0-114">Aby uzyskać więcej informacji, zobacz [opartych na modelu Konwencji](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="cebf0-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="cebf0-115">Nasz Model</span><span class="sxs-lookup"><span data-stu-id="cebf0-115">Our Model</span></span>

<span data-ttu-id="cebf0-116">Zacznijmy od definiowania prosty model, który możemy użyć za pomocą naszych Konwencji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="cebf0-117">Dodaj następujące klasy do projektu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-117">Add the following classes to your project.</span></span>

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

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="cebf0-118">Wprowadzenie do niestandardowych Konwencji</span><span class="sxs-lookup"><span data-stu-id="cebf0-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="cebf0-119">Napiszmy Konwencji, który konfiguruje żadnej właściwości o nazwie klucza jako klucza podstawowego dla tego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="cebf0-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="cebf0-120">Konwencje są włączone w Konstruktorze modelu, którego dostęp można uzyskać poprzez zastąpienie OnModelCreating w kontekście.</span><span class="sxs-lookup"><span data-stu-id="cebf0-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="cebf0-121">Aktualizacja klasy ProductContext w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cebf0-121">Update the ProductContext class as follows:</span></span>

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

<span data-ttu-id="cebf0-122">Teraz, będą wszystkich właściwości w naszym modelu o nazwie klucza skonfigurowany jako klucz podstawowy jednostki, niezależnie od jej części.</span><span class="sxs-lookup"><span data-stu-id="cebf0-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="cebf0-123">Również zapytała Konwencji naszego dokładniejszą przez filtrowanie według typu właściwości, który będziemy do skonfigurowania:</span><span class="sxs-lookup"><span data-stu-id="cebf0-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="cebf0-124">To spowoduje skonfigurowanie wszystkich właściwości o nazwie klucza jako podstawowego klucza ich jednostki, ale tylko wtedy, gdy są one liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="cebf0-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="cebf0-125">Funkcją interesujące metody IsKey jest dodatek.</span><span class="sxs-lookup"><span data-stu-id="cebf0-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="cebf0-126">Co oznacza, że jeśli wywołujesz IsKey na wiele właściwości, a wszystkie staną się częścią klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="cebf0-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="cebf0-127">Jedno zastrzeżenie: to jest, czy podczas określania wielu właściwości klucza należy także określić, zamówienie tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="cebf0-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="cebf0-128">Można to zrobić, wywołując HasColumnOrder metody, takie jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="cebf0-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="cebf0-129">Ten kod konfiguruje typy w naszym modelu ma klucz złożony składający się z nazwy kolumny klucza int i ciąg.</span><span class="sxs-lookup"><span data-stu-id="cebf0-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="cebf0-130">Jeśli firma Microsoft umożliwia wyświetlenie modelu w Projektancie wyglądała następująco:</span><span class="sxs-lookup"><span data-stu-id="cebf0-130">If we view the model in the designer it would look like this:</span></span>

![Klucz złożony](~/ef6/media/compositekey.png)

<span data-ttu-id="cebf0-132">Inny przykład Konwencji właściwość to skonfigurować wszystkie właściwości daty/godziny w swój model do mapowania typu datetime2 w programie SQL Server zamiast daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="cebf0-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="cebf0-133">Można to osiągnąć przy użyciu następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="cebf0-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="cebf0-134">Klasy Konwencji</span><span class="sxs-lookup"><span data-stu-id="cebf0-134">Convention Classes</span></span>

<span data-ttu-id="cebf0-135">Innym sposobem definiowania Konwencji jest użycie klasy Konwencji do hermetyzacji z Konwencji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="cebf0-136">Korzystając z klasy Konwencji, a następnie utworzyć typ, który dziedziczy z klasy Konwencji w przestrzeni nazw System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="cebf0-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="cebf0-137">Możemy utworzyć klasę Konwencji z Konwencją datetime2, który wcześniej pokazaliśmy, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="cebf0-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

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

<span data-ttu-id="cebf0-138">Aby poinformować EF, użyj tej Konwencji, dodaj go do kolekcji konwencje w OnModelCreating, który jeśli wykonywano wraz z tym przewodnikiem będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="cebf0-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="cebf0-139">Jak widać, że wystąpienie Konwencji naszego możemy dodać do kolekcji Konwencji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="cebf0-140">Dziedziczenie z Konwencji oferuje wygodny sposób grupowania i udostępnianie konwencje przez zespoły lub projekty.</span><span class="sxs-lookup"><span data-stu-id="cebf0-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="cebf0-141">Na przykład można mieć biblioteki klas w języku wspólny zbiór konwencji, że projekty wszystkie Twojej organizacji użyj.</span><span class="sxs-lookup"><span data-stu-id="cebf0-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="cebf0-142">Atrybuty niestandardowe</span><span class="sxs-lookup"><span data-stu-id="cebf0-142">Custom Attributes</span></span>

<span data-ttu-id="cebf0-143">Innym zastosowaniem doskonałe Konwencji jest aby włączyć nowe atrybuty, które będą używane podczas konfigurowania modelu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="cebf0-144">Na przykład Utwórz atrybut, który możemy użyć, aby oznaczyć właściwości ciągu jako innego niż Unicode.</span><span class="sxs-lookup"><span data-stu-id="cebf0-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="cebf0-145">Teraz Utwórzmy Konwencji zastosowaniu tego atrybutu w naszym modelu:</span><span class="sxs-lookup"><span data-stu-id="cebf0-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="cebf0-146">Z niniejszej Konwencji firma Microsoft można dodać atrybutu NonUnicode do żadnego z naszych właściwości ciągów, które oznacza, że kolumna w bazie danych będą przechowywane jako varchar zamiast nvarchar.</span><span class="sxs-lookup"><span data-stu-id="cebf0-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="cebf0-147">Jedną z rzeczy uwag dotyczących niniejszej Konwencji jest to, że jeśli atrybut NonUnicode zostanie umieszczony na coś innego niż właściwość ciągu, a następnie go spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="cebf0-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="cebf0-148">Dzieje się tak, ponieważ nie można skonfigurować IsUnicode na dowolnego typu innego niż ciąg.</span><span class="sxs-lookup"><span data-stu-id="cebf0-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="cebf0-149">Jeśli tak się stanie, następnie można wprowadzić swoje Konwencji bardziej szczegółowe tak, aby go odfiltrowuje wszystkie elementy, które nie jest ciąg.</span><span class="sxs-lookup"><span data-stu-id="cebf0-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="cebf0-150">Chociaż powyżej Konwencji działa w przypadku definiowania atrybutów niestandardowych, które ma innego interfejsu API, które mogą być znacznie łatwiejsze do użycia, szczególnie gdy zachodzi potrzeba użycia właściwości z klasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cebf0-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="cebf0-151">W tym przykładzie użyjemy aktualizacja naszych atrybutu i zmień ją na atrybut IsUnicode, więc wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="cebf0-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

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

<span data-ttu-id="cebf0-152">Gdy będziemy już mieć, firma Microsoft można ustawić typu wartość logiczna na naszych atrybut Konwencji stwierdzić, czy właściwość powinna być Unicode.</span><span class="sxs-lookup"><span data-stu-id="cebf0-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="cebf0-153">Firma Microsoft może zrobić w Konwencji, które mamy już uzyskując ClrProperty klasy konfiguracji następująco:</span><span class="sxs-lookup"><span data-stu-id="cebf0-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="cebf0-154">Jest to dość proste, ale jest bardziej zwięzły sposób realizacji tego celu za pomocą Having metoda konwencje interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cebf0-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="cebf0-155">Having metoda ma parametr typu Func&lt;PropertyInfo, T&gt; który akceptuje PropertyInfo taka sama jak Where metody, ale oczekuje się, aby zwrócić obiekt.</span><span class="sxs-lookup"><span data-stu-id="cebf0-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="cebf0-156">Jeśli zwracany obiekt ma wartość null, a następnie właściwość nie zostanie skonfigurowany, co oznacza można odfiltrować właściwości z nią tak samo jak miejsca, ale różni się w tym będzie również przechwytywania zwróconego obiektu i przekazać go do metody konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="cebf0-157">Działa to podobnie do poniższych:</span><span class="sxs-lookup"><span data-stu-id="cebf0-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="cebf0-158">Atrybuty niestandardowe nie są Jedyny przypadek, kiedy używać Having metody przydaje się dowolnego miejsca, wymagających przeglądanie informacji o coś, co możesz filtrowania podczas konfigurowania właściwości lub typów.</span><span class="sxs-lookup"><span data-stu-id="cebf0-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="cebf0-159">Konfigurowanie typów</span><span class="sxs-lookup"><span data-stu-id="cebf0-159">Configuring Types</span></span>

<span data-ttu-id="cebf0-160">Do tej pory wszystkie nasze konwencje zostały dla właściwości, ale istnieje inny obszar konwencje interfejsu API na temat konfigurowania typów w modelu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="cebf0-161">Proces jest podobny do Konwencji, które zauważono pory, ale opcje konfigurowania wewnątrz będzie znajdować się w jednostce zamiast właściwości poziomu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="cebf0-162">Jedną z rzeczy, które konwencje poziomu typu mogą być bardzo przydatne podczas ulegnie zmianie tabeli konwencji nazewnictwa, aby mapować do istniejącego schematu, która różni się od domyślnej EF lub Utwórz nową bazę danych z różnych konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="cebf0-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="cebf0-163">W tym celu najpierw należy metodę, która może zaakceptować TypeInfo dla typu w naszym modelu i zwraca nazwę tabeli dla tego typu, co należy:</span><span class="sxs-lookup"><span data-stu-id="cebf0-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="cebf0-164">Ta metoda przyjmuje typ i zwraca ciąg, który używa małą znakami podkreślenia zamiast CamelCase.</span><span class="sxs-lookup"><span data-stu-id="cebf0-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="cebf0-165">W naszym modelu oznacza to, że klasa ProductCategory zostaną zmapowane do tabeli o nazwie produktu\_kategorii zamiast oddzielały kategorie Productcategory.</span><span class="sxs-lookup"><span data-stu-id="cebf0-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="cebf0-166">Gdy będziemy już mieć tej metody można nazywamy je w Konwencji następująco:</span><span class="sxs-lookup"><span data-stu-id="cebf0-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="cebf0-167">Ta konwencja konfiguruje każdy typ w naszym modelu do mapowania nazwy tabeli, który jest zwracany z metody naszych GetTableName.</span><span class="sxs-lookup"><span data-stu-id="cebf0-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="cebf0-168">Ta konwencja jest odpowiednikiem wywołania metody ToTable dla każdej jednostki w modelu używając interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="cebf0-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="cebf0-169">Jedno należy zwrócić uwagę na to jest, że po wywołaniu ToTable EF potrwa ciąg, który jest udostępniany jako nazwę tabeli dokładnie, bez jakichkolwiek pluralizacja, który normalnie jak podczas określania nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="cebf0-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="cebf0-170">To dlatego nazwa tabeli z Konwencji naszego produktu\_kategorii zamiast produktu\_kategorii.</span><span class="sxs-lookup"><span data-stu-id="cebf0-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="cebf0-171">Możemy rozwiązać, w Konwencji naszego poprzez wywołanie usługi pluralizacja określić główną przyczynę.</span><span class="sxs-lookup"><span data-stu-id="cebf0-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="cebf0-172">W poniższym kodzie użyto [rozpoznawania zależności](~/ef6/fundamentals/configuring/dependency-resolution.md) funkcja, dodany do programu EF6 można pobrać usługi pluralizacja, który był używany EF i naszych Nazwa tabeli w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="cebf0-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

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
> <span data-ttu-id="cebf0-173">Ogólny wersję GetService jest metodą rozszerzenia w przestrzeni nazw System.Data.Entity.Infrastructure.DependencyResolution, musisz dodać za pomocą instrukcji do kontekstu w taki sposób, aby można było go używać.</span><span class="sxs-lookup"><span data-stu-id="cebf0-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="cebf0-174">ToTable i dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="cebf0-174">ToTable and Inheritance</span></span>

<span data-ttu-id="cebf0-175">Innym ważnym aspektem ToTable jest to, że jeśli jawnie mapujesz typ danej tabeli, a następnie można zmienić strategię mapowania, która platforma EF użyje.</span><span class="sxs-lookup"><span data-stu-id="cebf0-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="cebf0-176">Jeśli wywołasz ToTable dla każdego typu w hierarchii dziedziczenia, przekazując nazwę typu jako nazwę tabeli, tak jak opisano powyżej, następnie zostanie zmieniony strategii mapowania Tabela wg hierarchii (TPH) domyślny Tabela wg typu (TPT).</span><span class="sxs-lookup"><span data-stu-id="cebf0-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="cebf0-177">Najlepszym sposobem, aby opisać to jest whith konkretny przykład:</span><span class="sxs-lookup"><span data-stu-id="cebf0-177">The best way to describe this is whith a concrete example:</span></span>

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

<span data-ttu-id="cebf0-178">Domyślnie pracownika i Menedżera są mapowane na tej samej tabeli (pracownicy) w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cebf0-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="cebf0-179">Tabela będzie zawierać zarówno pracowników i menedżerów, a kolumna dyskryminatora, który poinformuje, jakiego typu wystąpienia są przechowywane w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="cebf0-180">Jest to mapowanie TPH, ponieważ ma jedną tabelę dla hierarchii.</span><span class="sxs-lookup"><span data-stu-id="cebf0-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="cebf0-181">Jednak jeśli wywołasz ToTable na obu classe następnie każdego typu będzie zamiast tego można zamapować na własną tabelę, znany także jako TPT, ponieważ każdy typ ma własną tabelę.</span><span class="sxs-lookup"><span data-stu-id="cebf0-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="cebf0-182">Powyższy kod będzie zmapowana do struktury tabeli, która wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="cebf0-182">The code above will map to a table structure that looks like the following:</span></span>

![Przykład Tpt](~/ef6/media/tptexample.jpg)

<span data-ttu-id="cebf0-184">Można tego uniknąć, a obsługa TPH domyślnego mapowania na kilka sposobów:</span><span class="sxs-lookup"><span data-stu-id="cebf0-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="cebf0-185">Wywołaj ToTable z taką samą nazwę tabeli, dla każdego typu w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="cebf0-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="cebf0-186">Wywołaj ToTable tylko w klasie bazowej, hierarchii, w tym przykładzie, która byłaby pracownika.</span><span class="sxs-lookup"><span data-stu-id="cebf0-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="cebf0-187">Kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="cebf0-187">Execution Order</span></span>

<span data-ttu-id="cebf0-188">Konwencje działają w sposób ostatniego wins, taki sam jak interfejs Fluent API.</span><span class="sxs-lookup"><span data-stu-id="cebf0-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="cebf0-189">Oznacza to, że jeśli piszesz dwóch Konwencjach skonfigurowanych na tej samej opcji tej właściwości, a następnie ostatni z nich do wykonania usługi wins.</span><span class="sxs-lookup"><span data-stu-id="cebf0-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="cebf0-190">Na przykład w poniższym kodzie maksymalna długość wszystkich ciągów ma wartość 500, ale możemy następnie skonfiguruj wszystkie właściwości w modelu, który ma mieć maksymalną długość równą 250 o nazwie Name.</span><span class="sxs-lookup"><span data-stu-id="cebf0-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="cebf0-191">Ponieważ Konwencji, aby ustawić maksymalną długość do 250 po ten, który ustawia wszystkie ciągi na 500, wszystkie właściwości o nazwie Name w naszym modelu będą mieć MaxLength 250 podczas innych ciągów, takich jak opisy, będzie wynosić 500.</span><span class="sxs-lookup"><span data-stu-id="cebf0-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="cebf0-192">Za pomocą Konwencji w ten sposób oznacza, że może zapewnić Konwencję ogólnych dla typów lub właściwości w modelu i następnie zastąpić te je dla podzbiorów, które różnią się.</span><span class="sxs-lookup"><span data-stu-id="cebf0-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="cebf0-193">Fluent API i adnotacje danych może również zastąpić Konwencji w szczególnych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="cebf0-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="cebf0-194">W naszym powyższym przykładzie Jeśli firma Microsoft gdyby użyto Fluent API, aby ustawić maksymalną długość właściwości następnie może mieć testujemy go przed lub po Konwencji, ponieważ dokładniejszą Fluent API wygra nad bardziej ogólnych Konwencji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="cebf0-195">Konwencje wbudowane</span><span class="sxs-lookup"><span data-stu-id="cebf0-195">Built-in Conventions</span></span>

<span data-ttu-id="cebf0-196">Ponieważ konwencje niestandardowe mogą mieć wpływ domyślnych Konwencji Code First, może być przydatne do dodania konwencje do uruchomienia przed lub po innym Konwencji.</span><span class="sxs-lookup"><span data-stu-id="cebf0-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="cebf0-197">W tym celu można użyć AddBefore i AddAfter metod zbierania konwencje na Twoje pochodnego typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="cebf0-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="cebf0-198">Poniższy kod zwiększałoby klasy Konwencji utworzony wcześniej tak, aby było uruchamiane przed wbudowanej w Konwencji klucza odnajdywania.</span><span class="sxs-lookup"><span data-stu-id="cebf0-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="cebf0-199">Ma to być najbardziej przydatne podczas dodawania konwencje, które mają zostać uruchomione przed lub po wbudowanej Konwencji, lista wbudowanej konwencje można znaleźć tutaj: [Namespace System.Data.Entity.ModelConfiguration.Conventions](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="cebf0-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="cebf0-200">Można również usunąć konwencje, które mają być stosowane do modelu.</span><span class="sxs-lookup"><span data-stu-id="cebf0-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="cebf0-201">Aby usunąć z Konwencją, należy użyć metody Remove.</span><span class="sxs-lookup"><span data-stu-id="cebf0-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="cebf0-202">Oto przykład usuwania PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="cebf0-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```

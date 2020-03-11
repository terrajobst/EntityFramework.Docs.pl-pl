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
# <a name="custom-code-first-conventions"></a><span data-ttu-id="b050b-102">Niestandardowe Konwencje Code First</span><span class="sxs-lookup"><span data-stu-id="b050b-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="b050b-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b050b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b050b-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="b050b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b050b-105">W przypadku korzystania z Code First model jest obliczany na podstawie klas przy użyciu zestawu Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b050b-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="b050b-106">Domyślne [konwencje Code First](~/ef6/modeling/code-first/conventions/built-in.md) określają, jak, która właściwość jest kluczem podstawowym jednostki, nazwą tabeli, do której jest mapowany obiekt, i co określa precyzję i skalę kolumny dziesiętnej domyślnie.</span><span class="sxs-lookup"><span data-stu-id="b050b-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="b050b-107">Czasami te konwencje domyślne nie są idealnym rozwiązaniem dla modelu i należy je obejść przez skonfigurowanie wielu pojedynczych jednostek przy użyciu adnotacji danych lub interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b050b-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="b050b-108">Niestandardowe Konwencje Code First umożliwiają definiowanie własnych Konwencji, które zapewniają wartości domyślne konfiguracji dla modelu.</span><span class="sxs-lookup"><span data-stu-id="b050b-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="b050b-109">W tym instruktażu zapoznajemy różne typy niestandardowych Konwencji oraz sposób tworzenia każdego z nich.</span><span class="sxs-lookup"><span data-stu-id="b050b-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="b050b-110">Konwencje oparte na modelu</span><span class="sxs-lookup"><span data-stu-id="b050b-110">Model-Based Conventions</span></span>

<span data-ttu-id="b050b-111">Ta strona obejmuje interfejs API DbModelBuilder dla Konwencji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="b050b-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="b050b-112">Ten interfejs API powinien być wystarczający do tworzenia większości Konwencji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="b050b-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="b050b-113">Istnieje również możliwość utworzenia Konwencji opartych na modelu, które manipulują ostatnim modelem po jego utworzeniu — do obsługi zaawansowanych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="b050b-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="b050b-114">Aby uzyskać więcej informacji, zobacz [konwencje oparte na modelu](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="b050b-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="b050b-115">Nasz model</span><span class="sxs-lookup"><span data-stu-id="b050b-115">Our Model</span></span>

<span data-ttu-id="b050b-116">Zacznijmy od zdefiniowania prostego modelu, który będzie używany z naszymi konwencjami.</span><span class="sxs-lookup"><span data-stu-id="b050b-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="b050b-117">Dodaj następujące klasy do projektu.</span><span class="sxs-lookup"><span data-stu-id="b050b-117">Add the following classes to your project.</span></span>

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

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="b050b-118">Wprowadzenie do Konwencji niestandardowych</span><span class="sxs-lookup"><span data-stu-id="b050b-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="b050b-119">Napiszmy Konwencję, która konfiguruje dowolny klucz o nazwie jako klucz podstawowy dla tego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="b050b-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="b050b-120">Konwencje są włączane w konstruktorze modelu, do którego można uzyskać dostęp poprzez zastępowanie OnModelCreating w kontekście.</span><span class="sxs-lookup"><span data-stu-id="b050b-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="b050b-121">Zaktualizuj klasę ProductContext w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b050b-121">Update the ProductContext class as follows:</span></span>

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

<span data-ttu-id="b050b-122">Teraz Każda właściwość w naszym modelu o nazwie Key zostanie skonfigurowana jako klucz podstawowy każdego podmiotu.</span><span class="sxs-lookup"><span data-stu-id="b050b-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="b050b-123">Możemy również bardziej szczegółowo wprowadzać konwencje przez filtrowanie według typu właściwości, którą zamierzamy skonfigurować:</span><span class="sxs-lookup"><span data-stu-id="b050b-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="b050b-124">Spowoduje to skonfigurowanie wszystkich właściwości o nazwie klucz jako klucz podstawowy swojej jednostki, ale tylko wtedy, gdy są one liczbami całkowitymi.</span><span class="sxs-lookup"><span data-stu-id="b050b-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="b050b-125">Ciekawą funkcją metody IsKey jest to, że jest to dodatek.</span><span class="sxs-lookup"><span data-stu-id="b050b-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="b050b-126">Oznacza to, że jeśli wywołasz IsKey na wielu właściwościach i staną się one częścią klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="b050b-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="b050b-127">Jednym z tych warunków jest to, że w przypadku określenia wielu właściwości klucza należy również określić zamówienie dla tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="b050b-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="b050b-128">Można to zrobić przez wywołanie metody HasColumnOrder podobnej do poniższego:</span><span class="sxs-lookup"><span data-stu-id="b050b-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="b050b-129">Ten kod skonfiguruje typy w naszym modelu w taki sposób, aby zawierał klucz złożony składający się z kolumny klucza int i kolumny Nazwa ciągu.</span><span class="sxs-lookup"><span data-stu-id="b050b-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="b050b-130">Jeśli przeglądasz model w projektancie, będzie to wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b050b-130">If we view the model in the designer it would look like this:</span></span>

![Klucz złożony](~/ef6/media/compositekey.png)

<span data-ttu-id="b050b-132">Innym przykładem Konwencji właściwości jest skonfigurowanie wszystkich właściwości DateTime w modelu do mapowania na typ datetime2 w SQL Server zamiast DateTime.</span><span class="sxs-lookup"><span data-stu-id="b050b-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="b050b-133">Można to osiągnąć przy użyciu następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b050b-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="b050b-134">Klasy Konwencji</span><span class="sxs-lookup"><span data-stu-id="b050b-134">Convention Classes</span></span>

<span data-ttu-id="b050b-135">Innym sposobem definiowania Konwencji jest użycie klasy Konwencji do hermetyzacji Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b050b-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="b050b-136">Korzystając z klasy Konwencji, należy utworzyć typ, który dziedziczy z klasy Konwencji w przestrzeni nazw System. Data. Entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="b050b-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="b050b-137">Możemy utworzyć klasę Konwencji z Konwencją datetime2, którą wykazałeś wcześniej, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b050b-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

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

<span data-ttu-id="b050b-138">Aby poinformować Dr o konieczności użycia tej Konwencji, należy dodać ją do kolekcji Konwencji w OnModelCreating, co w przypadku, gdy użytkownik wykonał następujące czynności z przewodnikiem:</span><span class="sxs-lookup"><span data-stu-id="b050b-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="b050b-139">Jak widać, dodajemy wystąpienie naszej Konwencji do kolekcji Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b050b-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="b050b-140">Dziedziczenie z Konwencji zapewnia wygodny sposób grupowania i udostępniania Konwencji dla zespołów lub projektów.</span><span class="sxs-lookup"><span data-stu-id="b050b-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="b050b-141">Można na przykład mieć bibliotekę klas ze wspólnym zestawem Konwencji używanym przez wszystkie projekty organizacji.</span><span class="sxs-lookup"><span data-stu-id="b050b-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="b050b-142">Atrybuty niestandardowe</span><span class="sxs-lookup"><span data-stu-id="b050b-142">Custom Attributes</span></span>

<span data-ttu-id="b050b-143">Innym doskonałym zastosowaniem Konwencji jest włączenie nowych atrybutów do użycia podczas konfigurowania modelu.</span><span class="sxs-lookup"><span data-stu-id="b050b-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="b050b-144">Aby to zilustrować, Utwórzmy atrybut, którego możemy użyć do oznaczania właściwości ciągu jako innych niż Unicode.</span><span class="sxs-lookup"><span data-stu-id="b050b-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="b050b-145">Teraz Utwórzmy Konwencję, aby zastosować ten atrybut do naszego modelu:</span><span class="sxs-lookup"><span data-stu-id="b050b-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="b050b-146">W tej konwencji można dodać atrybut niebędący znakiem Unicode do dowolnej właściwości ciągu, co oznacza, że kolumna w bazie danych będzie przechowywana jako varchar zamiast nvarchar.</span><span class="sxs-lookup"><span data-stu-id="b050b-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="b050b-147">Jedną z elementów, które należy zwrócić uwagę, jest to, że jeśli umieścisz atrybut niezgodny ze standardem Unicode dla elementu innego niż właściwość String, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="b050b-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="b050b-148">Jest to spowodowane tym, że nie można skonfigurować elementu isunicode dla dowolnego typu innego niż ciąg.</span><span class="sxs-lookup"><span data-stu-id="b050b-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="b050b-149">W takim przypadku można uczynić swoją Konwencję bardziej szczegółowo, aby odfiltrować wszystkie elementy, które nie są ciągami.</span><span class="sxs-lookup"><span data-stu-id="b050b-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="b050b-150">Chociaż powyższa Konwencja dotyczy definiowania atrybutów niestandardowych, istnieje inny interfejs API, który może być dużo łatwiejszy do użycia, szczególnie w przypadku, gdy chcesz użyć właściwości z klasy Attribute.</span><span class="sxs-lookup"><span data-stu-id="b050b-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="b050b-151">Na potrzeby tego przykładu będziemy aktualizować nasz atrybut i zmieniać go na atrybut isunicode, więc wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="b050b-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

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

<span data-ttu-id="b050b-152">Po tym, możemy ustawić bool dla naszego atrybutu, aby określić, czy właściwość powinna być w formacie Unicode.</span><span class="sxs-lookup"><span data-stu-id="b050b-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="b050b-153">Możemy to zrobić w ramach Konwencji, aby uzyskać dostęp do ClrPropertyowej klasy konfiguracji w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b050b-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="b050b-154">Jest to bardzo proste, ale istnieje bardziej zwięzły sposób osiągnięcia tego przy użyciu metody interfejsu API Konwencji.</span><span class="sxs-lookup"><span data-stu-id="b050b-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="b050b-155">Metoda HAVING ma parametr typu Func&lt;PropertyInfo, T&gt;, który akceptuje PropertyInfo tak samo jak Metoda WHERE, ale oczekiwano zwrócenia obiektu.</span><span class="sxs-lookup"><span data-stu-id="b050b-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="b050b-156">Jeśli zwracany obiekt ma wartość null, właściwość nie zostanie skonfigurowana, co oznacza, że właściwości można odfiltrować w taki sam sposób, jak w przypadku, gdy jest to inna metoda, która również przechwytuje zwracany obiekt i przekazuje go do metody Configure.</span><span class="sxs-lookup"><span data-stu-id="b050b-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="b050b-157">Działa to w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b050b-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="b050b-158">Atrybuty niestandardowe nie są jedynym powodem korzystania z metody HAVING. jest to przydatne wszędzie tam, gdzie trzeba się dowiedzieć, na czym polega filtrowanie podczas konfigurowania typów lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="b050b-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="b050b-159">Konfigurowanie typów</span><span class="sxs-lookup"><span data-stu-id="b050b-159">Configuring Types</span></span>

<span data-ttu-id="b050b-160">Dotychczas wszystkie nasze konwencje zostały przeznaczone dla właściwości, ale istnieje inny obszar interfejsu API Konwencji do konfigurowania typów w modelu.</span><span class="sxs-lookup"><span data-stu-id="b050b-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="b050b-161">Środowisko jest podobne do obowiązujących Konwencji, ale opcje w obszarze Konfiguracja będą znajdować się w jednostkach, a nie na poziomie właściwości.</span><span class="sxs-lookup"><span data-stu-id="b050b-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="b050b-162">Jedną z elementów, które mogą być w rzeczywistości użyteczne w przypadku zmiany konwencji nazewnictwa tabel, w celu zamapowania na istniejący schemat, który różni się od wartości domyślnej EF lub utworzyć nową bazę danych z inną konwencją nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="b050b-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="b050b-163">Aby to zrobić, najpierw musimy przyjąć metodę, która może akceptować elementelement dla typu w naszym modelu i zwracać informacje o nazwie tabeli dla tego typu:</span><span class="sxs-lookup"><span data-stu-id="b050b-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="b050b-164">Ta metoda przyjmuje typ i zwraca ciąg, który używa małych liter z podkreśleniami zamiast CamelCase.</span><span class="sxs-lookup"><span data-stu-id="b050b-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="b050b-165">W naszym modelu oznacza to, że Klasa ProductCategory zostanie zmapowana do tabeli o nazwie Product\_Category zamiast ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="b050b-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="b050b-166">Po zastosowaniu tej metody możemy ją wywoływać w Konwencji podobnej do tej:</span><span class="sxs-lookup"><span data-stu-id="b050b-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="b050b-167">Ta konwencja umożliwia skonfigurowanie każdego typu w naszym modelu do mapowania na nazwę tabeli zwracaną z naszej metody gettablename.</span><span class="sxs-lookup"><span data-stu-id="b050b-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="b050b-168">Ta konwencja jest równoznaczna z wywołaniem metody ToTable dla każdej jednostki w modelu przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b050b-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="b050b-169">Jednym z nich jest to, że po wywołaniu ToTable EF przyjmuje ciąg, który podano jako dokładną nazwę tabeli, bez żadnego z pluralizacja, które normalnie zwykle podczas określania nazw tabel.</span><span class="sxs-lookup"><span data-stu-id="b050b-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="b050b-170">Z tego względu nazwa tabeli z naszej Konwencji to produkt\_kategorii, a nie kategorii\_produktu.</span><span class="sxs-lookup"><span data-stu-id="b050b-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="b050b-171">Możemy rozwiązać ten problem w naszej Konwencji, wykonując wywołanie do usługi pluralizacja Service wypróbujemy.</span><span class="sxs-lookup"><span data-stu-id="b050b-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="b050b-172">W poniższym kodzie zostanie użyta funkcja [rozpoznawania zależności](~/ef6/fundamentals/configuring/dependency-resolution.md) dodana w Ef6 do pobrania usługi pluralizacja, która mogłaby zostać użyta, i pluralize naszej nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="b050b-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

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
> <span data-ttu-id="b050b-173">Ogólna wersja GetService jest metodą rozszerzającą w przestrzeni nazw System. Data. Entity. Infrastructure. DependencyResolution, dlatego należy dodać instrukcję using do kontekstu, aby można było jej używać.</span><span class="sxs-lookup"><span data-stu-id="b050b-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="b050b-174">ToTable i dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="b050b-174">ToTable and Inheritance</span></span>

<span data-ttu-id="b050b-175">Innym ważnym aspektem ToTable jest to, że jeśli jawnie mapujesz typ do danej tabeli, możesz zmienić strategię mapowania, która będzie używana przez EF.</span><span class="sxs-lookup"><span data-stu-id="b050b-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="b050b-176">Jeśli wywołasz ToTable dla każdego typu w hierarchii dziedziczenia, przekazanie nazwy typu jako nazwy tabeli jak wspomniano powyżej, zmienisz domyślną strategię mapowania tabeli na hierarchię (TPH) na typ tabeli (TPT).</span><span class="sxs-lookup"><span data-stu-id="b050b-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="b050b-177">Najlepszym sposobem opisywania tej metody jest whith konkretnego przykładu:</span><span class="sxs-lookup"><span data-stu-id="b050b-177">The best way to describe this is whith a concrete example:</span></span>

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

<span data-ttu-id="b050b-178">Domyślnie zarówno pracownik, jak i Menedżer są zamapowane na tę samą tabelę (pracownicy) w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b050b-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="b050b-179">Tabela będzie zawierać zarówno pracowników, jak i menedżerów z kolumną rozróżniacza, która informuje, jakiego typu wystąpienie jest przechowywane w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="b050b-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="b050b-180">Jest to mapowanie TPH, ponieważ istnieje jedna tabela dla hierarchii.</span><span class="sxs-lookup"><span data-stu-id="b050b-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="b050b-181">Jeśli jednak wywołasz ToTable na obu Classe, wówczas każdy typ będzie mapowany do własnej tabeli, znanej również jako TPT, ponieważ każdy typ ma własną tabelę.</span><span class="sxs-lookup"><span data-stu-id="b050b-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="b050b-182">Powyższy kod zostanie zmapowany na strukturę tabeli, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="b050b-182">The code above will map to a table structure that looks like the following:</span></span>

![Przykład TPT](~/ef6/media/tptexample.jpg)

<span data-ttu-id="b050b-184">Można to uniknąć i zachować domyślne mapowanie TPH na kilka sposobów:</span><span class="sxs-lookup"><span data-stu-id="b050b-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="b050b-185">Wywołaj ToTable z tą samą nazwą tabeli dla każdego typu w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="b050b-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="b050b-186">Wywołaj ToTable tylko w klasie podstawowej hierarchii, w naszym przykładzie, który byłby pracownikiem.</span><span class="sxs-lookup"><span data-stu-id="b050b-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="b050b-187">Kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="b050b-187">Execution Order</span></span>

<span data-ttu-id="b050b-188">Konwencje działają w ostatnim systemie WINS, tak samo jak w przypadku interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b050b-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="b050b-189">Oznacza to, że jeśli piszesz dwie konwencje, które skonfigurują tę samą opcję tej samej właściwości, to ostatni z nich do wykonania usługi WINS.</span><span class="sxs-lookup"><span data-stu-id="b050b-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="b050b-190">Przykładowo w kodzie poniżej maksymalnej długości wszystkich ciągów jest ustawiona na 500, ale następnie skonfigurujemy wszystkie właściwości o nazwie name w modelu, aby mieć maksymalną długość 250.</span><span class="sxs-lookup"><span data-stu-id="b050b-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="b050b-191">Ze względu na to, że Konwencja ustawiająca maksymalną długość na 250 jest późniejsza niż ta, która ustawia wszystkie ciągi na 500, wszystkie właściwości o nazwie name w naszym modelu będą mieć wartość MaxLength 250, natomiast wszystkie inne ciągi, takie jak opisy, byłyby 500.</span><span class="sxs-lookup"><span data-stu-id="b050b-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="b050b-192">Stosowanie Konwencji w ten sposób oznacza, że można dostarczyć ogólną Konwencję dla typów lub właściwości w modelu, a następnie overide je dla podzestawów, które różnią się.</span><span class="sxs-lookup"><span data-stu-id="b050b-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="b050b-193">Interfejs API Fluent i adnotacje danych mogą również służyć do przesłania Konwencji w określonych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="b050b-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="b050b-194">W naszym przykładzie powyżej, jeśli użyto interfejsu API Fluent do ustawienia maksymalnej długości właściwości, możemy ją umieścić przed lub po Konwencji, ponieważ bardziej szczegółowy interfejs API Fluent będzie omawiać bardziej ogólną Konwencję konfiguracyjną.</span><span class="sxs-lookup"><span data-stu-id="b050b-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="b050b-195">Konwencje wbudowane</span><span class="sxs-lookup"><span data-stu-id="b050b-195">Built-in Conventions</span></span>

<span data-ttu-id="b050b-196">Ponieważ konwencje niestandardowe mogą mieć wpływ domyślne konwencje Code First, może być przydatne do dodawania Konwencji do uruchamiania przed lub po innej konwencji.</span><span class="sxs-lookup"><span data-stu-id="b050b-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="b050b-197">W tym celu można użyć metod addbefore i addafter w ramach kolekcji Konwencji w pochodnym kontekście DbContext.</span><span class="sxs-lookup"><span data-stu-id="b050b-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="b050b-198">Poniższy kod dodaje utworzoną wcześniej klasę Konwencji tak, aby była uruchamiana przed wbudowaną Konwencją odnajdowania kluczy.</span><span class="sxs-lookup"><span data-stu-id="b050b-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="b050b-199">Jest to najbardziej używany podczas dodawania Konwencji, które muszą zostać uruchomione przed lub po wbudowaną konwencje, Lista wbudowanych Konwencji można znaleźć tutaj: [System. Data. Entity. ModelConfiguration. Conventions](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="b050b-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="b050b-200">Można również usunąć konwencje, które nie mają być stosowane do modelu.</span><span class="sxs-lookup"><span data-stu-id="b050b-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="b050b-201">Aby usunąć Konwencję, użyj metody Remove.</span><span class="sxs-lookup"><span data-stu-id="b050b-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="b050b-202">Oto przykład usunięcia PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="b050b-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```

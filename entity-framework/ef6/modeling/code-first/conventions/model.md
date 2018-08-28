---
title: Na podstawie modelu Konwencji - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c1f416ba599445c07bd946714e9b6208b62f5951
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996950"
---
# <a name="model-based-conventions"></a><span data-ttu-id="1b9e0-102">Konwencje opartych na modelu</span><span class="sxs-lookup"><span data-stu-id="1b9e0-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="1b9e0-103">**EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="1b9e0-104">Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="1b9e0-105">Model oparty konwencje są zaawansowane metody oparte na Konwencji model konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="1b9e0-106">W przypadku większości scenariuszy [niestandardowego kodu pierwszego Konwencji interfejsu API na DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="1b9e0-107">Opis interfejsu API DbModelBuilder konwencje zaleca się przed rozpoczęciem korzystania z modelu na podstawie Konwencji.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="1b9e0-108">Konwencje na podstawie modelu pozwala na tworzenie konwencje, które wpływają na właściwości i tabel, które nie są konfigurowane za pomocą standardowych konwencji.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="1b9e0-109">Przykładem tego są dyskryminatora kolumn w tabeli na hierarchii modeli i skojarzenia niezależnie od kolumny.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="1b9e0-110">Tworzenie z Konwencją</span><span class="sxs-lookup"><span data-stu-id="1b9e0-110">Creating a Convention</span></span>   

<span data-ttu-id="1b9e0-111">Pierwszym krokiem w tworzeniu Konwencji model oparty jest wybór, gdy w potoku Konwencji musi zostać zastosowana do modelu.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="1b9e0-112">Istnieją dwa typy modelu Konwencji, Store (S-Space) i dotycząca pojęć (C-Space).</span><span class="sxs-lookup"><span data-stu-id="1b9e0-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="1b9e0-113">Konwencja C + spacja jest stosowany do modelu, który aplikacja zostanie skompilowana Konwencji S miejsca jest stosowane do wersji modelu, który reprezentuje bazy danych i elementy kontrolki, takie jak jak automatycznie generowanych kolumny mają nazwy.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="1b9e0-114">Konwencja modelu to klasa, która rozszerza IConceptualModelConvention lub IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="1b9e0-115">Te interfejsy, oba akceptują typ ogólny, który może być typu MetadataItem, która jest używana do filtrowania typu danych, który dotyczy Konwencji.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="1b9e0-116">Dodawanie Konwencję</span><span class="sxs-lookup"><span data-stu-id="1b9e0-116">Adding a Convention</span></span>   

<span data-ttu-id="1b9e0-117">Konwencje modelu zostaną dodane w taki sam sposób, jak regularne konwencje klasy.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="1b9e0-118">W **OnModelCreating** metody, Dodaj Konwencji do listy konwencje dla modelu.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="1b9e0-119">Można również dodać Konwencję względem innego Konwencja, za pomocą Conventions.AddBefore\< \> lub Conventions.AddAfter\< \> metody.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="1b9e0-120">Aby uzyskać więcej informacji dotyczących Konwencji, których dotyczy platformy Entity Framework, zobacz sekcję Uwagi.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="1b9e0-121">Przykład: Dyskryminatora modelu Konwencji</span><span class="sxs-lookup"><span data-stu-id="1b9e0-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="1b9e0-122">Zmienianie nazw kolumn, generowane przez EF jest przykładem coś, co nie można zrobić z innymi konwencjami interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="1b9e0-123">Jest to sytuacja w przypadku, gdy za pomocą modelu Konwencji jest jedynym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="1b9e0-124">Przykładowy sposób konfigurowania wygenerowanych kolumn przy użyciu konwencji model oparty jest dostosowanie sposobu, w kolumny dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="1b9e0-125">Poniżej znajduje się przykład Konwencji prosty model na podstawie zmienia nazwę każdej kolumny w modelu o nazwie "Dyskryminatora" na "Obiektu EntityType".</span><span class="sxs-lookup"><span data-stu-id="1b9e0-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="1b9e0-126">W tym kolumny, że deweloper po prostu o nazwie "Dyskryminatora".</span><span class="sxs-lookup"><span data-stu-id="1b9e0-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="1b9e0-127">Ponieważ kolumna "Dyskryminatora" jest kolumną wygenerowanego musi on zostać uruchomiony w obszarze S.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="1b9e0-128">Przykład: IA ogólne, zmiana nazwy Konwencji</span><span class="sxs-lookup"><span data-stu-id="1b9e0-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="1b9e0-129">Inny przykład bardziej złożonego modelu na podstawie Konwencji w akcji jest skonfigurować sposób noszą niezależnych skojarzenia (IAs).</span><span class="sxs-lookup"><span data-stu-id="1b9e0-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="1b9e0-130">Jest to sytuacja, w którym Model konwencje są stosowane, ponieważ usługi IAs są generowane przez EF i nie są dostępne w modelu, które mogą uzyskiwać dostęp do interfejsu API DbModelBuilder.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="1b9e0-131">Gdy EF generuje IA, tworzy kolumnę o nazwie EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="1b9e0-132">Na przykład skojarzenie o nazwie klienta z kolumną klucza o nazwie CustomerId ta aplikacja wygeneruje kolumnę o nazwie Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="1b9e0-133">Następujące paski Konwencji "\_" znak poza nazwa kolumny, która jest generowana dla IA.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="1b9e0-134">Rozszerzanie istniejących konwencji</span><span class="sxs-lookup"><span data-stu-id="1b9e0-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="1b9e0-135">Jeśli należy napisać Konwencji, która jest podobna do jednego Konwencji, które platformy Entity Framework już ma zastosowanie do modelu zawsze można rozszerzyć tej Konwencji, aby uniknąć konieczności ponownego pisania jej od podstaw.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="1b9e0-136">Jest na przykład aby zamienić istniejący identyfikator dopasowania Konwencja, za pomocą niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="1b9e0-137">Dodatkowa korzyść do przesłonięcia klucza Konwencji jest, że przeciążonej ma zostać wywołana tylko wtedy, gdy żaden klucz już wykrycia lub jawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="1b9e0-138">Lista konwencje używanym przez program Entity Framework jest dostępny tutaj: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b9e0-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="1b9e0-139">Następnie należy dodać naszej nowej Konwencji przed istniejącej Konwencji klucza.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="1b9e0-140">Po dodamy CustomKeyDiscoveryConvention, możemy usunąć IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="1b9e0-141">Jeśli nie możemy usunąć istniejące IdKeyDiscoveryConvention, ta Konwencja będzie nadal pierwszeństwo Konwencji odnajdywania identyfikator, ponieważ jest uruchamiane, najpierw, ale w przypadku, gdy nie ma właściwości "key" zostanie znaleziony, zostanie uruchomiony Konwencji "id".</span><span class="sxs-lookup"><span data-stu-id="1b9e0-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="1b9e0-142">Zobaczymy to zachowanie, ponieważ każda Konwencja widzi modelu jako zaktualizowany zgodnie z Konwencją poprzedniego (a nie na obsługiwaniu na nim niezależnie i wszystkie połączone ze sobą) powoduje, że jeśli na przykład poprzedniego Konwencji zaktualizowany nazwę kolumny, aby dopasować stanie się coś odsetek swoje niestandardowe Konwencji (jeśli wcześniej nazwę nie zainteresowania), a następnie go będą stosowane do tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="1b9e0-143">Uwagi</span><span class="sxs-lookup"><span data-stu-id="1b9e0-143">Notes</span></span>  

<span data-ttu-id="1b9e0-144">Lista konwencje, które obecnie są stosowane przez program Entity Framework jest dostępna w tej dokumentacji MSDN: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b9e0-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="1b9e0-145">Ta lista jest pobierane bezpośrednio z naszego kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="1b9e0-146">Kod źródłowy platformy Entity Framework 6 jest dostępna w [GitHub](https://github.com/aspnet/entityframework6/) wiele z Konwencji używanych przez program Entity Framework jest dobrym punktami startowymi dla modelu niestandardowego na podstawie Konwencji.</span><span class="sxs-lookup"><span data-stu-id="1b9e0-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  

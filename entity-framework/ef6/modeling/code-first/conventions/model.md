---
title: Konwencje oparte na modelu — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656167"
---
# <a name="model-based-conventions"></a><span data-ttu-id="243d8-102">Konwencje oparte na modelu</span><span class="sxs-lookup"><span data-stu-id="243d8-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="243d8-103">**Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="243d8-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="243d8-104">Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="243d8-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="243d8-105">Konwencje oparte na modelu są zaawansowaną metodą konfiguracji modelu opartej na Konwencji.</span><span class="sxs-lookup"><span data-stu-id="243d8-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="243d8-106">W przypadku większości scenariuszy należy użyć [interfejsu API niestandardowego Code First Konwencji na DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) .</span><span class="sxs-lookup"><span data-stu-id="243d8-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="243d8-107">Przed zastosowaniem Konwencji opartych na modelu zaleca się zrozumienie interfejsu API DbModelBuilder dla Konwencji.</span><span class="sxs-lookup"><span data-stu-id="243d8-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="243d8-108">Konwencje oparte na modelu umożliwiają tworzenie konwencji mających wpływ na właściwości i tabele, których nie można konfigurować za pośrednictwem standardowych konwencji.</span><span class="sxs-lookup"><span data-stu-id="243d8-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="243d8-109">Przykłady tych są kolumnami rozróżniacza w tabelach dla modeli hierarchii i niezależnych skojarzeniach.</span><span class="sxs-lookup"><span data-stu-id="243d8-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="243d8-110">Tworzenie Konwencji</span><span class="sxs-lookup"><span data-stu-id="243d8-110">Creating a Convention</span></span>   

<span data-ttu-id="243d8-111">Pierwszym krokiem w tworzeniu konwencji opartej na modelu jest wybór, kiedy w potoku należy zastosować Konwencję do modelu.</span><span class="sxs-lookup"><span data-stu-id="243d8-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="243d8-112">Istnieją dwa typy Konwencji modelu, koncepcyjne (C-Space) i magazyn (przestrzenie S).</span><span class="sxs-lookup"><span data-stu-id="243d8-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="243d8-113">Konwencja C-Space jest stosowana do modelu, który kompiluje aplikacja, natomiast Konwencja S-Space jest stosowana do wersji modelu, która reprezentuje bazę danych i kontroluje elementy, takie jak automatyczne generowanie kolumn.</span><span class="sxs-lookup"><span data-stu-id="243d8-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="243d8-114">Konwencja modelu jest klasą, która rozciąga się od IConceptualModelConvention lub IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="243d8-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="243d8-115">Te interfejsy akceptują typ ogólny, który może być typu Data, który jest używany do filtrowania typu danych, którego dotyczy dana Konwencja.</span><span class="sxs-lookup"><span data-stu-id="243d8-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="243d8-116">Dodawanie Konwencji</span><span class="sxs-lookup"><span data-stu-id="243d8-116">Adding a Convention</span></span>   

<span data-ttu-id="243d8-117">Konwencje modelu są dodawane w taki sam sposób jak w przypadku zwykłych Konwencji klas.</span><span class="sxs-lookup"><span data-stu-id="243d8-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="243d8-118">W metodzie **OnModelCreating** Dodaj Konwencję do listy Konwencji dla modelu.</span><span class="sxs-lookup"><span data-stu-id="243d8-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="243d8-119">Konwencja można również dodać w odniesieniu do innej konwencji przy użyciu konwencji. addprzed\<\> lub Konwencji. addpo\<\> metod.</span><span class="sxs-lookup"><span data-stu-id="243d8-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="243d8-120">Aby uzyskać więcej informacji na temat Konwencji, które Entity Framework mają zastosowanie, zobacz sekcję Uwagi.</span><span class="sxs-lookup"><span data-stu-id="243d8-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="243d8-121">Przykład: Konwencja modelu rozróżniacza</span><span class="sxs-lookup"><span data-stu-id="243d8-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="243d8-122">Zmiana nazw kolumn generowanych przez EF jest przykładem, że nie można wykonać operacji z innymi interfejsami API Konwencji.</span><span class="sxs-lookup"><span data-stu-id="243d8-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="243d8-123">Jest to sytuacja, w której korzystanie z Konwencji modelu jest jedyną opcją.</span><span class="sxs-lookup"><span data-stu-id="243d8-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="243d8-124">Przykład użycia konwencji opartej na modelu do konfigurowania wygenerowanych kolumn jest dostosowywany do sposobu nazywania kolumn rozróżniacza.</span><span class="sxs-lookup"><span data-stu-id="243d8-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="243d8-125">Poniżej znajduje się przykład prostej konwencji opartej na modelu, która zmienia nazwę każdej kolumny w modelu o nazwie "rozróżniacz" na "EntityType".</span><span class="sxs-lookup"><span data-stu-id="243d8-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="243d8-126">Obejmuje to kolumny, które deweloper po prostu nazywa "rozróżniaczem".</span><span class="sxs-lookup"><span data-stu-id="243d8-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="243d8-127">Ponieważ kolumna "rozróżniacz" jest wygenerowaną kolumną, należy ją uruchomić w przestrzeni S.</span><span class="sxs-lookup"><span data-stu-id="243d8-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="243d8-128">Przykład: ogólna Konwencja zmiany nazwy</span><span class="sxs-lookup"><span data-stu-id="243d8-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="243d8-129">Innym bardziej skomplikowanym przykładem Konwencji opartych na modelu w akcji jest skonfigurowanie sposobu nazywania niezależnych skojarzeń (IAs).</span><span class="sxs-lookup"><span data-stu-id="243d8-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="243d8-130">Jest to sytuacja, w której stosowane są konwencje modelu, ponieważ usługa IAs jest generowana przez EF i nie jest obecna w modelu, do którego interfejs API DbModelBuilder może uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="243d8-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="243d8-131">Gdy Dr generuje IA, tworzy kolumnę o nazwie EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="243d8-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="243d8-132">Na przykład dla skojarzenia o nazwie klient z kolumną klucza o nazwie CustomerId generowanie kolumny o nazwie Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="243d8-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="243d8-133">Poniższa Konwencja przykreśla znak "\_" z nazwy kolumny, która jest generowana dla IA.</span><span class="sxs-lookup"><span data-stu-id="243d8-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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
        for (int i = 0; i < properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a><span data-ttu-id="243d8-134">Rozszerzanie istniejących konwencji</span><span class="sxs-lookup"><span data-stu-id="243d8-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="243d8-135">Jeśli zachodzi konieczność zapisania Konwencji podobnej do jednej z Konwencji, która Entity Framework już dotyczy modelu, zawsze można ją rozłożyć, aby uniknąć konieczności ponownego zapisywania go od podstaw.</span><span class="sxs-lookup"><span data-stu-id="243d8-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="243d8-136">Przykładem jest zastępowanie istniejącej Konwencji dopasowywania identyfikatorów z niestandardowym.</span><span class="sxs-lookup"><span data-stu-id="243d8-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="243d8-137">Dodatkową korzyścią zastąpienia Konwencji klucza jest to, że zastąpiona metoda zostanie wywołana tylko wtedy, gdy nie wykryto już klucza lub jest on jawnie skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="243d8-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="243d8-138">Lista Konwencji, które są używane przez Entity Framework, jest dostępna tutaj: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="243d8-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="243d8-139">Następnie musimy dodać naszą nową Konwencję przed istniejącą Konwencją klucza.</span><span class="sxs-lookup"><span data-stu-id="243d8-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="243d8-140">Po dodaniu CustomKeyDiscoveryConvention można usunąć IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="243d8-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="243d8-141">Jeśli nie usuniesz istniejącej IdKeyDiscoveryConvention, ta konwencja nadal ma pierwszeństwo przed Konwencją odnajdowania identyfikatorów od momentu jego uruchomienia, ale w przypadku braku właściwości "Key" zostanie uruchomiona Konwencja "ID".</span><span class="sxs-lookup"><span data-stu-id="243d8-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="243d8-142">Widzimy takie zachowanie, ponieważ każda Konwencja widzi model zaktualizowany przez poprzednią Konwencję (a nie działa niezależnie, a wszystkie łączą się ze sobą), tak aby Jeśli na przykład w poprzedniej Konwencji Zaktualizowano nazwę kolumny w celu dopasowania do elementu interesuje Cię niestandardową Konwencję (Jeśli przed tą nazwą nie jest interesująca), zostanie ona zastosowana do tej kolumny.</span><span class="sxs-lookup"><span data-stu-id="243d8-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="243d8-143">Uwagi</span><span class="sxs-lookup"><span data-stu-id="243d8-143">Notes</span></span>  

<span data-ttu-id="243d8-144">Lista Konwencji, które są obecnie stosowane przez Entity Framework, jest dostępna w dokumentacji MSDN tutaj: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="243d8-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="243d8-145">Ta lista jest pobierana bezpośrednio z naszego kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="243d8-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="243d8-146">Kod źródłowy dla Entity Framework 6 jest dostępny w witrynie [GitHub](https://github.com/aspnet/entityframework6/) i wiele konwencji używanych przez Entity Framework to dobre punkty wyjścia dla niestandardowych Konwencji opartych na modelu.</span><span class="sxs-lookup"><span data-stu-id="243d8-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  

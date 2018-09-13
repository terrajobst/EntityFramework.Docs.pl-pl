---
title: Na podstawie modelu Konwencji - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: fb79164f71cb3afff705a83f5078a13d043abca8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490938"
---
# <a name="model-based-conventions"></a>Konwencje opartych na modelu
> [!NOTE]
> **EF6 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.  

Model oparty konwencje są zaawansowane metody oparte na Konwencji model konfiguracji. W przypadku większości scenariuszy [niestandardowego kodu pierwszego Konwencji interfejsu API na DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) powinny być używane. Opis interfejsu API DbModelBuilder konwencje zaleca się przed rozpoczęciem korzystania z modelu na podstawie Konwencji.  

Konwencje na podstawie modelu pozwala na tworzenie konwencje, które wpływają na właściwości i tabel, które nie są konfigurowane za pomocą standardowych konwencji. Przykładem tego są dyskryminatora kolumn w tabeli na hierarchii modeli i skojarzenia niezależnie od kolumny.  

## <a name="creating-a-convention"></a>Tworzenie z Konwencją   

Pierwszym krokiem w tworzeniu Konwencji model oparty jest wybór, gdy w potoku Konwencji musi zostać zastosowana do modelu. Istnieją dwa typy modelu Konwencji, Store (S-Space) i dotycząca pojęć (C-Space). Konwencja C + spacja jest stosowany do modelu, który aplikacja zostanie skompilowana Konwencji S miejsca jest stosowane do wersji modelu, który reprezentuje bazy danych i elementy kontrolki, takie jak jak automatycznie generowanych kolumny mają nazwy.  

Konwencja modelu to klasa, która rozszerza IConceptualModelConvention lub IStoreModelConvention.  Te interfejsy, oba akceptują typ ogólny, który może być typu MetadataItem, która jest używana do filtrowania typu danych, który dotyczy Konwencji.  

## <a name="adding-a-convention"></a>Dodawanie Konwencję   

Konwencje modelu zostaną dodane w taki sam sposób, jak regularne konwencje klasy. W **OnModelCreating** metody, Dodaj Konwencji do listy konwencje dla modelu.  

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

Można również dodać Konwencję względem innego Konwencja, za pomocą Conventions.AddBefore\< \> lub Conventions.AddAfter\< \> metody. Aby uzyskać więcej informacji dotyczących Konwencji, których dotyczy platformy Entity Framework, zobacz sekcję Uwagi.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Przykład: Dyskryminatora modelu Konwencji  

Zmienianie nazw kolumn, generowane przez EF jest przykładem coś, co nie można zrobić z innymi konwencjami interfejsów API.  Jest to sytuacja w przypadku, gdy za pomocą modelu Konwencji jest jedynym rozwiązaniem.  

Przykładowy sposób konfigurowania wygenerowanych kolumn przy użyciu konwencji model oparty jest dostosowanie sposobu, w kolumny dyskryminatora.  Poniżej znajduje się przykład Konwencji prosty model na podstawie zmienia nazwę każdej kolumny w modelu o nazwie "Dyskryminatora" na "Obiektu EntityType".  W tym kolumny, że deweloper po prostu o nazwie "Dyskryminatora". Ponieważ kolumna "Dyskryminatora" jest kolumną wygenerowanego musi on zostać uruchomiony w obszarze S.  

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

## <a name="example-general-ia-renaming-convention"></a>Przykład: IA ogólne, zmiana nazwy Konwencji   

Inny przykład bardziej złożonego modelu na podstawie Konwencji w akcji jest skonfigurować sposób noszą niezależnych skojarzenia (IAs).  Jest to sytuacja, w którym Model konwencje są stosowane, ponieważ usługi IAs są generowane przez EF i nie są dostępne w modelu, które mogą uzyskiwać dostęp do interfejsu API DbModelBuilder.  

Gdy EF generuje IA, tworzy kolumnę o nazwie EntityType_KeyName. Na przykład skojarzenie o nazwie klienta z kolumną klucza o nazwie CustomerId ta aplikacja wygeneruje kolumnę o nazwie Customer_CustomerId. Następujące paski Konwencji "\_" znak poza nazwa kolumny, która jest generowana dla IA.  

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

## <a name="extending-existing-conventions"></a>Rozszerzanie istniejących konwencji   

Jeśli należy napisać Konwencji, która jest podobna do jednego Konwencji, które platformy Entity Framework już ma zastosowanie do modelu zawsze można rozszerzyć tej Konwencji, aby uniknąć konieczności ponownego pisania jej od podstaw.  Jest na przykład aby zamienić istniejący identyfikator dopasowania Konwencja, za pomocą niestandardowego.   Dodatkowa korzyść do przesłonięcia klucza Konwencji jest, że przeciążonej ma zostać wywołana tylko wtedy, gdy żaden klucz już wykrycia lub jawnie skonfigurowane. Lista konwencje używanym przez program Entity Framework jest dostępny tutaj: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Następnie należy dodać naszej nowej Konwencji przed istniejącej Konwencji klucza. Po dodamy CustomKeyDiscoveryConvention, możemy usunąć IdKeyDiscoveryConvention.  Jeśli nie możemy usunąć istniejące IdKeyDiscoveryConvention, ta Konwencja będzie nadal pierwszeństwo Konwencji odnajdywania identyfikator, ponieważ jest uruchamiane, najpierw, ale w przypadku, gdy nie ma właściwości "key" zostanie znaleziony, zostanie uruchomiony Konwencji "id".  Zobaczymy to zachowanie, ponieważ każda Konwencja widzi modelu jako zaktualizowany zgodnie z Konwencją poprzedniego (a nie na obsługiwaniu na nim niezależnie i wszystkie połączone ze sobą) powoduje, że jeśli na przykład poprzedniego Konwencji zaktualizowany nazwę kolumny, aby dopasować stanie się coś odsetek swoje niestandardowe Konwencji (jeśli wcześniej nazwę nie zainteresowania), a następnie go będą stosowane do tej kolumny.  

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

## <a name="notes"></a>Uwagi  

Lista konwencje, które obecnie są stosowane przez program Entity Framework jest dostępna w tej dokumentacji MSDN: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Ta lista jest pobierane bezpośrednio z naszego kodu źródłowego.  Kod źródłowy platformy Entity Framework 6 jest dostępna w [GitHub](https://github.com/aspnet/entityframework6/) wiele z Konwencji używanych przez program Entity Framework jest dobrym punktami startowymi dla modelu niestandardowego na podstawie Konwencji.  

---
title: Konwencje oparte na modelu — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419172"
---
# <a name="model-based-conventions"></a>Konwencje oparte na modelu
> [!NOTE]
> **Ef6 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.  

Konwencje oparte na modelu są zaawansowaną metodą konfiguracji modelu opartej na Konwencji. W przypadku większości scenariuszy należy użyć [interfejsu API niestandardowego Code First Konwencji na DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) . Przed zastosowaniem Konwencji opartych na modelu zaleca się zrozumienie interfejsu API DbModelBuilder dla Konwencji.  

Konwencje oparte na modelu umożliwiają tworzenie konwencji mających wpływ na właściwości i tabele, których nie można konfigurować za pośrednictwem standardowych konwencji. Przykłady tych są kolumnami rozróżniacza w tabelach dla modeli hierarchii i niezależnych skojarzeniach.  

## <a name="creating-a-convention"></a>Tworzenie Konwencji   

Pierwszym krokiem w tworzeniu konwencji opartej na modelu jest wybór, kiedy w potoku należy zastosować Konwencję do modelu. Istnieją dwa typy Konwencji modelu, koncepcyjne (C-Space) i magazyn (przestrzenie S). Konwencja C-Space jest stosowana do modelu, który kompiluje aplikacja, natomiast Konwencja S-Space jest stosowana do wersji modelu, która reprezentuje bazę danych i kontroluje elementy, takie jak automatyczne generowanie kolumn.  

Konwencja modelu jest klasą, która rozciąga się od IConceptualModelConvention lub IStoreModelConvention.  Te interfejsy akceptują typ ogólny, który może być typu Data, który jest używany do filtrowania typu danych, którego dotyczy dana Konwencja.  

## <a name="adding-a-convention"></a>Dodawanie Konwencji   

Konwencje modelu są dodawane w taki sam sposób jak w przypadku zwykłych Konwencji klas. W metodzie **OnModelCreating** Dodaj Konwencję do listy Konwencji dla modelu.  

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

Konwencja można również dodać w odniesieniu do innej konwencji przy użyciu konwencji. addprzed\<\> lub Konwencji. addpo\<\> metod. Aby uzyskać więcej informacji na temat Konwencji, które Entity Framework mają zastosowanie, zobacz sekcję Uwagi.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Przykład: Konwencja modelu rozróżniacza  

Zmiana nazw kolumn generowanych przez EF jest przykładem, że nie można wykonać operacji z innymi interfejsami API Konwencji.  Jest to sytuacja, w której korzystanie z Konwencji modelu jest jedyną opcją.  

Przykład użycia konwencji opartej na modelu do konfigurowania wygenerowanych kolumn jest dostosowywany do sposobu nazywania kolumn rozróżniacza.  Poniżej znajduje się przykład prostej konwencji opartej na modelu, która zmienia nazwę każdej kolumny w modelu o nazwie "rozróżniacz" na "EntityType".  Obejmuje to kolumny, które deweloper po prostu nazywa "rozróżniaczem". Ponieważ kolumna "rozróżniacz" jest wygenerowaną kolumną, należy ją uruchomić w przestrzeni S.  

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

## <a name="example-general-ia-renaming-convention"></a>Przykład: ogólna Konwencja zmiany nazwy   

Innym bardziej skomplikowanym przykładem Konwencji opartych na modelu w akcji jest skonfigurowanie sposobu nazywania niezależnych skojarzeń (IAs).  Jest to sytuacja, w której stosowane są konwencje modelu, ponieważ usługa IAs jest generowana przez EF i nie jest obecna w modelu, do którego interfejs API DbModelBuilder może uzyskać dostęp.  

Gdy Dr generuje IA, tworzy kolumnę o nazwie EntityType_KeyName. Na przykład dla skojarzenia o nazwie klient z kolumną klucza o nazwie CustomerId generowanie kolumny o nazwie Customer_CustomerId. Poniższa Konwencja przykreśla znak "\_" z nazwy kolumny, która jest generowana dla IA.  

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

## <a name="extending-existing-conventions"></a>Rozszerzanie istniejących konwencji   

Jeśli zachodzi konieczność zapisania Konwencji podobnej do jednej z Konwencji, która Entity Framework już dotyczy modelu, zawsze można ją rozłożyć, aby uniknąć konieczności ponownego zapisywania go od podstaw.  Przykładem jest zastępowanie istniejącej Konwencji dopasowywania identyfikatorów z niestandardowym.   Dodatkową korzyścią zastąpienia Konwencji klucza jest to, że zastąpiona metoda zostanie wywołana tylko wtedy, gdy nie wykryto już klucza lub jest on jawnie skonfigurowany. Lista Konwencji, które są używane przez Entity Framework, jest dostępna tutaj: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Następnie musimy dodać naszą nową Konwencję przed istniejącą Konwencją klucza. Po dodaniu CustomKeyDiscoveryConvention można usunąć IdKeyDiscoveryConvention.  Jeśli nie usuniesz istniejącej IdKeyDiscoveryConvention, ta konwencja nadal ma pierwszeństwo przed Konwencją odnajdowania identyfikatorów od momentu jego uruchomienia, ale w przypadku braku właściwości "Key" zostanie uruchomiona Konwencja "ID".  Widzimy takie zachowanie, ponieważ każda Konwencja widzi model zaktualizowany przez poprzednią Konwencję (a nie działa niezależnie, a wszystkie łączą się ze sobą), tak aby Jeśli na przykład w poprzedniej Konwencji Zaktualizowano nazwę kolumny w celu dopasowania do elementu interesuje Cię niestandardową Konwencję (Jeśli przed tą nazwą nie jest interesująca), zostanie ona zastosowana do tej kolumny.  

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

Lista Konwencji, które są obecnie stosowane przez Entity Framework, jest dostępna w dokumentacji MSDN tutaj: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Ta lista jest pobierana bezpośrednio z naszego kodu źródłowego.  Kod źródłowy dla Entity Framework 6 jest dostępny w witrynie [GitHub](https://github.com/aspnet/entityframework6/) i wiele konwencji używanych przez Entity Framework to dobre punkty wyjścia dla niestandardowych Konwencji opartych na modelu.  

---
title: Konwencje Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419291"
---
# <a name="code-first-conventions"></a><span data-ttu-id="28ddc-102">Konwencje Code First</span><span class="sxs-lookup"><span data-stu-id="28ddc-102">Code First Conventions</span></span>
<span data-ttu-id="28ddc-103">Code First pozwala na opisywanie modelu przy użyciu C# lub Visual Basic klas platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="28ddc-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="28ddc-104">Podstawowy kształt modelu jest wykrywany przy użyciu konwencji.</span><span class="sxs-lookup"><span data-stu-id="28ddc-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="28ddc-105">Konwencje to zestawy reguł, które są używane do automatycznego konfigurowania modelu koncepcyjnego na podstawie definicji klas podczas pracy z Code First.</span><span class="sxs-lookup"><span data-stu-id="28ddc-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="28ddc-106">Konwencje są zdefiniowane w przestrzeni nazw System. Data. Entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="28ddc-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="28ddc-107">Możesz również skonfigurować model przy użyciu adnotacji danych lub interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="28ddc-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="28ddc-108">Pierwszeństwo jest przyznany do konfiguracji za pomocą interfejsu API Fluent, a następnie adnotacje danych, a następnie konwencje.</span><span class="sxs-lookup"><span data-stu-id="28ddc-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="28ddc-109">Aby uzyskać więcej informacji, zobacz [Adnotacje dotyczące danych](~/ef6/modeling/code-first/data-annotations.md), usługi [Fluent API i relacje](~/ef6/modeling/code-first/fluent/relationships.md)z [interfejsem API Fluent — typy & właściwości](~/ef6/modeling/code-first/fluent/types-and-properties.md) i [interfejs API Fluent z VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="28ddc-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="28ddc-110">Szczegółowa lista konwencji Code First jest dostępna w [dokumentacji interfejsu API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="28ddc-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="28ddc-111">Ten temat zawiera omówienie Konwencji używanych przez Code First.</span><span class="sxs-lookup"><span data-stu-id="28ddc-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="28ddc-112">Odnajdowanie typów</span><span class="sxs-lookup"><span data-stu-id="28ddc-112">Type Discovery</span></span>  

<span data-ttu-id="28ddc-113">W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny).</span><span class="sxs-lookup"><span data-stu-id="28ddc-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="28ddc-114">Oprócz definiowania klas należy również pozwolić, aby **DbContext** wie, które typy mają być uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="28ddc-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="28ddc-115">W tym celu należy zdefiniować klasę kontekstową, która pochodzi z **DbContext** i uwidacznia właściwości **nieogólnymi** dla typów, które mają być częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="28ddc-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="28ddc-116">Code First będzie zawierać te typy, a także będzie ściągać w dowolnych typach, do których istnieją odwołania, nawet jeśli typy, do których istnieją odwołania, są zdefiniowane w innym zestawie.</span><span class="sxs-lookup"><span data-stu-id="28ddc-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="28ddc-117">Jeśli typy uczestniczą w hierarchii dziedziczenia, wystarczy zdefiniować Właściwość **nieogólnymi** dla klasy bazowej, a typy pochodne zostaną automatycznie uwzględnione, jeśli znajdują się w tym samym zestawie co Klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="28ddc-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="28ddc-118">W poniższym przykładzie istnieje tylko jedna właściwość **nieogólnymi** zdefiniowana w klasie **SchoolEntities** (**departaments**).</span><span class="sxs-lookup"><span data-stu-id="28ddc-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="28ddc-119">Code First używa tej właściwości do odnajdywania i ściągania dowolnych typów, do których istnieją odwołania.</span><span class="sxs-lookup"><span data-stu-id="28ddc-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="28ddc-120">Jeśli chcesz wykluczyć typ z modelu, Użyj atrybutu **NotMapped** lub **DbModelBuilder. ignore** Fluent API.</span><span class="sxs-lookup"><span data-stu-id="28ddc-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="28ddc-121">Konwencja klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="28ddc-121">Primary Key Convention</span></span>  

<span data-ttu-id="28ddc-122">Code First wniosku, że właściwość jest kluczem podstawowym, jeśli właściwość klasy ma nazwę "ID" (bez uwzględniania wielkości liter) lub nazwę klasy, po której następuje wartość "ID".</span><span class="sxs-lookup"><span data-stu-id="28ddc-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="28ddc-123">Jeśli typ właściwości klucz podstawowy ma wartość numeryczną lub identyfikator GUID, zostanie on skonfigurowany jako kolumna tożsamości.</span><span class="sxs-lookup"><span data-stu-id="28ddc-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="28ddc-124">Konwencja relacji</span><span class="sxs-lookup"><span data-stu-id="28ddc-124">Relationship Convention</span></span>  

<span data-ttu-id="28ddc-125">W Entity Framework właściwości nawigacji umożliwiają nawigowanie po relacjach między dwoma typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="28ddc-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="28ddc-126">Każdy obiekt może mieć właściwość nawigacji dla każdej relacji, w której uczestniczy.</span><span class="sxs-lookup"><span data-stu-id="28ddc-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="28ddc-127">Właściwości nawigacji umożliwiają Nawigowanie między relacjami i zarządzanie nimi w obu kierunkach, zwracając obiekt odniesienia (Jeśli liczebność jest równa jeden lub zero-lub-jeden) lub kolekcji (Jeśli liczebność ma wiele wartości).</span><span class="sxs-lookup"><span data-stu-id="28ddc-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="28ddc-128">Code First wnioskuje relacje na podstawie właściwości nawigacji zdefiniowanych w Twoich typach.</span><span class="sxs-lookup"><span data-stu-id="28ddc-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="28ddc-129">Oprócz właściwości nawigacji zaleca się dołączenie właściwości klucza obcego dla typów, które reprezentują obiekty zależne.</span><span class="sxs-lookup"><span data-stu-id="28ddc-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="28ddc-130">Każda właściwość o tym samym typie danych co Właściwość głównego klucza głównego i o nazwie, która ma następujący format reprezentuje klucz obcy relacji: "\<nazwa właściwości nawigacji\>\<głównej nazwy właściwości klucza podstawowego\>", "\<głównej nazwy klasy\>" \<nazwa właściwości klucza podstawowego\>"lub"\<głównej nazwy właściwości klucza podstawowego\>".</span><span class="sxs-lookup"><span data-stu-id="28ddc-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="28ddc-131">Jeśli zostanie znalezionych wiele dopasowań, pierwszeństwo jest podany w kolejności podanej powyżej.</span><span class="sxs-lookup"><span data-stu-id="28ddc-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="28ddc-132">W przypadku wykrywania klucza obcego nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="28ddc-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="28ddc-133">Gdy zostanie wykryta właściwość klucza obcego, Code First ustala liczebność relacji na podstawie wartości null klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="28ddc-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="28ddc-134">Jeśli właściwość dopuszcza wartość null, relacja jest zarejestrowana jako opcjonalna. w przeciwnym razie relacja jest zarejestrowana zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="28ddc-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="28ddc-135">Jeśli klucz obcy jednostki zależnej nie dopuszcza wartości null, Code First ustawia kaskadowe usuwanie dla relacji.</span><span class="sxs-lookup"><span data-stu-id="28ddc-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="28ddc-136">Jeśli klucz obcy podmiotu zależnego ma wartość null, Code First nie ustawi kaskadowego usuwania dla relacji i po usunięciu podmiotu zabezpieczeń klucz obcy zostanie ustawiony na wartość null.</span><span class="sxs-lookup"><span data-stu-id="28ddc-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="28ddc-137">Sposób mnożenia i usuwania kaskadowego wykrywany przez Konwencję można zastąpić za pomocą interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="28ddc-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="28ddc-138">W poniższym przykładzie właściwości nawigacji i klucz obcy są używane do definiowania relacji między klasami działów i kursów.</span><span class="sxs-lookup"><span data-stu-id="28ddc-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="28ddc-139">Jeśli istnieje wiele relacji między tymi samymi typami (na przykład załóżmy, że definiujesz klasy **osoba** i **książka** , gdzie Klasa **Person** zawiera właściwości nawigacji **ReviewedBooks** i **AuthoredBooks** , a Klasa **Book** zawiera właściwości nawigacji **autora** i **recenzenta** ) musisz ręcznie skonfigurować relacje przy użyciu adnotacji danych lub interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="28ddc-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="28ddc-140">Aby uzyskać więcej informacji, zobacz [Adnotacje dotyczące danych — relacje](~/ef6/modeling/code-first/data-annotations.md) i [interfejs API Fluent — relacje](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="28ddc-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="28ddc-141">Konwencja typów złożonych</span><span class="sxs-lookup"><span data-stu-id="28ddc-141">Complex Types Convention</span></span>  

<span data-ttu-id="28ddc-142">Gdy Code First odnajduje definicję klasy, w której nie można wywnioskować klucza podstawowego, a klucz podstawowy nie jest zarejestrowany za pomocą adnotacji danych lub interfejsu API Fluent, typ jest automatycznie rejestrowany jako typ złożony.</span><span class="sxs-lookup"><span data-stu-id="28ddc-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="28ddc-143">Wykrywanie typu złożonego wymaga również, aby typ nie miał właściwości, które odwołują się do typów jednostek i nie jest przywoływany przez właściwość kolekcji innego typu.</span><span class="sxs-lookup"><span data-stu-id="28ddc-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="28ddc-144">Uwzględniając następujące definicje klas Code First mogą wnioskować, że **szczegóły** są typu złożonego, ponieważ nie ma klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="28ddc-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="28ddc-145">Konwencja parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="28ddc-145">Connection String Convention</span></span>  

<span data-ttu-id="28ddc-146">Aby dowiedzieć się więcej na temat Konwencji używanych przez DbContext do wykrywania połączenia do użycia, zobacz [połączenia i modele](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="28ddc-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="28ddc-147">Usuwanie Konwencji</span><span class="sxs-lookup"><span data-stu-id="28ddc-147">Removing Conventions</span></span>  

<span data-ttu-id="28ddc-148">Można usunąć wszystkie konwencje zdefiniowane w przestrzeni nazw System. Data. Entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="28ddc-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="28ddc-149">Poniższy przykład usuwa **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="28ddc-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="28ddc-150">Konwencje niestandardowe</span><span class="sxs-lookup"><span data-stu-id="28ddc-150">Custom Conventions</span></span>  

<span data-ttu-id="28ddc-151">Konwencje niestandardowe są obsługiwane w programie EF6 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="28ddc-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="28ddc-152">Aby uzyskać więcej informacji, zobacz [konwencje Code First niestandardowych](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="28ddc-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>

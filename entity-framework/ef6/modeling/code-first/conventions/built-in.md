---
title: Pierwszy konwencje - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912062"
---
# <a name="code-first-conventions"></a><span data-ttu-id="782a4-102">Pierwszy konwencje związane z</span><span class="sxs-lookup"><span data-stu-id="782a4-102">Code First Conventions</span></span>
<span data-ttu-id="782a4-103">Kod najpierw umożliwia opisują modelu przy użyciu klas języka C# lub Visual Basic .NET.</span><span class="sxs-lookup"><span data-stu-id="782a4-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="782a4-104">Wykryto kształtu podstawowego modelu przy użyciu Konwencji.</span><span class="sxs-lookup"><span data-stu-id="782a4-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="782a4-105">Konwencje to zestawy reguł, które są używane do automatycznego konfigurowania modelu koncepcyjnego oparte na definicje klas, podczas pracy z usługą Code First.</span><span class="sxs-lookup"><span data-stu-id="782a4-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="782a4-106">Konwencje są zdefiniowane w przestrzeni nazw System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="782a4-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="782a4-107">Model można dodatkowo skonfigurować przy użyciu adnotacji danych lub interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="782a4-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="782a4-108">Pierwszeństwo jest przyznawane konfiguracji za pomocą interfejsu API fluent adnotacje danych, a następnie Konwencji.</span><span class="sxs-lookup"><span data-stu-id="782a4-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="782a4-109">Aby uzyskać więcej informacji, zobacz [adnotacje danych](~/ef6/modeling/code-first/data-annotations.md), [interfejs Fluent API — relacje](~/ef6/modeling/code-first/fluent/relationships.md), [interfejs Fluent API — typy & właściwości](~/ef6/modeling/code-first/fluent/types-and-properties.md) i [Fluent interfejsu API za pomocą VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="782a4-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="782a4-110">Szczegółowa lista konwencje Code First jest dostępna w [dokumentacji interfejsu API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="782a4-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="782a4-111">Ten temat zawiera omówienie Konwencji używanych przez rozwiązanie Code First.</span><span class="sxs-lookup"><span data-stu-id="782a4-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="782a4-112">Typ odnajdywania</span><span class="sxs-lookup"><span data-stu-id="782a4-112">Type Discovery</span></span>  

<span data-ttu-id="782a4-113">Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena).</span><span class="sxs-lookup"><span data-stu-id="782a4-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="782a4-114">Oprócz definiowania klasy, należy również umożliwić **DbContext** wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="782a4-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="782a4-115">Aby to zrobić, należy zdefiniować klasy kontekstu, która pochodzi od klasy **DbContext** i udostępnia **DbSet** właściwości dla typów, które mają być częścią modelu.</span><span class="sxs-lookup"><span data-stu-id="782a4-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="782a4-116">Kod najpierw będzie zawierać tych typów i również będzie ściągać wszystkie typy odwołania, nawet jeśli przywoływane typy są definiowane w innym zestawie.</span><span class="sxs-lookup"><span data-stu-id="782a4-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="782a4-117">Jeśli Twoje typy brać udziału w hierarchii dziedziczenia, jest wystarczający, aby zdefiniować **DbSet** właściwości dla klasy bazowej, a typów pochodnych zostanie automatycznie dołączony, jeśli są w tym samym zestawie jako klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="782a4-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="782a4-118">W poniższym przykładzie jest tylko jedna **DbSet** właściwości zdefiniowane w **SchoolEntities** klasy (**działów**).</span><span class="sxs-lookup"><span data-stu-id="782a4-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="782a4-119">Kod najpierw używa tej właściwości do odnalezienia i pobieraj żadnych typów odwołania.</span><span class="sxs-lookup"><span data-stu-id="782a4-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="782a4-120">Aby wyłączyć typ z modelu, należy użyć **NotMapped** atrybutu lub **DbModelBuilder.Ignore** wygodnego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="782a4-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="782a4-121">Konwencja klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="782a4-121">Primary Key Convention</span></span>  

<span data-ttu-id="782a4-122">Kod najpierw wnioskuje, że właściwość jest kluczem podstawowym, czy właściwości klasy ma nazwę "ID" (bez uwzględniania wielkości liter), a następnie nazwę klasy "ID".</span><span class="sxs-lookup"><span data-stu-id="782a4-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="782a4-123">Jeśli typ właściwość klucza podstawowego jest wartością liczbową lub identyfikator GUID zostanie on skonfigurowany jako kolumny tożsamości.</span><span class="sxs-lookup"><span data-stu-id="782a4-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="782a4-124">Konwencja relacji</span><span class="sxs-lookup"><span data-stu-id="782a4-124">Relationship Convention</span></span>  

<span data-ttu-id="782a4-125">Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji.</span><span class="sxs-lookup"><span data-stu-id="782a4-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="782a4-126">Każdy obiekt może mieć właściwości nawigacji dla każdej relacji, w których uczestniczy.</span><span class="sxs-lookup"><span data-stu-id="782a4-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="782a4-127">Właściwości nawigacji pozwala na przechodzenie relacji i zarządzanie nimi w obu kierunkach, zwracając obiekt odwołania (Jeśli liczebność jest jedną lub zero lub jeden) lub kolekcji (Jeśli liczebność to wiele).</span><span class="sxs-lookup"><span data-stu-id="782a4-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="782a4-128">Kod najpierw wnioskuje relacje na podstawie właściwości nawigacji zdefiniowany dla typów.</span><span class="sxs-lookup"><span data-stu-id="782a4-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="782a4-129">Oprócz właściwości nawigacji zaleca się, że zawrzesz właściwości klucza obcego na typy, które reprezentują obiekty zależne.</span><span class="sxs-lookup"><span data-stu-id="782a4-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="782a4-130">Dowolną właściwość przy użyciu tego samego typu danych jako główną właściwość klucza podstawowego i nazwą, która jest zgodna z jedną z następujących formatów reprezentuje klucz obcy dla relacji: "\<nazwy właściwości nawigacji\>\<podmiotu zabezpieczeń Nazwa właściwości klucza podstawowego\>','\<Nazwa klasy jednostki\>\<nazwa właściwość klucza podstawowego\>", lub"\<nazwa główna właściwość klucza podstawowego\>".</span><span class="sxs-lookup"><span data-stu-id="782a4-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="782a4-131">Jeśli nie zostaną znalezione wiele dopasowań pierwszeństwo jest przyznawane w kolejności podanej powyżej.</span><span class="sxs-lookup"><span data-stu-id="782a4-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="782a4-132">Wykrywanie klucza obcego nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="782a4-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="782a4-133">Po wykryciu właściwości klucza obcego Code First wnioskuje liczebność relacji, w oparciu o dopuszczania wartości Null klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="782a4-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="782a4-134">Jeśli właściwość ma wartość null następnie relacji jest zarejestrowany jako opcjonalna. w przeciwnym razie relacja jest zarejestrowany zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="782a4-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="782a4-135">Jeśli klucz obcy dla jednostki zależne nie dopuszcza wartości null, następnie Code First ustawia usuwanie kaskadowe relacji.</span><span class="sxs-lookup"><span data-stu-id="782a4-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="782a4-136">Jeśli klucz obcy dla jednostki zależne ma wartość null, Code First nie ustawia usuwanie kaskadowe relacji i gdy podmiot zabezpieczeń został usunięty klucz obcy zostanie ustawiona na wartość null.</span><span class="sxs-lookup"><span data-stu-id="782a4-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="782a4-137">Liczebność i cascade Usuń zachowanie wykryta przez Konwencję może zostać przesłonięta przez przy użyciu interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="782a4-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="782a4-138">W poniższym przykładzie właściwości nawigacji i klucza obcego są używane do definiowania relacji między klasami dział i kursów.</span><span class="sxs-lookup"><span data-stu-id="782a4-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="782a4-139">Jeśli masz wiele relacji między tymi samymi typami (na przykład, załóżmy, że należy zdefiniować **osoby** i **książki** klas, gdzie **osoby** klasa zawiera  **ReviewedBooks** i **AuthoredBooks** właściwości nawigacji i **książki** klasa zawiera **Autor** i  **Weryfikacja** właściwości nawigacji), musisz ręcznie skonfigurować relacje przy użyciu adnotacji danych lub interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="782a4-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="782a4-140">Aby uzyskać więcej informacji, zobacz [adnotacje danych — relacje](~/ef6/modeling/code-first/data-annotations.md) i [interfejs Fluent API — relacje](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="782a4-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="782a4-141">Typy złożone Konwencji</span><span class="sxs-lookup"><span data-stu-id="782a4-141">Complex Types Convention</span></span>  

<span data-ttu-id="782a4-142">Gdy Code First odnajduje definicję klasy, w którym klucz podstawowy, nie można wywnioskować, a nie klucza podstawowego jest zarejestrowana przy użyciu adnotacji danych lub interfejsu API fluent, ten typ jest automatycznie zarejestrowany jako typ złożony.</span><span class="sxs-lookup"><span data-stu-id="782a4-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="782a4-143">Wykrywanie typu złożonego wymaga również, że typ nie ma właściwości, które odwołują się typów jednostek i nie odwołuje się właściwość kolekcji innego typu.</span><span class="sxs-lookup"><span data-stu-id="782a4-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="782a4-144">Biorąc pod uwagę następujące definicje klas Code First może wywnioskować, **szczegóły** jest typem złożonym, ponieważ ma ona bez klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="782a4-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="782a4-145">Konwencja ciąg połączenia</span><span class="sxs-lookup"><span data-stu-id="782a4-145">Connection String Convention</span></span>  

<span data-ttu-id="782a4-146">Aby dowiedzieć się o z konwencjami, czy kontekst DbContext używa do odnajdywania połączenie zobacz [połączenia i modele](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="782a4-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="782a4-147">Usuwanie Konwencji</span><span class="sxs-lookup"><span data-stu-id="782a4-147">Removing Conventions</span></span>  

<span data-ttu-id="782a4-148">Możesz usunąć dowolne konwencje zdefiniowany w przestrzeni nazw System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="782a4-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="782a4-149">Poniższy przykład usuwa **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="782a4-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="782a4-150">Konwencje niestandardowe</span><span class="sxs-lookup"><span data-stu-id="782a4-150">Custom Conventions</span></span>  

<span data-ttu-id="782a4-151">Konwencje niestandardowe są obsługiwane w EF6 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="782a4-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="782a4-152">Aby uzyskać więcej informacji, zobacz [konwencje pierwszy kod niestandardowy](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="782a4-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>

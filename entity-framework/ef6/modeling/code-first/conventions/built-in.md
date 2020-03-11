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
# <a name="code-first-conventions"></a>Konwencje Code First
Code First pozwala na opisywanie modelu przy użyciu C# lub Visual Basic klas platformy .NET. Podstawowy kształt modelu jest wykrywany przy użyciu konwencji. Konwencje to zestawy reguł, które są używane do automatycznego konfigurowania modelu koncepcyjnego na podstawie definicji klas podczas pracy z Code First. Konwencje są zdefiniowane w przestrzeni nazw System. Data. Entity. ModelConfiguration. Conventions.  

Możesz również skonfigurować model przy użyciu adnotacji danych lub interfejsu API Fluent. Pierwszeństwo jest przyznany do konfiguracji za pomocą interfejsu API Fluent, a następnie adnotacje danych, a następnie konwencje. Aby uzyskać więcej informacji, zobacz [Adnotacje dotyczące danych](~/ef6/modeling/code-first/data-annotations.md), usługi [Fluent API i relacje](~/ef6/modeling/code-first/fluent/relationships.md)z [interfejsem API Fluent — typy & właściwości](~/ef6/modeling/code-first/fluent/types-and-properties.md) i [interfejs API Fluent z VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Szczegółowa lista konwencji Code First jest dostępna w [dokumentacji interfejsu API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Ten temat zawiera omówienie Konwencji używanych przez Code First.  

## <a name="type-discovery"></a>Odnajdowanie typów  

W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny). Oprócz definiowania klas należy również pozwolić, aby **DbContext** wie, które typy mają być uwzględnione w modelu. W tym celu należy zdefiniować klasę kontekstową, która pochodzi z **DbContext** i uwidacznia właściwości **nieogólnymi** dla typów, które mają być częścią modelu. Code First będzie zawierać te typy, a także będzie ściągać w dowolnych typach, do których istnieją odwołania, nawet jeśli typy, do których istnieją odwołania, są zdefiniowane w innym zestawie.  

Jeśli typy uczestniczą w hierarchii dziedziczenia, wystarczy zdefiniować Właściwość **nieogólnymi** dla klasy bazowej, a typy pochodne zostaną automatycznie uwzględnione, jeśli znajdują się w tym samym zestawie co Klasa bazowa.  

W poniższym przykładzie istnieje tylko jedna właściwość **nieogólnymi** zdefiniowana w klasie **SchoolEntities** (**departaments**). Code First używa tej właściwości do odnajdywania i ściągania dowolnych typów, do których istnieją odwołania.  

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

Jeśli chcesz wykluczyć typ z modelu, Użyj atrybutu **NotMapped** lub **DbModelBuilder. ignore** Fluent API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Konwencja klucza podstawowego  

Code First wniosku, że właściwość jest kluczem podstawowym, jeśli właściwość klasy ma nazwę "ID" (bez uwzględniania wielkości liter) lub nazwę klasy, po której następuje wartość "ID". Jeśli typ właściwości klucz podstawowy ma wartość numeryczną lub identyfikator GUID, zostanie on skonfigurowany jako kolumna tożsamości.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Konwencja relacji  

W Entity Framework właściwości nawigacji umożliwiają nawigowanie po relacjach między dwoma typami jednostek. Każdy obiekt może mieć właściwość nawigacji dla każdej relacji, w której uczestniczy. Właściwości nawigacji umożliwiają Nawigowanie między relacjami i zarządzanie nimi w obu kierunkach, zwracając obiekt odniesienia (Jeśli liczebność jest równa jeden lub zero-lub-jeden) lub kolekcji (Jeśli liczebność ma wiele wartości). Code First wnioskuje relacje na podstawie właściwości nawigacji zdefiniowanych w Twoich typach.  

Oprócz właściwości nawigacji zaleca się dołączenie właściwości klucza obcego dla typów, które reprezentują obiekty zależne. Każda właściwość o tym samym typie danych co Właściwość głównego klucza głównego i o nazwie, która ma następujący format reprezentuje klucz obcy relacji: "\<nazwa właściwości nawigacji\>\<głównej nazwy właściwości klucza podstawowego\>", "\<głównej nazwy klasy\>" \<nazwa właściwości klucza podstawowego\>"lub"\<głównej nazwy właściwości klucza podstawowego\>". Jeśli zostanie znalezionych wiele dopasowań, pierwszeństwo jest podany w kolejności podanej powyżej. W przypadku wykrywania klucza obcego nie jest rozróżniana wielkość liter. Gdy zostanie wykryta właściwość klucza obcego, Code First ustala liczebność relacji na podstawie wartości null klucza obcego. Jeśli właściwość dopuszcza wartość null, relacja jest zarejestrowana jako opcjonalna. w przeciwnym razie relacja jest zarejestrowana zgodnie z wymaganiami.  

Jeśli klucz obcy jednostki zależnej nie dopuszcza wartości null, Code First ustawia kaskadowe usuwanie dla relacji. Jeśli klucz obcy podmiotu zależnego ma wartość null, Code First nie ustawi kaskadowego usuwania dla relacji i po usunięciu podmiotu zabezpieczeń klucz obcy zostanie ustawiony na wartość null. Sposób mnożenia i usuwania kaskadowego wykrywany przez Konwencję można zastąpić za pomocą interfejsu API Fluent.  

W poniższym przykładzie właściwości nawigacji i klucz obcy są używane do definiowania relacji między klasami działów i kursów.  

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
> Jeśli istnieje wiele relacji między tymi samymi typami (na przykład załóżmy, że definiujesz klasy **osoba** i **książka** , gdzie Klasa **Person** zawiera właściwości nawigacji **ReviewedBooks** i **AuthoredBooks** , a Klasa **Book** zawiera właściwości nawigacji **autora** i **recenzenta** ) musisz ręcznie skonfigurować relacje przy użyciu adnotacji danych lub interfejsu API Fluent. Aby uzyskać więcej informacji, zobacz [Adnotacje dotyczące danych — relacje](~/ef6/modeling/code-first/data-annotations.md) i [interfejs API Fluent — relacje](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Konwencja typów złożonych  

Gdy Code First odnajduje definicję klasy, w której nie można wywnioskować klucza podstawowego, a klucz podstawowy nie jest zarejestrowany za pomocą adnotacji danych lub interfejsu API Fluent, typ jest automatycznie rejestrowany jako typ złożony. Wykrywanie typu złożonego wymaga również, aby typ nie miał właściwości, które odwołują się do typów jednostek i nie jest przywoływany przez właściwość kolekcji innego typu. Uwzględniając następujące definicje klas Code First mogą wnioskować, że **szczegóły** są typu złożonego, ponieważ nie ma klucza podstawowego.  

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

## <a name="connection-string-convention"></a>Konwencja parametrów połączenia  

Aby dowiedzieć się więcej na temat Konwencji używanych przez DbContext do wykrywania połączenia do użycia, zobacz [połączenia i modele](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Usuwanie Konwencji  

Można usunąć wszystkie konwencje zdefiniowane w przestrzeni nazw System. Data. Entity. ModelConfiguration. Conventions. Poniższy przykład usuwa **PluralizingTableNameConvention**.  

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

## <a name="custom-conventions"></a>Konwencje niestandardowe  

Konwencje niestandardowe są obsługiwane w programie EF6 lub nowszym. Aby uzyskać więcej informacji, zobacz [konwencje Code First niestandardowych](~/ef6/modeling/code-first/conventions/custom.md).

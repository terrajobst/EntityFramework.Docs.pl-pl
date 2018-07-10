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
# <a name="code-first-conventions"></a>Pierwszy konwencje związane z
Kod najpierw umożliwia opisują modelu przy użyciu klas języka C# lub Visual Basic .NET. Wykryto kształtu podstawowego modelu przy użyciu Konwencji. Konwencje to zestawy reguł, które są używane do automatycznego konfigurowania modelu koncepcyjnego oparte na definicje klas, podczas pracy z usługą Code First. Konwencje są zdefiniowane w przestrzeni nazw System.Data.Entity.ModelConfiguration.Conventions.  

Model można dodatkowo skonfigurować przy użyciu adnotacji danych lub interfejsu API fluent. Pierwszeństwo jest przyznawane konfiguracji za pomocą interfejsu API fluent adnotacje danych, a następnie Konwencji. Aby uzyskać więcej informacji, zobacz [adnotacje danych](~/ef6/modeling/code-first/data-annotations.md), [interfejs Fluent API — relacje](~/ef6/modeling/code-first/fluent/relationships.md), [interfejs Fluent API — typy & właściwości](~/ef6/modeling/code-first/fluent/types-and-properties.md) i [Fluent interfejsu API za pomocą VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Szczegółowa lista konwencje Code First jest dostępna w [dokumentacji interfejsu API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Ten temat zawiera omówienie Konwencji używanych przez rozwiązanie Code First.  

## <a name="type-discovery"></a>Typ odnajdywania  

Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena). Oprócz definiowania klasy, należy również umożliwić **DbContext** wiedzieć, jakie typy, które mają zostać uwzględnione w modelu. Aby to zrobić, należy zdefiniować klasy kontekstu, która pochodzi od klasy **DbContext** i udostępnia **DbSet** właściwości dla typów, które mają być częścią modelu. Kod najpierw będzie zawierać tych typów i również będzie ściągać wszystkie typy odwołania, nawet jeśli przywoływane typy są definiowane w innym zestawie.  

Jeśli Twoje typy brać udziału w hierarchii dziedziczenia, jest wystarczający, aby zdefiniować **DbSet** właściwości dla klasy bazowej, a typów pochodnych zostanie automatycznie dołączony, jeśli są w tym samym zestawie jako klasa bazowa.  

W poniższym przykładzie jest tylko jedna **DbSet** właściwości zdefiniowane w **SchoolEntities** klasy (**działów**). Kod najpierw używa tej właściwości do odnalezienia i pobieraj żadnych typów odwołania.  

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

Aby wyłączyć typ z modelu, należy użyć **NotMapped** atrybutu lub **DbModelBuilder.Ignore** wygodnego interfejsu API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Konwencja klucza podstawowego  

Kod najpierw wnioskuje, że właściwość jest kluczem podstawowym, czy właściwości klasy ma nazwę "ID" (bez uwzględniania wielkości liter), a następnie nazwę klasy "ID". Jeśli typ właściwość klucza podstawowego jest wartością liczbową lub identyfikator GUID zostanie on skonfigurowany jako kolumny tożsamości.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Konwencja relacji  

Platformy Entity Framework właściwości nawigacji Podaj sposób przechodzenia relację między dwoma typami encji. Każdy obiekt może mieć właściwości nawigacji dla każdej relacji, w których uczestniczy. Właściwości nawigacji pozwala na przechodzenie relacji i zarządzanie nimi w obu kierunkach, zwracając obiekt odwołania (Jeśli liczebność jest jedną lub zero lub jeden) lub kolekcji (Jeśli liczebność to wiele). Kod najpierw wnioskuje relacje na podstawie właściwości nawigacji zdefiniowany dla typów.  

Oprócz właściwości nawigacji zaleca się, że zawrzesz właściwości klucza obcego na typy, które reprezentują obiekty zależne. Dowolną właściwość przy użyciu tego samego typu danych jako główną właściwość klucza podstawowego i nazwą, która jest zgodna z jedną z następujących formatów reprezentuje klucz obcy dla relacji: "\<nazwy właściwości nawigacji\>\<podmiotu zabezpieczeń Nazwa właściwości klucza podstawowego\>','\<Nazwa klasy jednostki\>\<nazwa właściwość klucza podstawowego\>", lub"\<nazwa główna właściwość klucza podstawowego\>". Jeśli nie zostaną znalezione wiele dopasowań pierwszeństwo jest przyznawane w kolejności podanej powyżej. Wykrywanie klucza obcego nie jest uwzględniana wielkość liter. Po wykryciu właściwości klucza obcego Code First wnioskuje liczebność relacji, w oparciu o dopuszczania wartości Null klucza obcego. Jeśli właściwość ma wartość null następnie relacji jest zarejestrowany jako opcjonalna. w przeciwnym razie relacja jest zarejestrowany zgodnie z wymaganiami.  

Jeśli klucz obcy dla jednostki zależne nie dopuszcza wartości null, następnie Code First ustawia usuwanie kaskadowe relacji. Jeśli klucz obcy dla jednostki zależne ma wartość null, Code First nie ustawia usuwanie kaskadowe relacji i gdy podmiot zabezpieczeń został usunięty klucz obcy zostanie ustawiona na wartość null. Liczebność i cascade Usuń zachowanie wykryta przez Konwencję może zostać przesłonięta przez przy użyciu interfejsu API fluent.  

W poniższym przykładzie właściwości nawigacji i klucza obcego są używane do definiowania relacji między klasami dział i kursów.  

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
> Jeśli masz wiele relacji między tymi samymi typami (na przykład, załóżmy, że należy zdefiniować **osoby** i **książki** klas, gdzie **osoby** klasa zawiera  **ReviewedBooks** i **AuthoredBooks** właściwości nawigacji i **książki** klasa zawiera **Autor** i  **Weryfikacja** właściwości nawigacji), musisz ręcznie skonfigurować relacje przy użyciu adnotacji danych lub interfejsu API fluent. Aby uzyskać więcej informacji, zobacz [adnotacje danych — relacje](~/ef6/modeling/code-first/data-annotations.md) i [interfejs Fluent API — relacje](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Typy złożone Konwencji  

Gdy Code First odnajduje definicję klasy, w którym klucz podstawowy, nie można wywnioskować, a nie klucza podstawowego jest zarejestrowana przy użyciu adnotacji danych lub interfejsu API fluent, ten typ jest automatycznie zarejestrowany jako typ złożony. Wykrywanie typu złożonego wymaga również, że typ nie ma właściwości, które odwołują się typów jednostek i nie odwołuje się właściwość kolekcji innego typu. Biorąc pod uwagę następujące definicje klas Code First może wywnioskować, **szczegóły** jest typem złożonym, ponieważ ma ona bez klucza podstawowego.  

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

## <a name="connection-string-convention"></a>Konwencja ciąg połączenia  

Aby dowiedzieć się o z konwencjami, czy kontekst DbContext używa do odnajdywania połączenie zobacz [połączenia i modele](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Usuwanie Konwencji  

Możesz usunąć dowolne konwencje zdefiniowany w przestrzeni nazw System.Data.Entity.ModelConfiguration.Conventions. Poniższy przykład usuwa **PluralizingTableNameConvention**.  

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

Konwencje niestandardowe są obsługiwane w EF6 i nowszych wersjach. Aby uzyskać więcej informacji, zobacz [konwencje pierwszy kod niestandardowy](~/ef6/modeling/code-first/conventions/custom.md).

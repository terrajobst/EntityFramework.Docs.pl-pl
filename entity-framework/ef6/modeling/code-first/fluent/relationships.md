---
title: Interfejs Fluent API — relacje - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
caps.latest.revision: 3
ms.openlocfilehash: 7437b395fbed07dcb8c93cd8418317611e14a60b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912662"
---
# <a name="fluent-api---relationships"></a>Interfejs Fluent API — relacji
> [!NOTE]
> Ta strona zawiera informacje o konfigurowaniu relacje w modelu Code First przy użyciu interfejsu API fluent. Aby uzyskać ogólne informacje dotyczące relacji w EF i uzyskiwania dostępu i manipulowanie danymi za pomocą relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).  

Podczas pracy z Code First, należy zdefiniować model przez definiowanie klas CLR Twojej domeny. Domyślnie program Entity Framework używa Code First Konwencji do mapowania klas schematu bazy danych. Jeśli używasz Code First konwencji nazewnictwa, w większości przypadków możesz polegać na Code First, aby zdefiniować relacje między tabelami, usługi na podstawie kluczy obcych i właściwości nawigacji, definiujące na klasy. Jeśli nie zgodne z konwencjami podczas definiowania klas, lub jeśli chcesz zmienić sposób pracy Konwencji, można użyć wygodnego interfejsu API lub adnotacje danych do konfigurowania Twoich zajęciach, więc Code First można mapować relacje między tabelami.  

## <a name="introduction"></a>Wprowadzenie  

Podczas konfigurowania relacji z interfejsu API fluent, rozpoczynać wystąpienia EntityTypeConfiguration i następnie użyć do określenia typu relacji, które ta jednostka uczestniczy w HasRequired, HasOptional czy HasMany metody. Metody HasRequired i HasOptional przyjmują wyrażenie lambda reprezentujące właściwość nawigacji odwołania. Metoda HasMany pobiera Wyrażenie lambda reprezentujące właściwość nawigacji kolekcji. Następnie można skonfigurować właściwości nawigacji odwrotność przy użyciu metod WithRequired WithOptional i WithMany. Metody te mają przeciążenia, które nie przyjmują argumentów i może służyć do określenia kardynalności z tego jednokierunkowe.  

Następnie można skonfigurować właściwości klucza obcego przy użyciu metody HasForeignKey. Ta metoda przyjmuje Wyrażenie lambda reprezentujące właściwość, która ma być używany jako klucza obcego.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Konfigurowanie relacji wymagana do opcjonalne (jeden do — Zero lub jeden)  

Poniższy przykład umożliwia skonfigurowanie relacji jeden do zero lub jeden. OfficeAssignment ma właściwość InstructorID klucz podstawowy i klucz obcy, ponieważ nazwa właściwości nie jest zgodna z Konwencją, metoda HasKey jest używane do konfigurowania klucza podstawowego.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Konfigurowanie relacji, w którym obu końcach są wymagane (jeden do jednego)  

W większości przypadków Entity Framework można wywnioskować typu jest zależna od i która podmiotu zabezpieczeń w relacji. Jednak gdy zarówno końców relacji są wymagane, lub obie strony są opcjonalne platformy Entity Framework nie może zidentyfikować zależnych od ustawień lokalnych i główną. Gdy wymagane są oba końce relacji, należy użyć WithRequiredPrincipal lub WithRequiredDependent po metodzie HasRequired. Po obu stronach relacji są opcjonalne, należy użyć WithOptionalPrincipal lub WithOptionalDependent po metodzie HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Konfigurowanie relacji wiele do wielu  

Poniższy kod służy do konfigurowania relacji wiele do wielu między typami kurs i instruktora. W poniższym przykładzie domyślnych Konwencji Code First są używane do tworzenia tabelę sprzężenia. W wyniku tworzenia CourseInstructor tabeli z kolumnami Course_CourseID i Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Jeśli chcesz określić nazwę tabeli sprzężenia i nazwy kolumn w tabeli, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu metody mapy. Poniższy kod generuje CourseInstructor tabeli z kolumnami CourseID i InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Konfigurowanie relacji z jedną właściwością nawigacji  

(Nazywane również jednokierunkowe) jednokierunkową relacja jest, gdy właściwość nawigacji jest zdefiniowane tylko na jednym z końców relacji, a nie w obu. Zgodnie z Konwencją Code First zawsze interpretuje jednokierunkowa relacja jako jeden do wielu. Na przykład jeśli chcesz, aby bezpośredni związek pomiędzy instruktora oraz przypisane OfficeAssignment, w którym masz właściwości nawigacji na typie przez instruktorów, należy skonfigurować tę relację przy użyciu interfejsu API fluent.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Włączanie usuwanie kaskadowe  

Usuwanie kaskadowe na relacji można skonfigurować przy użyciu metody WillCascadeOnDelete. Jeśli klucz obcy dla jednostki zależne nie dopuszcza wartości null, następnie Code First ustawia usuwanie kaskadowe relacji. Jeśli klucz obcy dla jednostki zależne ma wartość null, Code First nie ustawia usuwanie kaskadowe relacji i gdy podmiot zabezpieczeń został usunięty klucz obcy zostanie ustawiona na wartość null.  

Możesz usunąć tych konwencji usuwanie kaskadowe przy użyciu:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

Poniższy kod konfiguruje relację jako wymaganą, a następnie wyłącza usuwanie kaskadowe.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Konfigurowanie złożony klucz obcy  

Jeśli klucz podstawowy dla typu dział obejmowało DepartmentID i nazwę właściwości, konfigurowania klucza podstawowego dla działu i klucz obcy dla typów kurs w następujący sposób:  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Zmiana nazw klucz obcy, który nie jest zdefiniowany w modelu  

Jeśli nie chcesz zdefiniować klucz obcy dla typu CLR, ale aby określić nazwę, jakie powinien mieć w bazie danych, wykonaj następujące czynności:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Konfigurowanie nazwy klucza obcego, który nie jest zgodna z Konwencją pierwszy kodu  

Właściwość klucza obcego w klasie kurs został wywołany SomeDepartmentID zamiast DepartmentID, należy wykonać następujące czynności, aby określić, że SomeDepartmentID jako klucza obcego:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Model używany w przykładach  

Poniższy model Code First jest używany dla przykładów na tej stronie.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  

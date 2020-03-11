---
title: Interfejs API Fluent — relacje — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419074"
---
# <a name="fluent-api---relationships"></a>Interfejs API Fluent — relacje
> [!NOTE]
> Ta strona zawiera informacje dotyczące konfigurowania relacji w modelu Code First za pomocą interfejsu API Fluent. Aby uzyskać ogólne informacje na temat relacji w EF oraz jak uzyskać dostęp do danych i manipulować nimi przy użyciu relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).  

Podczas pracy z Code First należy zdefiniować model, definiując klasy środowiska CLR domeny. Domyślnie Entity Framework używa konwencji Code First do mapowania klas do schematu bazy danych. Jeśli używasz konwencji nazewnictwa Code First, w większości przypadków można polegać na Code First w celu skonfigurowania relacji między tabelami w oparciu o klucze obce i właściwości nawigacji zdefiniowane dla klas. Jeśli nie przestrzegasz konwencji podczas definiowania klas lub chcesz zmienić sposób działania Konwencji, możesz użyć interfejsu API Fluent lub adnotacji danych do skonfigurowania klas, aby Code First można było zmapować relacje między tabelami.  

## <a name="introduction"></a>Wprowadzenie  

Podczas konfigurowania relacji z interfejsem API Fluent należy rozpocząć od wystąpienia EntityTypeConfiguration, a następnie użyć metody HasRequired, HasOptional lub HasMany, aby określić typ relacji, w której uczestniczy ta jednostka. Metody HasRequired i HasOptional przyjmują wyrażenie lambda reprezentujące właściwość nawigacji referencyjnej. Metoda HasMany przyjmuje wyrażenie lambda reprezentujące właściwość nawigacji kolekcji. Następnie można skonfigurować właściwość nawigacji odwrotnej przy użyciu metod WithRequired, WithOptional i WithMany. Metody te mają przeciążenia, które nie przyjmują argumentów i mogą być używane do określania kardynalności z jednokierunkowymi nawigacjami.  

Następnie można skonfigurować właściwości klucza obcego przy użyciu metody HasForeignKey. Ta metoda przyjmuje wyrażenie lambda reprezentujące właściwość, która ma być używana jako klucz obcy.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Konfigurowanie relacji wymagane-do-opcjonalne (jeden-do-zero-lub-jednego)  

Poniższy przykład służy do konfigurowania relacji jeden-do-zerowe-lub-jednego. OfficeAssignment ma właściwość InstructorID, która jest kluczem podstawowym i kluczem obcym, ponieważ nazwa właściwości nie jest zgodna z Konwencją używaną przez metodę Haskey — do konfigurowania klucza podstawowego.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Konfigurowanie relacji, w której oba punkty końcowe są wymagane (jeden do jednego)  

W większości przypadków Entity Framework może wnioskować, który typ jest zależny i który jest podmiotem zabezpieczeń w relacji. Jeśli jednak oba punkty końcowe relacji są wymagane lub obie strony są opcjonalne Entity Framework nie mogą identyfikować elementów zależnych i głównych. Gdy wymagane są oba punkty końcowe relacji, użyj WithRequiredPrincipal lub WithRequiredDependent po metodzie HasRequired. Gdy oba punkty końcowe relacji są opcjonalne, użyj WithOptionalPrincipal lub WithOptionalDependent po metodzie HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Konfigurowanie relacji wiele-do-wielu  

Poniższy kod służy do konfigurowania relacji wiele-do-wielu między kursem a typem instruktora. W poniższym przykładzie domyślne konwencje Code First są używane do tworzenia tabeli sprzężeń. W wyniku tego tworzona jest tabela CourseInstructor z kolumnami Course_CourseID i Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Jeśli chcesz określić nazwę tabeli sprzężenia i nazwy kolumn w tabeli, należy wykonać dodatkową konfigurację przy użyciu metody map. Poniższy kod generuje tabelę CourseInstructor z kolumnami CourseID i InstructorID.  

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

Relacja jednokierunkowa (nazywana również jednokierunkowe) jest definiowana, gdy właściwość nawigacji jest zdefiniowana tylko dla jednej relacji, a nie na obu. Zgodnie z Konwencją, Code First zawsze interpretuje dwukierunkową relację jako jeden-do-wielu. Na przykład jeśli chcesz utworzyć relację jeden do jednego między instruktorem i OfficeAssignment, gdzie masz właściwość nawigacji tylko dla typu instruktora, musisz użyć interfejsu API Fluent, aby skonfigurować tę relację.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Włączanie usuwania kaskadowego  

Można skonfigurować kaskadowe usuwanie dla relacji za pomocą metody WillCascadeOnDelete. Jeśli klucz obcy jednostki zależnej nie dopuszcza wartości null, Code First ustawia kaskadowe usuwanie dla relacji. Jeśli klucz obcy podmiotu zależnego ma wartość null, Code First nie ustawi kaskadowego usuwania dla relacji i po usunięciu podmiotu zabezpieczeń klucz obcy zostanie ustawiony na wartość null.  

Te konwencje usuwania kaskadowego można usunąć za pomocą następujących funkcji:  

Element modelbuilder. Conventions. Remove\<OneToManyCascadeDeleteConvention\>()  
Element modelbuilder. Conventions. Remove\<ManyToManyCascadeDeleteConvention\>()  

Poniższy kod służy do konfigurowania relacji, która ma być wymagana, a następnie wyłączenie usuwania kaskadowego.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Konfigurowanie złożonego klucza obcego  

Jeśli klucz podstawowy dla typu działu składający się z właściwości DepartmentID i Name, należy skonfigurować klucz podstawowy dla działu i klucz obcy dla następujących typów kursów:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Zmiana nazwy klucza obcego, który nie jest zdefiniowany w modelu  

Jeśli zdecydujesz się nie definiować klucza obcego w typie CLR, ale chcesz określić nazwę, którą powinna zawierać w bazie danych, wykonaj następujące czynności:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Konfigurowanie nazwy klucza obcego, która nie jest zgodna z Konwencją Code First  

Jeśli właściwość klucza obcego w klasie kursów została wywołana SomeDepartmentID zamiast DepartmentID, należy wykonać następujące czynności, aby określić, że SomeDepartmentID ma być kluczem obcym:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Model używany w przykładach  

Następujący model Code First jest używany dla przykładów na tej stronie.  

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

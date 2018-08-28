---
title: Interfejs Fluent API — relacje - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: e13a1370f46362e0f2ca3743ec5b063ce6f87cbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994765"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="911f2-102">Interfejs Fluent API — relacji</span><span class="sxs-lookup"><span data-stu-id="911f2-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="911f2-103">Ta strona zawiera informacje o konfigurowaniu relacje w modelu Code First przy użyciu interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="911f2-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="911f2-104">Aby uzyskać ogólne informacje dotyczące relacji w EF i uzyskiwania dostępu i manipulowanie danymi za pomocą relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="911f2-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="911f2-105">Podczas pracy z Code First, należy zdefiniować model przez definiowanie klas CLR Twojej domeny.</span><span class="sxs-lookup"><span data-stu-id="911f2-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="911f2-106">Domyślnie program Entity Framework używa Code First Konwencji do mapowania klas schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="911f2-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="911f2-107">Jeśli używasz Code First konwencji nazewnictwa, w większości przypadków możesz polegać na Code First, aby zdefiniować relacje między tabelami, usługi na podstawie kluczy obcych i właściwości nawigacji, definiujące na klasy.</span><span class="sxs-lookup"><span data-stu-id="911f2-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="911f2-108">Jeśli nie zgodne z konwencjami podczas definiowania klas, lub jeśli chcesz zmienić sposób pracy Konwencji, można użyć wygodnego interfejsu API lub adnotacje danych do konfigurowania Twoich zajęciach, więc Code First można mapować relacje między tabelami.</span><span class="sxs-lookup"><span data-stu-id="911f2-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="911f2-109">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="911f2-109">Introduction</span></span>  

<span data-ttu-id="911f2-110">Podczas konfigurowania relacji z interfejsu API fluent, rozpoczynać wystąpienia EntityTypeConfiguration i następnie użyć do określenia typu relacji, które ta jednostka uczestniczy w HasRequired, HasOptional czy HasMany metody.</span><span class="sxs-lookup"><span data-stu-id="911f2-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="911f2-111">Metody HasRequired i HasOptional przyjmują wyrażenie lambda reprezentujące właściwość nawigacji odwołania.</span><span class="sxs-lookup"><span data-stu-id="911f2-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="911f2-112">Metoda HasMany pobiera Wyrażenie lambda reprezentujące właściwość nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="911f2-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="911f2-113">Następnie można skonfigurować właściwości nawigacji odwrotność przy użyciu metod WithRequired WithOptional i WithMany.</span><span class="sxs-lookup"><span data-stu-id="911f2-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="911f2-114">Metody te mają przeciążenia, które nie przyjmują argumentów i może służyć do określenia kardynalności z tego jednokierunkowe.</span><span class="sxs-lookup"><span data-stu-id="911f2-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="911f2-115">Następnie można skonfigurować właściwości klucza obcego przy użyciu metody HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="911f2-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="911f2-116">Ta metoda przyjmuje Wyrażenie lambda reprezentujące właściwość, która ma być używany jako klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="911f2-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="911f2-117">Konfigurowanie relacji wymagana do opcjonalne (jeden do — Zero lub jeden)</span><span class="sxs-lookup"><span data-stu-id="911f2-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="911f2-118">Poniższy przykład umożliwia skonfigurowanie relacji jeden do zero lub jeden.</span><span class="sxs-lookup"><span data-stu-id="911f2-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="911f2-119">OfficeAssignment ma właściwość InstructorID klucz podstawowy i klucz obcy, ponieważ nazwa właściwości nie jest zgodna z Konwencją, metoda HasKey jest używane do konfigurowania klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="911f2-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="911f2-120">Konfigurowanie relacji, w którym obu końcach są wymagane (jeden do jednego)</span><span class="sxs-lookup"><span data-stu-id="911f2-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="911f2-121">W większości przypadków Entity Framework można wywnioskować typu jest zależna od i która podmiotu zabezpieczeń w relacji.</span><span class="sxs-lookup"><span data-stu-id="911f2-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="911f2-122">Jednak gdy zarówno końców relacji są wymagane, lub obie strony są opcjonalne platformy Entity Framework nie może zidentyfikować zależnych od ustawień lokalnych i główną.</span><span class="sxs-lookup"><span data-stu-id="911f2-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="911f2-123">Gdy wymagane są oba końce relacji, należy użyć WithRequiredPrincipal lub WithRequiredDependent po metodzie HasRequired.</span><span class="sxs-lookup"><span data-stu-id="911f2-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="911f2-124">Po obu stronach relacji są opcjonalne, należy użyć WithOptionalPrincipal lub WithOptionalDependent po metodzie HasOptional.</span><span class="sxs-lookup"><span data-stu-id="911f2-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="911f2-125">Konfigurowanie relacji wiele do wielu</span><span class="sxs-lookup"><span data-stu-id="911f2-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="911f2-126">Poniższy kod służy do konfigurowania relacji wiele do wielu między typami kurs i instruktora.</span><span class="sxs-lookup"><span data-stu-id="911f2-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="911f2-127">W poniższym przykładzie domyślnych Konwencji Code First są używane do tworzenia tabelę sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="911f2-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="911f2-128">W wyniku tworzenia CourseInstructor tabeli z kolumnami Course_CourseID i Instructor_InstructorID.</span><span class="sxs-lookup"><span data-stu-id="911f2-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="911f2-129">Jeśli chcesz określić nazwę tabeli sprzężenia i nazwy kolumn w tabeli, należy wykonać dodatkowe czynności konfiguracyjne przy użyciu metody mapy.</span><span class="sxs-lookup"><span data-stu-id="911f2-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="911f2-130">Poniższy kod generuje CourseInstructor tabeli z kolumnami CourseID i InstructorID.</span><span class="sxs-lookup"><span data-stu-id="911f2-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="911f2-131">Konfigurowanie relacji z jedną właściwością nawigacji</span><span class="sxs-lookup"><span data-stu-id="911f2-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="911f2-132">(Nazywane również jednokierunkowe) jednokierunkową relacja jest, gdy właściwość nawigacji jest zdefiniowane tylko na jednym z końców relacji, a nie w obu.</span><span class="sxs-lookup"><span data-stu-id="911f2-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="911f2-133">Zgodnie z Konwencją Code First zawsze interpretuje jednokierunkowa relacja jako jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="911f2-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="911f2-134">Na przykład jeśli chcesz, aby bezpośredni związek pomiędzy instruktora oraz przypisane OfficeAssignment, w którym masz właściwości nawigacji na typie przez instruktorów, należy skonfigurować tę relację przy użyciu interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="911f2-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="911f2-135">Włączanie usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="911f2-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="911f2-136">Usuwanie kaskadowe na relacji można skonfigurować przy użyciu metody WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="911f2-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="911f2-137">Jeśli klucz obcy dla jednostki zależne nie dopuszcza wartości null, następnie Code First ustawia usuwanie kaskadowe relacji.</span><span class="sxs-lookup"><span data-stu-id="911f2-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="911f2-138">Jeśli klucz obcy dla jednostki zależne ma wartość null, Code First nie ustawia usuwanie kaskadowe relacji i gdy podmiot zabezpieczeń został usunięty klucz obcy zostanie ustawiona na wartość null.</span><span class="sxs-lookup"><span data-stu-id="911f2-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="911f2-139">Możesz usunąć tych konwencji usuwanie kaskadowe przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="911f2-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="911f2-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="911f2-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="911f2-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="911f2-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="911f2-142">Poniższy kod konfiguruje relację jako wymaganą, a następnie wyłącza usuwanie kaskadowe.</span><span class="sxs-lookup"><span data-stu-id="911f2-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="911f2-143">Konfigurowanie złożony klucz obcy</span><span class="sxs-lookup"><span data-stu-id="911f2-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="911f2-144">Jeśli klucz podstawowy dla typu dział obejmowało DepartmentID i nazwę właściwości, konfigurowania klucza podstawowego dla działu i klucz obcy dla typów kurs w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="911f2-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="911f2-145">Zmiana nazw klucz obcy, który nie jest zdefiniowany w modelu</span><span class="sxs-lookup"><span data-stu-id="911f2-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="911f2-146">Jeśli nie chcesz zdefiniować klucz obcy dla typu CLR, ale aby określić nazwę, jakie powinien mieć w bazie danych, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="911f2-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="911f2-147">Konfigurowanie nazwy klucza obcego, który nie jest zgodna z Konwencją pierwszy kodu</span><span class="sxs-lookup"><span data-stu-id="911f2-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="911f2-148">Właściwość klucza obcego w klasie kurs został wywołany SomeDepartmentID zamiast DepartmentID, należy wykonać następujące czynności, aby określić, że SomeDepartmentID jako klucza obcego:</span><span class="sxs-lookup"><span data-stu-id="911f2-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="911f2-149">Model używany w przykładach</span><span class="sxs-lookup"><span data-stu-id="911f2-149">Model Used in Samples</span></span>  

<span data-ttu-id="911f2-150">Poniższy model Code First jest używany dla przykładów na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="911f2-150">The following Code First model is used for the samples on this page.</span></span>  

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

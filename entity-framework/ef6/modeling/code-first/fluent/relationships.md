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
# <a name="fluent-api---relationships"></a><span data-ttu-id="f9abb-102">Interfejs API Fluent — relacje</span><span class="sxs-lookup"><span data-stu-id="f9abb-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="f9abb-103">Ta strona zawiera informacje dotyczące konfigurowania relacji w modelu Code First za pomocą interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f9abb-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="f9abb-104">Aby uzyskać ogólne informacje na temat relacji w EF oraz jak uzyskać dostęp do danych i manipulować nimi przy użyciu relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="f9abb-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="f9abb-105">Podczas pracy z Code First należy zdefiniować model, definiując klasy środowiska CLR domeny.</span><span class="sxs-lookup"><span data-stu-id="f9abb-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="f9abb-106">Domyślnie Entity Framework używa konwencji Code First do mapowania klas do schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f9abb-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="f9abb-107">Jeśli używasz konwencji nazewnictwa Code First, w większości przypadków można polegać na Code First w celu skonfigurowania relacji między tabelami w oparciu o klucze obce i właściwości nawigacji zdefiniowane dla klas.</span><span class="sxs-lookup"><span data-stu-id="f9abb-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="f9abb-108">Jeśli nie przestrzegasz konwencji podczas definiowania klas lub chcesz zmienić sposób działania Konwencji, możesz użyć interfejsu API Fluent lub adnotacji danych do skonfigurowania klas, aby Code First można było zmapować relacje między tabelami.</span><span class="sxs-lookup"><span data-stu-id="f9abb-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="f9abb-109">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="f9abb-109">Introduction</span></span>  

<span data-ttu-id="f9abb-110">Podczas konfigurowania relacji z interfejsem API Fluent należy rozpocząć od wystąpienia EntityTypeConfiguration, a następnie użyć metody HasRequired, HasOptional lub HasMany, aby określić typ relacji, w której uczestniczy ta jednostka.</span><span class="sxs-lookup"><span data-stu-id="f9abb-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="f9abb-111">Metody HasRequired i HasOptional przyjmują wyrażenie lambda reprezentujące właściwość nawigacji referencyjnej.</span><span class="sxs-lookup"><span data-stu-id="f9abb-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="f9abb-112">Metoda HasMany przyjmuje wyrażenie lambda reprezentujące właściwość nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="f9abb-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="f9abb-113">Następnie można skonfigurować właściwość nawigacji odwrotnej przy użyciu metod WithRequired, WithOptional i WithMany.</span><span class="sxs-lookup"><span data-stu-id="f9abb-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="f9abb-114">Metody te mają przeciążenia, które nie przyjmują argumentów i mogą być używane do określania kardynalności z jednokierunkowymi nawigacjami.</span><span class="sxs-lookup"><span data-stu-id="f9abb-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="f9abb-115">Następnie można skonfigurować właściwości klucza obcego przy użyciu metody HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="f9abb-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="f9abb-116">Ta metoda przyjmuje wyrażenie lambda reprezentujące właściwość, która ma być używana jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="f9abb-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="f9abb-117">Konfigurowanie relacji wymagane-do-opcjonalne (jeden-do-zero-lub-jednego)</span><span class="sxs-lookup"><span data-stu-id="f9abb-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="f9abb-118">Poniższy przykład służy do konfigurowania relacji jeden-do-zerowe-lub-jednego.</span><span class="sxs-lookup"><span data-stu-id="f9abb-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="f9abb-119">OfficeAssignment ma właściwość InstructorID, która jest kluczem podstawowym i kluczem obcym, ponieważ nazwa właściwości nie jest zgodna z Konwencją używaną przez metodę Haskey — do konfigurowania klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="f9abb-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="f9abb-120">Konfigurowanie relacji, w której oba punkty końcowe są wymagane (jeden do jednego)</span><span class="sxs-lookup"><span data-stu-id="f9abb-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="f9abb-121">W większości przypadków Entity Framework może wnioskować, który typ jest zależny i który jest podmiotem zabezpieczeń w relacji.</span><span class="sxs-lookup"><span data-stu-id="f9abb-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="f9abb-122">Jeśli jednak oba punkty końcowe relacji są wymagane lub obie strony są opcjonalne Entity Framework nie mogą identyfikować elementów zależnych i głównych.</span><span class="sxs-lookup"><span data-stu-id="f9abb-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="f9abb-123">Gdy wymagane są oba punkty końcowe relacji, użyj WithRequiredPrincipal lub WithRequiredDependent po metodzie HasRequired.</span><span class="sxs-lookup"><span data-stu-id="f9abb-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="f9abb-124">Gdy oba punkty końcowe relacji są opcjonalne, użyj WithOptionalPrincipal lub WithOptionalDependent po metodzie HasOptional.</span><span class="sxs-lookup"><span data-stu-id="f9abb-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="f9abb-125">Konfigurowanie relacji wiele-do-wielu</span><span class="sxs-lookup"><span data-stu-id="f9abb-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="f9abb-126">Poniższy kod służy do konfigurowania relacji wiele-do-wielu między kursem a typem instruktora.</span><span class="sxs-lookup"><span data-stu-id="f9abb-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="f9abb-127">W poniższym przykładzie domyślne konwencje Code First są używane do tworzenia tabeli sprzężeń.</span><span class="sxs-lookup"><span data-stu-id="f9abb-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="f9abb-128">W wyniku tego tworzona jest tabela CourseInstructor z kolumnami Course_CourseID i Instructor_InstructorID.</span><span class="sxs-lookup"><span data-stu-id="f9abb-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="f9abb-129">Jeśli chcesz określić nazwę tabeli sprzężenia i nazwy kolumn w tabeli, należy wykonać dodatkową konfigurację przy użyciu metody map.</span><span class="sxs-lookup"><span data-stu-id="f9abb-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="f9abb-130">Poniższy kod generuje tabelę CourseInstructor z kolumnami CourseID i InstructorID.</span><span class="sxs-lookup"><span data-stu-id="f9abb-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="f9abb-131">Konfigurowanie relacji z jedną właściwością nawigacji</span><span class="sxs-lookup"><span data-stu-id="f9abb-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="f9abb-132">Relacja jednokierunkowa (nazywana również jednokierunkowe) jest definiowana, gdy właściwość nawigacji jest zdefiniowana tylko dla jednej relacji, a nie na obu.</span><span class="sxs-lookup"><span data-stu-id="f9abb-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="f9abb-133">Zgodnie z Konwencją, Code First zawsze interpretuje dwukierunkową relację jako jeden-do-wielu.</span><span class="sxs-lookup"><span data-stu-id="f9abb-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="f9abb-134">Na przykład jeśli chcesz utworzyć relację jeden do jednego między instruktorem i OfficeAssignment, gdzie masz właściwość nawigacji tylko dla typu instruktora, musisz użyć interfejsu API Fluent, aby skonfigurować tę relację.</span><span class="sxs-lookup"><span data-stu-id="f9abb-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="f9abb-135">Włączanie usuwania kaskadowego</span><span class="sxs-lookup"><span data-stu-id="f9abb-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="f9abb-136">Można skonfigurować kaskadowe usuwanie dla relacji za pomocą metody WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="f9abb-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="f9abb-137">Jeśli klucz obcy jednostki zależnej nie dopuszcza wartości null, Code First ustawia kaskadowe usuwanie dla relacji.</span><span class="sxs-lookup"><span data-stu-id="f9abb-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="f9abb-138">Jeśli klucz obcy podmiotu zależnego ma wartość null, Code First nie ustawi kaskadowego usuwania dla relacji i po usunięciu podmiotu zabezpieczeń klucz obcy zostanie ustawiony na wartość null.</span><span class="sxs-lookup"><span data-stu-id="f9abb-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="f9abb-139">Te konwencje usuwania kaskadowego można usunąć za pomocą następujących funkcji:</span><span class="sxs-lookup"><span data-stu-id="f9abb-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="f9abb-140">Element modelbuilder. Conventions. Remove\<OneToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="f9abb-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="f9abb-141">Element modelbuilder. Conventions. Remove\<ManyToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="f9abb-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="f9abb-142">Poniższy kod służy do konfigurowania relacji, która ma być wymagana, a następnie wyłączenie usuwania kaskadowego.</span><span class="sxs-lookup"><span data-stu-id="f9abb-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="f9abb-143">Konfigurowanie złożonego klucza obcego</span><span class="sxs-lookup"><span data-stu-id="f9abb-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="f9abb-144">Jeśli klucz podstawowy dla typu działu składający się z właściwości DepartmentID i Name, należy skonfigurować klucz podstawowy dla działu i klucz obcy dla następujących typów kursów:</span><span class="sxs-lookup"><span data-stu-id="f9abb-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="f9abb-145">Zmiana nazwy klucza obcego, który nie jest zdefiniowany w modelu</span><span class="sxs-lookup"><span data-stu-id="f9abb-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="f9abb-146">Jeśli zdecydujesz się nie definiować klucza obcego w typie CLR, ale chcesz określić nazwę, którą powinna zawierać w bazie danych, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="f9abb-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="f9abb-147">Konfigurowanie nazwy klucza obcego, która nie jest zgodna z Konwencją Code First</span><span class="sxs-lookup"><span data-stu-id="f9abb-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="f9abb-148">Jeśli właściwość klucza obcego w klasie kursów została wywołana SomeDepartmentID zamiast DepartmentID, należy wykonać następujące czynności, aby określić, że SomeDepartmentID ma być kluczem obcym:</span><span class="sxs-lookup"><span data-stu-id="f9abb-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="f9abb-149">Model używany w przykładach</span><span class="sxs-lookup"><span data-stu-id="f9abb-149">Model Used in Samples</span></span>  

<span data-ttu-id="f9abb-150">Następujący model Code First jest używany dla przykładów na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="f9abb-150">The following Code First model is used for the samples on this page.</span></span>  

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

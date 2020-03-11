---
title: Interfejs API Fluent — Konfigurowanie i mapowanie właściwości i typów — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419067"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Interfejs API Fluent — Konfigurowanie i mapowanie właściwości i typów
Podczas pracy z Entity Framework Code First domyślnym zachowaniem jest mapowanie klas POCO na tabele przy użyciu zestawu Konwencji rozszerzania do EF. Czasami jednak nie ma potrzeby stosowania tych konwencji lub nie należy ich stosować do zamapowania jednostek na coś innego niż te, które są podyktowane przez konwencje.  

Istnieją dwa podstawowe sposoby konfigurowania EF do używania czegoś innego niż konwencji, czyli [adnotacji](~/ef6/modeling/code-first/data-annotations.md) lub interfejsu API Fluent systemu szyfrowania plików. Adnotacje obejmują tylko podzestaw funkcji interfejsu API Fluent, więc istnieją scenariusze mapowania, których nie można osiągnąć przy użyciu adnotacji. Ten artykuł został zaprojektowany z założeniami, aby zademonstrować, jak używać interfejsu API Fluent do konfigurowania właściwości.  

Najczęściej dostępnym interfejsem API Fluent kodu jest zastąpienie metody [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) w pochodnym [kontekście DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Poniższe przykłady zaprojektowano w celu pokazania, jak wykonywać różne zadania za pomocą interfejsu API Fluent oraz jak można skopiować kod i dostosować go do własnych potrzeb modelu, jeśli chcesz zobaczyć model, którego można używać z usługą AS, na końcu tego artykułu.  

## <a name="model-wide-settings"></a>Ustawienia całego modelu  

### <a name="default-schema-ef6-onwards"></a>Domyślny schemat (EF6 lub nowszy)  

Począwszy od EF6 można użyć metody HasDefaultSchema na DbModelBuilder, aby określić schemat bazy danych, który ma być używany dla wszystkich tabel, procedury składowane itd. To ustawienie domyślne zostanie przesłonięte dla wszystkich obiektów jawnie skonfigurowanych dla innego schematu.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Konwencje niestandardowe (EF6 lub nowszy)  

Począwszy od EF6, można utworzyć własne konwencje, aby uzupełnić te zawarte w Code First. Aby uzyskać więcej informacji, zobacz [konwencje Code First niestandardowych](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Odwzorowanie właściwości  

Metoda [Właściwości](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) służy do konfigurowania atrybutów dla każdej właściwości należącej do jednostki lub typu złożonego. Metoda Property służy do uzyskania obiektu konfiguracji dla danej właściwości. Opcje obiektu konfiguracji są specyficzne dla konfigurowanego typu; Isunicode jest dostępny tylko dla właściwości ciągu na przykład.  

### <a name="configuring-a-primary-key"></a>Konfigurowanie klucza podstawowego  

Entity Framework Konwencji dla kluczy podstawowych jest:  

1. Klasa definiuje właściwość, której nazwa to "ID" lub "ID"  
2. lub nazwa klasy, po której następuje wartość "ID" lub "ID"  

Aby jawnie ustawić właściwość jako klucz podstawowy, można użyć metody Haskey —. W poniższym przykładzie metoda Haskey — jest używana do konfigurowania klucza podstawowego InstructorID dla typu OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Konfigurowanie złożonego klucza podstawowego  

Poniższy przykład konfiguruje właściwości DepartmentID i Name w postaci złożonego klucza podstawowego typu działu.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Wyłączanie tożsamości dla numerycznych kluczy podstawowych  

Poniższy przykład ustawia właściwość DepartmentID na system. ComponentModel. DataAnnotations. DatabaseGeneratedOption. None, aby wskazać, że wartość nie zostanie wygenerowana przez bazę danych.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Określanie maksymalnej długości właściwości  

W poniższym przykładzie właściwość Name nie może być dłuższa niż 50 znaków. Jeśli wartość jest dłuższa niż 50 znaków, zostanie wyświetlony wyjątek [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) . Jeśli Code First tworzy bazę danych z tego modelu, ustawi również maksymalną długość kolumny Nazwa na 50 znaków.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Konfigurowanie właściwości, która ma być wymagana  

W poniższym przykładzie właściwość Name jest wymagana. Jeśli nazwa nie zostanie określona, zostanie wyświetlony wyjątek DbEntityValidationException. Jeśli Code First tworzy bazę danych z tego modelu, kolumna użyta do przechowania tej właściwości zazwyczaj nie dopuszcza wartości null.  

> [!NOTE]
> W niektórych przypadkach może nie być możliwe, aby kolumna w bazie danych nie mogła dopuszczać wartości null, mimo że właściwość jest wymagana. Na przykład w przypadku korzystania z danych strategii dziedziczenia TPH dla wielu typów jest przechowywany w jednej tabeli. Jeśli typ pochodny zawiera wymaganą właściwość, nie można wprowadzić wartości null w kolumnie, ponieważ nie wszystkie typy w hierarchii będą mieć tę właściwość.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Konfigurowanie indeksu dla jednej lub wielu właściwości  

> [!NOTE]
> **Dr 6.1 tylko** -atrybut indeksu został wprowadzony w Entity Framework 6,1. Jeśli używasz wcześniejszej wersji, informacje przedstawione w tej sekcji nie są stosowane.  

Tworzenie indeksów nie jest natywnie obsługiwane przez interfejs API Fluent, ale można korzystać z pomocy technicznej dla elementu **indexattribute** za pośrednictwem interfejsu API Fluent. Atrybuty indeksu są przetwarzane przez dołączenie do modelu adnotacji modelu, która następnie jest później przekształcana w indeks w bazie danych w potoku. Można ręcznie dodać te same adnotacje przy użyciu interfejsu API Fluent.  

Najprostszym sposobem jest utworzenie wystąpienia **indexattribute** , które zawiera wszystkie ustawienia dla nowego indeksu. Następnie można utworzyć wystąpienie klasy **IndexAnnotation** , która jest typem określonym przez EF, który przekonwertuje ustawienia **indexattribute** na adnotację modelu, która może być przechowywana w modelu EF. Można je następnie przesłać do metody **HasColumnAnnotation** w interfejsie API Fluent, określając **indeks** nazwy adnotacji.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Aby zapoznać się z pełną listą ustawień dostępnych w **indeksie indexattribute**, zapoznaj się z sekcją *indeks* w obszarze [Code First adnotacje danych](~/ef6/modeling/code-first/data-annotations.md). Dotyczy to również dostosowywania nazwy indeksu, tworzenia unikatowych indeksów i tworzenia indeksów obejmujących wiele kolumn.  

Można określić wiele adnotacji indeksów dla pojedynczej właściwości, przekazując tablicę **indexattribute** do konstruktora **IndexAnnotation**.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Określanie, aby nie mapować właściwości CLR do kolumny w bazie danych  

Poniższy przykład pokazuje, jak określić, że właściwość typu CLR nie jest zamapowana na kolumnę w bazie danych.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mapowanie właściwości CLR do określonej kolumny w bazie danych  

Poniższy przykład mapuje nazwę właściwości CLR do kolumny bazy danych Departmentname.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Zmiana nazwy klucza obcego, który nie jest zdefiniowany w modelu  

Jeśli zdecydujesz się nie definiować klucza obcego w typie CLR, ale chcesz określić nazwę, którą powinna zawierać w bazie danych, wykonaj następujące czynności:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Konfigurowanie, czy właściwość String obsługuje zawartość Unicode  

Domyślnie ciągi są w formacie Unicode (nvarchar w SQL Server). Można użyć metody isunicode, aby określić, że ciąg powinien być typu varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Konfigurowanie typu danych kolumny bazy danych  

Metoda [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) umożliwia mapowanie do różnych reprezentacji tego samego typu podstawowego. Użycie tej metody nie pozwala na wykonywanie konwersji danych w czasie wykonywania. Należy pamiętać, że funkcja isunicode jest preferowanym sposobem ustawiania kolumn na varchar, ponieważ jest to baza danych niezależny od.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Konfigurowanie właściwości typu złożonego  

Istnieją dwa sposoby konfigurowania właściwości skalarnych w typie złożonym.  

Możesz wywołać właściwość w ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Można również użyć notacji kropka, aby uzyskać dostęp do właściwości typu złożonego.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Konfigurowanie właściwości, która ma być używana jako optymistyczny Token współbieżności  

Aby określić, że właściwość w jednostce reprezentuje Token współbieżności, można użyć atrybutu ConcurrencyCheck lub metody IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Można również użyć metody IsRowVersion, aby skonfigurować właściwość jako wersję wiersza w bazie danych. Ustawienie właściwości jako wersji wiersza automatycznie konfiguruje ją jako optymistyczny Token współbieżności.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mapowanie typu  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Określanie, że Klasa jest typu złożonego  

Zgodnie z Konwencją typ, który nie ma określonego klucza podstawowego, jest traktowany jako typ złożony. Istnieją scenariusze, w których Code First nie wykrywa typu złożonego (na przykład jeśli masz właściwość o nazwie ID, ale nie ma znaczenia, że jest to klucz podstawowy). W takich przypadkach można użyć interfejsu API Fluent, aby jawnie określić, że typ jest typem złożonym.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Określanie, aby nie mapować typu jednostki CLR na tabelę w bazie danych  

Poniższy przykład pokazuje, jak wykluczyć typ CLR z mapowania do tabeli w bazie danych.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapowanie typu jednostki na określoną tabelę w bazie danych  

Wszystkie właściwości działu zostaną zamapowane na kolumny w tabeli o nazwie dział t_.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Można również określić nazwę schematu w następujący sposób:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mapowanie dziedziczenia tabeli na hierarchię (TPH)  

W scenariuszu mapowania TPH wszystkie typy w hierarchii dziedziczenia są mapowane na pojedynczą tabelę. Kolumna rozróżniacza służy do identyfikowania typu każdego wiersza. Podczas tworzenia modelu przy użyciu Code First TPH jest domyślną strategią dla typów, które uczestniczą w hierarchii dziedziczenia. Domyślnie kolumna rozróżniacz jest dodawana do tabeli o nazwie "rozróżniacz", a nazwa typu CLR każdego typu w hierarchii jest używana dla wartości rozróżniacza. Domyślne zachowanie można zmienić za pomocą interfejsu API Fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapowanie dziedziczenia według typu tabeli (TPT)  

W scenariuszu mapowania TPT wszystkie typy są mapowane na poszczególne tabele. Właściwości, które należą wyłącznie do typu podstawowego lub typu pochodnego, są przechowywane w tabeli, która jest mapowana na ten typ. Tabele mapowane na typy pochodne również przechowują klucz obcy, który łączy tabelę pochodną z tabelą bazową.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapowanie dziedziczenia klasy opartej na tabeli (TPC)  

W scenariuszu mapowania TPC wszystkie typy nieabstrakcyjne w hierarchii są mapowane do poszczególnych tabel. Tabele mapowane na klasy pochodne nie mają relacji z tabelą, która jest mapowana na klasę bazową w bazie danych. Wszystkie właściwości klasy, łącznie z dziedziczonymi właściwościami, są mapowane na kolumny odpowiadającej tabeli.  

Wywołaj metodę MapInheritedProperties, aby skonfigurować każdy typ pochodny. MapInheritedProperties ponownie mapuje wszystkie właściwości, które były dziedziczone z klasy bazowej na nowe kolumny w tabeli dla klasy pochodnej.  

> [!NOTE]
> Należy zauważyć, że ponieważ tabele uczestniczące w hierarchii dziedziczenia TPC nie współdzielą klucza podstawowego, podczas wstawiania w tabelach, które są mapowane na podklasy, nie są dostępne klucze jednostkowe, jeśli masz wartości wygenerowane przez bazę danych z tym samym inicjatorem tożsamości. Aby rozwiązać ten problem, możesz określić inną początkową wartość inicjatora dla każdej tabeli lub wyłączyć tożsamość we właściwości klucza podstawowego. Tożsamość jest wartością domyślną dla właściwości klucza Integer podczas pracy z Code First.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapowanie właściwości typu jednostki na wiele tabel w bazie danych (podział jednostki)  

Dzielenie jednostek umożliwia rozproszenie właściwości typu jednostki między wieloma tabelami. W poniższym przykładzie jednostka działu jest dzielona na dwie tabele: Department i DepartmentDetails. Dzielenie jednostek używa wielu wywołań metody map do mapowania podzestawu właściwości do konkretnej tabeli.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mapowanie wielu typów jednostek na jedną tabelę w bazie danych (podział tabeli)  

Poniższy przykład mapuje dwa typy jednostek, które współużytkują klucz podstawowy z jedną tabelą.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapowanie typu jednostki, aby wstawiać/aktualizować/usuwać procedury składowane (EF6 lub nowszy)  

Począwszy od EF6 można zmapować jednostki, aby używać procedur składowanych do wstawiania aktualizacji i usuwania. Aby uzyskać więcej informacji, zobacz [Code First wstawiania/aktualizowania/usuwania procedur składowanych](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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

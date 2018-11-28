---
title: Interfejs Fluent API — Konfigurowanie i mapowania właściwości i typy — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: 75f8a179ac9a70ad390fc7ab2a6c5e714e701b8b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339806"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Interfejs API Fluent — Konfigurowanie i mapowania właściwości i typy
Podczas pracy z programu Entity Framework Code First to zachowanie domyślne jest do mapowania klas usługi POCO tabel przy użyciu zestawu konwencje wbudowanymi do programów EF. Czasami jednak użytkownik nie może lub nie powinny być zgodne z konwencjami te i zamapować jednostek na coś innego niż co zgodnie z konwencjami.  

Istnieją dwa główne sposoby, można skonfigurować EF, aby użyć coś innego niż konwencje, to znaczy [adnotacje](~/ef6/modeling/code-first/data-annotations.md) lub interfejs fluent API w systemu szyfrowania plików. Adnotacje dotyczą tylko podzbiór funkcji interfejsu API fluent, więc istnieją scenariusze mapowania, które nie mogą być osiągnięte korzystanie z adnotacji. Ten artykuł jest przeznaczony do pokazują, jak skonfigurować właściwości za pomocą interfejsu API fluent.  

Kod pierwszego interfejsu API fluent najczęściej odbywa się przez zastąpienie [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) metody w swojej pochodnej [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Poniższe przykłady są przeznaczone do przedstawiają sposób wykonywania różnych zadań przy użyciu wygodnego interfejsu api i umożliwiają skopiowania kod i dostosować go do potrzeb Twojego modelu, jeśli chcesz zobaczyć model, który może być używany z jako — jest to znajduje się na końcu tego artykułu.  

## <a name="model-wide-settings"></a>Ustawienia dla całego modelu  

### <a name="default-schema-ef6-onwards"></a>Schemat domyślny (od wersji EF6)  

Uruchamianie platformy EF6 umożliwia metoda HasDefaultSchema na DbModelBuilder Określ schemat bazy danych do użycia dla wszystkich tabel, procedur składowanych, itp. To ustawienie domyślne, zostaną zastąpione dla obiektów, które jawnie skonfigurujesz inny schemat.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Konwencje niestandardowe (od wersji EF6)  

Począwszy od tworzenia własnych Konwencji odpowiadającym, aby uzupełnić te dołączone w Code First platformy EF6. Aby uzyskać więcej informacji, zobacz [konwencje pierwszy kod niestandardowy](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mapowanie właściwości  

[Właściwość](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) metoda służy do konfigurowania atrybutów dla każdej właściwości należących do jednostki lub typ złożony. Metoda właściwość jest używana do uzyskiwania obiektu konfiguracji dla danej właściwości. Opcje na obiekt konfiguracji są specyficzne dla typu skonfigurowana; Na przykład jest dostępne tylko na właściwości parametrów IsUnicode.  

### <a name="configuring-a-primary-key"></a>Konfigurowanie klucza podstawowego  

Konwencja platformy Entity Framework, kluczy podstawowych jest:  

1. Klasa definiuje właściwość, której nazwa to "ID" lub "Id"  
2. lub nazwa klasy, po której następuje "ID" lub "Id"  

Aby jawnie ustawić właściwość, która ma być kluczem podstawowym, można użyć metody HasKey. W poniższym przykładzie metoda HasKey służy do konfigurowania InstructorID klucz podstawowy dla typu OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Konfigurowanie złożony klucz podstawowy  

Poniższy przykład umożliwia skonfigurowanie DepartmentID i nazwy właściwości na złożony klucz podstawowy typu działu.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Wyłączanie tożsamości liczbowych kluczy podstawowych  

Poniższy przykład ustawia właściwość DepartmentID System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None, aby wskazać, że wartość nie zostanie wygenerowany przez bazę danych.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Określanie maksymalnego we właściwości  

W poniższym przykładzie nazwa właściwości, należy nie może przekraczać 50 znaków. Jeśli wprowadzisz wartość, która jest dłuższa niż 50 znaków, otrzymasz [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) wyjątku. Jeśli Code First tworzy bazę danych z tego modelu do 50 znaków zostanie ustawiony także maksymalna długość nazwy kolumny.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Konfigurowanie wymaganych właściwości  

W poniższym przykładzie właściwość Name jest wymagany. Jeśli nie określisz nazwy, wystąpi wyjątek DbEntityValidationException. Jeśli Code First tworzy bazę danych z tego modelu kolumny używane do przechowywania tej właściwości zwykle będzie innych niż null.  

> [!NOTE]
> W niektórych przypadkach może nie być możliwe dla kolumny w bazie danych, może nie dopuszczać wartości null, nawet jeśli właściwość jest wymagana. Na przykład, gdy przy użyciu danych strategii TPH dziedziczenia dla wielu typów są przechowywane w jednej tabeli. Jeśli typ pochodny obejmuje wymagana właściwość kolumny nie można dokonać innych niż null, ponieważ nie wszystkie typy w hierarchii mają ta właściwość.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Konfigurowanie indeksu na co najmniej jednej właściwości  

> [!NOTE]
> **EF6.1 począwszy tylko** -atrybutu indeksu została wprowadzona w programie Entity Framework 6.1. Informacje przedstawione w tej sekcji nie ma zastosowania, jeśli używasz starszej wersji.  

Tworzenie indeksów nie jest natywnie obsługiwane przez interfejs Fluent API, ale umożliwia korzystanie z pomocy technicznej dla **IndexAttribute** za pośrednictwem interfejsu API Fluent. Atrybuty indeksu są przetwarzane przez dołączenie adnotacją modelu na model, który następnie jest przekształcane w indeksu w bazie danych później w potoku. Można ręcznie dodać te same adnotacje przy użyciu interfejsu API Fluent.  

W tym celu najłatwiej można utworzyć wystąpienia **IndexAttribute** zawierający wszystkie ustawienia dla nowego indeksu. Następnie można utworzyć wystąpienia **IndexAnnotation** czyli EF określonego typu który przekonwertuje **IndexAttribute** ustawienia do adnotacji modelu, które mogą być przechowywane w modelu platformy EF. Te mogą być następnie przekazywany do **HasColumnAnnotation** metody w interfejsie API Fluent, określając nazwę **indeksu** dla adnotacji.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Aby uzyskać pełną listę ustawień dostępnych w **IndexAttribute**, zobacz *indeksu* części [adnotacje danych na pierwszym kodu](~/ef6/modeling/code-first/data-annotations.md). W tym dostosowywania nazwę indeksu, tworzenie indeksów unikatowych i tworzenie indeksów wielokolumnowych.  

Można określić wiele adnotacji indeksu na jedną właściwość, przekazując tablicę **IndexAttribute** do konstruktora **IndexAnnotation**.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Określanie nie można zamapować właściwość CLR z kolumną w bazie danych  

Poniższy przykład pokazuje, jak określić, czy właściwość na typ CLR nie została zamapowana na kolumnę w bazie danych.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mapowanie właściwości CLR do określonej kolumny w bazie danych  

Poniższy przykład mapuje właściwość CLR nazwę kolumny bazy danych DepartmentName.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Zmiana nazw klucz obcy, który nie jest zdefiniowany w modelu  

Jeśli nie chcesz zdefiniować klucz obcy dla typu CLR, ale aby określić nazwę, jakie powinien mieć w bazie danych, wykonaj następujące czynności:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Skonfigurowanie, czy właściwość ciągu obsługuje zawartość Unicode  

Domyślnie ciągi są Unicode (nvarchar w programie SQL Server). Metoda IsUnicode służy do określenia, czy ciąg powinien być typu varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Konfigurowanie typu danych kolumny bazy danych  

[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) metoda umożliwia mapowanie na różne reprezentacje tego samego typu podstawowego. Przy użyciu tej metody nie włącza wykonywania konwersji danych w czasie wykonywania. Należy pamiętać, że IsUnicode preferowanym sposobem tworzenia kolumn ustawienie varchar, ponieważ jest niezależny od bazy danych.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Konfigurowanie właściwości typu złożonego  

Istnieją dwa sposoby konfigurowania właściwości skalarne w typie złożonym.  

Właściwości można wywołać w ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Zapisu kropkowego umożliwia również dostęp do właściwości typu złożonego.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Konfigurowanie właściwości, aby służyć jako Token optymistycznej współbieżności  

Aby określić, że właściwości w obiekcie reprezentuje tokenem współbieżności, można użyć atrybutu ConcurrencyCheck lub metoda IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Metoda IsRowVersion służy również do skonfigurowania właściwości, aby być w wersji wierszy w bazie danych. Ustawianie właściwości jako wersja wiersza automatycznie skonfiguruje je jako token optymistycznej współbieżności.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mapowanie typu  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Określanie, czy klasa jest typem złożonym  

Zgodnie z Konwencją typ, który ma określony klucz podstawowy jest traktowany jako typ złożony. Istnieją sytuacje, w którym Code First nie wykrywa typ złożony (na przykład, jeśli użytkownik ma właściwość o nazwie identyfikator, ale nie oznaczają, aby mogła być kluczem podstawowym). W takich przypadkach należy użyć interfejsu API fluent jawnie określić, że typ jest typem złożonym.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Określanie nie można zamapować typ CLR jednostki do tabeli w bazie danych  

Poniższy przykład pokazuje, jak wykluczyć typu CLR z mapowany do tabeli w bazie danych.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapowania typ jednostki do określonej tabeli w bazie danych  

Wszystkie właściwości działu zostanie zamapowane do kolumn w tabeli o nazwie t_ działu.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Można również określić nazwę schematu następująco:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mapowanie dziedziczenia tabela wg hierarchii (TPH)  

W tym scenariuszu mapowania TPH wszystkich typów w hierarchii dziedziczenia są mapowane na pojedynczą tabelę. Kolumna dyskryminatora jest używany do identyfikowania typu każdego wiersza. Podczas tworzenia modelu przy użyciu Code First, TPH jest strategia domyślna dla typów, które uczestniczą w hierarchii dziedziczenia. Domyślnie kolumna dyskryminatora zostanie dodana do tabeli o nazwie "Dyskryminatora" i nazwę typu CLR poszczególnych typów w hierarchii jest używany dla wartości dyskryminatora. Zachowanie domyślne można modyfikować za pomocą interfejsu API fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapowanie dziedziczenia tabela wg typu (TPT)  

W tym scenariuszu mapowania TPT wszystkie typy są mapowane na poszczególnych tabel. Właściwości, które należą wyłącznie do typu podstawowego lub typu pochodnego są przechowywane w tabeli, która mapuje do tego typu. Tabele mapowane na typy pochodne również przechowywać klucz obcy, który tworzy sprzężenie tabeli pochodnej z tabeli podstawowej.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapowanie dziedziczenia klasy tabeli na konkretny (TPC)  

W tym scenariuszu mapowania TPC wszystkie typy nieabstrakcyjnej w hierarchii są mapowane na poszczególnych tabel. Tabele, które mapują do klas pochodnych nie mają relacji do tabeli, która mapuje do klasy bazowej w bazie danych. Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli.  

Wywołaj metodę MapInheritedProperties, aby skonfigurować każdego typu pochodnego. MapInheritedProperties ponownie mapuje wszystkie właściwości, które były dziedziczone z klasy bazowej do nowych kolumn w tabeli dla klasy pochodnej.  

> [!NOTE]
> Należy pamiętać, że ponieważ nie mają tabele uczestniczących w hierarchii dziedziczenia TPC klucz podstawowy będzie jednostki zduplikowane klucze podczas wstawiania do tabel, które są mapowane do podklasy, jeśli masz wartości bazy danych, wygenerowane za pomocą tego samego Inicjator właściwości identity. Aby rozwiązać ten problem, możesz określić wartość różnych inicjatora początkowej dla każdej tabeli lub wyłączyć tożsamość na właściwość klucza podstawowego. Tożsamość jest wartością domyślną dla właściwości klucza liczby całkowitej, podczas pracy z usługą Code First.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapowanie właściwości typu jednostki z wieloma tabelami w bazie danych (jednostka dzielenie)  

Jednostki podział umożliwia właściwości typu jednostki, aby być rozkładane na wiele tabel. W poniższym przykładzie jednostki dział zostanie podzielona na dwie tabele: dział i DepartmentDetails. Podział jednostki używa wielu wywołań metody mapy do mapowania podzbiór właściwości do określonej tabeli.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mapowanie typów jednostek do jednej tabeli w bazie danych (tabeli dzielenie)  

Poniższy przykład mapuje dwóch typów jednostek, które mają klucz podstawowy do jednej tabeli.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapowanie typu jednostki do wstawiania/aktualizowania/usuwania procedur składowanych (od wersji EF6)  

Uruchamianie platformy EF6 można mapować jednostki używanie procedur składowanych do wstawiania, aktualizacji i usuwania. Aby uzyskać więcej informacji, zobacz [kodu pierwszy wstawiania/aktualizowania/usuwania procedur składowanych](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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

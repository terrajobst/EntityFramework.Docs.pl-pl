---
title: Interfejs Fluent API — Konfigurowanie i mapowania właściwości i typy — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 031376d2fc4778e6f0fa2434ab7ccfd45d436c4a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490203"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="996fc-102">Interfejs API Fluent — Konfigurowanie i mapowania właściwości i typy</span><span class="sxs-lookup"><span data-stu-id="996fc-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="996fc-103">Podczas pracy z programu Entity Framework Code First to zachowanie domyślne jest do mapowania klas usługi POCO tabel przy użyciu zestawu konwencje wbudowanymi do programów EF.</span><span class="sxs-lookup"><span data-stu-id="996fc-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="996fc-104">Czasami jednak użytkownik nie może lub nie powinny być zgodne z konwencjami te i zamapować jednostek na coś innego niż co zgodnie z konwencjami.</span><span class="sxs-lookup"><span data-stu-id="996fc-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="996fc-105">Istnieją dwa główne sposoby, można skonfigurować EF, aby użyć coś innego niż konwencje, to znaczy [adnotacje](~/ef6/modeling/code-first/data-annotations.md) lub interfejs fluent API w systemu szyfrowania plików.</span><span class="sxs-lookup"><span data-stu-id="996fc-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="996fc-106">Adnotacje dotyczą tylko podzbiór funkcji interfejsu API fluent, więc istnieją scenariusze mapowania, które nie mogą być osiągnięte korzystanie z adnotacji.</span><span class="sxs-lookup"><span data-stu-id="996fc-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="996fc-107">Ten artykuł jest przeznaczony do pokazują, jak skonfigurować właściwości za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="996fc-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="996fc-108">Kod pierwszego interfejsu API fluent najczęściej odbywa się przez zastąpienie [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) metody w swojej pochodnej [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="996fc-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="996fc-109">Poniższe przykłady są przeznaczone do przedstawiają sposób wykonywania różnych zadań przy użyciu wygodnego interfejsu api i umożliwiają skopiowania kod i dostosować go do potrzeb Twojego modelu, jeśli chcesz zobaczyć model, który może być używany z jako — jest to znajduje się na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="996fc-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="996fc-110">Ustawienia dla całego modelu</span><span class="sxs-lookup"><span data-stu-id="996fc-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="996fc-111">Schemat domyślny (od wersji EF6)</span><span class="sxs-lookup"><span data-stu-id="996fc-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="996fc-112">Uruchamianie platformy EF6 umożliwia metoda HasDefaultSchema na DbModelBuilder Określ schemat bazy danych do użycia dla wszystkich tabel, procedur składowanych, itp. To ustawienie domyślne, zostaną zastąpione dla obiektów, które jawnie skonfigurujesz inny schemat.</span><span class="sxs-lookup"><span data-stu-id="996fc-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="996fc-113">Konwencje niestandardowe (od wersji EF6)</span><span class="sxs-lookup"><span data-stu-id="996fc-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="996fc-114">Począwszy od tworzenia własnych Konwencji odpowiadającym, aby uzupełnić te dołączone w Code First platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="996fc-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="996fc-115">Aby uzyskać więcej informacji, zobacz [konwencje pierwszy kod niestandardowy](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="996fc-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="996fc-116">Mapowanie właściwości</span><span class="sxs-lookup"><span data-stu-id="996fc-116">Property Mapping</span></span>  

<span data-ttu-id="996fc-117">[Właściwość](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) metoda służy do konfigurowania atrybutów dla każdej właściwości należących do jednostki lub typ złożony.</span><span class="sxs-lookup"><span data-stu-id="996fc-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="996fc-118">Metoda właściwość jest używana do uzyskiwania obiektu konfiguracji dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="996fc-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="996fc-119">Opcje na obiekt konfiguracji są specyficzne dla typu skonfigurowana; Na przykład jest dostępne tylko na właściwości parametrów IsUnicode.</span><span class="sxs-lookup"><span data-stu-id="996fc-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="996fc-120">Konfigurowanie klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="996fc-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="996fc-121">Konwencja platformy Entity Framework, kluczy podstawowych jest:</span><span class="sxs-lookup"><span data-stu-id="996fc-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="996fc-122">Klasa definiuje właściwość, której nazwa to "ID" lub "Id"</span><span class="sxs-lookup"><span data-stu-id="996fc-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="996fc-123">lub nazwa klasy, po której następuje "ID" lub "Id"</span><span class="sxs-lookup"><span data-stu-id="996fc-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="996fc-124">Aby jawnie ustawić właściwość, która ma być kluczem podstawowym, można użyć metody HasKey.</span><span class="sxs-lookup"><span data-stu-id="996fc-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="996fc-125">W poniższym przykładzie metoda HasKey służy do konfigurowania InstructorID klucz podstawowy dla typu OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="996fc-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="996fc-126">Konfigurowanie złożony klucz podstawowy</span><span class="sxs-lookup"><span data-stu-id="996fc-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="996fc-127">Poniższy przykład umożliwia skonfigurowanie DepartmentID i nazwy właściwości na złożony klucz podstawowy typu działu.</span><span class="sxs-lookup"><span data-stu-id="996fc-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="996fc-128">Wyłączanie tożsamości liczbowych kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="996fc-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="996fc-129">Poniższy przykład ustawia właściwość DepartmentID System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None, aby wskazać, że wartość nie zostanie wygenerowany przez bazę danych.</span><span class="sxs-lookup"><span data-stu-id="996fc-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="996fc-130">Określanie maksymalnego we właściwości</span><span class="sxs-lookup"><span data-stu-id="996fc-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="996fc-131">W poniższym przykładzie nazwa właściwości, należy nie może przekraczać 50 znaków.</span><span class="sxs-lookup"><span data-stu-id="996fc-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="996fc-132">Jeśli wprowadzisz wartość, która jest dłuższa niż 50 znaków, otrzymasz [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) wyjątku.</span><span class="sxs-lookup"><span data-stu-id="996fc-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="996fc-133">Jeśli Code First tworzy bazę danych z tego modelu do 50 znaków zostanie ustawiony także maksymalna długość nazwy kolumny.</span><span class="sxs-lookup"><span data-stu-id="996fc-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="996fc-134">Konfigurowanie wymaganych właściwości</span><span class="sxs-lookup"><span data-stu-id="996fc-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="996fc-135">W poniższym przykładzie właściwość Name jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="996fc-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="996fc-136">Jeśli nie określisz nazwy, wystąpi wyjątek DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="996fc-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="996fc-137">Jeśli Code First tworzy bazę danych z tego modelu kolumny używane do przechowywania tej właściwości zwykle będzie innych niż null.</span><span class="sxs-lookup"><span data-stu-id="996fc-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="996fc-138">W niektórych przypadkach może nie być możliwe dla kolumny w bazie danych, może nie dopuszczać wartości null, nawet jeśli właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="996fc-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="996fc-139">Na przykład, gdy przy użyciu danych strategii TPH dziedziczenia dla wielu typów są przechowywane w jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="996fc-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="996fc-140">Jeśli typ pochodny obejmuje wymagana właściwość kolumny nie można dokonać innych niż null, ponieważ nie wszystkie typy w hierarchii mają ta właściwość.</span><span class="sxs-lookup"><span data-stu-id="996fc-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="996fc-141">Konfigurowanie indeksu na co najmniej jednej właściwości</span><span class="sxs-lookup"><span data-stu-id="996fc-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="996fc-142">**EF6.1 począwszy tylko** -atrybutu indeksu została wprowadzona w programie Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="996fc-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="996fc-143">Informacje przedstawione w tej sekcji nie ma zastosowania, jeśli używasz starszej wersji.</span><span class="sxs-lookup"><span data-stu-id="996fc-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="996fc-144">Tworzenie indeksów nie jest natywnie obsługiwane przez interfejs Fluent API, ale umożliwia korzystanie z pomocy technicznej dla **IndexAttribute** za pośrednictwem interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="996fc-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="996fc-145">Atrybuty indeksu są przetwarzane przez dołączenie adnotacją modelu na model, który następnie jest przekształcane w indeksu w bazie danych później w potoku.</span><span class="sxs-lookup"><span data-stu-id="996fc-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="996fc-146">Można ręcznie dodać te same adnotacje przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="996fc-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="996fc-147">W tym celu najłatwiej można utworzyć wystąpienia **IndexAttribute** zawierający wszystkie ustawienia dla nowego indeksu.</span><span class="sxs-lookup"><span data-stu-id="996fc-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="996fc-148">Następnie można utworzyć wystąpienia **IndexAnnotation** czyli EF określonego typu który przekonwertuje **IndexAttribute** ustawienia do adnotacji modelu, które mogą być przechowywane w modelu platformy EF.</span><span class="sxs-lookup"><span data-stu-id="996fc-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="996fc-149">Te mogą być następnie przekazywany do **HasColumnAnnotation** metody w interfejsie API Fluent, określając nazwę **indeksu** dla adnotacji.</span><span class="sxs-lookup"><span data-stu-id="996fc-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="996fc-150">Aby uzyskać pełną listę ustawień dostępnych w **IndexAttribute**, zobacz *indeksu* części [adnotacje danych na pierwszym kodu](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="996fc-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="996fc-151">W tym dostosowywania nazwę indeksu, tworzenie indeksów unikatowych i tworzenie indeksów wielokolumnowych.</span><span class="sxs-lookup"><span data-stu-id="996fc-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="996fc-152">Można określić wiele adnotacji indeksu na jedną właściwość, przekazując tablicę **IndexAttribute** do konstruktora **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="996fc-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="996fc-153">Określanie nie można zamapować właściwość CLR z kolumną w bazie danych</span><span class="sxs-lookup"><span data-stu-id="996fc-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="996fc-154">Poniższy przykład pokazuje, jak określić, czy właściwość na typ CLR nie została zamapowana na kolumnę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="996fc-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="996fc-155">Mapowanie właściwości CLR do określonej kolumny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="996fc-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="996fc-156">Poniższy przykład mapuje właściwość CLR nazwę kolumny bazy danych DepartmentName.</span><span class="sxs-lookup"><span data-stu-id="996fc-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="996fc-157">Zmiana nazw klucz obcy, który nie jest zdefiniowany w modelu</span><span class="sxs-lookup"><span data-stu-id="996fc-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="996fc-158">Jeśli nie chcesz zdefiniować klucz obcy dla typu CLR, ale aby określić nazwę, jakie powinien mieć w bazie danych, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="996fc-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="996fc-159">Skonfigurowanie, czy właściwość ciągu obsługuje zawartość Unicode</span><span class="sxs-lookup"><span data-stu-id="996fc-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="996fc-160">Domyślnie ciągi są Unicode (nvarchar w programie SQL Server).</span><span class="sxs-lookup"><span data-stu-id="996fc-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="996fc-161">Metoda IsUnicode służy do określenia, czy ciąg powinien być typu varchar.</span><span class="sxs-lookup"><span data-stu-id="996fc-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="996fc-162">Konfigurowanie typu danych kolumny bazy danych</span><span class="sxs-lookup"><span data-stu-id="996fc-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="996fc-163">[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) metoda umożliwia mapowanie na różne reprezentacje tego samego typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="996fc-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="996fc-164">Przy użyciu tej metody nie włącza wykonywania konwersji danych w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="996fc-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="996fc-165">Należy pamiętać, że IsUnicode preferowanym sposobem tworzenia kolumn ustawienie varchar, ponieważ jest niezależny od bazy danych.</span><span class="sxs-lookup"><span data-stu-id="996fc-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="996fc-166">Konfigurowanie właściwości typu złożonego</span><span class="sxs-lookup"><span data-stu-id="996fc-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="996fc-167">Istnieją dwa sposoby konfigurowania właściwości skalarne w typie złożonym.</span><span class="sxs-lookup"><span data-stu-id="996fc-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="996fc-168">Właściwości można wywołać w ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="996fc-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="996fc-169">Zapisu kropkowego umożliwia również dostęp do właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="996fc-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="996fc-170">Konfigurowanie właściwości, aby służyć jako Token optymistycznej współbieżności</span><span class="sxs-lookup"><span data-stu-id="996fc-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="996fc-171">Aby określić, że właściwości w obiekcie reprezentuje tokenem współbieżności, można użyć atrybutu ConcurrencyCheck lub metoda IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="996fc-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="996fc-172">Metoda IsRowVersion służy również do skonfigurowania właściwości, aby być w wersji wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="996fc-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="996fc-173">Ustawianie właściwości jako wersja wiersza automatycznie skonfiguruje je jako token optymistycznej współbieżności.</span><span class="sxs-lookup"><span data-stu-id="996fc-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="996fc-174">Mapowanie typu</span><span class="sxs-lookup"><span data-stu-id="996fc-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="996fc-175">Określanie, czy klasa jest typem złożonym</span><span class="sxs-lookup"><span data-stu-id="996fc-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="996fc-176">Zgodnie z Konwencją typ, który ma określony klucz podstawowy jest traktowany jako typ złożony.</span><span class="sxs-lookup"><span data-stu-id="996fc-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="996fc-177">Istnieją sytuacje, w którym Code First nie wykrywa typ złożony (na przykład, jeśli użytkownik ma właściwość o nazwie identyfikator, ale nie oznaczają, aby mogła być kluczem podstawowym).</span><span class="sxs-lookup"><span data-stu-id="996fc-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="996fc-178">W takich przypadkach należy użyć interfejsu API fluent jawnie określić, że typ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="996fc-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="996fc-179">Określanie nie można zamapować typ CLR jednostki do tabeli w bazie danych</span><span class="sxs-lookup"><span data-stu-id="996fc-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="996fc-180">Poniższy przykład pokazuje, jak wykluczyć typu CLR z mapowany do tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="996fc-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="996fc-181">Mapowania typ jednostki do określonej tabeli w bazie danych</span><span class="sxs-lookup"><span data-stu-id="996fc-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="996fc-182">Wszystkie właściwości działu zostanie zamapowane do kolumn w tabeli o nazwie t_ działu.</span><span class="sxs-lookup"><span data-stu-id="996fc-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="996fc-183">Można również określić nazwę schematu następująco:</span><span class="sxs-lookup"><span data-stu-id="996fc-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="996fc-184">Mapowanie dziedziczenia tabela wg hierarchii (TPH)</span><span class="sxs-lookup"><span data-stu-id="996fc-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="996fc-185">W tym scenariuszu mapowania TPH wszystkich typów w hierarchii dziedziczenia są mapowane na pojedynczą tabelę.</span><span class="sxs-lookup"><span data-stu-id="996fc-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="996fc-186">Kolumna dyskryminatora jest używany do identyfikowania typu każdego wiersza.</span><span class="sxs-lookup"><span data-stu-id="996fc-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="996fc-187">Podczas tworzenia modelu przy użyciu Code First, TPH jest strategia domyślna dla typów, które uczestniczą w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="996fc-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="996fc-188">Domyślnie kolumna dyskryminatora zostanie dodana do tabeli o nazwie "Dyskryminatora" i nazwę typu CLR poszczególnych typów w hierarchii jest używany dla wartości dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="996fc-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="996fc-189">Zachowanie domyślne można modyfikować za pomocą interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="996fc-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="996fc-190">Mapowanie dziedziczenia tabela wg typu (TPT)</span><span class="sxs-lookup"><span data-stu-id="996fc-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="996fc-191">W tym scenariuszu mapowania TPT wszystkie typy są mapowane na poszczególnych tabel.</span><span class="sxs-lookup"><span data-stu-id="996fc-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="996fc-192">Właściwości, które należą wyłącznie do typu podstawowego lub typu pochodnego są przechowywane w tabeli, która mapuje do tego typu.</span><span class="sxs-lookup"><span data-stu-id="996fc-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="996fc-193">Tabele mapowane na typy pochodne również przechowywać klucz obcy, który tworzy sprzężenie tabeli pochodnej z tabeli podstawowej.</span><span class="sxs-lookup"><span data-stu-id="996fc-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="996fc-194">Mapowanie dziedziczenia klasy tabeli na konkretny (TPC)</span><span class="sxs-lookup"><span data-stu-id="996fc-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="996fc-195">W tym scenariuszu mapowania TPC wszystkie typy nieabstrakcyjnej w hierarchii są mapowane na poszczególnych tabel.</span><span class="sxs-lookup"><span data-stu-id="996fc-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="996fc-196">Tabele, które mapują do klas pochodnych nie mają relacji do tabeli, która mapuje do klasy bazowej w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="996fc-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="996fc-197">Wszystkie właściwości klasy, w tym właściwości dziedziczonych są mapowane na kolumny odpowiedniej tabeli.</span><span class="sxs-lookup"><span data-stu-id="996fc-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="996fc-198">Wywołaj metodę MapInheritedProperties, aby skonfigurować każdego typu pochodnego.</span><span class="sxs-lookup"><span data-stu-id="996fc-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="996fc-199">MapInheritedProperties ponownie mapuje wszystkie właściwości, które były dziedziczone z klasy bazowej do nowych kolumn w tabeli dla klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="996fc-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="996fc-200">Należy pamiętać, że ponieważ nie mają tabele uczestniczących w hierarchii dziedziczenia TPC klucz podstawowy będzie jednostki zduplikowane klucze podczas wstawiania do tabel, które są mapowane do podklasy, jeśli masz wartości bazy danych, wygenerowane za pomocą tego samego Inicjator właściwości identity.</span><span class="sxs-lookup"><span data-stu-id="996fc-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="996fc-201">Aby rozwiązać ten problem, możesz określić wartość różnych inicjatora początkowej dla każdej tabeli lub wyłączyć tożsamość na właściwość klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="996fc-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="996fc-202">Tożsamość jest wartością domyślną dla właściwości klucza liczby całkowitej, podczas pracy z usługą Code First.</span><span class="sxs-lookup"><span data-stu-id="996fc-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="996fc-203">Mapowanie właściwości typu jednostki z wieloma tabelami w bazie danych (jednostka dzielenie)</span><span class="sxs-lookup"><span data-stu-id="996fc-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="996fc-204">Jednostki podział umożliwia właściwości typu jednostki, aby być rozkładane na wiele tabel.</span><span class="sxs-lookup"><span data-stu-id="996fc-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="996fc-205">W poniższym przykładzie jednostki dział zostanie podzielona na dwie tabele: dział i DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="996fc-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="996fc-206">Podział jednostki używa wielu wywołań metody mapy do mapowania podzbiór właściwości do określonej tabeli.</span><span class="sxs-lookup"><span data-stu-id="996fc-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="996fc-207">Mapowanie typów jednostek do jednej tabeli w bazie danych (tabeli dzielenie)</span><span class="sxs-lookup"><span data-stu-id="996fc-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="996fc-208">Poniższy przykład mapuje dwóch typów jednostek, które mają klucz podstawowy do jednej tabeli.</span><span class="sxs-lookup"><span data-stu-id="996fc-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="996fc-209">Mapowanie typu jednostki do wstawiania/aktualizowania/usuwania procedur składowanych (od wersji EF6)</span><span class="sxs-lookup"><span data-stu-id="996fc-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="996fc-210">Uruchamianie platformy EF6 można mapować jednostki używanie procedur składowanych do wstawiania, aktualizacji i usuwania.</span><span class="sxs-lookup"><span data-stu-id="996fc-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="996fc-211">Aby uzyskać więcej informacji, zobacz [kodu pierwszy wstawiania/aktualizowania/usuwania procedur składowanych](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="996fc-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="996fc-212">Model używany w przykładach</span><span class="sxs-lookup"><span data-stu-id="996fc-212">Model Used in Samples</span></span>  

<span data-ttu-id="996fc-213">Poniższy model Code First jest używany dla przykładów na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="996fc-213">The following Code First model is used for the samples on this page.</span></span>  

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

---
title: Odtwarzanie przez EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416764"
---
# <a name="reverse-engineering"></a><span data-ttu-id="d5efc-102">Odwrócenie inżynierii</span><span class="sxs-lookup"><span data-stu-id="d5efc-102">Reverse Engineering</span></span>

<span data-ttu-id="d5efc-103">Odtwarzanie jest procesem tworzenia szkieletu klas typu jednostki i klasy DbContext na podstawie schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="d5efc-104">Można ją wykonać przy użyciu polecenia `Scaffold-DbContext` narzędzia EF Core Console Menedżera pakietów (PMC) lub polecenia `dotnet ef dbcontext scaffold` narzędzia interfejsu wiersza polecenia (CLI) platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d5efc-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="d5efc-105">Instalowanie programu</span><span class="sxs-lookup"><span data-stu-id="d5efc-105">Installing</span></span>

<span data-ttu-id="d5efc-106">Przed odwróceniem zespołu należy zainstalować [Narzędzia PMC](xref:core/miscellaneous/cli/powershell) (tylko Visual Studio) lub [Narzędzia interfejsu wiersza polecenia](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="d5efc-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="d5efc-107">Zobacz linki, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="d5efc-107">See links for details.</span></span>

<span data-ttu-id="d5efc-108">Należy również zainstalować odpowiedniego [dostawcę bazy danych](xref:core/providers/index) dla schematu bazy danych, który ma być odtwarzany.</span><span class="sxs-lookup"><span data-stu-id="d5efc-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="d5efc-109">Parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="d5efc-109">Connection string</span></span>

<span data-ttu-id="d5efc-110">Pierwszym argumentem polecenia są parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="d5efc-111">Narzędzia użyją tych parametrów połączenia, aby odczytać schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="d5efc-112">Sposób tworzenia cudzysłowów i ucieczki parametrów połączenia zależy od powłoki, która jest używana do wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="d5efc-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="d5efc-113">Szczegółowe informacje można znaleźć w dokumentacji powłoki.</span><span class="sxs-lookup"><span data-stu-id="d5efc-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="d5efc-114">Na przykład program PowerShell wymaga wyznaczenia znaku `$`, ale nie `\`.</span><span class="sxs-lookup"><span data-stu-id="d5efc-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="d5efc-115">Klucze tajne konfiguracji i użytkownika</span><span class="sxs-lookup"><span data-stu-id="d5efc-115">Configuration and User Secrets</span></span>

<span data-ttu-id="d5efc-116">Jeśli masz projekt ASP.NET Core, możesz użyć składni `Name=<connection-string>`, aby odczytać parametry połączenia z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d5efc-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="d5efc-117">Jest to dobre rozwiązanie przy użyciu [Narzędzia tajnego Menedżera](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) w celu zachowania podzielenia hasła bazy danych od kodu bazowej.</span><span class="sxs-lookup"><span data-stu-id="d5efc-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="d5efc-118">Nazwa dostawcy</span><span class="sxs-lookup"><span data-stu-id="d5efc-118">Provider name</span></span>

<span data-ttu-id="d5efc-119">Drugi argument jest nazwą dostawcy.</span><span class="sxs-lookup"><span data-stu-id="d5efc-119">The second argument is the provider name.</span></span> <span data-ttu-id="d5efc-120">Nazwa dostawcy jest zwykle taka sama jak nazwa pakietu NuGet dostawcy.</span><span class="sxs-lookup"><span data-stu-id="d5efc-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="d5efc-121">Określanie tabel</span><span class="sxs-lookup"><span data-stu-id="d5efc-121">Specifying tables</span></span>

<span data-ttu-id="d5efc-122">Wszystkie tabele w schemacie bazy danych są domyślnie odtwarzane w postaci typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="d5efc-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="d5efc-123">Można ograniczyć, które tabele są odtworzone, określając schematy i tabele.</span><span class="sxs-lookup"><span data-stu-id="d5efc-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="d5efc-124">`-Schemas` parametru w parametrze PMC i opcji `--schema` w interfejsie wiersza polecenia można użyć do uwzględnienia każdej tabeli w schemacie.</span><span class="sxs-lookup"><span data-stu-id="d5efc-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="d5efc-125">`-Tables` (PMC) i `--table` (CLI) mogą być używane do dołączania określonych tabel.</span><span class="sxs-lookup"><span data-stu-id="d5efc-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="d5efc-126">Aby uwzględnić wiele tabel w kryterium PMC, użyj tablicy.</span><span class="sxs-lookup"><span data-stu-id="d5efc-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="d5efc-127">Aby dołączyć wiele tabel w interfejsie wiersza polecenia, należy wybrać opcję wiele razy.</span><span class="sxs-lookup"><span data-stu-id="d5efc-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="d5efc-128">Zachowywanie nazw</span><span class="sxs-lookup"><span data-stu-id="d5efc-128">Preserving names</span></span>

<span data-ttu-id="d5efc-129">Nazwy tabel i kolumn są rozwiązywane w celu lepszego dopasowania do konwencji nazewnictwa platformy .NET dla typów i właściwości.</span><span class="sxs-lookup"><span data-stu-id="d5efc-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="d5efc-130">Określenie przełącznika `-UseDatabaseNames` w obszarze PMC lub opcji `--use-database-names` w interfejsie wiersza polecenia spowoduje wyłączenie tego zachowania, zachowując możliwie jak najwięcej nazw baz danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="d5efc-131">Nieprawidłowe identyfikatory platformy .NET nadal będą stałe i są one zgodne z konwencjami nazewnictwa platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="d5efc-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="d5efc-132">Interfejs API Fluent lub adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="d5efc-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="d5efc-133">Typy jednostek są domyślnie konfigurowane przy użyciu interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d5efc-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="d5efc-134">Określ `-DataAnnotations` (PMC) lub `--data-annotations` (CLI), aby zamiast tego używać adnotacji danych, gdy jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="d5efc-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="d5efc-135">Na przykład korzystanie z interfejsu API Fluent spowoduje przetworzenie szkieletu:</span><span class="sxs-lookup"><span data-stu-id="d5efc-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="d5efc-136">Używanie adnotacji danych spowoduje szkielet:</span><span class="sxs-lookup"><span data-stu-id="d5efc-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="d5efc-137">Nazwa kontekstu</span><span class="sxs-lookup"><span data-stu-id="d5efc-137">DbContext name</span></span>

<span data-ttu-id="d5efc-138">Nazwa klasy DbContext z szkieletem jest nazwą bazy danych domyślnie sufiksem z *kontekstem* .</span><span class="sxs-lookup"><span data-stu-id="d5efc-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="d5efc-139">Aby określić inny sposób, użyj `-Context` w PMC i `--context` w interfejsie wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d5efc-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="d5efc-140">Katalogi i przestrzenie nazw</span><span class="sxs-lookup"><span data-stu-id="d5efc-140">Directories and namespaces</span></span>

<span data-ttu-id="d5efc-141">Klasy jednostek i Klasa DbContext są szkieletowe w katalogu głównym projektu i używają domyślnej przestrzeni nazw projektu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="d5efc-142">Można określić katalog, w którym są szkieletowe klasy przy użyciu `-OutputDir` (PMC) lub `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="d5efc-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="d5efc-143">Przestrzeń nazw będzie główną przestrzenią nazw oraz nazwami wszystkich podkatalogów w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="d5efc-144">Można również użyć `-ContextDir` (PMC) i `--context-dir` (CLI) do tworzenia szkieletu klasy DbContext w oddzielnym katalogu z klas typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="d5efc-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="d5efc-145">Jak to działa</span><span class="sxs-lookup"><span data-stu-id="d5efc-145">How it works</span></span>

<span data-ttu-id="d5efc-146">Odtwarzanie rozpocznie się, odczytując schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="d5efc-147">Odczytuje informacje o tabelach, kolumnach, ograniczeniach i indeksach.</span><span class="sxs-lookup"><span data-stu-id="d5efc-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="d5efc-148">Następnie używa informacji o schemacie do tworzenia modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5efc-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="d5efc-149">Tabele są używane do tworzenia typów jednostek; kolumny są używane do tworzenia właściwości; i klucze obce są używane do tworzenia relacji.</span><span class="sxs-lookup"><span data-stu-id="d5efc-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="d5efc-150">Na koniec model jest używany do generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="d5efc-151">Odpowiednie klasy typu jednostki, interfejs API Fluent i adnotacje danych są szkieletami, aby można było ponownie utworzyć ten sam model z poziomu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5efc-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="limitations"></a><span data-ttu-id="d5efc-152">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="d5efc-152">Limitations</span></span>

* <span data-ttu-id="d5efc-153">Nie wszystkie elementy dotyczące modelu mogą być reprezentowane za pomocą schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="d5efc-154">Na przykład informacje o [**hierarchiach dziedziczenia**](../modeling/inheritance.md), [**typach będących własnością**](../modeling/owned-entities.md)i [**podziałie tabeli**](../modeling/table-splitting.md) nie są obecne w schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d5efc-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="d5efc-155">W związku z tym konstrukcje te nigdy nie będą odtwarzane.</span><span class="sxs-lookup"><span data-stu-id="d5efc-155">Because of this, these constructs will never be reverse engineered.</span></span>
* <span data-ttu-id="d5efc-156">Ponadto **niektóre typy kolumn** mogą nie być obsługiwane przez dostawcę EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5efc-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="d5efc-157">Te kolumny nie zostaną uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-157">These columns won't be included in the model.</span></span>
* <span data-ttu-id="d5efc-158">Można zdefiniować [**tokeny współbieżności**](../modeling/concurrency.md)w modelu EF Core, aby uniemożliwić dwóm użytkownikom Aktualizowanie tej samej jednostki w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="d5efc-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="d5efc-159">Niektóre bazy danych mają specjalny typ reprezentujący ten typ kolumny (na przykład rowversion w SQL Server), w którym można odtworzyć te informacje. Jednak inne tokeny współbieżności nie będą odtwarzane.</span><span class="sxs-lookup"><span data-stu-id="d5efc-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>
* <span data-ttu-id="d5efc-160">[Funkcja C# typu referencyjnego 8 dopuszczających wartość null](/dotnet/csharp/tutorials/nullable-reference-types) nie jest obecnie obsługiwana w przypadku odtwarzania C# : EF Core zawsze generuje kod, który zakłada, że funkcja jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="d5efc-160">[The C# 8 nullable reference type feature](/dotnet/csharp/tutorials/nullable-reference-types) is currently unsupported in reverse engineering: EF Core always generates C# code that assumes the feature is disabled.</span></span> <span data-ttu-id="d5efc-161">Na przykład kolumny tekstu dopuszczające wartość null będą szkieletem jako właściwość o typie `string`, a nie `string?`, z interfejsem API Fluent lub adnotacjami danych używanymi do konfigurowania, czy właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="d5efc-161">For example, nullable text columns will be scaffolded as a property with type `string` , not `string?`, with either the Fluent API or Data Annotations used to configure whether a property is required or not.</span></span> <span data-ttu-id="d5efc-162">Można edytować kod szkieletowy i zastąpić je adnotacjami o C# wartości null.</span><span class="sxs-lookup"><span data-stu-id="d5efc-162">You can edit the scaffolded code and replace these with C# nullability annotations.</span></span> <span data-ttu-id="d5efc-163">Obsługa tworzenia szkieletów dla typów odwołań do wartości null jest śledzona przez [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)problemu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-163">Scaffolding support for nullable reference types is tracked by issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="d5efc-164">Dostosowywanie modelu</span><span class="sxs-lookup"><span data-stu-id="d5efc-164">Customizing the model</span></span>

<span data-ttu-id="d5efc-165">Kod wygenerowany przez EF Core to Twój kod.</span><span class="sxs-lookup"><span data-stu-id="d5efc-165">The code generated by EF Core is your code.</span></span> <span data-ttu-id="d5efc-166">Możesz go zmienić.</span><span class="sxs-lookup"><span data-stu-id="d5efc-166">Feel free to change it.</span></span> <span data-ttu-id="d5efc-167">Zostanie on wygenerowany ponownie tylko w przypadku ponownego odtworzenia tego samego modelu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-167">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="d5efc-168">Kod szkieletowy reprezentuje *jeden* model, który może być używany w celu uzyskania dostępu do bazy danych, ale nie jest to *jedyny* model, który może być używany.</span><span class="sxs-lookup"><span data-stu-id="d5efc-168">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="d5efc-169">Dostosuj klasy typu jednostki i klasy DbContext do Twoich potrzeb.</span><span class="sxs-lookup"><span data-stu-id="d5efc-169">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="d5efc-170">Można na przykład zmienić nazwy typów i właściwości, wprowadzić hierarchie dziedziczenia lub podzielić tabelę na wiele jednostek.</span><span class="sxs-lookup"><span data-stu-id="d5efc-170">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="d5efc-171">Można również usunąć nieunikatowe indeksy, nieużywane sekwencje i właściwości nawigacji, opcjonalne właściwości skalarne i nazwy ograniczeń z modelu.</span><span class="sxs-lookup"><span data-stu-id="d5efc-171">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="d5efc-172">Można również dodać dodatkowe konstruktory, metody, właściwości itp.</span><span class="sxs-lookup"><span data-stu-id="d5efc-172">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="d5efc-173">użycie innej klasy częściowej w oddzielnym pliku.</span><span class="sxs-lookup"><span data-stu-id="d5efc-173">using another partial class in a separate file.</span></span> <span data-ttu-id="d5efc-174">Takie podejście działa nawet wtedy, gdy zamierzasz ponownie odtworzyć model.</span><span class="sxs-lookup"><span data-stu-id="d5efc-174">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="d5efc-175">Aktualizowanie modelu</span><span class="sxs-lookup"><span data-stu-id="d5efc-175">Updating the model</span></span>

<span data-ttu-id="d5efc-176">Po wprowadzeniu zmian w bazie danych może być konieczne zaktualizowanie modelu EF Core w celu odzwierciedlenia tych zmian.</span><span class="sxs-lookup"><span data-stu-id="d5efc-176">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="d5efc-177">W przypadku, gdy baza danych jest prosta, może być najłatwiej ręcznie wprowadzić zmiany w modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5efc-177">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="d5efc-178">Na przykład zmiana nazwy tabeli lub kolumny, usunięcie kolumny lub aktualizacja typu kolumny to proste zmiany wprowadzane w kodzie.</span><span class="sxs-lookup"><span data-stu-id="d5efc-178">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="d5efc-179">Bardziej znaczące zmiany nie są jednak łatwe do ręcznego wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="d5efc-179">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="d5efc-180">Jednym z typowych przepływów pracy jest ponowne odtwarzanie modelu z bazy danych przy użyciu `-Force` (PMC) lub `--force` (CLI), aby zastąpić istniejący model zaktualizowanym.</span><span class="sxs-lookup"><span data-stu-id="d5efc-180">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="d5efc-181">Inną często żądaną funkcją jest możliwość aktualizowania modelu z bazy danych przy zachowaniu dostosowania, takiego jak renames, hierarchie typów itd. Użyj [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) problemu, aby śledzić postęp tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="d5efc-181">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="d5efc-182">Jeśli odwrócisz model z bazy danych, wszelkie zmiany wprowadzone w plikach zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="d5efc-182">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>

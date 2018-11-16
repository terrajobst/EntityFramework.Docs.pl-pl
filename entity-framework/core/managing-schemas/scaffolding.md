---
title: Odtwarzanie — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688683"
---
# <a name="reverse-engineering"></a><span data-ttu-id="2796c-102">Odtwarzanie</span><span class="sxs-lookup"><span data-stu-id="2796c-102">Reverse Engineering</span></span>

<span data-ttu-id="2796c-103">Odtwarzanie jest proces tworzenia szkieletów klasy typów jednostek i klasy DbContext, na podstawie schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="2796c-104">Mogą być wykonywane przy użyciu `Scaffold-DbContext` polecenia narzędzia konsoli Menedżera pakietów (PMC) EF Core lub `dotnet ef dbcontext scaffold` polecenia narzędzia .NET interfejsu wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="2796c-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="2796c-105">Instalowanie programu</span><span class="sxs-lookup"><span data-stu-id="2796c-105">Installing</span></span>

<span data-ttu-id="2796c-106">Przed odtwarzania, należy zainstalować jedną [narzędzia PMC](xref:core/miscellaneous/cli/powershell) (tylko Visual Studio) lub [narzędzi interfejsu wiersza polecenia](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="2796c-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="2796c-107">Zobacz linki, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="2796c-107">See links for details.</span></span>

<span data-ttu-id="2796c-108">Należy także zainstalować odpowiednią [dostawcy bazy danych](xref:core/providers/index) dla schematu bazy danych ma zostać odtworzona.</span><span class="sxs-lookup"><span data-stu-id="2796c-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="2796c-109">Parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="2796c-109">Connection string</span></span>

<span data-ttu-id="2796c-110">Pierwszy argument polecenia jest ciąg połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="2796c-111">Narzędzia użyje te parametry połączenia można odczytać schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="2796c-112">Jak oferty i wprowadzić ciąg połączenia zależy od Shell, które używają można wykonać polecenia.</span><span class="sxs-lookup"><span data-stu-id="2796c-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="2796c-113">Zajrzyj do dokumentacji powłoki, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="2796c-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="2796c-114">Na przykład programu PowerShell wymaga jako znak ucieczki `$` znaków, ale nie `\`.</span><span class="sxs-lookup"><span data-stu-id="2796c-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="2796c-115">Konfiguracja i wpisami tajnymi użytkowników</span><span class="sxs-lookup"><span data-stu-id="2796c-115">Configuration and User Secrets</span></span>

<span data-ttu-id="2796c-116">Jeśli masz projekt platformy ASP.NET Core, możesz użyć `Name=<connection-string>` składni można odczytać parametrów połączenia z konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2796c-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="2796c-117">Ta funkcja działa dobrze z [narzędzie Menedżer klucz tajny](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) być oddzielone od kodu hasła bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="2796c-118">Nazwa dostawcy</span><span class="sxs-lookup"><span data-stu-id="2796c-118">Provider name</span></span>

<span data-ttu-id="2796c-119">Drugi argument jest nazwą dostawcy.</span><span class="sxs-lookup"><span data-stu-id="2796c-119">The second argument is the provider name.</span></span> <span data-ttu-id="2796c-120">Nazwa dostawcy jest zwykle taka sama jak nazwa pakietu NuGet dostawcy.</span><span class="sxs-lookup"><span data-stu-id="2796c-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="2796c-121">Określanie tabel</span><span class="sxs-lookup"><span data-stu-id="2796c-121">Specifying tables</span></span>

<span data-ttu-id="2796c-122">Wszystkie tabele w schemacie bazy danych są odtwarzane do typów jednostek, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2796c-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="2796c-123">Można ograniczyć, które tabele są odtwarzane, określając schematy i tabele.</span><span class="sxs-lookup"><span data-stu-id="2796c-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="2796c-124">`-Schemas` Parametru w konsoli zarządzania Pakietami i `--schema` opcji interfejsu wiersza polecenia może służyć do uwzględnienia w każdej tabeli w schemacie.</span><span class="sxs-lookup"><span data-stu-id="2796c-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="2796c-125">`-Tables` (PMC) i `--table` (CLI) może służyć do uwzględnienia określonych tabel.</span><span class="sxs-lookup"><span data-stu-id="2796c-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="2796c-126">Aby uwzględnić wiele tabel w konsoli zarządzania Pakietami, skorzystaj z tablicy.</span><span class="sxs-lookup"><span data-stu-id="2796c-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="2796c-127">Aby uwzględnić wiele tabel w interfejsu wiersza polecenia, należy określić opcję wiele razy.</span><span class="sxs-lookup"><span data-stu-id="2796c-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="2796c-128">Zachowywanie nazwy</span><span class="sxs-lookup"><span data-stu-id="2796c-128">Preserving names</span></span>

<span data-ttu-id="2796c-129">Nazwy tabel i kolumn są stałe, aby lepiej dopasować je do konwencji nazewnictwa platformy .NET, właściwości i typy domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2796c-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="2796c-130">Określanie `-UseDatabaseNames` przełącznika w konsoli zarządzania Pakietami lub `--use-database-names` opcji interfejsu wiersza polecenia spowoduje wyłączenie to zachowanie, zachowując możliwie oryginalne nazwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="2796c-131">Nieprawidłowe identyfikatory .NET nadal zostanie rozwiązany i syntetyzowany nazwy, takie jak właściwości nawigacji nadal będą zgodne z konwencjami nazewnictwa platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="2796c-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="2796c-132">Interfejs API Fluent lub adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="2796c-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="2796c-133">Typy jednostek są konfigurowane przy użyciu interfejsu API Fluent domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2796c-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="2796c-134">Określ `-DataAnnotations` (PMC) lub `--data-annotations` (CLI), aby zamiast tego użyć adnotacji danych, gdy jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="2796c-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="2796c-135">Na przykład za pomocą interfejsu API Fluent będzie tworzenia szkieletu tej.</span><span class="sxs-lookup"><span data-stu-id="2796c-135">For example, using the Fluent API will scaffold the this.</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="2796c-136">Podczas korzystania z adnotacji danych będzie tworzenia szkieletu to.</span><span class="sxs-lookup"><span data-stu-id="2796c-136">While using Data Annotations will scaffold this.</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="2796c-137">Nazwa typu DbContext</span><span class="sxs-lookup"><span data-stu-id="2796c-137">DbContext name</span></span>

<span data-ttu-id="2796c-138">Nazwa klasy DbContext szkieletu będzie nazwa bazy danych z sufiksem *kontekstu* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2796c-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="2796c-139">Aby określić inne konto, użyj `-Context` w konsoli zarządzania Pakietami i `--context` w interfejsie wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2796c-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="2796c-140">Katalogi i przestrzenie nazw</span><span class="sxs-lookup"><span data-stu-id="2796c-140">Directories and namespaces</span></span>

<span data-ttu-id="2796c-141">Klas jednostek i klasy DbContext są szkieletu do katalogu głównego projektu i użyj projektu domyślny obszar nazw.</span><span class="sxs-lookup"><span data-stu-id="2796c-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="2796c-142">Można określić katalog, w przypadku, gdy klasy są szkieletu za pomocą `-OutputDir` (PMC) lub `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="2796c-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="2796c-143">Przestrzeń nazw będzie głównej przestrzeni nazw, a także nazw podkatalogów w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="2796c-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="2796c-144">Można również użyć `-ContextDir` (PMC) i `--context-dir` (CLI) do tworzenia szkieletu klasy DbContext w oddzielnym katalogu z klasami typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="2796c-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="2796c-145">Jak to działa</span><span class="sxs-lookup"><span data-stu-id="2796c-145">How it works</span></span>

<span data-ttu-id="2796c-146">Odtwarzanie rozpoczyna się od odczytu schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="2796c-147">Odczytuje informacje o tabele, kolumny, ograniczenia i indeksy.</span><span class="sxs-lookup"><span data-stu-id="2796c-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="2796c-148">Następnie używa informacji o schemacie, aby utworzyć model programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="2796c-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="2796c-149">Tabele są używane do tworzenia typów jednostek. kolumny są używane do tworzenia właściwości; i kluczy obcych są używane do tworzenia relacji.</span><span class="sxs-lookup"><span data-stu-id="2796c-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="2796c-150">Na koniec model jest używany do generowania kodu.</span><span class="sxs-lookup"><span data-stu-id="2796c-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="2796c-151">Odpowiednie jednostki typu klasy, interfejsu API Fluent i danych adnotacje są szkieletu Aby ponownie utworzyć ten sam model aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2796c-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="2796c-152">Co nie działa</span><span class="sxs-lookup"><span data-stu-id="2796c-152">What doesn't work</span></span>

<span data-ttu-id="2796c-153">Nie wszystkie informacje o modelu może być reprezentowany za pomocą schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="2796c-154">Na przykład informacje o **hierarchii dziedziczenia**, **należące do typów**, i **tabeli podział** nie są obecne w schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2796c-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="2796c-155">W związku z tym te konstrukcje będzie nigdy nie być odtwarzane.</span><span class="sxs-lookup"><span data-stu-id="2796c-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="2796c-156">Ponadto **niektóre typy kolumn** nie mogą być obsługiwane przez dostawcę programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="2796c-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="2796c-157">Kolumny te nie zostaną uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="2796c-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="2796c-158">EF Core wymaga klucza każdego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="2796c-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="2796c-159">Tabele, jednak nie są wymagane do określenia klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="2796c-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="2796c-160">**Tabele bez klucza podstawowego** obecnie nie są odtwarzane.</span><span class="sxs-lookup"><span data-stu-id="2796c-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="2796c-161">Można zdefiniować **tokeny współbieżności** w modelu platformy EF Core uniemożliwić dwa użytkownikom aktualizowanie tej samej jednostki w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="2796c-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="2796c-162">Niektóre bazy danych ma specjalny typ do reprezentowania kolumna (na przykład rowversion w programie SQL Server) tego typu w takiej sytuacji firma Microsoft może wycofać inżynier te informacje; jednak inne tokeny współbieżności będzie nie być odtwarzane.</span><span class="sxs-lookup"><span data-stu-id="2796c-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="2796c-163">Dostosowywanie modelu</span><span class="sxs-lookup"><span data-stu-id="2796c-163">Customizing the model</span></span>

<span data-ttu-id="2796c-164">Kod wygenerowany przez platformę EF Core to Twój kod.</span><span class="sxs-lookup"><span data-stu-id="2796c-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="2796c-165">Możesz go zmienić.</span><span class="sxs-lookup"><span data-stu-id="2796c-165">Feel free to change it.</span></span> <span data-ttu-id="2796c-166">Zostaną tylko wygenerowane, jeśli możesz odtworzyć tego samego modelu ponownie.</span><span class="sxs-lookup"><span data-stu-id="2796c-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="2796c-167">Reprezentuje utworzony szkielet kodu *jeden* modelu, który może służyć do uzyskania dostępu do bazy danych, ale na pewno nie jest *tylko* modelu, który może służyć.</span><span class="sxs-lookup"><span data-stu-id="2796c-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="2796c-168">Dostosuj klasy typów jednostek i klasy DbContext odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="2796c-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="2796c-169">Na przykład można zmienić nazwy właściwości i typy, wprowadzają hierarchii dziedziczenia lub dzielenie tabeli do wielu jednostek.</span><span class="sxs-lookup"><span data-stu-id="2796c-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="2796c-170">Indeksy unikatowe, nieużywane sekwencji i właściwości nawigacji, opcjonalne właściwości skalarne i nazwy ograniczenia mogą również usuwać z modelu.</span><span class="sxs-lookup"><span data-stu-id="2796c-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="2796c-171">Można również dodać dodatkowe konstruktorów, metod, właściwości itd.</span><span class="sxs-lookup"><span data-stu-id="2796c-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="2796c-172">w oddzielnym pliku, przy użyciu innej klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="2796c-172">using another partial class in a separate file.</span></span> <span data-ttu-id="2796c-173">To podejście sprawdza się nawet wtedy, gdy zamierzasz odtwarzać modelu ponownie.</span><span class="sxs-lookup"><span data-stu-id="2796c-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="2796c-174">Aktualizowanie modelu</span><span class="sxs-lookup"><span data-stu-id="2796c-174">Updating the model</span></span>

<span data-ttu-id="2796c-175">Po wprowadzeniu zmian w bazie danych, może być konieczne zaktualizowanie modelu platformy EF Core w celu odzwierciedlenia tych zmian.</span><span class="sxs-lookup"><span data-stu-id="2796c-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="2796c-176">W przypadku prostych zmian w bazie danych może być łatwiej tylko ręcznie wprowadź zmiany w modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="2796c-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="2796c-177">Na przykład zmiana nazwy tabeli lub kolumny, usunięcie kolumny lub aktualizowanie typ kolumny są proste zmiany w kodzie.</span><span class="sxs-lookup"><span data-stu-id="2796c-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="2796c-178">Najbardziej znaczące zmiany nie są jednak jako łatwe upewnij ręcznie.</span><span class="sxs-lookup"><span data-stu-id="2796c-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="2796c-179">Jeden typowy przepływ pracy jest, aby odwrócić inżynier modelu z bazy danych ponownie, używając `-Force` (PMC) lub `--force` (CLI) w celu zastąpienia istniejącego modelu jeden z zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2796c-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="2796c-180">Kolejną funkcją najczęściej żądanych jest możliwość aktualizacji modelu z bazy danych przy jednoczesnym zachowaniu dostosowywania, takich jak zmiana nazwy, typu hierarchie itp. Użyj problem [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) do śledzenia postępu dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="2796c-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="2796c-181">Jeśli odtworzeniu modelu z bazy danych ponownie, wszelkie zmiany wprowadzone w plikach zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="2796c-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>

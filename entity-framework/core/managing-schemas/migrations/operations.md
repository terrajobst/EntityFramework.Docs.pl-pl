---
title: Operacje migracji niestandardowych — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416878"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="92a30-102">Operacje migracji niestandardowych</span><span class="sxs-lookup"><span data-stu-id="92a30-102">Custom Migrations Operations</span></span>

<span data-ttu-id="92a30-103">Interfejs API MigrationBuilder umożliwia wykonywanie wielu różnych rodzajów operacji podczas migracji, ale nie jest to wyczerpujące.</span><span class="sxs-lookup"><span data-stu-id="92a30-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="92a30-104">Jednak interfejs API jest również rozszerzalny, co pozwala na Definiowanie własnych operacji.</span><span class="sxs-lookup"><span data-stu-id="92a30-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="92a30-105">Istnieją dwa sposoby rozszerzeń interfejsu API: przy użyciu metody `Sql()` lub definiowania niestandardowych obiektów `MigrationOperation`.</span><span class="sxs-lookup"><span data-stu-id="92a30-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="92a30-106">Aby to zilustrować, przyjrzyjmy się implementacji operacji, która tworzy użytkownika bazy danych przy użyciu poszczególnych metod.</span><span class="sxs-lookup"><span data-stu-id="92a30-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="92a30-107">W naszych migracjach chcemy włączyć pisanie następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="92a30-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="92a30-108">Korzystanie z MigrationBuilder. SQL ()</span><span class="sxs-lookup"><span data-stu-id="92a30-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="92a30-109">Najprostszym sposobem implementacji niestandardowej operacji jest zdefiniowanie metody rozszerzenia, która wywołuje `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="92a30-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="92a30-110">Oto przykład, który generuje odpowiednie instrukcje Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="92a30-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="92a30-111">Jeśli migracje muszą obsługiwać wielu dostawców baz danych, można użyć właściwości `MigrationBuilder.ActiveProvider`.</span><span class="sxs-lookup"><span data-stu-id="92a30-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="92a30-112">Oto przykład obsługujący zarówno Microsoft SQL Server, jak i PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="92a30-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

<span data-ttu-id="92a30-113">To podejście działa tylko wtedy, gdy wiadomo, że każdy dostawca, w którym zostanie zastosowana operacja niestandardowa.</span><span class="sxs-lookup"><span data-stu-id="92a30-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="92a30-114">Korzystanie z MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="92a30-114">Using a MigrationOperation</span></span>

<span data-ttu-id="92a30-115">Aby rozdzielić operację niestandardową z SQL, można zdefiniować własne `MigrationOperation` do reprezentowania.</span><span class="sxs-lookup"><span data-stu-id="92a30-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="92a30-116">Operacja jest następnie przenoszona do dostawcy, aby mogła określić odpowiedni kod SQL do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="92a30-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="92a30-117">W tym podejściu Metoda rozszerzająca musi tylko dodać jedną z tych operacji do `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="92a30-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

<span data-ttu-id="92a30-118">Takie podejście wymaga, aby każdy dostawca wiedział, jak wygenerować SQL dla tej operacji w usłudze `IMigrationsSqlGenerator`.</span><span class="sxs-lookup"><span data-stu-id="92a30-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="92a30-119">Oto przykład przesłaniania generatora SQL Server w celu obsługi nowej operacji.</span><span class="sxs-lookup"><span data-stu-id="92a30-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="92a30-120">Zastąp domyślną usługę generatora SQL dla migracji z aktualizacją.</span><span class="sxs-lookup"><span data-stu-id="92a30-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```

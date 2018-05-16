---
title: Operacje migracji niestandardowej - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a><span data-ttu-id="bae66-102">Operacje migracji niestandardowych</span><span class="sxs-lookup"><span data-stu-id="bae66-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="bae66-103">Interfejs API MigrationBuilder umożliwia wykonywać wiele różnych operacji podczas migracji, ale jest daleko od wyczerpujący.</span><span class="sxs-lookup"><span data-stu-id="bae66-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="bae66-104">Jednak interfejsu API jest również możliwość definiowania własnych operacji extensible.</span><span class="sxs-lookup"><span data-stu-id="bae66-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="bae66-105">Istnieją dwa sposoby rozszerzania interfejsu API: przy użyciu `Sql()` metody, lub przez zdefiniowanie niestandardowego `MigrationOperation` obiektów.</span><span class="sxs-lookup"><span data-stu-id="bae66-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="bae66-106">Aby zilustrować, Przyjrzyjmy się implementacja operację, która tworzy użytkownika bazy danych przy użyciu każdego z podejść.</span><span class="sxs-lookup"><span data-stu-id="bae66-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="bae66-107">W naszym migracje chcemy włączenia zapisywania następujący kod:</span><span class="sxs-lookup"><span data-stu-id="bae66-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="bae66-108">Przy użyciu MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="bae66-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="bae66-109">Najprostszym sposobem, aby zaimplementować to operacja niestandardowa jest określenie metodę rozszerzenia, która wywołuje `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="bae66-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="bae66-110">Oto przykład generujący odpowiedniego języka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="bae66-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="bae66-111">Jeśli migracji konieczność obsługi wielu dostawców bazy danych, możesz użyć `MigrationBuilder.ActiveProvider` właściwości.</span><span class="sxs-lookup"><span data-stu-id="bae66-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="bae66-112">Oto przykład obsługi zarówno programu Microsoft SQL Server, jak i PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bae66-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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
}
```

<span data-ttu-id="bae66-113">Ta metoda działa tylko, jeśli znasz każdego dostawcy których zostaną zastosowane niestandardowych operacji.</span><span class="sxs-lookup"><span data-stu-id="bae66-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="bae66-114">Przy użyciu MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="bae66-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="bae66-115">Rozdzielenie operacja niestandardowa z SQL, można zdefiniować własny `MigrationOperation` do reprezentowania go.</span><span class="sxs-lookup"><span data-stu-id="bae66-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="bae66-116">Operacja są następnie przekazywane do dostawcy, dzięki czemu można określić odpowiednie SQL do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="bae66-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="bae66-117">Z tej metody, metoda rozszerzenia musi dodać jeden z tych operacji `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="bae66-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="bae66-118">Ta metoda wymaga każdego dostawcy dowiedzieć się, jak można wygenerować kodu SQL dla tej operacji w ich `IMigrationsSqlGenerator` usługi.</span><span class="sxs-lookup"><span data-stu-id="bae66-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="bae66-119">Oto przykład zastępowanie generator SQL Server do obsługi nowej operacji.</span><span class="sxs-lookup"><span data-stu-id="bae66-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="bae66-120">Zaktualizowano zastąpić domyślne migracje sql generatora usługi.</span><span class="sxs-lookup"><span data-stu-id="bae66-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```

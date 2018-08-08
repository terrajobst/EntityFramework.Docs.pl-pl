---
title: Operacje migracji niestandardowe — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 510d585534b4809179c905ee5b77cab4209a2b8f
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614288"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="f6f9c-102">Operacje migracji niestandardowe</span><span class="sxs-lookup"><span data-stu-id="f6f9c-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="f6f9c-103">Interfejs API MigrationBuilder umożliwia wykonywać wiele różnych operacji podczas migracji, ale go nie jest wyczerpująca.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="f6f9c-104">Interfejs API jest jednak również rozszerzalny, co pozwala zdefiniować własne operacje.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="f6f9c-105">Istnieją dwa sposoby, aby rozszerzyć interfejs API: przy użyciu `Sql()` metody lub przez zdefiniowanie niestandardowego `MigrationOperation` obiektów.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="f6f9c-106">Aby zilustrować, Przyjrzyjmy się wykonania operacji, która tworzy użytkownika bazy danych za pomocą danej metody.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="f6f9c-107">W naszym migracje chcemy włączenia zapisywania następujący kod:</span><span class="sxs-lookup"><span data-stu-id="f6f9c-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="f6f9c-108">Za pomocą MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="f6f9c-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="f6f9c-109">Najłatwiejszym sposobem realizowania operacji niestandardowej jest zdefiniowanie metodę rozszerzającą wywołująca `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="f6f9c-110">Oto przykład, który generuje odpowiednie języka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="f6f9c-111">Jeśli migracji potrzebne do obsługi wielu dostawców bazy danych, można użyć `MigrationBuilder.ActiveProvider` właściwości.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="f6f9c-112">Oto przykład obsługi zarówno programu Microsoft SQL Server, jak i bazy danych PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="f6f9c-113">To podejście tylko wtedy, gdy wiesz, co dostawcy której zostaną zastosowane niestandardowych operacji.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="f6f9c-114">Za pomocą MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="f6f9c-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="f6f9c-115">Aby oddzielić niestandardowych operacji z bazy danych SQL, można zdefiniować własne `MigrationOperation` go reprezentuje.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="f6f9c-116">Operacja są następnie przekazywane do dostawcy, dzięki czemu można określić, odpowiednie SQL w celu generowania.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="f6f9c-117">W przypadku tej metody, metoda rozszerzenia musi jedynie jedną z tych operacji, aby dodać `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="f6f9c-118">Takie podejście wymaga każdego dostawcy wiedzieć, jak wygenerować kodu SQL do wykonania tej operacji w ich `IMigrationsSqlGenerator` usługi.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="f6f9c-119">Oto przykład zastępowanie generator programu SQL Server do obsługi nowej operacji.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="f6f9c-120">Zaktualizowano zastąpić domyślnej migracji sql generator usługi.</span><span class="sxs-lookup"><span data-stu-id="f6f9c-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```

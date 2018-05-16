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
<a name="custom-migrations-operations"></a>Operacje migracji niestandardowych
============================
Interfejs API MigrationBuilder umożliwia wykonywać wiele różnych operacji podczas migracji, ale jest daleko od wyczerpujący. Jednak interfejsu API jest również możliwość definiowania własnych operacji extensible. Istnieją dwa sposoby rozszerzania interfejsu API: przy użyciu `Sql()` metody, lub przez zdefiniowanie niestandardowego `MigrationOperation` obiektów.

Aby zilustrować, Przyjrzyjmy się implementacja operację, która tworzy użytkownika bazy danych przy użyciu każdego z podejść. W naszym migracje chcemy włączenia zapisywania następujący kod:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Przy użyciu MigrationBuilder.Sql()
----------------------------
Najprostszym sposobem, aby zaimplementować to operacja niestandardowa jest określenie metodę rozszerzenia, która wywołuje `MigrationBuilder.Sql()`.
Oto przykład generujący odpowiedniego języka Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Jeśli migracji konieczność obsługi wielu dostawców bazy danych, możesz użyć `MigrationBuilder.ActiveProvider` właściwości. Oto przykład obsługi zarówno programu Microsoft SQL Server, jak i PostgreSQL.

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

Ta metoda działa tylko, jeśli znasz każdego dostawcy których zostaną zastosowane niestandardowych operacji.

<a name="using-a-migrationoperation"></a>Przy użyciu MigrationOperation
---------------------------
Rozdzielenie operacja niestandardowa z SQL, można zdefiniować własny `MigrationOperation` do reprezentowania go. Operacja są następnie przekazywane do dostawcy, dzięki czemu można określić odpowiednie SQL do wygenerowania.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Z tej metody, metoda rozszerzenia musi dodać jeden z tych operacji `MigrationBuilder.Operations`.

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

Ta metoda wymaga każdego dostawcy dowiedzieć się, jak można wygenerować kodu SQL dla tej operacji w ich `IMigrationsSqlGenerator` usługi. Oto przykład zastępowanie generator SQL Server do obsługi nowej operacji.

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

Zaktualizowano zastąpić domyślne migracje sql generatora usługi.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```

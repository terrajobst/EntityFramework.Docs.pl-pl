---
title: Operacje migracji niestandardowych — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812043"
---
# <a name="custom-migrations-operations"></a>Operacje migracji niestandardowych

Interfejs API MigrationBuilder umożliwia wykonywanie wielu różnych rodzajów operacji podczas migracji, ale nie jest to wyczerpujące. Jednak interfejs API jest również rozszerzalny, co pozwala na Definiowanie własnych operacji. Istnieją dwa sposoby rozszerzeń interfejsu API: przy użyciu metody `Sql()` lub definiowania niestandardowych obiektów `MigrationOperation`.

Aby to zilustrować, przyjrzyjmy się implementacji operacji, która tworzy użytkownika bazy danych przy użyciu poszczególnych metod. W naszych migracjach chcemy włączyć pisanie następującego kodu:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>Korzystanie z MigrationBuilder. SQL ()

Najprostszym sposobem implementacji niestandardowej operacji jest zdefiniowanie metody rozszerzenia, która wywołuje `MigrationBuilder.Sql()`. Oto przykład, który generuje odpowiednie instrukcje Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Jeśli migracje muszą obsługiwać wielu dostawców baz danych, można użyć właściwości `MigrationBuilder.ActiveProvider`. Oto przykład obsługujący zarówno Microsoft SQL Server, jak i PostgreSQL.

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

To podejście działa tylko wtedy, gdy wiadomo, że każdy dostawca, w którym zostanie zastosowana operacja niestandardowa.

## <a name="using-a-migrationoperation"></a>Korzystanie z MigrationOperation

Aby rozdzielić operację niestandardową z SQL, można zdefiniować własne `MigrationOperation` do reprezentowania. Operacja jest następnie przenoszona do dostawcy, aby mogła określić odpowiedni kod SQL do wygenerowania.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

W tym podejściu Metoda rozszerzająca musi tylko dodać jedną z tych operacji do `MigrationBuilder.Operations`.

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

Takie podejście wymaga, aby każdy dostawca wiedział, jak wygenerować SQL dla tej operacji w usłudze `IMigrationsSqlGenerator`. Oto przykład przesłaniania generatora SQL Server w celu obsługi nowej operacji.

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

Zastąp domyślną usługę generatora SQL dla migracji z aktualizacją.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```

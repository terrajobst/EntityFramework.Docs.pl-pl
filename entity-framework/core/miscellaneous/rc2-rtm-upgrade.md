---
title: Uaktualnianie z programu EF Core 1.0 RC2 do RTM — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949060"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Uaktualnianie z programu EF Core 1.0 RC2 do RTM

Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów 1.0.0 RC2 RTM.

## <a name="package-versions"></a>Wersje pakietów

Nazwy pakietów najwyższego poziomu, które zazwyczaj można zainstalować na aplikację nie zmienił się od wersji RC2 i RTM.

**Należy uaktualnić zainstalowane pakiety do wersji RTM:**

* Pakiety środowiska uruchomieniowego (na przykład `Microsoft.EntityFrameworkCore.SqlServer`) zmieniła się z `1.0.0-rc2-final` do `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Pakietu zmieniła się z `1.0.0-preview1-final` do `1.0.0-preview2-final`. Należy pamiętać, że narzędzia jest nadal w wersji wstępnej.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migracja istniejących może być konieczne maxLength dodane

W wersji RC2, definicji kolumny, w przypadku migracji wyglądał jak `table.Column<string>(nullable: true)` i długość kolumny został wyszukiwane w niektóre metadane są przechowywane w kodzie migracji. W wersji RTM, długość jest teraz zawarta w utworzony szkielet kodu `table.Column<string>(maxLength: 450, nullable: true)`.

Istniejące migracji, które zostały szkieletu przed użyciem RTM nie będzie miał `maxLength` określony argument. Oznacza to, maksymalna długość obsługiwane przez bazę danych, który będzie używany (`nvarchar(max)` w programie SQL Server). Może to być dobrym rozwiązaniem dla niektórych kolumn, ale także kolumny, które są częścią klucza, klucz obcy lub indeksu, należy zaktualizować maksymalną długość. Zgodnie z Konwencją 450 jest maksymalna długość używane do kluczy, kluczy obcych i indeksowanych kolumn. Jeśli długość skonfigurowano jawnie w modelu, następnie należy użyć tej długości zamiast tego.

**ASP.NET Identity**

Ta zmiana ma wpływ na projekty użycia produktu ASP.NET Identity, które zostały utworzone na podstawie pre-RTM szablonu projektu. Szablon projektu obejmuje migrację użyty do utworzenia bazy danych. Ta migracja musi być edytowany, aby określić maksymalną długość `256` dla następujących kolumn.

*  **AspNetRoles**

    * Nazwa

    * NormalizedName

*  **AspNetUsers**

   * Adres e-mail

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Nie można dokonać tej zmiany spowoduje następujący wyjątek podczas początkowej migracji jest stosowana do bazy danych.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Usuń "import" w pliku project.json

Jeśli zostały przeznaczone dla platformy .NET Core za pomocą RC2, trzeba było dodać `imports` do pliku project.json jako rozwiązanie tymczasowe dla niektórych zależności programu EF Core nie obsługuje .NET Standard. Te można teraz usunąć.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> Począwszy od wersji 1.0 RTM, [zestawu .NET Core SDK](https://www.microsoft.com/net/download/core) nie obsługuje już `project.json` lub tworzenia aplikacji platformy .NET Core przy użyciu programu Visual Studio 2015. Firma Microsoft zaleca [migracji z plików project.json do csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Jeśli używasz programu Visual Studio, zaleca się uaktualnienie do [programu Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>Platformy uniwersalnej systemu Windows: Dodać przekierowania powiązań

Przy próbie uruchomienia programu EF poleceń na wynikach projektów platformy uniwersalnej Windows (UWP) w następujący błąd:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Należy ręcznie dodać przekierowania powiązań do projektu platformy uniwersalnej systemu Windows. Utwórz plik o nazwie `App.config` w projekcie folder główny i dodać przekierowania do wersji poprawny zestaw.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```

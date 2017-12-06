---
title: Uaktualnianie z EF Core RC2 1.0 w wersji RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Uaktualnianie z EF Core 1.0 RC2 do wersji RTM

Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów RC2 do 1.0.0 RTM.

## <a name="package-versions"></a>Wersje pakietów

Nazwy pakietów najwyższego poziomu, które zazwyczaj można zainstalować na aplikację jednakowy RC2 i RTM.

**Należy uaktualnić do wersji RTM zainstalowanych pakietów:**

* Środowisko uruchomieniowe pakietów (np. `Microsoft.EntityFrameworkCore.SqlServer`) zmieniła się z `1.0.0-rc2-final` do `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Pakietu zmieniła się z `1.0.0-preview1-final` do `1.0.0-preview2-final`. Należy pamiętać, że narzędzia nadal wersji wstępnej.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migracja istniejących może być konieczne maxLength dodane

RC2, definicji kolumny w przypadku migracji tablicą jak `table.Column<string>(nullable: true)` i długość kolumny, która została wyszukiwane w niektóre metadane są przechowywane w kodzie migracji. W wersji RTM, długość jest teraz zawarta w kodzie szkieletu `table.Column<string>(maxLength: 450, nullable: true)`.

Wszystkie istniejące migracji, które zostały szkieletu przed użyciem RTM nie będą miały `maxLength` : określono nieprawidłowy argument. Oznacza to, maksymalna długość obsługiwana przez bazę danych, który będzie używany (`nvarchar(max)` w programie SQL Server). Może to być poprawnie dla niektórych kolumn, ale kolumny będące częścią klucza, klucz obcy lub indeks muszą zostać zaktualizowane maksymalną długość. Według Konwencji 450 jest maksymalną długość używane dla kluczy, kluczy obcych i indeksowanych kolumn. Jeśli długość skonfigurowano jawnie w modelu, następnie należy użyć tej długości zamiast tego.

**Tożsamość platformy ASP.NET**

Ta zmiana wpływa na projektów, użyj tożsamości platformy ASP.NET, które zostały utworzone z wersji pre-RTM szablonu projektu. Szablon projektu zawiera migracji używany do tworzenia bazy danych. Tej migracji należy edytować, aby określić maksymalną długość `256` dla następujących kolumn.

*  **AspNetRoles**

    * Nazwa

    * NormalizedName

*  **AspNetUsers**

   * Adres e-mail

   * NormalizedEmail

   * NormalizedUserName

   * Nazwa użytkownika

Nie można wprowadzić tej zmiany spowoduje następujący wyjątek podczas początkowej migracji jest stosowany do bazy danych.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>Oprogramowanie .NET core: Usuń "imports" w pliku project.json

Jeśli zostały przeznaczonych dla platformy .NET Core z RC2, trzeba było dodać `imports` do pliku project.json tymczasowo dla niektórych podstawowych EF zależności nie obsługuje .NET Standard. Te można teraz usunąć.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a>Platformy uniwersalnej systemu Windows: Dodaj przekierowania powiązania

Próba uruchomienia polecenia EF na projekty systemu Windows platformy Uniwersalnej powoduje następujący błąd:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Należy ręcznie Dodaj przekierowania powiązania do projektu platformy uniwersalnej systemu Windows. Utwórz plik o nazwie `App.config` w projekcie folder główny i Dodaj przekierowania do wersji zestawu poprawne.

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

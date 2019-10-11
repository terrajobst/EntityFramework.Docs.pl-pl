---
title: Uaktualnianie z wersji EF Core 1,0 RC2 do wersji RTM — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181362"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Uaktualnianie z wersji EF Core 1,0 RC2 do wersji RTM

Ten artykuł zawiera wskazówki dotyczące przesuwania aplikacji skompilowanej z pakietami RC2 do 1.0.0 RTM.

## <a name="package-versions"></a>Wersje pakietu

Nazwy pakietów najwyższego poziomu, które zazwyczaj instalują się do aplikacji, nie zmieniają się między RC2 i RTM.

**Należy uaktualnić zainstalowane pakiety do wersji RTM:**

* Pakiety środowiska uruchomieniowego (na przykład `Microsoft.EntityFrameworkCore.SqlServer`) zmieniły się z `1.0.0-rc2-final` na `1.0.0`.

* Pakiet `Microsoft.EntityFrameworkCore.Tools` został zmieniony z `1.0.0-preview1-final` na `1.0.0-preview2-final`. Należy zauważyć, że narzędzia nadal są w wersji wstępnej.

## <a name="existing-migrations-may-need-maxlength-added"></a>Istniejące migracje mogą wymagać dodania maxLength

W wersji RC2 Definicja kolumny w migracji wyglądała jak `table.Column<string>(nullable: true)` i długość kolumny została wyszukiwana w niektórych metadanych przechowywanych w kodzie związanym z migracją. W wersji RTM, długość jest teraz dołączana do kodu szkieletowego `table.Column<string>(maxLength: 450, nullable: true)`.

Wszystkie istniejące migracje, które były szkieletem przed użyciem RTM, nie będą miały określonego argumentu `maxLength`. Oznacza to, że maksymalna długość obsługiwana przez bazę danych zostanie użyta (`nvarchar(max)` na SQL Server). Może to być konieczne w przypadku niektórych kolumn, ale kolumny, które są częścią klucza, klucza obcego lub indeksu, muszą zostać zaktualizowane, aby zawierały maksymalną długość. Zgodnie z Konwencją 450 jest maksymalną długość używaną dla kluczy, kluczy obcych i indeksowanych kolumn. Jeśli w modelu określono jawnie długość, należy zamiast tego użyć tej długości.

**ASP.NET Identity**

Ta zmiana wpływa na projekty, które używają ASP.NET Identity i zostały utworzone na podstawie szablonu projektu sprzed-RTM. Szablon projektu zawiera migrację używaną do utworzenia bazy danych. Tę migrację należy edytować, aby określić maksymalną długość `256` dla następujących kolumn.

*  **AspNetRoles**

    * Name

    * NormalizedName

*  **AspNetUsers**

   * Email

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Niewprowadzenie tej zmiany spowoduje, że po zastosowaniu początkowej migracji do bazy danych wystąpi następujący wyjątek.

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: Usuwanie "Imports" w pliku Project. JSON

Jeśli celem jest program .NET Core z RC2, należy dodać `imports` do pliku Project. JSON jako tymczasowe obejście niektórych EF Core zależności, które nie obsługują .NET Standard. Teraz można je usunąć.

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
> Począwszy od wersji 1,0 RTM, [zestaw .NET Core SDK](https://www.microsoft.com/net/download/core) nie obsługuje już `project.json` ani tworzenia aplikacji platformy .NET Core przy użyciu programu Visual Studio 2015. Zalecamy [Migrowanie z pliku Project. JSON do csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Jeśli używasz programu Visual Studio, zalecamy przeprowadzenie uaktualnienia do [programu Visual studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Dodawanie przekierowań powiązań

Próba uruchomienia poleceń EF w projektach platforma uniwersalna systemu Windows (platformy UWP) skutkuje następującym błędem:

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

Należy ręcznie dodać przekierowania powiązań do projektu platformy UWP. Utwórz plik o nazwie `App.config` w folderze głównym projektu i dodaj przekierowania do poprawnych wersji zestawu.

```xml
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

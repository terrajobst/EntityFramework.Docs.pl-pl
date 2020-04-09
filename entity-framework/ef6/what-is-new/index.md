---
title: Co nowego - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136139"
---
# <a name="whats-new-in-ef6"></a>Co nowego w EF6

Zdecydowanie zaleca się użycie najnowszej wersji wydanej programu Entity Framework, aby zapewnić uzyskanie najnowszych funkcji i najwyższą stabilność.
Zdajemy sobie jednak sprawę, że może być konieczne użycie poprzedniej wersji lub że warto eksperymentować z nowymi ulepszeniami w najnowszej wersji wstępnej.
Aby zainstalować określone wersje ef, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-640"></a>EF 6.4.0

Środowisko uruchomieniowe EF 6.4.0 zostało wydane w nuget w grudniu 2019. Głównym celem EF 6.4 jest polerowanie funkcji i scenariuszy, które zostały dostarczone w EF 6.3. Zobacz [listę ważnych poprawek](https://github.com/dotnet/ef6/milestone/14?closed=1) na Github.

## <a name="ef-630"></a>EF 6.3.0

Środowisko uruchomieniowe EF 6.3.0 zostało wydane w nuget we wrześniu 2019. Głównym celem tej wersji było ułatwienie migracji istniejących aplikacji korzystających z EF 6 do .NET Core 3.0. Społeczność przyczyniła się również kilka poprawek i ulepszeń błędów. Szczegółowe informacje można znaleźć w przypadku problemów zamkniętych w każdym [kamieniu milowym](https://github.com/aspnet/EntityFramework6/milestones?state=closed) 6.3.0. Oto niektóre z bardziej znaczących z nich:

- Obsługa platformy .NET Core 3.0
  - Pakiet EntityFramework jest teraz przeznaczony dla platformy .NET Standard 2.1 oprócz programu .NET Framework 4.x.
  - Oznacza to, że EF 6.3 jest wieloplatformowy i obsługiwany w innych systemach operacyjnych oprócz Windows, takich jak Linux i macOS.
  - Polecenia migracji zostały przepisane do wykonywania poza procesem i pracy z projektami w stylu SDK.
- Obsługa identyfikatora hierarchii programu SQL Server.
- Poprawiono zgodność z Roslyn i NuGet PackageReference.
- Dodano `ef6.exe` narzędzie do włączania, dodawania, tworzenia skryptów i stosowania migracji z zestawów. Zastępuje to `migrate.exe`.

### <a name="ef-designer-support"></a>Pomoc techniczna dla projektantów EF

Obecnie nie ma obsługi przy użyciu projektanta EF bezpośrednio w projektach .NET Core lub .NET Standard lub w projekcie .NET Framework w stylu SDK. 

Można obejść to ograniczenie, dodając plik EDMX i wygenerowane klasy dla jednostek i DbContext jako połączone pliki do projektu .NET Core 3.0 lub .NET Standard 2.1 w tym samym rozwiązaniu.

Połączone pliki będą wyglądać następująco w pliku projektu:

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

Należy zauważyć, że plik EDMX jest połączony z jednostką EntityDeploy build akcji. Jest to specjalne zadanie MSBuild (teraz zawarte w pakiecie EF 6.3), który zajmuje się dodawaniem modelu EF do zestawu docelowego jako zasoby osadzone (lub kopiowanie go jako pliki w folderze wyjściowym, w zależności od metadanych artefakt przetwarzania ustawienie w EDMX). Aby uzyskać więcej informacji na temat tego, jak uzyskać tę konfigurację, zobacz nasz [przykład EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).

Ostrzeżenie: upewnij się, że stary styl (tj. nie-SDK stylu) .NET Framework projektu definiowania "prawdziwe" .edmx plik przychodzi _przed_ projektem definiowania łącza wewnątrz pliku .sln. W przeciwnym razie po otwarciu pliku .edmx w projektancie zostanie wyświetlony komunikat o błędzie "Struktura encji nie jest dostępna w ramach docelowej obecnie określonej dla projektu. Można zmienić docelową strukturę projektu lub edytować model w XmlEditor".

## <a name="past-releases"></a>Poprzednie wydania

Poprzednia [wersja](past-releases.md) strona zawiera archiwum wszystkich poprzednich wersji EF i głównych funkcji, które zostały wprowadzone w każdej wersji.

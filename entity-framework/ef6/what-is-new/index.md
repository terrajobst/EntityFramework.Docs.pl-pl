---
title: Co nowego — EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198039"
---
# <a name="whats-new-in-ef6"></a>Co nowego w programie EF6

Zdecydowanie zalecamy użycie najnowszej wydanej wersji Entity Framework, aby uzyskać najnowsze funkcje i największą stabilność.
Jednak firma Microsoft zdaje sobie sprawę, że może być konieczne użycie poprzedniej wersji lub przeprowadzenie eksperymentu z nowymi ulepszeniami w najnowszej wersji wstępnej.
Aby zainstalować określone wersje EF, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>DR 6.3.0

Środowisko uruchomieniowe EF 6.3.0 zostało wydane dla programu NuGet we wrześniu 2019. Głównym celem tej wersji jest ułatwienie migracji istniejących aplikacji korzystających z programu EF 6 do platformy .NET Core 3,0. Społeczność przyczyniła również kilka poprawek i ulepszeń błędów. Aby uzyskać szczegółowe informacje, zobacz problemy zamknięte w każdym 6.3.0 [punktów kontrolnych](https://github.com/aspnet/EntityFramework6/milestones?state=closed) . Poniżej przedstawiono niektóre z bardziej istotnych:

- Obsługa platformy .NET Core 3,0
  - Pakiet EntityFramework teraz jest przeznaczony dla .NET Standard 2,1 oprócz .NET Framework 4. x
  - Polecenia migracji zostały wprowadzone ponownie w celu wykonania poza procesem i pracy z projektami w stylu zestawu SDK
- Obsługa SQL Server HierarchyId
- Ulepszona zgodność z Roslyn i NuGet PackageReference
- Dodano `ef6.exe` narzędzie do włączania, dodawania, tworzenia skryptów i stosowania migracji z zestawów. Spowoduje to zastąpienie`migrate.exe`

### <a name="ef-designer-support"></a>Obsługa projektanta EF

Nie jest obecnie obsługiwane używanie projektanta EF bezpośrednio w projektach .NET Core lub .NET Standard. 

To ograniczenie można obejść przez dodanie pliku EDMX i wygenerowanych klas dla jednostek i DbContext jako połączonych plików do projektu .NET Core 3,0 lub .NET Standard 2,1 w tym samym rozwiązaniu.

Połączone pliki będą wyglądać następująco w pliku projektu:

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

Należy zauważyć, że plik EDMX jest połączony z akcją kompilacji EntityDeploy. Jest to specjalne zadanie MSBuild (teraz zawarte w pakiecie EF 6,3), które ma na celu dodanie modelu EF do zestawu docelowego jako zasobów osadzonych (lub skopiowanie go jako plików w folderze wyjściowym, w zależności od ustawienia przetwarzania artefaktów metadanych w EDMX). Aby uzyskać więcej informacji na temat sposobu uzyskania tego ustawienia, zobacz nasze [przykładowe edmx .NET Core](https://aka.ms/EdmxDotNetCoreSample).

## <a name="past-releases"></a>Poprzednie wydania

Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.

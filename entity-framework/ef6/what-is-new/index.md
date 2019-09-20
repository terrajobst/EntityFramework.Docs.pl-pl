---
title: Co nowego — EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149125"
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
- Dodano program Ef6. exe na potrzeby włączania, dodawania, tworzenia skryptów i stosowania migracji z zestawów. Spowoduje to zastąpienie migracji. exe

## <a name="past-releases"></a>Wcześniejsze wersje

Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.

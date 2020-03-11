---
title: Tworzenie modelu — EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419754"
---
# <a name="creating-a-model"></a>Tworzenie modelu

Model EF przechowuje szczegóły dotyczące sposobu mapowania klas aplikacji i właściwości do tabel i kolumn bazy danych. Istnieją dwa podstawowe sposoby tworzenia modelu EF:

- **Przy użyciu Code First**: deweloper zapisuje kod w celu określenia modelu. EF generuje modele i mapowania w czasie wykonywania na podstawie klas jednostek i dodatkowej konfiguracji modelu udostępnionej przez dewelopera.

- **Korzystanie z programu Dr Designer**: deweloper rysuje pola i linie, aby określić model przy użyciu narzędzia Dr Designer. Model otrzymany jest przechowywany jako plik XML w pliku z rozszerzeniem EDMX. Obiekty domeny aplikacji są zwykle generowane automatycznie z modelu koncepcyjnego.

## <a name="ef-workflows"></a>Przepływy pracy EF

Obie te podejścia mogą służyć do określania docelowej istniejącej bazy danych lub tworzenia nowej bazy danych, co powoduje 4 różne przepływy pracy.
Dowiedz się o tym, który z nich jest najlepszy:  

|                                           | Chcę tylko napisać kod...                                                                                                                   | Chcę używać projektanta...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Chcę utworzyć nową bazę danych**          | [Użyj **Code First** , aby zdefiniować model w kodzie, a następnie wygenerować bazę danych.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Użyj **model First** , aby zdefiniować model przy użyciu pól i wierszy, a następnie wygenerować bazę danych.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Potrzebuję dostępu do istniejącej bazy danych** | [Użyj **Code First** , aby utworzyć model oparty na kodzie, który jest mapowany do istniejącej bazy danych.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Użyj **Database First** , aby utworzyć model pól i linii, który jest mapowany do istniejącej bazy danych.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Obejrzyj wideo: jakiego przepływu pracy dr należy używać?

Ten krótki film wyjaśnia różnice i sposób znajdowania tego, który jest odpowiedni dla Ciebie.

**Przedstawione przez**: [Rowan Miller](https://romiller.com/)

![, który przepływ pracy](../media/whichworkflow-thumb.png) [wmv](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (zip)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Jeśli po obejrzeniu filmu wideo nadal nie uważasz, że chcesz użyć narzędzia Dr Designer lub Code First, zapoznaj się z nim.

## <a name="a-look-under-the-hood"></a>Spójrz na okap

Bez względu na to, czy korzystasz z programu Code First, czy z programu Dr Designer, model EF zawsze ma kilka składników:

- Obiekty domeny aplikacji lub typy jednostek. Jest to często określane jako warstwa obiektu.

- Model koncepcyjny składający się z typów jednostek i relacji specyficznych dla domeny, opisanych przy użyciu [Entity Data Model](~/ef6/resources/glossary.md#entity-data-model). Ta warstwa jest często nazywana literą "C", aby uzyskać _informacje koncepcyjne_.

- Model magazynu reprezentujący tabele, kolumny i relacje zgodnie z definicją w bazie danych. Ta warstwa jest często określana jako "S" dla _magazynu_.  

- Mapowanie między modelem koncepcyjnym i schematem bazy danych. To mapowanie jest często określane jako mapowanie "C-S".

Aparat mapowania EF korzysta z mapowania "C-S", aby przekształcić operacje na jednostki, takie jak tworzenie, odczytywanie, aktualizowanie i usuwanie w równoważnej operacji względem tabel w bazie danych.

Mapowanie między modelem koncepcyjnym i obiektami aplikacji jest często określane jako mapowanie "O-C". W porównaniu do mapowania "C-S" mapowanie "O-C" jest niejawne i jeden do jednego: jednostki, właściwości i relacje zdefiniowane w modelu koncepcyjnym są wymagane do dopasowania kształtów i typów obiektów .NET. Z EF4 i więcej, warstwa obiektów może składać się z obiektów prostych z właściwościami bez żadnych zależności w EF. Są one zazwyczaj określane jako zwykłe obiekty CLR (POCO), a Mapowanie typów i właściwości jest wykonywane na podstawie Konwencji uzgadniania nazw. Wcześniej w EF 3,5 istniały szczególne ograniczenia dotyczące warstwy obiektów, takie jak jednostki, które mają pochodzić od klasy EntityObject i muszą przenosić atrybuty EF w celu wdrożenia mapowania "O-C".

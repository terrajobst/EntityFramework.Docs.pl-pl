---
title: Tworzenie modelu - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: bd9843a93121f53518a307c9d2d43b68ae03369c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419754"
---
# <a name="creating-a-model"></a>Tworzenie modelu

Model EF przechowuje szczegółowe informacje o tym, jak klasy aplikacji i właściwości mapują do tabel i kolumn bazy danych. Istnieją dwa główne sposoby tworzenia modelu EF:

- **Za pomocą kodu pierwszy:** deweloper pisze kod, aby określić model. EF generuje modele i mapowania w czasie wykonywania na podstawie klas jednostek i konfiguracji modelu dodatkowe dostarczone przez dewelopera.

- **Za pomocą projektanta EF:** deweloper rysuje pola i linie, aby określić model przy użyciu projektanta EF. Wynikowy model jest przechowywany jako XML w pliku z rozszerzeniem EDMX. Obiekty domeny aplikacji są zazwyczaj generowane automatycznie z modelu koncepcyjnego.

## <a name="ef-workflows"></a>Przepływy pracy EF

Oba te podejścia mogą służyć do kierowania istniejącej bazy danych lub tworzenia nowej bazy danych, co powoduje 4 różnych przepływów pracy.
Dowiedz się, który z nich jest dla Ciebie najlepszy:  

|                                           | Chcę tylko napisać kod...                                                                                                                   | Chcę użyć projektanta...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tworzym nową bazę danych**          | [Użyj **code first,** aby zdefiniować model w kodzie, a następnie wygenerować bazę danych.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Model **pierwszy** służy do definiowania modelu przy użyciu pól i linii, a następnie generowania bazy danych.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Muszę uzyskać dostęp do istniejącej bazy danych** | [Użyj **code first,** aby utworzyć model oparty na kodzie, który mapuje do istniejącej bazy danych.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Użyj **najpierw bazy danych,** aby utworzyć modele pól i linii, które są mapne do istniejącej bazy danych.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Obejrzyj film: Jakiego przepływu pracy EF należy użyć?

Ten krótki film wyjaśnia różnice i jak znaleźć ten, który jest odpowiedni dla Ciebie.

**Przedstawione przez**: [Rowan Miller](https://romiller.com/)

![Który kciuk](../media/whichworkflow-thumb.png) przepływu pracy [WMV](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Jeśli po obejrzeniu filmu nadal nie czujesz się komfortowo, decydując, czy chcesz użyć projektanta EF lub Code First, dowiedz się obu!

## <a name="a-look-under-the-hood"></a>Spojrzenie pod maską

Niezależnie od tego, czy używasz Code First lub EF Designer, model EF zawsze ma kilka składników:

- Obiekty domeny aplikacji lub typy jednostek same. Jest to często określane jako warstwa obiektu

- Model koncepcyjny składający się z typów i relacji specyficznych dla domeny, opisany przy użyciu [modelu danych jednostki](~/ef6/resources/glossary.md#entity-data-model). Warstwa ta jest często określana literą "C", dla _koncepcyjnej_.

- Model magazynu reprezentujący tabele, kolumny i relacje zgodnie z definicją w bazie danych. Ta warstwa jest często określana z późniejszym "S", do _przechowywania_.  

- Mapowanie między modelem koncepcyjnym a schematem bazy danych. To mapowanie jest często określane jako mapowanie "C-S".

Aparat mapowania EF wykorzystuje mapowanie "C-S" do przekształcania operacji względem jednostek — takich jak tworzenie, odczytywanie, aktualizowanie i usuwanie — w równoważne operacje względem tabel w bazie danych.

Mapowanie między modelem koncepcyjnym a obiektami aplikacji jest często określane jako mapowanie "O-C". W porównaniu do mapowania "C-S", mapowanie "O-C" jest niejawne i jeden do jednego: jednostki, właściwości i relacje zdefiniowane w modelu koncepcyjnym są wymagane do dopasowania kształtów i typów obiektów .NET. Z EF4 i poza nią warstwa obiektów może składać się z prostych obiektów z właściwości bez żadnych zależności od EF. Są one zwykle określane jako plain-old CLR Obiektów (POCO) i mapowanie typów i właściwości jest wykonywane w oparciu o konwencje dopasowywania nazw. Wcześniej w EF 3.5 istniały określone ograniczenia dla warstwy obiektu, takie jak jednostki, które muszą pochodzić z EntityObject klasy i konieczności przenoszenia atrybutów EF do zaimplementowania mapowania "O-C".

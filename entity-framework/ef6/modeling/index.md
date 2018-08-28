---
title: Tworzenie modelu — EF6
author: divega
ms.date: 2018-07-05
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
ms.openlocfilehash: fda9caedfe1dd6c919bb1917bda007ad8ddd4539
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995580"
---
# <a name="creating-a-model"></a>Tworzenie modelu

Szczegółowe informacje dotyczące sposobu mapowania właściwości i klasy aplikacji do bazy danych, tabele i kolumny są przechowywane w modelu platformy EF. Istnieją dwa główne sposoby tworzenia modelu platformy EF:

- **Za pomocą funkcji Code First**: Deweloper pisze kod, aby określić model. EF generuje modeli i mapowania na środowisko uruchomieniowe oparte na klasach jednostki i modelu dodatkowych konfiguracji dostarczoną przez dewelopera.

- **Za pomocą projektanta EF**: Deweloper Rysuje linie, aby określić model przy użyciu projektanta EF i pola. Wynikowy modelu są przechowywane jako kod XML w pliku z rozszerzeniem EDMX. Obiektów domeny aplikacji są zwykle generowane automatycznie na podstawie modelu koncepcyjnego.

## <a name="ef-workflows"></a>Przepływy pracy EF

Obie metody może służyć do docelowych istniejącą bazę danych lub Utwórz nową bazę danych, co 4 różnych przepływów pracy.
Dowiedzieć się o tym, które w jednym jest najbardziej odpowiedni dla Ciebie:  

|                                           | Chcę napisać kod...                                                                                                                   | Chcę użyć projektanta...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Będę tworzyć nową bazę danych**          | [Użyj **Code First** do zdefiniować model w kodzie, a następnie wygenerować bazę danych.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Użyj **pierwszy Model** do zdefiniować model za pomocą linie i pola, a następnie wygenerować bazę danych.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Chcę korzystać z istniejącej bazy danych** | [Użyj **Code First** do utworzenia modelu na podstawie kodu, który mapuje do istniejącej bazy danych.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Użyj **Database First** do tworzenia modeli linie i pola, który jest mapowany do istniejącej bazy danych.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Obejrzyj film wideo: jakie EF przepływu pracy należy używać?

Ten krótki film wideo wyjaśnia różnice i jak znaleźć ten, który jest odpowiedni dla Ciebie.

**Osoba prezentująca**: [Rowan Miller](http://romiller.com/)

![WhichWorkflow_Thumb](../media/whichworkflow-thumb.png) [WMV](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Jeśli po obejrzeniu filmu, które nadal nie czujesz przy wyborze rozwiązania, jeśli chcesz użyć projektanta EF Code First Naucz jednocześnie.

## <a name="a-look-under-the-hood"></a>Wygląd kulisy

Niezależnie od tego, czy używasz Code First i projektancie platformy EF modelu platformy EF zawsze ma kilka składników:

- Obiektów domeny lub jednostki aplikacji typy samodzielnie. To jest często nazywany warstwy obiektu

- Model koncepcyjny składający się z typami encji specyficznego dla domeny i relacje, za pomocą [modelu Entity Data Model](~/ef6/resources/glossary.md#entity-data-model). Ta warstwa sytuację najczęściej nazywa się literą "C" dla _koncepcyjny_.

- Model magazynu reprezentujących tabele, kolumny i relacje, zgodnie z definicją w bazie danych. Ta warstwa sytuację najczęściej nazywa się przy użyciu nowszej "S", dla _magazynu_.  

- Mapowanie między modelu koncepcyjnego i schemat bazy danych. To mapowanie jest często nazywany mapowania "C-S".

Aparat mapowania firmy EF wykorzystuje "C-S" Mapowanie do przekształcania operacji wykonywanych względem jednostek — takie jak tworzenie, odczytu, aktualizacji i usuwania - w do równoważne operacji dla tabel w bazie danych.

Mapowanie między modelu koncepcyjnego i obiekty aplikacji często nazywa się "O-C" mapowanie. W porównaniu do mapowania "C-S", "O-C" jest mapowanie jeden do jednego i niejawne: jednostki, właściwości i relacje zdefiniowane w modelu koncepcyjnym muszą być zgodne, kształty i typy obiektów platformy .NET. Z EF4 i poza warstwy obiektów może składać się z prostych obiektów za pomocą właściwości bez wszelkie zależności od platformy EF. Te są zwykle określane jako zwykły stary obiektów CLR (POCO) i wykonywane jest mapowanie typów i właściwości na nazwę pasującą Konwencji. Wcześniej w wersji 3.5 EF wystąpiły określone ograniczenia dla warstwy obiektów, takich jak jednostki konieczności pochodzi od klasy EntityObject i konieczności wykonania EF atrybutów, aby zaimplementować mapowanie "O-C".

---
title: Przywracanie do obiektu ObjectContext w Entity Framework Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418659"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Przywracanie do obiektu ObjectContext w Entity Framework Designer
W przypadku wcześniejszej wersji Entity Framework modelu utworzonego za pomocą programu EF Designer wygenerowany został kontekst pochodzący z klas ObjectContext i Entity klasy, które pochodzą z obiektu EntityObject.

Począwszy od EF 4.1 zalecamy zamianę na szablon generowania kodu generujący kontekst pochodzący z klas obiektów DbContext i POCO.

W programie Visual Studio 2012 pobierany jest kod DbContext wygenerowany domyślnie dla wszystkich nowych modeli utworzonych za pomocą programu Dr Designer. Istniejące modele będą nadal generować kod oparty na obiekcie ObjectContext, chyba że zdecydujesz się na zamianę na generator kodu oparty na bazie DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Przywracanie z powrotem do generowania kodu ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Wyłącz generowanie kodu w kontekście DbContext

Generowanie pochodnych klas DbContext i POCO jest obsługiwane przez dwa pliki. tt w projekcie, po rozwinięciu pliku. edmx w Eksploratorze rozwiązań zostaną wyświetlone te pliki. Usuń oba te pliki z projektu.

![Pliki generacji kodu](~/ef6/media/codegenfiles.png)

Jeśli używasz programu VB.NET, musisz wybrać przycisk **Pokaż wszystkie pliki** , aby wyświetlić zagnieżdżone pliki.

![Pokaż wszystkie pliki](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. ponownie Włącz generowanie kodu ObjectContext

Otwórz model w programie Dr Designer, kliknij prawym przyciskiem myszy pustą sekcję powierzchni projektowej i wybierz polecenie **Właściwości**.

W okno Właściwości zmienić **strategię generowania kodu** z **Brak** na **domyślne**.

![Strategia generowania kodu](~/ef6/media/codegenstrategy.png)

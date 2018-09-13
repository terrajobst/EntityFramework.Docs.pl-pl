---
title: Powracanie do obiektu ObjectContext w Entity Framework Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488950"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Powracanie do obiektu ObjectContext w programie Entity Framework Designer
Za pomocą poprzedniej wersji programu Entity Framework modelu utworzone za pomocą projektanta EF wygeneruje kontekstu, który pochodzi od obiektu ObjectContext i klas jednostek, które pochodną EntityObject.

Począwszy od EF4.1 zalecane zamianą na szablon generowania kodu, który generuje wyprowadzanie z klas jednostek DbContext i POCO kontekstu.

W programie Visual Studio 2012 uzyskuje się kod DbContext generowane domyślnie dla wszystkich nowych modeli utworzone za pomocą projektanta EF. Istniejące modele będą w dalszym ciągu generowanie kodu na podstawie obiektu ObjectContext, chyba że zdecydujesz o przechodź do generatora kodu na podstawie typu DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Powracanie do generowania kodu obiektu ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Wyłącz generowanie kodu typu DbContext

Generowanie klas pochodnych typu DbContext i POCO odbywa się przez dwa .TT — pliki w projekcie, po rozwinięciu pola pliku edmx w Eksploratorze rozwiązań zobaczą te pliki. Usuń oba te pliki z projektu.

![Pliki generacji kodu](~/ef6/media/codegenfiles.png)

Jeśli używasz VB.NET będzie konieczne wybranie **Pokaż wszystkie pliki** przycisk, aby wyświetlić zagnieżdżone pliki.

![Pokaż wszystkie pliki](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Ponowne włączenie generowania kodu obiektu ObjectContext

Otwieranie modelu w Projektancie platformy EF, kliknij prawym przyciskiem myszy pustą sekcję projekt powierzchni i wybierz **właściwości**.

W oknie Zmień właściwości **strategia generowania kodu** z **Brak** do **domyślne**.

![Strategia generacji kodu](~/ef6/media/codegenstrategy.png)

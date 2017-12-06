---
title: "Baza danych dostawców - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Dostawcy bazy danych

Entity Framework Core korzysta z modelu dostawcy, aby umożliwić EF ma być używany do dostęp do wielu różnych baz danych. Niektóre pojęcia są wspólne dla baz danych i znajdują się w głównej EF podstawowych składników. Takie pojęcia zawierać wyrażenia zapytania składnika LINQ, transakcje, i śledzenia zmian obiektów raz są załadowane z bazy danych. Niektóre pojęcia są specyficzne dla określonego dostawcy. Na przykład dostawcy programu SQL Server umożliwia skonfigurowanie tabel zoptymalizowanych pod kątem pamięci (funkcja specyficzna dla programu SQL Server). Inne pojęcia są specyficzne dla klasy dostawców. Na przykład tworzenie dostawców EF Core relacyjnych baz danych na typowe `Microsoft.EntityFrameworkCore.Relational` biblioteki, która udostępnia interfejsy API do skonfigurowania mapowania kolumn i tabel ograniczeń klucza obcego, itp.

EF podstawowi dostawcy są tworzone przez różnych źródeł. Nie wszyscy dostawcy są obsługiwane w ramach projektu Entity Framework Core. Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.

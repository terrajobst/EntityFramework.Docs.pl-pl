---
title: Zapisywanie danych - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054209"
---
# <a name="saving-data"></a>Zapisywanie danych

Każde wystąpienie kontekstu ma `ChangeTracker` odpowiada celu śledzenia zmian, które muszą być zapisane w bazie danych. Po wprowadzeniu dowolnych zmian do wystąpień klas jednostki, te zmiany są zapisywane w `ChangeTracker` , a następnie zapisane w bazie danych podczas wywoływania `SaveChanges`. Dostawca bazy danych jest odpowiedzialny za tłumaczenia zmiany w operacjach bazy danych (np. `INSERT`, `UPDATE`, i `DELETE` polecenia dla relacyjnej bazy danych).

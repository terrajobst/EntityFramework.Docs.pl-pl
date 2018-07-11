---
title: Zapisywanie danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949398"
---
# <a name="saving-data"></a>Zapisywanie danych

Każde wystąpienie kontekstu ma `ChangeTracker` odpowiada do śledzenia zmian, które muszą zostać zapisane w bazie danych. Po wprowadzeniu dowolnych zmian do wystąpień klas jednostek, zmiany te są rejestrowane w `ChangeTracker` i następnie zapisywane w bazie danych podczas wywoływania `SaveChanges`. Dostawca bazy danych jest odpowiedzialny za tłumaczenie zmiany do określonej bazy danych operacji (na przykład `INSERT`, `UPDATE`, i `DELETE` polecenia do relacyjnej bazy danych).

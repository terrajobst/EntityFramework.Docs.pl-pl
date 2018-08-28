---
title: Zapisywanie danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998183"
---
# <a name="saving-data"></a>Zapisywanie danych

Każde wystąpienie kontekstu ma `ChangeTracker` odpowiada do śledzenia zmian, które muszą zostać zapisane w bazie danych. Po wprowadzeniu dowolnych zmian do wystąpień klas jednostek, zmiany te są rejestrowane w `ChangeTracker` i następnie zapisywane w bazie danych podczas wywoływania `SaveChanges`. Dostawca bazy danych jest odpowiedzialny za tłumaczenie zmiany do określonej bazy danych operacji (na przykład `INSERT`, `UPDATE`, i `DELETE` polecenia do relacyjnej bazy danych).

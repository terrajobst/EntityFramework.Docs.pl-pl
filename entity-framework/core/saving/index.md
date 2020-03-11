---
title: Zapisywanie danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417549"
---
# <a name="saving-data"></a>Zapisywanie danych

Każde wystąpienie kontekstu ma `ChangeTracker`, które jest odpowiedzialne za śledzenie zmian, które muszą być zapisywane w bazie danych. Podczas wprowadzania zmian w wystąpieniach klas jednostek te zmiany są rejestrowane w `ChangeTracker` a następnie zapisywane w bazie danych podczas wywoływania `SaveChanges`. Dostawca bazy danych jest odpowiedzialny za tłumaczenie zmian na operacje specyficzne dla bazy danych (na przykład `INSERT`, `UPDATE`i `DELETE` poleceń dla relacyjnej bazy danych).

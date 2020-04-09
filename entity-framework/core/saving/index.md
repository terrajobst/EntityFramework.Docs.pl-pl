---
title: Zapisywanie danych - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417549"
---
# <a name="saving-data"></a>Zapisywanie danych

Każde wystąpienie `ChangeTracker` kontekstu ma, który jest odpowiedzialny za śledzenie zmian, które muszą być zapisywane w bazie danych. Po wprowadzeniu zmian w wystąpieniach klas encji zmiany `ChangeTracker` te są rejestrowane w bazie `SaveChanges`danych, a następnie zapisywane w bazie danych podczas wywoływania . Dostawca bazy danych jest odpowiedzialny za tłumaczenie zmian na operacje `INSERT` `UPDATE`specyficzne `DELETE` dla bazy danych (na przykład , i polecenia dla relacyjnej bazy danych).

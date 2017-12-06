---
title: "Zarządzanie schematów bazy danych — podstawowe EF"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a>Zarządzanie schematów bazy danych
Podstawowe EF udostępnia dwa sposoby podstawowego elementu synchronizacja EF Core schematu modelu i bazy danych. Aby wybrać między nimi, należy zdecydować, czy model EF Core lub schemat bazy danych jest źródło prawdy.

Model EF Core były źródłem prawdy, należy użyć [migracje][1]. Po wprowadzeniu dowolnych zmian do modelu EF Core takie podejście przyrostowo dotyczy odpowiedniej zmiany schematu bazy danych tak, aby pozostaje zgodne z modelem EF Core.

Użyj [Reverse Engineering] [ 2] Jeśli chcesz, aby były źródłem prawdy schemat bazy danych. Takie podejście umożliwia tworzenie szkieletu obiektu DbContext i klas jednostek typu przez odtwarzanie schemat bazy danych do modelu EF Core.

> [!NOTE]
> [Tworzenie i upuść interfejsów API] [ 3] można również utworzyć schemat bazy danych z modelu EF Core. Są to jednak głównie do testowania, prototypowania i inne scenariusze, gdzie jest dopuszczalne porzucenie bazy danych.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md

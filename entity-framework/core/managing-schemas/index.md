---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994388"
---
# <a name="managing-database-schemas"></a>Zarządzanie schematami bazy danych
EF Core zawiera dwa podstawowe sposoby synchronizacja schematu modelu i bazie danych EF Core. Aby wybrać między nimi, należy zdecydować, czy modelu platformy EF Core lub schemat bazy danych jest źródło prawdziwych danych.

Aby modelu platformy EF Core jako źródło prawdziwych danych, należy użyć [migracje][1]. Po wprowadzeniu dowolnych zmian do modelu platformy EF Core, to podejście przyrostowo dotyczy odpowiedniej zmiany schematu bazy danych, aby pozostaje zgodny z modelu platformy EF Core.

Użyj [odwrotnego Engineering] [ 2] Jeśli schemat bazy danych jako źródło prawdziwych danych. Takie podejście umożliwia tworzenia szkieletu typu DbContext i klasami typu jednostki przez odtwarzanie schemat bazy danych do modelu platformy EF Core.

> [!NOTE]
> [Tworzenie i upuszczanie interfejsów API] [ 3] można również utworzyć schemat bazy danych z modelu platformy EF Core. Jednak są one głównie do testowania, tworzenia prototypów i innych scenariuszy, w których porzucenie bazy danych jest do zaakceptowania.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md

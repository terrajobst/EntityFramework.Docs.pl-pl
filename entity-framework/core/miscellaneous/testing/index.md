---
title: Testowanie składników przy użyciu EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655787"
---
# <a name="testing-components-using-ef-core"></a>Testowanie składników przy użyciu EF Core

Możesz testować składniki przy użyciu czegoś, które przybliżą połączenie z rzeczywistą bazą danych, bez nakładu pracy rzeczywistej operacji we/wy bazy danych.

Dostępne są dwie główne opcje:

* [Tryb in-memory programu SQLite](sqlite.md) umożliwia pisanie wydajnych testów względem dostawcy, który zachowuje się jak relacyjna baza danych.
* [Dostawca inMemory](in-memory.md) jest lekkim dostawcą z minimalnymi zależnościami, ale nie zawsze zachowuje się jak relacyjna baza danych.

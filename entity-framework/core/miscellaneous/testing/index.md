---
title: Testowanie składników przy użyciu EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181309"
---
# <a name="testing-components-using-ef-core"></a>Testowanie składników przy użyciu EF Core

Możesz testować składniki przy użyciu czegoś, które przybliżą połączenie z rzeczywistą bazą danych, bez nakładu pracy rzeczywistej operacji we/wy bazy danych.

Dostępne są dwie główne opcje:
 * [Tryb in-memory programu SQLite](sqlite.md) umożliwia pisanie wydajnych testów względem dostawcy, który zachowuje się jak relacyjna baza danych.
 * [Dostawca inMemory](in-memory.md) jest lekkim dostawcą z minimalnymi zależnościami, ale nie zawsze zachowuje się jak relacyjna baza danych.

---
title: Domyślny schemat — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655967"
---
# <a name="default-schema"></a><span data-ttu-id="01f1f-102">Schemat domyślny</span><span class="sxs-lookup"><span data-stu-id="01f1f-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="01f1f-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="01f1f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="01f1f-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="01f1f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="01f1f-105">Domyślny schemat to schemat bazy danych, w którym obiekty zostaną utworzone w przypadku, gdy schemat nie jest jawnie skonfigurowany dla tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="01f1f-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="01f1f-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="01f1f-106">Conventions</span></span>

<span data-ttu-id="01f1f-107">Według Konwencji dostawca bazy danych wybierze najbardziej odpowiedni domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="01f1f-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="01f1f-108">Na przykład Microsoft SQL Server będzie używać schematu `dbo`, a program SQLite nie użyje schematu (ponieważ schematy nie są obsługiwane w programie SQLite).</span><span class="sxs-lookup"><span data-stu-id="01f1f-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="01f1f-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="01f1f-109">Data Annotations</span></span>

<span data-ttu-id="01f1f-110">Nie można ustawić domyślnego schematu przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="01f1f-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="01f1f-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="01f1f-111">Fluent API</span></span>

<span data-ttu-id="01f1f-112">Możesz użyć interfejsu API Fluent, aby określić domyślny schemat.</span><span class="sxs-lookup"><span data-stu-id="01f1f-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]

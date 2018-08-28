---
title: Klucze alternatywne (ograniczenia unikatowe) - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994194"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="6bc56-102">Klucze alternatywne (ograniczenia unikatowe)</span><span class="sxs-lookup"><span data-stu-id="6bc56-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="6bc56-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="6bc56-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6bc56-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="6bc56-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6bc56-105">Ograniczenia unique został wprowadzony dla każdego klucza alternatywnego w modelu.</span><span class="sxs-lookup"><span data-stu-id="6bc56-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="6bc56-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="6bc56-106">Conventions</span></span>

<span data-ttu-id="6bc56-107">Zgodnie z Konwencją, indeksu i ograniczenia, które zostały wprowadzone dla klucza alternatywnego będą miały nazwę nadaną `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="6bc56-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="6bc56-108">Klucze alternatywne złożonego `<property name>` staje się listę nazw właściwości oddzielonych znakiem podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="6bc56-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6bc56-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="6bc56-109">Data Annotations</span></span>

<span data-ttu-id="6bc56-110">Nie można skonfigurować ograniczeń UNIQUE przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="6bc56-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6bc56-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="6bc56-111">Fluent API</span></span>

<span data-ttu-id="6bc56-112">Interfejs Fluent API umożliwiają skonfigurowanie nazwę indeksu i ograniczenie klucza alternatywnego.</span><span class="sxs-lookup"><span data-stu-id="6bc56-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]

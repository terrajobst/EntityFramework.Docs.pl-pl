---
title: Mapowanie kolumny — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929865"
---
# <a name="column-mapping"></a><span data-ttu-id="7827b-102">Mapowanie kolumn</span><span class="sxs-lookup"><span data-stu-id="7827b-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="7827b-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="7827b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7827b-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="7827b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7827b-105">Mapowanie kolumny określa dane, które kolumny powinien być odpytywane i zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7827b-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7827b-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="7827b-106">Conventions</span></span>

<span data-ttu-id="7827b-107">Zgodnie z Konwencją każda właściwość zostanie skonfigurowana do mapowania kolumny z taką samą nazwę jak właściwość.</span><span class="sxs-lookup"><span data-stu-id="7827b-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7827b-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="7827b-108">Data Annotations</span></span>

<span data-ttu-id="7827b-109">Korzystanie z adnotacji danych, aby skonfigurować kolumny, z którą właściwość jest zamapowana.</span><span class="sxs-lookup"><span data-stu-id="7827b-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="7827b-110">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="7827b-110">Fluent API</span></span>

<span data-ttu-id="7827b-111">Interfejs Fluent API umożliwiają skonfigurowanie kolumny, z którą właściwość jest zamapowana.</span><span class="sxs-lookup"><span data-stu-id="7827b-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]

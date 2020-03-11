---
title: Automatyczne wykrywanie zmian — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416978"
---
# <a name="automatic-detect-changes"></a>Automatyczne wykrywanie zmian
W przypadku korzystania z większości jednostek POCO określenie sposobu zmiany jednostki (i w związku z tym, które aktualizacje muszą zostać wysłane do bazy danych) jest obsługiwane przez algorytm wykrywania zmian. Wykrywanie zmian działa przez wykrywanie różnic między bieżącymi wartościami właściwości jednostki i oryginalnymi wartościami właściwości, które są przechowywane w migawce podczas wykonywania zapytania lub dołączania jednostki. Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.  

Domyślnie Entity Framework automatycznie wykrywa zmiany w przypadku wywołania następujących metod:  

- Nieogólnymi. Find  
- Nieogólnymi. Local  
- Nieogólnymi. Add  
- Nieogólnymi. AddRange
- Nieogólnymi. Remove  
- Nieogólnymi. RemoveRange
- Nieogólnymi. Attach  
- DbContext.SaveChanges  
- DbContext. GetValidationErrors  
- DbContext. entry  
- DbChangeTracker. wpisy  

## <a name="disabling-automatic-detection-of-changes"></a>Wyłączanie automatycznego wykrywania zmian  

W przypadku śledzenia dużej liczby jednostek w kontekście i wywołania jednej z tych metod wiele razy w pętli, można uzyskać znaczące ulepszenia wydajności, wyłączając wykrywanie zmian na czas trwania pętli. Na przykład:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Nie zapomnij ponownie włączyć wykrywania zmian po pętli — użyto instrukcji try/finally, aby upewnić się, że jest ona zawsze ponownie włączona, nawet jeśli kod w pętli zgłosi wyjątek.  

Alternatywą dla wyłączenia i ponownego włączenia jest pozostawienie automatycznego wykrycia, że zmiany są wyłączane przez cały czas i w kontekście wywołania. ChangeTracker. DetectChanges jawnie lub użyj serwerów proxy śledzenia zmian pracujemy. Obie te opcje są zaawansowane i mogą łatwo wprowadzać drobne błędy do aplikacji, dzięki czemu są one używane z opieką.  

Jeśli konieczne jest dodanie lub usunięcie wielu obiektów z kontekstu, należy rozważyć użycie Nieogólnymi. AddRange i Nieogólnymi. RemoveRange. Te metody automatycznie wykrywają zmiany tylko raz po zakończeniu operacji dodawania lub usuwania. 

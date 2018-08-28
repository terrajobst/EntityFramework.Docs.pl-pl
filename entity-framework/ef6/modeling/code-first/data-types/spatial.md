---
title: Przestrzenne — najpierw - Code EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: d15b407203a7dd7ddf92d0759af00f3ad5e41f57
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995215"
---
# <a name="spatial---code-first"></a>Przestrzenne - Code pierwszy
> [!NOTE]
> **EF5 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 5. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

Przewodnik krok po kroku i wideo pokazuje, jak do mapowania typów przestrzennych z Entity Framework Code First. Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.

W tym przewodniku będzie używać Code First, aby utworzyć nową bazę danych, ale można również użyć [Code First istniejącą bazę danych](~/ef6/modeling/code-first/workflows/existing-database.md).

Obsługa przestrzenne typu została wprowadzona w programie Entity Framework 5. Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzennych, wyliczeń i funkcji z wartościami przechowywanymi w tabeli, należy wskazać .NET Framework 4.5. Program Visual Studio 2012 jest przeznaczony dla platformy .NET 4.5 domyślnie.

Używanie typów danych przestrzennych, należy również użyć dostawcy środowiska Entity Framework, który obsługuje przestrzennych. Zobacz [Obsługa dostawców dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) Aby uzyskać więcej informacji.

Istnieją dwa typy danych przestrzennych głównego: geometry i położenia geograficznego. Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne GPS koordynuje). Typ danych Geometria reprezentuje euklidesowa współrzędnych (płaski).

## <a name="watch-the-video"></a>Obejrzyj wideo
To wideo pokazuje, jak do mapowania typów przestrzennych z Entity Framework Code First. Ilustruje też sposób używania zapytania LINQ można znaleźć odległość między dwiema lokalizacjami.

**Osoba prezentująca**: Julia Kornich

**Film wideo**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Musisz być w wersji programu Visual Studio 2012 Ultimate, Premium, Professional i Web Express zainstalowany w celu przeprowadzenia tego instruktażu.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  Na **pliku** menu wskaż **New**, a następnie kliknij przycisk **projektu**
3.  W okienku po lewej stronie kliknij **Visual C\#**, a następnie wybierz pozycję **konsoli** szablonu
4.  Wprowadź **SpatialCodeFirst** jako nazwę projektu i kliknij przycisk **OK**

## <a name="define-a-new-model-using-code-first"></a>Definiowanie nowego modelu za pomocą funkcji Code First

Korzystając z rozwiązania deweloperskiego Code First zwykle Rozpocznij od pisania klas .NET Framework, które definiują model koncepcyjny (domena). Poniższy kod definiuje klasę University.

Uniwersytecie ma właściwość lokalizacji, typu DbGeography. Aby użyć typu DbGeography, należy dodać odwołanie do zestawu System.Data.Entity, a także dodać System.Data.Spatial za pomocą instrukcji.

Otwórz plik Program.cs i wklej następujące instrukcje using na początku pliku:

``` csharp
using System.Data.Spatial;
```

Dodaj następującą definicję klasy University do pliku Program.cs.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>Definiowanie typu pochodnego typu DbContext

Oprócz definiowania jednostek, musisz zdefiniować klasę, która pochodzi od typu DbContext i udostępnia DbSet&lt;TEntity&gt; właściwości. DbSet&lt;TEntity&gt; właściwości umożliwiają kontekstu wiedzieć, jakie typy, które mają zostać uwzględnione w modelu.

Wystąpienie typu DbContext pochodzące zarządza obiektami jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, zmień śledzenie i przechowywanie danych w bazie danych.

Typy DbContext i DbSet są definiowane w zestawie platformy EntityFramework. Firma Microsoft doda odwołanie do tej biblioteki DLL przy użyciu pakietu EntityFramework NuGet.

1.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.
2.  Wybierz **Zarządzaj pakietami NuGet...**
3.  W oknie dialogowym pakiety zarządzania NuGet wybierz **Online** kartę i wybierz polecenie **EntityFramework** pakietu.
4.  Kliknij przycisk **instalacji**

Należy zauważyć, że oprócz zestawu platformy EntityFramework, odwołanie do zestawu System.ComponentModel.DataAnnotations jest także dodawane.

W górnej części pliku Program.cs Dodaj następującą instrukcję using:

``` csharp
using System.Data.Entity;
```

W pliku Program.cs Dodaj definicję kontekstu. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana metoda Main. Dodaj następujący kod do funkcji Main.

Ten kod dodaje dwa nowe obiekty University do kontekstu. Właściwości przestrzenne są inicjowane za pomocą metody DbGeography.FromText. Punkt lokalizacji geograficznej, reprezentowane jako WellKnownText jest przekazywany do metody. Kod następnie zapisuje dane. Następnie zapytania LINQ, która zwraca obiekt University, gdzie jego lokalizacja jest najbardziej zbliżony do określonej lokalizacji jest tworzony i wykonywane.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Skompilować i uruchomić aplikację. Program generuje następujące wyniki:

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Wyświetlanie wygenerowanych bazy danych

Po uruchomieniu aplikacji po raz pierwszy, platformy Entity Framework tworzy bazę danych. Dysponujemy programu Visual Studio 2012, baza danych zostanie utworzony w wystąpieniu programu LocalDB. Domyślnie platforma Entity Framework nazwy bazy danych po w pełni kwalifikowanej nazwie pochodnej kontekstu (w tym przykładzie, który jest **SpatialCodeFirst.UniversityContext**). Kolejne razy, zostanie użyta istniejącej bazy danych.  

Należy pamiętać, że jeśli wprowadzisz zmiany do modelu, po utworzeniu bazy danych, należy użyć migracje Code First do zaktualizowania schematu bazy danych. Zobacz [Code First dla nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) na przykład za pomocą migracji.

Aby wyświetlić i bazy danych, wykonaj następujące czynności:

1.  W menu głównym programu Visual Studio 2012, wybierz **widoku**  - &gt; **Eksplorator obiektów SQL Server**.
2.  Jeśli LocalDB, nie ma na liście serwerów, kliknij prawym przyciskiem myszy **programu SQL Server** i wybierz **dodawania serwera SQL** Użyj domyślnej **uwierzytelniania Windows** połączyć się z wystąpienia LocalDB
3.  Rozwiń węzeł LocalDB
4.  Unfold **baz danych** folder, aby zobaczyć nową bazę danych, a następnie przejdź do **uniwersytety** tabeli
5.  Aby wyświetlić dane, kliknij prawym przyciskiem myszy w tabeli i wybrać **widoku danych**

## <a name="summary"></a>Podsumowanie

W tym przewodniku zobaczyliśmy, jak za pomocą typów przestrzennych programu Entity Framework Code First. 

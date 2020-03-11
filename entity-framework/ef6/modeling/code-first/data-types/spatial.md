---
title: Przestrzenny Code First EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419102"
---
# <a name="spatial---code-first"></a>Code First przestrzenne
> [!NOTE]
> **EF5 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 5. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

Film wideo i przewodnik krok po kroku przedstawiają sposób mapowania typów przestrzennych przy użyciu Entity Framework Code First. Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.

Ten Instruktaż będzie używać Code First do tworzenia nowej bazy danych, ale można również użyć [Code First do istniejącej bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md).

Obsługa typów przestrzennych została wprowadzona w Entity Framework 5. Należy pamiętać, że aby korzystać z nowych funkcji, takich jak typ przestrzenny, wyliczenia i funkcje zwracające tabelę, należy określić wartość docelową .NET Framework 4,5. Program Visual Studio 2012 Domyślnie kieruje .NET 4,5.

Aby można było używać typów danych przestrzennych, należy również użyć dostawcy Entity Framework z obsługą przestrzenną. Aby uzyskać więcej informacji, zobacz [Obsługa dostawcy dla typów przestrzennych](~/ef6/fundamentals/providers/spatial-support.md) .

Istnieją dwa główne typy danych przestrzennych: Geografia i geometria. Typ danych Geografia przechowuje dane ellipsoidal (na przykład współrzędne geograficzne i Długość geograficzna). Typ danych geometrii reprezentuje układ współrzędnych Euclidean (płaski).

## <a name="watch-the-video"></a>Obejrzyj film
W tym filmie wideo pokazano, jak mapować typy przestrzenne za pomocą Entity Framework Code First. Pokazano również, jak używać zapytania LINQ, aby znaleźć odległość między dwiema lokalizacjami.

**Przedstawione przez**: Julia Kornich

**Wideo**: [wmv](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik, musisz mieć zainstalowaną wersję Visual Studio 2012, Ultimate, Premium, Professional lub Web Express Edition.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

1.  Otwórz program Visual Studio 2012
2.  W menu **plik** wskaż polecenie **Nowy**, a następnie kliknij pozycję **projekt** .
3.  W lewym okienku kliknij pozycję **Visual C\#** , a następnie wybierz szablon **konsoli**
4.  Wprowadź **SpatialCodeFirst** jako nazwę projektu, a następnie kliknij przycisk **OK** .

## <a name="define-a-new-model-using-code-first"></a>Zdefiniuj nowy model przy użyciu Code First

W przypadku korzystania z Code First projektowania zwykle zaczynasz od pisania klas .NET Framework, które definiują model koncepcyjny (domeny). Poniższy kod definiuje klasę University.

Uniwersytet ma właściwość Location typu DbGeography. Aby użyć typu DbGeography, należy dodać odwołanie do zestawu System. Data. Entity, a także dodać instrukcję system. Data. przestrzenny using.

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

## <a name="define-the-dbcontext-derived-type"></a>Zdefiniuj typ pochodny DbContext

Oprócz definiowania jednostek należy zdefiniować klasę, która dziedziczy z DbContext i uwidacznia Nieogólnymi&lt;&gt; właściwości. Nieogólnymi&lt;&gt; właściwości umożliwiają kontekstowi znać, które typy mają być uwzględnione w modelu.

Wystąpienie typu pochodnego DbContext zarządza obiektami obiektów w czasie wykonywania, co obejmuje wypełnianie obiektów danymi z bazy danych, śledzenie zmian i utrwalanie danych w bazie danych.

Typy DbContext i Nieogólnymi są zdefiniowane w zestawie EntityFramework. Dodamy odwołanie do tej biblioteki DLL przy użyciu pakietu NuGet EntityFramework.

1.  W Eksplorator rozwiązań kliknij prawym przyciskiem myszy nazwę projektu.
2.  Wybierz pozycję **Zarządzaj pakietami NuGet...**
3.  W oknie dialogowym Zarządzanie pakietami NuGet wybierz kartę **online** i wybierz pakiet **EntityFramework** .
4.  Kliknij przycisk **Instaluj**

Należy zauważyć, że oprócz zestawu EntityFramework dodawane jest również odwołanie do zestawu System. ComponentModel. DataAnnotations.

W górnej części pliku Program.cs Dodaj następującą instrukcję using:

``` csharp
using System.Data.Entity;
```

W Program.cs Dodaj definicję kontekstu. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Utrwalanie i pobieranie danych

Otwórz plik Program.cs, w którym jest zdefiniowana Metoda Main. Dodaj następujący kod do funkcji main.

Kod dodaje dwa nowe obiekty University do kontekstu. Właściwości przestrzenne są inicjowane przy użyciu metody DbGeography. FromText. Punkt geografii reprezentowany jako WellKnownText jest przenoszona do metody. Następnie kod zapisuje dane. Następnie zapytanie LINQ, które zwraca obiekt University, gdzie jego lokalizacja znajduje się najbliżej określonej lokalizacji, jest konstruowany i wykonywany.

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

Skompiluj i uruchom aplikację. Program tworzy następujące dane wyjściowe:

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Wyświetlanie wygenerowanej bazy danych

Po pierwszym uruchomieniu aplikacji Entity Framework tworzy bazę danych. Ze względu na to, że zainstalowano program Visual Studio 2012, baza danych zostanie utworzona w wystąpieniu LocalDB. Domyślnie Entity Framework nazw bazy danych po w pełni kwalifikowana nazwa kontekstu pochodnego (w tym przykładzie jest to **SpatialCodeFirst. UniversityContext**). Kolejne użycie istniejącej bazy danych będzie możliwe.  

Należy pamiętać, że jeśli wprowadzisz zmiany w modelu po utworzeniu bazy danych, należy użyć Migracje Code First, aby zaktualizować schemat bazy danych. Zapoznaj się z tematem [Code First do nowej bazy danych](~/ef6/modeling/code-first/workflows/new-database.md) , aby zapoznać się z przykładem użycia migracji.

Aby wyświetlić bazę danych i dane, wykonaj następujące czynności:

1.  W menu głównym programu Visual Studio 2012 wybierz pozycję **wyświetl** -&gt; **Eksplorator obiektów SQL Server**.
2.  Jeśli LocalDB nie znajduje się na liście serwerów, kliknij prawym przyciskiem myszy na **SQL Server** i wybierz polecenie **Dodaj SQL Server** Użyj domyślnego **uwierzytelniania systemu Windows** , aby nawiązać połączenie z wystąpieniem LocalDB
3.  Rozwiń węzeł LocalDB
4.  Unfold folder **baz danych** w celu wyświetlenia nowej bazy danych i przechodzenia do tabeli **uniwersytetów**
5.  Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**

## <a name="summary"></a>Podsumowanie

W tym instruktażu przedstawiono sposób używania typów przestrzennych z Entity Framework Code First. 

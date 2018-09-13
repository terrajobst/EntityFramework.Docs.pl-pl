---
title: Szablony generowania kodu projektanta - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: 8479d4e76e6db43072c382792c69250ae032af62
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490200"
---
# <a name="designer-code-generation-templates"></a>Szablonów generowania kodu projektanta
Podczas tworzenia modelu przy użyciu programu Entity Framework Designer klas i kontekst pochodna są generowane automatycznie dla Ciebie. Oprócz generowania kodu domyślne oferujemy są również szablony, których można dostosować program code, który pobiera wygenerowany. Te szablony stanowią one szablonów tekstowych T4, dzięki czemu możesz dostosowywać szablony, jeśli to konieczne.

Kod, który pobiera wygenerowany domyślnie, zależy od wersji programu Visual Studio Tworzenie modelu w:

-   Modele utworzone w programie Visual Studio 2012 i 2013 generuje prosty klas obiektów POCO i kontekście, który pochodzi z uproszczonego DbContext.
-   Modele utworzone w programie Visual Studio 2010, spowoduje wygenerowanie klas jednostek, które wynikają z EntityObject i kontekstu, która pochodzi z obiektu ObjectContext.
  > [!NOTE]    
  > Firma Microsoft zaleca, przełączanie do szablonu DbContext Generator po dodaniu modelu.

Ta strona obejmuje dostępnych szablonów i następnie zawiera instrukcje dotyczące dodawania szablonu do modelu.

## <a name="available-templates"></a>Dostępne szablony

Poniższe szablony są dostarczane przez zespół programu Entity Framework:

### <a name="dbcontext-generator"></a>DbContext Generator

Ten szablon generuje prosty klas obiektów POCO i kontekstu, która pochodzi od typu DbContext przy użyciu platformy EF6.
To jest szablon zalecana, chyba że masz powód, aby użyć jednego z innych szablonów wymienionych poniżej.
Jest również szablon generowania kodu, Pobierz domyślnie, jeśli używane są nowe wersje programu Visual Studio (Visual Studio 2013 lub nowszy): po utworzeniu nowego modelu ten szablon jest używany domyślnie i pliki T4 (.tt) zostały zagnieżdżone w pliku edmx.

#### <a name="older-versions-of-visual-studio"></a>Starsze wersje programu Visual Studio
- **Program Visual Studio 2012:** można pobrać **EF 6.x DbContextGenerator** szablonów, musisz zainstalować najnowszą **Entity Framework Tools for Visual Studio** — zobacz [pobrać jednostki Framework](~/ef6/fundamentals/install.md) strony, aby uzyskać więcej informacji.
- **Program Visual Studio 2010:** **EF 6.x DbContextGenerator** szablony nie są dostępne dla programu Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Generator DbContext dla platformy EF 5.x

Jeśli używasz starszej wersji pakietu EntityFramework NuGet (po jednej z wersji głównej 5) należy użyć **EF 5.x DbContext Generator** szablonu.

Jeśli używasz programu Visual Studio 2013 lub 2012 ten szablon jest już zainstalowana.

Jeśli używasz programu Visual Studio 2010 będzie konieczne wybranie **Online** karcie podczas dodawania szablonu, aby go pobrać z galerii Visual Studio. Alternatywnie można zainstalować szablonu bezpośrednio z galerii programu Visual Studio wcześniej. Ponieważ szablony są wliczone w nowszych wersjach programu Visual Studio w wersji w galerii można zainstalować tylko w programie Visual Studio 2010.

- [EF 5.x DbContext Generator dla języka C#](http://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [EF 5.x DbContext Generator dla witryn sieci Web w języku C#](http://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [EF 5.x DbContext Generator VB.NET](http://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [EF 5.x DbContext Generator dla witryn sieci Web VB.NET](http://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Generator DbContext dla platformy EF 4.x

Jeśli używasz starszej wersji pakietu EntityFramework NuGet (po jednym z głównych wersję 4) należy użyć **EF 4.x DbContext Generator** szablonu. To można znaleźć w **Online** karcie podczas dodawania szablonu lub szablonu można zainstalować bezpośrednio z galerii programu Visual Studio wcześniej.

- [EF 4.x DbContext Generator dla języka C#](http://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [EF 4.x DbContext Generator dla witryn sieci Web w języku C#](http://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [EF 4.x DbContext Generator VB.NET](http://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [EF 4.x DbContext Generator dla witryn sieci Web VB.NET](http://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>EntityObject Generator

Ten szablon spowoduje wygenerowanie klas jednostek, które wynikają z EntityObject i kontekstu, która pochodzi z obiektu ObjectContext.

> [!NOTE]
> Należy wziąć pod uwagę przy użyciu generatora DbContext

DbContext Generator jest teraz zalecane szablon dla nowych aplikacji. DbContext Generator wykorzystuje prostsze API DbContext. EntityObject Generator jest nadal dostępne do obsługi istniejących aplikacji.

**Visual Studio 2010, 2012 &amp; 2013**

Będzie konieczne wybranie **Online** karcie podczas dodawania szablonu, aby go pobrać z galerii Visual Studio. Alternatywnie można zainstalować szablonu bezpośrednio z galerii programu Visual Studio wcześniej.

- [EF 6.x EntityObject Generator dla języka C#](http://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [EF 6.x EntityObject Generator dla witryn sieci Web w języku C#](http://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [EF 6.x EntityObject Generator VB.NET](http://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [EF 6.x EntityObject Generator dla witryn sieci Web VB.NET](http://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Generator EntityObject dla platformy EF 5.x**


Jeśli używasz programu Visual Studio 2012 lub 2013 będzie konieczne wybranie **Online** karcie podczas dodawania szablonu, aby go pobrać z galerii Visual Studio. Alternatywnie można zainstalować szablonu bezpośrednio z galerii programu Visual Studio wcześniej. Ponieważ szablony są wliczone w programie Visual Studio 2010 wersji w galerii można zainstalować tylko w programie Visual Studio 2012 &amp; 2013.

- [EF 5.x EntityObject Generator dla języka C#](http://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [EF 5.x EntityObject Generator dla witryn sieci Web w języku C#](http://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [EF 5.x EntityObject Generator VB.NET](http://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [EF 5.x EntityObject Generator dla witryn sieci Web VB.NET](http://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Wystarczy ObjectContext generowania kodu bez konieczności edytowania szablonu możesz [powrócić do generowania kodu EntityObject](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Jeśli używasz programu Visual Studio 2010, ten szablon jest już zainstalowana. Jeśli tworzysz nowy model w programie Visual Studio 2010, ten szablon jest używany przez domyślny, ale .tt, w których pliki nie są uwzględniane w projekcie. Jeśli chcesz dostosować szablon, należy dodać go do projektu.

### <a name="self-tracking-entities-ste-generator"></a>Śledzenie własnym Generator jednostki (set)

Ten szablon generuje klas jednostek Self-Tracking i kontekstu, która pochodzi z obiektu ObjectContext. W aplikacji EF kontekst jest odpowiedzialny za śledzenie zmian w jednostkach. Jednak w scenariuszach N-warstwowa kontekst może nie być dostępne w warstwie, która modyfikuje jednostki. Jednostki własnym śledzenie pomóc śledzić zmiany w dowolnej warstwy. Aby uzyskać więcej informacji, zobacz [jednostek Self-Tracking](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> WKLEJ szablon niezalecane

Nie zalecamy już przy użyciu szablonu Wklej kod w nowej aplikacji, jej nadal będzie dostępna do obsługi istniejących aplikacji. Odwiedź stronę [artykułu odłączone jednostki](~/ef6/fundamentals/disconnected-entities/index.md) innych opcji, firma Microsoft zaleca w scenariuszach N-warstwowej.

> [!NOTE]
> Dostępna jest wersja 6.x nie EF, Wklej kod szablonu.


> [!NOTE]
> Nie ma żadnych wersji programu Visual Studio 2013 szablonu Wklej kod.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Jeśli używasz programu Visual Studio 2012 będzie konieczne wybranie **Online** karcie podczas dodawania szablonu, aby go pobrać z galerii Visual Studio. Alternatywnie można zainstalować szablonu bezpośrednio z galerii programu Visual Studio wcześniej. Ponieważ szablony są zawarte w Visual Studio 2010 w wersji w galerii można zainstalować tylko w programie Visual Studio 2012.

- [EF 5.x WKLEJ Generator dla języka C#](http://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [EF 5.x WKLEJ Generator dla witryn sieci Web w języku C#](http://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [Generator WKLEJ 5.x EF VB.NET](http://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [EF 5.x WKLEJ Generator dla witryn sieci Web VB.NET](http://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Program Visual Studio 2010 **

Jeśli używasz programu Visual Studio 2010, ten szablon jest już zainstalowana.

### <a name="poco-entity-generator"></a>Obiektów POCO Generator jednostki

Ten szablon generuje klas obiektów POCO i kontekstu, która pochodzi z obiektu ObjectContext

> [!NOTE]
> Należy wziąć pod uwagę przy użyciu generatora DbContext

DbContext Generator jest teraz zalecane szablon Generowanie klas POCO w nowych aplikacji. DbContext Generator korzysta z nowego interfejsu API typu DbContext i może generować prostsze POCO klasy. Generator jednostki obiektów POCO jest nadal dostępne do obsługi istniejących aplikacji.

> [!NOTE]
> Nie ma żadnych EF 5.x i 6.x EF wersję szablonu Wklej kod.

> [!NOTE]
> Nie ma żadnych wersji programu Visual Studio 2013 szablonu POCO.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Program Visual Studio 2012 &amp; programu Visual Studio 2010

Będzie konieczne wybranie **Online** karcie podczas dodawania szablonu, aby go pobrać z galerii Visual Studio. Alternatywnie można zainstalować szablonu bezpośrednio z galerii programu Visual Studio wcześniej.

- [EF 4.x POCO Generator dla języka C#](http://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [EF 4.x POCO Generator dla witryn sieci Web w języku C#](http://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [EF 4.x POCO Generator VB.NET](http://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [EF 4.x POCO Generator dla witryn sieci Web VB.NET](http://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Co to są szablony "Witryny sieci Web"

Szablony "Witryny sieci Web" (na przykład **EF 5.x Generator DbContext dla języka C\# witryn sieci Web**) są przeznaczone do użytku w projektów witryny sieci Web utworzone za pomocą **pliku —&gt; New -&gt; witryny sieci Web...** . Są one różne od aplikacji sieci Web utworzone za pomocą **pliku —&gt; New -&gt; projektu...** , które używają szablonów standardowych. Firma Microsoft oferuje osobnymi szablonami, ponieważ system szablonu elementu w programie Visual Studio wymaga, aby je.

## <a name="using-a-template"></a>Przy użyciu szablonu

Aby rozpocząć korzystanie z szablonów generowania kodu, kliknij prawym przyciskiem myszy pusty miejscu na powierzchni projektowej, w Projektancie platformy EF i wybierz **Dodaj element generowanie kodu...** .

![Dodaj element generacji kodu](~/ef6/media/add-code-gen-item.png)

Jeśli masz już zainstalowaną szablonu chcesz użyć (lub został on uwzględniony w programie Visual Studio), a następnie będzie on dostępny w obszarze **kodu** lub **danych** sekcji w menu po lewej stronie.

![Zainstalowany](~/ef6/media/installed.png)

Jeśli nie masz jeszcze szablonu zainstalowany, wybierz opcję **Online** z menu po lewej stronie i wyszukaj odpowiedni szablon.

![Wyszukaj](~/ef6/media/search.png) 

Jeśli używasz programu Visual Studio 2012, nowe pliki .tt będzie można zagnieździć w file.* edmx

> [!NOTE]
> W przypadku modeli utworzonych w programie Visual Studio 2012, musisz usunąć szablonów używanych dla domyślnego generowania kodu w przeciwnym razie otrzymasz klasy zduplikowane oraz wygenerowany kontekst. Domyślne pliki są  **&lt;nazwę modelu&gt;.tt** i  **&lt;nazwę modelu&gt;. context.tt**. 

![Szablony VS2012](~/ef6/media/vs2012-templates.png)

Jeśli używasz programu Visual Studio 2010 pliki tt są dodawane bezpośrednio do swojego projektu.  

![Szablony VS2010](~/ef6/media/vs2010-templates.png)

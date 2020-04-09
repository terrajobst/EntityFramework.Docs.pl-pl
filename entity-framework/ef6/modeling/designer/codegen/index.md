---
title: Szablony generowania kodu projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78418677"
---
# <a name="designer-code-generation-templates"></a>Szablony generowania kodu projektanta
Podczas tworzenia modelu przy użyciu Entity Framework Designer klasy i kontekst pochodny są generowane automatycznie dla Ciebie. Oprócz domyślnego generowania kodu udostępniamy również szereg szablonów, które mogą służyć do dostosowywania kodu, który zostanie wygenerowany. Szablony te są dostarczane jako szablony tekstu T4, co pozwala dostosować szablony w razie potrzeby.

Kod, który zostanie wygenerowany domyślnie zależy od wersji programu Visual Studio, w której model zostanie utworzony:

-   Modele utworzone w programie Visual Studio 2012 & 2013 r. wygenerują proste klasy jednostek POCO i kontekst, który wywodzi się z uproszczonego DbContext.
-   Modele utworzone w programie Visual Studio 2010 wygenerują klasy jednostek, które pochodzą z EntityObject i kontekstu, który pochodzi z ObjectContext.
  > [!NOTE]    
  > Zalecamy przełączenie się do szablonu generatora DbContext po dodaniu modelu.

Ta strona obejmuje dostępne szablony, a następnie zawiera instrukcje dotyczące dodawania szablonu do modelu.

## <a name="available-templates"></a>Dostępne szablony

Następujące szablony są dostarczane przez zespół entity framework:

### <a name="dbcontext-generator"></a>DbContext Generator

Ten szablon wygeneruje proste klasy jednostek POCO i kontekst, który pochodzi z DbContext przy użyciu EF6.
Jest to zalecany szablon, chyba że masz powód, aby użyć jednego z innych szablonów wymienionych poniżej.
Jest to również szablon generowania kodu, który jest domyślnie utrzymywane, jeśli używasz najnowszych wersji programu Visual Studio (Visual Studio 2013 nowszych): Podczas tworzenia nowego modelu ten szablon jest używany domyślnie i pliki T4 (.tt) są zagnieżdżone w pliku .edmx.

#### <a name="older-versions-of-visual-studio"></a>Starsze wersje programu Visual Studio
- **Visual Studio 2012:** Aby uzyskać **szablony EF 6.x DbContextGenerator,** należy zainstalować najnowsze **narzędzia entity framework dla programu Visual Studio** — zobacz stronę Pobierz [platformę encji, aby](~/ef6/fundamentals/install.md) uzyskać więcej informacji.
- **Visual Studio 2010:** **Szablony EF 6.x DbContextGenerator** nie są dostępne dla programu Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Generator DbContext dla EF 5.x

Jeśli używasz starszej wersji pakietu EntityFramework NuGet (jeden z główną wersją 5) należy użyć **szablonu generatora EF 5.x DbContext.**

Jeśli używasz programu Visual Studio 2013 lub 2012, ten szablon jest już zainstalowany.

Jeśli używasz programu Visual Studio 2010, należy wybrać kartę **Online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie można zainstalować szablon bezpośrednio z galerii programu Visual Studio z wyprzedzeniem. Ponieważ szablony są zawarte w nowszych wersjach programu Visual Studio wersje w galerii można zainstalować tylko w programie Visual Studio 2010.

- [EF 5.x Generator DbContext dla C #](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [Ef 5.x Generator DbContext dla witryn sieci Web w języku C#](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [EF 5.x Generator DbContext dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [EF 5.x DbContext Generator dla VB.NET stron internetowych](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Generator DbContext dla EF 4.x

Jeśli używasz starszej wersji pakietu EntityFramework NuGet (jeden z główną wersją 4) należy użyć **szablonu generatora EF 4.x DbContext.** Można to znaleźć na karcie **W trybie online** podczas dodawania szablonu lub można zainstalować szablon bezpośrednio z programu Visual Studio Gallery z wyprzedzeniem.

- [EF 4.x Generator DbContext dla C #](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [Ef 4.x Generator DbContext dla witryn sieci Web w języku C#](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [EF 4.x Generator DbContext dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [EF 4.x DbContext Generator dla VB.NET stron internetowych](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>EntityObject Generator

Ten szablon wygeneruje klasy jednostek, które pochodzą z EntityObject i kontekstu, który pochodzi z ObjectContext.

> [!NOTE]
> Rozważ użycie generatora DbContext

Generator DbContext jest teraz zalecanym szablonem dla nowych aplikacji. Generator DbContext korzysta z prostszego interfejsu API DbContext. Generator entityobject nadal jest dostępny do obsługi istniejących aplikacji.

**Visual Studio 2010, 2012 &amp; 2013**

Podczas dodawania szablonu należy wybrać kartę **Online,** aby pobrać go z galerii programu Visual Studio. Alternatywnie można zainstalować szablon bezpośrednio z galerii programu Visual Studio z wyprzedzeniem.

- [EF 6.x Generator entityobject dla C #](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Ef 6.x EntityObject Generator dla witryn sieci Web w języku C#](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [EF 6.x Generator entityobject dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [EF 6.x EntityObject Generator dla VB.NET witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Generator entityobject dla EF 5.x**


Jeśli używasz programu Visual Studio 2012 lub 2013, musisz wybrać kartę **Online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie można zainstalować szablon bezpośrednio z galerii programu Visual Studio z wyprzedzeniem. Ponieważ szablony są zawarte w programie Visual Studio 2010 wersje w galerii można &amp; zainstalować tylko w programie Visual Studio 2012 2013.

- [EF 5.x Generator entityobject dla C #](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Ef 5.x EntityObject Generator dla witryn sieci Web w języku C#](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [EF 5.x Generator entityobject dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [EF 5.x EntityObject Generator dla VB.NET stron internetowych](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Jeśli chcesz po prostu generacji kodu ObjectContext bez konieczności edytowania szablonu można [przywrócić do generowania kodu EntityObject](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Jeśli używasz programu Visual Studio 2010, ten szablon jest już zainstalowany. Jeśli tworzysz nowy model w programie Visual Studio 2010, ten szablon jest używany domyślnie, ale pliki .tt nie są uwzględnione w projekcie. Jeśli chcesz dostosować szablon, musisz dodać go do projektu.

### <a name="self-tracking-entities-ste-generator"></a>Generator jednostek samoobjudy (STE)

Ten szablon wygeneruje klasy jednostki samośledzenia i kontekst, który pochodzi z ObjectContext. W aplikacji EF kontekst jest odpowiedzialny za śledzenie zmian w jednostkach. Jednak w scenariuszach N-tier kontekst może nie być dostępny w warstwie, która modyfikuje jednostki. Jednostki samoobsługowe ułatwią śledzenie zmian w dowolnej warstwie. Aby uzyskać więcej informacji, zobacz [Jednostki samoobsługowe](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Szablon STE nie jest zalecany

Nie zaleca się już używania szablonu STE w nowych aplikacjach, nadal jest on dostępny do obsługi istniejących aplikacji. Odwiedź [artykuł rozłączonych jednostek,](~/ef6/fundamentals/disconnected-entities/index.md) aby uzyskać inne opcje, które zalecamy w scenariuszach n-warstwowych.

> [!NOTE]
> Nie ma wersji EF 6.x szablonu STE.


> [!NOTE]
> Nie ma wersji szablonu STE w programie Visual Studio 2013.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Jeśli używasz programu Visual Studio 2012, musisz wybrać kartę **Online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie można zainstalować szablon bezpośrednio z galerii programu Visual Studio z wyprzedzeniem. Ponieważ szablony są zawarte w programie Visual Studio 2010 wersje w galerii można zainstalować tylko w programie Visual Studio 2012.

- [EF 5.x Generator STE dla C #](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [Ef 5.x Generator STE dla witryn sieci Web języka C#](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [EF 5.x Generator STE do VB.NET](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [EF 5.x Generator STE dla VB.NET stron internetowych](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010**

Jeśli używasz programu Visual Studio 2010, ten szablon jest już zainstalowany.

### <a name="poco-entity-generator"></a>Generator jednostek POCO

Ten szablon wygeneruje klasy jednostek POCO i kontekst, który wywodzi się z ObjectContext

> [!NOTE]
> Rozważ użycie generatora DbContext

Generator DbContext jest teraz zalecanym szablonem do generowania klas POCO w nowych aplikacjach. Generator DbContext korzysta z nowego interfejsu API DbContext i może generować prostsze klasy POCO. Generator jednostek POCO jest nadal dostępny do obsługi istniejących aplikacji.

> [!NOTE]
> Nie ma ef 5.x lub EF 6.x wersja szablonu STE.

> [!NOTE]
> Nie ma wersji szablonu POCO w programie Visual Studio 2013.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Podczas dodawania szablonu należy wybrać kartę **Online,** aby pobrać go z galerii programu Visual Studio. Alternatywnie można zainstalować szablon bezpośrednio z galerii programu Visual Studio z wyprzedzeniem.

- [Ef 4.x Generator POCO dla C #](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Ef 4.x Generator POCO dla witryn sieci Web w języku C#](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [EF 4.x Generator POCO do VB.NET](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [Ef 4.x Generator POCO dla VB.NET stron internetowych](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Co to są szablony "Witryn sieci Web"

Szablony "Witryny sieci Web" (na przykład **EF 5.x\# DbContext Generator dla witryn sieci C)** są przeznaczone do użytku w projektach witryny sieci Web utworzonych za pośrednictwem **File -&gt; New -&gt; Web Site...**. Różnią się one od aplikacji sieci Web, utworzonych za pośrednictwem **file -&gt; new -&gt; Project...**, które używają standardowych szablonów. Udostępniamy oddzielne szablony, ponieważ wymaga tego system szablonów elementów w programie Visual Studio.

## <a name="using-a-template"></a>Korzystanie z szablonu

Aby rozpocząć korzystanie z szablonu generowania kodu, kliknij prawym przyciskiem myszy puste miejsce na powierzchni projektowej w projektancie EF i wybierz pozycję **Dodaj element generowania kodu...**.

![Dodaj element gen kodu](~/ef6/media/add-code-gen-item.png)

Jeśli masz już zainstalowany szablon, którego chcesz użyć (lub został on uwzględniony w programie Visual Studio), będzie on dostępny w sekcji **Kod** lub **Dane** z lewego menu.

![Zainstalowano](~/ef6/media/installed.png)

Jeśli nie masz jeszcze zainstalowanego szablonu, wybierz opcję **Online** z lewego menu i wyszukaj odpowiedni szablon.

![Wyszukiwanie](~/ef6/media/search.png) 

Jeśli używasz programu Visual Studio 2012, nowe pliki .tt będą zagnieżdżone w pliku .edmx.*

> [!NOTE]
> W przypadku modeli utworzonych w programie Visual Studio 2012 należy usunąć szablony używane do generowania kodu domyślnego, w przeciwnym razie będą generowane duplikaty klas i kontekstu. Domyślne pliki to ** &lt;nazwa&gt;modelu .tt** i ** &lt;&gt;nazwa modelu .context.tt**. 

![Szablony VS2012](~/ef6/media/vs2012-templates.png)

Jeśli używasz programu Visual Studio 2010, pliki tt są dodawane bezpośrednio do projektu.  

![Szablony VS2010](~/ef6/media/vs2010-templates.png)

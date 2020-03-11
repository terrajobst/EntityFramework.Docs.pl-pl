---
title: Szablony generowania kodu projektanta — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418677"
---
# <a name="designer-code-generation-templates"></a>Szablony generowania kodu projektanta
Podczas tworzenia modelu przy użyciu Entity Framework Designer klas i kontekst pochodny są automatycznie generowane. Oprócz domyślnej generacji kodu udostępniamy również kilka szablonów, których można użyć do dostosowania generowanego kodu. Te szablony są udostępniane w postaci szablonów tekstowych T4, co pozwala na dostosowanie szablonów w razie potrzeby.

Kod, który jest generowany domyślnie, zależy od wersji programu Visual Studio, w której tworzysz model:

-   Modele utworzone w programie Visual Studio 2012 & 2013 wygenerują proste klasy jednostek POCO oraz kontekst, który pochodzi z uproszczonego kontekstu DbContext.
-   Modele utworzone w programie Visual Studio 2010 generują klasy jednostek, które pochodzą z obiektu EntityObject i kontekstu, który pochodzi od obiektu ObjectContext.
  > [!NOTE]    
  > Zalecamy przełączenie do szablonu generatora DbContext po dodaniu modelu.

Ta strona obejmuje dostępne szablony, a następnie zawiera instrukcje dotyczące dodawania szablonu do modelu.

## <a name="available-templates"></a>Dostępne szablony

Zespół Entity Framework udostępnia następujące szablony:

### <a name="dbcontext-generator"></a>Generator DbContext

Ten szablon spowoduje wygenerowanie prostych klas jednostek POCO oraz kontekstu, który pochodzi z DbContext przy użyciu EF6.
Jest to zalecany szablon, chyba że masz powód, aby użyć jednego z innych szablonów wymienionych poniżej.
Jest to również szablon generowania kodu, który jest domyślnie używany, jeśli są używane najnowsze wersje programu Visual Studio (Visual Studio 2013 lub nowszy): podczas tworzenia nowego modelu ten szablon jest wykorzystywany domyślnie, a pliki T4 (. tt) są zagnieżdżone w pliku edmx.

#### <a name="older-versions-of-visual-studio"></a>Starsze wersje programu Visual Studio
- **Program Visual Studio 2012:** Aby uzyskać szablony **DbContextGenerator Dr 6. x** , należy zainstalować najnowsze **Entity Framework Tools dla programu Visual Studio** — Zobacz stronę [pobieranie Entity Framework](~/ef6/fundamentals/install.md) , aby uzyskać więcej informacji.
- **Program Visual Studio 2010:** Szablony **DbContextGenerator Dr 6. x** nie są dostępne dla programu Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Generator DbContext dla programu EF 5. x

Jeśli używasz starszej wersji pakietu NuGet EntityFramework (jeden z wersją główną 5), konieczne będzie użycie szablonu **generatora Ef 5. x DbContext** .

Jeśli używasz Visual Studio 2013 lub 2012, ten szablon jest już zainstalowany.

Jeśli używasz programu Visual Studio 2010, musisz wybrać kartę **online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie możesz zainstalować szablon bezpośrednio z galerii programu Visual Studio. Ponieważ szablony są zawarte w nowszych wersjach programu Visual Studio, wersje w galerii można zainstalować tylko w programie Visual Studio 2010.

- [Dr 5. x DbContext generator dlaC#](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [Dr 5. x DbContext generator dla C# witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [Dr 5. x DbContext generator dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [Dr 5. x DbContext generator dla witryn sieci Web VB.NET](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Generator DbContext dla EF 4. x

Jeśli używasz starszej wersji pakietu NuGet EntityFramework (jeden z wersją główną 4), należy użyć szablonu **generatora Ef 4. x DbContext** . Ten element można znaleźć na karcie **online** podczas dodawania szablonu lub można zainstalować szablon bezpośrednio z galerii programu Visual Studio.

- [Dr 4. x DbContext generator dlaC#](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [Generator programu EF 4. x DbContext C# dla witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [Generator EF 4. x DbContext dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [Generator programu EF 4. x DbContext dla witryn sieci Web VB.NET](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>Generator obiektu EntityObject

Ten szablon generuje klasy jednostek, które pochodzą z obiektu EntityObject i kontekstu, który pochodzi od obiektu ObjectContext.

> [!NOTE]
> Rozważ użycie generatora DbContext

W przypadku nowych aplikacji jest teraz zalecany szablon generatora DbContext. Generator kontekstu DbContext wykorzystuje prostszy interfejs API DbContext. Generator EntityObject nadal jest dostępny do obsługi istniejących aplikacji.

**Visual Studio 2010, 2012 &amp; 2013**

Musisz wybrać kartę **online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie możesz zainstalować szablon bezpośrednio z galerii programu Visual Studio.

- [Generator programu EF 6. x EntityObject dlaC#](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Generator programu EF 6. x EntityObject C# dla witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [Generator programu EF 6. x EntityObject dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [Generator programu EF 6. x EntityObject dla witryn sieci Web VB.NET](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Generator obiektów EntityObject dla programu EF 5. x**


Jeśli używasz programu Visual Studio 2012 lub 2013, musisz wybrać kartę **online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie możesz zainstalować szablon bezpośrednio z galerii programu Visual Studio. Ponieważ szablony są zawarte w programie Visual Studio 2010, wersje w galerii można zainstalować tylko w programie Visual Studio 2012 &amp; 2013.

- [Generator programu Dr 5. x EntityObject dlaC#](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Generator programu Dr 5. x EntityObject C# dla witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [Generator programu Dr 5. x EntityObject dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [Generator programu Dr 5. x EntityObject dla witryn sieci Web VB.NET](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Jeśli chcesz, aby generowanie kodu ObjectContext nie wymagało edytowania szablonu, można [powrócić do generowania kodu EntityObject](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Jeśli używasz programu Visual Studio 2010, ten szablon jest już zainstalowany. W przypadku utworzenia nowego modelu w programie Visual Studio 2010 ten szablon jest używany domyślnie, ale pliki TT nie są uwzględniane w projekcie. Jeśli chcesz dostosować szablon, musisz dodać go do projektu.

### <a name="self-tracking-entities-ste-generator"></a>Generator obiektów samośledzących (Wklej)

Ten szablon generuje klasy jednostek z autośledzeniem i kontekst, który pochodzi od obiektu ObjectContext. W aplikacji EF kontekst jest odpowiedzialny za śledzenie zmian w jednostkach. Jednak w scenariuszach N-warstwowych kontekst może nie być dostępny w warstwie, która modyfikuje jednostki. Samośledzące jednostki ułatwiają śledzenie zmian w dowolnej warstwie. Aby uzyskać więcej informacji, zobacz [samośledzące jednostki](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Nie zaleca się stosowania szablonu wklej

Nie zalecamy już używania szablonu Wklej w nowych aplikacjach. nadal będzie on dostępny do obsługi istniejących aplikacji. Zapoznaj się z [artykułem odłączonych jednostek](~/ef6/fundamentals/disconnected-entities/index.md) , aby uzyskać inne opcje, które zalecamy w przypadku scenariuszy N-warstwowych.

> [!NOTE]
> Nie istnieje wersja Dr 6. x szablonu Wklej.


> [!NOTE]
> Brak Visual Studio 2013 wersji szablonu Wklej.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Jeśli używasz programu Visual Studio 2012, musisz wybrać kartę **online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie możesz zainstalować szablon bezpośrednio z galerii programu Visual Studio. Ponieważ szablony są zawarte w programie Visual Studio 2010 wersje w galerii można zainstalować tylko w programie Visual Studio 2012.

- [Dr 5. x wklej generator dlaC#](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [Generator programu Dr 5. x wklej C# dla witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [Dr 5. x wklej generator dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [Generator programu Dr 5. x wklej dla witryn sieci Web VB.NET](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010 * *

Jeśli używasz programu Visual Studio 2010, ten szablon jest już zainstalowany.

### <a name="poco-entity-generator"></a>Generator jednostek POCO

Ten szablon generuje klasy jednostek POCO i kontekst, który pochodzi od obiektu ObjectContext

> [!NOTE]
> Rozważ użycie generatora DbContext

Generator kontekstu DbContext jest teraz zalecanym szablonem do generowania klas POCO w nowych aplikacjach. Generator kontekstu DbContext wykorzystuje nowy interfejs API DbContext i może generować prostsze klasy POCO. Generator jednostek POCO nadal jest dostępny do obsługi istniejących aplikacji.

> [!NOTE]
> Nie istnieje wersja EF 5. x lub dr 6. x szablonu Wklej.

> [!NOTE]
> Brak Visual Studio 2013 wersji szablonu POCO.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Musisz wybrać kartę **online** podczas dodawania szablonu, aby pobrać go z galerii programu Visual Studio. Alternatywnie możesz zainstalować szablon bezpośrednio z galerii programu Visual Studio.

- [Generator POCOa Dr 4. x dlaC#](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Generator POCOa Dr 4. x C# dla witryn sieci Web](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [Dr 4. x Generator POCO dla VB.NET](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [Generator POCOa Dr 4. x dla witryn sieci Web VB.NET](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Co to są szablony "witryny sieci Web"

Szablony "witryny sieci Web" (na przykład: **Dr 5. x DbContext generator dla witryn C\# Web**Sites) są używane w projektach witryn sieci Web utworzonych za pośrednictwem **&gt; nowej witryny sieci Web&gt; plik**.... Różnią się one od aplikacji sieci Web, utworzonych za pomocą **&gt; nowego&gt; projektu...** , które używają standardowych szablonów. Udostępniamy osobne szablony, ponieważ wymagają one systemu szablonów elementów w programie Visual Studio.

## <a name="using-a-template"></a>Korzystanie z szablonu

Aby rozpocząć korzystanie z szablonu generowania kodu, kliknij prawym przyciskiem myszy puste miejsce na powierzchni projektowej w projektancie EF i wybierz polecenie **Dodaj element generowania kodu..** ..

![Dodaj element gen Code](~/ef6/media/add-code-gen-item.png)

Jeśli wcześniej zainstalowano szablon, którego chcesz użyć (lub który został uwzględniony w programie Visual Studio), będzie on dostępny w sekcji **kod** lub **dane** z menu po lewej stronie.

![Zainstalowane](~/ef6/media/installed.png)

Jeśli szablon nie został jeszcze zainstalowany, wybierz pozycję **online** w menu po lewej stronie i wyszukaj odpowiedni szablon.

![Wyszukiwanie](~/ef6/media/search.png) 

W przypadku korzystania z programu Visual Studio 2012 nowe pliki. tt będą zagnieżdżone w pliku edmx. *

> [!NOTE]
> W przypadku modeli utworzonych w programie Visual Studio 2012 należy usunąć szablony używane do domyślnego generowania kodu. w przeciwnym razie będą istnieć zduplikowane klasy i kontekst. Domyślne pliki są **&lt;nazwy modelu&gt;. tt** i **&lt;nazwy modelu&gt;. Context.tt**. 

![Szablony VS2012](~/ef6/media/vs2012-templates.png)

W przypadku korzystania z programu Visual Studio 2010 pliki TT są dodawane bezpośrednio do projektu.  

![Szablony VS2010](~/ef6/media/vs2010-templates.png)

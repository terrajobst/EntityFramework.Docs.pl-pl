---
title: Relacje — projektancie platformy EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
caps.latest.revision: 3
ms.openlocfilehash: df752722dafbeff3042acdc95a58741f6e0f271d
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912674"
---
# <a name="relationships---ef-designer"></a>Relacje — projektancie platformy EF
> [!NOTE]
> Ta strona zawiera informacje o konfigurowaniu relacje w modelu przy użyciu projektanta EF. Aby uzyskać ogólne informacje dotyczące relacji w EF i uzyskiwania dostępu i manipulowanie danymi za pomocą relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).

Skojarzenia definiowania relacji między typami encji w modelu. W tym temacie pokazano, jak mapować skojarzenia z Entity Framework Designer (Projektant EF). Na poniższej ilustracji przedstawiono główne systemu windows, które są używane podczas pracy z projektancie platformy EF.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Podczas tworzenia modelu koncepcyjnego ostrzeżenia dotyczące podmiotów niezmapowanych i skojarzenia może pojawić się na liście błędów. Można zignorować te ostrzeżenia, ponieważ po dokonaniu wyboru wygenerować bazę danych z modelu, błędy znikną.

## <a name="associations-overview"></a>Omówienie skojarzenia

Podczas projektowania modelu przy użyciu projektanta EF plik edmx reprezentuje model. W pliku edmx **skojarzenia** element definiuje relację między dwoma typami encji. Skojarzenia należy określić typy jednostek, które są zaangażowane w relacji i możliwa liczba typów jednostek na każdym końcu relacji, który jest znany jako liczebności. Liczebność elementu end skojarzenia mogą mieć wartość równą jeden (1), zero lub jeden (od 0 do 1) lub wielu (\*). Informacja ta jest określona w dwóch podrzędny **zakończenia** elementów.

W czasie wykonywania wystąpienia typu jednostki na jednym końcu asocjacji jest możliwy za pośrednictwem właściwości nawigacji lub klucze obce (Jeśli użytkownik chce udostępnić klucze obce w jednostkach). Za pomocą kluczy obcych widoczne, relacji między jednostkami odbywa się za pomocą **ReferentialConstraint** elementu (element podrzędny **skojarzenia** elementu). Zalecane jest, należy zawsze udostępnić klucze obce w przypadku relacji w jednostkach.

> [!NOTE]
> W wielu do wielu (\*:\*) klucze obce nie można dodać do jednostki. W \*:\* relacji, informacji o skojarzeniu odbywa się za pomocą tworzenie niezależnych obiektów.

Aby uzyskać informacje o elementach CSDL (**ReferentialConstraint**, **skojarzenia**, itp.) zobacz [Specyfikacja CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Tworzenie i usuwanie skojarzenia

Tworzenie skojarzenia z aktualizacjami projektancie platformy EF modelu zawartości pliku edmx. Po utworzeniu skojarzenia, należy utworzyć mapowania dla skojarzenia (zostało to omówione w dalszej części tego tematu).

> [!NOTE]
> W tej sekcji założono, dodano już jednostkami, z którymi chcesz utworzyć skojarzenie między do modelu.

### <a name="to-create-an-association"></a>Aby utworzyć skojarzenie

1.  Kliknij prawym przyciskiem myszy pusty obszar powierzchni projektu, wskaż opcję **Dodaj nowe**i wybierz **skojarzenie...** .
2.  Wypełnij ustawienia dla skojarzenia w **Dodawanie skojarzenia** okna dialogowego.

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
> Użytkownik może nie dodać właściwości nawigacji lub właściwości klucza obcego z jednostkami: końcach asocjacji, czyszcząc ** właściwość nawigacji ** i ** dodać właściwości klucza obcego do &lt;Nazwa typu jednostki&gt; jednostki ** pola wyboru. Jeśli dodasz tylko jedną właściwość nawigacji, stowarzyszenia będą traversable tylko w jednym kierunku. Jeśli dodasz żadnych właściwości nawigacji, użytkownik musi dodać właściwości klucza obcego w celu uzyskania dostępu do jednostek na końcach asocjacji.
3.  Kliknij przycisk **OK**.

### <a name="to-delete-an-association"></a>Aby usunąć skojarzenie

Aby usunąć czy skojarzenie, jedną z następujących czynności:

-   Kliknij prawym przyciskiem myszy skojarzenia w Projektancie platformy EF powierzchni i wybierz **Usuń**.

- LUB —

-   Wybierz jeden lub więcej skojarzeń, a następnie naciśnij klawisz DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Zawierają właściwości klucza obcego w jednostkach (ograniczenia referencyjne)

Zalecane jest, należy zawsze udostępnić klucze obce w przypadku relacji w jednostkach. Platformy Entity Framework używa ograniczenia referencyjnego do identyfikowania, że właściwość działa jako klucz obcy relacji.

Gdy zaznaczono ***właściwości klucza obcego, aby dodać &lt;Nazwa typu jednostki&gt; jednostki*** pola wyboru przy tworzeniu relacji, to ograniczenie referencyjne została dodana dla Ciebie.

Gdy używasz projektancie platformy EF umożliwiają dodawanie lub edytowanie ograniczenia referencyjnego projektancie platformy EF dodaje lub modyfikuje **ReferentialConstraint** element CSDL zawartość pliku edmx.

-   Kliknij dwukrotnie skojarzenia, które chcesz edytować.
    **Ograniczenia referencyjnego** pojawi się okno dialogowe.
-   Z **jednostki** listy rozwijanej wybierz jednostkę główną w ograniczeniu referencyjnym.
    Właściwości klucza jednostki są dodawane do **klucz jednostki** listy w oknie dialogowym.
-   Z **zależne** listy rozwijanej wybierz jednostki zależne w ograniczeniu referencyjnym.
-   Dla każdego klucza podmiotu zabezpieczeń, kluczu zależnych, wybierz odpowiedni klucz zależne z listy rozwijanej w **zależne klucz** kolumny.

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   Kliknij przycisk **OK**.

## <a name="create-and-edit-association-mappings"></a>Tworzenie i edytowanie mapowania skojarzenia

Można określić sposób mapowania bazy danych w skojarzenie **szczegóły mapowania** okna projektancie platformy EF.

> [!NOTE]
> Można mapować tylko szczegółów dotyczących skojarzeń, które nie mają określone ograniczenia referencyjnego. Jeśli określono ograniczenia referencyjnego właściwości klucza obcego znajduje się w jednostce i szczegóły mapowania można użyć tej jednostki na kolumny klucza obcego mapuje do kontroli.

### <a name="create-an-association-mapping"></a>Utwórz mapowanie skojarzenia

-   Kliknij prawym przyciskiem myszy skojarzenie w projekt powierzchni i wybierz **mapowania tabeli,**.
    Spowoduje to wyświetlenie mapowanie skojarzenia w **szczegóły mapowania** okna.
-   Kliknij przycisk **Dodaj tabelę lub widok**.
    Listy rozwijanej pojawia się, który zawiera wszystkie tabele w modelu magazynu.
-   Wybierz tabelę, do której będzie zmapowana skojarzenia.
    **Szczegóły mapowania** okno wyświetla obu końcach asocjacji i właściwości klucza dla typu jednostki, w każdej **zakończenia**.
-   Dla każdej właściwości klucza, kliknij przycisk **kolumny** pola, a następnie wybierz kolumnę, do którego będzie zmapowana właściwości.

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Edytuj mapowanie skojarzenia

-   Kliknij prawym przyciskiem myszy skojarzenie w projekt powierzchni i wybierz **mapowania tabeli,**.
    Spowoduje to wyświetlenie mapowanie skojarzenia w **szczegóły mapowania** okna.
-   Kliknij przycisk **mapuje &lt;nazwy tabeli&gt;**.
    Listy rozwijanej pojawia się, który zawiera wszystkie tabele w modelu magazynu.
-   Wybierz tabelę, do której będzie zmapowana skojarzenia.
    **Szczegóły mapowania** okno wyświetla obu końcach asocjacji i właściwości klucza dla typu jednostki, na każdym końcu.
-   Dla każdej właściwości klucza, kliknij przycisk **kolumny** pola, a następnie wybierz kolumnę, do którego będzie zmapowana właściwości.

## <a name="edit-and-delete-navigation-properties"></a>Edytuj i Usuń właściwości nawigacji

Właściwości nawigacji są właściwościami skrótów, które są używane do lokalizowania jednostek na końcach asocjacji w modelu. Podczas tworzenia skojarzenia między dwoma typami jednostki można utworzyć właściwości nawigacji.

#### <a name="to-edit-navigation-properties"></a>Aby edytować właściwości nawigacji

-   Wybierz właściwość nawigacji na powierzchni projektanta EF.
    W programie Visual Studio wyświetlane są informacje dotyczące właściwości nawigacji **właściwości** okna.
-   Zmień ustawienia właściwości **właściwości** okna.

#### <a name="to-delete-navigation-properties"></a>Można usunąć właściwości nawigacji

-   Jeśli klucze obce nie są widoczne na typy jednostek w modelu koncepcyjnym, usunięcie właściwości nawigacji może spowodować odpowiedniego skojarzenia traversable tylko w jednym kierunku lub nie traversable wcale.
-   Kliknij prawym przyciskiem myszy właściwość nawigacji w Projektancie platformy EF powierzchni i wybierz **Usuń**.

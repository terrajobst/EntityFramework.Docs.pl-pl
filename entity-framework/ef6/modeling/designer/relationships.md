---
title: Relacje-Dr Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418246"
---
# <a name="relationships---ef-designer"></a>Relacje — Projektant EF
> [!NOTE]
> Ta strona zawiera informacje o konfigurowaniu relacji w modelu przy użyciu narzędzia Dr Designer. Aby uzyskać ogólne informacje na temat relacji w EF oraz jak uzyskać dostęp do danych i manipulować nimi przy użyciu relacji, zobacz [relacje & właściwości nawigacji](~/ef6/fundamentals/relationships.md).

Skojarzenia definiują relacje między typami jednostek w modelu. W tym temacie przedstawiono sposób mapowania skojarzeń z Entity Framework Designer (program EF Designer). Na poniższej ilustracji przedstawiono główne okna, które są używane podczas pracy z programem Dr Designer.

![Projektant EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> Podczas budowania modelu koncepcyjnego w Lista błędów mogą pojawić się ostrzeżenia dotyczące niezamapowanych jednostek i skojarzeń. Te ostrzeżenia można zignorować, ponieważ po wybraniu opcji wygenerowania bazy danych z modelu te błędy zostaną odrzucone.

## <a name="associations-overview"></a>Omówienie skojarzeń

Podczas projektowania modelu przy użyciu narzędzia Dr Designer plik. edmx reprezentuje model. W pliku. edmx element **Association** definiuje relację między dwoma typami jednostek. Skojarzenie musi określać typy jednostek, które są uwzględnione w relacji, oraz liczbę typów jednostek na każdym końcu relacji, która jest znana jako liczebność. Liczebność elementu end skojarzenia może mieć wartość jeden (1), zero lub jeden (0.. 1) lub wiele (\*). Te informacje są określone w dwóch podrzędnych elementach **End** .

W czasie wykonywania wystąpienia typu jednostki na jednym końcu skojarzenia są dostępne za poorednictwem właściwości nawigacji lub kluczy obcych (Jeśli zdecydujesz się uwidocznić klucze obce w jednostkach). Gdy klucze obce są uwidocznione, relacje między jednostkami są zarządzane za pomocą elementu **ReferentialConstraint** (element podrzędny elementu **Association** ). Zaleca się, aby zawsze ujawniać klucze obce dla relacji w jednostkach.

> [!NOTE]
> W przypadku wielu do wielu (\*:\*) nie można dodać kluczy obcych do jednostek. W \*:\*, informacje o skojarzeniu są zarządzane za pomocą niezależnego obiektu.

Aby uzyskać informacje na temat elementów CSDL (**ReferentialConstraint**, **Association**itp.), zobacz [specyfikację CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Tworzenie i usuwanie skojarzeń

Tworzenie skojarzenia przy użyciu narzędzia Dr Designer aktualizuje zawartość modelu pliku. edmx. Po utworzeniu skojarzenia należy utworzyć mapowania dla skojarzenia (omówione w dalszej części tego tematu).

> [!NOTE]
> W tej sekcji założono, że dodano już jednostki, dla których chcesz utworzyć skojarzenie między modelem.

### <a name="to-create-an-association"></a>Aby utworzyć skojarzenie

1.  Kliknij prawym przyciskiem myszy pusty obszar na powierzchni projektowej, wskaż polecenie **Dodaj nowy**i wybierz pozycję **kojarzenie...** .
2.  Wypełnij ustawienia skojarzenia w oknie dialogowym **Dodawanie skojarzenia** .

    ![Dodaj skojarzenie](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Można wybrać, aby nie dodawać właściwości nawigacji lub właściwości klucza obcego do jednostek na końcu skojarzenia przez wyczyszczenie **właściwości nawigacji **i **Dodawanie właściwości klucza obcego do &lt;nazwy typu jednostki&gt; **pola wyboru jednostki. Jeśli dodasz tylko jedną właściwość nawigacji, skojarzenie będzie przepływać tylko w jednym kierunku. Jeśli dodasz brak właściwości nawigacji, musisz dodać właściwości klucza obcego w celu uzyskania dostępu do jednostek na końcu skojarzenia.
    
3.  Kliknij przycisk **OK**.

### <a name="to-delete-an-association"></a>Aby usunąć skojarzenie

Aby usunąć skojarzenie, wykonaj jedną z następujących czynności:

-   Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektanta EF i wybierz polecenie **Usuń**.

- Oraz

-   Wybierz co najmniej jedno skojarzenie i naciśnij klawisz DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Uwzględnij właściwości klucza obcego w jednostkach (ograniczenia referencyjne)

Zaleca się, aby zawsze ujawniać klucze obce dla relacji w jednostkach. Entity Framework używa ograniczenia referencyjnego, aby określić, że Właściwość działa jako klucz obcy dla relacji.

Jeśli zaznaczono pole wyboru ***Dodaj właściwości klucza obcego do &lt;nazwy typu jednostki&gt; jednostki*** podczas tworzenia relacji, to ograniczenie referencyjne zostało dodane dla Ciebie.

W przypadku dodawania lub edytowania ograniczenia referencyjnego za pomocą programu EF Projektant EF dodaje lub modyfikuje element **ReferentialConstraint** w zawartości CSDL pliku. edmx.

-   Kliknij dwukrotnie skojarzenie, które chcesz edytować.
    Zostanie wyświetlone okno dialogowe **ograniczenie referencyjne** .
-   Z listy rozwijanej  **głównej** wybierz jednostkę główną w ograniczeniu referencyjnym.
    Właściwości klucza jednostki są dodawane do listy **klucz podmiotu zabezpieczeń** w oknie dialogowym.
-   Z listy rozwijanej  **zależne** wybierz jednostkę zależną w ograniczeniu referencyjnym.
-   Dla każdego klucza podmiotu zabezpieczeń, który ma klucz zależny, wybierz odpowiedni klucz zależny z listy rozwijanej w kolumnie **klucz zależny** .

    ![Ref — ograniczenie](~/ef6/media/refconstraint.png)

-   Kliknij przycisk **OK**.

## <a name="create-and-edit-association-mappings"></a>Tworzenie i edytowanie mapowań skojarzeń

Można określić sposób mapowania skojarzenia do bazy danych w oknie **szczegóły mapowania** w programie Dr Designer.

> [!NOTE]
> Można mapować tylko szczegóły dla skojarzeń, które nie mają określonego ograniczenia referencyjnego. Jeśli określono ograniczenie referencyjne, właściwość klucza obcego jest uwzględniona w jednostce i można użyć szczegółów mapowania dla jednostki, aby określić kolumnę, do której jest mapowany klucz obcy.

### <a name="create-an-association-mapping"></a>Tworzenie mapowania skojarzenia

-   Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektowej i wybierz pozycję **Mapowanie tabeli**.
    Spowoduje to wyświetlenie mapowania skojarzenia w oknie **szczegóły mapowania** .
-   Kliknij pozycję **Dodaj tabelę lub widok**.
    Zostanie wyświetlona lista rozwijana zawierająca wszystkie tabele w modelu magazynu.
-   Wybierz tabelę, do której zostanie zmapowane skojarzenie.
    W oknie **szczegóły mapowania** są wyświetlane końce skojarzenia i właściwości klucza dla typu jednostki na każdym **końcu**.
-   Dla każdej właściwości klucza kliknij pole  **kolumny** , a następnie wybierz kolumnę, do której zostanie zamapowana właściwość.

    ![Szczegóły mapowania 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Edytowanie mapowania skojarzenia

-   Kliknij prawym przyciskiem myszy skojarzenie na powierzchni projektowej i wybierz pozycję **Mapowanie tabeli**.
    Spowoduje to wyświetlenie mapowania skojarzenia w oknie **szczegóły mapowania** .
-   Kliknij pozycję **mapy, aby &lt;nazwę tabeli&gt;** .
    Zostanie wyświetlona lista rozwijana zawierająca wszystkie tabele w modelu magazynu.
-   Wybierz tabelę, do której zostanie zmapowane skojarzenie.
    W oknie **szczegóły mapowania** są wyświetlane końce skojarzenia i właściwości klucza dla typu jednostki na każdym końcu.
-   Dla każdej właściwości klucza kliknij pole  **kolumny** , a następnie wybierz kolumnę, do której zostanie zamapowana właściwość.

## <a name="edit-and-delete-navigation-properties"></a>Edytuj i Usuń właściwości nawigacji

Właściwości nawigacji są właściwościami skrótu, które są używane do lokalizowania jednostek na końcu skojarzenia w modelu. Właściwości nawigacji można utworzyć podczas tworzenia skojarzenia między dwoma typami jednostek.

#### <a name="to-edit-navigation-properties"></a>Aby edytować właściwości nawigacji

-   Wybierz właściwość nawigacji na powierzchni projektanta EF.
    Informacje o właściwości nawigacji są wyświetlane w oknie **Właściwości** programu Visual Studio .
-   Zmień ustawienia właściwości w oknie **właściwości** .

#### <a name="to-delete-navigation-properties"></a>Aby usunąć właściwości nawigacji

-   Jeśli klucze obce nie są ujawnione w typach jednostek w modelu koncepcyjnym, usunięcie właściwości nawigacji może spowodować, że odpowiednie skojarzenie zostanie przesunięte tylko w jednym kierunku lub w ogóle nie przechodzą.
-   Kliknij prawym przyciskiem myszy właściwość nawigacji na powierzchni projektanta EF i wybierz polecenie **Usuń**.

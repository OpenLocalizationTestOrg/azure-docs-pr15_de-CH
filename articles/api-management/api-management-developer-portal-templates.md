<properties 
    pageTitle="Azure API Management-Entwicklerportal mithilfe von Vorlagen anpassen | Microsoft Azure" 
    description="Informationen Sie zum Entwicklerportal Azure API Management mithilfe von Vorlagen anpassen." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Azure API Management-Entwicklerportal mithilfe von Vorlagen anpassen

Azure API Management bietet mehrere Funktionen ermöglichen Administratoren [das Erscheinungsbild Entwicklerportal](api-management-customize-portal.md)anpassen sowie das Anpassen des Inhalts der Entwickler Portalseiten mithilfe einer Reihe von Vorlagen, die den Inhalt der Seiten selbst konfigurieren. [DotLiquid](http://dotliquidmarkup.org/) Syntax und bereitgestellten Satz von lokalisierten Zeichenfolgenressourcen, Symbole und Steuerelemente verwenden, haben Sie die Flexibilität den Inhalt der Seiten konfigurieren, Bedarf mithilfe von Vorlagen.

## <a name="developer-portal-templates-overview"></a>Entwicklerübersicht Vorlagen

Entwickler der Vorlagen im Entwicklerportal Administratoren der Dienstinstanz API Management verwaltet. Um Entwickler Vorlagen verwalten, Ihre API Management-Dienstinstanz in der Azure-Verwaltungsportal navigieren Sie, und klicken Sie auf **Durchsuchen**.

![Entwicklerportal][api-management-browse]

Sind Sie bereits im Publisher-Portal, können Sie durch Klicken auf **Entwicklerportal**Entwicklerportal zugreifen.

![Developer Portal Menü][api-management-developer-portal-menu]

Zugriff auf die Entwickler Vorlagen klicken Sie links auf die Anpassung Menü anpassen und auf **Vorlagen**.

![Entwickler der Vorlagen][api-management-customize-menu]

Liste der Vorlagen zeigt verschiedene Kategorien von Vorlagen für die verschiedenen Seiten im Entwicklerportal. Jede Vorlage ist anders, aber die Schritte zum Bearbeiten und veröffentlichen Sie die Änderung entsprechen. Um eine Vorlage zu bearbeiten, klicken Sie auf den Namen der Vorlage.

![Entwickler der Vorlagen][api-management-templates-menu]

Auf einer Vorlage wird auf der Portalseite Entwickler, die diese Vorlage angepasst. Vorlage wird in diesem Beispiel wird die **Produktliste** angezeigt. Die **Produktliste** Vorlage steuert Bildschirmbereich im roten Rechteck angezeigt. 

![Vorlage für Produkte][api-management-developer-portal-templates-overview]

Einige Vorlagen, wie Vorlagen **Benutzerprofil** anpassen verschiedene Teile derselben Seite. 

![Profil von Benutzervorlagen][api-management-user-profile-templates]

Editor für jede Vorlage Developer Portal hat zwei Abschnitte am unteren Rand der Seite angezeigt. Links zeigt den Bearbeitungsbereich der Vorlage und die rechte Seite zeigt das Datenmodell für die Vorlage. 

Vorlage bearbeiten im Bereich enthält das Markup, das das Aussehen und Verhalten der entsprechenden Seite im Entwicklerportal steuert. Das Markup in der Vorlage verwendet die Syntax [DotLiquid](http://dotliquidmarkup.org/) . Eine beliebte Editor für DotLiquid ist [DotLiquid für Designer](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Änderungen zur Vorlage bei der Bearbeitung werden in Echtzeit im Browser, jedoch sind nicht sichtbar für Ihre Kunden Sie [Speichern](#to-save-a-template) und [Veröffentlichen](#to-publish-a-template) Sie die Vorlage.

![Vorlagen-markup][api-management-template]

**Vorlage** Datenfenster enthält eine Anleitung zum Datenmodell für die Entitäten in einer bestimmten Vorlage zur Verfügung. Es enthält dieses Handbuch mit live-Daten, die derzeit im Entwicklerportal angezeigt werden. Sie können auf das Rechteck in der oberen rechten Ecke des Bereichs **Vorlagendaten** Vorlage Bereiche erweitern.

![Vorlage-Datenmodell][api-management-template-data]

Im vorherigen Beispiel sind zwei Produkte im Entwicklerportal angezeigt, die im **Vorlagendaten** angezeigten Daten abgerufen wurden, wie im folgenden Beispiel gezeigt.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Das Markup in der **Liste** Vorlage verarbeitet die Daten zur gewünschte Ausgabe durch Durchlaufen der Auflistung Produkte Informationen und einen Link zu jedem einzelnen Produkt. Hinweis Die `<search-control>` und `<page-control>` Elemente im Markup. Diese steuern die Anzeige der Suche und Steuerelemente auf der Seite paging. `ProductsStrings|PageTitleProducts`ist eine lokalisierte Zeichenfolge, die enthält die `h2` Headertext für die Seite. Eine Liste der Zeichenfolge Ressourcen, Steuerelemente und Symbolen in Developer Portal Vorlagen, siehe [API Management Entwicklerreferenz Vorlagen](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Vorlage speichern

Um eine Vorlage zu speichern, klicken Sie im Vorlageneditor speichern.

![Vorlage speichern][api-management-save-template]

Gespeicherte Änderungen werden im Entwicklerportal erst veröffentlicht werden.

## <a name="to-publish-a-template"></a>Veröffentlichen eine Vorlage

Gespeicherte Vorlagen können entweder einzeln oder zusammen veröffentlicht werden. Eine einzelne Vorlage auf Veröffentlichen im Vorlageneditor.

![Publikationsvorlage][api-management-publish-template]

Klicken Sie auf **Ja,** um zu bestätigen, und stellen Sie die Vorlage auf die Entwicklerportal.

![Bestätigen Sie veröffentlichen][api-management-publish-template-confirm]

Um alle derzeit nicht veröffentlichte Vorlage veröffentlichen, klicken Sie in der Vorlagenliste **Veröffentlichen** . Nicht veröffentlichte Vorlagen werden durch ein Sternchen nach dem Vorlagennamen angegeben. In diesem Beispiel werden die **Produktliste** und **Produkt** -Vorlagen veröffentlicht.

![Veröffentlichen von Vorlagen][api-management-publish-templates]

Klicken Sie auf **Veröffentlichen Anpassungen** zu bestätigen.

![Bestätigen Sie veröffentlichen][api-management-publish-customizations]

Neu veröffentlichte Vorlagen sind im Entwicklerportal sofort wirksam.

## <a name="to-revert-a-template-to-the-previous-version"></a>Um eine Vorlage auf die vorherige version

Um eine Vorlage auf die vorherige veröffentlichten Version, klicken Sie auf zurück im Vorlageneditor.

![Vorlage zurücksetzen][api-management-revert-template]

Klicken Sie auf **Ja** zu bestätigen.

![Bestätigen][api-management-revert-template-confirm]

Zuvor veröffentlichte Version der Vorlage wird im Entwicklerportal, nachdem der Wiederherstellungsvorgang abgeschlossen ist.

## <a name="to-restore-a-template-to-the-default-version"></a>Eine Vorlage zur Standardversion wiederherstellen

Wiederherstellen von Vorlagen auf die Standardversion ist ein zweistufiger Prozess. Zuerst müssen die Vorlagen wiederhergestellt werden und müssen die wiederhergestellten Versionen veröffentlicht werden.

Klicken Sie zum Wiederherstellen eine Vorlage zur Standardversion auf Wiederherstellen im Vorlageneditor.

![Vorlage zurücksetzen][api-management-reset-template]

Klicken Sie auf **Ja** zu bestätigen.

![Bestätigen][api-management-reset-template-confirm]

Um alle Vorlagen ihre Standardversionen wiederherzustellen, klicken Sie auf die Vorlage **Standardvorlagen wiederherstellen** .

![Wiederherstellen von Vorlagen][api-management-restore-templates]

Die wiederhergestellten Vorlagen müssen einzeln oder alle gleichzeitig publiziert werden durch die Schritte [zum Veröffentlichen einer Vorlage](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Vorlagen-Entwicklerreferenz

Referenzinformationen für Entwickler Vorlagen, Zeichenfolgenressourcen, Symbole und Steuerelemente finden Sie unter [API Management Entwicklerreferenz Vorlagen](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Einen video ansehen

Im folgenden Video wie API und Operation Seiten in Vorlagen Entwicklerportal Diskussionsrunden und Filter hinzufügen.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png








<properties 
    pageTitle="Verwendung von Eigenschaften in Azure API Management-Richtlinien" 
    description="Erfahren Sie, wie Eigenschaften in Azure API Management-Richtlinien verwendet." 
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


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Verwendung von Eigenschaften in Azure API Management-Richtlinien

API-Richtlinien sind eine leistungsfähige Funktion des Systems, die der Herausgeber das Verhalten der API durch Konfiguration ändern. Richtlinien sind eine Sammlung von Anweisungen, die auf die Anforderung sequenziell ausgeführt oder eine API auf. Richtlinien können mit Literaltext Werte, Richtlinienausdrücke und Eigenschaften erstellt werden. 

Jede API Management Service-Instanz hat eine Properties-Auflistung der Schlüssel-Wert-Paare für die Dienstinstanz. Diese Eigenschaften können verwendet werden, Konstante Werte für alle API-Konfiguration und Richtlinien verwalten. Jede Eigenschaft verfügt über die folgenden Attribute.


| Attribut | Typ            | Beschreibung                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Name      | Zeichenfolge          | Der Name der Eigenschaft. Es kann nur Buchstaben, Ziffern, Punkt, Bindestrich enthalten und Unterstriche. |
| Wert     | Zeichenfolge          | Der Wert der Eigenschaft. Es kann nicht leer sein oder nur aus Leerzeichen bestehen.                           |
| Geheimnis    | boolescher Wert         | Bestimmt, ob der Wert einen geheimen Schlüssel sollten oder nicht verschlüsselt.                                |
| Tags      | Array von Zeichenfolgen | Optional tags, wenn die Eigenschaft Filtern verwendet werden.                               |

Eigenschaften werden im Publisher-Portal auf der Registerkarte **Eigenschaften** konfiguriert. Im folgenden Beispiel werden drei Eigenschaften konfiguriert.

![Eigenschaften][api-management-properties]

Eigenschaftswerte können Literalzeichenfolgen und [Policy-Ausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx)enthalten. Die folgende Tabelle zeigt drei vorherigen Beispieleigenschaften und ihre Attribute. Der Wert der `ExpressionProperty` ist ein Richtlinienausdruck eine Zeichenfolge, die das aktuelle Datum und die Uhrzeit zurück. Die Eigenschaft `ContosoHeaderValue` als ein Schlüssel markiert, sodass der Wert nicht angezeigt wird.

| Name               | Wert                      | Geheimnis | Tags    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | False  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | True   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | False  |         |

## <a name="to-use-a-property"></a>Eine Eigenschaft

Um eine Eigenschaft in einer Richtlinie zu verwenden, platzieren den Eigenschaftennamen in doppelte Paar Klammern wie `{{ContosoHeader}}`, wie im folgenden Beispiel gezeigt.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

In diesem Beispiel `ContosoHeader` dient als Name eines Headers in eine `set-header` Politik und `ContosoHeaderValue` als Wert dieses Headers verwendet. Wenn diese Richtlinie während einer Anforderung oder Antwort an das Gateway API Management ausgewertet wird `{{ContosoHeader}}` und `{{ContosoHeaderValue}}` durch ihre jeweiligen Eigenschaftswerte ersetzt werden.

Eigenschaften können als vollständige Attribut- oder Elementwerte wie im vorherigen Beispiel verwendet, aber sie werden eingefügt oder kombiniert mit einer literalen Ausdruck wie im folgenden Beispiel gezeigt:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Eigenschaften können auch Richtlinienausdrücke enthalten. Im folgenden Beispiel wird die `ExpressionProperty` verwendet wird.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Wenn diese Richtlinie ausgewertet wird, `{{ExpressionProperty}}` durch ihren Wert ersetzt: `@(DateTime.Now.ToString())`. Da ein Richtlinienausdruck Wert Ausdruck ausgewertet und die Richtlinie mit der Ausführung wird fortgesetzt.

Dies testen im Entwicklerportal durch Aufrufen einer Operation, die eine Richtlinie mit Eigenschaften im Bereich. Im folgenden Beispiel wird ein Vorgang mit zwei Beispiel aufgerufen `set-header` mit Eigenschaften. Beachten Sie, dass die Antwort enthält zwei benutzerdefinierte Header, die Eigenschaften mit Richtlinien konfiguriert wurden.

![Entwicklerportal][api-management-send-results]

Wenn Sie [API-Inspektor Trace](api-management-howto-api-inspector.md) für einen, die zwei vorherigen Beispielrichtlinien mit Eigenschaften enthält betrachten, sehen Sie die beiden `set-header` mit den Eigenschaftswerten sowie die Auswertung von Ausdrücken Richtlinie für die Eigenschaft, die die Richtlinie Ausdruck eingefügt.

![API-Inspektor trace][api-management-api-inspector-trace]

Beachten Sie, dass Eigenschaftenwerte Richtlinienausdrücke enthalten Werte anderer Eigenschaften enthalten können. Wenn Text enthält eine Referenz für einen Eigenschaftswert, z. B. verwendet wird `Property value text {{MyProperty}}`, diese Referenz nicht ersetzt und werden als Teil des Eigenschaftenwerts.

## <a name="to-create-a-property"></a>Zum Erstellen einer Eigenschaft

Um eine Eigenschaft zu erstellen, klicken Sie auf der Registerkarte **Eigenschaften** auf **Eigenschaft hinzufügen** .

![Eigenschaft hinzufügen][api-management-properties-add-property-menu]

**Name** und **Wert** Werte erforderlich. Wenn dieser Eigenschaft ein Schlüssel ist, aktivieren Sie das Kontrollkästchen **Dies ist ein geheimer Schlüssel** . Geben Sie eine oder mehrere optionale Tags unterstützen Sie beim Organisieren Ihrer Eigenschaften, und klicken Sie auf **Speichern**.

![Eigenschaft hinzufügen][api-management-properties-add-property]

Beim Speichern einer neuen Eigenschaft **Eigenschaft suchen** -Textfeld mit dem Namen der neuen Eigenschaft gefüllt, und die neue Eigenschaft wird angezeigt. Um alle Eigenschaften anzuzeigen, deaktivieren Textbox **Eigenschaft suchen** und drücken Sie die EINGABETASTE.

![Eigenschaften][api-management-properties-property-saved]

Informationen zum Erstellen einer Eigenschaft mithilfe der REST-API finden Sie unter [Erstellen einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Eine Eigenschaft bearbeiten

Um eine Eigenschaft zu bearbeiten, klicken Sie auf **Bearbeiten** neben der Eigenschaft bearbeiten.

![Eigenschaft bearbeiten][api-management-properties-edit]

Änderungen Sie gewünschte, und klicken Sie auf **Speichern**. Wenn Sie den Eigenschaftennamen ändern, werden alle Richtlinien, die auf die Eigenschaft verweisen automatisch zum Verwenden des neuen Namens.

![Eigenschaft bearbeiten][api-management-properties-edit-property]

Informationen zum Bearbeiten einer Eigenschaft mithilfe der REST-API finden Sie unter [Bearbeiten einer Eigenschaft die REST-API verwenden](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Zum Löschen einer Eigenschaft

Um eine Eigenschaft zu löschen, klicken Sie neben der Eigenschaft löschen **Löschen** .

![Eigenschaft löschen][api-management-properties-delete]

Klicken Sie auf **Ja, löschen** bestätigen.

![Löschen bestätigen][api-management-delete-confirm]

>[AZURE.IMPORTANT] Richtlinien die Eigenschaft verwiesen, können Sie erfolgreich erst gelöscht werden, die Eigenschaft über alle Richtlinien zu entfernen, die sie verwenden.

Informationen zum Löschen einer Eigenschaft mithilfe der REST-API finden Sie unter [Löschen einer Eigenschaft mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Suchen und Filtern Eigenschaften

Die Registerkarte **Eigenschaften** umfasst Such- und Filterfunktionen, um Ihre Eigenschaften verwalten. Um die Eigenschaftenliste Eigenschaftenname filtern, geben Sie einen Suchbegriff in das Textfeld der **Eigenschaft suchen** . Um alle Eigenschaften anzuzeigen, deaktivieren Textbox **Eigenschaft suchen** und drücken Sie die EINGABETASTE.

![Suche][api-management-properties-search]

Liste der Tag Werte filtern, geben Sie ein oder mehrere Tags in das Textfeld **nach Tags Filtern** . Zum Anzeigen aller Eigenschaften eingeben deaktivieren **Filtern Sie nach Tags** Textbox und drücken

![Filter][api-management-properties-filter]

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zum Arbeiten mit Richtlinien
    -   [Richtlinien-API-Management](api-management-howto-policies.md)
    -   [Richtlinie Bezug](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Einen video ansehen

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png


<properties 
    pageTitle="Richtlinien in Azure API Management | Microsoft Azure" 
    description="Informationen Sie zum Erstellen, bearbeiten und Konfigurieren von Richtlinien in API Management." 
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


#<a name="policies-in-azure-api-management"></a>Richtlinien in Azure API Management

Richtlinien sind in Azure API Management eine leistungsstarke Fähigkeit des Systems, die der Herausgeber das Verhalten der API durch Konfiguration ändern. Richtlinien sind eine Sammlung von Anweisungen, die auf die Anforderung sequenziell ausgeführt oder eine API auf. Beliebte Anweisungen enthalten Konvertierung von XML, JSON und rufen Rate Begrenzung verwenden, um eingehende Anrufe von einem Entwickler beschränken. Viele weitere Richtlinien sind sofort verfügbar.

[Policy Reference][] vollständige Richtlinien und deren Einstellungen anzeigen

Richtlinien gelten in dem Gateway zwischen Consumer API und die verwaltete API. Das Gateway empfängt alle Anfragen und normalerweise weiterleitet, die zugrunde liegende API unverändert. Jedoch eine Richtlinie für die eingehende Anforderung und die ausgehende Antwort verwenden kann.

Richtlinienausdrücke können als Attribute oder Textwerte in API-Verwaltungsrichtlinien verwendet werden, die Richtlinie nicht anders angegeben. Einige Richtlinien wie die [Kontrollfluss][] und [Variable][] basieren auf Richtlinienausdrücken. Weitere Informationen finden Sie unter [Erweiterte Richtlinien][] und [Gruppenrichtlinien Ausdrücke][].

## <a name="scopes"> </a>Richtlinien konfigurieren
Richtlinien können global oder im Bereich eines [Produkts][], [API][] oder [Vorgang][]konfiguriert werden. Navigieren Sie zum Konfigurieren einer Richtlinie zum Richtlinien-Editor im Publisher-Portal.

![Menü Richtlinien][policies-menu]

Der Richtlinien-Editor besteht aus drei Hauptabschnitte: Richtlinie Rahmen (oben), Policy-Definition, Richtlinien bearbeitet werden (links) und die Anweisungen Liste (rechts):

![Richtlinien-editor][policies-editor]

Zum Konfigurieren einer Richtlinie müssen Sie zunächst den Bereich auswählen, auf dem die Richtlinie gelten soll. Im Screenshot unten **Starter** ist Produkt ausgewählt. Beachten Sie, dass das quadratische Symbol neben dem Richtliniennamen auf dieser Ebene bereits eine Richtlinie angewendet wird.

![Gültigkeitsbereich][policies-scope]

Da bereits eine Richtlinie angewendet wurde, wird die Konfiguration in der Definition angezeigt.

![Konfigurieren][policies-configure]

Die Richtlinie wird schreibgeschützt zuerst. Bearbeitung der Definition auf die Aktion **Konfigurieren** .

![Bearbeiten][policies-edit]

Policy-Definition ist ein einfaches XML-Dokument, das eine Sequenz von eingehenden und ausgehenden beschreibt. Die XML-Daten kann direkt im Definitionsfenster bearbeitet werden. Rechts wird eine Liste von Anweisungen bereitgestellt und Anweisungen für den aktuellen Bereich aktiviert und hervorgehoben. wie **Grenze aufrufen Rate** -Anweisung in der Abbildung oben.

Auf aktivierte Anweisung wird die entsprechende XML an der Position des Cursors in der Definition hinzufügen. 

>[AZURE.NOTE] Wenn die gewünschte Richtlinie nicht aktiviert ist, sicherstellen Sie, dass Sie im richtigen Bereich für diese Richtlinie. Jede Erklärung dient zur Verwendung in bestimmten Bereichen und Richtlinien. Überprüfen Sie die Richtlinie Abschnitte und Bereiche für eine Abschnitt **Verwendung** für diese Richtlinie [Policy Reference][].

Eine vollständige Liste der Richtlinien und ihre Einstellungen stehen [Policy Reference][].

Beispielsweise fügen Sie eine neue Anweisung um eingehende Anfragen auf bestimmte IP-Adressen beschränken, platzieren Sie den Cursor innerhalb des Inhalts von der `inbound` -XML-Element und **schränken Aufrufer IPS-** Anweisung auf.

![Richtlinien][policies-restrict]

Dies fügt einen XML-Ausschnitt an der `inbound` Element, das zum Konfigurieren der Anweisung Hinweise.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Eingehende Anfragen und akzeptieren ändern nur die IP-Adresse 1.2.3.4 XML wie folgt:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Speichern][policies-save]

Abschluss Anweisungen für die Richtlinie konfigurieren auf **Speichern** , und die Änderung sofort an API Management Gateway weitergegeben.

##<a name="sections"> </a>Verständnis Konfiguration

Eine Richtlinie ist eine Reihe von Anweisungen, die eine Anforderung und Antwort ausführen. Die Konfiguration aufgeteilt entsprechend `inbound`, `backend`, `outbound`, und `on-error` Abschnitte wie in der folgenden Konfiguration dargestellt.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Liegt ein Fehler bei der Verarbeitung einer Anforderung, alle verbleibenden Schritte der `inbound`, `backend`, oder `outbound` Abschnitte übersprungen und springt die Aussagen in der `on-error` Abschnitt. Mit Policy-Anweisungen in der `on-error` Abschnitt überprüfen Sie die Fehlermeldung mit der `context.LastError` -Eigenschaft prüfen und Anpassen der Fehler Antwort mit der `set-body` Richtlinie, und was passiert, wenn ein Fehler auftritt. Es gibt integrierte Maßnahmen und Fehler während der Verarbeitung der Richtlinien-Fehlercodes. Weitere Informationen finden Sie unter [Fehlerbehandlung in API-Richtlinien](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Da Richtlinien auf verschiedenen Ebenen (global Product-api und Operation) angegeben werden ermöglicht die Konfiguration für die Reihenfolge angeben, in der die Richtliniendefinition Aussagen in Bezug auf die übergeordnete Richtlinie ausgeführt. 

Richtlinienbereiche werden in der folgenden Reihenfolge ausgewertet.

1. Globale
2. Produktumfang
3. API-Bereich
4. Operation-Bereich

Die Aussagen darin erfolgt gemäß der Position der der `base` Element, sofern vorhanden.

Beispielsweise haben Sie eine Richtlinie auf globaler Ebene und eine Richtlinie für eine API konfiguriert dann immer, dass bestimmte API beide Richtlinien gelten. API-Management ermöglicht deterministischen Reihenfolge der kombinierten Richtlinien über das Basiselement. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

Im obigen Beispiel-Richtliniendefinition die `cross-domain` würde-Anweisung vor die wiederum höheren Richtlinien folgen der `find-and-replace` Richtlinie.

Wird dieselbe Richtlinie zweimal in den Richtlinien wird die zuletzt ausgewertete Richtlinie angewendet. Dies können Sie in einem höheren Bereich definierten Richtlinien überschreiben. Klicken Sie die Richtlinien im aktuellen Bereich in den Gruppenrichtlinien-Editor **Effektive Richtlinie für ausgewählte Bereich berechnen**.

Beachten Sie, dass globale Richtlinie keine übergeordnete Richtlinie und verwenden die `<base>` Element es hat keine Auswirkung. 

## <a name="next-steps"></a>Nächste Schritte

Checken Sie folgende Video Richtlinie Ausdrücken.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Policy Reference]: api-management-policy-reference.md
[Produkt]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Vorgang]: api-management-howto-add-operations.md

[Erweiterte Richtlinien]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Kontrollfluss]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Variable festlegen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Richtlinienausdrücke]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png

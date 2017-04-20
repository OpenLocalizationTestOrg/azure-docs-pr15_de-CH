<properties
   pageTitle="Lernen und BizTalk Regeln API-app in Ihrer Anwendung Logik erstellen | Microsoft Azure"
   description="In diesem Thema werden die Funktionen von BizTalk Regeln Connector und beschreibt ihre Verwendung"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk-Regeln

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Geschäftsregeln kapselt die Maßnahmen und Beschlüsse, die Geschäftsprozesse steuern. Diese Richtlinien möglicherweise formell verfahrenshandbücher, Verträge und Abkommen definiert oder als wissen und Know-how verkörperten Mitarbeiter existiert. Diese Richtlinien sind dynamisch und ändern über Zeit ändern soll Unternehmen, Vorschriften oder aus anderen Gründen.

Diese Maßnahmen in traditionellen Programmiersprachen erfordert Zeit und Koordinierung und ermöglicht keine nicht-Programmierer an Erstellung und Pflege von Unternehmens-Policies. BizTalk Regeln ermöglicht diese Maßnahmen und den Rest des Geschäftsprozesses zu entkoppeln. Dadurch wird für die erforderliche Änderung Geschäftsrichtlinien ohne Beeinträchtigung den Rest des Geschäftsprozesses.

##<a name="why-rules"></a>Warum Regeln

Es gibt 3 wichtigsten Gründe für BizTalk Geschäftsregeln Geschäftsprozess:

* Entkoppeln Sie Geschäftslogik von Anwendungscode
- Ermöglichen Sie Business Analysten mehr Kontrolle über Business Logic management
+ Ändern von Geschäftslogik schneller zur Produktion

##<a name="rules-concepts"></a>Regeln (Konzepte)

##<a name="vocabulary"></a>Vokabular

Ein _Vokabular_ ist eine Auflistung von Definitionen von Anzeigenamen für die Datenverarbeitung Objekte Regeln und Aktionen aus. Vokabulardefinitionen erleichtern die Regeln zu lesen, zu verstehen, Personen in einer bestimmten Domäne.

Vokabulare sollen die Lücke zwischen Geschäftssemantik und Implementierung. Beispielsweise zeigt eine Spalte in einer bestimmten Zeile in einer bestimmten Datenbank eine SQL-Abfrage als eine Bindung für einen Genehmigungsstatus. Statt so komplexe Darstellung in einer Regel, können Sie stattdessen eine Vokabulardefinition zugeordnet, Datenbindungsfeature mit Anzeigenamen "Status" erstellen. Anschließend können Sie in eine beliebige Anzahl von Regeln "Status" aufnehmen und Regelmodul kann die entsprechenden Daten aus der Tabelle abzurufen.

##<a name="rule"></a>Regel

Geschäftsregeln sind deklarative Aussagen, die die Durchführung von Geschäftsprozessen zu steuern. Eine Regel besteht aus einer Bedingung und Aktionen. Die Bedingung wird ausgewertet und ergibt True, initiiert der Regel-Engine eine oder mehrere Aktionen.
In der Geschäftsregel-Framework Regeln werden mit dem folgenden Format:

_IF_ _Bedingung_ _Dann_ _Aktion_

Beispiel:

*IF ist kleiner oder gleich Mittel*  
*Führen Sie Transaktion und Drucken Eingang*

Diese Regel bestimmt, ob eine Transaktion durch Anwenden von Geschäftslogik in Form eines Vergleichs von zwei monetäre Werte in Form einer Transaktion und Mittel durchgeführt werden.
Sie können die Geschäftsregel erstellen, ändern und Bereitstellen von Geschäftsregeln. Alternativ können Sie diese Aufgaben programmgesteuert ausführen.

###<a name="conditions"></a>Startbedingungen

Eine Bedingung ist ein wahr/falsch (Boolean) Ausdruck besteht aus einem oder mehreren Prädikaten.
In unserem Beispiel ist das Prädikat kleiner oder gleich der Menge und der verfügbaren Mittel zugewiesen. Diese Bedingung wird immer True oder False ausgewertet.
Prädikate können kombiniert werden, mit den logischen Operatoren AND, OR und nicht zu einem logischen Ausdruck möglicherweise sehr groß, aber immer True oder False ausgewertet werden.

###<a name="actions"></a>Aktionen

Aktionen folgen funktionale Bewertung der Bedingung. Wenn eine Bedingung erfüllt ist, werden eine entsprechende Aktion oder Aktionen initiiert.
In unserem Beispiel sind "Verhalten Transaktion" und "Drucken" Aktionen durchgeführt werden, wenn und nur wenn die Bedingung (in diesem Fall "Betrag höchstens Mittel") true ist.
Aktionen sind Vorgänge Satz XML-Dokumente in das Geschäftsregel-Framework dargestellt.

##<a name="policy"></a>Richtlinie

Eine Richtlinie ist eine logische Gruppierung von Regeln. Sie erstellen eine Richtlinie, speichern, testen und Sie mit den Ergebnissen zufrieden in einer produktiven Umgebung verwenden.

###<a name="policy-composition"></a>Richtlinie Komposition

Sie können Richtlinien im Portal Regeln erstellen. Eine Richtlinie kann eine große Gruppe von Regeln enthalten, in der Regel Verfassen einer Richtlinie Regeln, die zu einer spezifischen Domäne im Kontext der Anwendung gehören, die die Richtlinie verwenden.

###<a name="policy-testing"></a>Richtlinie testen
Effektiv können Sie einen Testlauf der Richtlinie ausführen, bevor es in einer produktiven Umgebung verwendet wird. Regeln-Portal können Sie Eingaben in einer Richtlinie bereitstellen die Richtlinie ausgeführt und die Ausgabe anzeigen. Die Ausgabe enthält Protokolle, Regel, Bewertung der Bedingung und entsprechenden Ausgaben.

##<a name="sample-scenario---insurance-claims"></a>Beispielszenario - Versicherungsansprüchen
Nehmen wir ein Beispielszenario und wie wir für dieselbe Geschäftslogik verfassen erläutern.

![ALT-text][1]

In einem Szenario mit einfach Versicherungsansprüchen übermittelt der Antragsteller seinen Versicherungsanspruch (über jeden beliebigen Client Website, Telefon App usw.). An des Unternehmens Anspruch Processing Unit und basierend auf dem Ergebnis der Verarbeitung wird diesem Anspruch anfordern, Forderung kann entweder genehmigt, abgelehnt oder auf weitere manuelle Verarbeitung.
Forderung Processing Unit in diesem Szenario wäre die Geschäftslogik des Systems umfasst. Unter Betrachtung dieser Einheit, können wir Folgendes angezeigt:

![ALT-text][2]

Lassen Sie uns nun mithilfe von Geschäftsregeln diese Geschäftslogik implementieren.


##<a name="creation-of-rules-api-app"></a>Erstellen von Regeln API-App


1. Anmeldung bei Azure-Portal
2. Select-Markt und suchen *BizTalk Regeln* neu >
3. BizTalk-Regeln aus der Liste auswählen Öffnet das Blade BizTalk-Regeln
4. Wählen Sie die Schaltfläche *Erstellen* ![Alt-Text][3]
1. Neues Blatt geöffnet wird, geben Sie die folgenden Informationen:  
    1. Name – Geben Sie einen Namen für Ihre API-Regeln
    1. App Service-Plan – auswählen oder Erstellen einer neuen App Service-Plan
    1. Tarif – wählen Sie den Tarif diese App angehören soll
    1. Ressourcengruppe – wählen oder erstellen Sie die Ressourcengruppe, in die Anwendung befinden sollten,
    2. Abonnement - wählen Sie das Abonnement verwenden möchten
    1. Position – wählen Sie den geografischen Standort, möchten die App bereitgestellt werden.
4.  Wählen Sie *Erstellen*. Innerhalb weniger Minuten würden Ihre BizTalk Regeln API-Anwendung erstellt werden.

##<a name="vocabulary-creation"></a>Vokabular erstellen
Nach dem Erstellen einer BizTalk Regeln API-App, wäre der nächste Schritt Vokabulare erstellen. Erwartung ist der Entwickler mehr Rolle dieser Übung tun. Hier ist dies getan:


1. Starten der BizTalk Regeln API App über das Portal zu durchsuchen -> API-Apps - ><Your Rules API App>. Diese erhalten Regeln API App Dashboard ähnlich wie unten Sie:

   ![ALT-text][4]

2. Wählen Sie 2. "Vokabulardefinitionen". Dies würde Vokabular Authoring Bildschirm 3. Wählen Sie "hinzufügen" Hinzufügen neuer Vokabulardefinitionen angezeigt.
Es gibt 2 Arten Vokabulardefinitionen unterstützt-Literals und XML.

##<a name="literal-definition"></a>Literalen Definition
1.  Nach dem Klicken auf "Hinzufügen" öffnet ein neues Blatt "Definition hinzufügen". Geben Sie die folgenden Werte
  1.    Name – nur alphanumerische Zeichen ohne Sonderzeichen erwartet. Dies sollte auch für Ihr vorhandenes Vokabular Liste eindeutig sein.
  2.    Beschreibung – optionales Feld.
  3.    Definitionstyp – stehen 2 unterstützt. Wählen Sie in diesem Beispiel Literal
  4.    Datentyp, ermöglicht dies den Datentyp der Definition auswählen. 4-Datentypen werden unterstützt: ich.  Zeichenfolge in Anführungszeichen ("Beispiel" String) diese Werte eingegeben werden  
    II. Boolean – Dies ist entweder true oder false  
    III.    Nummer – kann jede Dezimalzahl  
    IV. DateTime-bedeutet dies, dass die Def vom Datentyp. Daten müssen dieses Format tt/mm/jjjj hh: mm: AM\PM eingegeben  
  5. Input – ist, den Wert Ihrer Definition eingeben. Die hier eingegebenen Werte müssen dem ausgewählten Datentyp entsprechen. Sie können entweder einen einzelnen Wert einer Gruppe von Werten, getrennt durch Kommas oder einen Bereich von Werten, die das Schlüsselwort *,*eingeben. Beispielsweise können Sie einen eindeutigen Wert 1 eingeben. eine Reihe 1, 2, 3; oder einen 1 bis 5. Hinweis Bereich nur für unterstützt Zahlen.
  6. Wählen Sie *OK*.

![ALT-text][5]
##<a name="xml-definition"></a>XML-Definition
Wenn Vokabular Typ im XML-Format ausgewählt wird, muss die folgenden Eingaben angegeben werden  
  ein.    Schema, wird durch Klicken auf diese neue Blade ermöglicht Benutzer entweder aus einer Liste bereits hochgeladenen Schemas oder eine neue Hochladen auswählen geöffnet.
b.    XPATH-neben diesem Eingabefeld wird nur nach Ablauf der im vorherigen Schritt ein Schema auswählen. Auf diese zeigt das Schema, das ausgewählt wurde, und ermöglicht dem Benutzer den Knoten aus, für den eine Vokabulardefinition erstellt werden soll.  
  c.    Tatsache – diese Eingabe identifiziert das Regelmodul zur Verarbeitung der Datenobjekt zugeführt werden würde. Dies ist eine erweiterte Eigenschaft und wird standardmäßig das übergeordnete Element des ausgewählten XPATH. Besonders wichtig für Verkettung und Auflistung wird.

![ALT-text][6]

### <a name="add-bulk"></a>Fügen Sie Massen hinzu
Die oben aufgeführten Schritte haben Erfahrung zum Erstellen von Vokabulardefinitionen erfasst. Nach dem Erstellen werden erstellte Vokabulardefinitionen in Listenform angezeigt. Gibt es Anforderungen, mehrere Definitionen aus dem Schema anstatt die Schritte jedes Mal erstellen können. Dies wird hinzufügen Bulk-Funktion sehr nützlich.
Auswählen "Bulk hinzufügen" nehmen Sie ein neues Blatt. Der erste Schritt ist das Schema auszuwählen, für die mehrere Definitionen erstellt werden. Hiermit öffnen neuen Blade zulassen, entweder aus einer Liste der bereits hochgeladenen Schemas auswählen oder eine neue Hochladen zulassen.
Jetzt wird die XPath-Eigenschaft aufgehoben. Hiermit wird der Viewer öffnen, mehrere Knoten ausgewählt werden können.
Die Namen für mehrere Definitionen erstellt standardmäßig den Namen der ausgewählten Knoten. Diese können jederzeit nach der Erstellung geändert werden.

![ALT-text][7]

##<a name="policy-creation"></a>Erstellen von Richtlinien
Nachdem der Entwickler erforderlichen Vokabulare erstellt, erwartet der Wirtschaftsanalytiker ein Erstellen von Geschäftsrichtlinien Portal Azure wäre.  
    1. in der erstellten Regeln App ist eine Richtlinie Linse auf die der Benutzer in der Erstellungsseite geht.  
    2. dieser Seite zeigt die Liste von Richtlinien diese Regeln App hat. Die Analyst kann eine neue Richtlinie hinzuzufügen, einfach einen Richtliniennamen eingeben und die Tab-Taste zweimal drücken. Mehrere Richtlinien können in einer einzelnen Regeln API App befinden.  
    3. wählen die erstellte Richtlinie dauert den Benutzer die Seite Richtlinie Details kann eine Regeln finden, die in der Richtlinie.  
    ![ALT-Text][8]
 4.  Wählen Sie "Hinzufügen", um eine neue Regel hinzuzufügen. Hierdurch gelangen Sie zu einem neuen Blatt.

##<a name="rule-creation"></a>Regel erstellen
Eine Regel ist die Auflistung der Bedingung und Aktion. Aktionen werden ausgeführt, wenn die Bedingung True ergibt. Blatt Regel erstellen Geben Sie einen eindeutigen Regelnamen (für diese Richtlinie) und Beschreibung (optional).
Im Feld Bedingung (wenn) kann zum Erstellen komplexer bedingter Anweisungen verwendet werden. Folgen der unterstützten Schlüsselwörter:  
1.  Und bedingter operator  
2.  Oder bedingte operator  
3.  wird\_nicht\_vorhanden  
4.  ist vorhanden  
5.  falsch  
6.  ist\_gleich\_,  
7.  ist\_größer\_als  
8.  ist\_größer\_als\_gleich\_,  
9.  ist\_in  
10. ist\_weniger\_als  
11. ist\_weniger\_als\_gleich\_,  
12. ist\_nicht\_in  
13. ist\_nicht\_gleich\_,  
14. MOD  
15. true  

Das Action(THEN) kann mehrere Anweisungen zeilenweise zu auszuführenden Aktionen enthalten. Folgen der unterstützten Schlüsselwörter:  
1.  ist gleich  
2.  falsch  
3.  true  
4.  Anhalten  
5.  MOD  
6.  NULL  
7.  Aktualisieren  

Die Bedingung und Aktion Felder bieten Intellisense, um sich schnell eine Regel erstellen. Dies kann durch Drücken von STRG + LEERTASTE oder Eingabe erst ausgelöst werden. Stichwörter übereinstimmenden Zeichen automatisch unten gefiltert und angezeigt. Alle Schlüsselwörter und Vokabulardefinitionen zeigt das Intellisense-Fenster.
![ALT-text][9]

##<a name="explicit-forward-chaining"></a>Explizite Vorwärtsverkettung
BizTalk-Regeln unterstützt explizite vorwärts verketten, wenn Benutzer Regeln auf bestimmte Aktionen erneut auswerten möchten, können sie diese Auslösen mit bestimmten Schlüsselwörtern. Die folgenden sind unterstützten Schlüsselwörter:  
   1.   Update <vocabulary definition> – dieses Schlüsselwort alle Regeln mit der angegebenen Vokabulardefinition im Zustand erneut ausgewertet.  
   2.   Halt – dieses Schlüsselwort beendet alle regelausführungen

##<a name="enabledisable-rules"></a>Auf Regeln
Jede Regel in der Richtlinie kann aktiviert oder deaktiviert werden. Alle Regeln sind standardmäßig aktiviert. Deaktivierte Regeln nicht werden während der richtlinienausführung ausgeführt. Auf Regeln können dafür aus dem Blatt direkt-Befehle stehen in der Befehlsleiste am oberen Rand der Blade oder aus der Richtlinie im Kontextmenü (Rechtsklick auf eine Regel) kann auf.

##<a name="rule-priority"></a>Regelpriorität
Die Regeln einer Richtlinie werden nacheinander ausgeführt. Die Priorität der Ausführung wird durch die Reihenfolge bestimmt, in der Richtlinie auftreten. Diese Priorität kann durch einfaches Ziehen und Ablegen der Regel geändert werden.

##<a name="test-policy"></a>Testrichtlinie
Mit dem Befehl "Richtlinie testen" in das Blatt testen können Sie Ihre Richtlinien testen. Dieses Blatt sehen Sie eine Liste von Vokabulardefinitionen in der Richtlinie, die eine Benutzereingabe erfordern. Benutzer können Werte für diese Eingaben für ihre Testszenarien manuell hinzufügen. Auch Benutzer können importieren XMLs Eingaben überprüfen. Sobald alle Eingaben sind, kann der Test ausgeführt und Ausgaben für jede Vokabulardefinition werden in die Ausgabespalte für den Vergleich. Klicken Sie Wirtschaftsanalytiker angezeigten Protokolle, auf "Protokolle anzeigen" ausführungsprotokolle anzeigen. Zum Speichern der Protokolle steht die Option "Ausgabe speichern" speichern alle Daten für unabhängige Analyse testen.

## <a name="using-rules-in-logic-apps"></a>Mithilfe von Regeln in Logik-Apps
Nachdem die Richtlinie erstellt und getestet hat, kann er jetzt zum Verbrauch. Links der Homepage des Portals Logik Apps auswählen können Sie eine neue Logik-App erstellen. Erstellte Logik-App starten Sie anschließend *Trigger und Aktionen*. Sie können dann die Vorlage *neu erstellen* auswählen. Gehen Sie Ihre BizTalk Regeln API App Anwendung Logik hinzufügen. Dies erledigt werden die Möglichkeit der Regeln API-Anwendung (Aktion) auf. Die Liste der auszuführenden Richtlinien zählen. Wählen Sie eine bestimmte Richtlinie nach dem erforderlichen Eingaben für die Richtlinie muss. Benutzer können die Ausgabe von Regeln API-App downstream für weitere Entscheidungen.

## <a name="using-rules-via-apis"></a>Regeln über APIs
Regeln API-App kann auch über einen umfangreichen Satz von APIs aufgerufen werden. Benutzer werden nicht nur mit Logik Apps und Regeln in einer Anwendung durch REST-Aufrufe verwenden. Die genaue REST-APIs können auf "API-Definition" objektiv im Dashboard Regeln angezeigt werden.

![ALT-text][10]

Folgendes ist ein Beispiel einer API c Verwendung#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Beachten Sie, dass die oben genannten Regeln API-App die Sicherheitsstufe auf "Öffentliche Anon" festgelegt. Gesetzt aus der API-App mit - alle -> Einstellungen -> Zugriffsebene.

![ALT-text][11]

## <a name="editing-vocabulary-and-policy"></a>Vokabular und Richtlinie bearbeiten
Einer der Hauptvorteile der Verwendung von Geschäftsregeln ist, dass sich Geschäftslogik viel schnellere Produktion abgelegt werden können. Jede Änderung Vokabular und Richtlinien wird sofort in der Produktion angewendet. Benutzer muss lediglich die entsprechende Vokabulardefinition oder Richtlinie durchsuchen und ändern, damit sie wirksam.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG

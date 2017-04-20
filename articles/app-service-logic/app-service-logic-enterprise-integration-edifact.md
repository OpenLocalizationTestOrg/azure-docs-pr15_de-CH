<properties 
    pageTitle="Enterprise-Integration mit EDIFACT | Microsoft Azure" 
    description="Erfahren Sie, wie mit EDIFACT Abkommen Logik apps erstellen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Enterprise-Integration mit EDIFACT 

> [AZURE.NOTE] Diese Seite enthält die EDIFACT-Features Logik Apps. Informationen zu X12 finden Sie [hier](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>EDIFACT Vereinbarung 
Bevor Sie EDIFACT-Nachrichten austauschen können, müssen Sie EDIFACT Vereinbarung und Ihre Integration Konto speichern. Die folgenden Schritte führt Sie durch den Prozess der Erstellung einer Vereinbarung EDIFACT.

### <a name="heres-what-you-need-before-you-get-started"></a>Sie benötigen zunächst
- Ein [Konto Integration](./app-service-logic-enterprise-integration-accounts.md) in Azure-Abonnement definiert  
- Mindestens zwei [Partner](./app-service-logic-enterprise-integration-partners.md) bereits in Ihrem Konto integration  

>[AZURE.NOTE]Beim Erstellen einer Vereinbarung muss Inhalte in Nachrichten, die Sie, die empfangen/senden werden, vom Partner der Vertragstyp übereinstimmen.    


Nach [einer Integration Konto](./app-service-logic-enterprise-integration-accounts.md) und [Partner](./app-service-logic-enterprise-integration-partners.md)haben, können Sie eine EDIFACT-Vereinbarung folgendermaßen erstellen:  

### <a name="from-the-azure-portal-home-page"></a>Von Azure Homepage des Portals

Nach dem Anmelden in [Azure-Portal](http://portal.azure.com "Azure-Portal"):  
1. Wählen Sie im Menü auf der linken Seite **Durchsuchen** .  

>[AZURE.TIP]Wenn Sie den Link **Suchen** nicht sehen, müssen Sie zuerst das Menü zu erweitern. Wählen Sie zunächst den **Menü** -Link, der befindet reduzierten Menü links oben.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. *Integration* in das Suchfeld Filter geben und **Integration Konten** aus der Liste der Ergebnisse auswählen.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. Öffnet **Integration Konten** Blatt wählen Sie das Integration in dem Sie die Vereinbarung erstellen. Wenn eine Integration Konten Listen angezeigt [Erstellen Sie einen ersten](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Wählen Sie das Musterelement **Abkommen** . Wenn Abkommen Kachel zuerst hinzufügen nicht angezeigt wird.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Wählen Sie **Add** Abkommen Blatt, das geöffnet wird.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Geben Sie einen **Namen** für die Vereinbarung und wählen Sie dann den **Typ** für EDIFACT **Host Partner** **Host Identität**, **Gastpartner**, **Gast Identität**Abkommen Blatt, das geöffnet wird.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Nach dem Festlegen der vereinbarungseigenschaften wählen Sie **Erhalten Einstellungen** konfigurieren, wie Nachrichten über diese Vereinbarung behandelt werden.  
8. Control Settings erhalten gliedert sich in folgende Abschnitte Bezeichner, Bestätigung, Schemas, Steuerelement Zahlen, Validierung, Einstellungen und Batch-Verarbeitung. Konfigurieren Sie diese Eigenschaften auf Grundlage Ihrer Vereinbarung mit dem Partner Sie Nachrichten austauschen. Hier wird ein Überblick über diese Steuerelemente wie soll diese Vereinbarung zu identifizieren und eingehende Nachrichten entsprechend zu konfigurieren:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Wählen Sie **OK** , um Ihre Einstellungen speichern.  

### <a name="identifiers"></a>Bezeichner

|Eigenschaft|Beschreibung |
|---|---|
|UNB6.1 (Empfänger Verweis Kennwort)|Geben Sie einen alphanumerischen Wert zwischen 1 und 14 Zeichen.|
|UNB6.2 (Empfänger Verweis Qualifier)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal zwei Zeichen.|

### <a name="acknowledgments"></a>Danksagungen 

|Eigenschaft|Beschreibung |
|----|----|
|Nachricht (CONTRL)|Aktivieren Sie dieses Kontrollkästchen Austausch Absender eine technische Bestätigung (CONTRL) zurückzukehren. Die Bestätigung wird an den Austausch Absender auf der Basis senden für die Vereinbarung gesendet.|
|Bestätigung (CONTRL)|Aktivieren Sie dieses Kontrollkästchen, um einer funktionsbestätigung (CONTRL) an den Absender Austausch die Bestätigung zurückgegeben Austausch Absender auf der Basis senden für die Vereinbarung an.|

### <a name="schemas"></a>Schemas

|Eigenschaft|Beschreibung |
|----|----|
|UNH2.1 (TYP)|Wählen Sie eine Transaktion festlegen.|
|UNH2.2 (VERSION)|Geben Sie die Versionsnummer angezeigt. (Minimum, Zeichen, maximal drei Zeichen).|
|UNH2.3 (VERSION)|Geben Sie die Versionsnummer angezeigt. (Minimum, Zeichen, maximal drei Zeichen).|
|UNH2.5 (ZUGEORDNETE ZUGEORDNETEN CODE)|Geben Sie den zugewiesenen. (Maximal sechs Zeichen. Muss alphanumerisch sein).|
|UNG2.1 (APP ABSENDER-ID)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal 35 Zeichen.|
|UNG2.2 (APP ABSENDERQUALIFIZIERER)|Geben Sie einen alphanumerischen Wert mit maximal vier Zeichen.|
|SCHEMA|Wählen Sie das zuvor hochgeladene Schema von Ihrem zugeordneten Integration verwenden möchten.|

### <a name="control-numbers"></a>Steuerelement Zahlen

|Eigenschaft|Beschreibung |
|----|----|
|Nicht Kontrollnummer Interchange Duplikate zulassen|Aktivieren Sie dieses Kontrollkästchen doppelte Austauschvorgänge zu blockieren. Wenn aktiviert, prüft EDIFACT decodieren Aktion, dass die Austauschkontrollnummer (UNB5) für den empfangenen Austausch nicht zuvor verarbeitete Austauschkontrollnummer. Wenn eine Übereinstimmung gefunden wird, die der Austausch nicht verarbeitet.
|Doppelter UNB5 (täglich) überprüfen|Wenn Sie sich entschieden, um doppelte Austauschkontrollnummern zuzulassen, können Sie die Anzahl der Tage angeben, an denen die Überprüfung ausgeführt wird, geben den entsprechenden Wert für die Option **doppelte UNB5 (täglich) überprüfen** .|
|Nicht Gruppe Steuerelement Anzahl Duplikate zulassen|Aktivieren Sie dieses Kontrollkästchen, um Austauschvorgänge mit doppelter Kontrolle Gruppen (UNG5) zu blockieren.|
|Transaktion Satz Steuerelement Anzahl Duplikate nicht zulassen|Aktivieren Sie dieses Kontrollkästchen, um Austauschvorgänge mit doppelten Satz Steuerelement Buchungsnummern (UNH1) zu blockieren.|
|EDIFACT-Steuerelement Bestätigungsnummer|Zum Festlegen von Set Referenznummern Transaktion eine Bestätigung verwendet werden Geben Sie einen Wert für das Präfix Bereich Referenznummern und Suffix.|

### <a name="validations"></a>Prüfung

|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|Geben Sie die Nachricht. Jede Zeile Überprüfung abgeschlossen ist, wird ein anderes automatisch hinzugefügt. Wenn keine Regeln angegeben sind, markiert die Zeile als Standard für die Validierung verwendet wird.|
|EDI-Überprüfung|Aktivieren Sie das Kontrollkästchen die EDI-Datentypen gemäß EDI-Eigenschaften des Schemas, Einschränkungen, leere Elemente und nachfolgende Trennzeichen Validierung.|
|Erweiterte Prüfung|Aktivieren Sie das Kontrollkästchen Erweiterte (XSD) Validation von Austauschvorgängen Absenders Austausch aktivieren. Dazu gehören Validierung der Feldlänge Optionalität und Wiederholungsanzahl neben Überprüfung des XSD-Datentyps.|
|Führenden/abschließenden Nullen zulassen|Wählen Sie **Zulassen** zu führenden/abschließenden Nullen. **NotAllowed** erlauben keine führenden/abschließenden Nullen oder die voranstehende **Zuschneiden** und nachfolgende Nullen.|
|Zuschneiden von führenden/abschließenden Nullen|Aktivieren Sie das Kontrollkästchen alle führenden oder nachgestellten Nullen Zuschneiden|
|Nachfolgende Trennzeichen-Richtlinie|Wählen Sie **Unzulässig** soll nicht für nachfolgende Trennzeichen und Trennzeichen ein Austausch von Austausch Absender erhalten. Austausch nachfolgende Trennzeichen und Trennzeichen enthält, ist sie nichtig. Wählen Sie **Optional** Austauschvorgänge mit oder ohne nachfolgende Trennzeichen und Trennzeichen akzeptieren. Wählen Sie **obligatorisch** , wenn empfangene Austausch nachfolgende Trennzeichen und Trennzeichen enthalten muss.|

### <a name="internal-settings"></a>Einstellungen

|Eigenschaft|Beschreibung |
|----|----|
|Erstellen Sie leere XML-Tags dürfen nachgestellte Trennzeichen|Aktivieren Sie dieses Kontrollkästchen auf der Austausch Absender leere XML-Tags für nachfolgende Trennzeichen enthalten.|
|Batchverarbeitung eingehende Verarbeitung|Optionen:</br></br>**Austausch von Teilen als Transaktion - suspend Transaktionssätze in Fehler**: analysiert jede Transaktion durch Anwenden des entsprechenden Umschlags auf die Transaktion in einem Austausch in einer separaten XML-Dokument festgelegt. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung und dann nur die Transaktionssätze angehalten werden. Austausch von Teilen als Transaktion - Austausch Fehler unterbrechen: analysiert jede Transaktion durch die Anwendung des entsprechenden Umschlags ein Austausch in einer separaten XML-Dokument festgelegt. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung wird der gesamte Austausch angehalten wird.</br></br>**Beibehalten Austausch - Transaktionssätze in Fehler unterbrechen**: intakt Austausch, erstellen ein XML-Dokument für den gesamten Batch Austausch. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung wird nur die Transaktionssätze angehalten werden, während andere Transaktionssätze verarbeitet werden.</br></br>**Beibehalten Austausch - Austausch bei Fehler anhalten**: intakt Austausch, erstellen ein XML-Dokument für den gesamten Batch Austausch. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung wird der gesamte Austausch angehalten.|

Die Vereinbarung kann eingehende Nachrichten verarbeiten, die gewählte Einstellungen entsprechen.

Die Einstellungen, die Nachrichten verarbeiten, die an Partner gesendet:  
10. Wählen Sie **Senden Einstellungen** zu konfigurieren, wie Nachrichten über diese Vereinbarung behandelt werden.  

Steuerelement gesendet Settings gliedert sich in folgende Abschnitte Bezeichner, Bestätigung, Schemas, Umschläge, Zeichensätze und Trennzeichen, Steuerelement Zahlen und Validierung. 

Hier ist eine Ansicht dieser Steuerelemente. Treffen Sie die Grundlage wie Partnern über diese Vereinbarung gesendeten Nachrichten verarbeitet werden soll:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Wählen Sie **OK** , um Ihre Einstellungen speichern.  

### <a name="identifiers"></a>Bezeichner
|Eigenschaft|Beschreibung |
|----|----|
|UNB1.2 (Syntax Version)|Wählen Sie einen Wert zwischen **1** und **4**ein.|
|UNB2.3 (Reverse Routing Absenderadresse)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen mit maximal 14 Zeichen.|
|UNB3.3 (Empfänger Reverse Routingadresse)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen mit maximal 14 Zeichen.|
|UNB6.1 (Empfänger Verweis Kennwort)|Geben Sie einen alphanumerischen Wert mindestens ein und maximal 14 Zeichen.|
|UNB6.2 (Empfänger Verweis Qualifier)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal zwei Zeichen.|
|UNB7 (Anwendung Reference ID)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen mit maximal 14 Zeichen|

### <a name="acknowledgment"></a>Bestätigung
|Eigenschaft|Beschreibung |
|----|----|
|Nachricht (CONTRL)|Aktivieren Sie dieses Kontrollkästchen, wenn der gehostete Partner erwartet um eine technische Bestätigung (CONTRL) erhalten. Diese Einstellung gibt an, dass der gehostete Partner, der die Nachricht sendet eine Bestätigung von gastpartner anfordert.|
|Bestätigung (CONTRL)|Aktivieren Sie dieses Kontrollkästchen, wenn der gehostete Partner erwartet eine funktionale Bestätigung (CONTRL). Diese Einstellung gibt an, dass der gehostete Partner, der die Nachricht sendet eine Bestätigung von gastpartner anfordert.|
|Generieren Sie SG1-SG4 Schleife für akzeptierte Transaktionssätze|Eine funktionale Bestätigung anfordern, wählen Sie dieses Kontrollkästchen, um die Generierung von SG1-SG4 Schleifen in funktionale CONTRL Danksagungen für akzeptierte Transaktionssätze erzwingen.|

### <a name="schemas"></a>Schemas
|Eigenschaft|Beschreibung |
|----|----|
|UNH2.1 (TYP)|Wählen Sie eine Transaktion festlegen.|
|UNH2.2 (VERSION)|Geben Sie die Versionsnummer angezeigt.|
|UNH2.3 (VERSION)|Geben Sie die Versionsnummer angezeigt.|
|SCHEMA|Wählen Sie das zu verwendende Schema. Schemas befinden sich in Ihrem Konto Integration. Zugriff auf Ihre Schemas zunächst verknüpfen Sie Ihr Konto Integration für Ihre Anwendung Logik.|

### <a name="envelopes"></a>Umschläge
|Eigenschaft|Beschreibung |
|----|----|
|UNB8 (Prioritätscode Processing)|Geben Sie einen alphabetischen Wert ist nicht mehr als ein Zeichen.|
|UNB10 (Kommunikation Vertrag)|Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal 40 Zeichen.|
|UNB11 (Test Indikator)|Aktivieren Sie dieses Kontrollkästchen, um anzugeben, dass der Austausch generiert Daten|
|Anwenden von UNA-Segment (Service Zeichenfolge Beratung)|Aktivieren Sie dieses Kontrollkästchen generiert ein UNA-Segment für den Austausch gesendet werden.|
|UNG Segmente (Funktion Gruppenkopf) anwenden|Aktivieren Sie dieses Kontrollkästchen gruppieren Segmente in der Funktionsgruppenkopfzeile Gast Partner gesendete Nachrichten erstellen. Folgende Werte werden verwendet, um UNG Segmente zu erstellen:</br></br>**UNG1**Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal sechs Zeichen.</br></br>**UNG2.1**Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal 35 Zeichen.</br></br>**UNG2.2**Geben Sie einen alphanumerischen Wert mit maximal vier Zeichen.</br></br>**UNG3.1**Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal 35 Zeichen.</br></br>**UNG3.2**Geben Sie einen alphanumerischen Wert mit maximal vier Zeichen.</br></br>**UNG6**Geben Sie einen alphanumerischen Wert mindestens ein und maximal drei Zeichen.</br></br>**UNG7.1**Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal drei Zeichen.</br></br>**UNG7.2**Geben Sie einen alphanumerischen Wert mindestens ein Zeichen und maximal drei Zeichen.</br></br>**UNG7.3**Geben Sie einen alphanumerischen Wert mindestens 1 Zeichen und maximal 6 Zeichen.</br></br>**UNG8**Geben Sie einen alphanumerischen Wert mindestens ein Zeichen mit maximal 14 Zeichen.|

### <a name="character-sets-and-separators"></a>Zeichensätze und Trennzeichen
Außer der Zeichensatz können Trennzeichen für jeden Nachrichtentyp verwendet einen anderen Satz eingeben. Wenn ein Zeichensatz nicht für eine bestimmte Nachrichtenschema angegeben ist, wird der Standardzeichensatz verwendet.

|Eigenschaft|Beschreibung |
|----|----|
|UNB1.1 (System-ID)|Wählen Sie den EDIFACT-Zeichensatz auf ausgehenden Austausch.|
|Schema|Wählen Sie ein Schema aus der Dropdown-Liste. Nach Abschluss jeder Zeile wird eine neue Zeile hinzugefügt. Wählen Sie für das ausgewählte Schema Trennzeichen verwendet werden soll:</br></br>**Komponenten-Elementtrennzeichen** – Geben Sie ein einzelnes Zeichen zusammengesetzten Daten trennen.</br></br>**Daten Elementtrennzeichen** – Geben Sie ein einzelnes Zeichen einfache Datenelemente in zusammengesetzten Daten trennen.</br></br></br></br>**Ersatzzeichen** – wählen Sie dieses Kontrollkästchen, wenn die Nutzlast enthaltenen Daten Zeichen werden auch als Daten, Segment oder Komponente Trennzeichen verwendet. Sie können dann ein Ersatzzeichen eingeben. Beim Generieren der ausgehenden EDIFACT-Nachricht werden alle Instanzen von Trennzeichen in der Nutzlast mit dem angegebenen Zeichen ersetzt.</br></br>**Segmentabschlusszeichen** – Geben Sie ein einzelnes Zeichen an das Ende einer EDI-Segment.</br></br>**Suffix** – wählen Sie das Zeichen, das mit der Segment-ID verwendet wird. Ein Suffix festlegen, kann das Segment Terminator Element leer sein. Wenn Segmentabschlusszeichen leer ist, müssen Sie ein Suffix angeben.|

### <a name="control-numbers"></a>Steuerelement Zahlen
|Eigenschaft|Beschreibung |
|----|----|
|UNB5 (Interchange Kontrollnummer)|Geben Sie ein Präfix ein Wertebereich für die Austauschkontrollnummer und einem Suffix. Diese Werte werden verwendet, um einen ausgehenden Austausch erstellen. Präfix und Suffix sind optional. erforderlich ist. Die Nummer wird bei jeder neuen Nachricht erhöht; Präfix und Suffix bleiben unverändert.|
|UNG5 (Gruppe Kontrollnummer)|Geben Sie ein Präfix ein Wertebereich für die Austauschkontrollnummer und einem Suffix. Diese Werte dienen die Gruppenkontrollnummer generieren. Präfix und Suffix sind optional. erforderlich ist. Die Nummer wird für jede neue Nachricht erhöht, bis der Maximalwert erreicht ist; Präfix und Suffix bleiben unverändert.|
|UNH1 (Message Header-Nummer)|Geben Sie ein Präfix ein Wertebereich für die Austauschkontrollnummer und einem Suffix. Diese Werte dienen die Referenznummer Nachricht Header generieren. Präfix und Suffix sind optional. die Referenznummer ist erforderlich. Die Nummer wird bei jeder neuen Nachricht erhöht; Präfix und Suffix bleiben unverändert.|

### <a name="validations"></a>Prüfung
|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|Diese Option ermöglicht eine Validierung auf der Empfänger des Austauschs. Diese Validierung führt EDI-Überprüfung Transaktionssatz Datenelemente Datentypen Einschränkungen und leere Daten überprüfen und Schulung Trennzeichen.|
|EDI-Überprüfung|Aktivieren Sie das Kontrollkästchen die EDI-Datentypen gemäß EDI-Eigenschaften des Schemas, Einschränkungen, leere Elemente und nachfolgende Trennzeichen Validierung.|
|Erweiterte Prüfung|Diese Option ermöglicht erweiterten Überprüfung des Absenders Austausch empfangenen Austauschvorgänge. Dazu gehören Validierung der Feldlänge Optionalität und Wiederholungsanzahl neben Überprüfung des XSD-Datentyps. Aktiviert Validierung Erweiterung ohne EDI-Überprüfung aktivieren oder umgekehrt.|
|Lassen Sie führenden/abschließenden Nullen zu|Diese Option gibt an, dass ein EDI-Austauschs von der Partei erhalten keine Validierung fehlschlägt, wenn ein Datenelement eines EDI-Austauschs nicht der Anforderung aufgrund der Länge oder Leerzeichen entspricht, jedoch entspricht der Länge erforderlich wenn sie entfernt werden.|
|Zuschneiden von führenden/abschließenden Nullen|Diese Option wird die führenden und nachfolgenden Nullen trimmen.|
|Nachfolgende Trennzeichen|Diese Option gibt ein EDI-Austauschs von der Partei erhalten nicht Validierung fehlschlägt, wenn ein Datenelement eines EDI-Austauschs entspricht nicht der Länge erforderlich (oder nachfolgende) führende Nullen oder Leerzeichen entspricht der Länge erforderlich wenn sie entfernt werden.</br></br>Wählen Sie **Unzulässig** soll nicht für nachfolgende Trennzeichen und Trennzeichen ein Austausch von Austausch Absender erhalten. Austausch nachfolgende Trennzeichen und Trennzeichen enthält, ist sie nichtig.</br></br>Wählen Sie **Optional** Austauschvorgänge mit oder ohne nachfolgende Trennzeichen und Trennzeichen akzeptieren.</br></br>Wählen Sie **obligatorisch** , wenn empfangene Austausch nachfolgende Trennzeichen und Trennzeichen enthalten muss.|

Nach dem Wählen Sie **OK** auf dem geöffneten Blatt:  
12. Wählen Sie das Musterelement **Abkommen** Blade-Integration Konto und finden Sie neue Vereinbarung aufgeführt.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>Weitere Informationen
- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket")  

<properties 
    pageTitle="Übersicht über X12 und Enterprise-Integrationspaket | Microsoft Azure App Service | Microsoft Azure" 
    description="Informationen zur Verwendung von X12 zu Logik apps erstellen" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Enterprise-Integration mit X12 

>[AZURE.NOTE]Diese Seite behandelt die X12 Features Logik Apps. Informationen zum EDIFACT [hier](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Erstellen einer X12 Vereinbarung 
Bevor Sie X12 austauschen können Nachrichten, müssen Sie eine X12 erstellen Abkommens und Ihr Konto Integration. Die folgenden Schritte führen Sie durch den Prozess der Erstellung einer X12 Vereinbarung.

### <a name="heres-what-you-need-before-you-get-started"></a>Sie benötigen zunächst
- Ein [Konto Integration](./app-service-logic-enterprise-integration-accounts.md) in Azure-Abonnement definiert  
- Mindestens zwei [Partner](./app-service-logic-enterprise-integration-partners.md) bereits in Ihrem Konto integration  

>[AZURE.NOTE]Beim Erstellen einer Vereinbarung muss der Inhalt der Datei Vereinbarung der Vertragstyp übereinstimmen.    


Nachdem [ein Integration Konto](./app-service-logic-enterprise-integration-accounts.md) und [Partner](./app-service-logic-enterprise-integration-partners.md)haben, können Sie eine X12 erstellen Abkommens folgendermaßen:  

### <a name="from-the-azure-portal-home-page"></a>Von Azure Homepage des Portals

Nach dem Anmelden in [Azure-Portal](http://portal.azure.com "Azure-Portal"):  
1. Wählen Sie im Menü auf der linken Seite **Durchsuchen** .  

>[AZURE.TIP]Wenn Sie den Link **Suchen** nicht sehen, müssen Sie zuerst das Menü zu erweitern. Wählen Sie zunächst den **Menü** -Link, der befindet reduzierten Menü links oben.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integration* in das Suchfeld Filter geben und **Integration Konten** aus der Liste der Ergebnisse auswählen.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. Öffnet **Integration Konten** Blatt wählen Sie das Integration in dem Sie die Vereinbarung erstellen. Wenn eine Integration Konten Listen angezeigt [Erstellen Sie einen ersten](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Wählen Sie das Musterelement **Abkommen** . Wenn Abkommen Kachel zuerst hinzufügen nicht angezeigt wird.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Wählen Sie **Add** Abkommen Blatt, das geöffnet wird.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Geben Sie einen **Namen** für die Vereinbarung und wählen Sie dann den **Typ** **Host Partner** **Host Identität**, **Gastpartner**, **Gast Identität**Abkommen Blatt, das geöffnet wird.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Nachdem Sie den Empfang Eigenschaften festgelegt haben, wählen Sie die Schaltfläche **OK**  
Weiter:  
8. Wählen Sie **Erhalten Einstellungen** zu konfigurieren, wie Nachrichten über diese Vereinbarung behandelt werden.  
9. Die folgenden Abschnitten Bezeichner, Bestätigung, Schemas, Umschläge, Steuerelement Zahlen, Validierung und Einstellungen erhalten Einstellungen steuern unterteilt. Konfigurieren Sie diese Eigenschaften auf Grundlage Ihrer Vereinbarung mit dem Partner Sie Nachrichten austauschen. Hier wird ein Überblick über diese Steuerelemente wie soll diese Vereinbarung zu identifizieren und eingehende Nachrichten entsprechend zu konfigurieren:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Wählen Sie **OK** , um Ihre Einstellungen speichern.  

### <a name="identifiers"></a>Bezeichner

|Eigenschaft|Beschreibung |
|---|---|
|ISA1 (Autorisierung Qualifier)|Den Wert des Qualifizierers Autorisierung aus der Dropdown-Liste auswählen|
|ISA2|Optional. Autorisierungswert eingeben. Wenn der ISA1 eingegebene Wert als 00, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10.|
|ISA3 (Sicherheit Qualifier)|Den Wert des Qualifizierers aus der Dropdown-Liste auswählen|
|ISA4|Optional. Geben Sie Security Informationen. Wenn der für ISA3 eingegebene Wert als 00, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10.|

### <a name="acknowledgments"></a>Danksagungen 

|Eigenschaft|Beschreibung |
|----|----|
|TA1 erwartet|Aktivieren Sie dieses Kontrollkästchen Austausch Absender eine technische Bestätigung (TA1) zurückzukehren. Diese Anerkennungen werden Austausch Absender auf der Basis senden für die Vereinbarung.|
|Anlagen erwartet|Aktivieren Sie dieses Kontrollkästchen Austausch Absender eine funktionale (FA) Bestätigung zurückzukehren. Wählen Sie, ob die Empfangsbestätigungen 997 oder 999 basierend auf Schemaversionen mit arbeiten soll. Diese Anerkennungen werden Austausch Absender auf der Basis senden für die Vereinbarung.|
|AK2-IK2-Schleife enthalten|Aktivieren Sie dieses Kontrollkästchen Generation AK2 Loops in funktionale Danksagungen für akzeptierte Transaktionssätze aktivieren. Hinweis: Dieses Kontrollkästchen ist nur aktiviert, wenn das Kontrollkästchen Anlagen erwartet.|

### <a name="schemas"></a>Schemas

Wählen Sie ein Schema für jede Buchungsart (ST1) und Absender Anwendung (GS2). Die Empfangspipeline disassembliert die eingehende Nachricht durch Vergleichen der Werte für ST1 und GS2 in der eingehenden Nachricht hier festgelegten Werte und das Schema mit dem Schema der eingehenden Nachricht hier festgelegt.

|Eigenschaft|Beschreibung |
|----|----|
|Version|Wählen Sie die X12 Version|
|Art des Geschäftes (ST01)|Wählen Sie die Art des Geschäftes|
|Absender-Anwendung (GS02)|Wählen Sie die Absender-Anwendung|
|Schema|Wählen Sie die Schemadatei möchten uns. Schemadateien befinden sich in Ihrem Konto Integration.|

### <a name="envelopes"></a>Umschläge

|Eigenschaft|Beschreibung |
|----|----|
|ISA11 Verwendung|Verwenden Sie dieses Feld in einem Trennzeichen angeben:</br></br>Wählen Sie Standard ID der dezimale Notation von "." statt Dezimalschreibweise des eingehenden Dokuments in der EDI-Empfangspipeline.</br></br>Wählen Sie Wiederholung Trennzeichen das Trennzeichen für wiederholte Vorkommen ein einfaches Datenelement oder wiederholte Datenstruktur an. (^) Wird beispielsweise normalerweise als Trennzeichen für Wiederholung verwendet. HIPAA-Schemas können Sie nur (^) verwenden.|

### <a name="control-numbers"></a>Steuerelement Zahlen

|Eigenschaft|Beschreibung |
|----|----|
|Nicht Kontrollnummer Interchange Duplikate zulassen|Aktivieren Sie diese Option doppelte Austauschvorgänge zu blockieren. Wenn aktiviert, überprüft das BizTalk-Portal Austauschkontrollnummer (ISA13) für den Austausch empfangen die Austauschkontrollnummer nicht übereinstimmen. Wenn eine Übereinstimmung gefunden wird, verarbeitet die Empfangspipeline Austausch nicht.<br/>Wenn Sie sich entschieden, um doppelte Austauschkontrollnummern zuzulassen, können Sie die Anzahl der Tage angeben an denen die Überprüfung ausgeführt wird, geben den entsprechenden Wert für Kontrollkästchen für doppelte ISA13 alle X Tage.|
|Nicht Gruppe Steuerelement Anzahl Duplikate zulassen|Aktivieren Sie diese Option Austauschvorgänge mit doppelten Gruppe Steuerelement blockiert.|
|Transaktion Satz Steuerelement Anzahl Duplikate nicht zulassen|Aktivieren Sie diese Option Austauschvorgänge mit doppelten Satz Steuerelement Buchungsnummern blockieren.|

### <a name="validations"></a>Prüfung

|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|EDI-Nachrichtentyp 850 Bestellung oder Bestätigung 999-Implementierung.|
|EDI-Überprüfung|Führt eine Validierung EDI-Datentypen gemäß EDI-Eigenschaften des Schemas, Einschränkungen, leere Elemente und nachfolgende Trennzeichen.|
|Erweiterte Prüfung|Ist der Datentyp nicht EDI, Validierung Anforderung Element ist und Wiederholung, Enumerationen und Validierung Element Länge (min/Max) zulässig.|
|Führenden/abschließenden Nullen zulassen|Zusätzlichen Speicherplatz und, die führende oder nachfolgende Nullen werden beibehalten. Sie werden nicht entfernt.|
|Nachfolgende Trennzeichen-Richtlinie|Nachfolgende Trennzeichen empfangen Austausch generiert. Optionen NotAllowed, Optional oder obligatorisch.|

### <a name="internal-settings"></a>Einstellungen

|Eigenschaft|Beschreibung |
|----|----|
|Konvertieren Sie implizites Dezimalformat Nn Basis 10 numerischen Wert|Konvertiert eine EDI-Zahl, die mit dem Format Nn in einen numerischen Wert Basis 10 der zwischengeschalteten XML im BizTalk-Portal angegeben ist.|
|Erstellen Sie leere XML-Tags dürfen nachgestellte Trennzeichen|Aktivieren Sie dieses Kontrollkästchen auf der Austausch Absender leere XML-Tags für nachfolgende Trennzeichen enthalten.|
|Batchverarbeitung eingehende Verarbeitung|Austausch von Teilen als Transaktionssätze - suspend Transaktionssätze Fehler: analysiert jede Transaktion durch Anwenden des entsprechenden Umschlags auf die Transaktion in einem Austausch in einer separaten XML-Dokument festgelegt. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung und BizTalk-Dienste nur Transaktionssätze hält. </br></br>Austausch von Teilen als Transaktionssätze - Austausch Fehler unterbrechen: analysiert jede Transaktion durch die Anwendung des entsprechenden Umschlags ein Austausch in einer separaten XML-Dokument festgelegt. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung und BizTalk-Dienste den gesamten Austausch hält.</br></br>Erhalten der Austausch - Transaktionssätze bei Fehler anhalten: intakt Austausch, erstellen ein XML-Dokument für den gesamten Batch Austausch. Diese Option setzt OnAe oder weitere Transaktion im Austausch bei der Validierung und BizTalk-Dienste nur diese Transaktion legt gleichzeitig zu anderen Transaktionssätze unterbricht.</br></br>Erhalten der Austausch - Austausch bei Fehler anhalten: intakt Austausch, erstellen ein XML-Dokument für den gesamten Batch Austausch. Diese Option setzt mindestens eine Transaktion im Austausch bei der Validierung und BizTalk-Dienste den gesamten Austausch hält.</br></br>|

Ihr Vertrag kann eingehende Nachrichten verarbeiten, die gewählte Schema entsprechen.

Die Einstellungen, die Nachrichten verarbeiten, die an Partner gesendet:  
11. Wählen Sie **Senden Einstellungen** zu konfigurieren, wie Nachrichten über diese Vereinbarung behandelt werden.  

Steuerelement gesendet Settings gliedert sich in folgende Abschnitte Bezeichner, Bestätigung, Schemas, Umschläge, Steuerelement Zahlen, Zeichensätze und Trennzeichen und Validierung. 

Hier ist eine Ansicht dieser Steuerelemente. Treffen Sie die Grundlage wie Partnern über diese Vereinbarung gesendeten Nachrichten verarbeitet werden soll:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Wählen Sie **OK** , um Ihre Einstellungen speichern.  

### <a name="identifiers"></a>Bezeichner
|Eigenschaft|Beschreibung |
|----|----|
|Autorisierung Qualifizierer (ISA1)|Den Wert des Qualifizierers Autorisierung aus der Dropdown-Liste auswählen|
|ISA2|Autorisierungswert eingeben. Wenn dieser Wert als 00 ist, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10.|
|Sicherheit-Qualifizierer (ISA3)|Den Wert des Qualifizierers aus der Dropdown-Liste auswählen|
|ISA4|Geben Sie Security Informationen. Wenn dieser Wert als 00 für das Textfeld Wert (ISA4) Geben Sie einen alphanumerischen Wert mindestens und höchstens 10.|

### <a name="acknowledgment"></a>Bestätigung
|Eigenschaft|Beschreibung |
|----|----|
|TA1 erwartet|Aktivieren Sie dieses Kontrollkästchen Austausch Absender eine technische Bestätigung (TA1) zurückzukehren. Diese Einstellung gibt an, dass der Host-Partner, der die Nachricht sendet eine Bestätigung von gastpartner in der Vereinbarung anfordert. Diese Empfangsbestätigungen erwartet Host Partners basierend auf dem erhalten der Vereinbarung.|
|Anlagen erwartet|Aktivieren Sie dieses Kontrollkästchen zum Austausch Absender eine funktionale (FA) Bestätigung zurückkehren und dann auswählen, ob die bereitgestellten 997 oder 999 basierend auf Schemaversionen, mit dem Sie arbeiten. Diese Empfangsbestätigungen erwartet Host Partners basierend auf dem erhalten der Vereinbarung.|
|FA-Version|Wählen Sie die FA-version|

### <a name="schemas"></a>Schemas
|Eigenschaft|Beschreibung |
|----|----|
|Version|Wählen Sie die X12 Version|
|Art des Geschäftes (ST01)|Wählen Sie die Art des Geschäftes|
|SCHEMA|Wählen Sie das zu verwendende Schema. Schemas befinden sich in Ihrem Konto Integration. Zugriff auf Ihre Schemas zunächst verknüpfen Sie Ihr Konto Integration für Ihre Anwendung Logik.|

### <a name="envelopes"></a>Umschläge
|Eigenschaft|Beschreibung |
|----|----|
|ISA11 Verwendung|Verwenden Sie dieses Feld in einem Trennzeichen angeben:</br></br>Wählen Sie Standard ID der dezimale Notation von "." statt Dezimalschreibweise des eingehenden Dokuments in der EDI-Empfangspipeline.</br></br>Wählen Sie Wiederholung Trennzeichen das Trennzeichen für wiederholte Vorkommen ein einfaches Datenelement oder wiederholte Datenstruktur an. (^) Wird beispielsweise normalerweise als Trennzeichen für Wiederholung verwendet. HIPAA-Schemas können Sie nur (^) verwenden.</br>|
|Wiederholung-Trennzeichen|Geben Sie das Trennzeichen Wiederholung|
|Steuerelement-Versionsnummer (ISA12)|Wählen Sie das BizTalk-Portal zum Generieren eines ausgehenden Austauschs dient Version der standard X12.|
|Verwendung Indikator (ISA15)|Geben Sie, ob der Kontext eines Austauschs Informationen, Daten (P) oder Testdaten Sie (T). Die EDI empfangen Pipeline fördert diese Eigenschaft zum Kontext.|
|Schema|Sie geben wie BizTalk Services GS und ST-Segmente für ein X12-codierten Austausch generiert, die an die Sendepipeline sendet.</br></br>Sie können Werte von GS1, GS2, GS3, GS4, GS5, GS7 und GS8 von Datenelementen mit der Buchungsart und Release Version Elemente zuordnen. Das BizTalk-Portal bestimmt eine XML-Nachricht enthält die Werte für die Buchungsart und Release Version Elemente in einer Zeile des Rasters, und füllt die Datenelemente GS1, GS2, GS3, GS4, GS5, GS7 und GS8 im Umschlag der ausgehenden Austausch mit den Werten aus der gleichen Zeile des Rasters. Die Werte der Buchungsart und Release Version Elemente müssen eindeutig sein.</br></br>Optional. Wählen Sie für GS1 aus der Dropdown-Liste einen Wert für den Funktionscode.</br></br>Erforderlich. Geben Sie einen alphanumerischen Wert für GS2 Anwendung Absender mit mindestens zwei und maximal 15 Zeichen.</br></br>Erforderlich. Geben Sie für GS3 einen alphanumerischen anwendungsempfänger mit mindestens zwei und maximal 15 Zeichen.</br></br>Optional. GS4 wählen Sie CCYYMMDD oder JJMMTT.</br></br>Optional. Wählen Sie GS5 HHMM, HHMMSS oder HHMMSSdd.</br></br>Optional. Wählen Sie für GS7 aus der Dropdown-Liste einen Wert für die zuständige Behörde.</br></br>Optional. Geben Sie einen alphanumerischen Wert für GS8 für das Dokument mindestens ein Zeichen und höchstens 12 Zeichen zugeordnet.</br></br>**Hinweis**: sind die Werte, die das BizTalk-Portal GS Felder des Austauschs erstellt wird, wenn die Buchung eingeben und Release Version Elemente in der gleichen Zeile sind für den Austausch zugeordnet.|

### <a name="control-numbers"></a>Steuerelement Zahlen
|Eigenschaft|Beschreibung |
|----|----|
|Interchange Kontrollnummer (ISA13)|Erforderlich. Geben Sie einen Wertebereich für die Austauschkontrollnummer von BizTalk Services Portal generieren einen ausgehenden Austausch verwendet. Geben Sie einen numerischen Wert mindestens 1 und maximal 999999999.|
|Gruppenkontrollnummer (GS06)|Erforderlich. Geben Sie die Zahlen, die das BizTalk-Portal für die Gruppenkontrollnummer verwenden soll. Geben Sie einen numerischen Wert mindestens ein Zeichen und maximal neun Zeichen.|
|Transaktionsmenge Kontrollnummer (ST02)|Festlegen Kontrolle Anzahl (ST02) Geben Sie eine numerische Werte für mittlere Pflichtfelder und alphanumerische Werte für die optionalen Präfix und Suffix. Maximal alle vier Felder ist neun Zeichen.|
|Präfix|Um den Zahlenbereich Transaktion Satz Steuerelement verwendet eine Bestätigung festzulegen, geben Sie Werte in Felder die ACK-Nummer (ST02). Geben Sie einen numerischen Wert für die mittleren beiden Felder und einen alphanumerischen Wert (falls gewünscht) für die Präfix und Suffix. Mittlere Felder sind erforderlich und die minimalen und maximalen Werte für die Kontrollnummer; Präfix und Suffix sind optional. Die maximale Länge für alle drei Felder ist neun Zeichen.|
|Suffix|Um den Zahlenbereich Transaktion Satz Steuerelement verwendet eine Bestätigung festzulegen, geben Sie Werte in Felder die ACK-Nummer (ST02). Geben Sie einen numerischen Wert für die mittleren beiden Felder und einen alphanumerischen Wert (falls gewünscht) für die Präfix und Suffix. Mittlere Felder sind erforderlich und die minimalen und maximalen Werte für die Kontrollnummer; Präfix und Suffix sind optional. Die maximale Länge für alle drei Felder ist neun Zeichen.|

### <a name="character-sets-and-separators"></a>Zeichensätze und Trennzeichen
Außer der Zeichensatz können Trennzeichen für jeden Nachrichtentyp verwendet einen anderen Satz eingeben. Wenn ein Zeichensatz nicht für eine bestimmte Nachrichtenschema angegeben ist, wird der Standardzeichensatz verwendet.

|Eigenschaft|Beschreibung |
|----|----|
|Zeichensatz verwendet werden|Wählen Sie die X12-Zeichensatz Eigenschaften überprüfen, die Sie für die Vereinbarung eingeben.</br></br>**Hinweis**: der BizTalk-Dienste Portal nur mithilfe dieser Einstellung um der zugehörigen Vereinbarung Eigenschaften eingegebenen Werte zu validieren. Die Empfangspipeline oder Sendepipeline ignoriert diese Eigenschaft Zeichensatz bei der Ausführungszeit.|
|Schema|Wählen Sie das Pluszeichen (+) Symbol und wählen Sie ein Schema aus der Dropdown Liste. Wählen Sie für das ausgewählte Schema Trennzeichen verwendet werden soll:</br></br>Komponenten-Elementtrennzeichen – Geben Sie ein einzelnes Zeichen zusammengesetzten Daten trennen.</br></br>Element-Trennzeichen – geben Sie ein einzelnes Zeichen einfache Datenelemente in zusammengesetzten Daten trennen.</br></br></br></br>Ersatzzeichen – wählen Sie dieses Kontrollkästchen, wenn die Nutzlast enthaltenen Daten Zeichen werden auch als Daten, Segment oder Komponente Trennzeichen verwendet. Sie können dann ein Ersatzzeichen eingeben. Erstellung der ausgehenden Nachricht X12 alle Instanzen von Trennzeichen in der Nutzlast Daten mit dem angegebenen Zeichen ersetzt.</br></br>Segment Terminator – Geben Sie ein einzelnes Zeichen an das Ende einer EDI-Segment.</br></br>Suffix – wählen Sie das Zeichen, das mit der Segment-ID verwendet wird. Ein Suffix festlegen, kann das Segment Terminator Element leer sein. Wenn Segmentabschlusszeichen leer ist, müssen Sie ein Suffix angeben.|

### <a name="validation"></a>Validierung
|Eigenschaft|Beschreibung |
|----|----|
|Nachrichtentyp|Diese Option ermöglicht eine Validierung auf der Empfänger des Austauschs. Diese Validierung führt EDI-Überprüfung Transaktionssatz Datenelemente Datentypen Einschränkungen und leere Daten überprüfen und nachfolgende Trennzeichen.|
|EDI-Überprüfung||
|Erweiterte Prüfung|Diese Option ermöglicht erweiterten Überprüfung des Absenders Austausch empfangenen Austauschvorgänge. Dazu gehören Validierung der Feldlänge Optionalität und Wiederholungsanzahl neben Überprüfung des XSD-Datentyps. Aktiviert Validierung Erweiterung ohne EDI-Überprüfung aktivieren oder umgekehrt.|
|Führende / nachgestellte Nullen zulassen|Diese Option gibt an, dass ein EDI-Austauschs von der Partei erhalten keine Validierung fehlschlägt, wenn ein Datenelement eines EDI-Austauschs nicht der Anforderung aufgrund der Länge oder Leerzeichen entspricht, jedoch entspricht der Länge erforderlich wenn sie entfernt werden.|
|Nachfolgende Trennzeichen|Diese Option gibt ein EDI-Austauschs von der Partei erhalten nicht Validierung fehlschlägt, wenn ein Datenelement eines EDI-Austauschs entspricht nicht der Länge erforderlich (oder nachfolgende) führende Nullen oder Leerzeichen entspricht der Länge erforderlich wenn sie entfernt werden.</br></br>Wählen Sie unzulässig soll nicht für nachfolgende Trennzeichen und Trennzeichen ein Austausch von Austausch Absender erhalten. Austausch nachfolgende Trennzeichen und Trennzeichen enthält, ist sie nichtig.</br></br>Wählen Sie Optional Austauschvorgänge mit oder ohne nachfolgende Trennzeichen und Trennzeichen akzeptieren.</br></br>Wählen Sie obligatorisch, wenn empfangene Austausch nachfolgende Trennzeichen und Trennzeichen enthalten muss.|

Nach dem Wählen Sie **OK** auf der öffnen:  
13. Wählen Sie das Musterelement **Abkommen** Blade-Integration Konto und finden Sie neue Vereinbarung aufgeführt.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Weitere Informationen
- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket")  

<properties
    pageTitle="Versionsinformationen für Azure BizTalk-Dienste | Microsoft Azure BizTalk-Dienste"
    description="Beschreibt bekannte Probleme für Azure BizTalk-Dienste" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Versionshinweise für Azure BizTalk-Dienste

Die Versionshinweise für Microsoft Azure BizTalk-Dienste enthalten bekannte Probleme in dieser Version.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Was ist neu im November-Update der BizTalk-Dienste
* Verschlüsselung ruhender kann im BizTalk-Portal aktiviert. [Aktivieren Sie die Verschlüsselung ruhender im BizTalk-Portal](https://msdn.microsoft.com/library/azure/dn874052.aspx)anzeigen

## <a name="update-history"></a>Aktualisierungshistorie

### <a name="october-update"></a>Oktober aktualisieren

* Organisationseinheiten Konten werden unterstützt:  
 * **Szenario**: registriert eine BizTalk Service-Bereitstellung mit einem microsoftkonto (z. B. user@live.com). In diesem Szenario können Benutzer nur Microsoft Account BizTalk Service über das Portal für BizTalk-Dienste verwalten. Organisatorische Konto kann nicht verwendet werden.  
 * **Szenario**: registriert eine BizTalk-Service-Bereitstellung mit einem organisatorischen Konto in Azure Active Directory (wie user@fabrikam.com oder user@contoso.com). In diesem Szenario können nur Azure Active Directory Benutzer innerhalb derselben Organisation BizTalk Service über das Portal für BizTalk-Dienste verwalten. Ein Microsoft-Konto kann nicht verwendet werden.  
* Wenn BizTalk Service im klassischen Azure-Portal erstellen, werden Sie automatisch im BizTalk-Portal registriert.
 * **Szenario**: Erstellen einer BizTalk-Service in der Azure-Verwaltungsportal anmelden, und wählen Sie **Verwalten** zum ersten Mal. Beim Öffnen des BizTalk-Portals wird BizTalk Service automatisch registriert und ist für die Bereitstellung bereit.  
 [Registrieren und Aktualisieren einer Bereitstellung BizTalk-Dienst das BizTalk-Portal](https://msdn.microsoft.com/library/azure/hh689837.aspx)anzeigen  

### <a name="august-14-update"></a>14. August aktualisieren
* Vereinbarung und Bridge Entkopplung – Handel Partner Agreements und Brücken sind jetzt im BizTalk-Portal entkoppelt. Jetzt erstellen Agreements und Brücken getrennt und Laufzeit Brücken sich basierend auf den Werten in der EDI-Nachricht beheben. Siehe [Abkommen in Azure BizTalk-Dienste erstellen](https://msdn.microsoft.com/library/azure/hh689908.aspx), [eine EDI-Brücke BizTalk Services-Portal erstellen](https://msdn.microsoft.com/library/azure/dn793986.aspx), [erstellen eine AS2-Brücke mit BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn793993.aspx)und [wie Brücken beheben Abkommen zur Laufzeit?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Die Option zum Erstellen von Vorlagen für Verträge auslaufen.  
* Für die Vereinbarung Seite senden können Sie jetzt anderes Trennzeichen für jedes Schema angeben. Diese Konfiguration wird unter protokolleinstellungen für senden Seite Vereinbarung angegeben. Weitere Informationen finden Sie unter [Erstellen einer X12 Vereinbarung Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689847.aspx) und [Erstellen Abkommens EDIFACT in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Zwei neue Entitäten werden für denselben Zweck auch TPM OM-API hinzugefügt. Siehe [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) und [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standard XSD-Konstrukte einschließlich abgeleitete Typen werden jetzt unterstützt. Finden Sie [mit standardmäßigen XSD-Konstrukte in Karten](https://msdn.microsoft.com/library/azure/dn793987.aspx) und [Abgeleitete Typen im Zuordnen von Szenarien und Beispiele](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 unterstützt neue MIC Algorithmen zum Signieren von Nachrichten und neuen Verschlüsselungsalgorithmen. Siehe [Erstellen Abkommens AS2 in Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Bekannte Probleme

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Verbindungsprobleme nach BizTalk Portal Update Services

  Haben Sie das BizTalk-Portal zu öffnen, während BizTalk Services aktualisiert wird, ändert sich für den Dienst ausgeführt, möglicherweise Verbindungsprobleme mit der BizTalk-Dienste sind.  
  Als Workaround können den Browser neu starten, Browser-Cache löschen oder das Portal im privaten Modus starten.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio-IDE kann nicht das Artefakt gefunden werden, wenn ein Fehler oder eine Warnung in einem Projekt der BizTalk-Dienste auf
Installieren Sie Visual Studio 2012 Update 3 RC 1 um das Problem zu beheben.  

### <a name="custom-binding-project-reference"></a>Benutzerdefinierte Bindung Projektverweis
Berücksichtigen Sie die folgenden mit BizTalk Services-Projekt in einer Visual Studio-Projektmappe:  
* In der gleichen Visual Studio-Projektmappe ist ein BizTalk Services-Projekt und eine benutzerdefinierte Bindung Projekt. BizTalk Service-Projekt hat einen Verweis auf diese benutzerdefinierte Bindung Projektdatei.
* BizTalk Service-Projekt hat einen Verweis auf eine benutzerdefinierte Bindung/Verhalten-DLL.

"Build" der Projektmappe in Visual Studio erfolgreich. Dann 'Neu erstellen' oder 'Bereinigen' die Lösung. Danach neu erstellen oder bereinigen wieder tritt folgender Fehler auf:  
  Datei konnte nicht kopiert <Path to DLL> , "bin\Debug\FileName.dll". Der Prozess kann nicht die Datei "bin\Debug\FileName.dll" zugreifen, da es von einem anderen Prozess verwendet wird.  

#### <a name="workaround"></a>Abhilfe
* Wenn [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) installiert ist, müssen Sie die folgenden beiden Optionen:

  * Starten Sie Visual Studio oder

  * Starten Sie die Projektmappe neu. Führen Sie dann einen Build der Lösung.  

* Wenn [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) nicht installiert ist, öffnen Sie Task-Manager, klicken Sie auf der Registerkarte Prozesse klicken Sie auf die MSBuild.exe-Prozess und klicken Sie auf die Schaltfläche Prozess beenden.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>BasicHttpRelay Endpunkte Routing wird nicht von Brücken und BizTalk Services-Portal unterstützt, wenn nicht druckbare Zeichen als Header heraufgestuft werden

Verwenden Sie nicht druckbare Zeichen als Teil des höher gestuften Eigenschaften für Nachrichten, können keine Nachrichten an Relay Ziele weitergeleitet, die die BasicHttpRelay-Bindung verwenden. Außerdem stehen höher gestuften Eigenschaften, die als Tracking für Blobs URL codiert und nicht codierte Ziele gehören.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN wird asynchron gesendet, auch wenn die asynchrone MDN Option nicht aktiviert ist  
Szenario dieser – Wenn Sie das Kontrollkästchen **asynchrone MDN senden** , geben Sie einen URL zum asynchronen MDN senden, und deaktivieren Sie das Kontrollkästchen **Senden asynchrone MDN** wieder die MDN wird weiterhin gesendet an die angegebene URL, obwohl asynchrone MDNs senden nicht aktiviert ist.  
Um dieses Problem zu umgehen müssen angegebene URL vor dem Deaktivieren des Kontrollkästchen **Senden asynchrone MDN** löschen und anschließend weiter AS2-Vereinbarung.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Leerzeichen nach einem gültigen Austausch verursachen eine leere Nachricht an den Suspend-Endpunkt  
Sind Leerzeichen über ein IEA-Segment, der Disassembler behandelt dies als Ende des aktuellen Austausch und an der nächsten Leerzeichen als nächste Nachricht sucht. Da dies keine gültige Austausch ist, können Sie beobachten, eine Nachricht erfolgreiche an das Ziel gesendet und eine leere Suspend-Endpunkt Nachricht.  
### <a name="tracking-in-biztalk-services-portal"></a>Im BizTalk-Portal nachverfolgen  
Die Verarbeitung von EDI-Nachrichten und Zusammenhänge werden Überwachungsereignissen erfasst. Schlägt eine Nachricht außerhalb der Bühne Protokoll zeigt Überwachung erfolgreich. Finden Sie in diesem Fall in Abschnitt Spalte **Details** **verfolgen** Fehlerdetails.
Die X12 empfangen und gesendet Settings ([Erstellen einer X12 Vereinbarung Azure BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689847.aspx)) Informationen auf der Bühne Protokoll.  

### <a name="update-agreement"></a>Update-Vereinbarung  
Das BizTalk-Portal können Sie Qualifizierer Identität ändern, wenn eine Vereinbarung konfiguriert ist. Dies führt zu inkonsistenten Eigenschaften. Beispielsweise ist eine Vereinbarung mit ZZ:1234567 und ZZ:7654321 der Qualifizierer. In den Einstellungen BizTalk Services-Portal ändern Sie ZZ:1234567 zu 01:ChangedValue. Öffnen Sie die Vereinbarung und 01:ChangedValue anstelle von ZZ:1234567 angezeigt.
Kennzeichner einer Identität zu ändern, Löschen der Vereinbarung, **Identitäten** im Profil aktualisieren und neu erstellen Abkommens.  
> AZURE. Warnung Dies wirkt sich auf X12 und AS2.  

### <a name="as2-attachments"></a>AS2-Anlagen  
AS2 Nachrichten nicht unterstützt werden Anlagen senden und empfangen. Insbesondere Anlagen werden ignoriert und Nachrichtentext als reguläre AS2-Nachricht verarbeitet.  
### <a name="resources-remembering-path"></a>Ressourcen: Pfad speichern  
Wenn Sie **Ressourcen**hinzufügen, Bedenken im Dialogfeld nicht den Pfad früher eine Ressource hinzufügen. Zum Speichern des zuvor verwendete Pfad versuchen Sie BizTalk Services-Portal-Website für **Vertrauenswürdige Sites** in Internet Explorer.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Benennen Sie die Entität einer Brücke und das Projekt ohne Speichern schließen führt Fehler die Entität erneut öffnen
Ein Szenario in der folgenden Reihenfolge:  
* Brücke (z. B. eine unidirektionale XML-Bridge) zu einem BizTalk Service-Projekt hinzufügen  

* Umbenennen Sie die Brücke durch Angeben eines Werts für die Eigenschaft Name der Entität Dadurch wird die Datei zugeordneten bridgeconfig mit dem angegebenen Namen.  

* Schließen Sie die .bcs-Datei (durch Schließen der Registerkarte in Visual Studio) ohne speichern.  

* Öffnen Sie die Datei .bcs erneut im Projektmappen-Explorer.  
Sie werden bemerken, dass zwar die zugehörige bridgeconfig Datei den neuen Namen angegebene, der Entitätsnamen auf der Entwurfsoberfläche noch den alten Namen. Wenn Sie versuchen, die Brückenkonfiguration durch Doppelklicken auf die Komponente Bridge öffnen, erhalten Sie folgende Fehlermeldung:  
  '<old name>'Entität zugeordnete Datei'<old name>bridgeconfig ' ist nicht vorhanden  
Um dieses Szenario zu vermeiden, stellen Sie sicher, speichern nachdem Sie die Entitäten in einem BizTalk Service-Projekt umbenannt.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk Service-Projekt wird auch wenn ein Element aus einem Visual Studio-Projekt ausgeschlossen wurde erfolgreich erstellt
Betrachten Sie ein Szenario, in dem ein Artefakt (z. B. eine XSD-Datei) hinzufügen, BizTalk Service-Projekt gehören Sie das Artefakt in der Bridge-Konfiguration (z. B. durch als eine Anforderung Nachrichtentyp angeben) und aus der Visual Studio-Projekt ausschließen. In diesem Fall geben beim Erstellen des Projekts nicht Fehler, solange das gelöschte Artefakt auf dem Datenträger an derselben Position aus, in dem es im Visual Studio-Projekt eingeschlossen wurde.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>BizTalk Service-Projekt überprüft nicht Schema Verfügbarkeit beim Konfigurieren der Brücken
In einem Projekt BizTalk Service Wenn ein Schema, das dem Projekt hinzugefügt wird einem anderen Schema importiert überprüft BizTalk Service-Projekt nicht, ob das importierte Schema dem Projekt hinzugefügt. Wenn Sie versuchen, ein Projekt zu erstellen, erhalten Sie keine Buildfehler auftreten.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Die Antwortnachricht für eine XML-Anforderung-Antwort ist immer der Zeichensatz UTF-8
In dieser Version wird der Zeichensatz der Antwort aus einer XML-Anforderung-Antwort-Bridge immer UTF-8 festgelegt.
### <a name="user-defined-datatypes"></a>Benutzerdefinierte Datentypen
Die BizTalk-Adapter Pack Adaptern das Feature BizTalk Adapter können benutzerdefinierte Datentypen für Adapter Operationen nutzen.
Wenn benutzerdefinierte Datentypen verwenden, kopieren Sie die Dateien (.dll) Laufwerk: \Programme\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ oder auf den globalen Assemblycache (GAC) auf dem Server des BizTalk Adapter-Dienstes. Andernfalls kann der folgende Fehler auf dem Client auftreten:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Es wird empfohlen, um GACUtil.exe verwenden, eine Datei im globalen Assemblycache installiert. GACUtil.exe beschreibt, wie Sie dieses Programm und die Visual Studio-Befehlszeilenoptionen verwenden.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Neustart der BizTalk Adapter Service-Website
Installieren der **BizTalk Adapter-Laufzeit** * erstellt die * *BizTalk Adapter Service* * Website in IIS enthält die * *BAService* * Anwendung.* *BAService** Anwendung intern Relay-Bindung verwendet, um lokale Endpunkt in die Cloud erweitern. Für einen Dienst lokal wird der entsprechenden Relay-Endpunkt am Dienstbus registriert nur beim Starten des lokalen Dienstes.  

Beenden und Starten einer Anwendung ist die Konfiguration für das automatische Starten einer Anwendung nicht berücksichtigt. So **BAService** beendet ist, müssen Sie immer **BizTalk Adapter Service** -Website stattdessen starten. Starten Sie oder beenden Sie die Anwendung **BAService** nicht.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Sonderzeichen sollten für Adresse und Entität LOB-Komponenten nicht verwendet werden
Sie sollten nicht für Adresse und Entität LOB Komponenten Sonderzeichen. Wenn Sie dies tun, erhalten Sie Fehler, beim Bereitstellen des BizTalk Service-Projekts. Für bestimmte Zeichen wie "%" die Website BizTalk Adapter möglicherweise ins beendet, und Sie müssen manuell starten.
### <a name="test-map-with-get-context-property"></a>Testen Sie mit Get-Context-Eigenschaft
Enthält eine Transformation eine **Kontexteigenschaft abrufen** Map-Vorgang fehl **Zuordnung testen** . Als vorübergehende Lösung eine Zeichenfolge verketten Zuordnungsvorgang mit unechten Daten ersetzen Sie Zuordnungsvorgang **Kontexteigenschaft zu erhalten** . Das Zielschema füllen wird und zulassen, dass andere Transformation-Funktionalität zu testen.
### <a name="test-map-property-does-not-display"></a>Testeigenschaft Karte wird nicht angezeigt.
**Zuordnung testen** Eigenschaften werden nicht in Visual Studio angezeigt. Dies kann auftreten, wenn **das Eigenschaftenfenster** und das Fenster **Projektmappen-Explorer** nicht gleichzeitig angedockt werden. Um dieses Problem zu beheben, docken Sie **Eigenschaften** und Fenster **Projektmappen-Explorer** .  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Dropdownliste formatieren DateTime ist grau unterlegt.
Wenn eine DateTime formatieren Map-Vorgang zur Entwurfsoberfläche hinzugefügt und konfiguriert, kann der Dropdown-Liste Format abgeblendet. Dies kann auftreten, wenn Computer Anzeige **Mittel – 125 %** oder **größer-150 %**festgelegt ist. Beheben Sie Einstellen der **kleiner – 100 % (Standard)** mit den folgenden Schritten:  
1. Öffnen Sie das **Bedienfeld** , und klicken Sie auf **Darstellung und Anpassung**.
2. Klicken Sie auf **Anzeigen**.
3. Klicken Sie auf **kleiner – 100 % (Standard)** , und klicken Sie auf **Übernehmen**.

Der Dropdown-Liste **Format** sollte nun wie erwartet funktionieren.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Doppelte Abkommen im BizTalk-Portal
Szenario:
1. Erstellen Sie eine Vereinbarung über die Trading Partner-OM-API.
2. Die Vereinbarung im BizTalk-Portal auf zwei verschiedenen Registerkarten geöffnet.
3. Bereitstellen Sie das Abkommen über die Registerkarten.
4. Daher erhalten die Abkommen bereitgestellt sodass doppelte Einträge im Portal BizTalk

**Abhilfe**. Öffnen Sie eines doppelten Abkommen im BizTalk-Portal und zurücknehmen.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Brücken verwenden aktualisierte nicht selbst nach der Aktualisierung ein Zertifikat im Speicher Artefakt
Beachten Sie Folgendes:  

**Szenario 1: Fingerabdruck-Zertifikate bei der Nachrichtenübermittlung von einer Brücke zu einem Dienstendpunkt Sicherung mit**  
Betrachten Sie ein Szenario, in dem Fingerabdruck-Zertifikate im BizTalk Service-Projekt verwenden. Sie aktualisieren das Zertifikat im BizTalk-Portal mit demselben Namen, aber einem anderen Fingerabdruck, aber BizTalk Service-Projekt nicht entsprechend aktualisiert. In diesem Fall kann die Netzwerkbrücke Nachrichten verarbeiten, weil älteren Zertifikatsdaten noch im Cache Kanal eventuell. Schlägt fehl, die Verarbeitung von Nachrichten.  

**Abhilfemaßnahme**: Aktualisieren des Zertifikats im BizTalk Service-Projekt und das Projekt erneut.  

**Szenario 2: Verhalten Namen basierende Zertifikate zum Übertragen von Nachrichten von einer Brücke zu einem Dienstendpunkt sichern zu verwenden**

Szenario: wo Verhalten anhand eines Namens mit Zertifikaten im Projekt BizTalk Service identifiziert. Sie aktualisieren das Zertifikat im BizTalk-Portal aber BizTalk Service-Projekt nicht entsprechend aktualisiert. In diesem Fall kann die Netzwerkbrücke Nachrichten verarbeiten, weil älteren Zertifikatsdaten noch im Cache Kanal eventuell. Schlägt fehl, die Verarbeitung von Nachrichten.  

**Abhilfemaßnahme**: Aktualisieren des Zertifikats im BizTalk Service-Projekt und das Projekt erneut.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Brücken weiterhin Nachrichten zu verarbeiten, auch wenn die SQL-Datenbank offline ist
Brücken BizTalk-Dienste weiterhin Nachrichten eine Weile verarbeiten ist die Microsoft Azure SQL-Datenbank (die die laufenden bereitgestellten Artefakte und Rohrleitungen Informationsspeicher) offline. Ist der BizTalk-Dienste zwischengespeicherten Artefakte und bridgekonfiguration verwendet.
Soll keine Brücken Nachrichten verarbeitet, wenn die SQL-Datenbank offline ist, können Sie die BizTalk-Dienste PowerShell-Cmdlets beenden oder Unterbrechen der BizTalk Service. Finden Sie im [Beispiel für Azure BizTalk Management](http://go.microsoft.com/fwlink/p/?LinkID=329019) Windows PowerShell-Cmdlets Operationen.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Lesen der XML-Nachricht in eine Brücke benutzerdefinierten Codekomponente enthält ein Stückliste Zeichen
Szenario: eine XML-Nachricht in eine Brücke benutzerdefinierten Code gelesen werden soll. Wenn Sie .NET API System.Text.Encoding.UTF8.GetString(bytes) ein Stückliste Zeichen in der Ausgabe am Anfang der Nachricht enthalten ist verwenden. Also, wenn Sie nicht, dass die Ausgabe das Stückliste Zeichen möchten, Sie verwenden ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Senden von Nachrichten mithilfe von WCF-Brücke ist nicht skalierbar.
Nachrichten mit WCF Brücke ist nicht geeignet. Sie sollten stattdessen HttpWebRequest verwenden, wenn einen skalierbaren Client.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>Aktualisierung: Fehler bei Token Anbieter nach dem Aktualisieren von BizTalk Services Vorschau auf allgemeine Verfügbarkeit
Es ist eine EDI- oder AS2-Vereinbarung mit aktiven Batches. Wenn BizTalk Service GA Vorschau aktualisiert wird, kann Folgendes eintreten:
* Fehler: Der Tokenanbieter konnte kein Sicherheitstoken bereitstellen. Token-Provider meldete: der Remotename konnte nicht aufgelöst werden.

* Stapelverarbeitungsaufgaben werden abgebrochen.

**Abhilfe**: nach der BizTalk-Dienst wird aktualisiert, um allgemeine Verfügbarkeit, erneut die Vereinbarung.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>Aktualisierung: Toolbox zeigt die alte Brücke Symbole nach dem Aktualisieren der BizTalk-Dienste SDK
Nach der Aktualisierung einer früheren Version von BizTalk-Dienste SDK die alten Symbole Brücken, weiterhin Toolbox alte Symbole für Brücken. BizTalk Service Project Designeroberfläche Brücke hinzufügen, zeigt die Fläche jedoch das neue Symbol.  

**Abhilfe**. Dieses Problem umgehen, indem .tbd Dateien löschen <system drive>: \Users\<Benutzer > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>Update: BizTalk-Portal Update Vorschau GA möglicherweise zeigen eine Fehlermeldung, dass die EDI-Funktion nicht verfügbar ist
Wenn Sie das BizTalk-Portal angemeldet sind während der BizTalk-Dienste GA Vorschau aktualisiert wird, erhalten Sie die Fehlermeldung im Portal:  

Diese Funktion steht nicht als Teil dieser Edition von Microsoft Azure BizTalk Services. Wechseln zum verwenden diese Funktionen geeignete Auflage.  

**Auflösung**: von der Portal, schließen und öffnen den Browser und melden Sie sich an das Portal abmelden.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>Update: Neue Überwachungsdaten erscheint nicht nach der Aktualisierung der BizTalk-Dienste auf GA
Angenommen Sie ein Szenario, in dem eine XML-Brücke BizTalk Services Vorschau Abonnements bereitgestellt. Nachrichten der Brücke und das entsprechende tracking-Daten auf das BizTalk-Portal ist. Wenn die Laufzeitbestandteile BizTalk Services-Portal und BizTalk-Dienste GA aktualisiert werden und Sie eine an den gleichen Bridge Endpunkt bereits bereitgestellt Nachricht, die Tracking-Daten nicht für Nachrichten, die nach der Aktualisierung erscheint.  

### <a name="pipelines-vs-bridges"></a>Pipelines V/s Brücken
In diesem Dokument sind den Begriff "Pipelines" und "Brücken" austauschbar. Beide im Wesentlichen Bedeutung dieselbe, die eine Message Processing Unit auf BizTalk-Dienste bereitgestellt.  

### <a name="concepts"></a>Konzepte  

[BizTalk-Dienste](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

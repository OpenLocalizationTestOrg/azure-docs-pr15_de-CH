<properties
   pageTitle="Fehlerbehandlung Azure Automatisierung | Microsoft Azure"
   description="Dieser Artikel enthält grundlegende Fehler Behandlung Schritte zur Problembehandlung häufig Azure Automation-Fehler beheben."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="Automatisierungsfehler Fehlerbehandlung"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Fehlerbehandlung Tipps für allgemeine Azure Automation-Fehler

Dieser Artikel beschreibt einige allgemeine Azure Automation-Fehler auftreten und schlägt mögliche Maßnahmen für die Fehlerbehandlung.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Beim Arbeiten mit Azure Automation Runbooks Authentifizierung Fehlerbehebung  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Szenario: Melden Sie sich bei Azure-Konto ist fehlgeschlagen

**Fehler:** Die Fehlermeldung "Unknown_user_type: Unbekannte Benutzertyp" beim Arbeiten mit AzureAccount hinzufügen oder Login-AzureRmAccount-Cmdlets.

**Grund für den Fehler:** Dieser Fehler tritt auf, wenn Anmeldeinformationen Ressourcenname ungültig ist oder wenn Benutzername und Kennwort, mit dem Sie tung der Automatisierung Anmeldeinformationen, ungültig sind.

**Tipps:** Um festzustellen, was falsch ist, gehen Sie folgendermaßen vor:  

1. Sicherstellen, dass Ihnen keine Sonderzeichen, einschließlich der **@** Zeichen in den Automatisierung Anlage Anmeldenamen, die Sie mit Azure herstellen.  

2. Überprüfen Sie Benutzername und Kennwort, die in Azure Automation Anmeldeinformationen im lokalen PowerShell ISE Editor verwenden können. Dazu können Sie die folgenden Cmdlets in der PowerShell ISE ausführen:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Schlägt die Authentifizierung lokal, bedeutet dies, dass Ihre Anmeldeinformationen Azure Active Directory ordnungsgemäß eingerichtet haben. Siehe [Tauglichkeit in Azure mithilfe von Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) Blogbeitrag zu Azure Active Directory-Konto korrekt einrichten.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Szenario: Nicht Azure-Abonnement gefunden

**Fehler:** Die Fehlermeldung "das Abonnement mit dem Namen ``<subscription name>`` wurde nicht gefunden" beim Arbeiten mit Cmdlets wählen Sie AzureSubscription oder AzureRmSubscription auswählen.

**Grund für den Fehler:** Dieser Fehler tritt auf, wenn der Namen nicht gültig ist oder Azure Active Directory-Benutzer versucht, die Abonnementdetails nicht als Administrator des Abonnements konfiguriert ist.

**Tipps:** Um festzustellen, ob Azure ordnungsgemäß authentifiziert und haben Zugriff auf das Abonnement, das Sie auswählen möchten, gehen Sie folgendermaßen vor:  

1. Stellen Sie sicher, dass Sie **Hinzufügen AzureAccount** ausführen, bevor das **Select-AzureSubscription** -Cmdlet ausführen.  

2. Wenn diese Fehlermeldung weiterhin angezeigt wird, ändern Sie den Code nach dem **Add-AzureAccount** -Cmdlet **Get-AzureSubscription** -Cmdlet hinzufügen und dann führen Sie den Code aus.  Nun enthält die Ausgabe von Get-AzureSubscription Ihre Abonnementdetails überprüfen.  
    * Wenn alle Abonnementdetails in der Ausgabe nicht angezeigt wird, bedeutet dies, dass das Abonnement noch initialisiert ist nicht.  
    * Wenn Sie die Abonnementdetails in der Ausgabe angezeigt werden, bestätigen Sie, dass Sie den richtigen Namen oder die ID mit dem **Select-AzureSubscription** -Cmdlet verwenden.   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Szenario: Azure Authentifizierung fehlgeschlagen Multifaktor-Authentifizierung aktiviert ist

**Fehler:** Die Fehlermeldung "Add-AzureAccount: AADSTS50079: starke Authentifizierung Registrierung (Nachweis oben) muss" bei Azure mit Azure Benutzername und Kennwort.

**Grund für den Fehler:** Haben Sie mehrstufige Authentifizierung Ihres Kontos Azure können Azure Active Directory-Benutzer Sie in Azure authentifizieren.  Stattdessen müssen Sie ein Zertifikat oder einen Dienstprinzipal verwenden in Azure.

**Tipps:** Um ein Zertifikat mit Azure Service Management-Cmdlets verwenden, finden Sie in [Erstellen und Hinzufügen eines Zertifikats zum Verwalten von Azure Diensten.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Einen Dienstprinzipal Azure-Ressourcen-Manager-Cmdlets finden Sie unter [Service Principal mit Azure-Portal](./resource-group-create-service-principal-portal.md) erstellen und [Authentifizierung Dienstprinzipalnamen Azure Ressourcenmanager.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Problembehandlung bei Fehlermeldungen beim Arbeiten mit runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Szenario:-Runbook scheitert das deserialisierte Objekt

**Fehler:** Ihr Runbook tritt der Fehler "kann keine Parameter binden ``<ParameterName>``. Nicht konvertieren die ``<ParameterType>`` Wert Deserialized ``<ParameterType>`` geben ``<ParameterType>``".

**Grund für den Fehler:** Ist Ihr Runbook PowerShell Workflow speichert komplexe Objekte in einem deserialisierten Format um Ihre Runbook Zustand beibehalten, wenn der Workflow ausgesetzt ist.  

**Problembehandlung:**  
Die folgenden drei Lösungen wird dieses Problem behoben:

1. Wenn Sie komplexe Objekte von einem Cmdlet Rohrleitungen, schließen Sie diese Cmdlets in ein InlineScript.  
2. Übergeben Sie den Namen oder Wert müssen aus das komplexe Objekt anstatt das gesamte Objekt.  

3. Verwenden Sie PowerShell Runbook statt ein Runbook PowerShell Workflow.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Szenario: Runbook Auftrag fehlgeschlagen, da das zugewiesene Kontingent überschritten

**Fehler:** Die Runbook Auftragsfehler mit dem Fehler "das Kontingent für die monatliche Summe Auftrag Laufzeit für dieses Abonnement erreicht wurde".

**Grund für den Fehler:** Dieser Fehler tritt auf, wenn die Ausführung des Auftrags 500 Minute freie Kontingent für Ihr Konto überschritten. Dieses Kontingent gilt für alle Projektaufgaben Testen einen Auftrag Starten eines Auftrags aus dem Portal, Ausführen eines Auftrags mit Webhooks und Planung eines Auftrags mit dem Azure Portal oder im Rechenzentrum Ausführung. Finden Sie mehr über Preise für Automatisierung [Automatisierung Preise](https://azure.microsoft.com/pricing/details/automation/).

**Tipps:** Möchten Sie mehr als 500 Minuten pro Monat Verarbeitung müssen das kostenlose Abonnements ändern Ebene, die grundlegende Stufe. Auf die grundlegende Ebene aktualisieren folgende Schritte aus:  

1. Azure-Abonnement anmelden  
2. Wählen Sie das Konto Automatisierung zu aktualisieren  
3. Klicken Sie **auf** > **Preisstufe und Verwendung** > **Preisstufe**  
4. **Auf dem Blatt **Auswählen der Tarif** auswählen**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Szenario: Cmdlet nicht erkannt, wenn ein Runbook ausführen

**Fehler:** Beruf Runbook tritt der Fehler "``<cmdlet name>``: der Begriff ``<cmdlet name>`` wird nicht als Name eines Cmdlet, Funktion, Skriptdatei oder entweder erkannt."

**Grund für den Fehler:** Dieser Fehler wird verursacht, des PowerShell-Moduls das Cmdlet findet in Ihrem Runbook verwenden.  Möglicherweise fehlt das Modul, das Cmdlet enthält das Konto, besteht ein Namenskonflikt mit einem Namen Runbook oder das Cmdlet ist auch in einem anderen Modul und Automatisierung der Name kann nicht aufgelöst werden.

**Tipps:** Alle der folgenden Schritte beheben das Problem:  

- Überprüfen Sie, dass Sie den Cmdlet-Namen korrekt eingegeben haben.  

- Stellen Sie sicher das Cmdlet in Automation-Konto vorhanden ist und dass keine Konflikte vorhanden sind. Überprüfen, ob das Cmdlet vorhanden ist, öffnen Sie ein Runbook im Bearbeitungsmodus und suchen für das Cmdlet in der Bibliothek oder ausführen soll **Get-Command ``<CommandName>`` **.  Sie haben überprüft das Cmdlet für das Konto, und keine Namenskonflikte mit anderen Cmdlets oder Runbooks, der Leinwand hinzugefügt und sicher, dass Sie gültigen Parameter in Ihrem Runbook festgelegt.  

- Wenn Sie einen Konflikt haben und das Cmdlet in zwei verschiedenen Modulen verfügbar ist, können Sie dies mit dem vollqualifizierten Namen für das Cmdlet beheben. Beispielsweise können Sie **ModuleName\CmdletName**.  

- Wenn Sie in eine hybride Arbeitskraftgruppe Runbook lokalen ausführen, stellen Sie sicher, dass das Modul-Cmdlet auf dem Computer installiert ist, die die Arbeitskraft Hybrid hostet.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Szenario: Ein langer Runbook häufiger mit Ausnahme: "der Auftrag kann nicht weiter ausgeführt, da wiederholt aus denselben Prüfpunkt entfernt wurde".

**Grund für den Fehler:** Dieser wird Entwurf Verhalten "Anteil" Überwachen von Prozessen in Azure-Automatisierung, die automatisch ein Runbook unterbricht, wenn mehr als 3 Stunden ausgeführt. Die Fehlermeldung wird jedoch keinen "Nächste" Optionen. Ein Runbook kann eine Reihe von Gründen ausgesetzt werden. Passieren hauptsächlich aufgrund von Fehlern angehalten. Z. B. eine nicht abgefangene Ausnahme in ein Runbook, ein Netzwerkfehler oder ein Absturz Runbook Worker Runbooks mit alle verursacht Runbook ausgesetzt und vom letzten Prüfpunkt Wenn fortgesetzt.

**Tipps:** Die dokumentierte Lösung dieses Problems ist zur Verwendung von Prüfpunkten in einem Workflow.  Weitere Informationen finden Sie weitere [Learning PowerShell](automation-powershell-workflow.md#Checkpoints)Workflows.  Eine ausführlichere Erläuterung "Anteil" und Checkpoint finden in diesem Blogartikel [Prüfpunkte in Runbooks verwenden](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Problembehandlung bei Fehlermeldungen beim Module importieren

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Szenario: Modul nicht importieren oder Cmdlets nicht nach dem Import ausgeführt werden

**Fehler:** Ein Modul importieren fehlschlägt oder erfolgreich importiert, aber keine Cmdlets extrahiert.

**Grund für den Fehler:** Gründe, die ein Modul in Azure Automation nicht erfolgreich importieren kann sind:  

- Die Struktur entspricht, die Automatisierung in der Struktur nicht.  

- Das Modul ist von einem anderen Modul nicht auf Ihr Konto Automatisierung bereitgestellt wurde.  

- Das Modul fehlt Abhängigkeit im Ordner.  

- **Neu-AzureRmAutomationModule** -Cmdlet wird verwendet, um das Modul hochladen und vollständige Speicherpfad ist nicht angegeben oder das Modul eine öffentlich zugängliche URL nicht geladen.  

**Problembehandlung:**  
Alle der folgenden Schritte beheben das Problem:  

- Stellen Sie sicher, dass das Modul das folgende Format verwendet:  
ModuleName.Zip **->** ModuleName oder Versionsnummer **->** (ModuleName.psm1, ModuleName.psd1)

- Öffnen Sie die Datei psd1 und sehen Sie, ob das Modul abhängig ist.  Andernfalls laden Sie diese Module Automation-Konto.  

- Stellen Sie sicher, dass alle referenzierten DLLs werden im Modul-Ordner.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Problembehandlung bei Fehlermeldungen beim Arbeiten mit dem gewünschten Zustand Konfiguration (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Szenario: Knoten ist in einem Fehlerstatus mit dem Fehler "Nicht gefunden"

**Fehler:** Der Knoten hat einen Bericht mit Status **fehlgeschlagen** mit dem Fehler "der Versuch, die Aktion vom Server https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction ist fehlgeschlagen, da eine gültige Konfiguration ``<guid>`` kann nicht gefunden werden."

**Grund für den Fehler:** Dieser Fehler tritt normalerweise einen Namen (z. B. ABC) der Knoten zugewiesen statt einen Namen für die Knoten (z.B. ABC. WebServer).  

**Problembehandlung:**  

- Stellen Sie sicher, dass Sie den Knoten "Knotenname Konfiguration" und nicht "Configuration Name" zuweisen.  

- Sie können eine Knotenkonfiguration eines Knotens mit Azure-Portal oder mit PowerShell-Cmdlet zuweisen.
    - Öffnen Sie das Blade **DSC-Knoten** um Knoten zu einem Knoten mit Azure-Portal zuweisen, wählen Sie einen Knoten aus und klicken Sie auf **Knotenkonfiguration zuweisen** .  
    - Verwenden Sie, um Knoten zu einem Knoten mit PowerShell-Cmdlet zuweisen, **Set-AzureRmAutomationDscNode** -cmdlet


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Szenario: Keine Knoten Konfigurationen (MOF-Dateien) wurden erstellt, wenn eine Konfiguration kompiliert wird

**Fehler:** DSC-Kompilierungsauftrag hält mit Fehler: "Kompilierung erfolgreich abgeschlossen, aber keine Knoten Konfiguration .mofs generiert wurden".

**Grund für den Fehler:** Wenn Ausdruck folgenden, die Schlüsselworts **Knoten** DSC-Konfiguration ergibt, $null dann keine Knoten Konfigurationen erzeugt werden.    

**Problembehandlung:**  
Alle der folgenden Schritte beheben das Problem:  

- Stellen Sie sicher, dass der Ausdruck neben das Schlüsselwort **Knoten** in die Konfigurationsdefinition nicht auf $null auswertet.  
- Wenn Sie beim Kompilieren der Konfigurations ConfigurationData übergeben, stellen Sie sicher, dass Sie die erwarteten Werte, die die Konfiguration erfordert von [ConfigurationData](automation-dsc-compile.md#configurationdata)übergeben werden.


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Szenario: DSC Knoten Bericht wird Zustand "in Bearbeitung" fest.

**Fehler:** DSC-Mitarbeiter gibt "Keine Instanz mit angegebenen Eigenschaftswerten gefunden."

**Grund für den Fehler:** Sie die WMF-Version aktualisiert und WMI beschädigt haben.  

**Tipps:** Führen Sie die Schritte im Blogbeitrag [DSC bekannte Probleme und Grenzen](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) , das Problem zu beheben.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Szenario: Anmeldeinformationen DSC-Konfiguration verwenden

**Fehler:** DSC-Kompilierungsauftrag wurde mit Fehler angehalten: "System.InvalidOperationException Fehler Eigenschaft"Anmeldeinformationen"vom Typ ``<some resource name>``: konvertieren und speichern ein verschlüsseltes Kennwort als Klartext nur zulässig ist, wenn PSDscAllowPlainTextPassword festgelegt auf True".

**Grund für den Fehler:** Sie haben Anmeldeinformationen in einer Konfiguration verwendet aber nicht ordnungsgemäße **ConfigurationData** um **PSDscAllowPlainTextPassword** für jeden Knotenkonfiguration auf True festgelegt.  

**Problembehandlung:**  
- Sollten Sie übergeben der richtigen **ConfigurationData** **PSDscAllowPlainTextPassword** für jeden Knotenkonfiguration gemäß der Konfiguration auf True festgelegt. Weitere Informationen finden Sie [in Azure Automation DSC Anlagen](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Nächste Schritte

Wenn die Problembehandlung Schritte befolgt haben und weitere zu diesem Artikel Hilfe können Sie:

- Hilfe von Azure-Experten. Senden Sie Ihre Anfrage an das [MSDN Azure oder Stapelüberlauf Foren.](https://azure.microsoft.com/support/forums/)

- Datei Azure Supportfall. Der [Azure-Support-Website](https://azure.microsoft.com/support/options/) und auf **Unterstützung** unter **technischen Support und Abrechnungssupport**.

- Buchen Sie Skript anfordern [Script](https://azure.microsoft.com/documentation/scripts/) Center, wenn Sie alternative Runbook Azure Automation oder ein Integrationsmodul suchen.

- Buchen Sie Feedback oder Feature Anfragen für Azure Automatisierung auf [Benutzer](https://feedback.azure.com/forums/34192--general-feedback).

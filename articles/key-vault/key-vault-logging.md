<properties
    pageTitle="Azure-Tresor anmelden | Microsoft Azure"
    description="Mit diesem Lernprogramm können Sie Einstieg in Azure Key Vault protokollieren."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Azure-Tresor Protokollierung #
Azure Key Vault ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter [Key Vault Preisseite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung  
Nach der Erstellung eine oder mehrere wichtige Depots wahrscheinlich möchten überwachen wie und wann Ihre Schlüssel zugegriffen, und von wem sind. Hierzu können Sie Aktivieren der Protokollierung für Schlüssel Depot in Azure Speicherkonto Informationen gespeichert, die Sie bereitstellen. Ein neuer Container mit dem Namen **Insights-Protokolle-AuditEvent** ist für das angegebene Speicherkonto automatisch erstellt und können diese gleichen Speicherkonto Erfassung von Protokollen für mehrere Schlüssel Depots.

Sie können die Protokollinformationen höchstens, 10 Minuten nach dem Schlüssel Depot Vorgang. In den meisten Fällen werden dadurch schneller.  Es ist Ihre Protokolle in das Speicherkonto verwalten:

- Verwenden Sie standard Azure Zugriffssteuerungsmethoden Sichern der Protokolle einschränken, wer auf sie zugreifen können.
- Löschen Sie Protokolle, die nicht mehr in das Speicherkonto behalten möchten.

Dieses Lernprogramm für erste Schritte mit Azure Schlüssel Protokollierung zum Erstellen Ihres Speicherkontos Protokollierung aktivieren und die Protokollierung Informationen gesammelt.  


>[AZURE.NOTE]  Dieses Lernprogramm enthält keine Hinweise zum wichtigsten Depots, Schlüssel oder Kennwörter zu erstellen. Diese Informationen finden Sie unter [Erste Schritte mit Azure Schlüssel](key-vault-get-started.md). Oder Hinweise plattformübergreifende Befehlszeilenschnittstelle [Lernprogramms entspricht](key-vault-manage-with-cli.md).
>
>Sie können nicht derzeit Azure Key Vault in Azure-Portal konfigurieren. Verwenden Sie diese Anleitung Azure PowerShell.

Die Protokolle, die Sie sammeln visualisiert mit Protokollanalyse Operations Management Suite. Weitere Informationen finden Sie unter [Azure Key Vault (Vorschau) Lösung Protokollanalyse](../log-analytics/log-analytics-azure-key-vault.md).

Übersicht über Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

- Einem bereits existierenden +++ Schlüssel, die Sie verwendet haben.  
- Azure PowerShell **mindestens Version 1.0.1**. Azure PowerShell installieren und Ihr Azure-Abonnement zuordnen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Wenn Sie Azure PowerShell bereits installiert und die Version von Azure PowerShell-Konsole nicht kennen, geben `(Get-Module azure -ListAvailable).Version`.  
- Ausreichend Speicherplatz auf Azure für Ihre Schlüssel Vault-Protokolle.


## <a id="connect"></a>Verbindung zu Ihren Abonnements ##

Starten Sie eine Azure PowerShell-Sitzung und melden Sie an, um Ihre Azure-Konto mit dem folgenden Befehl:  

    Login-AzureRmAccount

Geben Sie im Popupmenü Browserfenster Ihre Azure Benutzernamen und Kennwort. Azure PowerShell erhalten alle Abonnements, die mit diesem Konto und standardmäßig verwendet die erste.

Haben Sie mehrere Abonnements möglicherweise eine bestimmte angeben, die mit Ihrem Tresor Azure Schlüssel erstellt wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto finden Sie unter:

    Get-AzureRmSubscription

Um das Abonnement angeben, das mit Ihrem Schlüssel zugeordnet sind, die Sie anmelden, geben Sie:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Weitere Informationen zur Konfiguration von Azure PowerShell Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).


## <a id="storage"></a>Erstellen Sie ein neues Speicherkonto für die Protokolle ##

Obwohl Sie ein vorhandenes Speicherkonto für die Protokolle verwenden können, erstellen wir ein neues Speicherkonto, das gewidmet Key Vault-Protokolle. Zur Vereinfachung für Wenn wir dies später angeben, speichern wir die Details in einer Variablen namens **sa**.

Zur weiteren Vereinfachung des Managements auch verwenden derselben Ressourcengruppe wie wir, die unsere wichtigsten Depot enthält. [Einführung](key-vault-get-started.md)Ressourcengruppe heißt **ContosoResourceGroup** und werden wir Ostasien Speicherort verwenden. Ersetzen Sie diese Werte für Ihren eigenen ggf..

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, verwenden sie dieselbe Abonnement als Schlüssel Depot und Verwenden der Ressourcen-Manager-Bereitstellungsmodell statt Bereitstellung klassisch.

## <a id="identify"></a>Schlüssel-Depot für die Protokolle identifizieren ##

Unsere immer gestartet Lernprogramm wurde unsere wichtigsten Depotnamen ein **ContosoKeyVault**, damit wir diesen Namen und die Details in einer Variablen namens **kv**weiterhin:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Aktivieren der Protokollierung ##

Zum Aktivieren der Protokollierung für Schlüssel Depot verwenden wir das Cmdlet Set-AzureRmDiagnosticSetting zusammen mit den Variablen für unsere neue Speicherkonto und unsere wichtigsten Vault erstellten. Wir stellen auch die **-aktiviert** flag auf **$true** und Kategorie auf AuditEvent (die einzige Kategorie für Key Vault-Protokollierung).:


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Diese Ausgabe enthält:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Dies bestätigt, dass Protokollierung für den Schlüssel Tresor Speichern von Informationen in das Speicherkonto jetzt aktiviert ist.

Optional können Sie auch Aufbewahrungsrichtlinie für die Protokolle festlegen, sodass ältere Protokolle gelöscht. Beispielsweise Aufbewahrungsrichtlinie mit **RetentionEnabled -** Flag auf **$true** festgelegt und **RetentionInDays -** Parameter auf **90** festgelegt, sodass Protokolle, die älter als 90 Tage automatisch gelöscht werden.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Protokollierte Ereignisse:

- Alle authentifizierte REST API-Anfragen werden protokolliert der fehlgeschlagene Anfragen Zugriffsberechtigungen, Fehler oder fehlerhafte Anforderungen enthält.
- Operationen auf das Depot selbst, einschließlich erstellen, löschen, Key Vault-Richtlinien festlegen, und Aktualisierung Schlüssel Depot Attribute-tags.
- Vorgänge für Schlüssel und geheime Schlüssel im Schlüssel Tresor, einschließlich erstellen, ändern oder löschen diesen Schlüssel oder Kennwörter; Operationen wie, überprüfen verschlüsseln Sie, Entschlüsseln umschließen Sie und Entpacken Sie Schlüssel, erhalten Sie Geheimnisse, Liste Schlüssel und geheime Schlüssel und ihre Versionen zu.
- Nicht authentifizierte Anfragen, die eine 401-Antwort führen. Beispielsweise fordert, die keinen trägertoken oder ungültigen oder abgelaufenen oder haben ein ungültiges Token.  


## <a id="access"></a>Zugriff auf die Protokolle ##

Key Vault Protokolle befinden sich in **Insights-Protokolle-AuditEvent** -Container im Speicherkonto eingegebenen. Um alle Blobs in diesem Container aufzulisten, geben Sie Folgendes ein:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Die Ausgabe sieht in etwa wie folgt:

**Container-Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Name**

**----**

**ResourceId = ABONNEMENTS/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/Anbieter/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**ResourceId = ABONNEMENTS/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/Anbieter/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** ResourceId = ABONNEMENTS/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/Anbieter/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Wie diese Ausgabe sehen, die Blobs Namenskonvention: **ResourceId =<ARM resource ID>y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Die Werte für Datum und Uhrzeit verwenden UTC.

Da das gleiche Speicherkonto sammeln Protokolle für mehrere Ressourcen verwendet werden kann, ist die vollständige Ressourcen-ID in den BLOB-Namen zugreifen oder nur die Blobs, die Sie herunterladen sehr nützlich. Aber bevor wir dies tun, wir werden zunächst alle Blobs herunterladen.

Erstellen Sie zunächst einen Ordner zum Herunterladen der Blobs. Zum Beispiel:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Dann erhalten Sie eine Übersicht über alle Blobs:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Leiten Sie diese Liste durch 'Get-AzureStorageBlobContent"die Blobs in unserer Zielordner herunterladen:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Beim Ausführen dieser zweiten Befehl die **/** Trennzeichen im BLOB-Namen eine vollständige Ordnerstruktur unter dem Zielordner erstellen und diese Struktur herunterladen und Speichern von Blobs Dateien verwendet.

Blobs selektiv herunterladen verwenden Sie Platzhalter. Zum Beispiel:

- Wenn Sie mehrere Schlüssel Depots und Protokolle für nur einen Schlüssel Depot herunterladen möchten, mit dem Namen CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Haben Sie mehrere Ressourcengruppen und Protokolle für nur eine Ressourcengruppe herunterladen möchten, verwenden Sie `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Alle Protokolle für den Monat Januar 2016 herunterladen, verwenden Sie `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Sie nun können beginnen, was in den Protokollen ist. Aber bevor es weitergeht, zwei weitere Parameter für Get-AzureRmDiagnosticSetting, die Sie kennen müssen:

- Den Status der Diagnose Einstellungen für die wichtigsten Tresor Ressource Abfragen:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Protokollierung für die wichtigsten Vault-Ressource zu deaktivieren:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Interpretieren der Schlüssel Vault-Protokolle ##

Einzelne Blobs werden als Text, als JSON Blob gespeichert. Dies ist eine Beispiel-Protokolleintrag ausgeführt `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Die folgende Tabelle enthält die Feldnamen und.


| Feldname        | Beschreibung |
| ------------- |-------------|
| Zeit      | Datum und Uhrzeit (UTC).|
| resourceId      | Azure Ressourcenmanager Ressourcen-ID. Key Vault-Protokolle ist dies immer Schlüssel Depot Ressourcen-ID.|
| operationName      | Name des Vorgangs, wie in der folgenden Tabelle beschrieben.|
| operationVersion      | Dies ist die REST API-Version vom Client angefordert.|
| Kategorie      | Key Vault-Protokolle ist AuditEvent der verfügbaren Wert.|
| resultType      | Ergebnis REST-API.|
| resultSignature      | HTTP-Status.|
| resultDescription     | Zusätzliche Beschreibung über das Ergebnis, wenn verfügbar.|
| durationMs      | Zeit, die auf die REST-API-Anforderung in Millisekunden. Dies schließt nicht die Netzwerklatenz, damit die Zeit messen Sie auf dem Client diesmal nicht übereinstimmt.|
| callerIpAddress      | IP-Adresse des Clients, der Sie gestellt hat.|
| correlationId      | Eine optionale GUID, die der Client zum Korrelieren von clientseitigen Protokolle mit dienstseitigen (Schlüssel Depot) übergeben werden können.|
| Identität      | Identität aus dem Token vorgelegt wurde, wenn die REST-API-Anforderung. Dies ist normalerweise "User", "Service Principal" oder eine Kombination "Benutzer + AppId" wie eine Anforderung Azure PowerShell-Cmdlets.|
| Eigenschaften      | Dieses Feld enthält verschiedene Informationen über den Vorgang (OperationName). In den meisten Fällen enthält Informationen (die Useragent-Zeichenfolge vom Client übergeben), genaue REST-API-Anforderung URI und HTTP-Statuscode. Außerdem ein Objekt bei Anforderung (z. B. KeyCreate oder VaultGet) enthält es auch Schlüssel URI (als "Id"), Vault-URI oder Schlüssel-URI.|




**OperationName** Feldwerte sind im ObjectVerb-Format. Zum Beispiel:

- Alle Schlüssel Depot Operationen der "Vault`<action>`" wie `VaultGet` und `VaultCreate`.

- Alle wichtigen Vorgänge den "Schlüssel`<action>`" wie `KeySign` und `KeyList`.

- Alle geheimen Operationen der "geheimen`<action>`" wie `SecretGet` und `SecretListVersions`.

Die folgende Tabelle listet die OperationName und entsprechende REST API-Befehl.

| operationName        | REST API-Befehl |
| ------------- |-------------|
| Authentifizierung      | Über Active Directory Azure Endpunkt|
| VaultGet      | [Informationen Sie über wichtige vault](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Erstellt oder aktualisiert ein Schlüssel Depot](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Ein Schlüssel Depot löschen](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Ein Schlüssel Depot aktualisieren](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Liste aller wichtigen Depots in einer Ressourcengruppe](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Erstellen Sie einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Informationen Sie über einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Importieren eines Schlüssels in einem Tresor](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Sicherung eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Löschen eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Wiederherstellen eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Melden Sie sich mit einem Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Mit einem Schlüssel überprüfen](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Umschließen eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Entpacken Sie einen Schlüssel](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Mit einem Schlüssel verschlüsselt](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Mit einem Schlüssel entschlüsseln](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Aktualisieren eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Listen Sie die Schlüssel in einem Tresor](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Liste der Versionen eines Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Erstellen eines geheimen Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Passwort abrufen](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Aktualisieren eines geheimen Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Löschen eines geheimen Schlüssels](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Liste Geheimnisse in einem Tresor](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Versionen eines Geheimnisses](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Nächste Schritte ##

Ein, die eine Anwendung Azure Key Vault verwendet, finden Sie unter [Verwenden Azure Key Vault von einer Web-Anwendung](key-vault-use-from-web-application.md).

Programmierung Verweise finden Sie unter [Azure Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).

Finden Sie eine Liste der Cmdlets für Azure Key Vault Azure PowerShell 1.0 [Azure Key Vault Cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Ein Tutorial zur Schlüsselrotation und Überprüfen der Protokolle mit Azure Schlüssel finden Sie unter [Setup Key Vault Ende Schlüsselrotation und Überwachung](key-vault-key-rotation-log-monitoring.md).

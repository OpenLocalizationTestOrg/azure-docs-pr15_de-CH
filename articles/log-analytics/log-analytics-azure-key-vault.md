<properties
    pageTitle="Azure Key Vault Lösung Protokollanalyse | Microsoft Azure"
    description="Azure Key Vault-Lösung können Protokollanalyse Azure Key Vault-Protokolle überprüfen."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure Key Vault (Vorschau) Lösung Protokollanalyse

>[AZURE.NOTE] Dies ist eine [Vorschau Lösung](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Azure Key Vault-Lösung können Protokollanalyse Azure Key Vault AuditEvent-Protokolle überprüfen.

Protokollieren von Überwachungsereignissen Azure Key Vault können. Diese Protokolle werden in Azure BLOB-Speicher geschrieben, wo sie dann von Protokollanalyse suchen und Analyse indiziert werden können.

## <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung

Gehen Sie folgendermaßen vor, installieren und Konfigurieren der Azure Key Vault-Lösung:

1.  Aktivieren Sie [Diagnoseprotokoll für Schlüssel Depot](../key-vault/key-vault-logging.md) Ressourcen überwachen
2.  Konfigurieren Sie Protokollanalyse Protokolle mithilfe der Schritte in [JSON-Dateien im BLOB-Speicher](../log-analytics/log-analytics-azure-storage-json.md)aus dem BLOB-Speicher zu lesen.
3.  Aktivieren Sie Azure Key Vault-Lösung mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  

## <a name="review-azure-key-vault-data-collection-details"></a>Einzelheiten zur Datensammlung Azure Key Vault überprüfen

Azure Key Vault-Lösung sammelt Diagnoseprotokolle Azure Key Vault von Azure BLOB-Speicher.
Es ist kein Agent für die Datensammlung.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten Azure Key Vault.

| Plattform | Direkte agent | System Center Operations Manager (SCOM) agent | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Azure|![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ja](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 Minuten|

## <a name="use-azure-key-vault"></a>Verwenden von Azure-Tresor

Nach der Installation der Lösung können Sie die Zusammenfassung des Status mit der Zeit für die überwachten Schlüssel Depots mit **Azure Key Vault** anordnen Wunsch auf **der Übersichtsseite Protokollanalyse** anzeigen.

![Bild von Azure Key Vault-Kachel](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Nach die Kachel **Übersicht anklicken** können Übersichten der Protokolle anzeigen und dann einen Drilldown ausführen für folgende Details:

- Volume alle Schlüssel Vault-Operationen mit der Zeit
- Fehler bei Vorgang Volumes mit der Zeit
- Durchschnittliche Wartezeit für betriebliche Vorgang
- Quality of Service für Vorgänge die Anzahl der Vorgänge, die mehr als 1000 ms und eine Liste der Vorgänge, die mehr als 1000 ms

![Bild von Azure Key Vault-dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Bild von Azure Key Vault-dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Details für jeden Vorgang anzeigen

1. Klicken Sie auf der Seite **Übersicht** **Azure Key Vault** .
2. Das Dashboard **Azure Key Vault** lesen Sie die Zusammenfassung in einem Blade und dann auf eine detaillierte Informationen auf der Suchseite Protokoll anzeigen.

    Auf Suchseiten Protokoll Ergebnisse Zeit detaillierte und Suchverlauf Protokoll angezeigt. Sie können auch Filtern Facetten um die Ergebnisse einzuschränken.

## <a name="log-analytics-records"></a>Analytics-Datensätze

Azure Key Vault-Lösung analysiert Datensätze ein **KeyVaults** , die vom [AuditEvent-Protokolle](../key-vault/key-vault-logging.md) in Azure-Diagnose erfasst werden.  Eigenschaften für diese Datensätze sind in der folgenden Tabelle.  

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | IP-Adresse des Clients, der Anforderung |
| Kategorie      | Key Vault-Protokolle ist AuditEvent der verfügbaren Wert.|
| CorrelationId | Eine optionale GUID, die der Client zum Korrelieren von clientseitigen Protokolle mit dienstseitigen (Schlüssel Depot) übergeben werden können. |
| DurationMs | Zeit, die auf die REST-API-Anforderung in Millisekunden. Dies schließt nicht Netzwerklatenz, Zeit, die auf der Clientseite messen diesmal nicht übereinstimmt. |
| HttpStatusCode_d | Von der Anforderung zurückgegebenen HTTP-Statuscode |
| Id_s       | Eindeutige ID der Anforderung |
| Identity_o | Identität aus dem Token vorgelegt wurde, wenn die REST-API-Anforderung. Dies ist normalerweise "User", "Service Principal" oder eine Kombination "Benutzer + AppId" wie eine Anforderung Azure PowerShell-Cmdlets. |
| OperationName      | Name des Vorgangs, wie [Azure Key Vault](../key-vault/key-vault-logging.md) -Protokollierung|
| OperationVersion      | REST-API-Version vom Client angeforderten|
| RemoteIPLatitude | Breite des Clients, der Anforderung |
| RemoteIPLongitude | Länge des Clients, der Anforderung |
| RemoteIPCountry | Land des Clients, der Anforderung  |
| RequestUri_s | URI der Anforderung |
| Ressource   | Name des schlüsseltresors |
| ResourceGroup | Ressourcengruppe Key vault |
| ResourceId | Azure Ressourcenmanager Ressourcen-ID. Key Vault-Protokolle ist dies immer Schlüssel Depot Ressourcen-ID. |
| ResourceProvider | *MICROSOFT. SCHLÜSSELTRESOR* |
| ResultSignature  | HTTP-status|
| ResultType      | REST-API Ergebnis|
| SubscriptionId | Azure-Abonnement-ID des Abonnements mit Vault-Taste |


## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) Azure Key Vault Detaildaten anzeigen.

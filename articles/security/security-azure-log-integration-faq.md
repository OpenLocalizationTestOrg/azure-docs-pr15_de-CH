<properties
   pageTitle="Azure Protokoll Integration FAQ | Microsoft Azure"
   description="Diese FAQ beantwortet Fragen zur Azure Protokoll Integration."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Azure Protokoll Integration häufig gestellte Fragen (FAQ)

In diesem Dokument beantwortet Fragen zur Integration von Azure Protokoll, ein Dienst, der unformatierte Protokollen von Azure Ressourcen in Ihre lokalen Sicherheitsinformations- und Ereignismanagement (SIEM) Systeme integriert werden können. Diese Integration ermöglicht ein einheitliches Dashboard für alle Ihre Daten lokal oder in der Cloud, damit Sie sammeln, Korrelieren, analysieren und Warnung für Sicherheitsereignisse Anwendung zugeordnet.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Wie kann ich sehen, Speicherkonten aus denen Azure Protokoll Integration Azure VM Protokolle ziehen?

Führen Sie den Befehl **Azlog Quellliste**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Wie können die Proxykonfiguration aktualisieren?

Wenn die Proxyeinstellung Azure-Speicherzugriff nicht direkt erlaubt, öffnen Sie **AZLOG. EXE. CONFIG** Datei **c:\Program Files\Microsoft Azure Protokoll Integration**. Aktualisieren Sie die Datei **DefaultProxy** Abschnitt mit Proxy-Adresse Ihrer Organisation. Nach Update beenden Sie und starten Sie den Dienst mithilfe der Befehle **Azlog net Stop** und **net Start Azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Wie kann die Abonnementinformationen in Windows-Ereignisse anzeigen?

Append **Subscriptionid** angezeigter Name beim Hinzufügen der Quelle.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Das XML-Ereignis hat die Metadaten wie unten dargestellt, einschließlich der Abonnement-id

![Ereignis-XML][1]

## <a name="error-messages"></a>Fehlermeldungen

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Wenn Befehl **Azlog Createazureid**warum ich die Fehlermeldung erhalte?

Fehler:

  *Fehler beim Erstellen der Anwendung AAD - Mieter 72f988bf-86f1-41af-91ab-2d7cd011db37-Grund = "Verboten" - Nachricht = "Nicht genügend Berechtigungen um den Vorgang abzuschließen."*

**Azlog Createazureid** versucht, einen Dienstprinzipalnamen in der Azure AD Mieter für Abonnements erstellen auf dem Azure Login auf zugreifen. Ist Ihre Azure Anmeldung nur ein Gastbenutzer in Azure AD-Mandanten, dann schlägt der Befehl fehl "Ausreichende Berechtigungen, um den Vorgang abzuschließen." Fordern Sie Mieter Administrator Ihr Konto als Mitglied der Mieter hinzufügen.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Wenn Befehl **Azlog autorisieren**, warum ich die Fehlermeldung erhalte?

Fehler:

  *Warnung Rolle Aufgabe - AuthorizationFailed erstellen: Client janedo@microsoft.com' Objekt Id "fe9e03e4-4dad-4328-910f-fd24a9660bd2" hat keine Berechtigung für diesen Vorgang 'Microsoft.Authorization/roleAssignments/write' für Bereich "Abonnements/70 d 95299-d689-4c 97-b971-0d8ff0000000".*

**Azlog autorisieren** Befehl weist die Rolle Leser Dienstprinzipal Azure AD (erstellt mit **Azlog Createazureid**) Abonnements bereitgestellt. Ist Azure Login nicht Co-Administrator oder der Besitzer des Abonnements, schlägt es fehl mit Fehlermeldung 'Autorisierungsfehler'. Azure rollenbasierte Zugriffskontrolle (RBAC) Co-Administrator oder Besitzer ist erforderlich, um diese Aktion abzuschließen.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Wo finde ich die Definition der Eigenschaften im Überwachungsprotokoll?

Sieh:

- [Überwachen von Vorgängen mit Ressourcen-Manager](../resource-group-audit.md)
- [Liste der Ereignisse in einem Abonnement in Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Wo finde ich Informationen in Azure Security Center-Alarme?

Siehe [Verwalten und Sicherheitshinweise in Azure Security Center auf](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Wie kann ich ändern, was mit VM-Diagnose werden?

Weitere Informationen zum Abrufen, bearbeiten und [Mit PowerShell in einer virtuellen Maschine unter Windows Azure-Diagnose aktivieren](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) und Azure-Diagnose in Windows *(Bündel)* Konfiguration. Es folgt ein Beispiel:

### <a name="get-the-wad-config"></a>Bündel-Config abrufen

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Bündel-Konfiguration ändern

Im folgende Beispiel ist eine Konfiguration, nur EventID 4624 und EventId 4625 in das Sicherheitsereignisprotokoll erfasst. Microsoft Antimalware-Ereignisse werden im System-Ereignisprotokoll erfasst. Siehe [Consuming Events] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) ausführliche Informationen über die Verwendung von XPath-Ausdrücken.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Bündel-Konfiguration festlegen

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Überprüfen Sie nach Änderungen Speicherkonto um sicherzustellen, dass die richtigen Ereignisse gesammelt werden.

Zur Integration von Azure Protokoll Fragen senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png

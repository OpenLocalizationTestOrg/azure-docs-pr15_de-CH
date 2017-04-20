<properties
    pageTitle="Problembehandlung bei Microsoft Azure Stapel | Microsoft Azure"
    description="Azure Stapel Problembehandlung."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Problembehandlung bei Microsoft Azure Stapel

Dieses Dokument enthält allgemeine Informationen zur Problembehandlung für Azure.

Am besten stellen Sie sicher, dass der Bereitstellung alle [Vorschriften](azure-stack-deploy.md) und [Vorbereitung](azure-stack-run-powershell-script.md) vor der Installation erfüllt. 

Vorschläge für die Problembehandlung in diesem Abschnitt beschriebenen Probleme aus verschiedenen Quellen stammen und können Ihre Frage können nicht aufgelöst werden. Wenn ein Problem nicht auftritt, stellen Sie sicher überprüfen [Azure Stapel MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) für weitere Unterstützung und Weitere Informationen.

Codebeispiele ist und erwarteten Ergebnisse nicht garantiert werden. Dieser Abschnitt enthält häufig bearbeitet und Updates Verbesserungen umgesetzt werden.

  

## <a name="known-issues"></a>Bekannte Probleme

 - Die folgenden andauernd Fehler während der Bereitstellung auftreten die erfolgreiche Bereitstellung beeinträchtigen:
     - "Der Begriff"C:\WinRM\Start-Logging.ps1"nicht erkannt"
     - "EceAction aufrufen: kann nicht in ein null-Array index" 
     - "InvokeEceAction: Argument kann nicht"Message"Parameter gebunden werden, ist eine leere Zeichenfolge."
 - Schritt 60.61.93 mit Fehler "Anwendung mit"URI"nicht gefunden" fehlschlagen Bereitstellung auftreten Dies ist aufgrund der Anwendung in Azure Active Directory registriert sind.  Wenn diese Fehlermeldung weiterhin [erneut das Installationsskript](azure-stack-rerun-deploy.md) aus Schritt 60.61.93 bis zum Abschluss der Bereitstellung.
 - Sie sehen die Ressource **Verfügbarkeit** auf dem Markt wird unter Kategorie **VirtualMachine ARM** , diese Darstellung ist kein gravierendes Problem.
 - Beim Erstellen eines neuen virtuellen Computers im Portal in Schritt **Grundlagen** standardmäßig die Speicheroption SSD.  Diese Einstellung muss auf Festplatte oder auf die **Größe** der Bereitstellung geändert werden, wird nicht Größen wählen und Bereitstellung virtueller Computer. 
 - Sehen Sie AzureRM PowerShell-Module nicht mehr standardmäßig auf MAS CON01 VM installiert (in TP1 diese ClientVM genannt). Dieses Verhalten ist beabsichtigt, da gibt es eine alternative Methode, [diese Module installieren und verbinden](azure-stack-connect-powershell.md).  
 - Sie werden feststellen, dass **Microsoft.Insights** Ressourcenprovider für Mieter Abonnements nicht automatisch registriert wird. Möchten Sie Überwachungsdaten für eine VM als Mieter bereitgestellt, führen Sie den folgenden Befehl von PowerShell (nachdem Sie [Installieren und verbinden](azure-stack-connect-powershell.md) als Mieter): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Sie sehen Exportfunktionen im Portal für Ressourcengruppen, jedoch kein Text angezeigt und exportieren.      
 - Starten Sie eine Bereitstellung von Speicherressourcen verfügbar Kontingent überschreitet.  Die Bereitstellung schlägt fehl und kontoressourcen angehalten.  Zwei Wiederherstellungsoptionen sind verfügbar:
     - Dienstadministratoren können Änderung nicht sofort wirksam und häufig bis zu einer Stunde dauern verbreiten das Kontingent zu erhöhen.
     - Dienstadministratoren können einen Add-on-Plan mit zusätzlichen erstellen, Pächter dann das Abonnement hinzufügen kann.
 - Wenn das Portal VMs auf Azure Stapel Umgebungen mit 'Azure - China' Identität erstellen, Größen, **Größe** Schritt der Bereitstellung VM wird nicht angezeigt und können Bereitstellung fortzusetzen.
 - Ein Bereitstellungsfehler im Portal möglicherweise angezeigt werden, wenn die VM erfolgreich bereitgestellt wurde.
 - Wenn Sie eine Planung, Angebot oder Abonnement löschen, können VMs nicht gelöscht werden.
 - Sie sehen die VM-Erweiterungen auf dem Markt.
 - Einen virtueller Computer aus einer gespeicherten VM-Image kann nicht bereitgestellt werden.
 - Mieter auftreten Dienste, die nicht in ihrem Abonnement enthalten sind.  Beim Mieter dieser Ressourcen bereitstellen, erhalten sie eine Fehlermeldung.  Beispiel: Mieter Abonnement enthält nur Speicherressourcen.  Mieter sehen können andere Ressourcen wie VMs zu erstellen.  Wenn ein Mieter versucht, einen virtuellen Computer bereitstellen, erhalten sie in diesem Szenario eine Meldung, dass die VM erstellt werden kann. 
 - Wenn TP2 installieren, sollten Sie nicht Host aktivieren OS bereitgestellt, Azure Stack Setup-Skript ausführen oder möglicherweise einen Fehler, dass Windows messaging VHD bald abläuft.


## <a name="deployment"></a>Bereitstellung

### <a name="deployment-failure"></a>Bereitstellungsfehler
Wenn einen Fehler während der Installation auftreten, kann Azure Stack Installer Sie Installationsfehler anhand der [Bereitstellungsschritte erneut](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Am Ende der Bereitstellung die PowerShell-Sitzung noch geöffnet und keine Ausgabe anzeigen

Dies ist wahrscheinlich nur das Ergebnis des Standardverhaltens ein Befehlsfenster PowerShell ausgewählt wurde. Tatsächlich die POC-Bereitstellung erfolgreich war, aber das Skript wurde angehalten, wenn das Fenster. Sie können überprüfen, dass dies ist der Fall, suchen das Wort "in der Titelleiste des Befehlsfensters auswählen".  Drücken Sie ESC, um es zu deaktivieren und danach die Vollzugsmeldung angezeigt werden soll.

## <a name="templates"></a>Vorlagen

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Azure-Stapel wird nicht Azure Vorlage bereitstellen.

Stellen Sie sicher, dass:

- Die Vorlage muss Microsoft Azure-Dienst verwenden, die bereits verfügbar oder in der Vorschau in Azure Stapel.
- Wirkstoffe für eine bestimmte Ressource wird von der lokalen Instanz von Azure Stapel und eine gültige Position ("local" in Azure Stapel technischen Vorschau (TP) 2 Vs "East US" oder "Südindien" in Azure) richten.
- Sie überprüfen die geringfügige Unterschiede in der Azure-Ressourcen-Manager-Syntax abfangen Cmdlets Test-AzureRmResourceGroupDeployment [in diesem Artikel](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) .

Die Azure-Stapel Vorlagen bereits im [GitHub Repository](http://aka.ms/AzureStackGitHub/) können Sie Einstieg.

## <a name="virtual-machines"></a>Virtuelle Computer

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Nach dem Start des Microsoft Azure Stapel POC-Hosts sind meine Mieter VMs vom Hyper-V-Manager und wieder automatisch nach etwas warten?

Wie das System wieder Speichersubsystem und RPs Konsistenz festlegen müssen. Die benötigte Zeit hängt von der Hardware und Spezifikationen verwendet, jedoch möglicherweise einige Zeit nach einem Neustart des Hosts Mieter VMs und erkannt werden.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ich einige virtuelle Computer gelöscht haben und trotzdem VHD-Dateien auf dem Datenträger anzuzeigen. Erwarten dieses Verhalten ist zu?

Ja, ist das erwartete Verhalten. Es wurde so entwickelt, da:

- Wenn Sie einen virtuellen Computer löschen, werden Festplatten nicht gelöscht. Festplatten sind separate Ressourcen in der Ressourcengruppe.
- Ein Speicherkonto gelöscht wird, der Löschvorgang sofort durch Azure Ressourcenmanager (Portal PowerShell) sichtbar ist als Datenträger enthält möglicherweise werden noch im Speicher, bis die Garbagecollection ausführt.

"Verwaiste" virtueller Festplatten finden ist es wichtig zu wissen, sind Teil des Ordners für ein Speicherkonto, das gelöscht wurde. Wenn das Speicherkonto nicht gelöscht wurde, ist es normal, noch vorhanden sind.

Erfahren Sie mehr zum Konfigurieren des Aufbewahrung Schwellenwerts in [Speicher verwalten](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Installationsschritte
Die folgende Informationen zu Azure Stapel Installationsschritte möglicherweise nützlich für die Problembehandlung:  

| Index | Name | Beschreibung|
| ----- | ----- | -----|
|0,11 | (DEP) Physische Computer überprüfen | Überprüfen von Hardware und Betriebssystem-Konfiguration auf dem physischen Knoten. |
| 0,12 | (DEP) Konfigurieren der physischen Netzwerke für Prüfung des Konzepts | Konfigurieren von virtuellen Switches und Schnittstellen. |
| 0,14 | (DEP) Domäne bereitstellen | Bereitstellen von Active Directory auf dem virtuellen Computer. |
| 0,15 | (DEP) Konfigurieren Sie den Server der Domäne | Konfigurieren Sie mit Sicherheitsgruppen usw. Domänenserver. |
| 0,16 | (DEP) Konfigurieren der physischen Maschine | Konfigurieren von Netzwerk, Domänenbeitritt und Setup lokale Administratoren. |
| 0,18 | (STO) Speichercluster konfigurieren | Erstellen Sie Storage-Cluster, erstellen Sie einen Speicher-Pool und Server. |
| 0,19 | (CPI) Installation Fabric-Infrastruktur | Richten Sie die erforderlichen Komponenten für die Fabric-Bereitstellung. |
| 0,21 | (NETTO) BGP und NAT einrichten | BGP und NAT - benötigt nur für einen Knoten installiert. |
| 0,22 | (NETTO) Konfigurieren Sie NAT und Zeitserver | Den Zeitserver synchronisiert und NAT-Einträgen konfiguriert. |
| 40,41 | (CPI) Erstellen von Gastcomputern | Erstellen Sie die Verwaltung virtueller Computer. |
| 40.42 | (FBI) PowerShell JEA einrichten | PowerShell JEA für alle Rollen eingerichtet. |
| 40.43 | (FBI) Einrichten von Azure Stack-Zertifizierungsstelle | Azure Stack-Zertifizierungsstelle installiert. |
| 40.44 | (FBI) Konfigurieren der Zertifizierungsstelle Azure Stapel | Azure Stapel Zertifizierungsstelle konfiguriert. |
| 40.45 | (NETTO) Einrichten von NC-Folgen für VMs | NC-Folgen installiert auf den virtuellen Gastcomputern |
| 40.46 | (NETTO) Konfigurieren von NC-Folgen auf VMs | NC-Folgen auf den virtuellen Gastcomputern konfigurieren |
| 40,47 | (NETTO) Konfigurieren von Gastcomputern | Konfigurieren der Verwaltung virtueller Computer mit NC-ACLs. |
| 60.61.81 | (FBI) Bereitstellen Sie Rufzeichen für Stapel Azure Fabric - FabricRing Voraussetzung | VIPs für FabricRing erstellt |
| 60.61.82 | (FBI) Bereitstellen von Azure Stapel Fabric Rufzeichen - Fabric Ring Cluster bereitstellen | Installation und Konfiguration von Azure Stapel Fabric Ring Cluster. |
| 60.61.83 | (FBI) Bereitstellen von Admin-Erweiterung für Ressourcen | Installieren von Admin-Erweiterung für Ressourcen |
| 60.61.84 | (ACS) Azure einheitliche Speicher auf Knoten einrichten. | Installation und Konfiguration von Azure einheitliche Speicher auf Knoten. |
| 60.61.85 | (ACS) Azure einheitliche Speicher im Cluster-Ebene festlegen. | Installation und Konfiguration von Azure einheitliche Speicher Cluster auf. |
| 60.61.86 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für InfraServiceController |
| 60.61.87 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für KLI |
| 60.61.88 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für ASAppGateway |
| 60.61.89 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für Massenspeicher-Controller |
| 60.61.90 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für HealthMonitoring |
| 60.61.91 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für ECE |
| 60.61.92 | (FBI) Bereitstellen von Stapel Azure Fabric Controller Klingelzeichen - Voraussetzung | Erforderliche Komponenten für PMM |
| 60.61.93 | (Katal) AzureStack Service-Hauptbenutzer erstellen | Erstellen Sie Azure Graph Applications and Service Prinzipale in AAD. |
| 60.61.94 | (NETTO) Setup GW VMs | Auf den virtuellen Gastcomputern installiert GW. |
| 60.61.95 | (NETTO) Konfigurieren von GW VMs | GW konfiguriert auf den virtuellen Gastcomputern. |
| 60.61.96 | (NETTO) IDNS Hosts bereitstellen | IDNS Infrastruktur Hosts bereitstellen |
| 60.61.97 | (NETTO) Konfigurieren von iDNS | IDNS Rolle |
| 60.61.98 | (FBI) WSUS VMs einrichten | WSUS-Server installiert auf den virtuellen Gastcomputern. |
| 60.61.99 | (FBI) Konfigurieren von WSUS VMs | WSUS-Server konfiguriert auf den virtuellen Gastcomputern. |
| 60.61.100 | (FBI) SQL Azure VMs einrichten | Installiert SQL Azure-Server auf den virtuellen Gastcomputern |
| 60.61.101 | (Katal) Setup-Komponenten für VMs wurde. | Erforderliche Komponenten für Microsoft Azure auf den virtuellen Gastcomputern richtet. |
| 60.61.102 | (Katal) Setup konnte die VMs | Installiert Microsoft Azure Stapel auf dem Gastcomputer. |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.121 | (FBI) Bereitstellen von Ressourcen und Controller | Ressourcen und Controller installiert |
| 60.120.122 | (FBI) Konfiguration | Controller konfiguriert |
| 60.120.123 | (Katal) Konfigurieren von WAS VMs | Microsoft Azure Stapel konfiguriert auf den virtuellen Gastcomputern. |
| 60.120.124 | (Katal) Azure AAD Konfiguration. | Azure AD konfiguriert Azure Stapel. |
| 60.120.125 | (Katal) ADFS installieren | Installiert Active Directory Federation Services (ADFS) |
| 60.120.126 | (Katal) Installieren der ADFS-Diagramm | Installiert Azure Stapel-Diagramm |
| 60.120.127 | (Katal) Konfigurieren von ADFS | Konfigurieren von Active Directory Federation Services (ADFS) |
| 60.140.141 | (FBI) SRP konfigurieren | Anbieter von Speicher-Ressourcen konfiguriert |
| 60.140.142 | (ACS) Konfigurieren Sie Azure einheitliche Speicher. | Azure einheitliche Speicher konfiguriert. |
| 60.140.143 | (FBI) Storage-Konten erstellen | Erstellen Sie alle Speicherkonten von Anbietern verwendet werden. |
| 60.140.144 | (FBI) Verwendung für SRP | Registrieren Sie Verwendung für Anbieter. |
| 60.140.145 | (CPI) KLI migrieren Sie erstellten VMs, Hosts und Cluster | Objekte der erstellten VMs, Hosts und Cluster migriert, CPI |
| 60.140.146 | (FBI) Konfigurieren von Windows Defender | Konfigurieren von Windows Defender |
| 60.160.161 | (MO) Konfigurieren der Überwachung von Agent | Überwachen von Agent konfiguriert |
| 60.160.162 | (FBI) NFP Voraussetzung | NFP Komponenten installiert |
| 60.160.163 | (FBI) NFP-Bereitstellung | NFP installiert  |
| 60.160.164 | (FBI) NFP-Konfiguration | NFP konfiguriert |
| 60.160.165 | (FBI) CRP Voraussetzung | CRP-Komponenten installiert |
| 60.160.166 | (FBI) CRP-Bereitstellung | CRP installiert |
| 60.160.167 | (FBI) CRP-Konfiguration | CRP konfiguriert |
| 60.160.168 | (FBI) GFK Voraussetzung | FK-Komponenten installiert |
| 60.160.169 | (FBI) FK-Bereitstellung | GFK installiert  |
| 60.160.170 | (FBI) FK-Konfiguration | GFK konfiguriert |
| 60.160.174 | (FBI) Erforderliche URP | URP Komponenten installiert |
| 60.160.175 | (FBI) URP Bereitstellung | URP installiert  |
| 60.160.176 | (FBI) URP-Konfiguration | URP konfigurieren |
| 60.160.171 | (FBI) HRP Voraussetzung | HRP-Komponenten installiert |
| 60.160.172 | (FBI) HRP-Bereitstellung | HRP installiert  |
| 60.160.173 | (FBI) HRP-Konfiguration | HRP konfiguriert |
| 60.160.177 | (KV) Erforderliche Schlüsseltresor | Schlüsseltresor Komponenten installiert |
| 60.160.178 | (KV) Schlüsseltresor Bereitstellung | Schlüsseltresor installiert  |
| 60.160.179 | (KV) Schlüsseltresor-Konfiguration | Schlüsseltresor konfigurieren | 
| 60.190.191 | (FBI) Konfigurieren der Galerie | Konfigurieren der Galerie |
| 60.190.192 | (FBI) Fabric-Klingelzeichen konfigurieren | Fabric-Klingelzeichen konfigurieren |
| 60.221 | (FBI) Setup-Konsole VMs | Console Server installiert auf den virtuellen Gastcomputern. |
| 60.222 | (FBI) Setup-Konsole VMs | Verschieben Sie DVM Inhalt in der Konsole VM. |
| 251 | Bereiten Sie für zukünftige Host neu gestartet vor | Neustart-Richtlinie festlegen |


## <a name="next-steps"></a>Nächste Schritte

[Häufig gestellte Fragen](azure-stack-FAQ.md)

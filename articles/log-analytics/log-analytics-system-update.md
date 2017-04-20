<properties
    pageTitle="System Update Lösung in Protokollanalyse | Microsoft Azure"
    description="Systemupdates Lösung können in Protokollanalyse Sie fehlende Updates auf Servern in der Infrastruktur anwenden."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>System Update Lösung in Protokollanalyse

Systemupdates Lösung können in Protokollanalyse Sie fehlende Updates auf Servern in der Infrastruktur anwenden. Nach der Installation der Lösung können Sie die Updates anzeigen, die von überwachten Servern fehlen mit **Update Systemanalyse** nebeneinander auf der Seite **Übersicht** OMS.

Fehlende Updates gefunden werden, werden Details im Schaltpult **Updates** angezeigt. Das Dashboard **Updates** können fehlende Updates und entwickeln Sie einen Plan für die Server anzuwenden, die sie benötigen.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md)fügen Sie die Lösung System Update hinzu.  Es ist keine weitere Konfiguration erforderlich.

## <a name="system-update-data-collection-details"></a>System Update Einzelheiten zur Datensammlung

Systemanalyse Update sammelt Metadaten und Status mit Agents, die Sie aktiviert haben.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für System aktualisieren.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)|![Nein](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Nein](./media/log-analytics-system-update/oms-bullet-red.png)|![Ja](./media/log-analytics-system-update/oms-bullet-green.png)| 2 Mal pro Tag und 15 Minuten nach der Installation eines Updates|

Folgende Tabelle zeigt Beispiele für Datentypen Update Systemanalyse gesammelt:

|**Datentyp**|**Felder**|
|---|---|
|Metadaten|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, Netzwerkname, IP-Adresse, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-Adresse, NetbiosDomainName, LogicalProcessors, DNS-Name, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Zustand|StateChangeEventId State-ID, NewHealthState, OldHealthState, Rahmen, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Arbeiten mit updates

1. Klicken Sie auf der Seite **Übersicht** **Systemanalyse aktualisieren** .  
    ![System Update Bewertung Kachel](./media/log-analytics-system-update/sys-update-tile.png)
2. Klicken Sie im Schaltpult **Updates** zeigen Sie Update-Kategorien an  
    ![Updates dashboard](./media/log-analytics-system-update/sys-updates02.png)
3. Bildlauf nach rechts der Seite Blade **Kritisch/Sicherheitsupdates für Windows** und anschließend unter **Klassifizierung**auf **Sicherheitsupdates**.  
    ![Sicherheitsupdates](./media/log-analytics-system-update/sys-updates03.png)
4. Auf der Suchseite Protokoll zeigt eine Vielzahl von Informationen zu den Sicherheitsupdates, die in Ihrer Infrastruktur Server nicht gefunden wurden. Klicken Sie auf **Liste** , um detaillierte Informationen zu den Updates anzuzeigen.  
    ![Suchergebnisse - Liste](./media/log-analytics-system-update/sys-updates04.png)
5. Klicken Sie auf Protokoll suchen zeigt detaillierte Informationen zu den einzelnen Updates. Neben dem KBID klicken Sie auf **Ansicht** , um den entsprechenden Artikel auf der Microsoft Support-Website.  
    ![Suchergebnisse - Ansicht KB protokollieren](./media/log-analytics-system-update/sys-updates05.png)
6. Webbrowser wird die Microsoft Support-Webseite für das Update in einer neuen Registerkarte geöffnet. Hier werden Informationen zum Update fehlt.  
    ![Microsoft Support-Webseite](./media/log-analytics-system-update/sys-updates06.png)
7. Mit den Informationen Sie gefunden haben, erstellen Sie einen Plan fehlenden Updates manuell anzuwenden oder die verbleibenden Schritte automatisch Updates folgen.
8. Ggf. automatisch die fehlenden Updates **Updates** Dashboard zurück und dann unter **Update läuft**auf **Planen Sie eine Aktualisierung ausführen klicken**.  
    ![Update wird ausgeführt - geplante Registerkarte](./media/log-analytics-system-update/sys-updates07.png)
9. Klicken Sie auf **Update läuft** auf der Registerkarte **Geplante Tasks** auf **Hinzufügen** , erstellen Sie ein neues Update ausführen.  
    ![Geplante tab - hinzufügen](./media/log-analytics-system-update/sys-updates08.png)
10. Fügen Sie auf der Seite **Neues Update ausführen** , geben Sie ein Namen für das Update ausführen einzelne Computer oder Computergruppen hinzu, definieren Sie einen Zeitplan und dann auf **Speichern**.  
    ![Neue Update ausführen](./media/log-analytics-system-update/sys-updates09.png)
11. Die Registerkarte **Geplante** neue Update ausführen, zeigt die **Aktualisierung ausgeführt** haben Sie geplant.  
    ![Geplante Registerkarte](./media/log-analytics-system-update/sys-updates10.png)
12. Beginnt das Update ausführen, sehen Sie Informationen auf der Registerkarte **ausgeführt** .  
    ![Registerkarte ausführen](./media/log-analytics-system-update/sys-updates11.png)
13. Nach Abschluss der Aktualisierung ausführen **abgeschlossen** die Registerkarte Status angezeigt.
14. Wenn Updates ausführen Blade **Kritisch und Sicherheits-Updates von Windows** Update angewendet wurden sehen Sie reduziert die Anzahl der Updates.  
    ![Kritisch und Sicherheits-Updates von Windows Blade - Aktualisierungszähler reduziert](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Nächste Schritte

- [Suche Protokolle](log-analytics-log-searches.md) detaillierter Updatedaten anzeigen.

<properties
   pageTitle="Azure AD Verbindung synchronisieren: Planer | Microsoft Azure"
   description="Dieses Thema beschreibt integrierte Taskplaner Funktion in Azure AD-Verbindung synchronisieren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Verbindung synchronisieren: Planer
Dieses Thema beschreibt integrierte Taskplaner Azure AD Connect synchron (auch bekannt als Synchronisierungsmodul).

Dieses Feature wurde mit 1.1.105.0 (Februar 2016 veröffentlicht).

## <a name="overview"></a>Übersicht
Azure AD Connect-Synchronisierung synchronisiert Veränderungen in Ihrem lokalen Verzeichnis mit einen Planer. Gibt es zwei Planer Prozesse für Kennwort synchronisieren und ein anderes Attribut Object Sync und Wartungsaufgaben. In diesem Thema werden diese behandelt.

In früheren Versionen wurde des Planers für Objekte und Attribute in dem Synchronisierungsmodul und der Windows-Taskplaner oder separater Windows-Dienst wurde verwendet, um den Synchronisationsprozess auslösen. Der Planer ist die Synchronisierung mit der Versionen 1.1 engine und Anpassung erlauben. Die neue Standardeinstellung Synchronisierungsfrequenz beträgt 30 Minuten.

Der Planer ist zwei Aufgaben:

- **Synchronisierung wechseln**. Der Prozess zum Importieren, synchronisieren und Exportieren ändert.
- **Wartungsaufgaben**. Erneuern Sie Schlüssel und Zertifikate für das Zurücksetzen und das Gerät Registration Service (DRS) Löschen Sie alte Einträge im Protokoll.

Planer selbst wird immer ausgeführt, aber es kann konfiguriert werden nur eine oder keine dieser Aufgaben ausgeführt werden. Beispielsweise benötigen Sie eine eigene Synchronisierung Zyklus Prozess deaktivieren diese Tasks im Taskplaner können weiterhin die Wartungsaufgabe ausgeführt.

## <a name="scheduler-configuration"></a>Scheduler-Konfiguration
Zum Anzeigen die aktuellen Konfigurationen gehen PowerShell und ausführen `Get-ADSyncScheduler`. Es wird Folgendes angezeigt:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Beim Ausführen dieses Cmdlets **Synchronisierungsbefehl oder Cmdlet ist nicht verfügbar** angezeigt werden, ist das PowerShell-Modul nicht geladen. Dies kann vorkommen, wenn Sie Azure AD Connect auf einem Domänencontroller oder auf einem Server mit PowerShell Einschränkung als Standardeinstellungen. Wenn dieser Fehler angezeigt wird, führen Sie `Import-Module ADSync` das Cmdlet zur Verfügung.

- **AllowedSyncCycleInterval**. Am häufigsten können Azure AD Synchronisationen auftreten. Sie können nicht häufiger als synchronisiert und weiterhin unterstützt werden.
- **CurrentlyEffectiveSyncCycleInterval**. Der Zeitplan momentan in Kraft. Haben denselben Wert wie CustomizedSyncInterval (falls festgelegt) ist dies nicht als AllowedSyncInterval. Wenn Sie CustomizedSyncCycleInterval ändern, wird diese nach dem nächsten Synchronisierungszyklus wirksam.
- **CustomizedSyncCycleInterval**. Wenn die in einer beliebigen Frequenz als der Standardwert 30 Minuten ausgeführt werden soll, konfigurieren Sie diese Einstellung. In die Abbildung oben Planer wurde festgelegt, stattdessen stündlich. Wenn Sie diese auf einen Wert kleiner als AllowedSyncInterval festlegen, wird diese verwendet.
- **NextSyncCyclePolicyType**. Delta oder Initiale. Definiert sollte die nächste Ausführung nur Prozess Delta ändert, oder wenn die nächste Ausführung sollte eine vollständige importieren und Synchronisieren der würden auch neuen oder geänderten Regeln verarbeiten.
- **NextSyncCycleStartTimeInUTC**. Nächsten startet der Planer beim nächsten Zyklus synchronisieren.
- **PurgeRunHistoryInterval**. Die Zeit Protokolle aufbewahrt werden. Diese können im Synchronisation-Manager überprüft werden. Der Standardwert ist 7 Tage halten.
- **SyncCycleEnabled**. Gibt an, ob der Zeitplan importieren, Synchronisierung und Prozesse exportieren als Teil der Operation ausgeführt wird.
- **MaintenanceEnabled**. Gibt an, ob der Wartungsprozess aktiviert ist. Diese Zertifikate/Schlüssel aktualisiert und Protokoll löschen.
- **IsStagingModeEnabled**. Zeigt, wenn [staging-Modus](active-directory-aadconnectsync-operations.md#staging-mode) aktiviert ist.

Ändern Sie einige Optionen mit `Set-ADSyncScheduler`. Die folgenden Parameter können geändert werden:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Die Scheduler-Konfiguration wird in Azure AD gespeichert. Haben einen Stagingserver bewirkt eine Änderung auf dem primären Server auch Testserver (außer IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntax:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - Tage, HH - Stunden, mm - Minuten, ss - Sekunden

Beispiel:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Ändert den Planer alle 3 Stunden ausgeführt.

Beispiel:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Ändert den Zeitplan täglich.

## <a name="start-the-scheduler"></a>Planer starten
Der Planer wird standardmäßig alle 30 Minuten ausgeführt. In einigen Fällen soll einen Sync-Zyklus zwischen geplanten Zyklen ausgeführt oder einen anderen Typ ausführen müssen.

**Delta-Synchronisierung Zyklus**  
Delta-Synchronisierung Zyklus umfasst folgende Schritte:

- Deltaimport an allen Anschlüssen
- Delta-Synchronisierung an allen Anschlüssen
- An allen Anschlüssen exportieren

Möglicherweise haben Sie dringend ändern die sofort synchronisiert werden, deshalb müssen Sie manuell einen Zyklus ausführen. Möchten Sie manuell einen Zyklus von PowerShell ausführen führen Sie `Start-ADSyncSyncCycle -PolicyType Delta`.

**Vollständige Synchronisierung Zyklus**  
Wenn Sie eine der folgenden Konfiguration erstellt haben, müssen Sie einen vollständige Synchronisierung Zyklus ausgeführt (auch bekannt als Erste):

- Mehrere Objekte oder Attribute aus einem Quellverzeichnis hinzugefügt
- Die Regeln geändert
- Verschiedene Objekte sollen geändert [Filtern](active-directory-aadconnectsync-configure-filtering.md)

Wenn Sie eine dieser Änderungen vorgenommen haben, müssen Sie einen vollständige Synchronisierung Zyklus ausführen, damit das Synchronisierungsmodul die Möglichkeit hat, die Connector Leerzeichen Quellbereichs neu zu konsolidieren. Ein vollständige Synchronisierung Zyklus umfasst folgende Schritte:

- Vollständigen Import an allen Anschlüssen
- Vollständige Synchronisierung an allen Anschlüssen
- An allen Anschlüssen exportieren

Um eine vollständige Synchronisierung Zyklus zu initiieren, führen `Start-ADSyncSyncCycle -PolicyType Initial` von PowerShell aufgefordert. Dadurch wird einen vollständige Synchronisierung Zyklus gestartet.

## <a name="stop-the-scheduler"></a>Beenden des Planers
Wenn der Planer einen Synchronisierungszyklus möglicherweise beenden läuft. Wenn beispielsweise der Installationsassistent gestartet und diese Fehlermeldung:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Wenn ein Sync-Zyklus ausgeführt wird, ändern nicht Sie Konfiguration. Sie könnte warten, bis der Planer den Prozess beendet, aber Sie können beenden sie sofort Ihre Änderungen vornehmen können. Anhalten des aktuellen Zyklus ist nicht schädlich und Änderungen nicht abgearbeitet mit weiter verarbeitet werden.

1. Beginnen des Planers der Zyklus mit dem PowerShell-Cmdlet zu `Stop-ADSyncSyncCycle`.
2. Das Beenden des Planers wird nicht der aktuellen Verbindung von der aktuellen Aufgabe. Um den Connector zu erzwingen, die folgenden Aktionen: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Starten Sie **Synchronisierung Service** über das Startmenü. Wechseln zu **Connectors**, markieren Sie den Connector mit dem Status **ausgeführt** und wählen Sie Aktionen **Beenden** .

Der Planer ist noch aktiv und auf der nächsten Gelegenheit wieder gestartet.

## <a name="custom-scheduler"></a>Benutzerdefinierte Planer
In diesem Abschnitt dokumentierten Cmdlets sind nur in Build [1.1.130.0](active-directory-aadconnect-version-history.md#111300) und höher verfügbar.

Wenn der integrierte Taskplaner nicht Ihre Bedürfnisse erfüllen, können Sie die Connectors mit PowerShell planen.

### <a name="invoke-adsyncrunprofile"></a>Rufen Sie ADSyncRunProfile
Sie können ein Profil für einen Connector so starten:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Die Namen für [Konnektorname](active-directory-aadconnectsync-service-manager-ui-connectors.md) und [Profilnamen ausführen](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) finden [Synchronisierung Service Manager-Benutzeroberfläche](active-directory-aadconnectsync-service-manager-ui.md).

![Ausführungsprofil aufgerufen](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Die `Invoke-ADSyncRunProfile` Cmdlet ist synchron, d. h. es wird erst zurückgegeben, Kontrolle der Connector den Vorgang entweder erfolgreich oder Fehler abgeschlossen wurde.

Planen von Connectors wird in der folgenden Reihenfolge zu planen:

1. (Voll-Dreieck) Aus lokalen Verzeichnissen wie Active Directory importieren
2. (Voll-Dreieck) Importieren von Azure AD
3. (Voll-Dreieck) Synchronisierung von lokalen Verzeichnissen wie Active Directory
4. (Voll-Dreieck) Synchronisierung von Azure AD
5. Exportieren in Azure AD
6. Exportieren Sie in lokalen Verzeichnissen wie Active Directory

Betrachten der integrierte Taskplaner ist die Reihenfolge der Connectors ausführen.

### <a name="get-adsyncconnectorrunstatus"></a>ADSyncConnectorRunStatus abrufen
Sie können auch überwachen das Synchronisierungsmodul beschäftigt oder im Leerlauf ist. Dieses Cmdlet gibt ein leeres Resultset zurück, wenn das Synchronisierungsmodul im Leerlauf ist und einen Connector wird nicht ausgeführt. Wenn ein Connector ausgeführt wird, wird der Name der Verbindung zurückgegeben.

```
Get-ADSyncConnectorRunStatus
```

![Connector Ausführungsstatus](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
In der obigen Abbildung wird die erste Zeile aus dem Leerlauf das Synchronisierungsmodul ist. Die zweite Zeile beim Azure Active Directory Connector ausgeführt wird.

## <a name="scheduler-and-installation-wizard"></a>Planer und Installations-Assistenten
Wenn der Installations-Assistent gestartet wird, wird der Planer vorübergehend angehalten. Deshalb, weil davon ausgegangen wird, Sie Konfiguration und diese können nicht angewendet werden, wenn das Synchronisierungsmodul aktiv ausgeführt wird. Aus diesem Grund lassen der Installationsassistent geöffnet da das Synchronisierungsmodul Synchronisierung Aktionen durchführen stoppt.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

<properties
   pageTitle="Azure AD Verbindung synchronisieren: Aufgaben und Hinweise | Microsoft Azure"
   description="Dieses Thema beschreibt Betriebsaufgaben Azure AD-Verbindung synchronisieren und zum Betrieb dieser Komponente vorbereiten."
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
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Verbindung synchronisieren: Aufgaben und Prüfung
Dieses Thema soll beschreiben Betriebsaufgaben für Azure AD-Verbindung synchronisieren.

## <a name="staging-mode"></a>Stagingmodus
Stagingmodus kann für mehrere Szenarien verwendet:

-   Hohe Verfügbarkeit.
-   Testen Sie und Bereitstellen Sie neuer Konfiguration ändert.
-   Einführung eines neuen Servers und Außerbetriebnahme der alten.

Mit einem Server im staging-Modus können die Konfiguration ändern und Vorschau der Änderungen anzeigen, bevor Sie den Server aktivieren. Außerdem können vollständiger Import und vollständige Synchronisierung zu überprüfen, ob alle Änderungen erwartet werden, bevor diese Änderungen in Ihre produktionsumgebung.

Während der Installation können Sie den Server der **Modus**auswählen. Dadurch wird des Servers für Import und Synchronisierung, aber alle Ausfuhren nicht ausgeführt. Server im staging-Modus wird nicht Kennwort synchronisieren oder Kennwort Rückschreiben ausgeführt, auch wenn Sie diese Funktionen während der Installation aktiviert. Beim staging-Modus deaktivieren, Server exportiert Kennwort synchronisieren kann und Kennwort Rückschreiben aktiviert.

Mit der Synchronisierung Dienst-Manager können Sie noch einen Export erzwingen.

Server im staging-Modus weiterhin ändert aus Active Directory und Azure. Es hat immer eine Kopie der letzten Änderung und kann sehr schnell die Aufgaben eines anderen Servers. Wenn Sie den primären Server Konfiguration vornehmen, obliegt es Ihnen zu Änderungen an dem Server im Modus der Stagingdatenbank.

Für diejenigen mit älteren Sync-Technologien unterscheidet der Stagingdatenbank Modus Server eine eigene SQL-Datenbank hat. Diese Architektur ermöglicht dem Testserver Modus in einem anderen Rechenzentrum befinden.

### <a name="verify-the-configuration-of-a-server"></a>Überprüfen Sie die Konfiguration eines Servers
Gehen Sie folgendermaßen vor, um diese Methode anzuwenden:

1. [Vorbereiten](#prepare)
2. [Importieren und synchronisieren](#import-and-synchronize)
3. [Stellen Sie sicher](#verify)
4. [Aktive Server Switch](#switch-active-server)

#### <a name="prepare"></a>Vorbereiten

1. Installieren von Azure AD Connect **staging-Modus**aktivieren und deaktivieren Sie auf der letzten Seite des Installationsassistenten **Synchronisierung starten** . Dieser Modus ermöglicht das Synchronisierungsmodul manuell ausführen.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Zeichen aus/anmelden und wählen im Startmenü **Synchronisierungsdienst**.

#### <a name="import-and-synchronize"></a>Importieren und synchronisieren

1. Wählen Sie **Connectors**und der ersten Verbindung mit dem **Active Directory-Domänendienste**. Klicken Sie auf **Ausführen**, wählen Sie **vollständigen Import**und **OK**. Führen Sie diese Schritte für alle Konnektoren dieses Typs.
2. Markieren Sie den Verbinder mit **Azure Active Directory (Microsoft)**. Klicken Sie auf **Ausführen**, wählen Sie **vollständigen Import**und **OK**.
3. Stellen Sie sicher, dass die Registerkarte Connectors noch ausgewählt ist. Klicken Sie für jeden Connector mit **Active Directory Domain Services**auf **Ausführen**, wählen Sie **Delta-Synchronisierung**und **OK**.
4. Markieren Sie den Verbinder mit **Azure Active Directory (Microsoft)**. Klicken Sie auf **Ausführen**, wählen Sie **Delta-Synchronisierung**und **OK**.

Jetzt haben Sie bereitgestellt Export in Azure AD geändert und lokalen AD (verwenden Sie Exchange-Hybrid-Bereitstellung). Die nächsten Schritte können Sie überprüfen, was ist zu ändern, bevor Sie den Export in die Verzeichnisse beginnen.

#### <a name="verify"></a>Stellen Sie sicher

1. Cmd Prompt starten und zur`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Ausführen:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Der Name des Konnektors in Synchronisierungsdienst finden. Es hat einen Namen wie "contoso.com-AAD" Azure AD.
3. Ausführen:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Sie haben eine Datei %TEMP% namens export.csv in Microsoft Excel untersucht werden kann. Diese Datei enthält alle Änderungen, die exportiert werden sollen.
5. Erforderlichen Daten oder Konfigurationsinformationen ändern und diese Schritte erneut (importieren und synchronisieren und überprüfen) bis geändert, die exportiert werden sollen.

**Verständnis der export.csv-Datei**

Die Datei ist selbsterklärend. Einige Abkürzungen der Inhalt:

- OMODT-Typ ändern. Gibt an, ob der Vorgang auf einem ein hinzufügen, aktualisieren oder löschen.
- AMODT-Änderung Attributtyp. Gibt an, ob der Vorgang Ebene Attribut ein hinzufügen, aktualisieren oder löschen.

Wenn der Attributwert mehrwertig ist, ist nicht jede Änderung angezeigt. Die Anzahl der Werte hinzugefügt und entfernt wird angezeigt.

#### <a name="switch-active-server"></a>Aktive Server Switch

1. Auf dem aktiven Server den Server ausschalten (DirSync, FIM/Azure AD Sync) so nicht exportieren in Azure AD oder im staging-Modus (Azure AD Connect) festgelegt.
2. Der Installations-Assistent auf dem Server der **Modus** ausgeführt und **staging-Modus**deaktivieren.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Disaster recovery
Der Implementierungsentwurf gehört planen, was tun, wenn eine Katastrophe, Sync-Server verloren. Es gibt verschiedene Modelle zu verwenden, verwenden Sie mehrere Faktoren abhängig:

-   Was ist die Toleranz für nicht machen sich Objekte in Azure AD während der Ausfallzeit?
-   Akzeptieren Kennwortsynchronisation verwenden, die Benutzer, dass sie das alte Kennwort in Azure AD verwenden sie lokal ändern?
-   Haben Sie eine Abhängigkeit auf Echtzeit wie Kennwort Rückschreiben?

Je nach Antworten auf diese Fragen und Ihre Organisation kann eine der folgenden Strategien implementiert werden:

-   Erstellen Sie bei Bedarf neu.
-   Haben Sie einen Ersatzteil Standbyserver **staging-Modus**genannt.
-   Verwenden Sie virtuelle Computer.

Wenn Sie die integrierte SQL Express-Datenbank nicht verwenden, sollten Sie auch Abschnitt zur [Hohen Verfügbarkeit SQL](#sql-high-availability) überprüfen.

### <a name="rebuild-when-needed"></a>Erstellen Sie bei Bedarf neu.
Eine geeignete Strategie ist eine Server-Wiederherstellung bei Bedarf zu planen. Normalerweise installiert das Synchronisierungsmodul und führen anfänglichen Import und Synchronisierung innerhalb weniger Stunden abgeschlossen werden. Wenn ein Ersatzserver nicht verfügbar, kann vorübergehend einen Domänencontroller verwenden, um das Synchronisierungsmodul hosten.

Engine Synchronisierungsserver speichert keine Statusinformation über die Objekte, damit die Datenbank Daten in Active Directory und Azure AD erstellt werden kann. **SourceAnchor** -Attribut wird verwendet, Objekte aus lokalen und Cloud an. Wenn den Server vorhandenen Objekte lokal mit der Cloud neu erstellen, entspricht das Synchronisierungsmodul Objekte zusammen erneut auf Neuinstallation. Die Dinge zu dokumentieren sind die geänderte Konfiguration der Server wie filtern und Synchronisierung. Diese benutzerdefinierten Konfigurationen müssen Sie zunächst synchronisieren erneut angewendet werden.

### <a name="have-a-spare-standby-server---staging-mode"></a>Ein spare Standby-Server - staging-Modus
Wenn Sie eine komplexere Umgebung dann mindestens Standby-Server empfohlen wird. Während der Installation können Sie einen Server als der **Modus**aktivieren.

Weitere Informationen finden Sie in der [staging-Modus](#staging-mode).

### <a name="use-virtual-machines"></a>Virtuelle Computer
Allgemeine und unterstützte Methode werden das Synchronisierungsmodul auf einem virtuellen Computer ausgeführt. Der Host ein Problem hat, kann mit der Engine Synchronisierungsserver auf einen anderen Server migriert werden.

### <a name="sql-high-availability"></a>SQL-Verfügbarkeit
Wenn Sie nicht SQL Server Express, die mit Azure AD verbinden verwenden, sollten auch hoher Verfügbarkeit für SQL Server angesehen. Unterstützt nur Hochverfügbarkeits-Lösung ist SQL-clustering. Nicht unterstützte Lösungen umfassen Spiegelung und ständig.

## <a name="next-steps"></a>Nächste Schritte

**Themen (Übersicht)**  

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)  
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)  

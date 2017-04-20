<properties
   pageTitle="Azure AD Connect: Aktualisieren von einer früheren Version | Microsoft Azure"
   description="Erläutert die verschiedenen Methoden zum Aktualisieren auf die neueste Version von Azure Active Directory verbinden, einschließlich Upgrades und Migration des Swing."
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
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Aktualisierung von einer früheren Version auf die neueste
Dieses Thema beschreibt die verschiedenen Methoden können die Azure AD Connect-Installation auf die neueste Version aktualisieren. Halten Sie sich mit Versionen von Azure AD-Verbindung empfohlen. [Migration](#swing-migration) swing beschriebenen Schritte dienen auch bei wesentlichen Konfiguration ändern.

Aktualisieren von DirSync, finden Sie unter [Aktualisieren von Azure AD Synchronisierungsprogramm (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) statt.

Es gibt einige Strategien Azure AD Connect aktualisieren.

-Methode | Beschreibung
--- | ---
[Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) | Für Kunden mit einem express-Installation ist dies die einfachste Methode.
[In-Place-Aktualisierung](#in-place-upgrade) | Haben Sie einen einzelnen Server, aktualisieren Sie das Installation direkt auf dem gleichen Server.
[Swing-migration](#swing-migration) | Mit zwei Servern können Sie einen der Server mit der neuen Version oder Konfiguration vorbereiten und active Server ändern, wenn Sie bereit sind.

Erforderlichen Berechtigungen finden Sie unter [erforderliche Berechtigungen für Aktualisierung](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>In-Place-Aktualisierung
Ein direktes Upgrade ist für das Verschieben von Azure AD synchronisieren oder Azure AD verbinden. Es funktioniert nicht für DirSync oder mit FIM + Azure Active Directory Connector.

Diese Methode wird bevorzugt, wenn Sie einen einzelnen Server und weniger als 100.000 Objekten. Wenn alle Out-of-Box-Synchronisierungsregeln vollständigen Import und vollständige Synchronisierung nach der Aktualisierung auftreten gibt. Dadurch wird die neue Konfiguration auf alle Objekte im System angewendet wird. Dies kann abhängig von der Anzahl der Objekte im Bereich das Synchronisierungsmodul dauern. Normale Delta Synchronization Planer standardmäßig alle 30 Minuten wird unterbrochen, jedoch weiterhin Kennwortsynchronisation. Sie sollten die direkte Aktualisierung Wochenende. Wenn keine Out-of-Box-Konfiguration mit der neuen Azure AD verbinden gibt, wird eine normale Delta Import/Sync stattdessen gestartet.  
![In-Place-Aktualisierung](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Wenn Sie Out-of-Box-Regeln geändert haben, werden diese Standardkonfiguration Upgrade zurückgesetzt. Um sicherzustellen, dass Ihre Konfiguration zwischen Upgrades gehalten wird, sicherstellen Sie, dass die Änderungen gemäß [Best Practices für die Konfiguration ändern](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Swing-migration
Haben Sie eine komplexe Bereitstellung oder vielen Objekten kann es unpraktisch, ein direktes Upgrade auf live-System sein. Dies kann für einige Kunden mehrere Tage dauern, und während dieser Zeit keine deltaänderungen verarbeitet. Diese Methode wird auch verwendet, wenn Sie planen eine erhebliche Veränderung der Konfiguration und ausprobieren, bevor diese in die Cloud verschoben werden soll.

Für diese Szenarien empfiehlt es sich, eine Swing-Migration zu verwenden. Benötigen Sie (mindestens) zwei Server, einen aktiven und einem Testserver. Der aktive Server (durchgezogene blaue Linien im Bild unten) ist verantwortlich für die Produktion laden. Testserver (gestrichelte violette Linien im Bild unten) mit der neuen Version oder Konfiguration bereit und vollständig kann dieser Server ist verfügbar. Zum vorherige aktive Server mit der alten Version oder Konfiguration installiert, erfolgt die Testserver und aktualisiert.

Die beiden Server können verschiedene Versionen verwenden. Z. B. der aktive Server außer Betrieb genommen können Azure AD synchronisieren und neue Stagingserver Azure AD verbinden können. Verwenden Sie Swing-Migration zu einer neuen Konfiguration ist eine gute Idee, die gleichen Versionen auf zwei Servern.  
![Staging-server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Hinweis: Es wurde festgestellt, dass einige Kunden drei oder vier Server für dieses Szenario. Wenn Stagingserver aktualisiert wird, müssen Sie nicht bei [Disaster Recovery](active-directory-aadconnectsync-operations.md#disaster-recovery)einen backup-Server. Mit drei oder vier Servern kann eine Gruppe von Primär/Standby-Server mit der neuen Version vorbereitet werden sicherstellen, dass es immer ein Stagingserver bereit.

Diese Schritte kann auch zum Verschieben von Azure AD synchronisieren oder eine Lösung mit FIM + Azure Active Directory Connector. Diese Schritte funktionieren nicht für DirSync, aber die Swing Migration (so genannte parallele Bereitstellung) Methode mit Schritten zur DirSync finden Sie im [Upgrade Azure Active Directory (DirSync) synchronisieren](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Migrationsschritte swing

1. Verwenden von Azure AD Connect auf beiden Servern und möchten nur eine Änderung der Konfiguration active Server sicher und Stagingserver beide dieselbe Version verwenden. Das erleichtert später verglichen. Aktualisieren von Azure AD Sync haben diesen Servern verschiedene Versionen. Aktualisieren von einer älteren Version von Azure AD Connect ist eine gute Idee, mit beiden Servern mit der gleichen Version starten jedoch nicht.
2. Wenn einige benutzerdefinierte Konfiguration haben und Ihrem Entwicklungsserver nicht hat, gehen Sie unter [Konfiguration von aktiv in staging-Server verschieben](#move-custom-configuration-from-active-to-staging-server).
3. Aktualisieren von einer früheren Version von Azure AD Connect zu aktualisieren Sie Stagingserver auf die neueste Version. Verschieben von Azure AD Sync installieren Sie Azure AD Connect auf Ihrem Entwicklungsserver.
4. Können Sie das Synchronisierungsmodul vollständiger Import und vollständige Synchronisierung auf Ihrem Entwicklungsserver ausführen.
5. Überprüfen Sie, ob die neue Konfiguration [Überprüfen Sie die Konfiguration eines Servers](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)die Schritte unter **Überprüfen** bei unerwarteten Änderungen selbst verursacht hat. Wenn etwas ist nicht wie erwartet, zur korrekten Import und Synchronisierung und überprüfen, bevor die Daten gut aussieht. Folgendermaßen können in verknüpften Themen gefunden.
6. Testserver active Server zu wechseln. Dies ist der letzte Schritt **wechseln active Server** in [Überprüfen Sie die Konfiguration eines Servers](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Wenn Sie Azure AD Connect aktualisieren, aktualisieren Sie den Server jetzt im staging-Modus, um die neueste Version. Folgen Sie denselben Schritten der Daten und Konfiguration aktualisiert. Wenn Sie Azure AD-Synchronisierung aktualisiert haben, können jetzt deaktivieren und Ihre alten Server außer Betrieb setzen.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Konfiguration von aktiv in staging-Server verschieben
Wenn Sie Konfiguration der aktive Server geändert haben, müssen Sie sicherstellen, dass die gleiche Änderung auf dem Stagingserver angewendet werden.

Die erstellten benutzerdefinierten Synchronisierungsregeln können mit PowerShell verschoben. Andere muss auf beiden Systemen angewendet werden und kann nicht migriert werden.

Was müssen Sie sicherstellen, ist auf beiden Servern konfiguriert:

- Verbindung mit den gleichen Gesamtstrukturen.
- Jede Domäne und Organisationseinheit filtern.
- Die gleichen optionalen Funktionen wie Sync Kennwort und Kennwort Rückschreiben.

**Regeln für das Verschieben**  
Um eine benutzerdefinierte Synchronisation Regel zu verschieben, führen Sie folgende Schritte aus:

1. Öffnen Sie **Synchronisierung Regel-Editor** auf dem aktiven Server.
2. Wählen Sie die benutzerdefinierte Regel. Klicken Sie auf **Exportieren**. Dadurch wird ein Editor-Fenster. Speichern der temporären Datei mit der Erweiterung PS1. Dadurch ein PowerShell-Skript. Kopieren Sie die Datei ps1 Stagingserver.  
![Sync Regel exportieren](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Die Connector-GUID wird auf dem Stagingserver und muss geändert werden. Um die GUID abzurufen, starten Sie **Synchronisierung Regel-Editor**zu, wählen Sie eine Out-of-Box-Regeln denselben verbundene System darstellt und klicken Sie auf **Exportieren**. Ersetzen Sie die GUID in der PS1-Datei durch die GUID vom Testserver.
4. Führen Sie eine Aufforderung PowerShell PS1-Datei. Dadurch wird die Regel benutzerdefiniertes auf dem Testserver erstellt.
5. Haben Sie mehrere benutzerdefinierte Regeln, wiederholen Sie für alle benutzerdefinierten Regeln.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

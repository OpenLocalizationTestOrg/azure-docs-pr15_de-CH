<properties
    pageTitle="Azure AD Connect-Synchronisierung: Synchronisierung Service Manager-UI | Microsoft Azure"
    description="Verstehen der Registerkarte Connectors die Synchronisierung Service Manager Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect-Synchronisierung: Synchronisierung Service Manager

[Vorgänge](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Anschlüsse](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse-Suche](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Die Registerkarte Connectors zum alle Systeme verwalten, denen das Synchronisierungsmodul verbunden ist.

## <a name="connector-actions"></a>Connector-Aktionen

Aktion | Kommentar
--- | ---
Erstellen | Verwenden Sie nicht. Verwenden Sie für die Verbindung mit zusätzlichen Active Directory-Gesamtstrukturen der Installations-Assistent.
Eigenschaften | Für Domäne und Organisationseinheit Filtern verwendet.
[Löschen](#delete) | Löschen entweder die Daten in den Connectorspace oder Verbindung zu einer Gesamtstruktur löschen verwendet.
[Ausführungsprofile konfigurieren](#configure-run-profiles) | Außer Domäne filtern hier konfiguriert. Dadurch können Sie bereits konfigurierten Profile führen anzuzeigen.
Ausführen | Zum einmaligen Ausführen eines Profils zu starten.
Beenden | Beendet einen aktiven Profil Connector.
Verbindung exportieren | Verwenden Sie nicht.
Verbindung importieren | Verwenden Sie nicht.
Connector aktualisieren | Verwenden Sie nicht.
Schema aktualisieren | Aktualisiert das zwischengespeicherte Schema. Es ist die Option im Installations-Assistenten verwenden Sie da auch Updates Regeln synchronisieren.
[Connectorspace suchen](#search-connector-space) | Objekte verwendet und [ein Objekt](#follow-an-object-and-its-data-through-the-system)und seine Daten über das System.

### <a name="delete"></a>Löschen
Die Löschaktion wird für zwei verschiedene Dinge verwendet.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Die Option **nur Connectorspace löschen** entfernt alle Daten aber behalten Sie die Konfiguration.

Die Option **Löschen und Connectorspace** entfernt die Daten und die Konfiguration. Diese Option wird verwendet, wenn Sie einer Gesamtstruktur mehr herstellen möchten.

Beide Optionen alle Objekte synchronisieren und Aktualisieren der Metaverse-Objekte. Diese Aktion ist eine umfangreiche Operation.

### <a name="configure-run-profiles"></a>Ausführungsprofile konfigurieren
Diese Option können Sie die Laufzeit Profile für einen Connector konfiguriert.

![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Connectorspace suchen
Die Suchaktion Connector Space ist sinnvoll, Objekte suchen und Beheben von Problemen.

![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Starten Sie den **Bereich**. Suchen Sie basierend auf Daten (RDN, DN Anker, Teilstruktur), oder das Objekt (alle anderen Optionen).  
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Wenn Sie beispielsweise eine Teilstruktur Suche, erhalten Sie alle Objekte in einer Organisationseinheit.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) in diesem Raster können Sie auswählen, ein Objekt **Eigenschaften**und aus dem Quell-Connectorspace Metaverse und Ziel-Connectorspace [folgen](#follow-an-object-and-its-data-through-the-system) .

## <a name="follow-an-object-and-its-data-through-the-system"></a>Führen Sie ein Objekt und seine Daten über das system
Bei der Problembehandlung ein Problem mit Daten führen Sie ein Objekt aus dem Connectorspace Quelle in Metaverse und zum Ziel Connectorspace ist Schlüssel zu verstehen, warum Daten nicht die erwarteten Werte haben.

### <a name="connector-space-object-properties"></a>Connector Space Objekteigenschaften
**Importieren**  
Wenn Sie ein Cs-Objekt öffnen, werden mehrere Registerkarten oben Die Registerkarte **Importieren** zeigt die Daten, die nach einem Import bereitgestellt.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) den **Alten Wert** gezeigt, was derzeit gespeichert ist, und der **Neue Wert** was aus dem Quellsystem eingegangen und wurde noch nicht angewendet. In diesem Fall ein Fehler gibt, kann die Änderung angewendet werden.

**Fehler**  
Die Fehlerseite wird nur angezeigt, wenn es ein Problem mit dem Objekt. Einzelheiten Sie auf der Seite "Vorgänge" für Weitere Informationen zur [Problembehandlung bei Fehler](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Linie**  
Die Registerkarte Linie zeigt wie das Connectorspace-Objekt zum Metaverse-Objekt verknüpft ist. Sie sehen der Connector importiert zuletzt geändert vom verbundenen System und die Regeln zum Auffüllen der Daten im Metaverse.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) In der Spalte **Aktion** können Sie sehen, gilt eine **eingehende** Synchronisierung mit der **Bereitstellung**. Als dieses Connectorspace-Objekt vorhanden ist, bleibt das metaverseobjekt angibt. Wenn Synchronisierungsregeln stattdessen Synchronisierungsregel **Richtung **ausgehend** und**angezeigt werden, bedeutet dies, dass dieses Objekt gelöscht wird, wenn das Metaverse-Objekt gelöscht wird.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) auch sehen in der Spalte **PasswordSync** , dass eingehende Connectorspace ändert das Kennwort beitragen kann, da eine Synchronisierungsregel den Wert **True**hat. Dieses Kennwort wird dann an Azure AD über ausgehende Regel gesendet.

Auf der Registerkarte Linie erhalten in das Metaversum Sie durch Klicken auf [Metaverse-Objekteigenschaften](#metaverse-object-properties).

Am Ende aller Registerkarten befinden sich zwei Schaltflächen: **Vorschau** und **Protokolldateien**.

**Vorschau**  
Die Vorschauseite wird zum Objekt synchronisiert. Es empfiehlt sich, einige Kunden Synchronisierungsregeln Problembehandlung und eine Änderung auf ein einzelnes Objekt anzeigen möchten. Sie können zwischen **Vollständigen Synchronisierung** und **Delta-Synchronisierung**auswählen. Sie können auch **Vorschau generieren**hält nur die Änderung im Speicher, bis **Commit Vorschau**, welche alle Phasen Ziel Connector Räumen ändert.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) können Sie das Objekt und die für einen bestimmten Attributfluss angewendete Regel überprüfen.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Protokoll**  
Die Seite Protokoll wird zum Kennwort Synchronisierungsstatus und Verlauf [Kennwortsynchronisation Problembehandlung](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) Weitere Informationen finden Sie unter.

### <a name="metaverse-object-properties"></a>Metaverse-Objekt: Eigenschaften
**Attribute**  
Auf der Registerkarte Attribute können Sie Werte und Anschluss trug er.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**Connectors**  
Die Registerkarte Connectors Zeigt alle Connector Räume, die eine Darstellung des Objekts.
![Sync-Dienst-Manager](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) dieser Registerkarte können Sie das [Connectorspace-Objekt](#connector-space-object-properties)navigieren.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

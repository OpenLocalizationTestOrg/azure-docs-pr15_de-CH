<properties
    pageTitle="Azure AD Verbindung synchronisieren: Best Practices für die Konfiguration ändern | Microsoft Azure"
    description="Enthält bewährte Methoden zum Ändern der Standardkonfiguration von Azure AD-Verbindung synchronisieren."
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
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Verbindung synchronisieren: Best Practices für die Konfiguration ändern
Dieses Thema ist unterstützte und nicht unterstützte Änderungen Azure AD Connect Sync beschrieben.

Die Konfiguration von Azure AD Connect erstellt funktioniert "" für die meisten Umgebungen, die lokale Active Directory Azure AD synchronisieren. In einigen Fällen ist es jedoch notwendig, Ändern einer Konfiguration, um eine bestimmte Anforderung zu erfüllen.

## <a name="changes-to-the-service-account"></a>Ändern des Dienstkontos
Azure AD Connect Sync läuft unter einem Dienstkonto, das vom Assistenten erstellt. Dieses Dienstkonto enthält die Verschlüsselungsschlüssel für die Datenbank synchronisieren. Mit einem 127 Zeichen langes Kennwort erstellt, und das Kennwort ohne Ablaufdatum festgelegt ist.

- **Nicht unterstützte** ändern oder Zurücksetzen des Kennworts des Dienstkontos ist. Damit die Verschlüsselungsschlüssel zerstört und der Dienst ist nicht auf die Datenbank zugreifen und kann nicht starten.

## <a name="changes-to-the-scheduler"></a>Ändern der Planer
Ab den Versionen Build 1.1 (Februar 2016) kann der [Planer](active-directory-aadconnectsync-feature-scheduler.md) zu anderen Sync Zyklus als den Standardwert 30 Minuten konfigurieren.

## <a name="changes-to-synchronization-rules"></a>Regeln ändern
Der Assistent bietet eine Konfiguration, die für die häufigsten Szenarien funktioniert. Falls Sie die Konfiguration ändern möchten, müssen Sie diese Regeln immer noch eine unterstützte Konfiguration befolgen.

- Sie können [Ändern Attribut Flows](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) Flows direkte Attribut standardmäßig nicht für Ihr Unternehmen geeignet sind.
- Wenn Sie [fließen kein Attribut](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) und vorhandener Attributwerte in Azure AD entfernen, müssen Sie eine Regel für dieses Szenario erstellen.
- [Eine unerwünschte Synchronisierungsregel deaktivieren](#disable-an-unwanted-sync-rule) , statt Sie löschen. Während der Aktualisierung wird eine gelöschte Regel neu erstellt.
- [Eine der vordefinierten Regel ändern](#change-an-out-of-box-rule)sollten Sie eine Kopie der ursprünglichen Regel und Out-of-Box-Regel deaktivieren. Sync Regeleditor fordert und erleichtert.
- Exportieren Sie Ihre benutzerdefinierten Regeln für die Synchronisierung Regel-Editor. Der Editor stellt ein PowerShell-Skript, mit leicht in einem Systemausfall neu erstellen.

>[AZURE.WARNING] Die Out-of-Box-Synchronisierung haben eines Fingerabdrucks. Wenn Sie diese Regeln ändern, wird der Fingerabdruck nicht entsprechen. Wenn Sie versuchen, eine neue Version von Azure AD Connect zukünftig möglicherweise Probleme auftreten. Nur Änderungen Sie wie beschrieben in diesem Artikel.

### <a name="disable-an-unwanted-sync-rule"></a>Eine unerwünschte Synchronisierungsregel deaktivieren
Eine Out-of-Box Synchronisierungsregel nicht gelöscht. Es ist während der nächsten Aktualisierung neu.

In einigen Fällen produziert der Installations-Assistent eine Konfiguration, die für Ihre Topologie nicht funktionieren. Beispielsweise werden Wenn Sie eine Konto-Topologie wurden das Schema in der Gesamtstruktur mit dem Exchange-Schema erweitert, dann Regeln für Exchange für die Gesamtstruktur und der Gesamtstruktur erstellt. In diesem Fall müssen Sie für Exchange Sync-Regel deaktivieren.

![Deaktivierte Synchronisierungsregel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

In der obigen Abbildung hat der Installationsassistent alte Exchange 2003-Schema in der Gesamtstruktur gefunden. Dieses schemaerweiterung wurde hinzugefügt, bevor die Gesamtstruktur Fabrikam Umgebung eingeführt wurde. Um sicherzustellen, dass keine Attribute aus der alten Exchange-Implementierung synchronisiert werden, sollte die Synchronisierungsregel Siehe deaktiviert werden.

### <a name="change-an-out-of-box-rule"></a>Eine Out-of-Box-Regel ändern
Benötigen Sie eine Out-of-Box-Regel ändern, sollten Sie eine Kopie der Out-of-Box-Regel und deaktivieren Sie die Originalregel. Die geklonte Regel dann die Änderungen vornehmen. Der Regeleditor Sync hilft Ihnen mit diesen Schritten. Beim Öffnen einer Out-of-Box-Regel wird dieses Dialogfeld angezeigt:  
![Warnung aus Feld Regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Wählen Sie **Ja** , erstellen Sie eine Kopie der Regel. Die geklonte Regel wird geöffnet.  
![Geklonte Regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Diese Regel geklonte Änderungen Sie erforderlichen Umfang, Verknüpfung und Transformationen.

## <a name="next-steps"></a>Nächste Schritte

**Themen (Übersicht)**

- [Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen](active-directory-aadconnectsync-whatis.md)
- [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

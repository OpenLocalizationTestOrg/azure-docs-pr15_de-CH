<properties
    pageTitle="Roaming in Azure Active Directory Enterprise Aktivierungsstatus | Microsoft Azure"
    description="Häufig gestellte Fragen zu Enterprise Zustand Roamingeinstellungen in Windows-Geräte. Enterprise Zustand bietet Benutzern eine einheitliche Verwendung auf Windows-Geräten und verringert die Zeit für ein neues Gerät konfigurieren."
    services="active-directory"
    keywords="Enterprise-Zustand roaming Windows Cloud Enterprise Zustand roaming aktivieren"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Enterprise-Aktivierungsstatus in Azure Active Directory Roaming

Enterprise Zustand Roaming steht für eine Organisation mit einem Abonnement Premium Azure Active Directory (Azure AD). Weitere Informationen zu Azure AD-Abonnement finden Sie unter [Azure AD-Produktseite](https://azure.microsoft.com/services/active-directory).

Wenn Sie Enterprise Zustand Roaming aktivieren, Ihre Organisation automatisch Lizenzen für kostenlose, eingeschränktes Abonnement RMS erhalten. Diese Testversion ist auf verschlüsseln und Entschlüsseln von Enterprise-Einstellungen und Daten vom Enterprise Zustand Roaming-Dienst synchronisiert; Sie müssen ein bezahltes Abonnement für alle Funktionen von RMS verwenden.

Nach Erhalt einer Azure AD Premium-Abonnement, um Enterprise Zustand Roaming aktivieren folgendermaßen Sie vor:

1. Bei klassischen Azure-Portal.
2. Wählen Sie auf der linken Seite **ACTIVE DIRECTORY**, und wählen Sie das Verzeichnis für das Enterprise Zustand Roaming aktiviert werden soll.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Wechseln Sie zur Registerkarte **Konfigurieren** oben.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Unten auf der Seite Wählen Sie **Benutzer und ENTERPRISE-APP Daten SYNCHRONISIEREN können**, und klicken Sie auf **Speichern**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Ein Windows 10 mit Service Enterprise Zustand Roaming Roaming muss das Gerät mit einer Azure AD-Identität authentifizieren. Für Geräte, die in Azure Active Directory verbunden sind, ist der primäre Benutzername Azure AD-Identität, keine weitere Konfiguration erforderlich ist. Für Geräte, die einen herkömmlichen lokalen Active Directory verwenden, muss der IT-Administrator [Geräte Domäne Azure AD für Windows 10 Erfahrungen](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Sync-Datenspeicher
Enterprise Zustand Roaming-Daten werden in [Azure Regionen](https://azure.microsoft.com/regions/ ) gehostet, das am besten mit dem Land Wert in Azure Active Directory-Instanz ausgerichtet. Enterprise Zustand Roaming-Daten basierend auf drei große geografische Regionen unterteilt: Nordamerika, EMEA und APAC. Enterprise Zustand Roaming-Daten für den Mandanten ist lokal mit geografischen Region und Regionen nicht repliziert.  Beispielsweise Kunden, die deren Land Wert eines EMEA-Ländern haben, wie "Frankreich" oder "Sambia" werden die Daten in einem oder Azure Regionen Europa gehostet.  Kunden, die ihren Wert in Azure AD eines Nordamerika Ländern legen, wie "USA" oder "Kanada" ihre Daten in einer oder mehreren Azure Regionen in den USA gehostet.  Kunden, die ihren Wert in Azure AD eines APAC-Länder festgelegt, wie "Australien" oder "Neuseeland" ihre Daten in eine oder mehrere Regionen Asien Azure gehostet.  Länder und Antarktis-Daten werden in eine oder mehrere Azure-Regionen in den USA gehostet werden.  Der Wert wird als Teil des Erstellungsprozesses Azure AD-Verzeichnis festgelegt und kann nicht nachträglich geändert werden. 

Benötigen Sie weitere Informationen zum Speicherort Datei Sie einen Ticket [Azure unterstützt](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Verwalten von Enterprise-Zustand Roaming
Azure AD globale Administratoren können aktivieren und Deaktivieren von Enterprise Zustand Roaming im klassischen Azure-Portal.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globale Administratoren können Einstellungen synchronisieren auf bestimmte Sicherheitsgruppen beschränken.

Globale Administratoren können auch benutzerspezifische Gerät synchronisieren Statusbericht anzeigen, einen bestimmten Benutzer in der Liste **Benutzer** Active Directory auswählen klicken Sie auf die Registerkarte **Geräte** und **Geräte synchronisieren Einstellungen und Unternehmensdaten app**Ansicht auswählen.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Datenaufbewahrung
Azure über Enterprise Zustand Roaming synchronisierte Daten unbegrenzt erhalten ein manuelle Löschvorgang ausgeführt oder Daten zu veralteten bestimmt. 

**Explizit löschen:** Die Daten werden gelöscht, Azure Administrator einen Benutzer oder ein Verzeichnis gelöscht oder Administrator fordert explizit gelöscht werden soll.

- **Benutzer löschen**: Wenn ein Benutzer in Azure AD gelöscht wird, das Benutzerkonto Roamingdaten zum Löschen markiert und zwischen 90 und 180 Tagen gelöscht werden. 
- **Verzeichnis löschen**: Löschen ein ganzes Verzeichnis in Azure AD sofort betriebsbereit ist. Alle Einstellungsdaten verknüpft mit Verzeichnis zum Löschen markiert und zwischen 90 und 180 Tagen gelöscht werden. 
- **Auf Anforderung löschen**: will der Azure AD Administrator Daten oder Einstellungen des betreffenden Benutzers löschen, kann der Administrator einen Ticket mit [Azure unterstützt](https://azure.microsoft.com/support/)Datei. 

**Veraltete Daten löschen**: Daten, die nicht für ein Jahr ("Aufbewahrungsdauer") als veraltet angesehen und kann von Azure gelöscht zugegriffen wurde. Die Aufbewahrungsdauer ändern ist jedoch nicht weniger als 90 Tagen. Veralteten Daten festgelegt der Windows-Anwendung Einstellungen oder alle Benutzer bestimmte. Zum Beispiel:
 
- Wenn keine Geräte eine bestimmten Auflistung zugreifen (z. B. eine Anwendung vom Gerät entfernt oder Einstellungsgruppe wie "Thema" für alle Geräte eines Benutzers deaktiviert ist), und diese Auflistung nach der Aufbewahrungsdauer werden veraltete kann gelöscht werden. 
- Wenn Benutzer Einstellungen synchronisieren auf seine Geräte deaktiviert hat, dann Einstellungen Daten zugegriffen wird und Einstellungsdaten für diesen Benutzer ist veraltet und können gelöscht werden, nachdem die Aufbewahrungsdauer. 
- Azure AD-Verzeichnis Admin Enterprise Zustand Roaming für das gesamte Verzeichnis, dann alle Benutzer abschaltet, Directory stoppt Einstellungen synchronisieren und sämtliche Einstellungsdaten für alle Benutzer ist veraltet und können gelöscht werden, nachdem die Aufbewahrungsdauer. 

**Gelöschte Daten-Recovery**: die Datenaufbewahrungsrichtlinie ist nicht konfigurierbar. Sobald die Daten endgültig gelöscht wurde, werden wiederhergestellt. Allerdings ist es wichtig zu beachten, dass die Einstellungsdaten nur von Azure nicht das Endgerät gelöscht. Wenn ein Gerät später Enterprise Zustand Roaming Service verbindet, werden die Einstellung erneut synchronisiert und gespeichert werden in Azure.


## <a name="related-topics"></a>Verwandte Themen
- [Übersicht über die Enterprise Zustand Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
- [Einstellungen und roaming-FAQ](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Group Policy und MDM Einstellungen Einstellungen synchronisieren](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10 roaming Settings reference](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)

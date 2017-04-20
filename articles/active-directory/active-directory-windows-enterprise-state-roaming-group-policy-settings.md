<properties
    pageTitle="Gruppieren von Richtlinien-und MDM | Microsoft Azure"
    description="Informationen zu Gruppenrichtlinien und Mobilgerät (MDM) Einstellungen, die auf firmeneigenen Geräten verwendet werden soll. Diese Richtlinien werden auf ganze Gerät des Benutzers angewendet."
    services="active-directory"
    keywords="Was sind Richtlinien und MDM Einstellungen für Enterprise Zustand Roaming Enterprise Zustand Roaming, Windows Cloud"
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

# <a name="group-policy-and-mdm-settings"></a>Gruppenrichtlinien und MDM settings

Verwenden Sie diese Gruppenrichtlinie und Management (MDM) Einstellungen auf firmeneigenen Geräten diese Richtlinien auf ganze Gerät des Benutzers angewendet werden. Ein MDM Richtlinien Einstellungen Sync für persönliche deaktivieren, wird im Benutzerbesitz Gerät dieses Gerät negativ. Darüber hinaus sein andere Benutzerkonten auf dem Gerät auch die Richtlinie betroffen.

Unternehmen, die zu verwaltenden roaming für PDAs (nicht verwalteten) können Azure-Portal aktivieren oder Deaktivieren von roaming anstelle der Verwendung von Gruppenrichtlinien oder MDM
Die folgenden Tabellen beschreiben die Richtlinien zur Verfügung.

## <a name="mdm-settings"></a>MDM-Einstellung
MDM Richtlinien gelten für Windows 10 und Windows 10 Mobile.  Windows 10 Mobile Unterstützung ist nur für microsoftkonto über OneDrive Benutzerkonto roaming.  Siehe Abschnitt "Geräte und Endpunkte" Informationen zum Synchronisieren von Azure Anzeige welche Geräte unterstützt werden.

| Name                               | Beschreibung                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Microsoft-Konto-Verbindung zulassen | Ermöglicht Benutzern die Authentifizierung mit einem microsoftkonto auf dem Gerät |
| Sync Einstellungen zulassen             | Können Benutzer Windows-Einstellungen und app-Daten zugreifen. Durch Deaktivieren dieser Richtlinie deaktiviert Synchronisierung sowie auf mobile Geräte                  |

## <a name="group-policy-settings"></a>Gruppenrichtlinien
Die Gruppenrichtlinien gelten für Windows 10-Geräte, die Active Directory-Domäne angehören. Die Tabelle enthält auch ältere Einstellungen Synchronisierungseigenschaften verwalten scheint aber nicht funktionieren für Enterprise Zustand Roaming für Windows 10, die mit "In der Beschreibung verwenden Sie nicht" sind.

| Name                                | Beschreibung |
|-------------------------------------|-------------|
| : Block Microsoft Konten  |Diese Einstellung verhindert, dass Benutzer neue Microsoft-Konten auf diesem Computer hinzufügen|
| Nicht synchronisiert                         |Verhindert, dass Benutzer Windows-Einstellungen und app-Daten zugreifen|
| Keine Synchronisierung personalisieren             |Deaktiviert die Synchronisierung Designs Gruppe|
| Browsereinstellungen nicht synchronisiert        |Deaktiviert die Synchronisierung der Internet Explorer-Gruppe|
| Kennwörter nicht synchronisieren               |Deaktiviert die Synchronisierung Kennwörter Gruppe|
| Weitere Windows nicht synchronisiert  |Weitere Fenster Einstellungsgruppe synchronisieren deaktiviert|
| Desktop-Anpassung nicht synchronisiert |Verwenden Sie nicht. hat keine Auswirkung|
| Nicht auf gemessenen Verbindung synchronisieren  |Deaktiviert auf gemessene Verbindungen wie cellular 3G|
| Apps nicht synchronisiert                    |Verwenden Sie nicht. hat keine Auswirkung|
|App-Einstellungen nicht synchronisiert             |Deaktiviert das roaming von Anwendungsdaten|
|Start-Einstellungen nicht synchronisiert           |Verwenden Sie nicht. hat keine Auswirkung|


## <a name="related-topics"></a>Verwandte Themen
- [Übersicht über die Enterprise Zustand Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
- [Aktivieren Sie Enterprise-Status in Azure Active Directory roaming](active-directory-windows-enterprise-state-roaming-enable.md)
- [Einstellungen und roaming-FAQ](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows 10 roaming Settings reference](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)

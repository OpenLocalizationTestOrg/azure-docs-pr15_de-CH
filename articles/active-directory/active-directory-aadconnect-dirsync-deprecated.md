<properties
    pageTitle="Aktualisieren von DirSync und Azure AD synchronisieren | Microsoft Azure"
    description="Beschreibt die Aktualisierung von DirSync und Azure AD Sync Azure AD verbinden."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Aktualisieren Sie Windows Azure Active Directory-Synchronisierung ("DirSync") und Azure Active Directory-Synchronisierung ("Azure AD synchronisieren")
Azure AD Connect ist am besten verbinden Sie das lokale Verzeichnis mit Azure und Office 365. Dies ist der ideale Zeitpunkt, wie diese Tools jetzt veraltet sind und werden Ende April 13, 2017 zu Azure AD Verbinden von Windows Azure Active Directory-Synchronisierung (DirSync) oder Azure AD-Synchronisierung aktualisieren.

Zwei Identität-Synchronisierungsprogramme, die veraltet sind Gesamtstruktur-Kunden (DirSync) angeboten und für Gesamtstrukturen und andere fortgeschrittene Anwender (Azure AD-Synchronisierung). Diese älteren Tools wurde durch eine einzige Lösung für alle Szenarios ersetzt: Azure AD verbinden. Es bietet neue Funktionen, verbesserten Features und Unterstützung für neue Szenarien. Auf Ihrem lokalen Identitätsdaten Azure AD synchronisieren können und Office 365, wir empfehlen die Aktualisierung auf Azure AD verbinden.

Letzte Release DirSync kam im Juli 2014 und die letzte Version von Azure AD Sync wurde im Mai 2015 veröffentlicht.

## <a name="what-is-azure-ad-connect"></a>Was ist Azure AD verbinden
Azure AD Connect ist der Nachfolger von DirSync und Azure AD synchronisieren. Alle Szenarien kombiniert diese beiden unterstützt. Erfahren Sie mehr darüber in [der lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Veraltete Objekte planen

Datum | Kommentar
 --- | ---
13 April 2016 | Windows Azure Active Directory-Synchronisierung ("DirSync") und Microsoft Azure Active Directory-Synchronisierung ("Azure AD synchronisieren") bekannt sind, als veraltet.
13 April 2017 | Unterstützung endet. Kunden werden nicht mehr eine Support-Anfrage öffnen ohne Azure AD Connect zuerst aktualisieren.

## <a name="how-to-transition-to-azure-ad-connect"></a>Umstellung auf Azure AD verbinden
Wenn Sie auf zwei Arten Sie aktualisieren DirSync ausführen: In-Place-Upgrade und parallele Bereitstellung. Für die meisten Kunden ein direktes Upgrade empfohlen wird und wenn Sie eine Betriebssystem und weniger als 50.000 Objekte. In anderen Fällen wird empfohlen, eine parallele Bereitstellung führen, wo die Dirsync-Konfiguration auf einem neuen Server mit Azure AD Connect verschoben.

Bei Verwendung von Azure AD Sync ist ein direktes Upgrade empfohlen. Wenn Sie möchten, kann einen neuen Azure AD Connect Server parallel installieren und eine Swing-Migration von Azure AD Sync-Server Azure AD-Verbindung.

Lösung | Szenario
----- | -----
[Aktualisieren von DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Wenn Sie einen vorhandenen Dirsync-Server bereits ausgeführt haben.</li>
[Aktualisieren von Azure AD-Synchronisierung](active-directory-aadconnect-upgrade-previous-version.md)| <li>Wenn Sie von Azure AD Sync verschieben.</li>

Wenn Sie wie ein direktes Upgrade von DirSync Azure AD verbinden wollen, lesen Sie in diesem Channel 9 Video:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>Häufig gestellte Fragen
**F: Ich habe eine e-Mail-Benachrichtigung von Azure Team oder eine Nachricht von der Office 365-Nachrichtenstatus, aber ich verwende verbinden.**  
Die Benachrichtigung wurde auch eine Buildnummer 1.0 Azure AD Verbinden mit Kunden. \*.0 (mit einer Version vor 1.1). Microsoft empfiehlt Kunden mit Azure AD Connect-Versionen. Mit 1.1 Funktion für die [Automatische Aktualisierung](active-directory-aadconnect-feature-automatic-upgrade.md) es machen installiert einfach immer eine aktuelle Version von Azure AD Connect.

**F: wird DirSync-Azure AD-Synchronisierung unterbrechen 13 April 2017?**  
Nein. Wenn diese nicht mehr mit Azure AD werden das Datum wird zu einem späteren Zeitpunkt bekannt gegeben. Sie werden finden diese Informationen in diesem Thema beim verfügbar.

**F: werden welche Dirsync-Versionen können aus aktualisiert?**  
Es werden alle derzeit verwendeten Dirsync-Version aktualisieren.

**F: Azure AD Connector für FIM-MIM?**  
Azure AD Connector **für FIM-MIM wurde** als veraltet markiert. Es ist **Funktion fixieren**. keine neuen Funktionen hinzugefügt und erhält keine Fehlerkorrekturen. Microsoft empfiehlt Kunden mit Azure AD verbinden aus verschieben möchten. Es wird empfohlen alle Neuinstallationen verwendet wird nicht gestartet. Dieser Connector wird in Zukunft veraltet vorgestellt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

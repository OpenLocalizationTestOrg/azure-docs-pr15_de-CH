<properties
    pageTitle="Legen Sie Device Grundlage bedingter Richtlinien für Applikationen Azure Active Directory verbunden | Microsoft Azure"
    description="Festlegen Sie Richtlinien für Azure Active Directory verbunden Applications Gerät bedingten Zugriff."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Legen Sie Device Grundlage bedingter Richtlinien für Applikationen Azure Active Directory verbunden


Azure Active Directory (Azure AD) Gerät bedingten Zugriff helfen Unternehmen Ressourcen schützen:

- Zugriffsversuche von unbekannten oder nicht verwalteten Geräte.
- Geräte, die nicht die Sicherheitsrichtlinien des Unternehmens entsprechen.

Eine Übersicht über die Zugangskontrolle anzeigen Sie [Zugangskontrolle Azure Active Directory](active-directory-conditional-access.md)

Sie können Device Grundlage bedingter Richtlinien zu diesen festlegen:

- Office 365 SharePoint Online zu Ihrer Organisation Websites und Dokumente
- Office 365 Exchange Online zu Ihrer Organisation
- Software als Service (SaaS) einer Anwendung, die die Azure AD für Authentifizierung verbunden sind
- Lokale Anwendung, die mithilfe von Azure AD-Anwendungsproxy Services veröffentlicht werden

Gehen Sie zum Festlegen einer Richtlinie Gerät bedingten Zugriff im Azure-Portal für die spezifische Anwendung im Verzeichnis.


  ![Liste der Programme im Azure Portal Verzeichnis] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Applikationen")


Wählen Sie die Anwendung, und klicken Sie dann auf die Registerkarte **Konfigurieren** , um die bedingte Richtlinie festzulegen.  


  ![Konfigurieren der Anwendung] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Device Grundlage Zugriffsregeln")




Eine Richtlinie Gerät bedingten Zugriff im Abschnitt **Device Grundlage Zugriffsregeln** **Zugriffsregeln aktivieren**soll **auf**.

Gerät-basierten bedingte Zugriffsrichtlinie besteht aus drei Komponenten:

- **Zuweisen**. Der Bereich der Benutzer, denen die Richtlinie gilt.

- **Regeln**. Die Bedingung muss ein erfüllen, bevor die Anwendung zugreifen kann.

- **Anwendung erzwingen**. Clientanwendungen (nativ und Browser) wendet die Richtlinie an.

  ![Die drei Komponenten einer Richtlinie Gerätebasis] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Device Grundlage Zugriffsregeln")


## <a name="select-the-users-the-policy-applies-to"></a>Wählen Sie die Benutzer, denen die Richtlinie gilt

Im Abschnitt **Anwenden** können Sie den Bereich der Benutzer auswählen, für die, denen diese Richtlinie gilt.

Beim Erstellen einer Access Policy Bereichs für Benutzer stehen Ihnen zwei Optionen zur Verfügung:

- **Alle Benutzer**. Wenden Sie die Richtlinie auf alle Benutzer, die Zugriff auf die Anwendung.

- **Gruppen**. Beschränken Sie die Richtlinie für Benutzer, die Mitglied einer bestimmten Gruppe sind.

![Übernehmen der Richtlinie für alle Benutzer oder einer Gruppe] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Anwenden auf")


 Um einen Benutzer aus einer Richtlinie auszuschließen, aktivieren Sie das Kontrollkästchen **außer** . Dies ist hilfreich, wenn Sie Berechtigungen für einen bestimmten Benutzer vorübergehend Zugriff auf die Anwendung müssen. Wählen Sie diese Option beispielsweise, wenn einige Benutzer Geräte, die nicht für bedingten Zugriff bereit. Die Geräte möglicherweise nicht registriert oder sie sind nicht kompatibel.


## <a name="select-the-conditions-that-devices-must-meet"></a>Wählen Sie die Geräte erfüllen müssen

Verwenden Sie **Regeln** , die ein Gerät festzulegen erfüllen müssen, um Zugriff auf die Anwendung.

Sie können für solche Geräte Gerät bedingten Zugriff festlegen:

- Windows 10 Jahre Update Windows 8.1 und Windows 7
- WindowsServer 2016, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2
- iOS-Geräte (iPad, iPhone)
- Android-Geräte

Unterstützung für Mac kommt bald.

  ![Richtlinie für Geräte übernehmen] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Applikationen")

 >[AZURE.NOTE] Informationen zu den Unterschieden zwischen Domäne und Azure AD verknüpft finden Sie [verwenden Windows 10 Geräte am Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).

Sie haben zwei Optionen für Regeln:

- **Alle Geräte kompatibel sein müssen**. Alle Plattformen, die die Anwendung zugreifen müssen kompatibel sein. Auf Plattformen ausgeführt werden, die Geräte bedingten Zugriff nicht unterstützen Geräten der Zugriff verweigert.

- **Nur ausgewählte Geräte kompatibel sein müssen**. Nur bestimmte Plattformen müssen kompatibel sein. Zugriffsrechte für andere Plattformen oder andere Plattformen, die die Anwendung zugreifen können.

  ![Der Bereich für Regeln] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Applikationen")

Azure AD verbundenen Geräte sind kompatibel, wenn sie **kompatible** im Verzeichnis markiert sind Intune oder um ein Gerät von Drittanbietern, die in Azure AD integriert.

Ein Domäne entspricht wenn:

- Es ist bei Azure AD registriert. Viele Organisationen behandelt Geräte Domäne als vertrauenswürdige Geräte.
- Es wird **als** in Azure AD durch System Center Configuration Manager 2016 gekennzeichnet.

 ![Domäne Geräte kompatibel sind] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Regeln")

Windows persönlichen Geräte sind kompatibel, wenn sie **kompatible** im Verzeichnis markiert sind Intune oder um ein Gerät von Drittanbietern, die in Azure AD integriert.

Nicht-Windows-Geräte sind kompatibel, wenn sie als **konform** im Verzeichnis von Intune markiert sind.

 >[AZURE.NOTE] Weitere Informationen zum Festlegen von Azure AD Device-Kompatibilität in verschiedenen Management-Systeme finden Sie unter [bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md).


Sie können eine oder mehrere Plattformen für eine Gerät Zugriff auswählen. Dazu gehören Android, iOS, Windows Mobile (Windows 8.1 Telefonen und Tablets) und Windows (alle anderen Windows Geräte einschließlich aller Windows 10).
Auswertung erfolgt nur auf ausgewählten Plattformen. Wenn ein Gerät, das Zugriff auf eines der ausgewählten Plattformen nicht ausgeführt wird, kann das Gerät die Anwendung zugreifen, wenn der Benutzer Zugriff hat. Keine Geräterichtlinie wird ausgewertet.

![Wählen Sie Plattformen für Regeln] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Regeln")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Auswertung von Richtlinien für eine Anwendung festlegen

Geben Sie im Abschnitt **Anwendung erzwingen** die Anwendungstypen, die Richtlinie für Benutzer oder Gerätezugriff ausgewertet wird.

Sie haben zwei Optionen für den Typ auf:

- Browser und Formaten
- Native Anwendung

![Wählen Sie Browser oder systemeigene Anwendung] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Applikationen")

Wählen Sie zum Erzwingen von Richtlinien für Applikationen **für Browser und Formaten**. Sie können dann Folgendes einschließen:

- Browser (z. B. Microsoft Edge in Windows 10) oder IOS Safari.
- Applikationen, Active Directory Authentifizierung Library (ADAL) auf allen Plattformen verwenden oder WebAccountManager (WAM) API in Windows 10 verwenden.

>[AZURE.NOTE] Weitere Informationen zum Webbrowser-Unterstützung und andere Aspekte für den Benutzer ein Gerätebasis zugreift Certificate Authority geschützte Anwendung finden Sie unter [Azure Active Directory Zugangskontrolle](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Schützen Sie e-Mail von Exchange ActiveSync-basierte

Exchange ActiveSync können Sie in Office 365 Exchange Online Applikationen um e-Mail-Zugriff auf Exchange ActiveSync-basierte e-Mail-Programme zu blockieren.

![Exchange ActiveSync Compliance-Optionen] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Applikationen")

![Benötigen Sie ein kompatibles Gerät Zugriff auf e-Mails] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Applikationen")


## <a name="next-steps"></a>Nächste Schritte

- [Azure Active Directory Zugangskontrolle](active-directory-conditional-access.md)

<properties
    pageTitle="Azure Active Directory Geräteregistrierung-Übersicht | Microsoft Azure"
    description="ist die Grundlage für bedingten Zugriff gerätebasierte Szenarien. Bei einem Gerät Vorschriften Azure Active Directory Geräteregistrierung mit einer Identität, mit das Gerät authentifiziert werden, wenn sich der Benutzer anmeldet."
    services="active-directory"
    keywords="Registrierung des Geräts und Registrierung des Geräts aktivieren, geräteregistrierung MDM"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="Markvi"/>

# <a name="get-started-with-azure-active-directory-device-registration"></a>Erste Schritte mit Azure Active Directory Geräteregistrierung

Azure Active Directory Geräteregistrierung ist die Grundlage für bedingten Zugriff gerätebasierte Szenarien. Bei einem Gerät bietet Azure Active Directory Gerät mit einer Identität, mit das Gerät authentifiziert werden, wenn sich der Benutzer anmeldet. Authentifizierte Gerät und die Attribute des Geräts können dann verwendet werden, bedingte Richtlinien für Applikationen erzwingen, die in der Cloud und lokal gehostet werden.

Zusammen mit einem mobilen Gerät management(MDM) wie Windows Intune werden Geräteattribute in Azure Active Directory mit zusätzlichen Informationen aktualisiert. Dadurch werden bedingte Regeln erstellen, die Zugriff von Geräten der Standards für Sicherheit und Compliance zu erzwingen. Weitere Informationen zum Registrieren von Geräten in Windows Intune finden Sie unter [Geräte für die Verwaltung von Intune registrieren](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Szenarien von Azure Active Directory Geräteregistrierung aktiviert

Unterstützt für iOS, Android und Windows Azure Active Directory Geräteregistrierung Geräte. Die einzelnen Szenarien, die Azure AD Gerät Registrierung nutzen möglicherweise genauere Vorschriften und Plattformen unterstützt. Diese Szenarien sind wie folgt:

- **Bedingte sind lokal gehostete Anwendung zuzugreifen**: können registrierte Geräte mit Richtlinien für Applikationen AD FS mit Windows Server 2012 R2 verwenden. Weitere Informationen zum Einrichten von bedingten Zugriff für lokale finden Sie unter [Einrichten von lokalen Zugangskontrolle mit Azure Active Directory Geräteregistrierung](active-directory-conditional-access-on-premises-setup.md).

- **Zugangskontrolle für Office 365-Anwendung mit Windows Intune** : IT-Administratoren können bedingte Geräterichtlinien schützen Unternehmensressourcen gleichzeitig ermöglicht Information Worker auf kompatiblen Geräten Zugriff auf die Dienste bereitstellen. Weitere Informationen finden Sie unter [Bedingte Geräterichtlinien für Office 365-Diensten](active-directory-conditional-access-device-policies.md).

##<a name="setting-up-azure-active-directory-device-registration"></a>Einrichten von Azure Active Directory Geräteregistrierung

Sie müssen Azure AD Gerät Registrierung im Azure-Portal aktivieren, sodass mobile Geräte den Dienst finden, bekannten DNS-Datensätzen suchen. Sie müssen Ihr Unternehmen DNS konfigurieren, sodass Windows 10 Windows 8.1, Windows 7, Android und iOS Geräte ermitteln und nutzen können.
Sie können und mit der Administratorportal in Azure Active Directory registrierte Geräte aktivieren.

>[AZURE.NOTE]
 Aktuelle Informationen zum automatische Registrierung finden Sie unter [Automatische Registrierung des Windows-Domäne einrichten mit Azure Active Directory verbunden](active-directory-conditional-access-automatic-device-registration-setup.md).

### <a name="enable-azure-active-directory-device-registration-service"></a>Azure Active Directory Gerät Registrierungsdienst aktivieren

1. Melden Sie sich als Administrator in Microsoft Azure-Portal an.
2. Wählen Sie im linken Bereich **Active Directory**.
3. Wählen Sie auf der Registerkarte **Verzeichnis** das Verzeichnis ein.
4. Wählen Sie die Registerkarte **Konfigurieren** .
5. Gehen Sie zum Abschnitt **Geräte**bezeichnet.
6. Wählen Sie **Alle** **Benutzer möglicherweise Arbeitsplatz JOIN Geräte**.
7. Wählen Sie die maximale Anzahl der Geräte pro Benutzer autorisieren möchten.

>[AZURE.NOTE]
>Registrierung mit Windows Intune oder Mobile Device Management für Office 365 erfordert Arbeitsbereich verknüpfen. Wenn Sie einen dieser Dienste konfiguriert haben, alle und Schaltfläche deaktiviert.

Standardmäßig ist die zweistufige Authentifizierung für den Dienst nicht aktiviert. Zwei-Faktor-Authentifizierung wird jedoch empfohlen, wenn ein Gerät zu registrieren.

- Zweistufige Authentifizierung für diesen Dienst müssen, Sie einen zweistufige Authentifizierungsanbieter in Azure Active Directory konfigurieren und Konfigurieren von Benutzerkonten für mehrstufige Authentifizierung, finden Sie unter [Azure Active Directory mehrstufige Authentifizierung hinzufügen](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)

- Bei Verwendung von AD FS mit Windows Server 2012 R2 müssen Sie eine zweistufige Authentifizierungsmodul in AD FS konfigurieren, finden Sie unter [Verwenden mehrerer Authentifizierung mit Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Azure Active Directory Geräteregistrierung Discovery konfigurieren
Windows 7 und Windows 8.1 Geräte entdecken Gerät Registrierungsdienst durch Kombinieren der Benutzerkontoname mit einem bekannten Geräteregistrierung Servernamen.

Sie müssen DNS CNAME-Eintrag erstellen, der auf den A-Datensatz der Registrierungsdienst Azure Active Directory Gerät zugeordnet. CNAME-Eintrag verwenden bekanntes Präfix Enterpriseregistration gefolgt von Benutzerkonten in Ihrer Organisation verwendete Benutzerprinzipalnamen-Suffix. Verwendet Ihre Organisation mehrere Benutzerprinzipalnamen-Suffixe müssen mehrere CNAME-Einträge in DNS erstellt.

Beispielsweise verwenden zwei Benutzerprinzipalnamen-Suffixe in Ihrer Organisation mit dem Namen @contoso.com und @region.contoso.com, erstellen Sie DNS-Einträge.

| Eintrag                                     | Typ  | Adresse                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.Region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Anzeigen und Verwalten von Geräteobjekten in Active Directory Azure
1. Aus der Administratorportal Azure können Sie anzeigen, blockieren und Geräte zulassen. Ein Gerät, das gesperrt ist haben mehr Zugriff auf Programme, die nur registrierte Geräte konfiguriert werden.
2. Microsoft Azure-Portal als Administrator anmelden.
3. Wählen Sie im linken Bereich **Active Directory**.
4. Wählen Sie das Verzeichnis.
5. Wählen Sie die Registerkarte **Benutzer** . Wählen Sie einen Benutzer ihre Geräte anzeigen
6. Wählen Sie die Registerkarte **Geräte** .
7. **Registrierte Geräte** aus dem Dropdown-Menü auswählen
8. Hier können Sie, blockieren oder Zulassen von Benutzern registrierten Geräte.

## <a name="additional-topics"></a>Weitere Themen

Sie können Ihre Windows 7 und Windows 8.1 Domäne Geräte Azure AD Gerät Registrierung registrieren. Die folgenden Themen enthält weitere Informationen über die Komponenten und die Registrierung des Geräts auf Windows 7 und Windows 8.1 Konfiguration erforderlichen Schritte.

- [Automatische Registrierung bei Azure Active Directory-Domäne Windows-Geräte](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren Sie automatische Registrierung für Windows 7-Domäne beitreten-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Konfigurieren Sie automatische Registrierung für Windows 8.1 Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische Anmeldung mit Azure Active Directory für Windows 10 Domäne](active-directory-azureadjoin-devices-group-policy.md)

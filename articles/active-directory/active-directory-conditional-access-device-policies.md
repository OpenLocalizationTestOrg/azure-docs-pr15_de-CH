<properties
    pageTitle="Bedingte Gerät Zugriffsrichtlinien für Office 365-Diensten | Microsoft Azure"
    description="Details bezüglich wie Geräte-basierte Zugriffskontrolle auf Office 365-Diensten. Mitarbeiter (IAS) auf Office 365-Dienste wie Exchange und SharePoint Online in Arbeit oder Schule von ihren persönlichen Geräten wollen, möchte ihren IT-Administrator den Zugriff auf secure.IT-Admins bedingte Geräterichtlinien schützen Unternehmensressourcen gleichzeitig ermöglicht IAS auf kompatiblen Geräten Zugriff auf die Dienste bereitstellen können."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Bedingte Gerät Zugriffsrichtlinien für Office 365-Diensten

Der Begriff "Conditional access" viele Ursachen wie mehrstufige authentifizierten Benutzer authentifiziert zugeordnet wurde, kompatible Gerät usw.. Dieses Thema konzentriert sich hauptsächlich auf Gerätebasis Vorschriften zur Steuerung des Zugriffs auf Office 365-Diensten. Mitarbeiter (IAS) auf Office 365-Dienste wie Exchange und SharePoint Online in Arbeit oder Schule von ihren persönlichen Geräten wollen, möchte ihren IT-Administrator den Zugriff zu sichern. IT-Administratoren können bedingte Geräterichtlinien schützen Unternehmensressourcen gleichzeitig ermöglicht IAS auf kompatiblen Geräten Zugriff auf die Dienste bereitstellen. Bedingte Richtlinien nach Office 365 können Windows Intune Zugangskontrolle Portal konfiguriert werden.

Azure Active Directory erzwingt bedingte Richtlinien zum Sichern des Zugriffs auf Office 365-Diensten. Ein Mieter Admin kann eine bedingten Richtlinie erstellen, die einen Benutzer auf ein nicht konformes Gerät Zugriff auf Office 365-Dienst blockiert. Der Benutzer muss Unternehmensrichtlinien Gerät entsprechen, bevor Zugang zum Dienst gewährt werden kann. Alternativ kann der Administrator auch eine Richtlinie erstellen, die Benutzer einfach ihren Geräten Zugriff auf ein Office 365-Dienst registrieren. Richtlinien für alle Benutzer einer Organisation angewendet oder beschränkt auf wenige Zielgruppen und auf Weitere Zielgruppen sind erweitert.

Voraussetzung für Geräterichtlinien durchsetzen ist für Benutzer ihre Geräte mit Azure Active Directory Gerät Registrierungsdienst registriert. Sie können mehrstufige Authentifizierung (MFA) für Ihre Registrierung bei Azure Active Directory Gerät Registrierungsdienst Geräte aktivieren. MFA für Azure Active Directory Gerät Registrierungsdienst empfohlen. Wenn MFA aktiviert ist, sind Benutzer registrieren ihre Geräte mit Azure Active Directory Gerät Registrierungsdienst für zweite Authentifizierung gefordert.

##<a name="how-does-conditional-access-policy-work"></a>Wie funktioniert die bedingte Richtlinie?

Wenn ein Benutzer fordert Zugriff auf Office 365-Dienst von einer Plattform unterstützt, authentifiziert Azure Active Directory Benutzer und Gerät, von dem der Benutzer die Anforderung startet; und gewährt Zugriff auf den Dienst nur, wenn der Benutzer die Richtlinie für den Dienst entspricht. Benutzer, die nicht ihrem Gerät registriert sind Anweisungen Abhilfemaßnahmen zum Registrieren und auf Office 365 Unternehmensdienste kompatibel. Benutzer auf IOS- und Android-Geräte müssen ihre Geräte Anwendung Unternehmensportal anmelden. Wenn ein Benutzer Ihr Gerät anmeldet, wird das Gerät Azure Active Directory registriert und für Device-Management und Compliance registriert. Kunden müssen den Registrierungsdienst Azure Active Directory Gerät zusammen mit Windows Intune verwenden, um mobile Device Management für Office 365-Dienst aktivieren. Geräteregistrierung ist Voraussetzung für Benutzer auf Office 365 Dienste Geräterichtlinien erzwungen werden.

Wenn ein Benutzer Ihr Gerät erfolgreich registriert, wird das Gerät vertrauenswürdig. Azure Active Directory bietet Single-Sign-On Access Unternehmen Clientanwendungen und bedingte Richtlinie Zugriff auf Dienst nicht nur beim ersten Mal gewährt der Benutzer fordert den Zugriff jedoch jedem Benutzer fordert Zugriff zu erneuern. Der Benutzer wird verweigert Zugriff auf Anmeldeinformationen geändert, Gerät ist verloren gegangen oder gestohlen oder die Richtlinie nicht zum Zeitpunkt der Antrag auf Verlängerung erfüllt.

## <a name="deployment-considerations"></a>Aspekte der Bereitstellung:
Azure Active Directory Gerät Registrierungsdienst Gerät Registrierung verwenden.

Wenn Benutzer lokal authentifiziert werden, muss Active Directory Federation Services (AD FS) (1.0 und höher). Mehrstufige Authentifizierung für Arbeitsplatz schlägt fehl, wenn der Identitätsanbieter nicht mehrstufige Authentifizierung kann. AD FS 2.0 ist z. B. nicht mehrstufige Authentifizierung kann. Tenant-Administrator muss sicherstellen, dass der lokale AD FS MFA-fähig und gültige MFA-Methode aktiviert ist, bevor Sie Azure Active Directory Gerät Registrierungsdienst MFA aktivieren. Beispielsweise hat AD FS unter Windows Server 2012 R2 MFA-Funktionen. Weitere gültige (MFA) Authentifizierungsmethode auf dem AD FS-Server müssen vor dem Azure Active Directory Gerät Registrierungsdienst MFA aktivieren. Weitere Informationen zu unterstützten MFA-Methoden in AD FS finden Sie unter AD FS zusätzliche Authentifizierungsmethoden konfigurieren.

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

Was ist der Ausschluss Standardrichtlinie für nicht unterstützte Plattformen?

A: Derzeit werden bedingte Richtlinien auf Benutzer IOS- und Android-Geräte selektiv umgesetzt. Auf anderen Plattformen werden standardmäßig von der bedingten Richtlinie für IOS- und Android-Geräte. Mieter Admin, jedoch können die globale Richtlinie auf nicht unterstützten Plattformen Benutzern den Zugriff verweigern überschreiben.
Wegweiser für bedingte Richtlinie für Benutzer auf anderen Plattformen, einschließlich Windows zu erweitern ist.

F: wann bedingte Richtlinie auf Office 365-Diensten browserbasierte apps (z. B. OWA, Browser-basierte SharePoint) verlängert werden.

A: derzeit beschränkt bedingten Zugriff auf Office365 Dienste vielfältigen Anwendungsmöglichkeiten auf Gerät. Es ist auf der Roadmap bedingte Richtlinie für Benutzer, die Zugriff auf die Dienste von Browsern erweitern.

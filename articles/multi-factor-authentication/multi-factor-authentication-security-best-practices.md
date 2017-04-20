<properties 
    pageTitle="Security Best Practices für die Verwendung von Azure MFA"
    description="Dieses Dokument enthält Azure Konten Azure MFA mit best practices"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Bewährte Sicherheitsmethoden Azure AD-Konten mit Azure mehrstufige Authentifizierung

Beim Erweitern der Authentifizierungsprozess, ist mehrstufige Authentifizierung bevorzugt für die meisten Organisationen. Azure mehrstufige Authentifizierung (MFA) können Unternehmen ihre Sicherheit Auflagen gleichzeitig einfacher anmelden für die Benutzer. Dieser Artikel behandelt einige bewährte Methoden, die für die Annahme von Azure MFA Planung berücksichtigen sollten.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Best Practices für Azure mehrstufige Authentifizierung in der cloud
Um alle Benutzer mit mehrstufigen Authentifizierung und Vorteile der erweiterten Features Azure mehrstufige Authentifizierung bietet, müssen Sie Azure mehrstufige Authentifizierung für alle Benutzer zu aktivieren.  Dies geschieht mithilfe einer der folgenden:

- Azure AD Premium oder Enterprise Mobility Suite
- Mehrstufige Authentifizierungsanbieter

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure AD Premium/Enterprise Mobility Suite

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Empfohlene zunächst Annahme Azure MFA in der Cloud Azure AD Premium oder Enterprise Mobility Suite werden Lizenzen, die Benutzern zugewiesen.  Azure mehrstufige Authentifizierung ist Teil dieser Suites und so Ihre Organisation braucht zusätzlichen erweitern die Funktionalität mehrstufige Authentifizierung für alle Benutzer.

Wenn mehrstufige Authentifizierung einrichten sollten Sie folgende:

- Sie brauchen eine mehrstufige Authentifizierung Anbieter erstellen.  Azure AD Premium und Enterprise Mobility Suite enthält Azure mehrstufige Authentifizierung.  Einen Authentifizierungsanbieter erstellen Sie doppelte erhalten abgerechnet.
- Azure AD Connect ist nur eine lokale Active Directory-Umgebung mit einer Azure AD-Verzeichnis synchronisiert werden. Wenn Sie nur ein Azure AD-Verzeichnis, die nicht mit einer lokalen Instanz von Active Directory synchronisiert wird, brauchen Sie nicht Azure AD verbinden.


### <a name="multi-factor-auth-provider"></a>Mehrstufige Authentifizierungsanbieter

![Mehrstufige Authentifizierungsanbieter](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Haben Sie keine Azure AD Premium oder Enterprise Mobility Suite wird empfohlen zunächst Annahme Azure MFA in der Cloud ein MFA-Authentifizierungsanbieter erstellen. Obwohl MFA standardmäßig für globale Administratoren verfügbar, die Azure Active Directory ist bei der Bereitstellung von MFA für Ihre Organisation müssen Sie erweitern die mehrstufige Authentifizierung Funktionalität für alle Benutzer und benötigen Sie eine mehrstufige Authentifizierungsanbieter.
Wenn der Authentifizierungsanbieter müssen Sie ein Verzeichnis auswählen und Folgendes:

- Sie brauchen keine Azure AD-Verzeichnis eine mehrstufige Authentifizierung Anbieter erstellt.
- Sie benötigen Azure AD-Verzeichnis möchten Sie mehrstufige Authentifizierung aller Benutzer erweitern und Ihre globalen Administratoren zu Funktionen wie Verwaltungsportal, benutzerdefinierte Grußformeln und Berichte mehrstufige Authentifizierungsanbieter zugeordnet.
- Dirsync- oder AAD Sync, nur wenn lokale Active Directory-Umgebung mit einer Azure AD-Verzeichnis synchronisiert werden. Wenn Sie nur ein Azure AD-Verzeichnis, die nicht mit einer lokalen Instanz von Active Directory synchronisiert wird, benötigen Sie keine DirSync oder AAD Sync.
- Wenn Sie Azure AD Premium oder Enterprise Mobility Suite haben, müssen Sie nicht kombinierte Authentifizierungsanbieter erstellen. Müssen nur einem Benutzer eine Lizenz zuweisen, und Sie können dann für Benutzer zu aktivieren.
- Richtige Verwendungsmodell für Ihr Unternehmen (pro Auth oder aktivierten Benutzer) Wählen Sie nach Auswahl das Nutzungsmodell ändern;

### <a name="user-account"></a>Benutzerkonto
MFA für Ihre Benutzer aktivieren können Benutzerkonten in einer der drei wichtigsten Zustände werden: deaktiviert, aktiviert oder erzwungen.
Verwenden Sie die folgenden Richtlinien, um sicherzustellen, dass Sie die am besten geeignete Option für die Bereitstellung verwenden:

- Wenn der Benutzerstatus auf deaktiviert festgelegt ist, wird dieser Benutzer nicht mehrstufige Authentifizierung verwendet. Dies ist der Standardzustand.
- Status des Benutzers aktiviert festgelegt ist, bedeutet, dass der Benutzer ist jedoch die Registrierung wurde nicht abgeschlossen. Sie werden aufgefordert, bei der nächsten Anmeldung abzuschließen. Diese Einstellung wirkt sich nicht auf nicht Browser-apps aus. Alle apps weiterhin funktionsfähig, wenn die Registrierung abgeschlossen ist.
- Wenn Status des Benutzers erzwungen festgelegt ist, bedeutet dies, dass der Benutzer möglicherweise keine Registrierung abgeschlossen. Wenn sie den Registrierungsvorgang abgeschlossen haben werden sie mehrstufige Authentifizierung verwenden. Andernfalls wird der Benutzer aufgefordert werden bei der nächsten Anmeldung registrieren. Diese Einstellung wirkt sich nicht Browser-apps. Diese apps funktionieren erst, nachdem app-Kennwörter erstellt und verwendet werden.
- Verwenden Sie per e-Mail an die Benutzer zu MFA Annahme Benachrichtigung Benutzervorlage im Artikel [Erste Schritte mit Azure mehrstufige Authentifizierung in der Cloud](multi-factor-authentication-get-started-cloud.md) .

### <a name="supportability"></a>Wartungsfreundlichkeit

Da die meisten Benutzer sind daran gewöhnt, nur Kennwörter zur Authentifizierung verwenden, ist es wichtig, dass Ihr Unternehmen Bewusstsein für alle Benutzer zu diesem Vorgang. Dieses kann die Wahrscheinlichkeit reduziert, die Benutzern das Helpdesk für kleinere Probleme mit MFA aufrufen.
Es gibt jedoch einige Szenarien, in denen vorübergehend deaktivieren MFA erforderlich. Verwenden Sie die folgenden Richtlinien zu um verstehen, wie diese Szenarios:

- Stellen Sie sicher, dass Ihre technischen Support geschult sind, Handle Szenarien, in denen die mobile Anwendung oder das Telefon keine Benachrichtigung oder einen Anruf erhält, und aus diesem Grund ist der Benutzer nicht anmelden. Sie können eine einmalige Bypass-Option Benutzer einmal "umgehen" Multifaktor-Authentifizierung authentifizieren. Das ist temporär und läuft nach der angegebenen Anzahl von Sekunden ab.
- Bei Bedarf können Sie die Funktion vertrauenswürdige IP-Adressen in Azure MFA nutzen. Dieses Feature ermöglicht Administratoren verwaltet oder verbundenen Mieter Mehrfaktor-Authentifizierung für Benutzer umgehen, die von lokalen Firmenintranet anmelden. Die Features sind für Azure AD-Mandanten, die Azure AD Premium, Enterprise Mobility Suite oder Azure mehrstufige Authentifizierung Lizenzen verfügbar.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Best Practices für lokale Azure mehrstufige Authentifizierung
Wenn Ihr Unternehmen eine eigene Infrastruktur MFA nutzen, wird ein lokal Azure mehrstufige Authentifizierungsserver bereitgestellt werden. MFA-Serverkomponenten sind in der folgenden Abbildung dargestellt:

![Mehrstufige Authentifizierungsanbieter](./media/multi-factor-authentication-security-best-practices/server.png)
*nicht standardmäßig ** installiert, aber nicht standardmäßig aktiviert


Azure mehrstufige Authentifizierungsserver dienen Cloudressourcen und lokalen Ressourcen, die Azure AD-Konten zugreifen.  Diese kann jedoch nur mithilfe der Föderation.  Sie müssen AD FS haben und es Verbund mit Azure AD-Mandanten.
Serverinstallation mehrstufige Authentifizierung berücksichtigen Sie bei der folgenden:

- Wenn Sie Azure AD-Ressourcen mit Active Directory Federation Services sichern, dann der 1. Faktor der Authentifizierung erfolgt lokal mit AD FS und 2. Faktor berücksichtigt die Forderung ist lokal durchgeführt.
- Es ist nicht erforderlich, Azure mehrstufige Authentifizierungsserver auf die ADFS-Verbundserver mehrstufige Authentifizierung Adapter für AD FS auf AD FS unter Windows Server 2012 R2 installiert werden muss installiert sein. Installieren Sie den Server auf einem anderen Computer als eine unterstützte Version ist, und den AD FS-Adapter separat auf der ADFS-Verbundserver installieren. Siehe das Verfahren unter Anleitung auf Adapter separat installieren.
- Mehrstufige Authentifizierung AD FS Adapter-Installationsassistenten Sicherheitsgruppe PhoneFactor-Admins in Active Directory erstellt und dieser Gruppe anschließend AD FS-Dienstkonto von Ihrem Verbunddienst hinzugefügt. Es wird empfohlen, dass auf dem Domänencontroller überprüfen, ob die Gruppe PhoneFactor-Admins tatsächlich erstellt und AD FS-Dienstkonto ein Mitglied dieser Gruppe ist. Gegebenenfalls fügen Sie das AD FS-Dienstkonto PhoneFactor Admins-Gruppe auf dem Domänencontroller manuell hinzu.

### <a name="user-portal"></a>User Portal
Dieses Portal wird in einer Internet Information Server (IIS)-Website bietet Selbsthilfefunktionen und bietet einen vollständigen Satz von Benutzer Verwaltungsfunktionen ausgeführt. Verwenden Sie die folgenden Richtlinien um diese Komponente zu konfigurieren:

- IIS 6 oder höher ist erforderlich
- ASP.NET v2.0.507207 installiert und registriert haben
- Dieser Server kann in einem Umkreisnetzwerk bereitgestellt werden.



### <a name="app-passwords"></a>App-Kennwörter
Ihrer Organisation über SSO in Azure AD federated Azure MFA verwenden wollen, dann beachten Verwendung app-Kennwörter (merken, dass dies nur für föderierte (SSO) verwendet):

- App-Kennwort von Azure AD überprüft und umgeht somit Föderation. Föderation wird nur verwendet, wenn App-Kennwort einrichten.
- Für föderierte (SSO) werden Benutzer Kennwörter in der Organisations-Id gespeichert. Wenn der Benutzer das Unternehmen verlässt, besitzt dieser Organisations-Id mit DirSync in Echtzeit übertragen. Konto deaktivieren und Löschen von dauert bis zu 3 Stunden zu synchronisieren, deaktivieren/Löschen von App-Kennwort in Azure AD verzögern.
- Lokalen Client Access Control Settings werden vom App-Kennwort nicht berücksichtigt.
- Keine lokale Authentifizierung Protokollierung / Überwachung Funktion steht für App-Kennwort
- Mehr Endbenutzer Bildung ist für Microsoft Lync 2013-Client erforderlich.
- Bestimmte erweiterte Architektur erfordern mit einer Kombination aus organisatorischen Benutzernamen und Kennwörter und app-Kennwörter beim Kunden, wo sie authentifizieren mit mehrstufigen Authentifizierung. Für Clients, die eine vor-Ort-Infrastruktur authentifizieren, verwenden Sie eine Organisationseinheit Benutzername und Kennwort. Für Clients, die Azure AD authentifizieren, verwenden Sie das app-Kennwort.
- Standardmäßig Benutzer app Kennwörter erstellen, wenn Ihr Unternehmen erfordert weder Benutzer app Passwort in einigen Szenarien müssen Sie sicher, dass die Option Benutzer können app Kennwörter nicht Browseranwendungen Anmelden aktiviert.

## <a name="additional-considerations"></a>Weitere Aspekte
Verwenden Sie die Liste um zu verstehen, dass einige zusätzliche Aspekte und empfohlene Vorgehensweisen für die einzelnen Komponenten werden vor Ort bereitgestellt:

-Methode|Beschreibung
:------------- | :------------- |
[Active Directory-Verbunddienst](multi-factor-authentication-get-started-adfs.md)|Informationen zum Einrichten von Azure mehrstufige Authentifizierung mit AD FS.
[RADIUS-Authentifizierung](multi-factor-authentication-get-started-server-radius.md)|  Informationen zur Installation und Konfiguration von Azure MFA-Server mit RADIUS.
[IIS-Authentifizierung](multi-factor-authentication-get-started-server-iis.md)|Informationen zur Installation und Konfiguration von Azure MFA-Server mit IIS.
[Windows-Authentifizierung](multi-factor-authentication-get-started-server-windows.md)|  Informationen über Setup und Konfiguration von Azure MFA-Server mit Windows-Authentifizierung.
[LDAP-Authentifizierung](multi-factor-authentication-get-started-server-ldap.md)|Informationen über Setup und Konfiguration von Azure MFA-Server mit LDAP-Authentifizierung.
[Remote Desktop Gateway und Azure mehrstufige Authentifizierungsserver mittels RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informationen zum Einrichten und Konfigurieren von Azure MFA-Server mit Remotedesktopgateway RADIUS verwenden.
[Mit Windows Server Active Directory synchronisieren](multi-factor-authentication-get-started-server-dirint.md)|Informationen zum Einrichten und Konfigurieren der Synchronisierung zwischen Active Directory und Azure MFA-Server.
[Den Serverdienst Mobile Web App Azure mehrstufige Authentifizierung bereitstellen](multi-factor-authentication-get-started-server-webservice.md)|Informationen über Setup und Konfiguration von Azure MFA-Serverwebdienst.
[Erweiterte VPN-Konfiguration mit Azure mehrstufige Authentifizierung](multi-factor-authentication-advanced-vpn-configurations.md)|Informationen zum Konfigurieren von Cisco ASA, Citrix Netscaler und Juniper-Impuls sichere VPN-Appliances mit LDAP oder RADIUS.


## <a name="additional-resources"></a>Zusätzliche Ressourcen
Und in diesem Artikel einige best Practices für Azure MFA, sind andere Ressourcen, die Sie auch beim Planen der MFA-Bereitstellung verwenden können. Die Liste enthält einige wichtige Artikel, die Sie dabei unterstützen können:

- [Berichte in Azure mehrstufige Authentifizierung](multi-factor-authentication-manage-reports.md)
- [Setupfunktionalität Azure mehrstufige Authentifizierung](multi-factor-authentication-end-user-first-time.md)
- [Häufig gestellte Fragen zur Azure mehrstufige Authentifizierung](multi-factor-authentication-faq.md)

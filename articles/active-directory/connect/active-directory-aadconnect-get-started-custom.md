<properties
    pageTitle="Azure AD Connect: Benutzerdefinierte Installation | Microsoft Azure"
    description="Dieses Dokument beschreibt die benutzerdefinierten Installationsoptionen für Azure AD verbinden. Verwenden Sie diese Anleitung zum Installieren von Active Directory über Azure AD Connect."
    services="active-directory"
    keywords="Azure AD verbinden, installieren Sie Active Directory für Azure AD erforderliche Komponenten"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Benutzerdefinierte Installation von Azure AD verbinden
Azure AD Connect **Benutzerdefiniert** verwendet möchten weitere Optionen für die Installation. Es wird verwendet, haben Sie mehrere Gesamtstrukturen oder optionale Features in die express-Installation nicht behandelt konfigurieren möchten. Sie wird in allen Fällen verwendet, in denen die [**express**](active-directory-aadconnect-get-started-express.md) -Installationsoption nicht Ihre Bereitstellung oder Topologie entsprechen.

Vor der Installation von Azure AD Connect sicherstellen [Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) Herunterladen abgeschlossen Voraussetzung Schritte und [Azure AD Connect: Hardware und Komponenten](../active-directory-aadconnect-prerequisites.md). Außerdem sicherstellen Sie, dass Konten unter [Azure AD Connect Konten und Berechtigungen](active-directory-aadconnect-accounts-permissions.md)erforderlich sind.

Wenn Einstellungen entspricht nicht Ihrer Topologie beispielsweise DirSync aktualisieren, finden Sie unter [Dokumentation](#related-documentation) für andere Szenarien.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Benutzerdefinierte Einstellung Installation von Azure AD verbinden

### <a name="express-settings"></a>Express-Einstellungen
Klicken Sie auf dieser Seite auf **Anpassen** , um einen benutzerdefinierten Einstellungen Installation starten.

### <a name="install-required-components"></a>Installieren der erforderliche Komponenten
Beim Installieren der Synchronization Services können Sie optionale Konfigurationsabschnitt deaktiviert lassen und Azure AD Connect richtet alles automatisch. Richtet eine Instanz von SQL Server 2012 Express LocalDB entsprechenden Gruppen erstellen und Berechtigungen zuweisen. Wenn Sie die Standardeinstellungen ändern möchten, können Sie in der folgenden Tabelle, optionale Konfigurationsoptionen zu verstehen, die verfügbar sind.

![Erforderliche Komponenten](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Optionale Konfiguration  | Beschreibung
------------- | -------------
Verwenden Sie eine vorhandene SQL Server | Können Sie den SQL Server-Namen und den Instanznamen angeben. Wählen Sie diese Option, wenn Sie bereits einen Datenbankserver verfügen, den Sie verwenden möchten. Geben Sie den Instanznamen gefolgt von einer Zahl durch Kommas und Port im **Instanznamen** der SQL Server keinen Browsen aktiviert.
Verwenden Sie ein vorhandenes Dienstkonto | Azure AD Connect erstellt standardmäßig ein lokales Dienstkonto für Synchronization Services verwenden. Das Kennwort wird automatisch generierten und unbekannte Person installiert Azure AD verbinden. Wenn Sie einen entfernten SQL Server oder Proxy, der Authentifizierung erfordert, Sie benötigen ein Konto in der Domäne und das Kennwort kennen. Geben Sie in diesen Fällen das Dienstkonto verwenden. Sicherstellen Sie, dass der Benutzer die Installation eine SA in SQL ist eine Anmeldung für das Dienstkonto erstellt werden kann. Siehe [Azure AD Connect Konten und Berechtigungen](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Geben Sie benutzerdefinierte Synchronisierungsgruppen | Standardmäßig erstellt Azure AD Connect vier Gruppen lokal auf dem Server Synchronization Services installiert sind. Diese Gruppen sind: Administratorengruppe, Operatorengruppe durchsuchen-Gruppe und der Gruppe Kennwort zurücksetzen. Sie können eigene Gruppen angeben. Die Gruppen auf dem Server lokal sein und können sich nicht in der Domäne.

### <a name="user-sign-in"></a>Benutzer anmelden
Nach der Installation der erforderlichen Komponenten müssen Sie die Benutzer einzelne Anmelden auswählen. Die folgende Tabelle enthält eine kurze Beschreibung der verfügbaren Optionen. Eine vollständige Beschreibung der Methoden finden Sie unter [Benutzer anmelden](../active-directory-aadconnect-user-signin.md).

![Benutzer anmelden](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Single Sign-On-option | Beschreibung
------------- | -------------
Kennwortsynchronisierung | Benutzer können Microsoft-Cloud-Diensten wie Office 365 und das gleiche Kennwort verwenden sie in ihrem lokalen Netzwerk anmelden. Die Benutzer Kennwörter in Azure AD einen Kennwort-Hash synchronisiert und Authentifizierung erfolgt in der Cloud. Weitere Informationen finden Sie unter [Kennwortsynchronisation](../active-directory-aadconnectsync-implement-password-synchronization.md) .
Mit AD FS | Benutzer können Microsoft-Cloud-Diensten wie Office 365 und das gleiche Kennwort verwenden sie in ihrem lokalen Netzwerk anmelden.  Der Benutzer umgeleitet werden auf ihre lokalen Instanz AD FS Anmeldung und Authentifizierung erfolgt lokal.
Nicht konfigurieren | Weder Feature installiert und konfiguriert. Wählen Sie diese Option, wenn Sie 3rd Party Verbundserver oder einer anderen vorhandenen Projektmappe vorhanden.

### <a name="connect-to-azure-ad"></a>Verbinden mit Azure AD
Geben Sie auf den Azure AD-Bildschirm mit globalen Administratorkonto und Kennwort ein. **Mit AD FS** auf der vorherigen Seite ausgewählt wird, melden Sie sich nicht mit einem Konto in einer Domäne zu für Föderationen aktivieren. Eine Empfehlung ist ein Konto in der Standarddomäne **onmicrosoft.com** mit Azure AD-Verzeichnis verwenden.

Dieses Konto wird nur zum Erstellen eines Dienstkontos in Azure AD verwendet und wird nicht verwendet, nachdem der Assistent abgeschlossen ist.  
![Benutzer anmelden](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Wenn globale Administratorkonto MFA aktiviert ist, müssen Sie geben Sie das Kennwort erneut in das Popup-in und der MFA-Herausforderung. Die Herausforderung könnte einen Verifizierungscode oder einen Telefonanruf bereitstellen.  
![Benutzer anmelden MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Das globale Administratorkonto können auch [Privilegierte Identity Management](../active-directory-privileged-identity-management-getting-started.md) aktiviert.

Wenn Sie eine Fehlermeldung und Verbindungsprobleme, finden Sie Sie unter [Problembehandlung bei Verbindungsproblemen](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Seiten im Abschnitt synchronisieren

### <a name="connect-your-directories"></a>Verbinden Sie Ihre Verzeichnisse
Die Verbindung mit der Active Directory-Domänendienste-benötigt Azure AD verbinden die Anmeldeinformationen für ein Konto mit ausreichenden Berechtigungen. Sie können den Domänenteil NetBIOS- oder FQDN Format FABRIKAM\syncuser oder fabrikam.com\syncuser eingeben. Dieses Konto kann ein normales Benutzerkonto sein, da nur schreibgeschützte Berechtigungen benötigt. Je nach Szenario benötigen Sie möglicherweise weitere Berechtigungen. Weitere Informationen finden Sie in [Azure AD Connect-Benutzerkonten und Berechtigungen](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Verzeichnis verbinden](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD-in-Konfiguration
Auf dieser Seite können Sie die Benutzerprinzipalnamen-Domänen im lokalen AD DS und die in Azure AD überprüft. Auf dieser Seite können Sie das Attribut UserPrincipalName mit konfiguriert.

![Nicht überprüfte Domänen](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Überprüfen Sie jede Domäne **Nicht hinzugefügt** und **Nicht überprüft**gekennzeichnet. Sicherstellen Sie, dass die verwendeten Domänen in Azure AD überprüft wurden. Klicken Sie auf das Symbol aktualisieren Ihre Domänen überprüft haben. Weitere Informationen finden Sie unter [Hinzufügen und Domäne überprüfen](../active-directory-add-domain.md)

**UserPrincipalName** - Attribut UserPrincipalName ist Attribut Benutzer beim Anmelden Azure AD und Office 365. Die Domänen verwendet, auch das UPN-Suffix, überprüft in Azure AD vor die Benutzern synchronisiert werden. Microsoft empfiehlt zu Standard-Attribut UserPrincipalName. Wenn dieses Attribut nicht routbare und nicht überprüft werden kann, ist ein weiteres Attribut auswählen. Sie können beispielsweise e-Mails als mit dem ID-Attribut Verwenden ein anderes Attribut als UserPrincipalName als **Alternative ID**bezeichnet. Alternativer Wert muss den RFC822 Standard. Eine alternative Kennung kann mit Kennwort synchronisieren und Föderation verwendet werden.

>[AZURE.WARNING]
Verwenden eine alternative Kennung ist nicht kompatibel mit Office 365-Workloads. Weitere Informationen finden Sie unter [Alternativen Benutzernamen konfigurieren](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Domäne und Organisationseinheit filtern
Standardmäßig werden alle Domänen und Organisationseinheiten synchronisiert. Gibt einige Domänen oder Organisationseinheiten nicht Azure AD synchronisieren möchten, können Sie diese Domänen und Organisationseinheiten aufheben.  
![Filtern von DomainOU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) dieser Seite im Assistenten ist die Konfiguration Domäne filtern. Weitere Informationen finden Sie in der [Domäne filtern](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

Es ist auch möglich, dass einige Domänen nicht aufgrund einer Firewall erreichbar sind. Diese Domänen sind standardmäßig deaktiviert und eine Warnung.  
![Nicht erreichbare Domänen](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Wenn diese Warnung angezeigt wird, stellen Sie sicher, dass diese Domänen in der Tat nicht erreichbar sind und die Warnung.

### <a name="uniquely-identifying-your-users"></a>Eindeutig identifizieren Benutzer
Übereinstimmende über Gesamtstrukturen Feature können Sie definieren, wie Benutzer Ihre AD DS-Gesamtstrukturen in Azure AD dargestellt werden. Ein Benutzer kann entweder dargestellt werden nur einmal in allen Gesamtstrukturen oder eine Kombination der aktivierten und deaktivierten Konten. Der Benutzer kann auch in einigen Gesamtstrukturen als Kontakt dargestellt.

![Eindeutige](./media/active-directory-aadconnect-get-started-custom/unique.png)

Einstellung | Beschreibung
------------- | -------------
[Benutzer werden nur einmal in allen Gesamtstrukturen dargestellt.](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Alle Benutzer werden als einzelne Objekte in Azure AD erstellt. Die Objekte sind im Metaverse angeschlossen.
[Mail-Attribut](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Diese Option schließt Benutzer und Kontakte hat dieses Attribut den gleichen Wert in verschiedenen Gesamtstrukturen. Verwenden Sie diese Option, wenn Ihre Kontakte mit GALSync erstellt wurden.
[ObjectSID und MsExchangeMasterAccountSID / MsRTCSIP-OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Diese Option schließt einen aktivierten Benutzer in einer Gesamtstruktur mit einem deaktivierten Benutzer einer Ressourcengesamtstruktur. In Exchange wird diese Konfiguration als verknüpftes Postfach bezeichnet. Diese Option kann auch verwendet werden, wenn Sie nur Lync und Exchange nicht in der Gesamtstruktur vorhanden ist.
sAMAccountName und MailNickName | Diese Option verbindet Attribute, wenn erwartet wird, die ID des Benutzers zu finden.
Ein bestimmtes Attribut | Mit dieser Option können Sie Ihre eigenen Attribut auswählen. **Einschränkung:** Wählen Sie ein Attribut, das bereits im Metaverse befinden. Wenn Sie ein benutzerdefiniertes Attribut (nicht im Metaverse) auswählen, kann der Assistenten nicht abschließen.

**Quelle Anchor** - Attribut SourceAnchor ist ein Attribut, das während der Lebensdauer eines Benutzerobjekts unveränderlich ist. Es ist der Primärschlüssel für lokale Benutzer mit der Benutzer in Azure AD verknüpfen. Da das Attribut geändert werden kann, müssen Sie eine gute Attribut verwenden planen. ObjectGUID eignet. Dieses Attribut wird nicht geändert, wenn das Benutzerkonto zwischen Gesamtstrukturen-Domänen verschoben wird. In einer Umgebung mit mehreren Gesamtstrukturen, in denen Konten zwischen Gesamtstrukturen verschieben, muss ein anderes Attribut wie ein Attribut mit der EmployeeID verwendet werden. Vermeiden Sie Attribute, die geändert werden, wenn eine Person heiratet oder ändern. Können keine Attribute mit einem @-sign, , e-Mail- und UserPrincipalName verwendet werden können. Das Attribut ist auch Kleinschreibung beim Verschieben eines Objekts zwischen Gesamtstrukturen dazu die Groß-/Kleinschreibung beibehalten. Binäre Attribute werden base64-codierte aber andere Attribut im unverschlüsselten Zustand verbleiben. Verbundszenarios und einige Azure AD-Schnittstellen ist dieses Attribut auch ImmutableID. Weitere Informationen über quellanker finden [Konzepte](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Sync-Filterung basierend auf Gruppen
Die Filterung auf Gruppen können Sie nur eine kleine Teilmenge von Objekten für einen Piloten synchronisieren. Um dieses Feature verwenden zu diesem Zweck in der lokalen Active Directory erstellen Sie eine Gruppe. Fügen Sie Benutzer und Gruppen, die synchronisiert werden sollen, Azure AD als direkte Mitglieder. Später können Sie hinzufügen und Entfernen von Benutzern zu dieser Gruppe zum Verwalten der Liste von Objekten, die in Azure AD vorhanden sein sollten. Alle Objekte, die Sie synchronisieren möchten, müssen Mitglied der Gruppe sein. Benutzer, Gruppen, Kontakte und Computer-Geräte müssen alle direkten Mitglieder sein. Geschachtelte Gruppenmitgliedschaft wird nicht aufgelöst. Wenn Sie eine Gruppe hinzufügen, als Mitglied der Gruppe selbst hinzugefügt wird und keine Member.

![Sync-Filterung](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Diese Funktion dient nur eine pilot-Installation unterstützt. Verwenden Sie es in eine voll ausgestattete Produktionseinheit Bereitstellung.

In einer Bereitstellung ausgestattete Produktionseinheit wird es schwer zu einer einzelnen Gruppe mit allen Objekten synchronisiert. Verwenden Sie stattdessen eine der Methoden im [Filter konfigurieren](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Optionale Funktionen
Auf dieser Seite können Sie optionalen Funktionen für Ihre spezifischen Szenarios auswählen.

![Optionale Funktionen](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Haben Sie derzeit DirSync oder Azure AD active Sync aktivieren Sie Funktionen Rückschreiben in Azure AD verbinden.

Optionale Funktionen | Beschreibung
------------------- | -------------
Exchange-Hybrid-Bereitstellung | Das Feature Exchange-Hybrid-Bereitstellung ermöglicht die Koexistenz von Exchange-Postfächern sowohl lokal und in Office 365. Azure AD verbinden synchronisiert einen bestimmten Satz von [Attributen](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) von Azure AD in Ihrem lokalen Verzeichnis.
Azure AD app und Attribute filtern | Azure AD app und Attribut-Filterung aktivieren, kann die Gruppe von synchronisierten Attributen angepasst werden. Diese Option fügt zwei weitere Konfigurationsseiten zum Assistenten. Weitere Informationen finden Sie in [Azure AD app und Attribute Filtern](#azure-ad-app-and-attribute-filtering).
Synchronisierung von Kennwörtern | Bei Auswahl von Verbund-in Lösung können Sie diese Option aktivieren. Synchronisierung von Kennwörtern kann dann als eine Sicherungsoption verwendet werden. Weitere Informationen finden Sie unter [Kennwortsynchronisation](../active-directory-aadconnectsync-implement-password-synchronization.md).
Kennwort Rückschreiben | Kennwort Rückschreiben aktivieren, Kennwort ändern, die in Azure AD stammen zurückgeschrieben zum lokalen Verzeichnis. Weitere Informationen finden Sie unter [Erste Schritte mit Passwort-Management](../active-directory-passwords-getting-started.md).
Gruppenrückschreiben | Verwenden Sie das Feature **Office 365 Gruppen** , können Sie diese Gruppen in der lokalen Active Directory dargestellt haben. Diese Option ist nur verfügbar, wenn Sie Exchange in Ihre lokale Active Directory vorhanden. Weitere Informationen finden Sie unter [gruppenrückschreiben](../active-directory-aadconnect-feature-preview.md#group-writeback).
Gerät Rückschreiben | Können Sie Geräteobjekte Rückschreiben in Azure AD zu lokalen Active Directory für bedingten Zugriffsszenarien. Weitere Informationen finden Sie unter [Enabling Gerät Rückschreiben in Azure AD verbinden](../active-directory-aadconnect-feature-device-writeback.md).
Verzeichnis-Erweiterung Attribut synchronisieren | Aktivieren Sie Verzeichnis Extensions Attribut synchronisieren, werden Attribute angegeben Azure AD synchronisiert. Weitere Informationen finden Sie im [Verzeichnis Extensions](../active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD app und Attribute filtern
Wenn Sie Attribute mit Azure AD beschränken möchten, dann wählen Sie zunächst welche Dienste Sie verwenden. Sie Konfiguration auf dieser Seite ändern, ist ein neuer Dienst explizit ausgewählt werden, durch den Installationsassistenten erneut.

![Optionale Features Apps](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Basierend auf den im vorherigen Schritt ausgewählt haben, werden auf dieser Seite alle Attribute, die synchronisiert werden. Diese Liste ist eine Kombination aller Objekttypen synchronisiert wird. Sind einige bestimmte Attribute nicht synchronisieren müssen, können Sie diese Attribute aufheben.

![Optionale Features Attribute](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Entfernen von Attributen kann die Funktionalität beeinträchtigt. Best Practices und Vorschläge finden Sie unter [Attribute synchronisiert](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Verzeichnis-Erweiterung Attribut synchronisieren
Sie können das Schema in Azure AD mit benutzerdefinierten Attributen, die von Ihrer Organisation oder anderen Attributen in Active Directory hinzugefügt erweitern. Um dieses Feature zu verwenden, wählen Sie **Directory Extension-Attribut synchronisieren** auf **Optionale Funktionen** . Sie können weitere Attribute synchronisieren auf dieser Seite auswählen.

![Verzeichnis extensions](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Weitere Informationen finden Sie im [Verzeichnis Extensions](../active-directory-aadconnectsync-feature-directory-extensions.md).

## <a name="configuring-federation-with-ad-fs"></a>Föderation mit AD FS konfigurieren
Konfigurieren von AD FS mit Azure AD Connect ist mit nur wenigen Mausklicks. Vor der Konfiguration ist erforderlich.

- Eine Windows Server 2012 R2-Server für den Verbund mit Remoteverwaltung aktiviert
- Eine Windows Server 2012 R2-Server für den Web Application Proxy mit Remoteverwaltung aktiviert
- Ein SSL-Zertifikat ein Verbund zu verwenden (z. B. sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>AD FS-Konfiguration erforderlichen Komponenten
Konfigurieren Sie Ihre AD FS-Farm mit Azure AD verbinden sicher, dass WinRM auf dem Remoteserver aktiviert ist. Darüber hinaus über Anschlüsse Anforderung in [Tabelle 3: Azure AD Connect und Federation Server/WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap)aufgeführt.

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Erstellen einer neuen AD FS-Farm oder einer vorhandenen AD FS-Serverfarm
Eine vorhandene AD FS Serverfarm verwenden, oder Sie können eine neue AD FS-Farm zu erstellen. Wenn Sie eine neue erstellen, müssen Sie das SSL-Zertifikat bereitstellen. Wenn das SSL-Zertifikat durch ein Kennwort geschützt ist, werden Sie aufgefordert, das Kennwort.

![AD FS-Farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Wenn Sie eine vorhandene AD FS-Serverfarm verwenden, werden direkt an die Vertrauensstellung zwischen AD FS und Azure AD konfigurieren.

### <a name="specify-the-ad-fs-servers"></a>Geben Sie den AD FS-Server
Geben Sie die zu AD FS auf installieren. Sie können einen oder mehrere Server basierend auf der Kapazität Planung hinzufügen. Fügen Sie vor dem Durchführen dieser Konfiguration aller Server Active Directory. Microsoft empfiehlt die Installation einer einzelnen AD FS für Test- und Pilotphase Bereitstellungen. Hinzufügen und weitere Server skalieren Wunsch mit Azure AD Connect erneut nach der Erstkonfiguration bereitstellen.

>[AZURE.NOTE]
Stellen Sie sicher, dass alle Server einer Active Directory-Domäne verknüpft sind, bevor Sie mit dieser Konfiguration.

![AD FS-Server](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Geben Sie die Web Application Proxy Server
Geben Sie die gewünschte als Proxyservern Web Application. Web Application Proxyserver in der DMZ (extranet mit) bereitgestellt und Authentifizierungsanfragen von extranet unterstützt. Sie können einen oder mehrere Server basierend auf der Kapazität Planung hinzufügen. Microsoft empfiehlt einen einzelnen Proxy Webanwendungsserver für Test- und Pilotphase Bereitstellung installieren. Hinzufügen und weitere Server skalieren Wunsch mit Azure AD Connect erneut nach der Erstkonfiguration bereitstellen. Wir empfehlen eine entsprechende Zahl der Proxyserver Authentifizierung vom Intranet zu.

>[AZURE.NOTE]
<li> Das verwendete Konto kein lokaler Administrator auf dem AD FS-Server ist, werden für Administratoranmeldeinformationen aufgefordert werden.</li>
<li> Sicherstellen Sie, dass HTTP/HTTPS-Verbindung zwischen Azure AD Connect-Server und Web Application Proxy-Server vorhanden ist, bevor Sie diesen Schritt ausführen.</li>
<li> Sicherstellen Sie, dass HTTP/HTTPS-Verbindung zwischen Web Application Server und dem AD FS-Server Authentifizierungsanfragen durchlaufen können.</li>

![WebApp](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Sie werden aufgefordert, Anmeldeinformationen eingeben, damit der Web Application Server eine sichere Verbindung mit dem AD FS-Server herstellen kann. Diese Anmeldeinformationen müssen ein lokaler Administrator auf dem AD FS-Server.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Geben Sie das Dienstkonto für den AD FS-Dienst
Der AD FS-Dienst erfordert ein Domänendienstkonto authentifiziert Benutzer und Benutzerinformationen in Active Directory suchen. Es kann zwei Arten von Konten unterstützen:

- **Gruppe verwaltet Dienstkonto** - Active Directory-Domänendienste mit Windows Server 2012 eingeführt. Dieses Konto bietet Dienste wie AD FS ein einzelnes Konto ohne das Kennwort regelmäßig zu aktualisieren. Verwenden Sie diese Option, haben Sie bereits Windows Server 2012-Domänencontroller in der Domäne, die die AD FS-Servern gehören.
- **Domänenbenutzerkonto** - ein solches Konto müssen Sie ein Kennwort eingeben und das Kennwort regelmäßig zu aktualisieren, wenn das Kennwort geändert wird oder abläuft. Verwenden Sie diese Option nur wenn Sie nicht Windows Server 2012-Domänencontroller in der Domäne, die die AD FS-Servern gehören.

Wenn Sie Gruppe verwaltetes Dienstkonto ausgewählt und dieses Feature wurde nicht in Active Directory verwendet, werden Sie Organisations-Admin-Anmeldeinformationen aufgefordert. Diese Anmeldeinformationen zum Initiieren des Schlüsselspeichers und aktivieren Sie die Funktion in Active Directory.

![AD FS-Dienstkonto](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Wählen Sie die Azure AD-Domäne, die Sie eine Föderation eingehen möchten
Diese Konfiguration zum Verbund Beziehung zwischen AD FS und Azure AD einrichten. Er konfiguriert AD FS Problem Sicherheitstoken in Azure AD und Azure AD vertrauen Token aus dieser bestimmten AD FS-Instanz konfiguriert. Auf dieser Seite können nur eine einzige Domäne in der ursprünglichen Installation konfigurieren. Sie können weitere Domänen später konfigurieren, Azure AD Connect erneut ausführen.

![Azure AD-Domäne](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Überprüfen Sie für die Föderation aktiviert Azure AD-Domäne
Bei der Auswahl der Domäne verbunden sein bietet Azure AD-Verbindung erforderlichen Informationen nicht überprüfte Domäne überprüfen. Siehe [Hinzufügen und Domäne überprüfen](../active-directory-add-domain.md) zur Verwendung dieser Informationen.

![Azure AD-Domäne](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
AD-Verbindung versucht, die Domäne konfigurieren Phase überprüfen. Wenn Sie fortfahren, ohne die erforderlichen DNS-Einträge konfigurieren, kann der Assistent nicht um die Konfiguration abzuschließen.

## <a name="configure-and-verify-pages"></a>Konfigurieren und Seiten überprüfen
Die Konfiguration erfolgt auf dieser Seite.

>[AZURE.NOTE]
Bevor die Installation fortgesetzt und Föderation konfiguriert, unbedingt [Auflösung für Verbundserver](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers)konfiguriert haben.

![Konfigurieren](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Stagingmodus
Es ist möglich, einen neuen Synchronisierungsserver mit staging-Modus einrichten. Es wird nur unterstützt eine Synchronisierungsserver exportieren in ein Verzeichnis in der Cloud. Jedoch möchten Sie von einem anderen Server, z. B. eine laufende DirSync, verschieben Sie können aktivieren Azure AD verbinden staging-Modus. Aktiviert, das Synchronisierungsmodul importieren und Synchronisieren von Daten als normal, aber es nicht exportiert nichts in Azure AD oder AD. Die Funktionen Kennwort synchronisieren und Kennwort Rückschreiben sind im staging-Modus deaktiviert.

![Stagingmodus](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Im staging-Modus kann zu ändern, das Synchronisierungsmodul Was ist exportiert werden. Bei die Konfiguration zufrieden sind, führen Sie den Installationsassistenten erneut aus und deaktivieren Sie staging-Modus. Daten werden jetzt in Azure Active Directory von diesem Server exportiert. Sollten Sie den Server zur gleichen Zeit deaktivieren also nur ein Server aktiv exportieren.

Weitere Informationen finden Sie im [Staging-Modus](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Überprüfen Sie die Konfiguration der Föderation
Azure AD Connect überprüft die DNS-Einstellungen beim Klicken auf die Schaltfläche überprüfen.

![Abschließen](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Stellen Sie sicher](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Darüber hinaus führen Sie die folgenden Schritte:

- Überprüfen Sie, ob Sie von einem Browser aus einer Domäne hinzugefügten Computer im Intranet anmelden können: mit https://myapps.microsoft.com und überprüfen Sie die Anmeldung mit Ihrem Konto angemeldet. Das integrierte Administratorkonto AD DS nicht synchronisiert und kann zur Überprüfung verwendet.
- Überprüfen Sie, ob Sie von einem Gerät aus dem extranet anmelden können. Auf einem Heimcomputer oder ein mobiles Gerät mit https://myapps.microsoft.com und Anmeldeinformationen Sie Ihre.
- Rich Client-Anmeldung überprüfen. Verbinden mit https://testconnectivity.microsoft.com, wählen Sie die **Office 365** -Registerkarte und wählen **Office 365 anmelden Testlaufs**.

## <a name="next-steps"></a>Nächste Schritte
Nach der Installation abmelden und wieder anmelden an Windows-Manager für Synchronisierungsdienst oder Synchronisierung Regel-Editor verwenden.

Jetzt haben Sie Azure AD Connect installiert können Sie [prüfen und Lizenzen zuweisen](../active-directory-aadconnect-whats-next.md).

Weitere Informationen zu diesen Features, die bei der Installation konnten: [verhindern, dass versehentlich löscht](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) und [Azure AD Connect Health](../active-directory-aadconnect-health-sync.md).

Weitere Informationen zu diesen Themen: [Planer und zum Synchronisieren auslösen](../active-directory-aadconnectsync-feature-scheduler.md).

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Zugehörige Dokumentation

Thema |  
--------- | ---------
Azure AD Connect-Übersicht | [Integration von lokalen Identitäten in Azure Active Directory](../active-directory-aadconnect.md)
Installieren mit Express-Standardeinstellungen | [Schnellinstallation Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Aktualisieren von DirSync | [Aktualisieren von Azure AD Synchronisierungsprogramm (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Konten für die installation | [Weitere Informationen zu Azure AD Connect Konten und Berechtigungen](active-directory-aadconnect-accounts-permissions.md)

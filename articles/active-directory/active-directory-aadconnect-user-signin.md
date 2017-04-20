<properties
    pageTitle="Azure AD verbinden: Benutzer anmelden | Microsoft Azure"
    description="Azure AD verbinden Benutzer anmelden für benutzerdefinierte Einstellungen."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD verbinden Benutzer anmelden Optionen

Azure AD Connect ermöglicht die Cloud und lokale Ressourcen die gleichen Kennwörter anmelden. Dieser Artikel beschreibt die wichtigsten Konzepte für jede Identitätsmodell zu erleichtern, die Identität der Anmeldung Azure AD verwenden möchten.

Wenn Sie kennen Azure AD Identitätsmodell schon, Sie mehr über eine bestimmte Methode möchten nur klicken Sie auf entsprechende Thema.

* [Kennwortsynchronisierung](#password-synchronization)
* [Föderierte SSO (ADFS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Auswählen einer Benutzer-Methode
Für die meisten Unternehmen möchten Benutzer Office 365 anmelden, SaaS-Applikationen und anderen Azure AD Ressourcen, anhand die Standardoption Kennwort Synchronisierung empfiehlt.
Einige Organisationen haben jedoch bestimmte Gründe für die Verwendung von föderativen Zeichen wie AD FS-Option.  Dazu gehören:

- Ihre Organisation verfügt bereits über AD FS oder eine 3rd party Föderation Anbieter bereitgestellt
- Ihre Sicherheitsrichtlinie verhindert Kennworthashes in die Cloud synchronisieren
- Sie müssen treten nahtlose SSO (ohne zusätzliches Kennwort aufgefordert werden) bei Domäne Cloudressourcen zugreifen Computer im Unternehmensnetzwerk
- Sie benötigen einige spezifischen Funktionen hat AD FS
    - Lokale mehrstufige Authentifizierung mit einem Drittanbieter oder Smartcards (lernen MFA Drittanbieter für AD FS in Windows Server 2012 R2)
    - Active Directory-Integrationsfunktionen wie weiche Kontensperrung oder AD Kennwort und Arbeit Stunden
    - Bedingten Zugriff auf lokale und cloud-Ressourcen mit geräteregistrierung, Azure AD Join oder Intune MDM Richtlinien

### <a name="password-synchronization"></a>Synchronisierung von Kennwörtern
Mit Synchronisierung werden Hashwerte der Kennwörter von Azure AD aus lokalen Active Directory synchronisiert.  Kennwörter werden geändert oder lokal zurücksetzen, werden neue Kennwörter sofort in Azure AD synchronisiert, sodass Benutzer immer dasselbe Kennwort Cloudressourcen verwenden können wie für lokale.  Die Kennwörter werden nie Azure AD gesendet noch in Azure AD in Klartext gespeichert.
Kennwortsynchronisation kann mit Kennwort Zurückschreiben ermöglicht self Service-in Azure AD Passwort verwendet werden.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Weitere Informationen zur Synchronisierung von Kennwörtern](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Föderation mit einer neuen oder vorhandenen AD FS in Windows Server 2012 R2 farm
Mit verbundenen können Ihre Benutzer Azure Anzeige Dienste ihre Kennwörter für lokale und auf das Unternehmensnetzwerk anmelden ohne ihr Kennwort erneut eingeben.  Die Option Föderation mit AD FS können Sie zum Bereitstellen einer neuen oder vorhandenen AD FS in Windows Server 2012 R2 Farm angeben.  Wenn Sie eine vorhandene Serverfarm angeben, wird Azure AD verbinden die Vertrauensstellung zwischen der Farm und Azure AD konfigurieren, sodass Benutzer anmelden können.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Mit AD FS in Windows Server 2012 R2 bereitstellen, benötigen Sie

Wenn Sie eine neue Farm bereitstellen:

- Ein Windows Server 2012 R2-Server für die Föderation.
- Windows Server 2012 R2 Server Web Application Proxy.
- eine PFX-Datei mit einem Zertifikat für Ihre vorgesehenen Federation Service Name wie fs.contoso.com.

Wenn Sie eine neue Farm bereitstellen oder verwenden eine vorhandene Serverfarm:

- Lokale Administratorrechte auf Ihrem Verbundserver.
- Lokale Administratorrechte auf jeder Arbeitsgruppe (keiner Domäne angehörenden) Server Web Application Proxy Rolle bereitgestellt werden sollen.
- Der Computer, auf dem der Assistent ausgeführt, muss Verbindung zu anderen Computern herstellen AD FS oder Webproxy über Windows Remote Management-Anwendung installiert werden soll.

[Konfigurieren von SSO mit AD FS](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Melden Sie sich auf eine frühere Version von AD FS oder Lösung

Wenn Sie auf eine frühere Version von AD FS (z. B. AD FS 2.0) oder einem Drittanbieter Föderation bereits Cloud-Zeichen konfiguriert haben, können Sie Benutzer anmelden Konfiguration über Azure AD Connect überspringen.  Dadurch können Sie sich die letzte Synchronisierung und andere Funktionen von Azure AD Connect gleichzeitiger Verwendung der vorhandenen Projektmappe Zeichen auf.

[Azure AD Drittanbieter-Verbund-Kompatibilitätsliste](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Benutzer anmelden und Benutzerprinzipalname (UPN)

### <a name="understanding-user-principal-name"></a>Grundlegendes zu Benutzerprinzipalnamen

In Active Directory ist das Standard-UPN-Suffix der DNS-Name der Domäne, in dem Benutzerkonto erstellt. In den meisten Fällen ist dies als Organisationsdomäne im Internet registrierten Domänennamen. Sie können jedoch weitere Benutzerprinzipalnamen-Suffixe mit Active Directory-Domänen und Vertrauensstellungen hinzufügen.

Der UPN des Benutzers hat das Format username@domai. Beispielsweise für active Directory "contoso.com" Johannes möglicherweise UPN john@contoso.com. Der UPN des Benutzers basiert auf RFC 822. Obwohl Benutzerprinzipalnamen und e-Mail dasselbe Format verwenden, kann der Wert des UPN eines Benutzers oder möglicherweise nicht die e-Mail-Adresse des Benutzers entspricht.

### <a name="user-principal-name-in-azure-ad"></a>Benutzerprinzipalname in Azure AD

Azure AD Connect-Assistenten verwenden Sie das Attribut UserPrincipalName oder können Sie das Attribut (benutzerdefinierte Installation) angeben, um lokal als Prinzipalnamen des Benutzers in Azure AD verwendet werden. Dies ist der Wert, der zum Anmelden bei Azure AD verwendet werden. Wenn der Wert des Attributs principal Name Benutzer nicht überprüfte Domäne in Azure AD entspricht, dann Azure AD ersetzt Standard. onmicrosoft.com-Wert.

Jedes Verzeichnis in Azure Active Directory ist mit einem integrierten Domänennamen in der Form contoso.onmicrosoft.com, mit dem Sie die ersten Schritte mit Azure oder anderen Microsoft-Diensten. Verbessert und vereinfachen Anmelden mit benutzerdefinierten Domänen. Informationen zu benutzerdefinierten Domain lesen Namen in Azure AD und überprüfen eine Domäne [Hinzufügen der benutzerdefinierten Domänennamen in Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD-in-Konfiguration

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-in Konfiguration mit Azure AD verbinden
Azure AD Anmelden ist abhängig davon, ob Azure AD können das UPN-Suffix eines Benutzers eines benutzerdefinierten Domänen überprüft in Azure AD-Verzeichnis synchronisiert ist. Azure AD Connect bietet Hilfe beim Konfigurieren von Azure AD anmelden, so dass Benutzer anmelden in Cloud ähnlich wie lokale. Azure AD Connect Listet die Benutzerprinzipalnamen-Suffixe für die Domäne definiert und versucht, sie mit einer benutzerdefinierten Domäne in Azure AD und hilft Ihnen, die zu treffenden Maßnahmen.
Die Azure AD-Seite listet die Benutzerprinzipalnamen-Suffixe für lokale Active Directory definiert und zeigt den entsprechenden Status für jedes Suffix. Die Statuswerte werden eines der folgenden:

| Zustand | Beschreibung | Handlungsbedarf |
|:----|:----|:----|
|Überprüft| Azure AD Connect fand eine entsprechende Domäne in Azure AD überprüft. Alle Benutzer dieser Domäne können Anmelden mit ihren lokalen Anmeldeinformationen| Keine Aktion erforderlich |
|Nicht überprüft| Azure AD Connect gefunden übereinstimmende benutzerdefinierte Domäne in Azure AD jedoch nicht überprüft. Benutzerprinzipalnamen-Suffix für die Benutzer dieser Domäne geändert auf. onmicrosoft.com Suffix nach Synchronisierung, wenn die Domäne nicht überprüft. | Überprüfen Sie die benutzerdefinierte Domäne in Azure AD. [Weitere Informationen](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Nicht hinzugefügt| Das UPN-Suffix für benutzerdefinierte Domäne Azure AD Connect nicht gefunden. Benutzerprinzipalnamen-Suffix Benutzer aus dieser Domäne geändert auf. onmicrosoft.com die Domäne nicht hinzugefügt und Verifeid in Azure.| Hinzufügen und [Weitere](./active-directory-add-domain.md) das UPN-Suffix für benutzerdefinierte Domäne überprüfen|

Azure AD-Anmeldeseite listet UPN Suffix(s) in Azure AD mit dem aktuellen Überprüfungsstatus für lokalen Active Directory und die entsprechende benutzerdefinierte Domäne definiert. Benutzerdefinierte Installation können Sie jetzt das Attribut Benutzerprinzipalname auf den **Azure AD -** Seite auswählen.

![Azure AD-Anmeldeseite](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Sie können auf die Aktualisierungsschaltfläche den aktuellen Status der benutzerdefinierten Domänen von Azure AD erneut abrufen klicken.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Attribut für Benutzerprinzipalnamen in Azure AD auswählen

UserPrincipalName - Attribut UserPrincipalName ist der Benutzer verwenden, wenn sie in Azure AD anmelden und Office 365. Die Domänen verwendet, auch das UPN-Suffix, überprüft in Azure AD vor die Benutzern synchronisiert werden. Standard-Attribut UserPrincipalName beibehalten wird empfohlen. Wenn dieses Attribut nicht routbare und nicht überprüft werden, ist es möglich, wählen Sie ein anderes Attribut, z. B. e-Mail, als mit dem ID-Attribut Dies wird als alternative ID bezeichnet. Alternativer Wert muss den RFC822 Standard. Eine alternative Kennung kann mit Kennwort anmelden (SSO) und Föderation SSO-in Lösung verwendet werden.

>[AZURE.NOTE] Verwenden eine alternative Kennung ist nicht kompatibel mit Office 365-Workloads. Weitere Informationen finden Sie unter [Alternativen Benutzernamen konfigurieren](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Andere benutzerdefinierte Domäne Status und Auswirkung auf Azure anmelden
Es ist wichtig, die Beziehung zwischen der benutzerdefinierten Domäne Zustände in Azure AD-Verzeichnis und die Benutzerprinzipalnamen-Suffixe lokalen definiert. Wir durchlaufen Sie möglichen Azure anmelden Erfahrungen beim Einrichten der Azure AD Verbindung mit Sycnhronization.

Informationen unten angenommen, UPN-Suffix contoso.com sind derer im lokalen Verzeichnis als Teil des UPN, z. B. user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Express-Einstellungen / Kennwortsynchronisation
| Zustand         | Auswirkung auf Benutzer Azure anmelden |
|:-------------:|:----------------------------------------|
| Nicht hinzugefügt | In diesem Fall wurde keine benutzerdefinierte Domäne contoso.com in Azure AD-Verzeichnis hinzugefügt. Benutzer mit UPN-lokal mit Suffix @contoso.com, nicht mit ihrem lokalen Benutzerprinzipalnamen Azure anmelden. Sie müssen stattdessen einen neuen UPN gesendetes Azure AD durch Hinzufügen des Suffixes Azure AD Standardverzeichnis zu verwenden. Beispielsweise Synchronisierung Benutzer Azure AD-Verzeichnis azurecontoso.onmicrosoft.com Sie dann den lokalen Benutzer user@contoso.com erhält einen UPNuser@azurecontoso.onmicrosoft.com|
| Nicht überprüft | In diesem Fall haben wir eine benutzerdefinierte Domäne contoso.com in Azure AD-Verzeichnis hinzugefügt, jedoch noch nicht bestätigt wird. Wenn Sie fortfahren mit Benutzern ohne Überprüfung der Domäne synchronisieren, werden dann der Benutzer zugewiesen neuen UPN von Azure AD, wie es im Szenario "Nicht hinzugefügt".|
| Überprüft | In diesem Fall haben wir eine benutzerdefinierte Domäne contoso.com bereits hinzugefügt und in Azure AD Benutzerprinzipalnamen-Suffix überprüft. Benutzer werden ihre lokalen Benutzerprinzipalnamen, z. B. mit user@contoso.com, in Azure anmelden Nachdem sie Azure Active Directory synchronisiert sind|

###### <a name="ad-fs-federation"></a>AD FS-Verbund
Eine Föderation kann nicht mit den Standardeinstellungen erstellt. onmicrosoft.com Domäne in Azure AD oder eine nicht überprüfte benutzerdefinierte Domäne in Azure AD. Wenn Azure AD Connect-Assistent ausgeführt wird, wählt eine nicht überprüfte Domäne erstellen fordert eine Föderation mit dann Azure AD Connect Sie erforderlichen Datensätze erstellt, Ihren DNS-Server für die Domäne gehostet wird. Weitere Informationen finden Sie [hier](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Wenn Sie Benutzer anmelden als "Föderation mit AD FS" gewählt, müssen Sie eine benutzerdefinierte Domäne weiterhin eine Föderation in Azure AD erstellen. Für unsere Diskussion bedeutet dies, dass wir eine benutzerdefinierte Domäne contoso.com in Azure AD-Verzeichnis hinzugefügt haben.

| Zustand         | Auswirkung auf Benutzer Azure anmelden |
|:-------------:|:----------------------------------------|
| Nicht hinzugefügt | In diesem Fall Azure AD Connect nicht übereinstimmende benutzerdefinierte Domäne für UPN-Suffix contoso.com in Azure AD-Verzeichnis gefunden. Müssen Sie eine benutzerdefinierte Domäne contoso.com hinzufügen, benötigen Benutzer mit AD FS mit ihrem lokalen Benutzerprinzipalnamen wie user@contoso.com.|
| Nicht überprüft | In diesem Fall fordert Azure AD Connect Sie mit entsprechenden wie Ihre Domäne später überprüfen können|
| Überprüft | In diesem Fall können Sie mit der Konfiguration ohne weitere Aktion fortfahren|  

## <a name="changing-user-sign-in-method"></a>Ändern der Benutzer-Methode

Sie können die Benutzer-Methode Föderation Kennwort synchronisieren mit verfügbaren Aufgaben in Azure AD Verbinden nach der erstmaligen Konfiguration von Azure AD Connect mithilfe des Assistenten ändern. Azure AD Connect-Assistenten erneut ausführen und wird mit einer Liste von Aufgaben, die Sie angezeigt werden. Wählen Sie aus der Liste der Aufgaben **ändern Benutzer anmelden** . 

![Ändern Sie Benutzer Anmeldung"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Auf der nächsten Seite werden Sie aufgefordert, die Anmeldeinformationen für Azure AD.

![Verbinden mit Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Wählen Sie auf der Seite **Benutzer anmelden** **Kennwortsynchronisation**. Dies ändert das Verzeichnis federated Management.

![Verbinden mit Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Wenn Sie nur einen temporären Switch mit Kennwortsynchronisation herstellen, überprüfen Sie die **Benutzerkonten nicht konvertieren**. Die Option keine Überprüfung führt zu Konvertierung jedes Benutzers verbunden und kann mehrere Stunden dauern.
  
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

Erfahren Sie mehr über [Azure AD Connect: Konzepte](active-directory-aadconnect-design-concepts.md)



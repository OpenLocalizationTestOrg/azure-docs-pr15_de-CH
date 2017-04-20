<properties 
    pageTitle="Erste Schritte mit Zertifikat-Authentifizierung auf Android | Microsoft Azure" 
    description="Informationen Sie zum Zertifikatauthentifizierung Lösungen mit Android-Geräte konfigurieren" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>Erste Schritte mit Zertifikat-Authentifizierung auf Android - Public Preview  

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


In diesem Thema wird das Konfigurieren und nutzen Zertifikatauthentifizierung (CBA) auf Android-Gerät für Benutzer der Mandanten in Office 365 Enterprise, Unternehmen und Ausbildung veranschaulicht. 

CBA können Sie mit einem Clientzertifikat auf einem Gerät Android oder iOS Azure Active Directory authentifiziert werden, wenn Ihr Exchange-online-Konto zu verbinden: 

- Mobile Office-Programme wie Microsoft Outlook und Microsoft Word   
- Exchange ActiveSync (EAS) clients 

Konfigurieren dieses Features entfällt die Notwendigkeit, eine Kombination aus Benutzername und Kennwort bestimmte Mail und Microsoft Office-Programme auf Ihrem mobilen Gerät eingeben. 
 

## <a name="supported-scenarios-and-requirements"></a>Unterstützte Szenarien und Vorschriften  



### <a name="general-requirements"></a>Allgemeine Vorschriften 


Für alle Szenarien in diesem Thema ist Folgendes erforderlich:  

- Zugriff auf Zertifikat Autoritäten Client Zertifikate ausstellen.  

- Autoritäten Zertifikate müssen in Azure Active Directory konfiguriert werden. Ausführliche Schritte zum Abschließen der Konfiguration im Abschnitt [Erste Schritte](#getting-started) finden.  

- Die Stammzertifizierungsstelle und intermediate Zertifizierungsstellen müssen in Azure Active Directory konfiguriert werden.  

- Jede Zertifizierungsstelle muss eine Zertifikatssperrliste (CRL), die über einen Internetzugriff URL verwiesen werden kann.  

- Das Clientzertifikat muss für die Clientauthentifizierung ausgestellt werden.  


- Für Exchange ActiveSync-Clients muss das Clientzertifikat routingfähige e-Mail-Adresse des Benutzers in Exchange verfügen online Principal Name oder Wert RFC822-Namen im Feld Alternativer Antragstellername. Azure Active Directory Proxyadresse Attribut im Verzeichnis RFC822-Wert zugeordnet.  



### <a name="office-mobile-applications-support"></a>Office Unterstützung mobile 

| Apps                      | Unterstützung      |
| ---                       | ---          |
| Word / Excel / PowerPoint | ![Kontrollkästchen][1]  |
| OneNote                   | Demnächst  |
| OneDrive                  | ![Kontrollkästchen][1]  |
| Outlook                   | ![Kontrollkästchen][1]  |
| Yammer                    | ![Kontrollkästchen][1]  |
| Skype für Unternehmen        | ![Kontrollkästchen][1]  |


### <a name="requirements"></a>Vorschriften  

Die Geräteversion OS muss Android 5.0 (Lollipop) und höher. 

Verbundserver muss konfiguriert werden.  


ADFS-Token müssen folgenden Angaben für Azure Active Directory ein Clientzertifikat widerrufen  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Seriennummer des Clientzertifikats) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Die Zeichenfolge für den Aussteller des Clientzertifikats) 

Azure Active Directory fügt diese Ansprüche auf die Aktualisierungstoken in der ADFS-Token (oder andere SAML-Token) verfügbar sind. Wenn Aktualisierungstoken überprüft werden muss, wird diese Informationen die Sperrung zu überprüfen. 

Als bewährte Methode sollten Sie die ADFS-Fehlerseiten wie ein Benutzerzertifikat zu aktualisieren. 

Weitere Informationen finden Sie unter [Anpassen der AD FS-Anmeldung Seiten](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync-Clients unterstützen 


Bestimmte Exchange ActiveSync-Clientanwendungen Android 5.0 (Lollipop) oder höher werden unterstützt. Um festzustellen, ob Ihr e-Mail-Programm diese Funktion unterstützt, wenden Sie sich an Ihren Anwendungsentwickler. 



## <a name="getting-started"></a>Erste Schritte 


Zunächst müssen Sie die Zertifizierungsstellen in Azure Active Directory konfigurieren. Übertragen Sie für jede Zertifizierungsstelle Folgendes: 

- Den öffentlichen Teil des Zertifikats *CER* -Format 

- Die mit Internetzugriff URLs Wohnsitz Certificate Revocation Lists (CRL)
 

Im folgenden wird das Schema für eine Zertifizierungsstelle: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Zum Hochladen von Informationen können Sie das Modul Azure AD Windows PowerShell.  
Im folgenden sind Beispiele für hinzufügen, entfernen oder Ändern einer Zertifizierungsstelle. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Konfiguration von Azure AD-Mandanten Zertifikat basierte Authentifizierung 

1. Starten Sie Windows PowerShell mit Administratorrechten. 

2. Installieren Sie das Modul Azure AD. Müssen Sie installieren Version [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) oder höher.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Verbinden Sie mit Ihrem Ziel Mandanten: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Hinzufügen einer neuen Zertifizierungsstelle

1. Festlegen Sie verschiedener Eigenschaften der Zertifizierungsstelle und Azure Active Directory hinzufügen: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Die Zertifizierungsstellen zu erhalten: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Abrufen der Liste Zertifizierungsstellen

Rufen Sie für Ihren Mandanten in Azure Active Directory gespeicherten Zertifizierungsstellen: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Entfernen einer Zertifizierungsstelle

1.  Abrufen von Zertifizierungsstellen: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Entfernen Sie das Zertifikat für die Zertifizierungsstelle: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying einer Zertifizierungsstelle 

1.  Abrufen von Zertifizierungsstellen: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Ändern Sie die Eigenschaften der Zertifizierungsstelle: 

        $c[0].AuthorityType=1 

3. Legen Sie die **Zertifizierungsstelle**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Office mobile Anwendungstests  

Zertifikatauthentifizierung auf mobile Office-Anwendung zu testen: 

1.  Installieren Sie auf Ihrem Testgerät mobile Office-Anwendung (z. B. OneDrive) von Google Play Store.

2.  Stellen Sie sicher, dass das Zertifikat mit dem Testgerät bereitgestellt wurde. 

3.  Starten Sie die Anwendung. 

4.  Geben Sie Ihren Benutzernamen, und wählen Sie das Zertifikat verwenden möchten. 

Sie müssen angemeldet sein. 





## <a name="testing-exchange-activesync-client-applications"></a>Testen von Exchange ActiveSync-Clientanwendungen

Zugriff auf Exchange ActiveSync über Zertifikat-Authentifizierung muss ein EAS-Profil mit dem Client-Zertifikat zur Anwendung. EAS-Profil muss folgende Informationen enthalten:

- Das Zertifikat für die Authentifizierung verwendet werden 

- EAS-Endpunkt muß outlook.office365.com (dieses Feature in Exchange online Multi-Tenant-Umgebung derzeit unterstützt wird)

EAS-Profil kann konfiguriert und auf dem Gerät mit einem MDM wie Intune oder manuell das Zertifikat in die EAS-Profil auf dem Gerät gespeichert werden.  


### <a name="testing-eas-client-applications-on-android"></a>EAS-Clientanwendungen auf Android testen 

Zertifikatauthentifizierung mit einer Anwendung auf Android 5.0 (Lollipop) testen oder später führen Sie die folgenden Schritte aus:  

1. Konfigurieren Sie in der Anwendung, die den obigen Vorschriften entspricht EAS Profil.  


2. Sobald das Profil ordnungsgemäß konfiguriert ist, öffnen Sie die Anwendung und überprüfen Sie, Mail synchronisiert werden. 




## <a name="revocation"></a>Sperrung

Um ein Clientzertifikat widerrufen, Azure Active Directory die Zertifikatssperrliste (CRL) von URLs als Teil der Informationen der Zertifizierungsstelle abgerufen und zwischengespeichert. Veröffentlichen der letzten Timestamp (**Gültigkeitsdatum** Eigenschaft) in der CRL dienen die CRL ist noch gültig. Die CRL wird regelmäßig verwiesen, um Zugriff auf Zertifikate widerrufen, die Teil der Liste.

Wenn schneller gesperrt (z. B. wenn ein Benutzer ein Gerät verliert) erforderlich ist, kann das Authentifizierungstoken des Benutzers ungültig. Um das Autorisierungstoken für ungültig erklären, legen Sie das **StsRefreshTokenValidFrom** Feld für diesen bestimmten Benutzer mithilfe von Windows PowerShell. Feld **StsRefreshTokenValidFrom** für jeden Benutzer müssen Zugriff widerrufen möchten aktualisiert werden.
 
Um sicherzustellen, dass die Sperrung weiterhin, müssen Sie das **Gültigkeitsdatum** der CRL auf ein Datum nach der Wert von **StsRefreshTokenValidFrom** und das Zertifikat in Frage werden die CRL.
 
Die folgenden Schritte des Prozess zum Aktualisieren und Ungültigmachen das Autorisierungstoken durch Festlegen des **StsRefreshTokenValidFrom** . 

1. Verbinden Sie mit Administratoranmeldeinformationen für den MSOL-Dienst: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Den aktuellen Wert der StsRefreshTokensValidFrom für einen Benutzer abrufen: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Konfigurieren Sie einen neuen StsRefreshTokensValidFrom-Wert für Benutzer gleich den aktuellen Zeitstempel: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Das festgelegte Datum muss in der Zukunft. Wenn das Datum nicht in der Zukunft liegt, wird die **StsRefreshTokensValidFrom** -Eigenschaft nicht festgelegt. Wenn das Datum in der Zukunft liegt, wird **StsRefreshTokensValidFrom** auf die aktuelle Zeit (nicht das Datum vom Befehl Set-MsolUser) festgelegt. 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
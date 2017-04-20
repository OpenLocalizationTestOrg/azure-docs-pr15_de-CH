<properties
    pageTitle="Azure AD Verbinden mehrerer Domänen"
    description="Dieses Dokument beschreibt die Installation und Konfiguration von mehreren Domänen der obersten Ebene mit Office 365 und Azure AD."
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
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Unterstützung für mehrere Domänen für eine Föderation mit Azure
Die folgende Dokumentation enthält Hinweise können mehrere Domänen der obersten Ebene und untergeordnete Domänen eine Föderation mit Office 365 oder Azure AD-Domänen.

## <a name="multiple-top-level-domain-support"></a>Unterstützung für mehrere Domänen der obersten Ebene
Föderation mehrere, Domänen der obersten Ebene mit Azure erfordert einige zusätzliche Konfiguration nicht erforderlich ist, wenn eine Föderation mit einer Domäne der obersten Ebene.

Wenn eine Domäne mit Azure verbunden ist, werden mehrere Eigenschaften in der Domäne in Azure festgelegt.  Wichtigste ist IssuerUri.  Dies ist ein URI, der von Azure AD verwendet wird, um die Domäne zu identifizieren, der das Token zugeordnet ist.  Der URI muss an alles andere zu beheben, es muss ein gültiger URI sein.  Standardmäßig Azure AD wird dies auf den Wert des Bezeichners Federation Service in Ihrem lokalen AD FS Konfiguration.

>[AZURE.NOTE]Federation Service Bezeichner ist ein URI, der einen Verbunddienst eindeutig identifiziert.  Der Verbunddienst ist eine Instanz von AD FS, Funktionen wie dem Sicherheitstokendienst. 

Vew IssuerUri mithilfe von PowerShell-Befehl können `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![MsolDomainFederationSettings abrufen](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Ein Problem tritt auf, wenn es mehr als eine Domäne der obersten Ebene hinzufügen möchten.  Wir beispielsweise haben Setup Föderation zwischen Azure und Ihre lokale Umgebung.  Für dieses Dokument verwende ich bmcontoso.com.  Jetzt kann ich eine Domäne zweite, der obersten Ebene haben bmfabrikam.com.

![Domänen](./media/active-directory-multiple-domains/domains.png)

Wenn wir versuchen, unsere bmfabrikam.com Domäne verbunden sein konvertieren, erhalten wir einen Fehler.  Der Grund dafür ist Azure AD hat eine Einschränkung, die nicht IssuerUri Eigenschaft denselben Wert für mehrere Domänen zu ermöglichen.  
  

![Verbund-Fehler](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain Parameter

Um Abhilfe zu schaffen, müssen wir einen anderen IssuerUri hinzufügen die mit der `-SupportMultipleDomain` Parameter.  Dieser Parameter wird mit den folgenden Cmdlets verwendet:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Dieser Parameter stellt Azure AD die IssuerUri so konfigurieren, dass auf dem Namen der Domäne basiert.  Dies ist über Verzeichnisse in Azure AD eindeutig.  Den Parameter können PowerShell-Befehl erfolgreich abgeschlossen.

![Verbund-Fehler](./media/active-directory-multiple-domains/convert.png)

Einstellungen der neuen Domäne bmfabrikam.com sehen Sie Folgendes:

![Verbund-Fehler](./media/active-directory-multiple-domains/settings.png)

Beachten Sie, dass `-SupportMultipleDomain` ändert nicht die Endpunkte die weiterhin auf unsere Verbunddienst adfs.bmcontoso.com konfiguriert werden.

Noch etwas, `-SupportMultipleDomain` ist, dass dadurch das AD FS-System den korrekten Aussteller Wert in Azure AD ausgestellten Token enthält. Dazu der Domänenteil der Benutzer Benutzerprinzipalnamen und dies als Domäne IssuerUri, d. h. https://{upn Suffix} / Adfs/Services/Trust. 

Damit während der Authentifizierung in Azure AD oder Office 365 und das IssuerUri-Element in das Zugriffstoken des Benutzers zu der Domäne in Azure AD verwendet wird.  Wenn eine Übereinstimmung kann, die Authentifizierung fehl gefunden werden. 

Z. B. UPN eines Benutzers ist bsimon@bmcontoso.com, IssuerUri Element in den Token AD FS wird auf http://bmcontoso.com/adfs/services/trust festgelegt. Dies entspricht der Azure AD-Konfigurations und Authentifizierung erfolgreich.

Folgendes ist die angepasste Anspruchsregel, die Logik implementiert:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Nutzung den Switch - SupportMultipleDomain beim Versuch, neue oder bereits konvertieren Domänen hinzufügen, müssen Sie Setup die Vertrauensstellung ursprünglich begleiten.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Die Vertrauensstellung zwischen AD FS Azure aktualisieren
Wenn die Vertrauensstellung zwischen AD FS und Azure AD-Instanz nicht eingerichtet, müssen Sie diese Vertrauensstellung neu erstellen.  Denn wenn es ursprünglich ohne ist die `-SupportMultipleDomain` Parameter der IssuerUri mit dem Standardwert festgelegt ist.  Im Screenshot unten sehen, dass die IssuerUri https://adfs.bmcontoso.com/adfs/services/trust ist.

Nun, wenn wir erfolgreich eine neue Domäne in Azure AD-Portal hinzugefügt haben und versuchen, es mit konvertieren `Convert-MsolDomaintoFederated -DomainName <your domain>`, wir erhalten die folgende Fehlermeldung.

![Verbund-Fehler](./media/active-directory-multiple-domains/trust1.png)

Beim Hinzufügen der `-SupportMultipleDomain` Switch die folgende Fehlermeldung erhalten:

![Verbund-Fehler](./media/active-directory-multiple-domains/trust2.png)

Zur Ausführung `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` in der ursprünglichen Domäne auch führt zu einem Fehler.

![Verbund-Fehler](./media/active-directory-multiple-domains/trust3.png)

Gehen Sie unten eine zusätzliche Domäne der obersten Ebene hinzufügen.  Wenn Sie bereits eine Domäne hinzugefügt und nicht der `-SupportMultipleDomain` Parameter Start die Schritte zum Entfernen und Aktualisieren Ihrer ursprünglichen Domäne.  Wenn Sie eine Domäne der obersten Ebene hinzugefügt haben noch beginnen Sie mit den Schritten zum Hinzufügen einer Domäne mit PowerShell Azure AD verbinden.

Gehen Sie zu Microsoft Online-Vertrauensstellung entfernen Ihrer ursprünglichen Domäne.

2.  In der ADFS-Verbundserver öffnen **AD FS Verwaltung.** 
2.  Erweitern Sie auf der linken Seite **Vertrauensstellungen** und **Verlassen vertrauen**
3.  Auf der rechten Seite den Eintrag **Microsoft Office 365 Identitätsplattform** aus.
![Entfernen Sie Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
1.  Führen Sie auf einem Computer, [Azure Active Directory Modul für Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installiert ist Folgendes: `$cred=Get-Credential`.  
2.  Geben Sie den Benutzernamen und das Kennwort des globalen Administrators für Azure AD-Domäne, der Sie mit Föderation
2.  Geben Sie im PowerShell`Connect-MsolService -Credential $cred`
4.  Geben Sie im PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Dies ist für die ursprüngliche Domäne.  Verwenden der oben aufgeführten Domänen wäre:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Gehen Sie folgendermaßen vor, um die neue Domäne der obersten Ebene mit PowerShell hinzuzufügen

1.  Führen Sie auf einem Computer, [Azure Active Directory Modul für Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installiert ist Folgendes: `$cred=Get-Credential`.  
2.  Geben Sie den Benutzernamen und das Kennwort des globalen Administrators für Azure AD-Domäne, der Sie mit Föderation
2.  Geben Sie im PowerShell`Connect-MsolService -Credential $cred`
3.  Geben Sie im PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Gehen Sie zum Hinzufügen der neuen Domäne höchster Ebene mit Azure AD verbinden.

1.  Starten von Azure AD Connect vom Desktop oder im Startmenü
2.  Wählen Sie "Hinzufügen einer zusätzlichen Azure AD-Domäne" ![eine zusätzliche Azure AD-Domäne](./media/active-directory-multiple-domains/add1.png)
3.  Geben Sie Ihre Azure Anzeige und Active Directory-Anmeldeinformationen
4.  Wählen Sie die zweite Domäne für die Föderation konfigurieren möchten.
![Hinzufügen zusätzlicher Azure AD-Domäne](./media/active-directory-multiple-domains/add2.png)
5.  Klicken Sie auf Installieren


### <a name="verify-the-new-top-level-domain"></a>Überprüfen Sie die neue Domäne der obersten Ebene
Mit dem Befehl PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`aktualisierten IssuerUri anzeigen.  Der folgende Screenshot zeigt der Föderation Einstellungen auf unsere ursprüngliche Domäne http://bmcontoso.com/adfs/services/trust aktualisiert wurden

![MsolDomainFederationSettings abrufen](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Und der IssuerUri in unseren neuen Domäne zu https://bmfabrikam.com/adfs/services/trust

![MsolDomainFederationSettings abrufen](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Unterstützung für Unterdomänen
Beim Hinzufügen einer untergeordneten Domäne wegen behandelt wie Azure AD-Domänen erbt es die Einstellungen des übergeordneten Elements.  Dies bedeutet, dass die IssuerUri die Eltern übereinstimmen muss.

So sagen beispielsweise, fügen Sie corp.bmcontoso.com und bmcontoso.com haben.  Dies bedeutet, dass die IssuerUri für einen Benutzer von corp.bmcontoso.com muss **http://bmcontoso.com/adfs/services/trust.**  Aber für Azure AD über implementierte Standardregel generiert ein Token mit einem Emittenten **http://corp.bmcontoso.com/adfs/services/trust.** Wert erforderlich, die nicht mit der Domäne übereinstimmt und Authentifizierung schlägt fehl.

### <a name="how-to-enable-support-for-sub-domains"></a>Aktivieren der Unterstützung für Unterdomänen
Um dieses Problem umgehen, AD FS soll relying Party vertrauen für Microsoft Online aktualisiert werden.  Dazu müssen Sie eine benutzerdefinierte Anspruchsregel konfigurieren, damit es alle Unterdomänen aus der Benutzer-UPN-Suffix Streifen beim Erstellen von benutzerdefinierten Issuer-Wert. 

Die Behauptung dazu:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Gehen Sie folgendermaßen vor, eine benutzerdefinierte Ansprüche unterstützt Unterdomänen hinzufügen.

1.  Öffnen AD FS-Verwaltung
2.  Rechts klicken Sie Microsoft Online RP vertrauen und Anspruchsregeln bearbeiten
3.  Wählen die dritte Anspruchsregel und Ersetzen ![Antrag bearbeiten](./media/active-directory-multiple-domains/sub1.png)
4.  Ersetzen Sie den aktuellen Antrag:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    mit
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Forderung ersetzen](./media/active-directory-multiple-domains/sub2.png)
5.  Klicken Sie auf Ok.  Klicken Sie auf anwenden.  Klicken Sie auf Ok.  AD FS-Verwaltung zu schließen.


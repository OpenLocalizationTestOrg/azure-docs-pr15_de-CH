<properties
    pageTitle="Active Directory Federation Services Management und mit Azure AD Connect | Microsoft Azure"
    description="AD FS-Verwaltung mit Azure AD Connect und Anpassung der AD FS-in Benutzerfunktionalität Azure AD Verbinden mit PowerShell."
    keywords="AD FS, ADFS AD FS Management AAD verbinden verbinden, Anmeldung AD FS Anpassung vertrauen, Office 365, Föderation Identitätsempfänger reparieren"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services-Verwaltung und Anpassung mit Azure AD verbinden

Dieser Artikel Details Aufgaben im Zusammenhang mit Active Directory-Verbunddienste (AD FS), der mit Microsoft Azure Active Directory verbinden ausgeführt werden können und andere AD FS-Aufgaben, die für eine vollständige Konfiguration einen AD FS-Farm.

| Thema | Es umfasst |
|:------|:-------------|
|**AD FS-Verwaltung**|
|[Repariert die Vertrauensstellung](#repairthetrust)| Reparieren die Föderation Vertrauensstellung mit Office 365 |
|[Hinzufügen eines AD FS-server](#addadfsserver) | Erweitern von AD FS-Farm mit einem zusätzlichen AD FS-server|
|[Hinzufügen einer AD FS Web-Proxy-server](#addwapserver) | Die AD FS-Farm zusätzliche WAP Server erweitern|
|[Hinzufügen einer föderierten Domäne](#addfeddomain)| Hinzufügen einer föderierten Domäne|
| **AD FS-Anpassung**|
|[Hinzufügen eines benutzerdefinierten Firmenlogo oder Abbildung](#customlogo)| Ein AD FS-Anmeldeseite mit Firmenlogo Abbildung anpassen |
|[Beschreibung-Anmeldung](#addsignindescription) | Hinzufügen einer Beschreibung-Anmeldeseite |
|[AD FS Anspruchsregeln bearbeiten](#modclaims) | AD FS-Ansprüchen für Szenarien Föderation ändern |

## <a name="ad-fs-management"></a>AD FS-Verwaltung

Azure AD Connect bietet verschiedene Aufgaben im Zusammenhang mit ADFS Azure AD Verbinden mit minimalem Benutzereingriff ausgeführt werden können. Nach der Installation von Azure AD Connect-Assistent führen Sie den Assistenten erneut aus, um zusätzliche Aufgaben auszuführen.

### Repariert die Vertrauensstellung<a name=repairthetrust></a>

Azure AD verbinden kann für den aktuellen Zustand der Vertrauensstellung AD FS und Azure Active Directory überprüfen und entsprechende Maßnahmen die Vertrauensstellung zu reparieren. So reparieren Sie Ihre Azure AD und AD FS.

1. Wählen Sie aus der Liste zusätzliche Aufgaben **Reparatur AAD und ADFS vertrauen** .
![Reparieren von AAD und ADFS vertrauen](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Auf der Seite **Connect to Azure AD** Ihre globalen Administrator-Anmeldeinformationen für Azure AD, und klicken Sie auf **Weiter**.
![Verbinden mit Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Geben Sie auf der Seite **Anmeldeinformationen** die Anmeldeinformationen für den Domänenadministrator.
![RAS-Anmeldeinformationen](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Nachdem Sie auf **Weiter**klicken, Azure AD Connect Health Certificate überprüft und Probleme anzeigen.

    ![Status der Zertifikate](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Konfigurieren** Seite zeigt die Liste der Aktionen, die ausgeführt wird, um die Vertrauensstellung zu reparieren.

    ![Konfigurieren](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Klicken Sie auf **Installieren** , um die Vertrauensstellung zu reparieren.

>[AZURE.NOTE] Azure AD Connect kann nur Reparatur oder auf die selbstsignierte Zertifikate. Azure AD Connect Drittanbieter-Zertifikate nicht repariert werden.

### Hinzufügen eines AD FS-server<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect erfordert die PFX-Zertifikatdatei einen AD FS-Server hinzufügen. Daher können Sie diesen Vorgang nur, wenn die AD FS-Farm mithilfe von Azure AD-Verbindung konfiguriert.

1. **Bereitstellen einer zusätzlichen Verbundserver** und auf **Weiter.**
![Zusätzliche Verbundserver](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Auf der Seite **Connect to Azure AD** Anmeldeinformationen für Azure AD globaler Administrator und klicken Sie auf **Weiter**.
![Verbinden mit Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Anmeldeinformationen der Domäne Administrator.
![Anmeldeinformationen eines Domänenadministrators](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect fragt das Kennwort PFX-Datei, die Sie beim Konfigurieren der neuen AD FS-Farm mit Azure AD Connect bereitgestellt. Klicken Sie auf **Kennwort Geben Sie** das Kennwort für die PFX-Datei anzugeben.
![Kennwort](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![SSL-Zertifikat angeben](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Geben Sie auf **AD FS-Server** den Servernamen oder die IP-Adresse der AD FS-Farm hinzugefügt werden.
![AD FS-Server](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Klicken Sie auf **Weiter** und durch die letzte Seite **Konfigurieren** . Nach Azure AD Connect AD FS-Farm Server hinzufügen, Sie können die Verbindung überprüfen erhalten.
![Konfigurieren](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Installation abgeschlossen](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Hinzufügen einer AD FS Web-Proxy-server<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD-Verbindung erfordert PFX-Zertifikatdatei einen Webproxyserver Anwendung hinzufügen. Daher werden für diesen Vorgang nur, wenn Sie AD FS-Farm konfiguriert, mit Azure AD verbinden können.

1. **Bereitstellen von Web Application Proxy** aus der Liste der verfügbaren Aufgaben auswählen
![Webproxy-Anwendung bereitstellen](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Bereitstellen Sie Azure globaler Administrator-Anmeldeinformationen.
![Verbinden mit Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Geben Sie das Kennwort für die PFX-Datei, die Sie beim Konfigurieren der AD FS-Farm mit Azure AD Connect bereitgestellt, auf der Seite **Geben Sie SSL-Zertifikat** .
![Kennwort](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![SSL-Zertifikat angeben](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Fügen Sie den Server als ein Webproxy-Anwendung hinzugefügt werden. Da Web Application Proxyserver nicht der Domäne hinzugefügt werden kann, fragt der Assistent für administrative Anmeldeinformationen auf dem Server hinzugefügt wird.
![Administrative Serveranmeldeinformationen](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Anmeldeinformationen Sie auf der Seite **vertrauen Proxyanmeldeinformationen** administrative vertrauen Proxy konfigurieren und den Zugriff auf den primären Server in der Farm AD FS.
![Proxyanmeldeinformationen vertrauen](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Auf der Seite **Konfigurieren** zeigt der Assistent die Liste der Aktionen, die ausgeführt wird.
![Konfigurieren](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Klicken Sie auf **Installieren** , um die Konfiguration abzuschließen. Nach Abschluss die Konfiguration bietet der Assistent die Option, die Verbindung zu den Servern überprüfen. Klicken Sie auf **Überprüfen** , um Konnektivität zu überprüfen.
![Installation abgeschlossen](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Hinzufügen einer föderierten Domäne<a name=addfeddomain></a>

Es ist leicht zu einer Domäne mit Azure mithilfe von Azure AD-Verbindung verbunden sein. Azure AD Connect fügt die Domäne für die Föderation und Anspruchsregeln entsprechend richtig Emittenten bei mehreren Domänen Verbund mit Azure AD ändert.

1. Wählen Sie den Vorgang zum Hinzufügen einer Föderationsdomäne **eine zusätzliche Azure AD-Domäne**.
![Zusätzliche Azure AD-Domäne](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Auf der nächsten Seite des Assistenten Anmeldeinformationen der globale Administrator für Azure AD.
![Verbinden mit Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Auf der Seite **Anmeldeinformationen** die Anmeldeinformationen der Domäne Administrator.
![RAS-Anmeldeinformationen](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Auf der nächsten Seite bietet der Assistent eine Liste von Azure AD-Domänen mit dem lokalen Verzeichnis Föderation. Wählen Sie die Domäne aus der Liste aus.
![Azure AD-Domäne](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Nach Auswahl die Domäne wird der Assistent Ihnen entsprechende Informationen über weitere Aktionen, die der Assistent ausführen und die Auswirkung der Konfiguration. In einigen Fällen Wenn Sie Domäne auswählen, die noch nicht in Azure AD überprüft bieten der Assistent Informationen zur Domäne überprüfen. Weitere Informationen finden Sie in [Ihrem benutzerdefinierten Domänennamen in Azure Active Directory hinzufügen](active-directory-add-domain.md) .

5. Klicken Sie auf **Weiter**, und **Konfigurieren** Seite zeigt eine Liste der Aktionen, die Azure AD Connect ausgeführt wird. Klicken Sie auf **Installieren** , um die Konfiguration abzuschließen.
![Konfigurieren](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS-Anpassung

Die folgenden Abschnitte enthalten Informationen zu einigen gängigen Aufgaben, die Sie möglicherweise ausführen, wenn AD FS-Seite anpassen.

### Hinzufügen eines benutzerdefinierten Firmenlogo oder Abbildung<a name=customlogo></a>

Ändern das Logo des Unternehmens, die auf **der Seite** angezeigt wird, verwenden Sie die folgenden Windows PowerShell-Cmdlets und Syntax.

> [AZURE.NOTE] Empfohlen für das Logo sind 260 x 35 @ 96 dpi nicht größer als 10 KB Größe.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Der *TargetName* -Parameter ist erforderlich. Mit AD FS veröffentlicht Standarddesign heißt standardmäßig.


### Beschreibung-Anmeldung<a name=addsignindescription></a>

Um eine Anmeldeseite zur **Anmeldeseite**hinzuzufügen, verwenden Sie die folgenden Windows PowerShell-Cmdlets und Syntax.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### AD FS Anspruchsregeln bearbeiten<a name=modclaims></a>

AD FS unterstützt eine umfangreiche Anspruch, mit denen Sie benutzerdefinierte Anspruchsregeln erstellen. Weitere Informationen finden Sie in [Der Rolle des Anspruchsregelsprache](https://technet.microsoft.com/library/dd807118.aspx).

Die folgenden Abschnitte beschreiben, wie Sie benutzerdefinierte Regeln für einige Szenarien zu Azure AD schreiben und AD FS-Verbund.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Unveränderliche ID abhängig von einem Wert im Attribut vorhanden

Azure AD Connect Angabe ein Attribut, das als quellanker verwendet werden, wenn Objekte in Azure Active Directory synchronisiert werden. Wenn der Wert im benutzerdefinierten Attribut nicht leer ist, empfiehlt es sich, einen unveränderlichen ID Anspruch ausstellen. Sie können z. B. **ms-ds-Consistencyguid** als Attribut für den quellanker auswählen und möchten **ImmutableID** als **ms-ds-Consistencyguid** bei das Attribut einen Wert dafür. Wenn kein Wert für das Attribut ist, stellen Sie **ObjectGuid** unveränderlich ID aus  Sie können die Menge der benutzerdefinierten Anspruch wie im folgenden Abschnitt beschrieben erstellen.

**Regel 1: Abfragen von Attributen**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

In dieser Regel werden die Werte der **ms-ds-Consistencyguid** und **Objekt-GUID** für den Benutzer aus Active Directory Abfragen. Ändern des Speichernamens in entsprechenden Speichernamens in der AD FS-Bereitstellung. Außerdem ändern Sie Ansprüche auf eine ordnungsgemäße Anträge für die Föderation **ObjectGuid** und **ms-ds-Consistencyguid**definiert.

Auch **Hinzufügen** und kein **Problem**, Sie vermeiden ausgehende Problem für die Entität und können Werte als temporäre Werte verwenden. Stellt die Forderung einer späteren Regel nach dem Herstellen der Wert als unveränderlich-ID

**Regel 2: Prüfen Sie, ob ms-ds-Consistencyguid für den Benutzer vorhanden ist**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Diese Regel definiert ein temporäres Flag namens **Idflag** auf **Useguid** festgelegt ist, ist keine **ms-ds-Concistencyguid** des Benutzers ausgefüllt. Die Logik ist die Tatsache, dass AD FS nicht leere Ansprüche. Also beim Hinzufügen von Ansprüchen http://contoso.com/ws/2016/02/identity/claims/objectguid und http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid in Regel 1 haben am Ende einer **Msdsconsistencyguid** Anspruch nur, wenn der Wert für den Benutzer ausgefüllt wird. Wenn nicht ausgefüllt ist, sieht AD FS, sie haben einen leeren Wert sofort löscht. Alle Objekte werden **ObjectGuid**haben Anspruch immer werden nach Regel 1 ausgeführt wird.

**Regel 3: Ausstellen Sie ms-ds-Consistencyguid als unveränderlich ID, falls vorhanden**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dies ist eine implizite Überprüfung **vorhanden** . Der Wert für den Anspruch vorhanden, dann ausstellen Sie unveränderlich ID Im vorhergehenden Beispiel wird die Forderung **Nameidentifier** . Sie haben das entsprechende Anspruchstyp unveränderlich ID in Ihrer Umgebung ändern.

**Regel 4: Ausstellen Sie ObjectGuid als unveränderlich ID, wenn ms-ds-ConsistencyGuid nicht vorhanden ist**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In dieser Regel überprüfen Sie einfach temporäres Flag **Idflag**. Sie entscheiden, ob die Forderung basierend auf ihrem Wert ausgeben.

> [AZURE.NOTE] Die Reihenfolge dieser Regeln ist wichtig.

#### <a name="sso-with-a-subdomain-upn"></a>SSO mit einer Subdomäne UPN

Sie können mehr als eine Domäne mithilfe von Azure AD Connect Siehe [Hinzufügen einer neuen föderierten Domäne](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)verbunden sein. Sie müssen den UPN-Anspruch ändern, sodass Aussteller-ID der Stammdomäne und nicht die Domäne entspricht da verbundenen Stammdomäne auch die untergeordneten umfasst.

Standardmäßig ist die Forderung für Aussteller-ID als Regelsatz:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standardmäßige Aussteller-ID Forderung](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Die Standardregel einfach verwendet das UPN-Suffix und Aussteller-ID Forderung. Angenommen, John ist ein Benutzer in sub.contoso.com und contoso.com Verbund mit Azure AD. John tritt john@sub.contoso.com als Benutzernamen bei der Anmeldung bei Azure AD und die Herausgeber-ID Anspruch Regel in AD FS wird auf folgende Weise behandelt.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Anspruchswert:** http://sub.contoso.com/adfs/services/trust/

Damit nur die Stammdomäne in der Anspruchswert Aussteller, Ändern der Anspruchsregel folgt.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie über [Benutzer - Optionen](active-directory-aadconnect-user-signin.md).

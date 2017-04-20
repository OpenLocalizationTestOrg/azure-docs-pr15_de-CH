<properties
    pageTitle="Sichere Cloudressourcen Azure MFA mit AD FS"
    description="Dies ist die Seite Azure mehrstufige Authentifizierung die Verwendung Azure MFA mit AD FS in der Cloud beginnen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Sichern von Cloudressourcen Azure mehrstufige Authentifizierung mit AD FS

Verwenden Sie Ihre Organisation Azure Active Directory verbunden ist, Azure mehrstufige Authentifizierung oder Active Directory Federation Services für Ressourcen, die von Azure AD zugegriffen werden. Gehen Sie Azure Active Directory-Ressourcen mit Azure mehrstufige Authentifizierung oder Active Directory Federation Services sichern.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Sichere Verwendung von AD FS Azure AD-Ressourcen

Um Ihre Cloud-Ressourcen zu schützen Konto für Benutzer aktivieren zunächst Ansprüche Regel einrichten. Gehen Sie folgendermaßen vor, um die Schritte:

1. Verwenden Sie die Schritte [zu umgehen mehrstufige Authentifizierung für Benutzer](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) ein Konto aktivieren.
2. Die AD FS-Verwaltungskonsole zu starten.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Navigieren Sie zu **Verlassen vertrauen** und mit der rechten Maustaste auf die verlassen Partei vertrauen. Wählen Sie **Bearbeiten Anspruchsregeln...**
4. Klicken Sie auf **Regel... hinzufügen**
5. Wählen Sie in der Dropdown-Liste **eine benutzerdefinierte Regel Ansprüche senden** und auf **Weiter**.
6. Geben Sie einen Namen für die Anspruchsregel.
7. Benutzerdefinierte Regel: Fügen Sie den folgenden Text:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Entsprechende Forderung:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Klicken Sie auf **OK** und **Beenden**. Schließen Sie die AD FS-Verwaltungskonsole.

Benutzer können dann ausfüllen mit lokalen Methode (z. B. Smartcard) anmelden.

## <a name="trusted-ips-for-federated-users"></a>Vertrauenswürdige IP-Adressen für Föderationsbenutzer
Vertrauenswürdige IP-Adressen können Administratoren Bypass zweistufigen Authentifizierung für bestimmte IP-Adressen oder für Verbundbenutzer, die Anfragen von eigenen Intranet. In den folgenden Abschnitten wird beschrieben, wie Azure mehrstufige Authentifizierung vertrauenswürdige IP-Adressen mit Föderationsbenutzern und Bypass zweistufigen Authentifizierung konfigurieren, wenn eine Anforderung Intranet Föderationsbenutzer stammt. Dies geschieht durch AD FS eine Pass-Through-oder Filtern einer eingehenden Anspruch Vorlage Anspruchstyp im Unternehmensnetzwerk konfigurieren.

Dieses Beispiel verwendet Office 365 für unsere verlassen vertrauen.

### <a name="configure-the-ad-fs-claims-rules"></a>AD FS Ansprüche Regeln konfigurieren

Zunächst müssen wir ist AD FS-Ansprüchen konfiguriert. Wir erstellen zwei Regeln für den Anspruchstyp im Unternehmensnetzwerk und eine weitere für unsere Benutzer angemeldet.

1. AD FS-Verwaltung zu öffnen.
2. Wählen Sie auf der linken Seite **Verlassen vertrauen**.
3. Mit der rechten Maustaste auf **Microsoft Office 365 Identitätsplattform** und wählen **Anspruchsregeln bearbeiten** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Ausstellung transformieren Regeln klicken Sie auf **Regel hinzufügen.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Im Transformieren Anspruch Regel-Assistenten **übergeben oder Filter eines eingehenden Anspruchs** aus der Dropdown-Liste Wählen Sie aus und dann auf **Weiter**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Kontrollkästchen Name der Anspruchsregel benennen Sie der Regel. Beispiel: InsideCorpNet.
7. Die Dropdown-Liste neben eingehenden Anspruchstyp, wählen **Im Unternehmensnetzwerk**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klicken Sie auf **Fertig stellen**.
9. Klicken Sie auf Ausgabe transformieren Regeln auf **Regel hinzufügen**.
10. Im Transformieren Anspruch Regel-Assistenten wählen Sie **Senden Ansprüche über eine benutzerdefinierte Regel** aus der Dropdownliste aus und dann auf **Weiter**.
11. Im Feld unter Name der Anspruchsregel: Geben Sie *Behalten Benutzer angemeldet*.
12. Geben Sie im Feld benutzerdefinierte Regel:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klicken Sie auf **Fertig stellen**.
14. Klicken Sie auf **Übernehmen**.
15. Klicken Sie auf **Ok**.
16. AD FS-Verwaltung zu schließen.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurieren von Azure mehrstufige Authentifizierung vertrauenswürdigen IP-Adressen mit Föderationsbenutzern
Die Ansprüche sind, können wir vertrauenswürdigen IP-Adressen konfigurieren.

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie auf der linken Seite auf **Active Directory**.
3. Wählen Sie unter Verzeichnis das Verzeichnis vertrauenswürdigen IP-Adressen eingerichtet werden soll.
4. Für das Verzeichnis ausgewählt haben, klicken Sie auf **Konfigurieren**.
5. Klicken Sie im Abschnitt mehrstufige Authentifizierung auf **Einstellungen verwalten**.
6. Wählen Sie auf der Seite Einstellungen unter Vertrauenswürdige IP-Adressen, **multi-factor-Authentifizierung für Anfragen von Föderationsbenutzern Meine Intranet überspringen.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klicken Sie auf **Speichern**.
8. Nachdem die Updates angewendet wurden, klicken Sie auf **Schließen**.


Das wars! An diesem Punkt müssen Office 365 Föderationsbenutzer nur MFA verwenden, wenn ein Anspruch von außerhalb des Unternehmensintranets stammt.

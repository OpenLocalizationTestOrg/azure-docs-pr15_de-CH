<properties
    pageTitle="Verwenden von Azure MFA Server mit AD FS 2.0 | Microsoft Azure"
    description="Dies ist der Azure-mehrstufige Authentifizierungsseite, die Azure MFA mit AD FS 2.0 Schritte beschreibt."
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

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Sichere Cloud und lokalen Ressourcen Azure mehrstufige Authentifizierungsserver mit AD FS 2.0

Dieser Artikel ist für Organisationen, die lokale Ressourcen schützen möchten, die in Azure Active Directory verbunden oder in der Cloud. Schützen Sie Ihrer Ressourcen mithilfe von Azure mehrstufige Authentifizierungsserver und konfigurieren sie AD FS verwenden, sodass zweistufigen Authentifizierung für hochwertige Endpunkten ausgelöst wird.

Diese Dokumentation umfasst Azure mehrstufige Authentifizierungsserver mit AD FS 2.0.  Weitere Informationen zum [Sichern von Cloud und lokalen Ressourcen Azure mehrstufige Authentifizierungsserver mit Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)zu erhalten.


## <a name="secure-ad-fs-20-with-a-proxy"></a>Sichern von AD FS 2.0 mit einem proxy
Zum Sichern von AD FS 2.0 mit einem Proxy Serverinstallation Azure Mehrfaktor-Authentifizierung auf dem Proxyserver ADFS und den Server konfigurieren.

### <a name="configure-iis-authentication"></a>Konfigurieren der IIS-Authentifizierung

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver **IIS-Authentifizierung** im linken Menü.
2. Klicken Sie auf der Registerkarte **Formular basiert** .
3. Klicken Sie auf die **hinzufügen...** Schaltfläche.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Benutzername, Kennwort und Domänenvariablen automatisch erkennen, geben die Anmelde-URL (z. B. https://sso.contoso.com/adfs/ls) im Dialogfeld Auto-Configure Form-Based Website und klicken Sie auf OK.
5. Das Kontrollkästchen Sie **Azure mehrstufige Authentifizierung müssen Benutzer** Wenn alle Benutzer oder Server und zwei überprüft importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder zweistufige Überprüfung befreit werden, lassen Sie das Feld nicht aktiviert. Weitere Informationen zu diesem Feature finden Sie in der Hilfedatei.
6. Wenn die Seitenvariablen automatisch erkannt werden können, klicken Sie auf die **Geben manuell...** Schaltfläche im Dialogfeld Auto-Configure Form-Based Website.
7. Klicken Sie im Dialogfeld Add Form-Based Website geben Sie die URL der Anmeldeseite ADFS im Feld URL senden (wie https://sso.contoso.com/adfs/ls) und geben Sie eine Anwendung (optional). Name der Anwendung in Azure mehrstufige Authentifizierung Berichten angezeigt und kann in SMS oder Mobile App Authentifizierungsnachrichten angezeigt. Finden Sie die Hilfedatei für Weitere Informationen auf die URL senden.
8. Legen Sie das Format der Anforderung "POST oder GET."
9. Geben Sie den Benutzernamen Variablen (ctl00$ ContentPlaceHolder1$ UsernameTextBox) und Kennwort (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Wenn die formularbasierte Anmeldeseite eine Domäne Textfeld angezeigt wird, geben Namen Sie den Domäne sowie. Sie müssen zur Login-Seite in einem Webbrowser navigieren, mit der rechten Maustaste auf die Seite und wählen **Quelle anzeigen** der Namen der Eingabefelder in der Anmeldeseite.
10. Das Kontrollkästchen Sie **Azure mehrstufige Authentifizierung müssen Benutzer** Wenn alle Benutzer oder Server und zwei überprüft importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder zweistufige Überprüfung befreit werden, lassen Sie das Feld nicht aktiviert.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klicken Sie auf **Erweitert...** die Schaltfläche Erweitert überprüfen. Sie können einschließlich der Möglichkeit, eine benutzerdefinierte Denial Auslagerungsdatei wählen erfolgreiche Authentifizierungen für die Website Cookies zwischengespeichert und Auswählen der primären Anmeldeinformationen konfigurieren.
12. Da der Proxyserver ADFS nicht zu der Domäne hinzugefügt werden, können Sie LDAP, Verbindung mit dem Domänencontroller für Benutzer importieren und die Vorauthentifizierung. Klicken Sie im Dialogfeld Advanced Form-Based Website auf die Registerkarte **Authentifizierung** , und wählen Sie die Vorauthentifizierung Authentifizierungstyp **LDAP-Bindung** .
13. Klicken Sie auf **OK** , um das Dialogfeld Add Form-Based Website zurückzugeben. Die Hilfedatei für Weitere Informationen auf den erweiterten Einstellungen anzeigen
14. Klicken Sie auf **OK** , um das Dialogfeld zu schließen.
15. Die Variablen URL und Seite erkannt oder eingegeben haben, zeigt die Websitedaten im Bereich formularbasierte.
16. Klicken Sie auf der Registerkarte **Systemeigene Modul** und wählen den Server, der Website der ADFS-Proxy unter (wie "Standardwebsite") ausgeführt wird oder ADFS-Proxy-Anwendung (wie "ls" unter "Adfs") auf die gewünschte plug-in IIS aktivieren.
17. Klicken Sie auf den oberen Rand des Bildschirms **Aktivieren Sie IIS-Authentifizierung** .
18. Die IIS-Authentifizierung ist jetzt aktiviert.

### <a name="configure-directory-integration"></a>Konfigurieren der Verzeichnisintegration

IIS-Authentifizierung aktiviert, aber so die Vorauthentifizierung auf Ihrem Active Directory (AD) über LDAP müssen die LDAP-Verbindung mit dem Domänencontroller konfigurieren.

1. Klicken Sie auf das Symbol **Verzeichnisintegration** .
2. Auf der Registerkarte das Optionsfeld **bestimmten LDAP-Konfiguration verwenden** .
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Klicken Sie auf das **Bearbeiten...** Schaltfläche.
4. Füllen Sie im Dialogfeld LDAP-Konfiguration bearbeiten die Felder mit den Informationen zum Active Directory-Domänencontroller herstellen. Beschreibung der Felder sind in der folgenden Tabelle enthalten. Diese Informationen sind auch in der Azure mehrstufige Authentifizierungsserver enthalten.
5. Testen Sie die LDAP-Verbindung **Testen** klicken.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Wenn der LDAP-Verbindungstest erfolgreich war, klicken Sie auf die Schaltfläche **OK** .

### <a name="configure-company-settings"></a>Unternehmen konfigurieren

1. Anschließend klicken Sie auf **Einstellungen** und Registerkarte **Benutzernamen Auflösung** .
2. Aktivieren Sie das Optionsfeld **LDAP verwenden eindeutiger Bezeichnerattribut für übereinstimmende Benutzernamen** .
3. Wenn Benutzer ihren Benutzernamen im Format "Domäne\Benutzername" eingeben, muss der Server Bereich aus dem Benutzernamen entfernt, wenn die LDAP-Abfrage erstellt. Dies kann über einen Registrierungseintrag erfolgen.
4. Öffnen Sie den Registrierungseditor, und gehen Sie zu HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/positiv Netzwerke/PhoneFactor auf einem 64-Bit-Server. Wenn übernehmen Sie eine 32-Bit-Server "Wow6432Node" aus dem Weg. Erstellen Sie einen DWORD-Registrierungsschlüssel namens "UsernameCxz_stripPrefixDomain" und legen Sie den Wert auf 1. Azure mehrstufige Authentifizierung sichern jetzt ADFS-Proxy.

Stellen Sie sicher, dass Benutzer aus Active Directory in den Server importiert wurden. Finden Sie im [Abschnitt vertrauenswürdige IP -Adressen](#trusted-ips) , möchten Sie weiße Liste interner IP-Adressen, zweistufigen Authentifizierung nicht erforderlich ist, wenn von diesen Speicherorten auf der Website anmelden.

<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 direkt, ohne proxy

Sie können AD FS sichern, wenn AD FS-Proxy nicht verwendet wird. Azure mehrstufige Authentifizierungsserver auf dem AD FS-Server installieren und Konfigurieren des Servers pro die folgenden Schritte aus:

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver **IIS-Authentifizierung** im linken Menü.
2. Klicken Sie auf **HTTP** .
3. Klicken Sie auf die **hinzufügen...** Schaltfläche.
4. Geben Sie im Dialogfenster Basis-URL hinzufügen die URL für die ADFS-Website, in HTTP-Authentifizierung (wie https://sso.domain.com/adfs/ls/auth/integrated) im Feld Basis-URL ausgeführt wird. Geben Sie einen Anwendungsnamen (optional). Name der Anwendung in Azure mehrstufige Authentifizierung Berichten angezeigt und kann in SMS oder Mobile App Authentifizierungsnachrichten angezeigt.
5. Ggf. Anpassen der Leerlauftimeout und maximale Sitzungsdauer.
6. Das Kontrollkästchen Sie **Azure mehrstufige Authentifizierung müssen Benutzer** Wenn alle Benutzer oder Server und zwei überprüft importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder zweistufige Überprüfung befreit werden, lassen Sie das Feld nicht aktiviert. Weitere Informationen zu diesem Feature finden Sie in der Hilfedatei.
7. Das Kontrollkästchen Sie Cookie-Cache bei Bedarf.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klicken Sie auf **OK** .
9. Klicken Sie auf der Registerkarte **Systemeigene Modul** und wählen Sie Server, Websites (wie "Standardwebsite") oder ADFS-Anwendung (z. B. "ls" unter "Adfs" aus) auf die gewünschte plug-in IIS aktivieren.
10. Klicken Sie auf den oberen Rand des Bildschirms **Aktivieren Sie IIS-Authentifizierung** . Azure mehrstufige Authentifizierung sichern jetzt ADFS.

Stellen Sie sicher, dass Benutzer aus Active Directory in den Server importiert wurden. Im Abschnitt vertrauenswürdige IP-Adressen möchten Sie weiße Liste interner IP-Adressen, zweistufigen Authentifizierung nicht erforderlich ist, wenn von diesen Speicherorten auf der Website anmelden.


## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen
Vertrauenswürdige IP-Adressen können Benutzer Azure mehrstufige Authentifizierung für Website-Anfragen von bestimmten IP-Adressen oder Subnetze zu umgehen. Beispielsweise möchten Sie ausgenommen Benutzer zwei Überprüfung beim Anmelden im Büro. Dazu würden Sie Office Subnetz als Eintrag Vertrauenswürdige IP-Adressen angeben.

### <a name="to-configure-trusted-ips"></a>Konfigurieren von vertrauenswürdigen IP-Adressen


1. Klicken Sie auf die Registerkarte **Vertrauenswürdige IP -Adressen** im Abschnitt IIS-Authentifizierung.
1. Klicken Sie auf die **hinzufügen...** Schaltfläche.
1. Erscheint das Dialogfeld Vertrauenswürdige IP-Adressen hinzufügen wählen Sie eine **Einzelne IP-Adresse**, **IP-Adressbereich**oder **Subnetz** Optionsschaltflächen.
1. Geben Sie die IP-Adresse, IP-Adressbereich oder Subnetz weißen Liste hinzugefügt werden soll. Wenn ein Subnetz, wählen Sie die geeignete Netzmaske und klicken Sie auf **OK** . Die vertrauenswürdige IP-Adresse wurde nun hinzugefügt.


<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>

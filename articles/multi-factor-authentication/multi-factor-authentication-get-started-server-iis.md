<properties 
    pageTitle="IIS-Authentifizierung und Azure Multi-Factor Authentication Server"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die bei IIS-Authentifizierung und Azure mehrstufige Authentifizierungsserver bereitstellen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="iis-authentication"></a>IIS-Authentifizierung

IIS-Authentifizierung Teil Azure mehrstufige Authentifizierungsserver können Sie aktivieren und Konfigurieren von IIS-Authentifizierung für die Integration mit Microsoft IIS Web Applications. Azure mehrstufige Authentifizierungsserver installiert ein plug-in die Anforderungen an den IIS-Webserver um Azure mehrstufige Authentifizierung hinzufügen filtern können. IIS-Plug-in bietet Unterstützung für die formularbasierte Authentifizierung und HTTP-Authentifizierung. Vertrauenswürdige IPs ausgenommen internen IP-Adressen von zwei-Faktor-Authentifizierung konfiguriert werden kann.


![IIS-Authentifizierung](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>IIS formularbasierte Authentifizierung mit Azure Multi-Factor Authentication Server

Zum Sichern von IIS Web-Anwendung, die formularbasierte Authentifizierung verwendet installieren Sie Azure mehrstufige Authentifizierungsserver, auf dem IIS-Webserver und konfigurieren Sie Server pro folgendermaßen.

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver IIS-Authentifizierung im linken Menü.
2. Klicken Sie auf der Registerkarte Formular basiert.
3. Klicken Sie auf Hinzufügen... Schaltfläche.
4. Zur Benutzername Kennwort und Domäne Variablen automatisch, geben Sie die Anmelde-URL (z. B. https://localhost/contoso/auth/login.aspx) im Dialogfeld Auto-Configure Form-Based Website und klicken Sie auf OK.
5. Das Kontrollkästchen Sie mehrstufige Authentifizierung müssen Benutzer Übereinstimmung Wenn alle Benutzer oder Server und mehrstufige Authentifizierung importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder kombinierte Authentifizierung befreit werden, lassen Sie das Feld nicht aktiviert.
6. Wenn die Seitenvariablen automatisch erkannt werden können, klicken Sie auf die geben manuell... Schaltfläche im Dialogfeld Auto-Configure Form-Based Website.
7. Klicken Sie im Dialogfeld Add Form-Based Website geben Sie die URL der Anmeldeseite im Feld URL senden und geben Sie eine Anwendung (optional). Name der Anwendung in Azure mehrstufige Authentifizierung Berichten angezeigt und kann in SMS oder Mobile App Authentifizierungsnachrichten angezeigt. Finden Sie die Hilfedatei für Weitere Informationen auf die URL senden.
8. Wählen Sie das richtige Anforderungsformat. Dies ist für die meisten ASP.NET-Webanwendungen auf "POST oder GET" festgelegt.
9. Geben Sie Variable Username, Kennwortvariable und Domain-Variable (Wenn sie auf der Anmeldeseite angezeigt wird). Sie müssen zur Anmeldungsseite in einem Webbrowser mit der rechten Maustaste auf die Seite und wählen Sie den Namen der Eingabefelder auf der Seite "Quelltext anzeigen".
10. Das Kontrollkästchen Sie Azure mehrstufige Authentifizierung müssen Benutzer Übereinstimmung Wenn alle Benutzer oder Server und mehrstufige Authentifizierung importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder kombinierte Authentifizierung befreit werden, lassen Sie das Feld nicht aktiviert. Siehe Hilfedatei für Weitere Informationen zu dieser Funktion.
11.  Klicken Sie auf Erweitert... die Schaltfläche Erweitert, einschließlich der Möglichkeit, eine benutzerdefinierte Denial Auslagerungsdatei wählen erfolgreiche Authentifizierungen auf der Website für einen Zeitraum mit Cookies zwischengespeichert und wählen, ob der primäre Anmeldeinformationen für die Windows-Domäne, einem LDAP-Verzeichnis oder einen RADIUS-Server zu überprüfen. Klicken Sie auf OK, um das Dialogfeld Add Form-Based Website zurückzukehren. Die Hilfedatei für Weitere Informationen auf den erweiterten Einstellungen anzeigen
12. Klicken Sie auf die Schaltfläche OK.
13. Die Variablen URL und Seite erkannt oder eingegeben wurden, zeigt die Websitedaten im Bereich formularbasierte.
14. Finden Sie die IIS aktivieren Plug-Ins für Azure mehrstufige Authentifizierungsserver direkt Abschnitt die IIS-Konfiguration abgeschlossen.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Verwenden integrierte Windows-Authentifizierung mit Azure Multi-Factor Authentication Server

Um IIS Web-Anwendung sichern, die HTTP integrierte Windows-Authentifizierung verwendet, installieren Sie Azure mehrstufige Authentifizierungsserver, auf dem IIS-Webserver und konfigurieren Sie Server pro folgendermaßen.

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver IIS-Authentifizierung im linken Menü.
2. Klicken Sie auf HTTP. Klicken Sie auf der Registerkarte Formular basiert.
3. Klicken Sie auf Hinzufügen... Schaltfläche.
4. Im Dialogfenster Basis-URL hinzufügen Geben Sie die URL für die Webseite HTTP-Authentifizierung (z.B. http://localhost/owa) ausgeführt im Feld Basis-URL und einen Anwendungsnamen (optional). Name der Anwendung in Azure mehrstufige Authentifizierung Berichten angezeigt und kann in SMS oder Mobile App Authentifizierungsnachrichten angezeigt.
5. Passen Sie Leerlaufzeitlimit und maximale Sitzungsdauer ist standardmäßig nicht.
6. Das Kontrollkästchen Sie mehrstufige Authentifizierung müssen Benutzer Übereinstimmung Wenn alle Benutzer oder Server und mehrstufige Authentifizierung importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder kombinierte Authentifizierung befreit werden, lassen Sie das Feld nicht aktiviert. Siehe Hilfedatei für Weitere Informationen zu dieser Funktion.
7. Das Kontrollkästchen Sie Cookie-Cache bei Bedarf.
8. Klicken Sie auf die Schaltfläche OK.
9. Abschnitt [Aktivieren Sie IIS-Plugins Azure mehrstufige Authentifizierungsserver](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) direkt nach der IIS-Konfiguration abzuschließen.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Aktivieren Sie IIS-Plug-ins für Azure Multi-Factor Authentication Server

Nachdem Sie die formularbasierte konfiguriert haben oder HTTP-Authentifizierung URLs und Einstellungen müssen Sie die Speicherorte, Azure mehrstufige Authentifizierung IIS-Plug-ins geladen und aktiviert werden soll, in IIS auswählen. Gehen Sie folgendermaßen vor:

1. Wenn auf IIS 6, klicken Sie auf die Registerkarte ISAPI und wählen Sie die Website, die die Anwendung unter (z. B. Standardwebsite) ausgeführt wird, um die für diese Site plug-in Azure mehrstufige Authentifizierung ISAPI-Filter zu aktivieren.
2. Läuft unter IIS 7 oder höher, klicken Sie auf der Registerkarte systemeigene Modul, und wählen Sie Server, Websites oder Anwendung(en) plug-in IIS auf die gewünschte Verschlüsselungsstufe aktivieren.
3. Kontrollkästchen Sie aktivieren Sie die IIS-Authentifizierung am oberen Bildschirmrand. Azure mehrstufige Authentifizierung sichern jetzt ausgewählte IIS-Anwendung. Stellen Sie sicher, dass Benutzer in den Server importiert wurden. Siehe Abschnitt vertrauenswürdige IP-Adressen möchten zur weißen Liste interner IP-Adressen, die zwei-Faktor-Authentifizierung bei der Anmeldung auf der Website von Speicherorten nicht erforderlich ist.


## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen

Vertrauenswürdige IP-Adressen können Benutzer Azure mehrstufige Authentifizierung für Website-Anfragen von bestimmten IP-Adressen oder Subnetze zu umgehen. Beispielsweise empfiehlt Benutzer von Azure mehrstufige Authentifizierung bei der Anmeldung im Büro. Dazu würden Sie Office Subnetz als Eintrag Vertrauenswürdige IP-Adressen angeben. Konfigurieren von vertrauenswürdigen IP-Adressen gehen Sie folgendermaßen vor:

1. Klicken Sie auf die Registerkarte Vertrauenswürdige IP-Adressen im Abschnitt IIS-Authentifizierung.
2. Klicken Sie auf Hinzufügen... Schaltfläche.
3. Das Dialogfeld Vertrauenswürdige IP-Adressen hinzufügen angezeigt, wählen Sie einzelne IP-Adresse, IP-Bereich oder Subnet Optionsfeld.
4. Geben Sie die IP-Adresse, IP-Adressbereich oder Subnetz weißen Liste hinzugefügt werden soll. Wenn ein Subnetz, wählen Sie die geeignete Netzmaske und klicken Sie auf OK. Die weiße Liste wurde nun hinzugefügt.

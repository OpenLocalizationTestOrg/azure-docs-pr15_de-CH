<properties 
    pageTitle="LDAP-Authentifizierung und Azure Multi-Factor Authentication Server"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die bei LDAP-Authentifizierung und Azure mehrstufige Authentifizierungsserver bereitstellen."
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

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-Authentifizierung und Azure Multi-Factor Authentication Server


Standardmäßig sieht Azure mehrstufige Authentifizierungsserver importieren oder Benutzer aus Active Directory zu synchronisieren. Jedoch kann konfiguriert werden, um mit anderen LDAP-Verzeichnissen wie eine ADAM-Verzeichnis oder bestimmte Active Directory-Domänencontroller zu binden. Bei Verbindung mit einem Verzeichnis über LDAP konfiguriert können Azure mehrstufige Authentifizierungsserver konfiguriert werden als LDAP-Proxy, Authentifizierungen durchgeführt werden. Es ermöglicht auch die Verwendung der LDAP-Bindung als Ziel RADIUS Vorauthentifizierung der Benutzer bei Verwendung der IIS-Authentifizierung oder die Authentifizierung in Azure mehrstufige Authentifizierung User Portal.

Bei Azure Mehrfaktor-Authentifizierung als LDAP-Proxy wird Azure mehrstufige Authentifizierungsserver LDAP-Client (VPN-Anwendung, Anwendung) und der LDAP-Verzeichnisserver zwischen mehrstufige Authentifizierung hinzufügen. Für Azure mehrstufige Authentifizierung funktioniert müssen Azure mehrstufige Authentifizierungsserver die Kommunikation mit dem Client-Server und das LDAP-Verzeichnis konfiguriert. In dieser Konfiguration Azure mehrstufige Authentifizierungsserver akzeptiert LDAP-Anfragen von Client-Server-Applikationen und leitet sie an den LDAP-Zielserver, die primären Anmeldeinformationen überprüft. Die Antwort vom LDAP-Verzeichnis zeigt, dass die primären Anmeldeinformationen gültig sind, Azure mehrstufige Authentifizierung zweite Authentifizierung durchführt und sendet eine Antwort an den LDAP-Client. Die gesamte Authentifizierung wird nur erfolgreich, wenn die Authentifizierung mit dem LDAP-Server und die mehrstufige Authentifizierung erfolgreich.





## <a name="ldap-authentication-configuration"></a>LDAP-Konfiguration


Konfigurieren Sie die LDAP-Authentifizierung Serverinstallation Azure mehrstufige Authentifizierung auf einem WindowsServer. Gehen Sie folgendermaßen vor:

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver LDAP-Authentifizierung im linken Menü.
2. Aktivieren Sie das Kontrollkästchen LDAP-Authentifizierung aktivieren.![LDAP-Authentifizierung](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Ändern Sie auf der Registerkarte Clients TCP- und SSL-Anschluss bei Azure mehrstufige Authentifizierung LDAP-Dienst nicht standardmäßigen Ports gebunden werden soll, LDAP-Anfragen von Clients zu überwachen, die konfiguriert werden.
4. Wenn Sie LDAPS vom Client zum Server Azure mehrstufige Authentifizierung verwenden möchten, muss ein SSL-Zertifikat auf dem Server installiert, die der Server ausgeführt wird. Klicken Sie auf die Schaltfläche Durchsuchen... neben Sie das SSL-Zertifikat, und wählen Sie das installierte Zertifikat für die sichere Verbindung verwendet wird.
5. Klicken Sie auf Hinzufügen... Schaltfläche.
6. Geben Sie im Dialogfeld LDAP-Client hinzufügen die IP-Adresse der Appliance, Server oder Anwendung authentifiziert wird an den Server und einen Anwendungsnamen (optional). Name der Anwendung in Azure mehrstufige Authentifizierung Berichten angezeigt und kann in SMS oder Mobile App Authentifizierungsnachrichten angezeigt.
7. Das Kontrollkästchen Sie Azure Multifaktor-Authentifizierung müssen Benutzer Übereinstimmung Wenn alle Benutzer oder Server und für mehrteilige Authentifizierung importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurden oder für mehrteilige Authentifizierung befreit werden, lassen Sie das Feld nicht aktiviert. Siehe Hilfedatei für Weitere Informationen zu dieser Funktion.
8. Wiederholen Sie Schritte 5 bis 7, um weitere LDAP-Clients hinzufügen.
9. LDAP-Authentifizierung erhalten Azure mehrstufige Authentifizierung konfiguriert, muss Proxy diese Authentifizierungen für das LDAP-Verzeichnis. Daher das Ziel Registerkarte zeigt nur ein einziges, grau Option ein LDAP-Ziel. Konfigurieren Sie die LDAP-verzeichnisverbindung klicken Sie auf Verzeichnisintegration.
10. Wählen Sie auf der Registerkarte das Optionsfeld mit bestimmte LDAP-Konfiguration.
11. Klicken Sie auf Bearbeiten... Schaltfläche.
12. Füllen Sie die Felder mit den Informationen für die Verbindung zum LDAP-Verzeichnis, im Dialogfeld LDAP-Konfiguration bearbeiten. Beschreibung der Felder sind in der folgenden Tabelle enthalten. Hinweis: Diese Informationen ist auch in der Azure mehrstufige Authentifizierungsserver enthalten.![Verzeichnisintegration](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Testen Sie die LDAP-Verbindung testen klicken.
14. Wenn LDAP-Verbindungstest erfolgreich war, klicken Sie auf die Schaltfläche OK.
15. Klicken Sie auf die Registerkarte Filter. Der Server ist vorkonfiguriert, Container, Gruppen und Benutzer aus Active Directory laden. Binden an ein anderes LDAP-Verzeichnis müssen Sie wahrscheinlich die angezeigten bearbeiten. Klicken Sie auf den Link "Hilfe" für Weitere Informationen zu filtern.
16. Klicken Sie auf der Registerkarte Attribute. Der Server ist, Attribute aus Active Directory.
17. Vorkonfigurierte Attribut Zuordnung ändern oder ein anderes LDAP-Verzeichnis zu binden, klicken Sie auf Bearbeiten... Schaltfläche.
18. Ändern Sie im Dialogfeld Attribute bearbeiten LDAP-Attribut Mappings für das Verzeichnis. Attributnamen eingegeben oder ausgewählt werden können, die... neben jedem Feld.
19. Klicken Sie auf den Link "Hilfe" für Weitere Informationen zu Attributen.
20. Klicken Sie auf die Schaltfläche OK.
21. Klicken Sie auf Einstellungen und wählen Sie die Registerkarte Benutzernamen Auflösung.
22. wenn Active Directory von einem Server Domäne herstellen, sollten Sie mithilfe von Windows Sicherheits-IDs (SIDs) nach übereinstimmenden Benutzernamen Optionsfeld verlassen können. Wählen Sie anderenfalls LDAP verwenden eindeutiger Bezeichnerattribut für übereinstimmende Benutzernamen Optionsfeld. Wenn aktiviert, versucht Azure mehrstufige Authentifizierungsserver jeden Benutzernamen eine eindeutige Kennung im LDAP-Verzeichnis zu. Eine LDAP-Suche wird dem Benutzernamen in der Verzeichnisintegration definierten Attribute -> Attribute. Benutzerauthentifizierung Benutzernamen auf den eindeutigen Bezeichner des LDAP-Verzeichnisses gelöst als eindeutige Bezeichner für den Abgleich des Benutzers in der Datendatei Azure mehrstufige Authentifizierung verwendet werden. Dadurch wird bei Vergleichen lange und kurze Benutzername diese Formate schließt Azure mehrstufige Authentifizierung Serverkonfiguration. Der Server jetzt hört konfigurierten Ports für LDAP-Anfragen konfigurierte Clients und Proxy diese Anfragen an das LDAP-Verzeichnis für die Authentifizierung festgelegt.


## <a name="ldap-client-configuration"></a>LDAP-Clientkonfiguration

Verwenden Sie die Richtlinien für das konfigurieren den LDAP-Client:

- Konfigurieren Sie Ihre Anwendung, Server oder Ihre Anwendung über LDAP Azure mehrstufige Authentifizierung Server authentifizieren, als wäre Ihr LDAP-Verzeichnis. Sie sollten die gleiche Einstellung verwenden, die Sie normalerweise verwenden, Ihr LDAP-Verzeichnis außer den Servernamen oder die IP-Adresse die Azure mehrstufige Authentifizierung Server direkt Verbindung.
- Konfigurieren Sie den LDAP-Timeoutwert 30-60 Sekunden Zeit überprüft die Anmeldeinformationen des Benutzers mit dem LDAP-Verzeichnis, die zweite Authentifizierung, erhalten ihre Antwort und reagieren auf die LDAP-Anfrage.
- Wenn LDAPS, vertrauen die Einheit oder einen Server der LDAP-Abfragen auf Azure mehrstufige Authentifizierungsserver installierte Zertifikat.

<properties 
    pageTitle="RADIUS-Authentifizierung und Azure Multi-Factor Authentication Server"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die bei Bereitstellung von RADIUS-Authentifizierung und Azure mehrstufige Authentifizierungsserver."
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
    ms.date="08/15/2016"
    ms.author="kgremban"/>



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>RADIUS-Authentifizierung und Azure Multi-Factor Authentication Server

Abschnitt RADIUS-Authentifizierung können Sie die RADIUS-Authentifizierung für Azure mehrstufige Authentifizierungsserver aktivieren und konfigurieren. RADIUS ist eine Authentifizierung akzeptiert und diese Anfragen zu bearbeiten. Azure mehrstufige Authentifizierungsserver fungiert als RADIUS-Server und Ihre RADIUS-Clients (z. B. VPN-Gerät) bis Authentifizierungsziel, die Active Directory (AD), einem LDAP-Verzeichnis oder einem anderen RADIUS-Server um Azure mehrstufige Authentifizierung hinzufügen eingefügt. Für Azure mehrstufige Authentifizierung funktioniert müssen Sie Azure mehrstufige Authentifizierungsserver konfigurieren, damit es mit der Client-Server und Authentifizierungsziel kommunizieren kann. Azure mehrstufige Authentifizierungsserver akzeptiert Anfragen von RADIUS-Clients, überprüft die Anmeldeinformationen für die Authentifizierungsziel fügt Azure mehrstufige Authentifizierung und sendet eine Antwort an den RADIUS-Client. Die gesamte Authentifizierung wird nur erfolgreich, wenn der primäre Authentifizierung und Azure mehrstufige Authentifizierung erfolgreich.

>[AZURE.NOTE]
>MFA-Server unterstützt nur PAP (Password Authentication-Protokoll) und MSCHAPv2 (Microsoft Challenge Handshake Authentication Protocol)-RADIUS-Protokolle als RADIUS-Server.  Andere Protokolle wie EAP (extensible Authentication Protocol) können verwendet werden, wenn der MFA-Server als RADIUS-Proxy an einen anderen RADIUS-Server dient, wie Microsoft NPS unterstützt.
></br>
>Bei anderen Protokollen in dieser Konfiguration funktioniert unidirektionale SMS und EID Token nicht da MFA-Server keine erfolgreiche RADIUS-Antwort, die dieses Protokoll starten kann.


![RADIUS-Authentifizierung](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>RADIUS-Konfiguration

Um die RADIUS-Authentifizierung konfigurieren Serverinstallation Azure mehrstufige Authentifizierung auf einem WindowsServer. Haben eine Active Directory-Umgebung sollte der Server der Domäne im Netzwerk beigetreten. Gehen Sie zum Konfigurieren des Servers Azure mehrstufige Authentifizierung:

1. Klicken Sie in Azure mehrstufige Authentifizierungsserver RADIUS-Authentifizierung im linken Menü.
2. Kontrollkästchen Sie das Aktivieren RADIUS-Authentifizierung.
3. Ändern Sie auf der Registerkarte Clients Ports für Authentifizierung und Kontoführung Anschlüsse, wenn Azure Mehrfaktor-Authentifizierung RADIUS-Dienst nicht standardmäßigen Ports gebunden werden soll, RADIUS-Anfragen von Clients zu überwachen, die konfiguriert werden.
4. Klicken Sie auf Hinzufügen... Schaltfläche.
5. Geben Sie im Dialogfeld RADIUS-Client hinzufügen die IP-Adresse der Appliance/Server authentifiziert Azure mehrstufige Authentifizierungsserver, ein Anwendungsname (optional) und ein gemeinsamer geheimer Schlüssel. Der gemeinsame geheime Schlüssel müssen auf den Azure mehrstufige Authentifizierungsserver und Appliance-Server identisch sein. Name der Anwendung in Azure mehrstufige Authentifizierung Berichten angezeigt und kann in SMS oder Mobile App Authentifizierungsnachrichten angezeigt.
6. Das Kontrollkästchen Sie mehrstufige Authentifizierung müssen Benutzer Übereinstimmung Wenn alle Benutzer oder Server und mehrstufige Authentifizierung importiert werden. Wenn zahlreiche Benutzer noch nicht in den Server importiert wurde oder kombinierte Authentifizierung befreit werden, lassen Sie das Feld nicht aktiviert. Siehe Hilfedatei für Weitere Informationen zu dieser Funktion.
7. Das Kontrollkästchen Sie Enable fallback EID token Benutzer Azure mehrstufige Authentifizierung mobile Anwendung Authentifizierung verwenden und EID Passcodes als fallback Authentifizierung der Out-of-Band-Telefon, SMS oder Push-Benachrichtigung verwenden möchten.
8. Klicken Sie auf die Schaltfläche OK.
9. Wiederholen Sie Schritte 4 bis 8, um weitere RADIUS-Clients hinzufügen.
10. Klicken Sie auf der Registerkarte Ziel.
11. Wenn Azure mehrstufige Authentifizierungsserver auf einem Server Domäne in einer Active Directory-Umgebung installiert ist, wählen Sie Windows-Domäne.
12. Wenn ein LDAP-Verzeichnis Benutzer authentifiziert werden soll, wählen Sie LDAP-Bindung. Bei Verwendung der LDAP-Bindung müssen Sie Verzeichnisintegration klicken und die LDAP-Konfiguration auf der Registerkarte Bearbeiten, sodass der Server an das Verzeichnis binden kann. Informationen zum Konfigurieren von LDAP finden im Konfigurationshandbuch LDAP-Proxy.
13. Wenn Benutzer für einen anderen RADIUS-Server authentifiziert werden sollen, wählen Sie RADIUS-Server.
14. Konfigurieren Sie den Server, der Server Proxy werden RADIUS-Anfragen an durch Klicken auf Hinzufügen. Schaltfläche.
15. Geben Sie im Dialogfeld RADIUS-Server hinzufügen die IP-Adresse des RADIUS-Servers und ein gemeinsamer geheimer Schlüssel. Der gemeinsame geheime Schlüssel müssen auf der Azure Mehrfaktor-Authentifizierungsserver und den RADIUS-Server identisch sein. Ändern Sie Authentifizierungsanschluss und Kontoführungsport, wenn andere Ports vom RADIUS-Server verwendet werden.
16. Klicken Sie auf die Schaltfläche OK.
17. Sie müssen Azure mehrstufige Authentifizierung-Server als RADIUS-Client in der RADIUS-Server hinzufügen, damit Zugriffe von Azure mehrstufige Authentifizierungsserver gesendet verarbeitet wird. Verwenden Sie den gleichen gemeinsamen geheimen Schlüssel in Azure mehrstufige Authentifizierungsserver konfiguriert.
18. Sie können diesen Schritt zum Hinzufügen weiterer RADIUS-Server und konfigurieren Sie die Reihenfolge, in der der Server mit den Schaltflächen nach oben und nach unten nennen sollte, wiederholen. Die Serverkonfiguration von Azure mehrstufige Authentifizierung abgeschlossen ist. Der Server wartet jetzt konfigurierten Ports für Anfragen RADIUS-Clients konfiguriert.   


## <a name="radius-client-configuration"></a>RADIUS-Clientkonfiguration

Verwenden Sie die Richtlinien für das konfigurieren den RADIUS-Client:

- Konfigurieren der Appliance/Server authentifizieren über RADIUS Azure mehrstufige Authentifizierungsserver IP-Adresse, die als RADIUS-Server fungiert.
- Verwenden Sie den denselben gemeinsamen geheimen Schlüssel, der über konfiguriert wurde.
- Konfigurieren Sie den RADIUS-Timeoutwert 30-60 Sekunden Zeit überprüft die Anmeldeinformationen des Benutzers, mehrstufige Authentifizierung, erhalten ihre Antwort und reagieren auf RADIUS-Access-Request.

<properties 
    pageTitle="Remote Desktop Gateway und Azure mehrstufige Authentifizierungsserver mittels RADIUS"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die bei Bereitstellung von Desktop (RD) Gateway und Azure mehrstufige Authentifizierungsserver mit RADIUS."
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

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Remote Desktop Gateway und Azure mehrstufige Authentifizierungsserver mittels RADIUS

In vielen Fällen verwendet Remotedesktopgateway lokale NPS Benutzer authentifizieren. Dieses Dokument beschreibt die RADIUS, aus den Remotedesktopgateway (über den lokalen NPS) mehrstufige Authentifizierung Server weitergeleitet.

Mehrstufige Authentifizierungsserver sollte installiert werden, auf einem anderen Server, die Proxy die RADIUS-Anforderung an den NPS auf dem Remotedesktop-Gatewayserver. Nachdem NPS Benutzername und Kennwort überprüft, wird den zweiten Faktor der Authentifizierung vor dem zurückgegeben eines Ergebnisses an das Gateway führt eine Antwort an den Authentifizierungsserver mehrstufige zurückgegeben.





## <a name="configure-the-rd-gateway"></a>Konfigurieren von RD-Gateway

RD-Gateway muss zum Senden von RADIUS-Authentifizierung Azure mehrstufige Authentifizierungsserver konfiguriert werden. Installierte RD-Gateway konfiguriert und arbeiten in RD-Gateway-Eigenschaften. Wechseln Sie zur Registerkarte RD-CAP-Speicher und ändern Sie, um einen zentralen Server mit NPS statt lokalen Netzwerkrichtlinienserver verwenden. Fügen Sie ein oder mehrere Azure mehrstufige Authentifizierungsserver als RADIUS-Server hinzu, und geben Sie einen gemeinsamen geheimen Schlüssel für jeden Server.





## <a name="configure-nps"></a>Konfigurieren von NPS

RD-Gateway verwendet NPS Azure mehrstufige Authentifizierung die RADIUS-Anforderung an. Timeout muss geändert werden, verhindern RD-Gateway von Timeout, bevor mehrstufige Authentifizierung abgeschlossen hat. Gehen Sie folgendermaßen vor, um NPS konfigurieren.

1. NPS erweitern Sie das Menü RADIUS-Clients und Server in der linken Spalte, und klicken Sie auf Remote-RADIUS-Servergruppen. Die Eigenschaften des TS-GATEWAY-SERVERGRUPPE wechseln Bearbeiten Sie die RADIUS-Server angezeigt und wechseln Sie zur Registerkarte Lastenausgleich. Ändern Sie "ausgelassen Sekunden ohne Antwort, bevor die Anforderung gilt" und "Anzahl der Sekunden zwischen Anfragen beim Server als nicht verfügbar gekennzeichnet ist" 30-60 Sekunden. Klicken Sie auf der Registerkarte Authentifizierung-Konto und sicherstellen Sie, dass die angegebenen RADIUS-Ports die Ports, denen mehrstufige Authentifizierungsserver abgehört.
2. NPS muss auch für die RADIUS-Authentifizierung von Azure mehrstufige Authentifizierungsserver empfangen konfiguriert werden. Klicken Sie im linken Menü auf RADIUS-Clients. Azure mehrstufige Authentifizierung-Server als RADIUS-Client hinzufügen. Wählen Sie einen Anzeigenamen und geben Sie einen gemeinsamen geheimen Schlüssel an.
3. Erweitern Sie den Abschnitt Richtlinien im linken Navigationsbereich und Verbindungsanforderungsrichtlinien auf. Es sollte Verbindungsanforderungsrichtlinie namens TS GATEWAYAUTORISIERUNGSRICHTLINIE, die erstellt wurde, wenn RD-Gateway konfiguriert wurde. Diese Richtlinie leitet RADIUS-Anfragen an den Authentifizierungsserver mehrstufige.
4. Kopieren Sie diese Richtlinie, um eine neue zu erstellen. Fügen Sie in der neuen Richtlinie eine Bedingung mit dem Client angezeigten Namen mit dem Anzeigenamen aus Schritt 2 Azure Multi-Factor Authentication Server RADIUS-Client hinzu. Ändern Sie den Authentifizierungsanbieter auf lokalen Computer. Diese Richtlinie gewährleistet, dass beim RADIUS-Anforderung von Azure mehrstufige Authentifizierungsserver empfangen wird, die Authentifizierung erfolgt anstatt lokal RADIUS-Anforderung an den Server Azure mehrstufige Authentifizierung, bei der eine Schleife. Um die Schleife zu vermeiden, muss diese neue Richtlinie oberhalb der ursprünglichen bestellt werden, die mehrstufige Authentifizierungsserver weiterleitet.

## <a name="configure-azure-multi-factor-authentication"></a>Azure Mehrfaktor-Authentifizierung konfigurieren


--------------------------------------------------------------------------------



Azure mehrstufige Authentifizierungsserver ist als RADIUS-Proxy zwischen Remotedesktopgateway und NPS konfiguriert.  Es sollte auf einem Server Domäne installiert, unabhängig von den RD-Gatewayserver ist. Gehen Sie zum Konfigurieren des Servers Azure mehrstufige Authentifizierung.

1. Öffnen Sie Azure mehrstufige Authentifizierungsserver, und klicken Sie auf RADIUS-Authentifizierung. Kontrollkästchen Sie das Aktivieren RADIUS-Authentifizierung.
2. Sicherstellen Sie auf der Registerkarte Clients, dass die Ports entsprechen NPS Konfiguration, und klicken Sie auf Hinzufügen... Schaltfläche. Fügen Sie die RD-Gateway-Server-IP-Adresse, Anwendungsname (optional) und einen gemeinsamen geheimen Schlüssel. Der gemeinsame geheime Schlüssel müssen auf den Azure mehrstufige Authentifizierung und RD-Gateway übereinstimmen.
3. Klicken Sie auf die Registerkarte Ziel, und wählen Sie das Optionsfeld RADIUS-Server.
4. Klicken Sie auf Hinzufügen... Schaltfläche. Geben Sie die IP-Adresse, geheime und Ports des Netzwerkrichtlinienservers. Wenn mit einem zentralen NPS werden RADIUS-Client und RADIUS-Ziel identisch. Der gemeinsame geheime Schlüssel muss eine Einrichtung im Abschnitt RADIUS-Client des Netzwerkrichtlinienservers.

![RADIUS-Authentifizierung](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

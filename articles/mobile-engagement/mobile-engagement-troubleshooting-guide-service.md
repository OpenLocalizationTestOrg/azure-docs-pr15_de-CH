<properties 
   pageTitle="Azure Mobile Engagement Fehlerbehebung - Service" 
   description="Fehlerbehebungshandbücher für Azure Mobile Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-service-issues"></a>Fehlerbehebung bei Problemen mit Diensten

Im folgenden werden mögliche Probleme mit der Ausführung von Azure Mobile Engagement auftreten können.

## <a name="service-outages"></a>Dienstausfälle

### <a name="issue"></a>Problem
- Probleme, die scheinbar Dienstausfälle zu Azure Mobile Engagement zurückzuführen.

### <a name="causes"></a>Ursachen
- Probleme, die scheinbar Dienstausfälle zu Azure Mobile Engagement Ursachen können verschiedene Ursachen haben:
    - Vereinzelte Probleme angezeigt, die ursprünglich systemische aller Azure Mobile Engagement
    - Bekannte Probleme aufgrund von Serverausfällen (nicht immer scheint Serverstatus):
    - Planung verzögert Zielgruppenadressierung Fehler Badge Update Probleme, sammeln, Push mehr Statistiken beenden API nicht mehr funktioniert, neue apps oder Benutzer kann nicht erstellt werden, DNS-Fehler und Timeoutfehler Benutzeroberfläche, API oder Apps auf einem Gerät.
    - Cloud Abhängigkeit Ausfälle [Azure Service-Status](http://status.azure.com/)
    - Push Notification Services (PNS) Abhängigkeit Ausfälle
    - App Store Ausfälle

1) Die gleiche Funktion aus einem anderen kann testen, um festzustellen, ob das Problem beeinträchtigt ist
   
   - Azure Mobile Engagement integrierte Anwendung
   - Testgerät
   - Betriebssystem des Geräts Testversion
   - Kampagne
   - Benutzerkonto
   - Browser (IE, Firefox, Chrome, etc.)
   - Computer

2) Um festzustellen, ob das Problem nur die Benutzeroberfläche oder der APIs betrifft:

   - Testen Sie die gleiche Funktion von Azure Mobile Engagement Benutzeroberfläche und Azure Mobile Engagement API.

3) Zum Überprüfen, ob das Problem mit Ihrem Mobiltelefon:

   - Testen Sie während der Verbindung zum Internet über WIFI und über Ihr Mobiltelefon Netzwerk 3G verbunden.
   - Bestätigen Sie, dass Ihre Firewall Azure Mobile Engagement IP-Adressen oder Ports nicht blockiert wird.

4) Wenn das Problem mit dem Gerät testen:

   - Testen Sie, ob Ihr Gerät Azure Mobile Engagement mit anderen Azure Mobile Engagement integrierten app herstellen kann.
   - Test, der Ereignisse, Projekte und Abstürze von Ihrem Telefon erstellen, die in Azure Mobile Engagement Benutzeroberfläche sichtbar sind. 
   - Testen von Azure Mobile Engagement Benutzeroberfläche Pushbenachrichtigungen Geräts anhand der Geräte-ID senden 

5) Wenn das Problem mit der Anwendung testen:

   - Installieren Sie und Testen Sie die Anwendung aus einem Emulator nicht von einem physischen Gerät:
   
6) Wenn das Problem mit OS Endbenutzer Geräte erfordern eine Aktualisierung SDK zu testen:

   - Testen Sie die Anwendung auf verschiedenen Geräten mit verschiedenen Versionen des Betriebssystems.
   - Bestätigen Sie, dass Sie die neueste Version des SDK verwenden.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Konnektivität und falsche Informationen Fragen

### <a name="issue"></a>Problem
- Probleme beim Anmelden in Azure Mobile Engagement Benutzeroberfläche.
- Verbindungsfehler mit Azure Mobile Engagement API.
- Probleme beim Hochladen von App Info Tags API Gerät.
- Probleme beim Herunterladen von Protokollen oder exportierten Daten von Azure Mobile Engagement.
- Falsche Informationen in Azure Mobile Engagement UI angezeigt.
- Falsche Informationen in Azure Mobile Engagement-Protokollen angezeigt.

### <a name="causes"></a>Ursachen
* Bestätigen Sie, dass Ihr Benutzerkonto über ausreichende Berechtigungen zum Ausführen der Aufgabe verfügt.
* Bestätigen Sie, dass das Problem nicht auf einem Computer oder zum lokalen Netzwerk.
* Bestätigen Sie, dass der Dienst Azure Mobile Engagement keine Ausfälle gemeldet.
* Sicherzustellen Sie, dass Ihre App Info Tag Dateien alle diese Regeln:
    - Verwenden Sie nur den UTF8-Zeichensatz (ANSI-Zeichensatz wird nicht unterstützt).
    - Verwenden Sie ein Komma "," als Trennzeichen (einen Service-Anforderung zu Anforderung ändern, CSV-Trennzeichen aus einer kommagetrennten öffnen "," in ein anderes Zeichen wie ein Semikolon ";").
    - Verwenden Sie Kleinbuchstaben für boolesche Werte, "true" und "false".
    - Verwenden Sie eine maximale Dateigröße von 35 MB unterschreitet.
 

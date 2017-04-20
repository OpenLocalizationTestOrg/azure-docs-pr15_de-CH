<properties
    pageTitle="Aktivieren Sie Sitzungsaffinität mit dem Azure-Toolkit für Eclipse"
    description="Informationen Sie zum sitzungsaffinität mit dem Azure-Toolkit für Eclipse aktivieren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Sitzungsaffinität aktivieren #

Innerhalb der Azure-Toolkit für Eclipse können Sie HTTP-sitzungsaffinität oder "sticky Sessions" für Ihre Rollen. Die folgende Abbildung zeigt die **Lastenausgleich** Eigenschaften Dialogfeld für Sitzung Affinität Feature aktivieren:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Sitzungsaffinität für Ihre Rolle aktivieren ##

1. Rolle im Projekt-Explorer Eclipse Maustaste und klicken Sie dann auf **Load Balancing**auf **Azure**.
1. Im Dialogfeld **Eigenschaften für den Lastenausgleich WorkerRole1** :
    1. Überprüfen Sie **HTTP aktivieren sitzungsaffinität (sticky Sitzungen) für diese Rolle.**
    1. **Eingabe Endpunkt verwendet**wählen Sie einen input Endpunkt, beispielsweise **http (Public: 80, Private: 8080)**verwenden. Ihre Anwendung muss diesen Endpunkt als HTTP-Endpunkt. Sie können mehrere Endpunkte für Ihre Rolle, aber wählen Sie nur eine Kurznotiz Sessions unterstützen.
    1. Erneutes Erstellen der Anwendung.

Sobald aktiviert, wenn Sie mehr als eine Instanz der Rolle haben, weiterhin HTTP-Anfragen von einem bestimmten Client bearbeitet die gleiche Instanz der Rolle.

Eclipse Toolkit können dies durch Installieren eines speziellen IIS-Moduls Anwendung anfordern Routing (ARR) in alle Ihre Instanzen aufgerufen. ARR leitet HTTP-Anfragen an die entsprechende Rolleninstanz. Das Toolkit konfiguriert automatisch ausgewählten Endpunkt, so dass eingehende HTTP-Verkehr zuerst ARR Software weitergeleitet werden. Das Toolkit auch erstellt einen neuen internen Endpunkt, der Java Server konfiguriert anhören. Das ist der Endpunkt zur ARR HTTP-Datenverkehr an die entsprechende Rolleninstanz umgeleitet. Auf diese Weise fungiert jede Instanz der Rolle in der Bereitstellung mehrere Instanzen als reverse Proxy für alle anderen Instanzen sticky Sessions aktivieren.

## <a name="notes-about-session-affinity"></a>Hinweise zur sitzungsaffinität ##

* Sitzungsaffinität funktioniert nicht im Serveremulator. Die Einstellungen im Serveremulator ohne Buildprozesses angewendet werden oder berechnen Emulator ausführen, aber das Feature selbst funktioniert nicht im Serveremulator.
* Aktivieren der sitzungsaffinität führt erhöht Speicherplatz Bereitstellung in Azure zusätzlicher Software heruntergeladen und installiert die Rolleninstanzen beim Starten des Diensts in der Azure-Cloud.
* Die Zeit für jede Rolle initialisieren dauert länger.
* Interner Endpunkt wie ein Rerouter Datenverkehr, wie bereits erwähnt, werden added.ss

Beispiel zu Sitzungsdaten sitzungsaffinität aktiviert ist finden Sie unter [Verwalten von Sitzungsdaten mit Sitzung][].

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

[Wie Sie Sitzungsdaten mit Sitzung][]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Wie Sie Sitzungsdaten mit Sitzung]: http://go.microsoft.com/fwlink/?LinkID=699539
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

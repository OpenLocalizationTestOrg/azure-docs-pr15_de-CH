
<properties
    pageTitle="App an Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie mehr über die Vorschriften für apps in Azure RemoteApp verwenden möchten"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>App-Vorschriften

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp unterstützt streaming 32-Bit- oder 64-Bit Windows-basierten Anwendung en von Windows Server 2012 R2 Bild. Die meisten vorhandenen 32-Bit- oder 64-Bit Windows-basierten Anwendung ausführen "wie besehen" in Azure RemoteApp (Remote Desktop Services oder früher als Terminaldienste bezeichnet) Umgebung. Jedoch gibt es ein Unterschied zwischen und und - einige Programme ordnungsgemäß und gut, andere nicht. Die folgende Informationen Hilfestellung zur Anwendungsentwicklung in einer Remotedesktopdienste-Umgebung testen, um die Kompatibilität sicherzustellen.

Tipp: Wir arbeiten einige Arbeitsbeispiele Apps für Sie zu erstellen. Sie sehen neue Themen, die mit Microsoft Access, QuickBooks und App-V in RemoteApp besprechen.

## <a name="requirements"></a>Vorschriften
Drei solcher helfen, wenn Ihre Anwendung in RemoteApp ausführen:

1.  Programme, die alle [Zertifizierungsstellen an Windows desktop-apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) und [Remotedesktopdienste Programmierungsrichtlinien](https://msdn.microsoft.com/library/aa383490.aspx) einhalten müssen vollständige Kompatibilität mit RemoteApp.
2.  Anwendung sollte niemals Daten lokal speichern auf das Bild oder die RemoteApp-Instanzen, die verloren gehen können.  Nach dem Erstellen einer Auflistung RemoteApp Instanzen werden geklont sind statusfrei und sollten nur Programme enthalten. Daten in einer externen Quelle oder im Profil des Benutzers gespeichert.
3.  Benutzerdefinierte Bilder sollten niemals Daten, die verloren gehen können.  

## <a name="testing-your-apps"></a>Testen Ihrer apps
Gehen Sie folgendermaßen vor um Anwendungstests:

1.  Installieren Sie Windows Server 2012 R2 und der Anwendung
2.  Aktivieren von Remotedesktop
3.  Erstellen Sie zwei Benutzerkonten BenutzerA und BenutzerB Sicherheitsgruppe Remotedesktop beider Benutzerkonten hinzufügen.
4.  Überprüfen Sie Multi-Session-Kompatibilität durch die Einrichtung von zwei gleichzeitiger RDS Sitzungen mit dem PC beim Starten der Anwendungdes.
5.  Überprüfen von Anwendungsverhalten

## <a name="application-development-guidelines"></a>Richtlinien für die Entwicklung
Verwenden Sie die folgenden Richtlinien zur Anwendungsentwicklung für RemoteApp.

### <a name="multiple-users"></a>Mehrere Benutzer

- Installieren einer [Anwendung für einen einzelnen Benutzer ](https://msdn.microsoft.com/library/aa380661.aspx)kann Probleme in einer Mehrbenutzer-Umgebung erstellen.
- Programme sollten an benutzerspezifischen Speicherorten getrennt von globalen Informationen für alle Benutzer [Informationen speichern](https://msdn.microsoft.com/library/aa383452.aspx) .
- RemoteApp verwendet mehrere [Namespaces für Kernelobjekte](https://msdn.microsoft.com/library/aa382954.aspx). ein globaler Namespace wird hauptsächlich von Services in Client/Server-Anwendung verwendet.
- Es ist nicht davon ausgehen, dass den Computernamen oder die [IP-Adresse](https://msdn.microsoft.com/library/aa382942.aspx) dem Computer einen einzelnen Benutzer zugeordnet sind, da mehrere Benutzer gleichzeitig mit einem Remotedesktop-Sitzungshost (RD-Sitzungshost) Server anmelden können.

### <a name="performance"></a>Leistung
- Deaktivieren Sie [Grafikeffekte](https://msdn.microsoft.com/library/aa380822.aspx) RemoteApp Ihrer Anwendung hinzu.
- Zur Maximierung der CPU-Verfügbarkeit für alle Benutzer deaktivieren Sie [Hintergrundaufgaben](https://msdn.microsoft.com/library/aa380665.aspx) oder erstellen Sie effizienter Hintergrundaufgaben, die nicht ressourcenintensiv.
- Sie optimieren und Anwendung [Thread Verwendung](https://msdn.microsoft.com/library/aa383520.aspx) einer Mehrbenutzer Umgebung mit mehreren Prozessoren verteilen.
- Zur Optimierung der Leistung empfiehlt es Anwendung [erkennen](https://msdn.microsoft.com/library/aa380798.aspx) , ob sie in einer Clientsitzung ausgeführt werden.

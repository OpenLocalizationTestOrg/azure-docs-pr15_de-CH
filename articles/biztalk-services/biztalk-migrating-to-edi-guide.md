<properties   
    pageTitle="Migrieren von BizTalk Server EDI-Lösungen zu BizTalk Services Technical Guide | Microsoft Azure"
    description="Migrieren Sie EDI, MAK; Microsoft Azure BizTalk-Dienste"
    services="biztalk-services"
    documentationCenter="na"
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags 
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migrieren von BizTalk Server EDI-Lösungen zu BizTalk-Dienste: Technisches Handbuch

Autor: Tim Wieman und Nitin Mehrotra

Bearbeitung: Karthik Bharthy

Geschrieben: Microsoft Azure BizTalk Services – Februar 2014 freizugeben.

## <a name="introduction"></a>Einführung

Electronic Data Interchange (EDI) ist eine der häufigsten die Unternehmen Exchange Daten elektronisch, auch bezeichnet als Business-to-Business- oder B2B-Transaktionen. BizTalk Server hat die EDI-Unterstützung für mehr als zehn Jahren seit der ersten Version von BizTalk Server. Mit BizTalk-Diensten weiterhin Microsoft Unterstützung für EDI-Lösung auf Microsoft Azure-Plattform. B2B-Transaktionen sind meist außerhalb einer Organisation und daher ist es einfacher zu implementieren, wenn es auf eine Cloud implementiert wurde. Microsoft Azure bietet diese Möglichkeit über BizTalk-Dienste.

Während einige Kunden BizTalk-Dienste "Greenfield" Plattform für neue EDI-Lösungen betrachten, haben viele aktuelle BizTalk Server EDI-Lösung, die sie in Azure migrieren möchten. Da BizTalk Services EDI entworfen basiert auf wichtige Elemente BizTalk Server EDI-Architektur (trading Partner, Entitäten, Verträge), es kann BizTalk Server EDI-Artefakte zu BizTalk Services zu migrieren.

Dieses Dokument erläutert einige der Unterschiede mit Migrieren von BizTalk Server EDI BizTalk Services. Kenntnisse der BizTalk Server EDI-Verarbeitung und Handel Partnerabkommen ausgegangen. Finden Sie weitere Informationen über BizTalk Server EDI [Trading Partner Management mithilfe von BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welche Version von BizTalk Server EDI-Artefakte BizTalk-Dienste migriert werden kann?

BizTalk Server EDI-Modul wurde für BizTalk Server 2010 erheblich verbessert, wenn es Partnern, Profile und Übereinkommen umgebaut wurde. BizTalk-Dienste verwenden dasselbe Modell Handelspartner und Geschäftsbereiche innerhalb dieser Handelspartner organisieren. Migration von EDI-Artefakte von BizTalk Server 2010 und höher zu BizTalk-Dienste, ist daher ein weitaus Geradlinig Prozess. EDI-Artefakte mit Versionen vor BizTalk Server 2010 migrieren Sie zunächst BizTalk Server 2010 aktualisieren und migrieren Artefakte EDI in BizTalk-Dienste.

## <a name="scenariosmessage-flow"></a>Szenarien/Nachrichtenfluss

Als mit BizTalk Server EDI in BizTalk Services verarbeitet Trading Partner Management (TPM) Lösung basiert auf. TPM-Lösung besteht aus die folgenden Komponenten:

- Handelspartner, die Unternehmen in einer B2B-Transaktion dar.
- Profile, die Geschäftsbereiche innerhalb eines Handelspartners.
- Trading Partnerabkommen oder Übereinkünfte, die geschäftliche Vereinbarung zwischen zwei Partnerprofile darstellen.

Die folgende Abbildung zeigt die Ähnlichkeit als auch Unterschiede zwischen BizTalk Server EDI-Lösung und BizTalk EDI-Lösung:

![][EDImessageflow]

Die wichtigsten Unterschiede und Ähnlichkeit zwischen einer EDI-Lösung in BizTalk Server und BizTalk-Dienste sind:

- Wie BizTalk Server EDI-Nachricht und EDISend-Pipeline zum Senden einer EDI-Nachricht eine EDIReceive-Pipeline verwendet, verwendet BizTalk-Dienste ein empfangen von EDI-Bridge empfangen und EDI senden Bridge EDI-Nachrichten zu senden. In BizTalk Server Pipelines mit einer Vereinbarung mit senden oder Empfangsports. BizTalk-Dienste selbst kennzeichnet das Senden oder empfangen Bridge.
- In BizTalk Server nach EDIReceive Pipeline die EDI-Nachricht verarbeitet wird die Nachricht eine SQL Server-Datenbank ausgegeben. Die EdiSend-Pipeline nimmt dann die Nachricht aus der SQL Server-Datenbank, verarbeitet und dann sendet es an den Handelspartner.

    BizTalk-Dienste nach die EDI erhalten Bridge verarbeitet die EDI-Nachricht, leitet die Nachricht an einen externen Prozess. Der externe Prozess konnte Microsoft Azure oder lokal ausgeführt werden. Der externe Prozess sollte die Nachricht EDI senden Brücke weiterleiten; Nachricht ziehen senden Bridge nicht grundsätzlich. Nach der Verarbeitung der Nachricht leitet die Brücke EDI senden Nachrichten an den Handelspartner.

BizTalk-Dienste bietet eine einfach zu verwendende Konfiguration schnell erstellen und Bereitstellen einer B2B-Vereinbarung zwischen Handelspartnern ohne Konfiguration alle Microsoft Azure Compute-Instanzen (Web oder Worker-Rollen), alle Microsoft Azure SQL-Datenbanken oder Microsoft Azure Storage Konten. Komplexere Szenarien müssen Workflows oder anderen Dienst binden "herum" Handelspartner Vereinbarung also vor oder nach dem Handel Partner Vereinbarung EDI-Bridge verarbeitet. Detailliert tritt die folgende Sequenz von Ereignissen während einer EDI-Nachricht im BizTalk-Dienste.

1. Eine EDI-Meldung vom Handelspartner Fabrikam.  Zum Empfangen von EDI-Nachrichten von Handelspartnern, unterstützt der BizTalk-Dienste Transportprotokolle wie FTP, SFTP AS2 und HTTP/S.

2. Trading Partner Vereinbarung empfangsseitige Verarbeitung disassembliert die EDI-Nachricht in XML-Format.  Sie können Endpunkte einen Endpunkt Service Bus Relay, Service Bus Thema, Service Bus-Warteschlange oder Brücke BizTalk-Dienste Service Bus disassemblierte EDI-Nachricht (im XML-Format) weiterleiten.

3. Endpunkt für weitere benutzerdefinierte Verarbeitung konnte dann disassemblierten XML-Nachrichten empfangen werden.  Diese Endpunkte konnte beispielsweise eine lokale Komponente oder eine Microsoft Azure Compute-Instanz zur Weiterverarbeitung der Nachricht in einem Windows Workflow (WF) oder Windows Communication Foundation (WCF)-Dienst verarbeitet werden.

4. "Senden clientseitige Verarbeitung" des Handelspartners Vereinbarung baut die XML-Nachricht in EDI-Format und sendet sie an Handelspartner Contoso.  EDI-Nachrichten an Handelspartner senden, unterstützt der BizTalk-Dienste die gleichen Protokolle wie EDI-Nachrichten empfangen.

Dieses Dokument Weitere enthält einen begrifflichen Leitfaden migrieren auf bestimmte andere BizTalk Server EDI-Artefakte zu BizTalk-Dienste.

## <a name="sendreceive-ports-to-trading-partners"></a>Empfangen Sie senden und Ports an Handelspartner

In BizTalk Server richten Sie Empfangsspeicherorte und Empfangsports EDI-XML-Nachrichten von Handelspartnern und Sendeports EDI-XML-Nachrichten an Handelspartner senden eingerichtet. Anschließend binden Sie die Ports mithilfe der BizTalk Server-Verwaltungskonsole eine handelspartnervereinbarung. BizTalk-Dienste, die Speicherorte, Sie Nachrichten empfangen von Handelspartnern, senden Nachrichten an Handelspartner werden als Teil der handelspartnervereinbarung (als Teil von Transport) im BizTalk-Portal konfiguriert.  So müssen Sie nicht wirklich das Konzept der "Sendeports" und "Empfangsspeicherorte", in der BizTalk-Dienste. Weitere Informationen finden Sie unter [Erstellen von Verträgen](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pipelines (Brücken)

Pipelines werden in BizTalk Server EDI Nachricht verarbeiten Entitäten, die benutzerdefinierten Logik für bestimmte Verarbeitungsfunktionen gemäß der Anwendung auch können. Für BizTalk-Dienste würde die Entsprechung eine EDI-Brücke. Jedoch sind im BizTalk-Dienste jetzt EDI-Brücken "geschlossen".  Also kann keine EDI-Brücke benutzerdefinierten Aktivitäten hinzugefügt werden. Eine beliebige benutzerdefinierte Verarbeitung außerhalb der EDI-Brücke in der Anwendung erfolgt entweder vor oder nach die Nachricht als Teil des Abkommens Handelspartner Bridge eingegeben. EAI-Brücken können benutzerdefinierte Verarbeitung. Ggf. benutzerdefinierte Verarbeitung können Sie die EAI-Brücken vor oder nach die Nachricht vom EDI-Bridge verarbeitet wird. Weitere Informationen finden Sie unter [Brücken benutzerdefinierten Code enthalten](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Sie können einen Veröffentlichen-Abonnieren Fluss mit benutzerdefiniertem Code und/oder Service Bus messaging-Warteschlangen und Themen vor der handelspartnervereinbarung die Nachricht empfängt oder nach die Vereinbarung verarbeitet die Nachricht und leitet sie an einen Service Bus-Endpunkt.

**Szenarien/Nachrichtenfluss** in diesem Thema für Message Flow-Muster anzeigen

## <a name="agreements"></a>Verträge

Wenn Sie mit BizTalk Server 2010 Trading Partner Abkommen zur Verarbeitung von EDI vertraut sind, sehen BizTalk Services Partner Handelsabkommen vertraut. Die meisten vereinbarungseinstellungen identisch sind und die gleichen Begriffe verwendet. In einigen Fällen der Vereinbarung sind einfacher mit denselben Einstellungen in BizTalk Server verglichen. Microsoft Azure BizTalk Services unterstützt X12, EDIFACT und AS2 transport.

Microsoft Azure BizTalk-Dienste können auch **TPM Datenmigration** Handelspartner BizTalk Server-Modul BizTalk Services Portal Handelspartner und Verträge migrieren. Das TPM Data Migration-Tool steht als Teil eines Pakets Tools die [MAK-SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)heruntergeladen werden kann. Das Paket enthält außerdem eine Infodatei, die Anleitung zur Verwendung von Tools und grundlegende Informationen zur Problembehandlung für das Tool.

## <a name="schemas"></a>Schemas

BizTalk-Dienste bietet EDI-Schemata die BizTalk-Dienste Lösungen verwendet werden können.  Darüber hinaus können BizTalk Server EDI-Schemata auch mit BizTalk-Diensten verwendet werden, da der Stammknoten des EDI-Schema über BizTalk Server und BizTalk-Dienste ist. Daher werden Sie nicht direkt Ihre BizTalk Server EDI-Schemata EDI-Lösungen, die Sie entwickeln mithilfe der BizTalk-Dienste verwenden. Sie können auch die Schemas [MAK SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057)herunterladen.

## <a name="maps-transforms"></a>Karten (transformiert)

Karten in BizTalk Server heißen Transformationen BizTalk-Dienste. Migration wird von BizTalk Server-BizTalk-Dienste möglicherweise eine komplexeren Aufgaben (je nach Komplexität der Karte). BizTalk-Dienste zum Datenzuordnungs-Tool unterscheidet sich vom BizTalk-Mapper. Obwohl der Zuordnung größtenteils genauso aussieht, unterscheidet sich das Format der zugrunde liegenden. Für den Benutzer verfügbaren Funktoide (aufgerufene **Zuordnungsoperationen** BizTalk-Dienste) unterscheiden sich ebenfalls.  Sie können keine BizTalk-Zuordnung, direkt BizTalk-Dienste verwenden. Außerdem sind nicht alle Funktoide in BizTalk Server als Zuordnungsoperationen BizTalk-Dienste verfügbar.

### <a name="new-transform-operations"></a>Neue Transformationsvorgänge

Während die Liste der verfügbaren Map-Transformationsvorgänge unterscheidet die BizTalk Server-Mapper erscheinen, haben BizTalk Services transformiert neue derselben Aufgaben. BizTalk Services transformiert haben beispielsweise **Vorgänge auflisten** . Dies war nicht in der BizTalk-Mapper verfügbar.  Die **Liste Vorgänge** können erstellt und auf "Liste" eine Liste aus Elementen (auch bekannt als "Zeilen") besteht und jedes Element mehrere Member (auch bekannt als "Spalten") kann.  Sie können die Liste sortieren, Elemente auf eine Bedingung.

Ein weiteres Beispiel für neue Funktionen in BizTalk Services transformiert werden die **Schleife Operationen**.  Es ist schwierig, geschachtelte Schleifen in der BizTalk Server-Zuordnung zu erstellen.  Folglich Zuordnungsoperationen Schleife BizTalk Services transformiert hinzugefügt.

Ein weiteres Beispiel ist **If-Then-Else-** Ausdruck Zuordnungsvorgang.  Im BizTalk-Mapper If-Then-else-Vorgang ausführen konnte, aber mehrere Funktoide scheinbar einfache Aufgabe erforderlich.

### <a name="migrating-biztalk-server-maps"></a>Migrieren von BizTalk Server-Karten

Microsoft Azure BizTalk-Dienste bietet BizTalk-Dienste Transformationen ein Tool zum Migrieren von BizTalk Server zugeordnet. Die **BTMMigrationTool** steht als Teil der **Tools** Paket mit der [BizTalk-Dienste SDK herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=235057). Weitere Informationen zum Tool finden Sie unter [konvertieren eine BizTalk-Zuordnung BizTalk Services transformiert](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Sie können auch ein Beispiel von Sandro Pereira, MVP BizTalk zum [Migrieren von BizTalk Server wird der BizTalk-Dienste Transformationen](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx)betrachten.

## <a name="orchestrations"></a>Orchestrierung

Wenn Sie BizTalk Server-Orchestrierung verarbeitet Microsoft Azure migrieren möchten, müssten Orchestrierungen umgeschrieben werden, da Microsoft Azure standardisierter BizTalk Server nicht unterstützt.  Sie konnte die Orchestrierung Funktionalität in Windows Workflow Foundation 4.0 (WF4) Dienst schreiben.  Dies wäre eine vollständige Überarbeitung gibt es derzeit keine Migration von BizTalk Server-Orchestrierung zu WF4. Hier sind einige Ressourcen für Windows Workflow:

- [*Integration der WCF-Workflowdiensts Service Bus Warteschlangen mit Themen*](https://msdn.microsoft.com/library/azure/hh709041.aspx) Paolo Salvatori. 

- [ *Erstellen von apps mit Windows Workflow Foundation und Azure* Sitzung](http://go.microsoft.com/fwlink/p/?LinkId=237314) Build 2011-Konferenz.

- [*Windows Workflow Foundation-Entwicklercenter*](http://go.microsoft.com/fwlink/p/?LinkId=237315) auf MSDN.

- [*Dokumentation zu Windows Workflow Foundation 4 (WF4)*](https://msdn.microsoft.com/library/dd489441.aspx) auf MSDN.

## <a name="other-considerations"></a>Weitere Aspekte

Im folgenden werden einige Aspekte, die Sie beim Verwenden der BizTalk-Dienste stellen müssen.

### <a name="fallback-agreements"></a>Ausweichvereinbarungen

BizTalk Server EDI-Verarbeitung hat das Konzept der "Fallback Abkommen".  BizTalk-Dienste werden **nicht** bisher ein Fallback Vereinbarung Konzept.  Informationen finden Sie BizTalk-Dokumentationsthemen [Rolle der Abkommen in EDI-Verarbeitung](http://go.microsoft.com/fwlink/p/?LinkId=237317) und [globale Konfiguration oder Fallback Vereinbarungseigenschaften](https://msdn.microsoft.com/library/bb245981.aspx) Fallback Abkommen in BizTalk Server verwendet.

### <a name="routing-to-multiple-destinations"></a>Weiterleitung an mehrere Zielorte

BizTalk-Dienste Brücken im aktuellen Zustand unterstützt keine Weiterleitung von Nachrichten an mehrere Zielorte mit Veröffentlichen-Abonnieren-Modell. Stattdessen können Sie Nachrichten von Brücke BizTalk-Dienste zu einem Thema Service Bus weiterleiten die dann mehrere Abonnements die Nachricht an mehrere Endpunkte können.

## <a name="conclusion"></a>Abschluss

Microsoft Azure BizTalk-Dienste werden reguläre Meilensteine mehr Features und Funktionen hinzufügen. Bei jeder Aktualisierung freuen wir uns unterstützen erweiterte Funktionalität um End-to-End-Lösungen mit BizTalk Services und Azure Technologien zu erleichtern.

## <a name="see-also"></a>Siehe auch

[Enterprise-Anwendungsentwicklung mit Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png

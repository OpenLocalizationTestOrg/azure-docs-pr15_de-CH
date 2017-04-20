<properties
    pageTitle="Was ist .NET SDK Azure"
    description="Erfahren Sie, was in Azure .NET SDK enthalten."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Was ist das Azure SDK für .NET?

## <a name="overview"></a>Übersicht

Azure SDK für .NET ist eine Visual Studio-Tools, Befehlszeilenprogramme, Laufzeit-Binärdateien und Clientbibliotheken, mit denen Sie entwickeln, testen und Bereitstellen apps, die in Azure ausgeführt. Dieser Artikel beschreibt, was Sie erhalten, wenn Sie das SDK installieren. Sie können das SDK von der [Azure-Downloadseite](https://azure.microsoft.com/downloads/).

Azure SDK für .NET umfasst auch [Clientbibliotheken für Azure Services in Anspruch nehmen](http://go.microsoft.com/fwlink/?LinkId=510472). Diese Bibliotheken werden separat mit NuGet installiert.

##<a id="included"></a>In Azure SDK für .NET enthaltene

Azure SDK für .NET installiert die folgenden Produkte:

- [Visual Studio Community Edition 2015](#vwd)
- [Microsoft Azure Speicheremulator](#stgemulator)
- [Microsoft Azure Storage-Tools](#stgtools)
- [Microsoft Azure Authoring Tools](#auth)
- [Microsoft Azure-Emulator](#emulator)
- [Struktur HDInsight Tools für Visual Studio und Microsoft ODBC-Treiber](#hdinsight)
- [Microsoft Azure Bibliotheken für .NET](#libraries)
- [Microsoft Azure Mobile App SDK](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Azure Tools for Microsoft Visual Studio](#tools)
- [Microsoft ASP.NET und Webtools für Visual Studio](#wte)
- [Microsoft Azure Data Lake-Tools für Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio Community Edition 2015

Wenn Sie Visual Studio auf Ihrem Computer haben, installiert das SDK [Visual Studio Community Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Microsoft Azure Speicheremulator

Der [Speicheremulator Azure](http://msdn.microsoft.com/library/hh403989.aspx) verwendet eine SQL Server-Instanz und im lokalen Dateisystem Azure Storage (Warteschlangen, Tabellen, Blobs) simuliert, damit Sie lokal testen können.

###<a id="stgtools"></a>Microsoft Azure Storage-Tools

[AzCopy](http://aka.ms/AzCopy), ein Befehlszeilenprogramm, das Übertragen von Daten in und aus einem Azure Storage-Konto installiert.

###<a id="auth"></a>Microsoft Azure Authoring Tools

Dazu gehören die folgenden:

* Das [Befehlszeilentool CSPack](http://msdn.microsoft.com/library/gg432988.aspx) zum Bereitstellungspakete erstellen.
* das [Befehlszeilentool CSEncrypt](http://msdn.microsoft.com/library/hh404001.aspx) zum Verschlüsseln von Kennwörtern, die auf Instanzen Cloud-Dienst über eine Remotedesktopverbindung verwendet werden.
* Laufzeit-Binärdateien, die cloud-Projekte müssen für die Kommunikation mit der Common Language Runtime-Umgebung und Diagnose. Diese Binärdateien sind nicht verfügbar in NuGet-Pakete.

###<a id="emulator"></a>Microsoft Azure-Emulator

[Azure-Emulator](http://msdn.microsoft.com/library/dn339018.aspx) simuliert Cloud Service-Umgebung, sodass Cloud Projekte vor der Bereitstellung in Azure lokal auf Ihrem Computer getestet werden können.

###<a id="hdinsight"></a>Struktur HDInsight Tools für Visual Studio und Microsoft ODBC-Treiber

HDInsight Tools im Server-Explorer können Sie navigieren Struktur Datenbanken und verknüpfte Speicher für HDInsight-Cluster, erstellen Sie Tabellen erstellen und Struktur Anfragen. Weitere Informationen finden Sie unter [Erste Schritte mit HDInsight Hadoop Tools für Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

###<a id="libraries"></a>Microsoft Azure Bibliotheken für .NET

Dazu gehören die folgenden:

* NuGet-Pakete Azure Storage Service Bus und zwischenspeichern, die auf Ihrem Computer gespeichert sind, damit Visual Studio neuen Cloud Service beim Erstellen von Projekten können offline.
* Ein Visual Studio-Plug-in, mit dem [Cache Rolle](http://msdn.microsoft.com/library/dn386103.aspx) Projekte lokal in Visual Studio ausführen können.

###<a id="mobile"></a>Microsoft Azure Mobile App SDK

Tools für die Arbeit mit [Azure App Service Mobile Apps](app-service-mobile/app-service-mobile-value-prop.md).

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShell ermöglicht die [Automatisierung Azure-Umgebung und -Bereitstellung](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

###<a id="tools"></a>Microsoft Azure Tools for Microsoft Visual Studio

So können Sie mit Azure Ressourcen hauptsächlich Cloud-Dienste und virtuelle Computer arbeiten:

* [Erstellen, öffnen und Cloud Projekte veröffentlichen](cloud-services/cloud-services-dotnet-get-started.md).
* [Bereitstellungspakete für Cloud Projekte erstellen](http://msdn.microsoft.com/library/ff683672.aspx).
* [Azure Virtual Machines beim Erstellen neuer Webprojekte erstellen](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Erstellen von PowerShell-Skripts beim Erstellen neuer virtueller Maschinen](http://msdn.microsoft.com/library/dn642480.aspx).
* [Anzeigen und Verwalten von Cloud Service Project Settings in Visual Studio-Projekt-Eigenschaftsfenster](http://msdn.microsoft.com/library/ee405486.aspx).
* Anzeigen und Verwalten von [Clouddiensten](http://msdn.microsoft.com/library/ff683675.aspx), [virtuellen Maschinen](http://msdn.microsoft.com/library/jj131259.aspx)und [Service Bus](http://msdn.microsoft.com/library/jj149828.aspx) im Server-Explorer.
* [Im Debugmodus Remote für Cloud-Dienste und virtuelle Maschinen ausführen](http://msdn.microsoft.com/library/ff683670.aspx).
* [Automatisieren der Bereitstellung mit Azure Ressource Gruppe Bereitstellungsprojekte](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>Microsoft App Service-Tools für Visual Studio

Dadurch werden Azure Websites arbeiten:

* [Webprojekte Azure Websites veröffentlichen](app-service-web/web-sites-dotnet-get-started.md).
* [Webaufträge Azure veröffentlichen Konsole Anwendungsprojekte](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Azure-Website erstellen und SQL-Ressourcen beim Erstellen eines neuen Webprojekts oder beim Veröffentlichen eines Projekts](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Erstellen Sie PowerShell Bereitstellungsskripts beim Erstellen neuer Websites](http://msdn.microsoft.com/library/dn642480.aspx).
* [Verwalten und die Problembehandlung bei Azure Websites im Server-Explorer](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Im Debugmodus für Websites und Webaufträge Remote ausführen](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Sie müssen das Azure SDK für .NET dieser Features installieren. Sie sind auch im Visual Studio-Updates enthalten.

##<a id="datalake"></a>Microsoft Azure Data Lake-Tools für Visual Studio

Weitere Informationen finden Sie unter [Tutorial: Entwickeln U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Nicht enthaltene beim Installieren von Azure SDK für .NET

Es gibt einige Dinge, die Sie möglicherweise für Azure-Entwicklung, die bei der Installation des SDK enthalten sind. Die wichtigsten sind:

* [Client-Bibliotheken](http://go.microsoft.com/fwlink/?LinkId=510472).

    Azure SDK enthält Clientbibliotheken für die Nutzung der Azure-Dienste nicht alle werden installiert, wenn Sie das SDK installieren. Wenn die Anwendung eine Clientbibliothek, die das SDK installiert ist, erhalten Sie sie von [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Wenn Ihre Anwendung eine Clientbibliothek, die das SDK installiert wird verwendet, ist sollten mit der aktuellen Version unter "NuGet.org" aktualisieren.

    **Lokale Kopien von Client-Bibliotheken.** Azure SDK für .NET kopiert NuGet-Pakete für einige Azure Clientbibliotheken Storage Service Bus und Caching auf den Computer. Diese Clientbibliotheken werden automatisch in neue Cloud Service Projekte aufgenommen, lokale NuGet-Pakete ermöglichen Visual Studio zum Erstellen von Projekten, auch wenn Sie nicht mit dem Internet verbunden sind. Client-Bibliotheken werden im Allgemeinen aktualisiert häufiger als neue SDK-Versionen freigegeben werden, Clientbibliotheken unter "NuGet.org" sind oft aktueller als Sie im SDK erhalten.

    **Projektvorlagen, die Client-Bibliotheken enthalten.** Nur [Azure Cloud Service](cloud-services/cloud-services-dotnet-get-started.md) und Azure App Service [Mobile Apps](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Projektvorlagen enthalten automatisch einige Clientbibliotheken. Installieren Sie für andere Bibliotheken oder andere Vorlagen die [Client Bibliothek NuGet-Pakete](http://go.microsoft.com/fwlink/?LinkId=510472) , die Sie.

* [Mobile Apps Projektvorlagen](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobile Apps Vorlagen sind in Visual Studio 2013 Update 2 und höher verfügbar. Sie sind nicht in Visual Studio 2012 oder früheren Versionen und nicht in Visual Studio 2013 Update 1 oder früher, auch wenn das Azure SDK für .NET installieren.

##<a id="faq"></a>Häufig gestellte Fragen

- [Viele Azure-Funktionen sind in Visual Studio. Muss ich das Azure SDK für .NET installieren?](#azinvs)
- [Sie möchten eine Clientbibliothek. Müssen Azure SDK für .NET zu installieren?](#clientlib)
- [Wo finde ich ältere Versionen von Azure SDK für .NET?](#olderversions)
- [Was ist der Lifecycle-Richtlinien für Versionen von Azure SDK für .NET?](#lifecycle)
- [Die Gast-BS-Versionen ist das Azure SDK für .NET mit?](#guestos)
- [Wie wird deinstallieren das Azure SDK für .NET?](#uninstall)

###<a id="azinvs"></a>Viele Azure-Funktionen sind in Visual Studio. Muss ich das Azure SDK für .NET installieren?

In diesem Zusammenhang empfiehlt es sich, das SDK installieren für Azure mit den neuesten Tools entwickeln möchten. Wenn Sie nicht das SDK installieren möchten, können Sie tun, wenn Folgendes zutrifft:

* Sie haben das neueste [Update für Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)installiert.
* Sie entwickeln für Azure Websites oder Mobile Dienste Cloud-Services oder virtuellen Computern.
* Ihre Anwendung nicht Speicher oder Speicher verwendet, doch Sie brauchen Speicheremulator oder AzCopy-Tool.

###<a id="clientlib"></a>Sie möchten eine Clientbibliothek. Müssen Azure SDK für .NET zu installieren?

Das SDK installiert Clientbibliotheken, um Cloud Projekte erstellen, wenn Sie nicht mit dem Internet verbunden sind. Aktuelle Clientbibliotheken stehen in NuGet-Pakete unter ["NuGet.org"](http://go.microsoft.com/fwlink/?LinkId=510472). Weitere Informationen finden Sie weiter oben in diesem Dokument [nicht enthaltene Azure SDK für .NET installieren](#notincluded) .

###<a id="olderversions"></a>Wo finde ich ältere Versionen von Azure SDK für .NET?

Ältere Versionen finden Sie unter Downloadseite [Azure SDK für .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) .

###<a id="lifecycle"></a>Was ist der Lifecycle-Richtlinien für Versionen von Azure SDK für .NET?

[Microsoft Azure Cloud Services Support Lifecycle Policy](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq)finden Sie.

###<a id="guestos"></a>Die Gast-BS-Versionen ist das Azure SDK für .NET mit?

[Azure Gast Betriebssystemversionen und SDK Kompatibilitätsübersicht](http://msdn.microsoft.com/library/ee924680.aspx)anzeigen

###<a id="uninstall"></a>Wie wird deinstallieren das Azure SDK für .NET?

Jedes Element in diesem Artikel im [Lieferumfang Azure SDK für .NET](#included) angezeigt ist ein separates Programm in der Liste der installierten Programme im Fenster **Programme und Funktionen**.  Es gibt keine Möglichkeit, sie als Gruppe deinstallieren; Sie müssen jede Anwendung einzeln deinstallieren.

Wenn Azure SDK für .NET bereits installiert haben und eine neue Version installieren, ist in der Regel müssen die alte Version deinstallieren. In den meisten Fällen aktualisiert die SDK-Installation ein vorhandenes Programm statt hinzufügen eine neue und alte verlassen.

Jedoch möchten Sie nicht länger-Bedarf Reste der früheren Version entfernen, deinstallieren Sie nur Programme, die die ältere Versionsnummer und nur deinstallieren Sie Wenn dasselbe Programm mit einer neueren Version vorhanden ist. Beispielsweise nach Aktualisierung von 2.5 auf 2.6 2.5 und 2.6 Versionen von "Microsoft Azure Tools for Microsoft Visual Studio 2013" angezeigt werden kann und die Version 2.5 deinstallieren. Jedoch kann nur die Version 2.5 "Microsoft Azure Authoring Tools" angezeigt, und es wäre sicher zu deinstallieren.

Beachten Sie, dass irreführende Versionsnummern Programmtitel in **Programme und Funktionen** angezeigt.  SDK Version 2.6 enthält beispielsweise Microsoft Azure Mobile App SDK v1. 0", wenn das SDK für Visual Studio 2013 und"Microsoft Azure Mobile App SDK v2. 0"für Visual Studio 2015 installieren; die Version ist in diesem Fall die SDK-Version sondern Indikator der Visual Studio-Version die Anwendung gilt.

Dieser Artikel listet nicht jedes Programm, das jede frühere Version von Azure SDK enthalten; andere Programme, die Sie aus früheren SDK-Versionen deinstallieren sind SDK früher manchmal Programme enthalten, die in späteren Versionen weggelassen wurden. Z. B. Version 2.5 installiert "Azure Resource Manager Tools für Visual Studio", aber das liegt nicht in diesem Artikel nicht als separates Programm unter **Programme und Funktionen**angezeigt.  Dieser Artikel listet nur Programme, die in Azure SDK für .NET Version 2.6 enthalten sind.

> **Hinweis:** Das SDK enthält Programmen kann auch separat installiert werden in anderen Kontexten und notwendig sein, auch wenn Sie das vollständige SDK nicht. Dasselbe kann Programme gelten, die SDK-Versionen installiert wurden jedoch höher SDK ausgelassen wurden. Wenn Sie Programme deinstallieren, achten Sie zu vermeiden, entfernen etwas noch auf dem Computer benötigt wird.



##<a id="versions"></a>Versionen

Aktuelle Version sehen oder ältere Versionen finden Sie auf [Azure SDK für .NET Version](https://azure.microsoft.com/downloads/archive-net-downloads/) .

##<a id="resources"></a>Ressourcen

Zum Herunterladen der aktuellen Azure SDK für .NET oder einer Clientbibliothek finden Sie in der [Azure-Downloadseite](https://azure.microsoft.com/downloads/).

Azure SDK für .NET Code einschließlich Clientbibliotheken finden Sie unter [GitHub.com/Azure](https://github.com/azure/).

Azure Client Library-Referenzdokumentation finden Sie unter [Azure.NET Reference](https://azure.microsoft.com/documentation/api/).

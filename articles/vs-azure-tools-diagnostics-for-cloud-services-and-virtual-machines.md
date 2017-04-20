<properties
   pageTitle="Diagnose für Azure Cloud Services und virtuelle Maschinen konfigurieren | Microsoft Azure"
   description="Beschreibt das Konfigurieren von Diagnoseinformationen zum Debuggen von Azure Wolke Dienste und virtuelle Maschinen (VMs) in Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Diagnose für Azure Cloud Services und virtuelle Computer konfigurieren

Wenn Sie eine Azure-Clouddienst oder Azure Virtual Machine beheben müssen, können Sie mit Visual Studio einfacher Azure Diagnostics konfigurieren. Azure-Diagnose erfasst Daten und Daten auf virtuellen Maschinen und VM-Instanzen, die den Cloud-Dienst ausgeführt und diese Daten in ein Speicherkonto Ihrer Wahl. Weitere Informationen über das Diagnoseprotokoll in Azure finden Sie unter [Diagnoseprotokoll für Web-apps in Azure App Service aktivieren](./app-service-web/web-sites-enable-diagnostic-log.md) .

In diesem Thema wird das Aktivieren und Konfigurieren von Azure Diagnostics in Visual Studio vor und nach der Bereitstellung sowie in Azure VMs veranschaulicht. Sie zeigt auch die Typen von Diagnoseinformationen sammeln auswählen und die Informationen nach der Erfassung anzeigen.

Azure-Diagnose können auf folgende Weise:

- Sie können Konfigurationen in Visual Studio im Dialogfeld **Konfiguration Diagnose** Diagnose ändern. Die Standardeinstellungen werden in einer Datei namens diagnostics.wadcfgx (diagnostics.wadcfg in Azure SDK 2.4 oder früher) gespeichert. Alternativ können Sie die Konfigurationsdatei direkt ändern. Wenn Sie die Datei manuell aktualisieren, die geänderte Konfiguration wirksam die nächste Bereitstellung Cloud service in Azure oder Ausführen des Dienstes im Emulator.

- Verwenden Sie **Cloud Explorer** oder im **Server-Explorer** in Visual Studio die Diagnose für einen laufenden Cloud-Dienst oder virtuellen Computer ändern.

## <a name="azure-26-diagnostics-changes"></a>2.6 Azure Diagnostics ändern

Azure SDK 2.6 Projekte in Visual Studio wurden folgende geändert. (Diese Änderungen gelten auch für spätere Versionen von Azure SDK.)

- Lokale Emulator unterstützt jetzt Diagnose. Dies bedeutet Diagnosedaten sammeln und sicherstellen, dass Ihre Anwendung rechts Spuren während Sie entwickeln und Testen in Visual Studio erstellt. Die Verbindungszeichenfolge `UseDevelopmentStorage=true` Diagnose Datensammlung Cloud-Dienstprojekt in Visual Studio ausführen mit dem Emulator Azure-Speicher bietet. Alle Diagnosedaten werden im Speicherkonto (Development Storage).

- Verbindungszeichenfolge für Diagnose Storage-Konto (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) ist wieder in der Service-Konfigurationsdatei (.cscfg) gespeichert. In Azure SDK 2.5 wurde das Speicherkonto Diagnose in der Datei diagnostics.wadcfgx angegeben.

Es gibt einige wichtige Unterschiede zwischen wie die Verbindungszeichenfolge in Azure SDK 2.4 und früher gearbeitet und Funktionsweise in Azure SDK 2.6 oder höher.

- In Azure SDK 2.4 und früher die Verbindungszeichenfolge eine Laufzeit Diagnose Plug-in diente als Storage-Kontoinformationen für die Übertragung von Diagnoseprotokolle zu.

- In Azure SDK 2.6 und höher ist die Diagnose von Visual Studio verwendet diagnoseerweiterung mit den entsprechenden Informationen während der Veröffentlichung konfigurieren. Die Verbindungszeichenfolge können Sie verschiedene Konten für verschiedene Dienstkonfigurationen definieren, die beim Veröffentlichen von Visual Studio verwenden. Da das Diagnose-Plugin (nach Azure SDK 2.5) nicht mehr verfügbar ist, kann nicht .cscfg Datei allein Diagnose-Erweiterung aktivieren. Sie müssen die Erweiterung separat über PowerShell oder Visual Studio-Tools.

- Vereinfacht die diagnoseerweiterung mit PowerShell konfigurieren enthält die Paket-Ausgabe von Visual Studio öffentliche Konfigurations-XML für die diagnoseerweiterung für jede Rolle. Visual Studio verwendet die Verbindungszeichenfolge Diagnose die speicherkontoinformationen im öffentlichen Konfiguration auffüllen. Die Öffentliche Dateien werden im Ordner "Extensions" erstellt und folgen dem Muster PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. PowerShell basiert Einsatz können dieses Muster jede Konfiguration einer Rolle zuordnen.

- Die Verbindungszeichenfolge in der Datei .cscfg wird auch von [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) verwendet, die Diagnosedaten auf, damit die Registerkarte **Überwachung** angezeigt werden können. Die Verbindungszeichenfolge muss den Dienst zeigen ausführliche Daten im Portal konfigurieren.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrieren von Projekten auf Azure SDK 2.6 und höher

Migrieren Sie von Azure SDK 2.5 Azure SDK 2.6 oder höher, wäre eine Diagnose Speicherkonto in der Datei .wadcfgx angegeben, dann bleibt gibt es. Um die Flexibilität, verschiedene Konten für verschiedene Konfigurationen nutzen zu können, müssen Sie die Verbindungszeichenfolge manuell zum Projekt hinzufügen. Wenn Sie ein Projekt von Azure SDK 2.4 oder früher in Azure SDK 2.6 migrieren, werden die Verbindungszeichenfolgen Diagnose beibehalten. Beachten Sie jedoch die Änderung wie Verbindungszeichenfolgen in Azure SDK 2.6 wie im vorherigen Abschnitt behandelt werden.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Wie Visual Studio bestimmt das Speicherkonto für die Diagnose

- Wenn eine Diagnose-Verbindungszeichenfolge in der Datei .cscfg angegeben ist, verwendet Visual Studio diagnoseerweiterung beim Veröffentlichen und generieren die öffentlichen XML-Konfigurationsdateien beim Packen konfigurieren.

- Wenn keine Diagnose-Verbindungszeichenfolge in der Datei .cscfg angegeben wird, wird anschließend Visual Studio mit in der Datei .wadcfgx angegebenen Speicherkonto diagnoseerweiterung veröffentlichen und generieren die öffentlichen XML-Konfigurationsdateien beim Konfigurieren.

- Die Diagnose-Verbindungszeichenfolge in der Datei .cscfg Vorrang vor das Speicherkonto in der Datei .wadcfgx. Eine Diagnose-Verbindungszeichenfolge in der Datei .cscfg angegeben ist, dann Visual Studio verwendet wird, und ignoriert das Speicherkonto in .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Was ist die "Update Entwicklung Speicher Verbindungszeichenfolgen..." Kontrollkästchen tun?

Das Kontrollkästchen **Update Entwicklung Speicher Verbindungszeichenfolgen für Diagnose und Zwischenspeichern mit Microsoft Azure Storage Anmeldeinformationen beim Veröffentlichen in Microsoft Azure** bietet eine bequeme Möglichkeit, alle Verbindungszeichenfolgen Entwicklung Storage Konto während der Veröffentlichung angegebenen Azure Storage-Konto aktualisieren.

Angenommen, dieses Kontrollkästchen aktivieren und die Verbindungszeichenfolge Diagnose gibt `UseDevelopmentStorage=true`. Beim Veröffentlichen des Projekts in Azure aktualisiert Visual Studio automatisch Diagnose-Verbindungszeichenfolge das Speicherkonto im Webpublishing-Assistenten angegeben. Jedoch Wenn echte Speicherkonto Diagnose Verbindungszeichenfolge angegeben wurde, wird dann das Konto stattdessen verwendet.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnose Funktionsunterschiede zwischen Azure SDK 2.4 und früher und Azure SDK 2.5 und höher

Wenn Sie Ihr Projekt von Azure SDK 2.4 Azure SDK 2.5 oder höher aktualisieren, sollten Sie Funktionsunterschiede von Diagnose berücksichtigen.

- **Konfigurations-APIs sind veraltet** – programmgesteuerte Konfiguration von Diagnose in Azure SDK 2.4 oder früheren Versionen steht jedoch in Azure SDK 2.5 und höher veraltet ist. Wenn die Diagnosekonfiguration im Code definiert ist, müssen Sie diese Einstellung aus der migrierten Projekt für Diagnose Arbeiten neu konfigurieren. Diagnose-Konfigurationsdatei für Azure SDK 2.4 ist diagnostics.wadcfg und diagnostics.wadcfgx für Azure SDK 2.5 oder höher.

- **Diagnose für Cloudanwendungen Dienst kann nur auf Rollenebene, nicht auf Instanzebene konfiguriert werden.**

- **Die Diagnosekonfiguration wird aktualisiert, jedes Mal, wenn Sie Ihre Anwendung bereitstellen** , können dadurch Parität Fragen Ihre Diagnosekonfiguration im Server-Explorer ändern und dann erneut die app.

- **In der Konfigurationsdatei Diagnose nicht im Code in Azure SDK 2.5 und höher, Crash Dumps konfiguriert sind** – Speicherabbilder im Code konfiguriert haben, müssen Sie die Konfiguration manuell aus dem Code in die Konfigurationsdatei übertragen da die Speicherabbilder während der Migration in Azure SDK 2.6 übertragen werden nicht.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Aktivieren Sie Diagnose in der Cloud Projekte vor dem Ausbringen

In Visual Studio können Sie Diagnosedaten für Rollen zu sammeln, die in Azure ausgeführt wird, wenn Sie den Dienst vor der Bereitstellung im Emulator ausführen. Alle Änderungen auf Diagnose in Visual Studio werden in der Datei diagnostics.wadcfgx gespeichert. Diese Konfiguration Einstellungen Speicherkonto Diagnosedaten Speicherort Bereitstellung Cloud-Dienst.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Aktivieren der Diagnose vor der Bereitstellung in Visual Studio

1. Klicken Sie im Kontextmenü für die Rolle, die Sie interessieren **Eigenschaften Sie**und wählen Sie die Registerkarte **Konfiguration** im Fenster **Eigenschaften** die Rolle.

1. Abschnitt **Diagnose** stellen Sie sicher, dass das Kontrollkästchen **Diagnose aktivieren** .

    ![Zugriff auf die Option Diagnose aktivieren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Wählen Sie das Auslassungszeichen (...) an das Speicherkonto, die Diagnosedaten gespeichert werden soll. Gewählte Speicherkonto ist das Verzeichnis, wo Diagnosedaten gespeichert werden.

    ![Geben Sie das Speicherkonto verwenden](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. Geben Sie im Dialogfeld **Storage-Verbindungszeichenfolge erstellen** an, ob Sie mit Azure Speicheremulator ein Azure-Abonnement oder manuell Anmeldeinformationen eingegebenen.

    ![Im Dialogfeld Konto Speicher](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Wählt Microsoft Azure Speicheremulator Option Verbindungszeichenfolge ist, UseDevelopmentStorage = True.

  - Bei Auswahl der Option Ihr Abonnement Sie können Azure-Abonnement zu verwenden und den Kontonamen. Sie können die Schaltfläche Konten verwalten Ihre Azure-Abonnements verwalten.

  - Wenn die Option manuell eingegebenen Anmeldeinformationen werden Sie aufgefordert, geben den Namen und Schlüssel der Azure-Konto Sie verwenden möchten.

1. Wählen Sie im Dialogfeld **Konfiguration Diagnose** anzeigen **Konfigurieren** . Jede Registerkarte (außer **Allgemein** und **Verzeichnisse**) stellt eine diagnostische Daten, die Sie sammeln können. Die Standardregisterkarte **Allgemein**bietet die folgenden Diagnose Daten Auflistung Optionen: **nur Fehler**, **alle Informationen**und **benutzerdefinierten**. Die Standardoption **nur Fehler**hat den geringsten Speicherplatz Warn- oder Tracing Nachrichten übertragen wird. Die Option alle Informationen überträgt die meisten Informationen und ist daher die teuerste Option in Bezug auf Speicher.

    ![Aktivieren von Azure Diagnostics und Konfiguration](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. Beispielsweise wählen Sie die Option **Benutzerdefiniert** , die Daten anpassen zu können.

1. Im **Datenträgerkontingent in MB** angegeben, wie viel Speicherplatz Sie zuordnen, in das Speicherkonto für Diagnosedaten möchten. Wenn Sie möchten, können Sie den Standardwert ändern.

1. Auf jeder Registerkarte Diagnosedaten sammeln möchten, wählen Sie die **Übertragung aktivieren <log type> ** das Kontrollkästchen. Wenn Sie Anwendungsprotokolle sammeln möchten, wählen Sie Kontrollkästchen **Aktivieren Übertragung von Anwendungsprotokollen** Register **Anwendungsprotokolle** ein. Geben Sie außerdem jeden Datentyp Diagnose erforderlichen Informationen. Abschnitt **Diagnose-Datenquellen konfigurieren** später in diesem Thema Informationen auf jeder Registerkarte.

1. Nachdem Sie Auflistung alle Diagnosedaten aktiviert haben, wählen Sie **OK** .

1. Führen Sie Ihre Azure Cloud Service Project wie gewohnt in Visual Studio. Wie die Anwendung werden die Protokollinformationen aktiviert Azure Storage-Konto angegebene.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Diagnose in Azure virtuelle Computer aktivieren

In Visual Studio können Sie Diagnosedaten für Azure virtuelle Computer sammeln.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Diagnose in Azure virtuelle Computer aktivieren

1. Im **Server-Explorer**Azure Knoten auswählen und Verbinden mit der Azure-Abonnement, wenn Sie nicht bereits verbunden sind.

1. Erweitern Sie den Knoten **virtuelle Computer** . Sie können einen neuen virtuellen Computer erstellen oder wählen Sie eine bereits vorhanden ist.

1. Wählen Sie im Kontextmenü für den virtuellen Computer, der Sie interessieren **Konfigurieren**. Zeigt das Dialogfeld Konfiguration virtueller Computer.

    ![Konfiguration von Azure VM](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Wenn es nicht bereits installiert ist, fügen Sie Microsoft Monitoring Agent Diagnose-Erweiterung. Diese Erweiterung können Sie Diagnosedaten für Azure Virtual Machine sammeln. In der Liste Installierte Erweiterung wählen Sie wählen ein Dropdownmenü verfügbar Erweiterung und dann Microsoft Monitoring Agent-Diagnose.

    ![Azure Virtual Machine-Erweiterung installieren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Diagnosen werden für die virtuellen Computer. Weitere Informationen finden Sie unter Azure VM Extensions und Funktionen.

1. Wählen Sie die Erweiterung hinzufügen und Anzeigen der **Diagnose** Konfigurationsdialogfeld **Hinzufügen** .

1. Wählen Sie die Schaltfläche **Konfigurieren** , geben Sie ein Speicherkonto, und klicken Sie dann auf **OK** .

    Jede Registerkarte (außer **Allgemein** und **Verzeichnisse**) stellt eine diagnostische Daten, die Sie sammeln können.

    ![Aktivieren von Azure Diagnostics und Konfiguration](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Die Standardregisterkarte **Allgemein**bietet die folgenden Diagnose Daten Auflistung Optionen: **nur Fehler**, **alle Informationen**und **benutzerdefinierten**. Die Standardoption **nur Fehler**hat den geringsten Speicherplatz Warn- oder Tracing Nachrichten übertragen wird. Die Option **alle Informationen** überträgt die meisten Informationen und ist daher die teuerste Option in Bezug auf Speicher.

1. Beispielsweise wählen Sie die Option **Benutzerdefiniert** , die Daten anpassen zu können.

1. Im **Datenträgerkontingent in MB** angegeben, wie viel Speicherplatz Sie zuordnen, in das Speicherkonto für Diagnosedaten möchten. Wenn Sie möchten, können Sie den Standardwert ändern.

1. Auf jeder Registerkarte Diagnosedaten sammeln möchten, wählen Sie die **Übertragung aktivieren <log type> ** das Kontrollkästchen.

    Wenn Sie Anwendungsprotokolle sammeln möchten, wählen Sie Kontrollkästchen **Aktivieren Übertragung von Anwendungsprotokollen** Register **Anwendungsprotokolle** ein. Geben Sie außerdem jeden Datentyp Diagnose erforderlichen Informationen. Abschnitt **Diagnose-Datenquellen konfigurieren** später in diesem Thema Informationen auf jeder Registerkarte.

1. Nachdem Sie Auflistung alle Diagnosedaten aktiviert haben, wählen Sie **OK** .

1. Speichern Sie das aktualisierte Projekt.

    Sie sehen eine Meldung im Fenster **Microsoft Azure-Aktivitätsprotokoll** virtuellen Computer aktualisiert wurde.

## <a name="configure-diagnostics-data-sources"></a>Diagnose von Datenquellen konfigurieren

Nach dem Aktivieren der Diagnose-Datensammlung können Sie genau welche Datenquellen, die Sie sammeln möchten und welche Informationen gesammelt werden. Folgendes ist eine Liste der Registerkarten im Dialogfeld **Diagnose: Konfiguration** und jede Konfigurationsoption bedeutet.

### <a name="application-logs"></a>Anwendungsprotokolle

**Anwendungsprotokolle** enthalten Diagnoseinformationen durch eine Anwendung erstellt. Wenn Sie Anwendungsprotokolle erfassen möchten, aktivieren Sie das Kontrollkästchen **ermöglichen Übertragung der Anwendungsprotokolle** . Sie können erhöhen oder verringern die Anzahl der Minuten Wenn die Anwendungsprotokolle übertragen werden an das Speicherkonto **übertragen** Periode (min) ändern. Sie können auch die Informationen in das Protokoll durch das Protokollwert festlegen ändern. Beispielsweise können Sie **ausführlich** informieren oder **kritisch** nur schwerwiegende Fehler erfassen. Haben Sie bestimmte Diagnoseanbieter, der Anwendungsprotokolle ausgibt, können Sie diese Feld **Anbieter GUID** des Anbieters GUID hinzufügen erfassen.

  ![Anwendungsprotokolle](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Finden Sie weitere Informationen Anwendungsprotokolle [Diagnoseprotokoll für Web-apps in Azure App Service aktivieren](./app-service-web/web-sites-enable-diagnostic-log.md) .

### <a name="windows-event-logs"></a>Windows-Ereignisprotokolle

Wenn Sie Windows-Ereignisprotokolle erfassen möchten, aktivieren Sie das Kontrollkästchen **Übertragung der Windows-Ereignisprotokolle zu aktivieren** . Sie können erhöhen oder verringern die Anzahl der Minuten Wenn die Ereignisprotokolle übertragen werden an das Speicherkonto **übertragen** Periode (min) ändern. Aktivieren Sie die Kontrollkästchen für die Ereignistypen, die Sie verfolgen möchten.

  ![Ereignisprotokolle](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Wenn Sie Azure SDK 2.6 oder höher und eine Datenquelle angeben möchten, geben Sie ihn in den **<Data source name>** Text ein und wählen dann neben **Hinzufügen** . Die Datei diagnostics.cfcfg wird die Datenquelle hinzugefügt.

Wenn Sie Azure SDK 2.5 und eine Datenquelle angeben, können Sie damit die `WindowsEventLog` auf der diagnostics.wadcfgx-Datei, wie im folgenden Beispiel.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Leistungsindikatoren

Leistungsindikatorinformationen können Sie Engpässe im System suchen und System- und Leistung zu optimieren. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Leistungsindikatoren in einer Azure-Anwendung](https://msdn.microsoft.com/library/azure/hh411542.aspx) . Wenn Sie Leistungsindikatoren erfassen möchten, aktivieren Sie das Kontrollkästchen **Übertragung von Leistungsindikatoren aktivieren** . Sie können erhöhen oder verringern die Anzahl der Minuten Wenn die Ereignisprotokolle übertragen werden an das Speicherkonto **übertragen** Periode (min) ändern. Aktivieren Sie die Kontrollkästchen für die Leistungsindikatoren, die Sie verfolgen möchten.

  ![Leistungsindikatoren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Um einen Leistungsindikator zu verfolgen, die nicht mit der vorgeschlagenen Syntax Geben Sie ein, und klicken Sie dann auf **Hinzufügen** . Das Betriebssystem des virtuellen Computers bestimmt, welche Leistungsindikatoren überwachen können. Weitere Informationen zur Syntax finden Sie in der [Pfadangabe Zähler](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Infrastrukturprotokolle

Wenn Sie möchten Infrastrukturprotokolle enthalten Informationen zu Azure Diagnoseinfrastruktur, RAS-Modul und das RemoteForwarder-Modul Kontrollkästchen Sie die **Übertragung des Infrastruktur-Protokolle aktivieren** . Sie können erhöhen oder verringern die Anzahl der Minuten, wenn die Protokolle übertragen werden, an das Speicherkonto übertragen Periode (min) ändern.

  ![Diagnoseprotokolle Infrastruktur](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Weitere Informationen finden Sie [Durch Verwendung von Azure Diagnostics Logging Daten sammeln](https://msdn.microsoft.com/library/azure/gg433048.aspx) .

### <a name="log-directories"></a>Verzeichnisse

Sie möchten Verzeichnisse, enthalten Daten aus Verzeichnisse für Internetinformationsdienste (IIS) Anfragen Fehler bei Anfragen oder Ordnern, aktivieren Sie das Kontrollkästchen **ermöglichen Übertragung der Verzeichnisse** . Sie können erhöhen oder verringern die Anzahl der Minuten, wenn die Protokolle übertragen werden, an das Speicherkonto **übertragen** Periode (min) ändern.

Sie können Felder der Protokolle auswählen wie **IIS-Protokolle** und Anforderungsprotokolle **Fehler** erfassen möchten. Standardcontainernamen Speicher bereitgestellt, aber Sie können die Namen ggf. ändern.

Außerdem können Sie Protokolle in einem beliebigen Ordner aufnehmen. Nur die Pfadangabe im Abschnitt **von absolutes Verzeichnis** , und klicken Sie dann auf **Verzeichnis hinzufügen** . Die Protokolle werden die angegebenen Container erfasst.

  ![Verzeichnisse](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW-Protokolle

Wenn Sie [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (ETW) und ETW-Protokolle möchten, aktivieren Sie das Kontrollkästchen **Übertragung von ETW-Protokolle aktivieren** . Sie können erhöhen oder verringern die Anzahl der Minuten, wenn die Protokolle übertragen werden, an das Speicherkonto **übertragen** Periode (min) ändern.

Die Ereignisse werden aufgezeichnet, Ereignisquellen und Ereignis Manifeste, die Sie angeben. Um eine Ereignisquelle angeben, geben Sie einen Namen im Abschnitt **Ereignisquellen** und wählen Sie dann die Schaltfläche **Ereignisquelle hinzuzufügen** . Ebenso können Sie ein ereignismanifest im Abschnitt **Zeigt Ereignis** angeben und wählen Sie dann **Ereignis Manifest hinzufügen** .

  ![ETW-Protokolle](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Unterstützt der ETW-Framework in ASP.NET durch Klassen in [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110)-Namespace. Microsoft.WindowsAzure.Diagnostics Namespace erbt und Standard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) Klassen, ermöglicht die Verwendung von [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) Rahmen eine Protokollierung der Azure-Umgebung. Weitere Informationen finden Sie unter [nehmen Kontrolle der Protokollierung und Tracing in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) und [Diagnose in Azure Cloud Services und virtuelle Computer aktivieren](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Speicherabbilder

Wenn Sie möchten Informationen als eine Rolleninstanz stürzt ab, aktivieren Sie das Kontrollkästchen **ermöglichen Übertragung der Crash Dumps** . (Da ASP.NET die meisten Ausnahmen behandelt, ist dies im Allgemeinen nur für Workerthreads sinnvoll.) Erhöhen oder verringern den Prozentsatz des Speicherplatzes für die Speicherabbilder durch Ändern des Kontingentwertes **Verzeichnis (%)** . Sie können den Speichercontainer ändern, wo die Speicherabbilder werden gespeichert und können Sie auswählen, ob einen **Vollständiger** oder **Mini** -Dump sammeln möchten.

Derzeit Prozesse werden aufgelistet. Aktivieren Sie die Kontrollkästchen für die Prozesse, die Sie erfassen möchten. Um einen anderen Prozess zur Liste hinzuzufügen, geben Sie den Namen des Prozesses und dann die Schaltfläche **Prozess hinzufügen** .

  ![Speicherabbilder](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Siehe [nehmen Kontrolle der Protokollierung und Verfolgung in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) und [Microsoft Azure Diagnostics Teil 4: benutzerdefinierte Protokollierung Komponenten und Azure Diagnostics 1.3 ändert](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) Weitere Informationen.

## <a name="view-the-diagnostics-data"></a>Die Diagnosedaten anzeigen

Nachdem Sie die Diagnosedaten für einen Cloud-Dienst oder einen virtuellen Computer gesammelt haben, können Sie es anzeigen.

### <a name="to-view-cloud-service-diagnostics-data"></a>Cloud-Dienst Diagnosedaten anzeigen

1. Die Cloud-Dienst wie üblich bereitstellen und ausführen.

1. Die Diagnosedaten anzuzeigen in einen Bericht Visual Studio generiert oder Tabellen in Ihrem Speicherkonto. Zum Anzeigen der Daten in einem Bericht öffnen Sie **Cloud Explorer** oder im **Server-Explorer**zu, öffnen Sie das Kontextmenü des Knotens für die Rolle, die Sie interessieren, und dann **Ansicht Diagnosedaten**.

    ![Anzeigen von Diagnosedaten](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Ein Bericht die verfügbaren Daten angezeigt.

    ![Microsoft Azure-Diagnosebericht in Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Wenn die aktuellsten Daten angezeigt werden, möglicherweise die Übertragung bis zum verstreichen warten.

    Wählen Sie die Verknüpfung **Aktualisieren** , die Daten sofort zu aktualisieren, oder ein Intervall in **Aktualisierungsintervall** Dropdown-Listenfeld Daten automatisch aktualisiert. Um die Daten zu exportieren, wählen Sie **in CSV exportieren** eine CSV-Datei erstellen, die Sie in einer Kalkulationstabelle öffnen können.

    Öffnen Sie in **Cloud Explorer** oder im **Server-Explorer**das Speicherkonto, das der Bereitstellung zugeordnet ist.

1. Die Diagnose Tabellen Tabelle Viewer öffnen und die gesammelten Daten überprüfen. Öffnen Sie IIS-Protokolle und benutzerdefinierte Protokolle BLOB-Container. Anhand der folgenden Tabelle finden Sie den Tabelle oder BLOB-Container mit den Daten, die Sie interessieren. Neben dieser Protokolldatei enthalten Einträge in der EventTickCount, DeploymentId, Rolle und RoleInstance können Sie erkennen, welche virtuellen Computer und die Rolle die Daten generiert und. 

  	|Diagnosedaten|Beschreibung|Speicherort|
  	|---|---|---|
  	|Anwendungsprotokolle|Protokolle, die der Code durch Aufrufen von Methoden der System.Diagnostics.Trace-Klasse generiert.|WADLogsTable|
  	|Ereignisprotokolle|Diese Daten werden aus den Windows-Ereignisprotokollen auf den virtuellen Computern. Windows speichert Informationen in diesen Protokollen, aber auch verwenden, um Fehler auch Informationen protokollieren.|WADWindowsEventLogsTable|
  	|Leistungsindikatoren|Sie können auf einen beliebigen Leistungsindikator Daten, die auf dem virtuellen Computer verfügbar ist. Das Betriebssystem stellt Leistungsindikatoren für Statistiken wie Arbeitsspeicher Verwendung und Prozessor enthalten.|WADPerformanceCountersTable|
  	|Infrastrukturprotokolle|Diese Protokolle werden von Diagnoseinfrastruktur selbst generiert.|WADDiagnosticInfrastructureLogsTable|
  	|IIS-Protokolle|Diese Protokolle zeichnen Webanfragen. Wird der Cloud-Dienst viel Datenverkehr dieser Protokolle abgebildet, können Sie erfassen und speichern Sie diese Daten nur bei Bedarf.|Sie finden konnte Anforderung im BLOB-Container Bündel-Iis-Failedreqlogs unter einem Pfad für diese Bereitstellung, Rolle und Instanz protokolliert. Vollständige Protokolle unter Bündel Iis Logfiles finden. Alle Dateien werden in der Tabelle WADDirectories Posten.|
  	|Speicherabbilder|Diese Informationen stellen binäre Abbilder der Cloud-Dienst-Prozess (in der Regel eine Worker-Rolle).|Bündel Stauchung Dumps BLOB-container|
  	|Benutzerdefinierte Dateien|Meldet Sie vordefinierte Daten.|Sie können im Code den Speicherort der benutzerdefinierten Protokolldateien in das Speicherkonto angeben. Beispielsweise können Sie einen benutzerdefinierten blobcontainer angeben.|

1. Abgeschnittene Daten eines beliebigen Typs können Sie versuchen, erhöhen den Puffer für diese Daten Typ oder kürzen das Intervall zwischen die Übertragung von Daten vom virtuellen Computer Ihres Speicherkontos.

1. (optional) Bereinigen Sie Daten aus dem Speicherkonto gelegentlich zu Gesamtspeicherkosten.

1. In diesem Fall eine vollständige Bereitstellung Cloud-Dienst nimmt Änderungen an der Diagnosekonfiguration diagnostics.cscfg Datei (.wadcfgx für Azure SDK 2.5) in Azure aktualisiert Wenn Sie stattdessen eine vorhandene Bereitstellung aktualisieren, nicht die Datei .cscfg in Azure aktualisiert. Sie können noch Diagnose, ändern, indem Sie die Schritte im nächsten Abschnitt. Weitere Informationen eine vollständige Bereitstellung und Aktualisierung einer vorhandenen Bereitstellung finden Sie unter [Assistent Azure veröffentlichen](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Daten der virtuellen Maschine Diagnose anzeigen

1. Wählen Sie im Kontextmenü für den virtuellen Computer **Diagnosedaten anzeigen**.

    ![Anzeigen von Diagnosedaten in Azure Virtual machine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Öffnet das Fenster **Zusammenfassung Diagnose** .

    ![Zusammenfassung Azure Virtual Machine-Diagnose](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Wenn die aktuellsten Daten angezeigt werden, möglicherweise die Übertragung bis zum verstreichen warten.

    Wählen Sie die Verknüpfung **Aktualisieren** , die Daten sofort zu aktualisieren, oder ein Intervall in **Aktualisierungsintervall** Dropdown-Listenfeld Daten automatisch aktualisiert. Um die Daten zu exportieren, wählen Sie **in CSV exportieren** eine CSV-Datei erstellen, die Sie in einer Kalkulationstabelle öffnen können.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Nach der Bereitstellung Cloud Service Diagnose konfigurieren

Falls Sie ein Problem mit einer Wolke untersuchen, bereits ausgeführt, Sie möchten Daten sammeln, die Sie angegeben haben vor der Bereitstellung der Rolle. In diesem Fall können Sie die Daten sammeln, mit den Einstellungen im Server-Explorer starten. Diagnose für eine einzelne Instanz oder alle Instanzen können eine Rolle, je nachdem, ob das Diagnose Konfigurationsdialogfeld im Kontextmenü für die Instanz oder der Rolle öffnen. Konfigurieren den Rolle Knoten wenden Sie Änderungen für alle Instanzen. Konfigurieren den Knoten wenden Sie Änderungen für diese Instanz.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Diagnose für ausgeführten Clouddienst konfigurieren

1. Im Server-Explorer erweitern Sie den Knoten **Clouddienste** und dann Knoten die Rolle und/oder Instanz zu suchen.

    ![Diagnose konfigurieren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. Einstellungen Sie im Kontextmenü für eine Instanz oder eine Rolle Knoten **Update Diagnose**, und wählen Sie dann die Diagnose, die Sie sammeln möchten.

    Informationen zu den Konfigurationen finden Sie unter **Konfigurieren Diagnose Datenquellen** in diesem Thema. Informationen zum Anzeigen der Diagnosedaten finden Sie unter **anzeigen die Diagnosedaten** in diesem Thema.

    Datensammlung im **Server-Explorer**ändern, bleiben diese Änderungen erst vollständig Cloud-Dienst erneut. Verwenden Sie die Standardeinstellung Einstellungen veröffentlichen die Änderungen nicht überschrieben, da standardmäßig veröffentlicht wird die vorhandene Bereitstellung aktualisieren, anstatt eine vollständige erneute. Um sicherzustellen, dass die zum Zeitpunkt der Bereitstellung zu löschen, wechseln Sie zur Registerkarte **Erweiterte Einstellungen** im Webpublishing-Assistenten, und deaktivieren Sie das Kontrollkästchen **Bereitstellung aktualisieren** . Erneut mit diesem Kontrollkästchen deaktiviert zurückgesetzt die Einstellung auf die in der Datei .wadcfgx (oder wadcfg) wie über den Eigenschaften-Editor für die Rolle. Wenn Sie die Bereitstellung aktualisieren, hält Azure Alter Settings.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Problembehandlung bei der Azure-Cloud-service

Wenn treten Probleme mit der Cloud Service Projekte wie eine Rolle in einer Statusanzeige "beschäftigt" stecken wiederholt recycelt oder löst einen internen Serverfehler, Tools und Techniken, mit denen Sie diese Probleme diagnostizieren und beheben können. Beispiele für Probleme und Lösungsvorschläge sowie eine Übersicht über die Konzepte und Tools zum Diagnostizieren und Beheben dieser Fehler finden Sie in [Azure PaaS berechnen-Diagnosedaten](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Fragen & Antworten

**Was ist die Größe des Puffers und wie groß sollte?**

Auf jeder virtuellen Instanz beschränken Kontingente diagnostischen Daten im lokalen Dateisystem gespeichert werden können. Darüber hinaus geben Sie eine Puffergröße für jeden Diagnosedaten verfügbar ist. Diese Puffergröße verhält sich wie ein individuelles Kontingent für diesen Typ von Daten. Überprüfen Sie unten im Dialogfeld, bestimmen Sie insgesamt Kontingent und den Speicher bleibt. Wenn Sie größere Puffer oder mehr Datentypen angeben, nähern Sie die gesamte Quote. Diagnostics.wadcfg/.wadcfgx-Konfigurationsdatei bearbeiten, können Sie das gesamte Kontingent ändern. Die Diagnosedaten im gleichen Dateisystem als Ihre Anwendungsdaten gespeichert Wenn Ihre viel Festplattenspeicher Anwendung Diagnose Gesamtkontingent erhöhen sollten nicht.

**Wie ist lang die Übertragung, und wie lange sollte?**

Übertragung beträgt die Zeitspanne verstrichen ist, Daten erfasst. Nach jeder Übertragung werden Daten aus dem lokalen Dateisystem auf einem virtuellen Computer mit Tabellen in das Speicherkonto verschoben. Übersteigt der gesammelten Daten die Quote vor Ablauf einer Frist übertragen, werden ältere Daten verworfen. Sie sollten zu den übertragungszeitraum Sie Daten verlieren, da Ihre Daten die Puffergröße oder das gesamte Kontingent überschreitet.

**Sind Zeitstempel in welcher Zeitzone**

Die Zeitstempel sind in der lokalen Zeitzone des Rechenzentrums, die Cloud-Dienst hostet. Die folgenden drei Timestamp-Spalten in den Tabellen dienen.

  - **PreciseTimeStamp** ist der ETW-Zeitstempel des Ereignisses. Das ist die Zeit, die das Ereignis vom Client protokolliert.

  - **TIMESTAMP** ist PreciseTimeStamp der Upload Häufigkeit Grenze abgerundet. Also, wenn die Frequenz zum Hochladen Zeit 00:17:12 5 Minuten und das Ereignis ist TIMESTAMP 00:15:00.

  - **Timestamp** ist der Zeitstempel die Entität in Azure Tabelle erstellte.

**Wie verwalte ich Kosten beim Sammeln von Diagnoseinformationen?**

Die Standardeinstellungen (**Protokollebene** auf **Fehler** und **Zeitraum übertragen** auf **1 Minute**festgelegt) sollen Kosten zu minimieren. Computekosten erhöht, wenn weitere Diagnosedaten sammeln oder Übertragung Zeit verringern. Sammeln Sie nicht mehr Daten, als Sie benötigen, und vergessen Sie nicht, die Datensammlung deaktivieren, wenn Sie nicht mehr benötigen. Sie können immer es wieder auch zur Laufzeit aktivieren wie im vorherigen Abschnitt gezeigt.

**Wie erhalte ich Fehler Anforderungsprotokolle aus IIS?**

Standardmäßig wird IIS konnte Anforderungsprotokolle erfasst. Konfigurieren Sie IIS beim Bearbeiten der Datei web.config für Ihre Webrolle sammeln.

**Ich erhalte keine Ablaufverfolgungsinformationen aus RoleEntryPoint Methoden OnStart. Was ist los?**

Die Methoden des RoleEntryPoint werden im Kontext des WAIISHost.exe nicht IIS aufgerufen. Daher die Konfigurationsinformationen in web.config ermöglicht verfolgen normalerweise zutrifft. Zum Beheben dieses Problems Webprojekt Rolle fügen Sie eine config-Datei hinzu und Dateinamen Sie den die Ausgabeassembly entsprechend mit dem RoleEntryPoint Code. Im Webprojekt Rolle standardmäßig wäre der Name der config-Datei WAIISHost.exe.config. Fügen Sie die folgenden Zeilen in diese Datei ein:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Festgelegt, im Fenster **Eigenschaften** die Eigenschaft **In Ausgabeverzeichnis kopieren** auf **immer kopieren**.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Diagnoseprotokoll in Azure finden Sie unter [Aktivieren der Diagnose in Azure Cloud Services und virtuellen Maschinen](./cloud-services/cloud-services-dotnet-diagnostics.md) und [aktivieren das Diagnoseprotokoll für Web-apps in Azure App Service](./app-service-web/web-sites-enable-diagnostic-log.md).

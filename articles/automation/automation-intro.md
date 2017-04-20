<properties
    pageTitle="Was ist Azure Automatisierung | Microsoft Azure"
    description="Erfahren Sie, welchen Wert Azure Automatisierung bietet und Antworten auf häufig gestellte Fragen, die Sie erstellen, loslegen können Runbooks mit Azure Automation DSC."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Was ist Automatisierung Azure Automatisierung, Azure Automatisierungsbeispiele"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure Automation overview

Microsoft Azure Automatisierung ermöglicht Benutzern manuellen langer, fehleranfällig und häufig durchgeführte Aufgaben automatisieren, die häufig in einer Wolke und Enterprise-Umgebung ausgeführt werden. Es spart Zeit, erhöht die Zuverlässigkeit von regelmäßigen Verwaltungsaufgaben und plant auch sie automatisch in regelmäßigen Abständen ausgeführt werden. Automatisieren Prozesse Runbooks oder mit der gewünschten Konfiguration Konfigurationsmanagement automatisiert werden können. Dieser Artikel bietet Überblick Azure Automation und einige allgemeine Fragen beantwortet. Sie können zu anderen Artikeln in dieser Bibliothek für ausführlichere Informationen zu den verschiedenen Themen verweisen.


## <a name="automating-processes-with-runbooks"></a>Automatisierung von Prozessen mit runbooks

Ein Runbook ist eine Reihe von Aufgaben, die in Azure Automation einen automatisierten Prozess ausführen. Möglicherweise einfach wie Starten eines virtuellen Computers einen Eintrag erstellen oder möglicherweise eine komplexe Runbook, die kombiniert andere kleinere Runbooks um einen komplexen Vorgang auf mehrere Ressourcen oder sogar mehrere Wolken und lokalen Umgebungen auszuführen.  

Beispielsweise Sie möglicherweise haben einen vorhandenen manuellen Prozess für eine SQL-Datenbank abgeschnitten, wenn sie maximalen Größe nähert, die Verbindung mit dem Server mehrere Schritte umfasst mit der Datenbank verbinden, die aktuelle Größe der Datenbank, überprüfen Sie, ob der Schwellenwert überschritten abgeschnitten und Benutzer benachrichtigen. Statt einzelnen Schritte manuell ausführen, können Sie ein Runbook erstellen, die alle Tasks als ein einzelner Prozess ausgeführt werden. Sie starten Runbooks, die erforderlichen Informationen wie SQL-Servername, Datenbankname und Empfänger e-Mail- und lehnen Sie sich zurück, während der Vorgang abgeschlossen ist. 


## <a name="what-can-runbooks-automate"></a>Was können Runbooks automatisieren?

Runbooks in Azure Automation basieren auf Windows PowerShell oder Windows PowerShell Workflow sie nichts tun, PowerShell tun kann. Wenn eine Anwendung oder ein Dienst eine API verfügt, kann ein Runbook mit arbeiten. Haben Sie ein PowerShell-Modul für die Anwendung können Sie Modul in Azure laden und diese Cmdlets in Ihrem Runbook einschließen. Azure Automatisierung Runbooks in Azure Cloud und Cloudressourcen oder externe Ressourcen auf die Cloud zugreifen können. Mit [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)können Runbooks im lokalen Rechenzentrum verwalten lokale Ressourcen ausführen. 


## <a name="getting-runbooks-from-the-community"></a>Abrufen von Runbooks aus der Gemeinschaft

[Runbook Gallery](automation-runbook-gallery.md#runbooks-in-runbook-gallery) enthält Runbooks von Microsoft und die Gemeinschaft können entweder unverändert in Ihrer Umgebung oder für eigene Zwecke anpassen. Sie sind auch als Bezugnahmen auf die eigene Runbooks erstellen. Sie tragen auch eigene Runbooks der Galerie fest, dass andere Benutzer nützlich sein könnten. 


## <a name="creating-runbooks-with-azure-automation"></a>Erstellen von Runbooks mit Azure Automatisierung 

Können Sie [Ihre eigenen Runbooks erstellen](automation-creating-importing-runbook.md) oder Ändern von Runbooks [Runbook Gallery](http://msdn.microsoft.com/library/azure/dn781422.aspx) für Ihre Bedürfnisse. Es gibt drei verschiedene [Runbook Typen](automation-runbook-types.md) auf, die Sie können basierend auf Ihren Bedarf und PowerShell Erfahrung. Wenn Sie direkt mit PowerShell-Code arbeiten möchten, dann können Sie ein [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) oder [PowerShell Workflow Runbook](automation-runbook-types.md#powershell-workflow-runbooks) offline oder im Azure-Portal im [Text-Editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) bearbeiten. Wenn Sie ein Runbook ohne zugrunde liegenden Code bearbeiten möchten, können Sie ein [grafisch Runbook](automation-runbook-types.md#graphical-runbooks) mit [Grafikeditor](automation-graphical-authoring-intro.md) in Azure-Portal erstellen. 

Bevorzugen Sie gerade lesen? Sehen Sie sich die unter Video von Microsoft Ignite Sitzung im Mai 2015. Anmerkung: Konzepte und Features in diesem Video erläutert sind korrekt, Azure Automation sehr fortgeschritten ist, da dieses Video aufgezeichnet wurde, es jetzt besitzt eine umfangreichere Benutzeroberfläche im Azure-Portal und zusätzliche Funktionen unterstützt.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Konfigurationsmanagement mit der gewünschten Konfiguration automatisieren 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) ist eine Management-Plattform, mit der Sie verwalten, Bereitstellung und Konfiguration für physischen Hosts und virtuellen Maschinen mit deklarativer Syntax für PowerShell erzwungen. Konfigurationen können auf einem zentralen DSC-Server ziehen, die Zielcomputer automatisch abrufen und anwenden können. DSC bietet eine Reihe von PowerShell-Cmdlets, mit denen Sie Konfigurationen und Ressourcen zu verwalten.  

[Azure Automation DSC](automation-dsc-overview.md) ist eine cloudbasierte Lösung für PowerShell DSC, die Dienstleistungen für Unternehmen erforderlich.  Sie können der DSC-Ressourcen in Azure Automation und Konfigurationen für virtuellen oder physischen Computern, die sie von einem DSC Pull-Server in der Azure-Cloud abrufen.  Darüber hinaus reporting Services, die darüber informiert, dass Sie wichtige Ereignisse wie ihre zugewiesene Konfiguration Knoten abgewichen haben und eine neue Konfiguration angewendet wurde. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Erstellen mit Azure Automation DSC-Konfigurationen

[DSC Konfigurationen](automation-dsc-overview.md#azure-automation-dsc-terms) angeben gewünschten Status eines Knotens  Mehrere Knoten können die gleiche Konfiguration, um sicherzustellen, dass alle einen identischen Status verwalten anwenden.  Erstellen Sie eine Konfiguration mit einem beliebigen Text-Editor auf dem lokalen Computer und importieren es in Azure, können kompilieren, und Knoten.


## <a name="getting-modules-and-configurations"></a>Abrufen von Modulen und Konfigurationen 

Sie können [PowerShell-Module](automation-runbook-gallery.md#modules-in-powershell-gallery) mit Cmdlets, mit denen Sie in Ihren Runbooks und DSC-Konfigurationen von [PowerShell Galerie](http://www.powershellgallery.com/)abrufen. Dieser Katalog von Azure-Portal starten und direkt in Azure Automation Module importieren oder downloaden und manuell importieren. Module können nicht direkt von der Azure-Portal installieren, aber Sie können sie wie andere Modul zu installieren. 


## <a name="example-practical-applications-of-azure-automation"></a>Beispiel-Praxis der Azure-Automatisierung 

Es folgen Beispiele der Arten von Automatisierungsszenarien mit Azure Automation. 

* Erstellen Sie und kopieren Sie virtueller Computer in verschiedenen Azure-Abonnements. 
* Planen Sie Kopien von einem lokalen Computer ein Container Azure BLOB-Speicher. 
* Automatisieren Sie Sicherheitsfunktionen, wie Verweigern von einem Client fordert bei ein DOS-Angriff erkannt wird. 
* Sicherstellen Sie, dass Computer konfigurierten ständig ausgerichtet.
* Verwalten Sie kontinuierliche Bereitstellung von Anwendungscode in Cloud und lokale Infrastruktur. 
* Erstellen Sie Active Directory-Gesamtstruktur in Azure für die laborumgebung. 
* Kürzen Sie eine Tabelle in einer SQL-Datenbank, wenn DB maximalen Größe nähert. 
* Remoteaktualisierung umgebungseinstellungen für eine Azure-Website. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Was bedeutet Azure Automation, andere Automatisierungstools?

[Service Management-Automatisierung (SMA)](http://technet.microsoft.com/library/dn469260.aspx) soll in der privaten Cloud Verwaltungsaufgaben automatisieren. Es wird lokal in Ihrem Rechenzentrum als Komponente von [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/)installiert. SMA unterstützt keine [Grafiken Runbooks](automation-graphical-authoring-intro.md)SMA und Azure-Automatisierung verwenden dasselbe Runbook Format basierend auf Windows PowerShell und Windows PowerShell Workflow.  

Automatisierung von lokalen Ressourcen [System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) vorgesehen. Er verwendet einen anderen Runbook Format Azure Automatisierung und Service Management-Automatisierung und Benutzeroberfläche Runbooks ohne Skripte erstellen. Die Runbooks bestehen aus Aktivitäten Integration Packs, die speziell für Orchestrator geschrieben werden. 


## <a name="where-can-i-get-more-information"></a>Wo erhalte ich weitere Informationen? 

Eine Vielzahl von Ressourcen stehen Ihnen über Azure Automation und erstellen eigene Runbooks informieren. 

* **Azure-Bibliothek** sind Sie jetzt. Die Artikel in dieser Bibliothek enthalten vollständige Dokumentation der Konfiguration und Verwaltung von Azure und zum Erstellen eigener Runbooks. 
* [Azure PowerShell-Cmdlets](http://msdn.microsoft.com/library/jj156055.aspx) Informationen zur Automatisierung von Azure Vorgänge mithilfe von Windows PowerShell. Runbooks verwenden diese Cmdlets mit Azure Ressourcen arbeiten. 
* [Management Blog](https://azure.microsoft.com/blog/tag/azure-automation/) bietet die neueste Informationen von Microsoft Azure Automation und anderen Technologien. Abonnieren Sie diesen Blog mit Azure Automation Team Stand. 
* [Automatisierung Forum](http://go.microsoft.com/fwlink/p/?LinkId=390561) können Sie Fragen zur Azure-Automatisierung von Microsoft und der Community Automatisierung behandelt werden. 
* [Azure Automatisierung Cmdlets](https://msdn.microsoft.com/library/mt244122.aspx) Informationen zur Automatisierung von Verwaltungsaufgaben. Cmdlets zum Verwalten von Automation-Konten, Anlagen, Runbooks DSC enthält.


## <a name="can-i-provide-feedback"></a>Kann ich Feedback Geben? 

**Bitte geben Sie uns Feedback!** Wenn Sie eine Projektmappe Azure Automation Runbook oder ein Integrationsmodul suchen, Buchen Sie ein Skript anfordern Script Center. Haben Sie Feedback oder Feature Anfragen für Azure Automation, stellen Sie sie auf [Benutzer](http://feedback.windowsazure.com/forums/34192--general-feedback). Vielen Dank! 



<properties 
pageTitle="Cloud-Dienst Lebenszyklusereignisse behandeln | Microsoft Azure" 
description="Erfahren Sie, wie Methoden Lebenszyklus einer Cloud-Dienst Rolle in .NET verwendet werden" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Anpassen des Lebenszyklus einer Web- oder Arbeitskraft Rolle in .NET

Wenn eine Worker-Rolle erstellen, erweitern Sie [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) -Klasse bietet Methoden überschreiben, mit denen Sie auf Lebenszyklusereignisse reagieren. Für Webrollen ist diese Klasse optional, so dass Sie auf Lebenszyklusereignisse reagieren müssen.

## <a name="extend-the-roleentrypoint-class"></a>Erweitern Sie die RoleEntryPoint-Klasse

Die [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) -Klasse enthält Methoden, die von Azure aufgerufen wenn es **startet**, **wird ausgeführt**oder **beendet** Web- oder Worker-Rolle. Sie können diese Methoden zum Verwalten der Rolle Initialisierung Rollensequenzen Herunterfahren oder Ausführungsthread der Rolle optional überschreiben. 

Wenn **RoleEntryPoint**erweitern, sollten Sie die folgenden Verhaltensweisen der Methoden sein:

-   Methoden [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) und [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) zurück einen booleschen Wert, also von diesen Methoden **false** zurückgegeben.

     Code **false**zurück, wird die Rolle abrupt beendet ohne jede Herunterfahren eventuell an. Im Allgemeinen sollten Sie die **OnStart** -Methode **false** zurückgibt.
     
-   Nicht abgefangene Ausnahmen innerhalb einer Überladung der **RoleEntryPoint** wird als eine nicht behandelte Ausnahme behandelt.

     Tritt eine Ausnahme innerhalb eines Lebenszyklus Methoden Azure das [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) -Ereignis wird ausgelöst, und der Prozess wird beendet. Nach Ihrer Rolle offline geschaltet wurde, wird sie von Azure gestartet. Tritt ein Ausnahmefehler [Beenden](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) -Ereignis wird nicht ausgelöst, und die **OnStop** -Methode nicht aufgerufen.

Wenn Ihre Rolle nicht starten oder zwischen beschäftigt, Initialisierung und beenden Mitgliedstaaten recycling, kann Code eine nicht behandelte Ausnahme innerhalb eines Lebenszyklusereignisse werfen bei jedem die Rolle Neustart. In diesem Fall verwenden Sie [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) -Ereignis die Ursache der Ausnahme und entsprechend behandelt. Ihre Rolle kann auch von der Methode [ausgeführt](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) zurück, wodurch die Funktion neu starten. Weitere Informationen über Bereitstellung Zustände finden Sie [Allgemeine Probleme die Ursache Rollen an Recycling](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Wenn Sie Ihre Anwendung zu **Azure Tools for Microsoft Visual Studio** verwenden, erweitern die Rolle Projektvorlagen **RoleEntryPoint** -Klasse, in den Dateien *WebRole.cs* und *WorkerRole.cs* .

## <a name="onstart-method"></a>OnStart-Methode

Die **OnStart** -Methode wird aufgerufen, wenn die Instanz der Rolle in Azure online geschaltet wird. Beim Ausführen des OnStart-Codes die Rolleninstanz als **gebucht** gekennzeichnet ist und keine externen Datenverkehr, durch das System zum Lastenausgleich geleitet. Überschreiben Sie diese Methode, um Initialisierungsaufgaben Ereignishandler implementieren und Starten von [Azure Diagnostics](cloud-services-how-to-monitor.md)ausführen.

Wenn **OnStart** **true**zurückgibt, die Instanz erfolgreich initialisiert wird und Azure die **RoleEntryPoint.Run** -Methode aufruft. **OnStart** **false**zurück, beendet die Funktion sofort ohne keine Protokollsequenzen geplantes Herunterfahren ausgeführt.

Im folgenden Codebeispiel wird veranschaulicht, wie die **OnStart** -Methode überschreiben. Diese Methode konfiguriert und Diagnose Monitor startet, wenn die Rolleninstanz beginnt und Übertragung von Daten an ein Speicherkonto eingerichtet:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop-Methode

**OnStop** -Methode wird aufgerufen, nachdem eine Instanz der Rolle von Azure offline geschaltet wurde und bevor der Prozess beendet wird. Überschreiben Sie diese Methode, um für die Rolleninstanz heruntergefahren erforderlichen Code aufrufen.

> [AZURE.IMPORTANT] **OnStop** -Methode aus Code hat eine begrenzte Zeit in Anspruch, wenn sie aus anderen Gründen als einem Benutzer initiiertes Herunterfahren aufgerufen wird. Nach Ablauf dieser Zeitspanne wird der Prozess beendet, sodass Sie Code in die **OnStop** -Methode kann schnell sicherstellen oder toleriert nicht ausgeführt. **OnStop** -Methode wird aufgerufen, nachdem das **Beenden** -Ereignis ausgelöst wird.


## <a name="run-method"></a>Run-Methode

Sie können die **Run** -Methode implementieren einen langer Thread für die Rolleninstanz überschreiben.

Überschreiben der **Run** -Methode ist nicht erforderlich. die Implementierung beginnt einen Thread immer schläft. Wenn Sie die **Run** -Methode überschreiben, sollten Code unbegrenzt blockieren. Die **Run** -Methode zurückgegeben wird, ist die Rolle automatisch ordnungsgemäß recycelt; in anderen Worten Azure löst das Ereignis **beendet** und ruft die **OnStop** -Methode in die Herunterfahren-Sequenzen ausgeführt werden können, bevor die Rolle offline geschaltet wird.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Die ASP.NET Lebenszyklus Methoden für eine Webrolle

Methoden Lebenszyklus ASP.NET zusätzlich die **RoleEntryPoint** -Klasse können Sie Initialisierung und Herunterfahren Sequenzen eine Webrolle verwalten. Wenn Sie eine vorhandene ASP.NET Anwendung in Azure portieren, kann aus Kompatibilitätsgründen hilfreich sein. ASP.NET Lebenszyklus Methoden werden aus der **RoleEntryPoint** -Methoden aufgerufen. Die **Anwendung\_Start** Methode wird aufgerufen, nachdem die **RoleEntryPoint.OnStart** Methode. Die **Anwendung\_End** Methode wird aufgerufen, bevor die **RoleEntryPoint.OnStop** -Methode aufgerufen wird.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum [Erstellen einer Cloud Service-Paket](cloud-services-model-and-package.md).
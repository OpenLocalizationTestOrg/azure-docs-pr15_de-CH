<properties
   pageTitle="Standardgröße TEMP-Ordner ist zu klein für eine Rolle | Microsoft Azure"
   description="Eine Rolle Cloud-Dienst hat eine begrenzte Menge an Speicherplatz für den Ordner TEMP. Dieser Artikel enthält einige Vorschläge zum Speicherplatz zu vermeiden."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standardgröße TEMP-Ordner ist zu klein für eine Cloud Service Web-Worker-Rolle

Temporären Standardverzeichnis einer Arbeitskraft oder Web Rolle Cloud-Dienst hat eine maximale Größe von 100 MB irgendwann voll werden. Dieser Artikel beschreibt wie Sie verhindern, dass der Speicherplatz für das temporäre Verzeichnis.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Warum wird Speicherplatz Ausführen?

Die standardmäßigen Windows-Umgebungsvariablen TEMP und TMP stehen in der Anwendung ausgeführten Code. Zeigen Sie auf ein einzelnes Verzeichnis mit einer maximalen Größe von 100 MB, TEMP und TMP. Die in diesem Verzeichnis gespeicherten Daten werden über den Lebenszyklus von Cloud-Dienst nicht beibehalten; die Rolleninstanzen in einem Clouddienst wiederverwendet werden, ist das Verzeichnis gereinigt.

## <a name="suggestion-to-fix-the-problem"></a>Vorschlag zur Behebung des Problems

Implementieren Sie eine der folgenden Alternativen:

- Konfigurieren Sie einer lokalen Speicherressourcen, und greifen sie direkt anstatt TEMP oder TMP. Um eine lokale Speicherressource Code zugreifen, die in der Anwendung ausgeführt wird, wird rufen Sie die [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) -Methode auf. 

- Konfigurieren Sie einer lokalen Speicherressourcen und zeigen Sie TEMP und TMP-Verzeichnisse auf den Pfad der lokalen Speicherressourcen. Diese Änderung sollte in der [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) -Methode ausgeführt werden.

Im folgenden Codebeispiel wird veranschaulicht, wie die Zielverzeichnisse für TEMP und TMP aus innerhalb der OnStart-Methode zu ändern:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Einen Blog lesen beschreibt, die [von Azure Web Rolle ASP.NET temporären Ordner zu vergrößern](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Weitere [Artikel zur Fehlerbehebung](/?tag=top-support-issue&product=cloud-services) für Cloud-Dienste anzeigen

Weitere Informationen zum Cloud-Dienst Rolle Problemen mit Azure PaaS Computer Diagnosedaten zeigen Sie [Blogserie des Kevin Williamson an](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)

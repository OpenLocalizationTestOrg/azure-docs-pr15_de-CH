<properties
   pageTitle="Häufige Ursachen von Cloud-Dienst Rollen recycling | Microsoft Azure"
   description="Cloud-Dienstrolle, die plötzlich recycelt kann erhebliche Ausfallzeiten verursachen. Hier sind einige allgemeine Probleme, die Rollen wiederverwendet, wodurch Ausfallzeiten reduzieren helfen kann."
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
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Probleme, die Rollen Recycling verursachen

Dieser Artikel beschreibt einige häufige Ursachen von Bereitstellungsproblemen und bietet Tipps zur Problembehandlung, diese Probleme zu beheben. Angabe, dass ein Problem mit einer Anwendung ist, die Rolleninstanz nicht gestartet, oder zwischen initialisieren, beschäftigt und beenden wechselt.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Fehlende Laufzeit abhängig

Wenn eine Rolle in der Anwendung auf eine Assembly, die nicht Teil von.NET Framework oder der Azure Bibliothek verwaltet, müssen Sie diese Assembly explizit in das Anwendungspaket einschließen. Denken Sie daran nicht anderen Frameworks Microsoft Azure standardmäßig zur Verfügung stehen. Wenn Ihre Rolle ein Framework erforderlich ist, müssen Sie das Anwendungspaket Assemblys hinzufügen.

Überprüfen Sie vor dem Erstellen und Ihrer Anwendung Verpacken Folgendes:

- Bei Verwendung von Visual Studio stellen Sie sicher, dass die Eigenschaft **Lokale Kopie** auf **True** für jede referenzierte Assembly im Projekt festgelegt ist, die nicht Teil des Azure SDK oder.NET Framework.

- Stellen Sie sicher, dass die Datei web.config keine nicht verwendeten Assemblys im Compilation-Element verweist.

- Der **Buildvorgang** jede cshtml-Datei wird auf **Inhalt**festgelegt. Dadurch, dass Dateien einwandfrei im Paket und andere referenzierten Dateien im Paket angezeigt werden.

## <a name="assembly-targets-wrong-platform"></a>Falsche Assembly Ziele-Plattform

Azure ist eine 64-Bit-Umgebung. Daher funktioniert nicht für 32-Bit-Ziel kompilierten Assemblys auf Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rolle löst Ausnahmefehler beim Initialisieren oder beenden

Alle ausgelösten Ausnahmen von den Methoden der [RoleEntryPoint] -Klasse enthält die [OnStart], [OnStop]und Methoden [ausgeführt] werden, nicht behandelte Ausnahmen. In einer der folgenden Methoden eine nicht behandelte Ausnahme auftritt, wird die Rolle wiederverwenden. Wenn die Funktion mehrmals wiederverwendet, kann es eine nicht behandelte Ausnahme immer werfen versucht zu starten.

## <a name="role-returns-from-run-method"></a>Rolle gibt von Run-Methode

Die [Run] -Methode soll endlos ausgeführt. Wenn Ihr Code die [Run] -Methode überschreibt, sollte auf unbestimmte Zeit schlafen. Wenn die [Run] -Methode zurückgegeben wird, recycelt die Rolle.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Falsche Einstellung für DiagnosticsConnectionString

Wenn Anwendung Azure-Diagnose kann die Dienstkonfigurationsdatei geben die `DiagnosticsConnectionString` Einstellung. Diese Einstellung sollte eine HTTPS-Verbindung an das Speicherkonto in Azure fest.

Um sicherzustellen, dass die `DiagnosticsConnectionString` Einstellung korrekt ist, bevor das Anwendungspaket in Azure bereitstellen, überprüfen Sie Folgendes:  

- Die `DiagnosticsConnectionString` einem gültigen Konto in Azure festlegen.  
  Standardmäßig verweist dieser Einstellung Konto emulierten Speicher, sodass Sie explizit diese Einstellung ändern müssen, bevor Sie das Anwendungspaket bereitstellen. Wenn Sie diese Einstellung nicht ändern, wird eine Ausnahme ausgelöst, wenn die Rolleninstanz versucht, Diagnose Monitor starten. Dadurch kann die Rolleninstanz unbegrenzt wiederverwenden.

- Die Verbindungszeichenfolge wird im folgenden [Format](../storage/storage-configure-connection-string.md)angegeben. (Das Protokoll als HTTPS anzugeben.) Ersetzen Sie *MyAccountName* durch den Namen Speicherkonto und *MyAccountKey* zusammen mit der Zugriffstaste:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Wenn Sie die Anwendung mithilfe von Azure-Tools für Microsoft Visual Studio entwickeln, können Sie die [Eigenschaftenseiten](https://msdn.microsoft.com/library/ee405486) , um diesen Wert festzulegen.

## <a name="exported-certificate-does-not-include-private-key"></a>Exportierte Zertifikat enthält keinen privaten Schlüssel

Führen Sie eine Webrolle unter SSL müssen exportierte Zeugnisse den privaten Schlüssel enthält. Wenn Sie das Zertifikat exportieren *Windows Certificate Manager* verwenden, müssen Sie **Ja** für **den privaten Schlüssel** exportieren auswählen. Das Zertifikat muss im PFX-Format ist das einzige derzeit unterstützte Format exportiert werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere [Artikel zur Fehlerbehebung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Cloud-Dienste anzeigen

Zeigen Sie weitere Szenarien bei [der Kevin Williamson Blog](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)recycling Rolle an

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Ausführen]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx

<properties
   pageTitle="Problembehandlung bei Rollen, die nicht starten | Microsoft Azure"
   description="Hier sind einige Gründe, warum Cloud-Dienst Rollen nicht gestartet werden kann. Lösung für diese Probleme werden ebenfalls bereitgestellt."
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

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Problembehandlung bei Cloud-Dienst Rollen, die nicht gestartet werden

Hier sind einige allgemeine Probleme und Lösungen für Azure Cloud Services Rollen, die nicht gestartet werden.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Fehlende DLLs oder Abhängigkeiten

Nicht reagierende Rollen und Rollen **Initialisierung** **gebucht**und **Beenden** Zustände durchlaufen können nach fehlenden DLLs oder Assemblys verursacht werden.

Symptome von fehlenden DLLs oder Assemblys möglich:

- **Initialisierung**, **gebucht**und Status **beendet** und die Rolleninstanz ausschalten.
- Die Rolleninstanz **kann** verschoben, aber wenn Sie Ihre Web-Anwendung aufrufen, wird die Seite nicht angezeigt.

Es gibt einige empfohlene Methoden für diese Fragen.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Fehlende DLL-Probleme in einer Webrolle diagnostizieren

Wenn Sie zu einer Website navigieren, die bereitgestellt werden in einem Web Rolle und der Browser zeigt einen Serverfehler ähnlich der folgenden, möglicherweise eine DLL fehlt.

![Serverfehler in der Anwendung '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostizieren Sie durch Deaktivieren von benutzerdefinierten Fehlern Probleme

Genauere Informationen kann angezeigt werden, durch Konfiguration der Datei web.config für die Webrolle benutzerdefinierte Modus auf Off setzen und den Dienst bereitgestellt.

Vollständige Fehler anzeigen, ohne mit dem Remotedesktop:

1. Öffnen Sie die Projektmappe in Microsoft Visual Studio.

2. Im **Projektmappen-Explorer**die Datei web.config suchen und öffnen.

3. Suchen Sie in der Datei web.config im Abschnitt system.web, und fügen Sie folgende Zeile:

    ```xml
    <customErrors mode="Off" />
    ```

4. Speichern Sie die Datei.

5. Verpacken und den Dienst erneut.

Sobald der Dienst bereitgestellt wird, sehen Sie eine Fehlermeldung mit dem Namen der fehlenden Assembly oder DLL.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Diagnostizieren Sie Probleme mit Remote-Anzeige des Fehlers

Remotedesktop können zu der Rolle ausführlichere Fehlerinformationen Remote anzeigen. Gehen Sie die Fehler anzeigen mithilfe von Remotedesktop:

1. Stellen Sie sicher, dass Azure SDK 1.3 oder höher installiert ist.

2. Während der Bereitstellung der Lösung mithilfe von Visual Studio wählen Sie "Configure Remotedesktopverbindungen...". Weitere Informationen zum Konfigurieren der Remotedesktopverbindung finden Sie unter [Verwenden von Remotedesktop Azure-Rollen](../vs-azure-tools-remote-desktop-roles.md).

3. Microsoft Azure-Verwaltungsportal die Instanz Status **bereit**zeigt, klicken Sie auf eine der Instanzen.

4. Klicken Sie im Bereich **RAS** des Menübands **Verbinden** .

5. Melden Sie sich am virtuellen Computer mit den Anmeldeinformationen, die während der Remotedesktop-Konfiguration angegeben wurden.

6. Öffnen Sie ein Befehlsfenster.

7. Typ `IPconfig`.

8. Beachten Sie den Wert der IPv4-Adresse.

9. Öffnen Sie InternetExplorer.

10. Geben Sie die Adresse und den Namen der Website. Z. B. `http://<IPV4 Address>/default.aspx`.

Navigieren auf der Website wird nun genauere Fehlermeldungen zurück:

* Serverfehler in der Anwendung '/'.

* Beschreibung: Während der Ausführung der aktuellen Webanfrage ist eine nicht behandelte Ausnahme aufgetreten. Überprüfen Sie weitere Informationen über den Fehler und Ursprung im Code Stapelrahmen.

* Details der Ausnahme: System.IO.FIleNotFoundException: konnte nicht geladen werden, Datei oder Assembly "Microsoft.WindowsAzure.StorageClient, Version = 1.1.0.0, Kultur = Neutral, PublicKeyToken = 31bf856ad364e35' oder eine Abhänigkeit. Das System kann nicht die angegebene Datei nicht finden.

Zum Beispiel:

![Explizite Serverfehler in Anwendung '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnose von Problemen mit Serveremulator

Sie können Microsoft Azure-Serveremulator diagnose und Problembehandlung von Abhängigkeiten und web.config Fehler.

Mit dieser Diagnose besten verwenden Sie einen Computer oder einen virtuellen Computer mit einer Neuinstallation von Windows. Um der Azure-Umgebung zu simulieren, verwenden Sie Windows Server 2008 R2 X64.

1. Installieren Sie die eigenständige Version von [Azure SDK](https://azure.microsoft.com/downloads/).

2. Erstellen Sie das Projekt für den Cloud-Dienst auf dem Entwicklungscomputer.

3. Navigieren Sie in Windows Explorer in den Ordner Bin\debug der Cloud-Dienstprojekt.

4. Kopieren Sie die CSx Ordner- und .cscfg-Datei auf dem Computer, mit dem Sie Probleme Debuggen.

5. Öffnen Sie auf dem Computer bereinigen Azure SDK-Eingabeaufforderungsfenster als Typ `csrun.exe /devstore:start`.

6. Geben Sie an der Befehlszeile `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Wenn die Rolle gestartet wird, sehen Sie detaillierte Fehlerinformationen in Internet Explorer. Standard Windows Tools zur Problembehandlung können Sie um das Problem zu diagnostizieren.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostizieren von Problemen mit IntelliTrace

Für Arbeitskraft und Web-Rollen, die.NET Framework 4 verwenden können [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx)Sie das in [Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs)verfügbar ist.

Führen Sie diese Schritte, um den Dienst mit aktiviertem IntelliTrace bereitgestellt:

1. Bestätigen Sie, dass Azure SDK 1.3 oder höher installiert ist.

2. Die Lösung mithilfe von Visual Studio bereit. Während der Bereitstellung das Kontrollkästchen Sie **IntelliTrace für .NET 4 Rollen aktivieren** .

3. Sobald die Instanz gestartet wird, öffnen Sie **Server-Explorer**.

4. Erweitern Sie die **Azure\\Clouddienste** Knoten und die Bereitstellung.

5. Erweitern Sie die Bereitstellung, bis die Rolleninstanzen angezeigt. Klicken Sie auf eine der Instanzen.

6. Wählen Sie **View IntelliTrace Logs**. Die **IntelliTrace-Zusammenfassung** wird geöffnet.

7. Suchen Sie im Abschnitt Ausnahmen die Zusammenfassung. Wenn Ausnahmen werden im Abschnitt **Ausnahmedaten**gekennzeichnet.

8. Erweitern Sie die **Ausnahmedaten** und **System.IO.FileNotFoundException** Fehler ähnelt dem folgenden:

![Ausnahmedaten Datei oder assembly](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adresse fehlenden DLLs und Assemblys

Um fehlende DLL und Zusammenstellungsfehler, gehen Sie folgendermaßen vor:

1. Öffnen Sie die Projektmappe in Visual Studio.

2. Öffnen Sie im **Projektmappen-Explorer**den Ordner **Verweise** .

3. Klicken Sie auf die Assembly in der Fehlermeldung.

4. Suchen Sie im Bereich **Eigenschaften** **lokale Kopie** , und legen Sie den Wert auf **True**.

5. Cloud-Dienst erneut.

Nachdem Sie sichergestellt haben, dass alle Fehler korrigiert haben, können Sie den Dienst bereitstellen, ohne **IntelliTrace für .NET 4 Rollen aktivieren** das Kontrollkästchen.

## <a name="next-steps"></a>Nächste Schritte

Weitere [Artikel zur Fehlerbehebung](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) für Cloud-Dienste anzeigen

Informationen zum Cloud-Dienst Rolle Problemen mit Azure PaaS-Diagnosedaten [Kevin Williamsons blogreihe](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)angezeigt.

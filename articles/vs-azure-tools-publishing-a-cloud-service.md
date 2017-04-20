<properties
   pageTitle="Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools | Microsoft Azure"
   description="Enthält Informationen zum Azure Cloud Service-Projekte mit Visual Studio veröffentlicht."
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

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools

Mithilfe der Azure-Tools für Microsoft Visual Studio können Sie Ihre Azure-Anwendung direkt aus Visual Studio veröffentlichen. Visual Studio unterstützt integrierte veröffentlichen oder Testserver Produktionsumfeld eines Cloud-Diensts.

Vor dem Veröffentlichen einer Azure-Anwendung müssen Sie ein Azure-Abonnement. Cloud-Dienst und Speicher-Konto muss festgelegt werden, von der Anwendung verwendet werden. Sie können diese im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)einrichten.

>[AZURE.IMPORTANT] Wenn Sie veröffentlichen, können Sie die Deployment-Umgebung für den Clouddienst auswählen. Sie müssen auch ein Speicherkonto auswählen, mit der das Anwendungspaket für die Bereitstellung zu speichern. Nach der Bereitstellung ist das Speicherkonto Anwendungspaket entfernt.

Beim Entwickeln und Testen einer Anwendung Azure können Web Deploy Sie Änderungen für die Webrollen inkrementelle Publizieren. Nach dem Veröffentlichen der Anwendung ein Deployment-Umgebung ermöglicht Web Deploy Änderungen an den virtuellen Computer bereitstellen, die Web-Rolle ausgeführt wird. Sie müssen keinen Verpacken und veröffentlichen Sie die gesamte Azure Anwendung jedes Mal die Web-Rolle Testen der Änderung aktualisieren möchten. Dabei haben Sie in der Cloud testen, ohne die Anwendung veröffentlicht ein Deployment-Umgebung verfügbaren Web Rolle ändern.

Gehen Sie an Ihre Azure-Anwendung und eine Webrolle mithilfe von Web Deploy aktualisieren:

- Veröffentlichen oder eine Azure-Anwendung von Visual Studio-Paket

- Aktualisieren einer Webrolle als Teil des Entwicklungs- und Testzyklus

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Veröffentlichen oder eine Azure-Anwendung von Visual Studio-Paket

Beim Veröffentlichen der Azure-Anwendung führen Sie eine der folgenden Aufgaben:

- Ein Service-Paket zu erstellen: Sie können dieses Paket und Dienstkonfigurationsdatei an Ihre Anwendung in einer bereitstellungsumgebung von [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885).

- Veröffentlichen Sie Ihre Azure-Projekt von Visual Studio: veröffentlichen die Anwendung direkt in Azure Webpublishing-Assistenten verwenden. Informationen finden Sie unter [Assistent Azure veröffentlichen](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Ein Servicepaket von Visual Studio erstellen

1. Wenn Sie eine Anwendung veröffentlichen möchten, öffnen Sie Projektmappen-Explorer öffnen Sie des Kontextmenüs, das die Rollen enthält Azure-Projekt und wählen Sie veröffentlichen.

1. Gehen Sie folgendermaßen vor, um ein Servicepaket zu erstellen:  

  1. **Wählen Sie im Kontextmenü für den Azure-Projekt.**

  1. Klicken Sie im Dialogfeld **Paket Azure-Anwendung** wählen Sie die Dienstkonfiguration ein Paket erstellt werden soll aus und wählen Sie dann die Buildkonfiguration.

  1. (optional) Aktivieren von Remotedesktop für den Clouddienst nach der Veröffentlichung, **Remotedesktop für alle Rollen aktivieren** das Kontrollkästchen und wählen Sie die **Einstellung** Remotedesktop konfigurieren. Cloud-Dienst debuggen, nachdem Sie diese veröffentlichen, aktivieren Sie die Remotedebuggen **Aktivieren Remotedebugger für alle Rollen**auswählen.

      Weitere Informationen finden Sie unter [Verwenden von Remotedesktop Azure-Rollen](vs-azure-tools-remote-desktop-roles.md).

  1. Wählen Sie zum Erstellen des Pakets den **Paket** -Link.

      Datei-Explorer zeigt den Speicherort des neu erstellten Pakets. Sie können diesen Speicherort, damit Sie aus dem [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)verwenden können.

  1. Um dieses Paket zu einer Deployment-Umgebung veröffentlichen, verwenden Sie diesen Speicherort als Speicherort des Pakets einen Clouddienst erstellen und dieses Paket in eine Umgebung mit dem [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optional) Zum Abbrechen des Bereitstellungsprozess im Kontextmenü für den Positionsartikel im Aktivitätsprotokoll, wählen Sie **Abbrechen und entfernen**. Dies beendet den Bereitstellungsprozess und Deployment-Umgebung auf Azure gelöscht.

    >[AZURE.NOTE] Zum Deployment-Umgebung entfernen, nachdem sie bereitgestellt wurde, verwenden Sie das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optional) Nachdem die Rolleninstanzen gestartet haben, zeigt Visual Studio Deployment-Umgebung automatisch im **Cloud-Services** -Knoten im Server-Explorer. Hier sehen Sie den Status der einzelnen Instanzen. [Verwalten von Azure Ressourcen mit Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md)angezeigt. Die folgende Abbildung zeigt die Rolleninstanzen zwar noch im initialisieren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Aktualisieren einer Webrolle als Teil des Entwicklungs- und Testzyklus

Wenn Ihre app Backend-Infrastruktur ist jedoch die Webrollen müssen häufiger aktualisieren können Web Deploy Sie eine Web-Rolle im Projekt aktualisieren. Dies ist hilfreich, wenn Sie nicht neu erstellen und erneut bereitstellen Backend-Worker-Rollen oder wenn Sie mehrere Webrollen und nur die Webrollen aktualisieren möchten.

### <a name="requirements"></a>Vorschriften

Hier sind die Anforderungen mit Web Deploy Web-Rolle aktualisiert:

- **Für Entwicklung und testing nur zu Informationszwecken:** Die Änderungen werden direkt auf dem virtuellen Computer, auf dem die Web-Rolle ausgeführt. Hat dieses virtuellen Computers wiederverwendet werden, gehen die Änderungen verloren, da das ursprüngliche Paket, die veröffentlicht wird der virtuelle Computer für die Rolle neu. Sie müssen die Anwendung der letzten Änderung für die Web-Rolle zu veröffentlichen.

- **Können nur Webrollen aktualisiert:** Worker-Rollen können nicht aktualisiert werden. Darüber hinaus können Sie RoleEntryPoint im Web role.cs nicht aktualisieren.

- **Unterstützt nur eine einzelne Instanz einer Webrolle:** Sie mehrere Instanzen des Web-Rolle in Ihrer Deployment-Umgebung nicht möglich. Allerdings werden mehrere Webrollen jeder nur eine Instanz.

- **Müssen Sie Remotedesktopverbindungen aktivieren:** Dies ist erforderlich, damit Web Deploy verwenden können Benutzer und Kennwort eine Verbindung zu den virtuellen Computer die Änderungen auf dem Server bereitstellen, auf denen Internet Information Services (IIS) ausgeführt wird. Darüber hinaus müssen Sie den virtuellen Computer hinzufügen ein vertrauenswürdigen Zertifikats zu IIS auf diesem virtuellen Computer herstellen. (Dadurch wird sichergestellt, dass die Verbindung von Web Deploy verwendet IIS sicher ist.)

Vorausgesetzt, dass Sie den **Azure-Anwendung veröffentlichen** -Assistenten verwenden.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Zum Aktivieren Web Deploy beim Veröffentlichen der Anwendung

1. Um den **Web Deploy aktivieren** von Rollen Kontrollkästchen zu aktivieren, müssen Sie zunächst Remotedesktopverbindungen konfigurieren. Wählen Sie **Remotedesktop aktivieren** für alle Rollen aus und dann die Anmeldeinformationen, die mit der Verbindung Remote im **Remote Desktop-Konfiguration** , die angezeigt wird. Weitere Informationen finden Sie unter [Verwenden von Remotedesktop Azure-Rollen](vs-azure-tools-remote-desktop-roles.md) .

1. Wählen Sie Web Deploy aktivieren für alle Web-Rollen in der Anwendung **Aktivieren Web Deploy für alle Webrollen**.

    Ein gelbes Warndreieck angezeigt wird. Web Deploy verwendet eine nicht vertrauenswürdige, selbstsignierte Zertifikat standardmäßig für das Hochladen von Daten nicht empfohlen. Wenn Sie diesen Prozess für vertrauliche Daten sichern müssen, können Sie ein SSL Zertifikat für Web Deploy Verbindungen hinzufügen. Dieses Zertifikat muss einem vertrauenswürdigen Zertifikat. Informationen hierzu finden Sie im Abschnitt **Stellen Web bereitstellen sichern** weiter unten in diesem Thema.

1. Wählen Sie **Weiter** die **Zusammenfassung** angezeigt und dann **Veröffentlichen** Cloud-Dienst bereitgestellt.

    Cloud-Dienst veröffentlicht. Der virtuelle Computer, die erstellt wird hat Remoteverbindungen für IIS aktiviert, sodass Web Deploy verwendet werden können, die Webrollen aktualisieren, ohne erneut veröffentlichen.

    >[AZURE.NOTE] Wenn Sie mehr als eine Instanz für eine Webrolle konfiguriert haben, wird eine Warnung angezeigt, dass jede Webrolle eine Instanz im Paket beschränkt werden, die erstellt wird, um die Anwendung zu veröffentlichen. Wählen Sie **OK** , um fortzufahren. Wie im Abschnitt Anforderungen, haben Sie mehr als eine Webrolle aber nur eine Instanz jeder Rolle.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Aktualisieren die Webrolle mit Web Deploy

1. Zum Verwenden von Web Deploy ändern Sie Code zum Projekt für Ihre Web-Rollen in Visual Studio zu veröffentlichen, diesen Projektknoten in der Projektmappe Maustaste und zeigen Sie auf **Veröffentlichen**. Das Dialogfeld " **Web veröffentlichen** " wird angezeigt.

1. (Optional) Wenn Sie vertrauenswürdiges SSL-Zertifikat für Remoteverbindungen für IIS verwendet haben, kann das Kontrollkästchen **nicht vertrauenswürdiges Zertifikat zulassen** deaktivieren. Informationen zum Hinzufügen eines Zertifikats zur Web Deploy Sicherheit finden Sie im Abschnitt **Zu stellen Web Bereitstellen sicherer** weiter unten in diesem Thema.

1. Um Web Deploy verwenden, benötigt das Veröffentlichen der Benutzername und das Kennwort, das Sie für die Remotedesktopverbindung einrichten, wenn Sie zuerst das Paket veröffentlicht.

  1. Geben Sie im Feld **Benutzername**den Benutzernamen.

  1. **Kennwort**Geben Sie das Kennwort.

  1. (Optional) Wenn Sie dieses Kennwort in diesem Profil speichern möchten, wählen Sie **Kennwort speichern**.

1. Wählen Sie die Änderungen für Ihre Webrolle veröffentlichen **Veröffentlichen**.

    Die Statuszeile zeigt **Veröffentlichen gestartet**. Wenn die Veröffentlichung abgeschlossen ist, erscheint **Veröffentlichen erfolgreich** . Die Änderungen haben nun auf die Web-Rolle auf dem virtuellen Computer bereitgestellt. Jetzt können Sie Ihre Azure-Anwendung in Azure-Umgebung zum Testen ändern starten.

### <a name="to-make-web-deploy-secure"></a>Um Web Bereitstellen sicherer

1. Web Deploy verwendet eine nicht vertrauenswürdige, selbstsignierte Zertifikat standardmäßig für das Hochladen von Daten nicht empfohlen. Wenn Sie diesen Prozess für vertrauliche Daten sichern müssen, können Sie ein SSL Zertifikat für Web Deploy Verbindungen hinzufügen. Dieses Zertifikat muss ein vertrauenswürdiges Zertifikat, das von einer Zertifizierungsstelle (CA) erhalten.

    Um Web Deploy für jeden virtuellen Computer für jede Web-Rollen sicher, müssen Sie das vertrauenswürdige Zertifikat für Web möchten Hochladen der [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)bereitstellen. Dadurch wird sichergestellt, dass das Zertifikat mit dem virtuellen Computer hinzugefügt, die beim Veröffentlichen der Anwendung für die Web-Rolle erstellt.

1. Gehen Sie folgendermaßen vor, um IIS mit Remoteverbindungen vertrauenswürdiges SSL-Zertifikat hinzuzufügen:

  1. Anschließen der virtuellen Maschine, die Web-Rolle ausgeführt wird, wählen Sie die Instanz der Webrolle **Cloud Explorer** oder **Server-Explorer**und wählen Sie dann den Befehl **Verbinden mit Remotedesktop** . Weitere Informationen zum Herstellen einer Verbindung zu dem virtuellen Computer [Mithilfe von Remotedesktop Azure-Rollen](vs-azure-tools-remote-desktop-roles.md)anzeigen

      Browser fordert Sie zum Herunterladen ein. RDP-Datei.

  1. Fügen Sie ein SSL-Zertifikat öffnen Sie den Verwaltungsdienst im IIS-Manager. Aktivieren Sie SSL in IIS-Manager im **Aktionsbereich** **Bindings** Link öffnen. Das Dialogfeld **Website Bindung hinzufügen** wird angezeigt. Wählen Sie **Hinzufügen**und dann HTTPS in **der Dropdown-Liste** . Wählen Sie in der Liste **SSL-Zertifikat** das SSL-Zertifikat von einer Zertifizierungsstelle signiert wurde, die dem [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochgeladen. Weitere Informationen finden Sie unter [Konfigurieren der Verbindungszeichenfolge für den Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=215824).

      >[AZURE.NOTE] Wenn Sie ein vertrauenswürdiges SSL-Zertifikat gelbem Warndreieck im **Webpublishing-Assistenten**nicht mehr angezeigt hinzufügen.

## <a name="include-files-in-the-service-package"></a>Dateien in das Service-Paket einschließen

Sie müssen bestimmte Dateien in das Servicepaket enthalten, damit sie auf dem virtuellen Computer verfügbar sind, die für eine Rolle erstellt. Sie möchten z. B. eine .exe oder eine MSI-Datei, mit der ein Startskript zu Ihrem Servicepaket hinzufügen. Oder Sie müssen eine Assembly hinzufügen, eine Rolle oder Worker-Rolle Webprojekt. Dateien müssen sie die Lösung für Ihre Azure-Anwendung hinzugefügt werden.

### <a name="to-include-files-in-the-service-package"></a>Hinzufügen von Dateien in das Service-Paket

1. Gehen Sie folgendermaßen vor, um ein Service-Paket eine Assembly hinzufügen:

  1. Öffnen Sie im **Projektmappen-Explorer** den Projektknoten für das Projekt, das die referenzierte Assembly fehlt.

  1. Um die Assembly dem Projekt hinzuzufügen, öffnen Sie das Kontextmenü für Ordner **Verweise** und dann **Hinzufügen**. Das Dialogfeld Verweis hinzufügen wird angezeigt.

  1. Wählen Sie die Referenz zu hinzufügen, und klicken Sie dann auf **OK** .

      Der Verweis wird zu der Liste unter dem Ordner **Verweise** hinzugefügt.

  1. Öffnen Sie das Kontextmenü für die Assembly, die hinzugefügt, und wählen Sie **Eigenschaften**. **Das Fenster** wird angezeigt.

      Diese Assembly im Paket Dienst in der **Liste der lokalen Kopie** auswählen auf **True**.

1. Öffnen Sie im **Projektmappen-Explorer** den Projektknoten für das Projekt, das die referenzierte Assembly fehlt.

1. Um die Assembly dem Projekt hinzuzufügen, öffnen Sie das Kontextmenü für Ordner **Verweise** und dann **Hinzufügen**. Das Dialogfeld **Verweis hinzufügen** wird angezeigt.

1. Wählen Sie die Referenz zu hinzufügen, und klicken Sie dann auf **OK** .

    Der Verweis wird zu der Liste unter dem Ordner **Verweise** hinzugefügt.

1. Öffnen Sie das Kontextmenü für die Assembly, die hinzugefügt, und wählen Sie **Eigenschaften**. Das Fenster wird angezeigt.

1. Wählen Sie auf dieser Assembly im Paket Dienst in der Liste **Lokale Kopie** **True**.

1. Auf Dateien im Leistungspaket Rolle Webprojekt hinzugefügt wurden, öffnen Sie das Kontextmenü für die Datei und wählen Sie dann **Eigenschaften**. Wählen Sie im **Eigenschaftenfenster** **Content** im Listenfeld **Buildvorgang** .

1. Auf Dateien im Leistungspaket Worker-Rolle Projekt hinzugefügt wurden, öffnen Sie das Kontextmenü für die Datei, und wählen Sie **Eigenschaften**. Wählen Sie im Fenster **Eigenschaften** im Listenfeld **In Ausgabeverzeichnis kopieren** **Kopieren, wenn neuer** .

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Veröffentlichen in Azure von Visual Studio finden Sie unter [Assistent Azure veröffentlichen](vs-azure-tools-publish-azure-application-wizard.md).

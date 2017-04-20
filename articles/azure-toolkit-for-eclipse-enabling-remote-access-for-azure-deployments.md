<properties
    pageTitle="Aktivieren des Remotezugriffs für Azure Installationen in Eclipse"
    description="Informationen Sie zum Azure Bereitstellungen mit dem Azure-Toolkit für Eclipse zu aktivieren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Aktivieren des Remotezugriffs für Azure Installationen in Eclipse

Um die Bereitstellung zu beheben, kann aktivieren und RAS Verbindung zu virtuellen Computers der Bereitstellung verwenden. RAS beruht auf Remote Desktop Protocol (RDP). Sie können RAS für die Bereitstellung konfigurieren, nachdem es in Azure veröffentlicht haben oder wenn Sie Eclipse mit einem Windows-Betriebssystem verwenden, RAS konfigurieren können, bevor Sie Azure veröffentlichen. Beachten Sie, dass ein Remotedesktop-Client, die mit dem Betriebssystem kompatibel ist benötigt werden, um die Bereitstellung virtueller Computer in Azure zu verbinden.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>RAS aktivieren, bevor Sie in Azure bereitstellen

> [AZURE.NOTE] Um vor dem Bereitstellen der Anwendung in Azure aktivieren, müssen Sie Eclipse unter Windows ausgeführt werden.

Die folgende Abbildung zeigt die **RAS** -Eigenschaften Dialogfeld verwendet, um zu aktivieren.

![][ic719494]

Gibt es zwei Arten der **RAS** -Eigenschaften Dialogfeld:

* Klicken Sie auf **Erweitert** , **RAS** Abschnitt des Dialogfelds **in Azure veröffentlichen** .
* Öffnen Sie **das Eigenschaftendialogfeld des Azure-Projekts** .

Beim Erstellen eines neuen Azure-Bereitstellung haben das Projekt nicht Remote Access standardmäßig aktiviert. Allerdings können Sie problemlos RAS durch Angabe von Benutzername und Kennwort im Dialogfeld **Azure veröffentlichen** aktivieren. RAS-Kennwort wird verschlüsselt mit x. 509-Zertifikaten. Verwenden Sie nicht Ihr eigenes Zertifikat bieten die Verschlüsselung ein selbst signiertes Zertifikat der Azure-Plugin für Eclipse Lieferumfang abhängig. Dieses selbstsignierte Zertifikat befindet sich im Ordner **Cert** Azure Projekt sowohl als einer öffentlichen Zertifikatsdatei (SampleRemoteAccessPublic.cer) eine Zertifikatsdatei persönliche Informationsaustausches (PFX) (SampleRemoteAccessPrivate.pfx) gespeichert. Diese enthält den privaten Schlüssel des Zertifikats und hat ein Standardkennwort **Kennwort1**. Da dieses Kennwort öffentlich ist, sollte die Standardzertifikat verwendet nur zu Lernzwecken nicht für eine Bereitstellung in der Produktion wird. Also nicht zu, klickt um Remotesitzungen für die Bereitstellung aktiviert Sie Link **Erweitert** im Dialogfeld **Azure veröffentlichen** an Ihr eigenes Zertifikat. Beachten Sie, dass PFX-Version des Zertifikats mit dem gehosteten Dienst in Azure-Verwaltungsportal hochladen, damit, Azure das Kennwort entschlüsseln kann müssen.

Der Rest des Lernprogramms veranschaulicht die Azure Bereitstellungsprojekts zu aktivieren, die mit dem Remotezugriff deaktiviert ursprünglich erstellt wurde. Für dieses Lernprogramm erstellen wir ein neues selbstsigniertes Zertifikat und haben die PFX-Datei ein Kennwort Ihrer Wahl. Sie können auch mit einem Zertifikat von einer Zertifizierungsstelle ausgestellt.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>RAS aktivieren, nachdem Sie in Azure bereitgestellt haben

Wenn Sie Remotezugriff aktivieren, nachdem Sie in Azure bereitgestellt haben, gehen Sie folgendermaßen vor:

1. Azure-Verwaltungsportal mit Ihrem Azure-Konto anmelden
1. Wählen Sie in der **Cloud-Diensten**bereitgestellten Clouddienst
1. Klicken Sie in der Cloud Service-Webseite auf den Link **Konfigurieren**
1. Klicken Sie am Ende der Konfigurationsseite auf **Remote** link
1. Wenn Popup-Dialogfeld wird angezeigt:
    * Geben Sie die Rolle für die sollen RAS können
    * Aktivieren Sie das Kontrollkästchen **Remotedesktop aktivieren**
    * Geben Sie einen Benutzernamen und ein Kennwort für den Remotezugriff verwenden möchten
    * Wählen Sie das zu verwendende Zertifikat
1. Klicken Sie auf **OK** 

Sie sehen eine Meldung, dass die Konfiguration ändern, wird das wenige Minuten dauern kann. Nach Abschluss der Änderung der Konfiguration führen Sie die Schritte in diesem Artikel im Abschnitt **Remote anmelden** .
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Remote-Zugriff in Ihrem Paket aktivieren

1. Eclipse Projekt-Explorer-Bereich Azure-Projekts und **Eigenschaften**.

1. Im Dialogfeld **Eigenschaften** erweitern Sie **Azure** im linken Fensterbereich zu, und klicken Sie auf **RAS**.

1. Klicken Sie im Dialogfeld **RAS** sicherstellen Sie, dass **alle Rollen Remotedesktopverbindungen mit diesen Anmeldeinformationen akzeptieren aktivieren** aktiviert ist.

1. Geben Sie einen Benutzernamen für die Remotedesktopverbindung.

1. Geben Sie an und bestätigen Sie das Kennwort für den Benutzer. Den Benutzer Namen und das Kennwort Werte in diesem Dialogfeld werden verwendet, wenn eine Remotedesktop-Verbindung herzustellen. (Beachten Sie, dass dies kein separates Kennwort PFX-Kennwort ein.)

1. Geben Sie das Ablaufdatum für das Benutzerkonto.

1. Klicken Sie auf **neu** , um ein neues selbstsigniertes Zertifikat erstellen. (Alternativ ein Zertifikat kann aus Ihrem Arbeitsbereich bzw. das Dateisystem über die Schaltflächen **Arbeitsbereich** oder **Dateisystem** bzw. wählen jedoch für dieses Lernprogramm erstellen wir ein neues Zertifikat.)

    * Klicken Sie im Dialogfeld **Neues Zertifikat** Geben Sie an und bestätigen Sie das Kennwort für die PFX-Datei verwende.

    * Den Wert für **Name (CN)**übernehmen oder einen benutzerdefinierten Namen.

    * Geben Sie den Speicherort des neuen Zertifikats in Form von CER, Pfad und Dateiname. Dieser und den nächsten Schritt **Cert** -Ordner des Projekts Azure können jedoch einen anderen Speicherort auswählen können. Im Rahmen dieses Lernprogramms verwenden wir **c:\mycert\mycert.cer**. (Erstellen Sie **c:\mycert** Ordner vor oder verwenden Sie einen vorhandenen Ordner, falls gewünscht.)

    * Geben Sie den Pfad und Namen das neue Zertifikat und der private Schlüssel im PFX-Formular Speicherort. Im Rahmen dieses Lernprogramms verwenden wir **c:\mycert\mycert.pfx**. **Neues Zertifikat** Dialog sollte folgendermaßen aussehen (die Ordnerpfade aktualisiert, wenn Sie nicht **c:\mycert**) Folgendes:

        ![][ic712275]

    * Klicken Sie auf **OK** , um das Dialogfeld **Neues Zertifikat** zu schließen.

1. **RAS** Dialog sollte etwa wie folgt aussehen:</p>

    ![][ic719495]

1. Klicken Sie auf **OK** , um das Dialogfeld **RAS** zu schließen.
    
Erneutes Erstellen der Anwendung mit der Bereitstellung cloud festgelegt.

## <a name="to-log-in-remotely"></a>Remote anmelden

Wenn die Rolleninstanz bereit ist, können Sie Remote auf dem virtuellen Computer anmelden, die die Anwendung hostet.

* Wenn Eclipse auf Windows und die Option **Start Remotedesktop auf Bereitstellung** während der Bereitstellung in Azure, Sie eine Remotedesktopverbindung Anmeldebildschirm angezeigt, wenn die Bereitstellung beginnt. Wenn Sie Benutzernamen und Kennwort aufgefordert werden, geben Sie die Werte für den entfernten Benutzer angegeben und anmelden können.

* Eine weitere Möglichkeit Remote Anmelden ist der <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure-Verwaltungsportal</a>:

    * Klicken Sie in der Ansicht **Clouddienste** von Azure-Verwaltungsportal auf die Cloud-Dienst, klicken Sie auf **Instanzen**klicken Sie auf eine bestimmte Instanz und klicken Sie dann auf die Schaltfläche **Verbinden** . Die Schaltfläche **Verbinden** wird wie folgt in die Befehlszeile:

        ![][ic659273]

    * Nach dem Klicken auf die Schaltfläche **Verbinden** , werden Sie aufgefordert, eine RDP-Datei öffnen. Öffnen Sie die Datei, und befolgen. (Sie können auch diese Datei auf Ihrem lokalen Computer speichern und führen Sie die Datei per Doppelklick remote anmelden mit Ihrer virtuellen Maschine ohne zuerst Verwaltungsportal wechseln.)

    * Wenn Sie Benutzernamen und Kennwort aufgefordert werden, geben Sie die Werte für den entfernten Benutzer angegeben und anmelden können.

> [AZURE.NOTE] Sind auf einem Windows-Betriebssystem müssen Sie einen Remote Desktop Client, der mit dem Betriebssystem kompatibel ist und die Schritte zum Konfigurieren dieser Clients mit den Einstellungen der RDP-Datei herunterladen.

## <a name="see-also"></a>Siehe auch

[Azure-Toolkit für Eclipse][]

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

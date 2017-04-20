<properties
   pageTitle="Jede Windows-Anwendung auf jedem Gerät mit Azure RemoteApp ausführen | Microsoft Azure"
   description="Informationen Sie zum Windows-Anwendung mit Azure RemoteApp für Benutzer freigeben."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Jede Windows-Anwendung auf jedem Gerät mit Azure RemoteApp ausführen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Sie können an einer beliebigen Stelle eine Windows-Anwendung auf jedem Gerät,-Ausführen mit Azure RemoteApp. Ob es vor zehn Jahren eine benutzerdefinierte Anwendung oder eine Office-Anwendung ist, müssen Benutzer nicht mehr an ein bestimmtes Betriebssystem (z. B. Windows XP) für einige Anwendungsbereiche.

Azure RemoteApp Benutzer eigene Android oder Apple Geräte auch die gleiche Erfahrung unter Windows (oder auf Windows Phone) haben. Dies geschieht mit der Windows-Anwendung in einer virtuellen Windows-Maschinen auf Azure - Benutzer können darauf zugreifen überall eine Internet-Verbindung haben. 

Ein Beispiel genau dazu lesen.

In diesem Artikel werden wir alle unsere Benutzer zugreifen. Sie können jedoch jede Anwendung. Wie Sie Ihre Anwendung auf einem Computer mit Windows Server 2012 R2 installieren können, können Sie sie mit folgenden Schritten freigeben. Überprüfen Sie [app Vorschriften](remoteapp-appreqs.md) sicherstellen, dass Ihre Anwendung funktionieren.

Hinweis: Da Access eine Datenbank ist, und die Datenbank nützlich, wir einige zusätzliche Schritte, um Benutzern den Zugriff freigeben Data Access tun werden. Wenn Ihre Anwendung keine ist nicht benötigen die Benutzer eine Dateifreigabe zugreifen können, können Sie die Schritte in diesem Lernprogramm überspringen.

> [AZURE.NOTE] <a name="note"></a>Ein Azure-Konto zum Bearbeiten dieses Lernprogramms benötigen Sie:
> - [Ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)können: Sie erhalten ein Guthaben können Sie ausprobieren, bezahlte Azure Services und sogar nachdem sie verwendet bis hält das Konto verwenden kostenlosen Azure Services wie Websites. Kreditkarte wird nicht belastet, sofern explizit ändern und Fragen belastet.
> - Sie können [MSDN-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement erhalten Sie Credits monatlich für bezahlte Azure Services verwendet werden können.


## <a name="create-a-collection-in-remoteapp"></a>Erstellen Sie eine Auflistung in RemoteApp

Zunächst erstellen eine Auflistung. Die Sammlung dient als Container für Ihre apps und Benutzer. Jede Auflistung ist in einem Bild - eigene oder eine in Ihrem Abonnement inbegriffen. Für dieses Lernprogramm verwenden wir das Test Office 2013-Bild - enthält die Anwendung, die wir gemeinsam nutzen möchten.

1. Azure-Portal einen Bildlauf nach unten in der Struktur links Nav bis RemoteApp angezeigt. Öffnen Sie die Seite.
2. Klicken Sie auf **Erstellen einer RemoteApp-Sammlung**.
3. Klicken Sie auf **Quick erstellen** und geben Sie einen Namen für die Auflistung.
4. Wählen Sie den Bereich Ihrer Sammlung erstellen möchten. Wählen Sie für optimale Ergebnisse der Region an geografisch am nächsten greifen die Benutzer auf die Anwendung. Beispielsweise werden in diesem Lernprogramm der Benutzer in Redmond, Washington befinden. Nächste ist Azure **Westen der USA**.
5. Wählen Sie die Abrechnung verwenden möchten. Grundlegende Fakturierungsplan stellt 16 Benutzer auf einer großen Azure VM und standard Fakturierungsplan 10 Benutzer auf einer großen Azure VM. Als allgemeines Beispiel funktioniert die grundlegende ideal für Daten-Eintragstyp Workflow. Für eine Anwendung Produktivität wie Office empfiehlt standard Plan.
6. Wählen Sie schließlich das Office 2013 Professional Bild. Dieses Abbild enthält Office 2013-apps. Nur zur Erinnerung – dieses Bild ist nur für Testversion Sammlungen und Machbarkeitsstudien. Sie "können dieses Bild in einer Auflistung Produktion verwenden.
7. Klicken Sie auf jetzt **Erstellen RemoteApp-Sammlung**.

![Erstellen Sie eine Cloud-Sammlung in RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Erstellen einer Sammlung wird gestartet, aber kann bis zu einer Stunde dauern.

Sie können nun Benutzer hinzufügen.

## <a name="share-the-app-with-users"></a>Die Anwendung für Benutzer freigeben

Sobald Ihre Auflistung erfolgreich erstellt wurde, ist es Zeit, Zugriff für Benutzer veröffentlichen und fügen die Benutzer zugreifen sollen.

Wenn Sie beim Erstellen die Auflistung von Knoten Azure RemoteApp navigiert haben, starten Sie sich wieder in Azure Homepage.

2. Klicken Sie auf die Auflistung um zusätzliche Optionen und konfigurieren Sie die Gruppe erstellen.
![Eine neue Cloud-Sammlung von RemoteApp](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Auf der Registerkarte **Veröffentlichen** **Veröffentlichen** am unteren Bildschirmrand auf und klicken Sie dann auf **Veröffentlichen im menüprogramme**.
![Veröffentlichen eines RemoteApp-Programms](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Wählen Sie apps aus der Liste veröffentlichen möchten. Zu diesem Zweck haben wir Zugriff. Klicken Sie auf **abgeschlossen**. Warten Sie apps Veröffentlichung abgeschlossen.
![Veröffentlichen auf RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Abschluss die Anwendung veröffentlichen, Head über zur Registerkarte **Zugriff** allen Benutzern hinzufügen, die Zugriff auf Ihre apps. Geben Sie Benutzernamen (e-Mail-Adresse) für die Benutzer, und klicken Sie auf **Speichern**.

![RemoteApp Benutzer hinzufügen](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Jetzt ist es Zeit, Ihre Benutzer über die neuen apps und darauf zugreifen. Zu diesem Zweck e-Mail Benutzer auf Remote Desktop Client-Download-URL.
![Der Client download-URL für RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Konfigurieren des Zugriffs auf

Einige apps benötigen zusätzlichen Konfiguration nach der Bereitstellung über RemoteApp. Insbesondere werden Zugriff wir eine Dateifreigabe auf Azure erstellen, die alle Benutzer zugreifen können. (Wenn Sie dies tun möchten, können eine [Hybrid-Sammlung](remoteapp-create-hybrid-deployment.md) [statt unsere Cloud-Sammlung], die der Benutzer Zugriff auf Dateien und Informationen in Ihrem lokalen Netzwerk.) Dann müssen wir unsere Benutzer Azure Dateisystem ein lokales Laufwerk auf ihrem Computer zuordnen.

Der erste Teil, den Sie als Administrator. Dann haben wir einige Schritte für Benutzer.

1. Starten der Befehlszeilenschnittstelle (cmd.exe) veröffentlichen. In der Registerkarte **Veröffentlichen** **Cmd**auswählen und dann auf **Veröffentlichen > Veröffentlichen Programm Pfad**.
2. Geben Sie den Namen der Anwendung und den Pfad. Verwenden Sie zu diesem Zweck "Datei-Explorer" als Name und "% SYSTEMDRIVE%\windows\explorer.exe" als Pfad.
![Veröffentlichen Sie die Datei cmd.exe.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Sie müssen nun ein Azure- [Konto](../storage/storage-create-storage-account.md)erstellen. Bezeichnung unsere "Accessstorage" Wählen Sie einen sinnvollen Namen. (Um Highlander zitieren, es kann nur eine "Accessstorage") ![Unsere Azure Storage-Konto](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Kehren Sie zurück zu Ihrem Dashboard so können Sie den Pfad zu Ihrem Speicherort (Endpunkt). Sie hiermit ein, sich an einer Stelle kopieren.
![Der Speicherpfad Konto](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Als Nächstes erstellte das Speicherkonto benötigen Sie die primäre Taste. Klicken Sie auf **Manage Zugriffstasten**, und kopieren Sie die primäre Taste.
6. Nun den Kontext des Speicherkontos und erstellt eine neue Dateifreigabe auf. Führen Sie die folgenden Cmdlets in einem erweiterten Windows PowerShell-Fenster:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Damit unsere Aktie Cmdlets wir ausführen sind:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Jetzt ist der Benutzer aktivieren. Zuerst müssen Sie die Benutzer ein [RemoteApp-Client](remoteapp-clients.md)installieren. Als Nächstes müssen die Benutzer ein Laufwerk von ihrem Konto zu dieser Azure Freigabe erstellten und Access-Dateien hinzufügen. Dies ist, wie sie dies tun:

1. Im RemoteApp-Client Zugriff auf veröffentlichte apps. Starten Sie das Programm cmd.exe.
2. Führen Sie den folgenden Befehl auf einem Netzlaufwerk von Ihrem Computer auf die Dateifreigabe:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Sie **/ persistent** Parameter auf yes setzen, wird das zugeordnete Laufwerk sitzungsübergreifend beibehalten.
1. Starten Sie die Datei-Explorer app RemoteApp jetzt. Kopieren Sie alle Access-Dateien in der freigegebenen Anwendung zur Dateifreigabe verwenden möchten.
![Die Access-Dateien in Azure freigeben](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Schließlich öffnen Sie, und öffnen Sie die Datenbank freigegeben werden. Die Daten in Access von der Cloud sollte angezeigt werden.
![Eine echte Access-Datenbank von der cloud](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Jetzt Zugriff auf eines Ihrer Geräte - nur sicherstellen Sie, dass einen RemoteApp-Client installieren.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Nun, Sie haben eine Auflistung erstellen, versuchen Sie, eine [Auflistung mit Office 365](remoteapp-tutorial-o365anywhere.md)erstellen. Oder Sie erstellen eine [Hybrid-Sammlung ](remoteapp-create-hybrid-deployment.md), die das lokale Netzwerk zugreifen können.

<!--Image references-->
 

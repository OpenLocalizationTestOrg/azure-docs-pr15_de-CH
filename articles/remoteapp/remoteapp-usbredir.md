<properties 
    pageTitle="Wie umleiten Sie USB-Geräte in Azure RemoteApp? | Microsoft Azure" 
    description="Erfahren Sie mehr über das Vorgehen für USB-Geräte in Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Wie umleiten Sie USB-Geräte in Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Umleitung ermöglicht Benutzern die USB-Geräte an ihren Computer oder Tablet Apps in Azure RemoteApp verwenden. Beispielsweise wenn Sie Skype über Azure RemoteApp freigegeben, müssen Benutzer ihre Geräte Kameras verwenden können.

Bevor Sie fortfahren, sicherzustellen Sie, dass der USB-Umleitung Informationen [mit](remoteapp-redirection.md)Umleitung in Azure RemoteApp. Die empfohlene nusbdevicestoredirect: * funktioniert bei USB-Webcams und USB-Drucker oder multifunktionalen USB-Geräte nicht funktionieren. Standardmäßig und aus Sicherheitsgründen muss der Administrator Azure RemoteApp Umleitung Geräteklassen-GUID oder Geräteinstanz-ID aktivieren, damit Benutzer diese Geräte verwenden können.

Obwohl dieser Artikel Web Kamera Umleitung spricht, können einen ähnlichen Ansatz Umleiten von USB-Druckern und andere multifunktionale USB-Geräte, die nicht durch umgeleitet werden die **nusbdevicestoredirect:*** Befehl.

## <a name="redirection-options-for-usb-devices"></a>Umleitungsoptionen für USB-Geräte
Azure RemoteApp verwendet sehr ähnliche Mechanismen für USB-Geräte wie die Remotedesktopdienste umleiten. Die zugrunde liegende Technologie können Sie die korrekte Umleitung für ein Gerät, um das Beste beider allgemeine und RemoteFX USB Gerät wählen Umleitung mit den **Usbdevicestoredirect:s:** Befehl. Es gibt vier Elemente diesen Befehl:

| Verarbeitungsreihenfolge | Parameter           | Beschreibung                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Alle Geräte, die vom allgemeinen Umleitung abgeholt werden nicht markiert. Hinweis: Standardmäßig * funktioniert nicht für USB-Webcams.  |
|                  | {Geräteklassen-GUID} | Wählt alle Geräte, die die angegebenen Gerätesetupklasse entsprechen.                                                           |
|                  | USB\InstanceID      | Wählt ein USB-Gerät für die angegebene Instanz-ID angegeben                                                                  |
| 2                | -USB\Instance ID    | Die Umleitung Einstellungen für das angegebene Gerät entfernt.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Umleiten eines USB-Geräts mit der Geräteklassen-GUID
Gibt es zwei Arten der Geräteklassen-GUID finden, die für die Umleitung verwendet werden können. 

Die erste Option ist die Verwendung [Systemdefinierte Gerät Setup Klassen Debitoren zur Verfügung](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Wählen Sie die Klasse, die auf den lokalen Computer angeschlossenes Gerät entspricht. Für Digitalkameras könnte dies ein Bildverarbeitungsgerät Klasse oder Video Capture Klasse. Sie müssen dazu einige Experimente mit Geräteklassen, die richtige Klasse GUID finden mit lokal angeschlossenen USB-Gerät (in unserem Fall die Webcam).

Ein besseres oder die zweite Option ist folgendermaßen zu bestimmten Geräteklassen-GUID:

1. Öffnen Sie den Geräte-Manager, suchen Sie das Gerät, das umgeleitet, und klicken Sie darauf und öffnen Sie die Eigenschaften.
![Öffnen Sie den Geräte-Manager](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Wählen Sie auf der Registerkarte **Details** die **Klasse Guid**-Eigenschaft. Der Wert erscheint ist die GUID-Klasse für diese Art von Gerät.
![Kameraeigenschaften](./media/remoteapp-usbredir/ra-classguid.png)
3. Verwenden Sie den Klasse Guid-Wert Geräte umleiten, die es zuordnen.

Zum Beispiel:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Sie können mehrere Geräte Umleitung im gleichen Cmdlet kombinieren. Beispiel: Umleitung lokaler Speicher und eine USB-Webcam Cmdlet sieht folgendermaßen aus:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Beim Festlegen von Umleitung durch die Klassen-GUID werden alle Geräte dieser Klassen-GUID in der angegebenen Auflistung umgeleitet. Wenn mehrere Computer im lokalen Netzwerk, die denselben USB-Webcams sind, können Sie ein einzelnes Cmdlet alle Webkameras Umleitung ausführen.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Umleiten eines USB-Geräts mit der Geräte-Instanz-ID

Wenn Sie eine genauere Kontrolle und Umleitung pro Gerät steuern möchten, können Sie den Parameter **USB\InstanceID** Umleitung.

Der schwierigste Teil dieser Methode findet die USB-Geräte Instanz-ID. Sie benötigen Zugriff auf den Computer und das USB-Gerät. Gehen Sie dann folgendermaßen vor:

1. Aktivieren Sie die Umleitung in Remotedesktopsitzung unter [Wie verwende ich meine Geräte und Ressourcen in einer Remotedesktopsitzung?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Öffnen Sie eine Remotedesktopverbindung, und klicken Sie auf **Optionen anzeigen**.
3. Klicken Sie auf **Speichern** , um die aktuelle Verbindungszeichenfolge eine RDP-Datei speichern.  
    ![Speichern Sie als RDP-Datei](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Wählen Sie einen Dateinamen und einen Speicherort, z. B. "MyConnection.rdp" und "Dieses PC\Documents" und speichern Sie die Datei.
5. MyConnection.rdp mit einem Texteditor öffnen und die Instanz-ID des Geräts, die Sie umleiten möchten.

Verwenden Sie jetzt die Instanz-ID in das folgende Cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen helfen 
Wussten Sie, dass neben diesen Artikel Bewertung und Kommentare unten unten, Sie Artikel selbst ändern können? Fehlt etwas? Etwas? Habe ich etwas schreiben, die nur verwirrend? Bildlauf nach oben und auf Änderungen **auf GitHub bearbeiten** - die Überprüfung zu uns kommen und danach, sobald wir sie abzeichnen, sehen Sie Ihre Änderung und Verbesserung hier.
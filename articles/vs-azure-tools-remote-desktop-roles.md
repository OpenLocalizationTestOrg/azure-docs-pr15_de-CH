<properties 
   pageTitle="Azure-Rollen mit Remotedesktop | Microsoft Azure"
   description="Remotedesktop mit Azure-Rollen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Remotedesktop mit Azure-Rollen

Mithilfe von Azure SDK und Remote Desktop Services möglich Azure-Rollen und virtuellen Maschinen, die von Azure gehostet werden. In Visual Studio können Sie Remote Desktop Services aus einem Azure-Projekt. Um Remote Desktop Services aktivieren Sie Arbeitsprojekt erstellen, das eine oder mehrere Funktionen enthält und Azure veröffentlichen.

>[AZURE.IMPORTANT] Sie sollten eine Azure-Rolle für die Problembehandlung oder Entwicklung zugreifen. Jede virtuelle Maschine dient zum Ausführen einer bestimmten Rolle in Azure Anwendung, Client ausgeführt. Azure mit einen virtuellen Computer hosten, den Sie beliebig verwenden können, finden Sie unter Zugreifen auf Azure Virtual Machines im Server-Explorer.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Aktivieren und Verwenden von Remotedesktop eine Azure-Rolle

1. Öffnen Sie im Projektmappen-Explorer im Kontextmenü für das Projekt, und wählen Sie dann **Veröffentlichen**.

    **Azure-Anwendung veröffentlichen** -Assistent wird angezeigt.

    ![Befehl für ein Cloud-Dienstprojekt veröffentlichen](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. Unten **Microsoft Azure veröffentlichen Settings** -Seite des Assistenten wählen Sie den **Remotedesktop aktivieren** Kontrollkästchen Rollen. 

    **Konfiguration des Remotedesktop** -Dialogfeld wird angezeigt.

1. Wählen Sie unten im Dialogfeld **Remote Desktop-Konfiguration** **Weitere Optionen** . 
 
    Dies zeigt ein Dropdown-Listenfeld, das können Sie erstellen, oder wählen Sie ein Zertifikat, sodass Sie Anmeldeinformationen über Remotedesktop Verbindung verschlüsseln können.

1. Wählen Sie in der Dropdown-Liste ** &lt;erstellen >**, oder wählen Sie eine vorhandene aus. 

    Wenn Sie ein vorhandenes Zertifikat auswählen, werden überspringen Sie die folgenden Schritte.

    >[AZURE.NOTE] Zertifikate, die für eine Remotedesktopverbindung unterscheiden sich von Zertifikaten, die für andere Azure Operationen verwenden. RAS-Zertifikat muss einen privaten Schlüssel.

    Das Dialogfeld **Zertifikat erstellen** .

    1. Geben Sie einen Anzeigenamen für das neue Zertifikat und wählen Sie dann auf **OK** . Das neue Zertifikat wird im Dropdown-Listenfeld angezeigt.

    1. Geben Sie im Dialogfeld **Remote Desktop-Konfiguration** einen Benutzernamen und ein Kennwort.
    
        Ein Konto kann nicht verwendet werden. Geben Sie den Benutzernamen für das neue Konto Administrator.

        >[AZURE.NOTE] Das Kennwort der Komplexität Anforderungen nicht, erscheint ein rotes Symbol neben das Textfeld Kennwort ein. Das Kennwort muss Großbuchstaben, Kleinbuchstaben und Zahlen oder Symbole enthalten.

    1. Wählen Sie ein Datum, der das Konto abläuft und welche Remotedesktopverbindungen blockiert werden.

    1. Nachdem Sie alle erforderlichen Informationen angegeben haben, wählen Sie **OK** .
    
        Die Dateien .cscfg und .csdef werden mehrere Einstellungen, mit denen RAS-Dienst hinzugefügt.

1. Wählen Sie in der **Microsoft Azure veröffentlichen** -Assistent **OK** Wenn Sie Cloud-Dienst veröffentlichen möchten.

    Wenn Sie nicht veröffentlichen möchten, wählen Sie **Abbrechen** . Konfigurationsinformationen werden gespeichert und können Sie den Cloud-Dienst später veröffentlichen.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Ein Azure-Rolle mit dem Remotedesktop herstellen

Nach dem Veröffentlichen des Cloud-Dienstes in Azure können Server-Explorer Sie die virtuellen Computer hostet Azure anmelden. 

1. Knoten Sie im Server-Explorer **Azure** , und erweitern Sie den Knoten für einen Cloud-Dienst und seine Funktionen eine Liste der Instanzen angezeigt.

1. Öffnen Sie das Kontextmenü für einen Knoten und wählen Sie **Verbinden Remotedesktop verwenden**.

    ![Verbindung über Remotedesktop](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Geben Sie den Benutzernamen und das Kennwort, das Sie zuvor erstellt haben. Sie sind jetzt in der Remotesitzung angemeldet.



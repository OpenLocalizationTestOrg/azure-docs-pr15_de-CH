<properties
   pageTitle="Verwalten von Konfigurationen und Profile | Microsoft Azure"
   description="Informationen zum Arbeiten mit Konfigurationen und Profile werden | die Standardeinstellungen für die Bereitstellung in Unternehmen speichern und Veröffentlichen für Clouddienste."
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

# <a name="how-to-manage-service-configurations-and-profiles"></a>Verwalten von Konfigurationen und Profile

## <a name="overview"></a>Übersicht

Wenn Sie Cloud-Dienst veröffentlichen, speichert Visual Studio Konfigurationsinformationen in zwei Arten von Konfigurationsdateien:-Konfigurationen und Profile. Konfigurationen (.cscfg-Dateien) speichern Einstellungen für Deployment-Umgebung für einen Azure-Cloud-Dienst. Azure verwendet diese Konfigurationsdateien verwalten von Cloud-Diensten. Auf der anderen Seite veröffentlichen Profilspeicher (.azurePubxml-Dateien) für Clouddienste. Diese Einstellungen aufgezeichnet, was Sie wählen, wenn Sie des Veröffentlichen-Assistenten mithilfe und lokal von Visual Studio verwendet werden. Dieses Thema erläutert, wie beide Arten von Konfigurationsdateien.

## <a name="service-configurations"></a>Konfigurationen

Sie können mehrere Dienstkonfigurationen für jede Bereitstellung Umgebungen erstellen. Beispielsweise könnte eine Konfiguration für eine lokale Umgebung erstellen, mit denen Sie zu einer Azure-Anwendung und eine andere Konfiguration für Ihre produktionsumgebung testen.

Sie können hinzufügen, löschen, umbenennen und ändern diese Ihre Bedürfnisse Dienstkonfigurationen. Sie können diese Dienstkonfigurationen aus Visual Studio verwalten, wie in der folgenden Abbildung dargestellt.

![Verwalten von Konfigurationen](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Das Dialogfeld **Konfigurationen verwalten** öffnen Sie Eigenschaftenseiten für die Rolle. Öffnen Sie die Eigenschaften für eine Rolle in Azure Projekt öffnen Sie des Kontextmenüs für diese Rolle und wählen Sie dann **Eigenschaften**. Auf **der Registerkarte** erweitern Sie die **Konfiguration** , und wählen Sie **Verwalten** , öffnen Sie das Dialogfeld **Konfigurationen verwalten** .

### <a name="to-add-a-service-configuration"></a>Hinzufügen eine Konfiguration

1. Öffnen Sie im Projektmappen-Explorer im Kontextmenü der Azure-Projekt zu, und wählen Sie **Konfigurationen verwalten**.

    Das Dialogfenster **Dienstkonfigurationen verwalten** .

1. Um eine Dienstkonfiguration hinzuzufügen, müssen Sie eine Kopie einer vorhandenen Konfiguration erstellen. Dazu wählen Sie die Konfiguration, die Sie aus der Liste und wählen dann **Kopie erstellen**möchten.

1. (Optional) Der Dienstkonfiguration umbenennen, neue Dienstkonfiguration aus der Liste auswählen und wählen **Umbenennen**. Geben Sie im Feld **Name** den Namen für diese Konfiguration und wählen Sie dann **OK**.

    Eine neue Service Konfigurationsdatei mit dem Namen ServiceConfiguration. [Neuer Name] .cscfg der Azure-Projekt im Projektmappen-Explorer hinzugefügt.


### <a name="to-delete-a-service-configuration"></a>So löschen Sie eine Konfiguration

1. Öffnen Sie im Projektmappen-Explorer im Kontextmenü der Azure-Projekt zu, und wählen Sie **Konfigurationen verwalten**.

    Das Dialogfenster **Dienstkonfigurationen verwalten** .

1. Um eine Konfiguration zu löschen, wählen Sie die Konfiguration, die Sie aus **der Liste** löschen und wählen Sie **Entfernen**möchten. Dialogfeld zu überprüfen, ob diese Konfiguration löschen möchten.

1. Wählen Sie **Löschen**.

     Azure-Projekt im Projektmappen-Explorer die Dienstkonfigurationsdatei entfernt.


### <a name="to-rename-a-service-configuration"></a>Umbenennen eine Konfiguration

1. Öffnen Sie im Projektmappen-Explorer im Kontextmenü der Azure-Projekt, und wählen Sie **Konfigurationen verwalten**.

    Das Dialogfenster **Dienstkonfigurationen verwalten** .

1. Zum Umbenennen einer Konfigurations wählen Sie neue Dienstkonfiguration aus **der Liste aus** , und wählen Sie **Umbenennen**. Geben Sie im Feld **Name** den Namen für diese Konfiguration verwendet werden soll, und klicken Sie auf **OK**.

    Service-Konfigurationsdatei ist in Azure-Projekt im Projektmappen-Explorer umbenannt.

### <a name="to-change-a-service-configuration"></a>Eine Konfiguration ändern

- Wenn Sie eine Konfiguration ändern möchten, öffnen Sie das Kontextmenü für die jeweilige Rolle in Azure-Projekt ändern möchten, und wählen Sie **Eigenschaften**. Siehe [wie: Konfigurieren von Rollen für eine Azure-Cloud-Dienst mit Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) Weitere Informationen.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Stellen verschiedene Kombinationen mit festlegen

Mithilfe eines Profils können Sie automatisch den **Webpublishing-Assistenten** mit verschiedenen Einstellungen für verschiedene Zwecke ausfüllen. Beispielsweise können Sie ein Profil für das Debuggen und weitere Version erstellt. In diesem Fall Ihr Profil **Debuggen** müssten **IntelliTrace** aktiviert und die Konfiguration zum **Debuggen** ausgewählt und Ihr Profil **Version** müsste **IntelliTrace** deaktiviert und **die Releasekonfiguration ausgewählt** . Unterschiedliche Profile können Sie einen Dienst mit einem anderen Speicherkonto bereitstellen.

Wenn Sie den Assistenten zum ersten Mal ausführen, wird ein Standardprofil erstellt. Visual Studio speichert das Profil in eine Datei mit einer Erweiterung .azurePubXml und Azure-Projekts im Ordner **Profile** hinzugefügt. Wenn Sie manuell verschiedene Optionen angeben, wenn Sie den Assistenten später ausführen, wird die Datei automatisch aktualisiert. Bevor Sie das folgende Verfahren ausführen, sollten Sie bereits Ihr Cloud-Dienst einmal veröffentlicht haben.

### <a name="to-add-a-profile"></a>Hinzufügen eines Profils

1. Öffnen Sie das Kontextmenü für Azure-Projekts und wählen Sie dann **Veröffentlichen**.

1. Neben der Liste **Zielprofil** wählen Sie **Profil speichern** , wie die folgende Abbildung zeigt. Dies erstellt ein Profil.

    ![Erstellen Sie ein neues Profil](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Nach dem Erstellen des Profils wählen Sie **< verwalten... >** in das **Zielprofil** aus.

    **Profile verwalten** -Dialogfeld wird angezeigt, wie die folgende Abbildung zeigt.

    ![Dialogfeld Profile verwalten](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Wählen Sie in **der Liste** ein Profil und wählen Sie die **Kopie erstellen**.

1. Wählen Sie die Schaltfläche **Schließen** .

    Das neue Profil in der Profilliste Ziel wird angezeigt.

1. Wählen Sie in der Liste **Zielprofil** das soeben erstellte Profil. Einstellungen Webpublishing-Assistenten sind mit den Optionen aus dem Profil ausgewählten gefüllt.

1. Wählen Sie die Schaltflächen **zurück** und **Weiter** auf jeder Seite des Veröffentlichen-Assistenten anzuzeigen und dann die Einstellungen für dieses Profil. Informationen finden Sie unter [Assistent Azure veröffentlichen](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Klicken Sie nach dem Anpassen der Einstellungen wählen Sie **Weiter** zurück zur Einstellungsseite. Das Profil wird beim Veröffentlichen des Diensts diese Einstellungen oder neben der Liste der Profile **Speichern** auswählen.

### <a name="to-rename-or-delete-a-profile"></a>Umbenennen oder Löschen eines Profils

1. Öffnen Sie das Kontextmenü für Azure-Projekts und wählen Sie dann **Veröffentlichen**.

1. Wählen Sie in der Liste **Zielprofil** **Verwalten**.

1. Im Dialogfeld **Profile verwalten** wählen Sie das Profil, das Sie löschen möchten, und wählen Sie **Entfernen**.

1. Wählen Sie im Dialogfeld Bestätigung **OK**.

1. Wählen Sie **Schließen**.

### <a name="to-change-a-profile"></a>Profil ändern

1. Öffnen Sie das Kontextmenü für Azure-Projekts und wählen Sie dann **Veröffentlichen**.

1. Wählen Sie in der Liste **Zielprofil** Profil, das Sie ändern möchten.

1. Wählen Sie die Schaltflächen **zurück** und **Weiter** auf jeder Seite des Veröffentlichen-Assistenten anzuzeigen, und ändern Sie dann die gewünschten. Informationen finden Sie unter [Assistent Azure veröffentlichen](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Wählen Sie nach Abschluss der Einstellungen **Weiter** zurück zur **Einstellungsseite** .

1. (Optional) Wählen Sie **Veröffentlichen** neue Einstellungen Cloud-Dienst veröffentlichen. Wenn Ihre Cloud-Dienst zu diesem Zeitpunkt veröffentlichen möchten und den Webpublishing-Assistenten schließen gefragt Visual Studio Sie das Profil speichern möchten.

## <a name="next-steps"></a>Nächste Schritte

Zum Konfigurieren von anderen Teilen der Azure-Projekts in Visual Studio finden Sie unter [Konfigurieren von Azure-Projekt](http://go.microsoft.com/fwlink/p/?LinkID=623075)

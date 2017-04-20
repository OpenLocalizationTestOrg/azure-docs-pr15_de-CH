<properties
   pageTitle="Vorzubereiten, zu veröffentlichen oder eine Azure-Anwendung aus Visual Studio bereitstellen | Microsoft Azure"
   description="Lernen Sie Verfahren zum Cloud und Speicher Kontodiensten einrichten und Konfigurieren der Azure-Anwendung."
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

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Bereiten Sie vor, zu veröffentlichen oder Bereitstellen einer Azure-Anwendung aus Visual Studio

## <a name="overview"></a>Übersicht

Bevor Sie ein Cloud-Dienstprojekt veröffentlichen können, müssen Sie Folgendes einrichten:

- **Cloud-Dienst** Rollen in der Azure-Umgebung ausführen

- Ein **Speicherkonto** , die BLOBs, Warteschlangen und Tabelle zugreifen.

Gehen Sie diese Dienste einrichten und Konfigurieren der Anwendung


## <a name="create-a-cloud-service"></a>Erstellen Sie einen Clouddienst

Um einen Cloud-Dienst in Azure veröffentlichen, müssen Sie zunächst einen Cloud-Dienst erstellen Ihre Rollen in der Azure-Umgebung ausgeführt. Cloud-Dienst können im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)wie im Abschnitt **einen Cloud-Dienst mithilfe der Azure-Verwaltungsportal erstellen**, später in diesem Thema beschrieben. Sie können einen Cloud-Dienst in Visual Studio erstellen, mit dem Veröffentlichen-Assistenten.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Cloud-Dienst erstellen mithilfe von Visual Studio

1. Öffnen Sie das Kontextmenü für den Azure-Projekt, und wählen Sie **Veröffentlichen**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Wenn Sie sich angemeldet haben, melden Sie sich mit Ihrem Benutzernamen und Kennwort für das Microsoft-Konto oder Organisationseinheit Konto Azure-Abonnement zugeordnet ist.

1. Wählen Sie die Schaltfläche **Weiter** , um die **Standardeinstellungen** anzuzeigen.

    ![Publishing Wizard Common Settings](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Wählen Sie in der **Cloud-Services** **Neu erstellen**. Das Dialogfenster **Azure Services erstellen** .

1. Geben Sie dem Cloud-Dienst. Der Name ist Teil der URL für den Dienst und muss daher eindeutig sein. Der Name wird nicht beachtet.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Um einen Cloud-Dienst mithilfe der Azure-Verwaltungsportal

1. Melden Sie sich im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=253103) auf der Microsoft-Website.

1. (optional) Eine Liste der Cloud-Dienste anzeigen, die Sie bereits erstellt haben, wählen Sie Clouddienste Link auf der linken Seite.

1. Wählen Sie die **+** -Symbol in der linken unteren Ecke, und wählen Sie dann im angezeigten Menü auf **Cloud-Dienst** . Anderen zwei Optionen **Schnell** und **Benutzerdefinierte**, angezeigt. Wunsch **Schnell erstellen**, können Sie einen Cloud-Dienst erstellen, durch Angabe der URL und den Bereich, in dem es physisch gehostet wird. Wunsch **Benutzerdefinierte**können Sie sofort einen Cloud-Dienst veröffentlichen, durch Angeben eines Pakets (.cspkg-Datei), eine Konfigurationsdatei (.cscfg) und ein Zertifikat. Benutzerdefinierte erstellen ist nicht erforderlich, wenn Sie den Cloud-Dienst mit dem Befehl **Veröffentlichen** in Azure-Projekt veröffentlichen möchten. **Veröffentlichen** wird im Kontextmenü für ein Azure-Projekt verfügbar.

1. Wählen Sie den Cloud-Dienst mithilfe von Visual Studio später veröffentlichen **Schnell erstellen** .

1. Geben Sie einen Namen für den Clouddienst. Die vollständige URL wird neben dem Namen angezeigt.

1. Wählen Sie in der Liste den Bereich befinden sich die meisten Benutzer.

1. Wählen Sie am unteren Fensterrand Link **Cloud-Dienst erstellen** .

## <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

Ein Speicherkonto Zugriff BLOBs, Warteschlangen und Tabelle. Sie können ein Speicherkonto mithilfe von Visual Studio oder das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=253103)erstellen.

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Ein Speicherkonto erstellen mithilfe von Visual Studio

1. Öffnen Sie im **Projektmappen-Explorer**im Kontextmenü für den Knoten **Speicher** , und wählen Sie **Storage-Konto erstellen**.

    ![Erstellen eines neuen Kontos Azure-Speicher](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Wählen Sie oder geben Sie die folgende Informationen für das neue Speicherkonto im Dialogfeld **Storage-Konto erstellen** .
    - Der Azure-Abonnement Sie das Speicherkonto hinzufügen möchten.
    - Der Name für das neue Speicherkonto verwenden möchten.
    - Region oder Gruppe (z. B. Westen der USA oder Asien).
    - Der Typ der Replikation für das Speicherkonto wie Geo-Redundant verwenden möchten.

1. Wenn Sie fertig sind, wählen Sie **Erstellen**. Neue Speicherkonto erscheint in den **Speicher** im **Server-Explorer**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Ein Speicherkonto mit klassischen Azure-Portal erstellen

1. Melden Sie sich im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=253103) auf der Microsoft-Website.

1. (Optional) Um Speicherkonten anzuzeigen, wählen Sie den **Speicher** -Link im Bereich auf der linken Seite.

1. Wählen Sie in der unteren linken Ecke der Seite die **+** Symbol.

1. Im angezeigten Menü Wählen Sie **Speicher**und dann **Schnell erstellen**.

1. Benennen Sie dem Speicherkonto, das eine eindeutige Url führt.

1. Benennen Sie Ihre Cloud-Dienst. Die vollständige URL wird neben dem Namen angezeigt.

1. Wählen Sie in der Liste der Regionen eine Region befinden sich die meisten Benutzer.

1. Geben Sie an, ob Sie Geo-Replikation aktivieren möchten. Aktivieren Sie Geo-Replikation werden Daten an mehreren physischen Standorten senkt die Möglichkeit eines Verlustes gespeichert. Dadurch Storage teurer, aber senken die Kosten Standort aktivieren, wenn das Speicherkonto anstatt die Funktion später erstellen. Weitere Informationen finden Sie unter [Geo-Replikation](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Wählen Sie am unteren Fensterrand Link **Storage-Konto erstellen** .

Nach der Erstellung Ihres Speicherkontos sehen Sie die URLs, mit denen Sie Zugriff auf Ressourcen in Azure-Speicherdienste und die primären und sekundären Zugriffstasten für Ihr Konto. Sie verwenden diese gegen die Speicherdienste authentifizieren.

>[AZURE.NOTE] Die sekundäre Taste ermöglicht den gleichen Zugriff auf das Speicherkonto als primäre Schlüssel und generiert eine Sicherung der primäre Schlüssel manipuliert. Darüber hinaus wird empfohlen, dass die Zugriffstasten in regelmäßigen Abständen neu generieren. Sie können eine Einstellung Verbindung Zeichenfolge an den sekundären während den Primärschlüssel Regenerieren Sie ändern den regenerierten Primärschlüssel verwenden und den sekundären Schlüssel neu erstellen.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Konfigurieren Sie Ihre Anwendung auf Dienste von Storage-Konto

Sie müssen keine Rolle konfigurieren, der greift auf Speicher-Services um Azure-Speicherdienste zu verwenden, die Sie erstellt haben. Hierzu können Sie mehrere Dienstkonfigurationen für Azure-Projekts. Standardmäßig werden zwei Azure-Projekts erstellt. Mithilfe mehrerer Dienstkonfigurationen verwenden dieselbe Verbindungszeichenfolge im Code können unterschiedliche Werte für eine Verbindungszeichenfolge in jeder Konfiguration haben. Sie können z. B. eine Konfiguration ausführen und Debuggen der Anwendung lokal Azure Speicheremulator und eine andere Konfiguration die Anwendung in Azure veröffentlichen. Weitere Informationen zu Service-Konfigurationen finden Sie unter [Konfigurieren der Azure mithilfe mehrerer Service Projektkonfigurationen](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>So konfigurieren Sie die Anwendung Dienste nutzen das Speicherkonto

1. Öffnen Sie in Visual Studio Ihre Azure-Lösung. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für jede Rolle in Azure-Projekts, das Speicher-Services zugreift, und wählen Sie **Eigenschaften**. Eine Seite mit dem Namen der Rolle wird im Visual Studio-Editor angezeigt. Die Seite zeigt die Felder der Registerkarte **Konfiguration** .

1. **Die Eigenschaftenseiten für die Rolle Einstellungen.**

1. Wählen Sie in der Liste **Konfiguration** der Name der Konfiguration, die Sie bearbeiten möchten. Wenn Sie alle Dienstkonfigurationen für diese Rolle ändern möchten, können Sie **Alle Konfigurationen**.  Weitere Informationen zum Aktualisieren von Konfigurationen finden Sie im Abschnitt **Verwalten von Verbindungszeichenfolgen für Speicherkonten** im Thema [Konfigurieren von Rollen für eine Azure-Cloud-Dienst mit Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Um jede Verbindung Zeichenfolge ändern, wählen Sie die **** neben der Einstellung. Das Dialogfeld **Storage-Verbindungszeichenfolge erstellen** .

1. Wählen Sie unter **Verbindung herstellen über** **Ihr Abonnement** .

1. Wählen Sie in der Liste **Abonnement** Ihres Abonnements. Wenn die Liste der Abonnements der enthält, die Sie möchten, wählen Sie **Veröffentlichen** Einstellungen herunterladen.

1. Wählen Sie in der Liste **Namen** speicherkontonamen. Azure Tools erhält Anmeldeinformationen Speicher automatisch mithilfe der Datei "publishsettings". Um Ihre Storage-Anmeldeinformationen manuell anzugeben, wählen Sie die Option **manuell eingegebenen Anmeldeinformationen** und dann fort. Sie können Ihren speicherkontonamen und Primärschlüssel aus dem [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)abrufen. Möchten Sie Ihren Speicher angegeben Konto manuell wählen Sie **OK** , um das Dialogfeld zu schließen.

1. Wählen Sie Verknüpfung Anmeldeinformationen **Konto geben** .

1. Geben Sie im Feld **Kontoname** den Namen Ihres Speicherkontos.

    >[AZURE.NOTE] [Klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)melden Sie an, und wählen Sie dann auf **Speichern** . Das Portal zeigt eine Liste der Speicherkonten. Wenn Sie ein Konto auswählen, wird eine Seite geöffnet. Sie können den Namen des Speicherkontos auf dieser Seite. Bei Verwendung eine frühere Version des klassischen Portal wird der Name Ihres Speicherkontos **Speicherkonten** an. Um diesen Namen zu kopieren, markieren sie im Fenster **Eigenschaften** der Ansicht und wählen Sie dann die Tasten STRG + C. In Visual Studio den Namen einfügen, wählen Sie das Textfeld **Kontoname** und dann die Tasten STRG + V.

1. Im Feld **Konto** Ihr Primärschlüssel eingeben oder kopieren und Einfügen aus dem [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885).
    Dieser Schlüssel kopieren:

    1. Wählen Sie am unteren Rand der Seite des entsprechenden Kontos **Schlüssel verwalten** .

    1. Markieren Sie auf der Seite **Schlüssel Zugriff verwalten** die primäre Taste, und wählen Sie dann die Tasten STRG + C.

    1. Fügen Sie in Azure Tools des Schlüssels im **kontoschlüssel** .

    1. Sie müssen auswählen, welche der folgenden Optionen bestimmen, wie der Dienst das Speicherkonto zugreifen:
        - **HTTP verwenden**. Dies ist die Standardoption. Z. B. `http://<account name>.blob.core.windows.net`.
        - Für eine sichere Verbindung **Verwenden HTTPS** . Z. B. `https://<accountname>.blob.core.windows.net`.
        - **Geben Sie benutzerdefinierte Endpunkte** für jeden der drei Dienste. Sie können dann diese Endpunkte für den bestimmten Dienst in das Feld eingeben.

        >[AZURE.NOTE] Wenn Sie benutzerdefinierte Endpunkte erstellen, können Sie eine komplexere Verbindungszeichenfolge erstellen. Bei Verwendung dieses Formats Zeichenfolge können Sie Storage-Endpunkte angeben, die einen benutzerdefinierten Domänennamen enthalten, den für das Speicherkonto mit dem BLOB-Dienst registriert haben. Außerdem können Sie nur Zugriff auf BLOB-Ressourcen in einem einzelnen Container mit einem gemeinsamen Zugriff gewähren. Weitere Informationen zum Erstellen von benutzerdefinierter Endpunkten finden Sie unter [Azure Storage-Verbindungszeichenfolgen konfigurieren](storage-configure-connection-string.md).

1. Diese Verbindung Zeichenfolge speichern wählen Sie **OK** und dann auf der Symbolleiste auf die Schaltfläche **Speichern** . Nachdem diese Änderungen erhalten den Wert der Verbindungszeichenfolge im Code Sie mithilfe von [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Beim Veröffentlichen der Anwendung in Azure wählen Sie die Konfiguration, die das Azure-Speicherkonto für die Verbindungszeichenfolge enthält. Nach der Anmeldung überprüfen Sie, ob die Anwendung erwartungsgemäß gegen Azure-Speicherdienste

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Veröffentlichen von apps in Azure aus Visual Studio finden Sie unter [Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools](vs-azure-tools-publishing-a-cloud-service.md).

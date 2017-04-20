<properties
    pageTitle="Azure BLOB-Speicher-Ressourcen mit dem Speicher-Explorer (Vorschau) | Microsoft Azure"
    description="Verwalten von Azure Blob-Container und Blobs mit Speicher-Explorer (Vorschau)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Ressourcen Sie Azure BLOB-Speicher-mit dem Speicher-Explorer (Vorschau)

## <a name="overview"></a>Übersicht

[Azure BLOB-Speicher](./storage/storage-dotnet-how-to-use-blobs.md) ist ein Dienst zum Speichern großer Mengen an unstrukturierten Daten wie Text oder binär, die überall in der Welt über HTTP oder HTTPS zugegriffen werden kann.
Sie können BLOB-Speicher Daten öffentlich verfügbar machen oder Anwendungsdaten Privat speichern. In diesem Artikel erfahren Sie, wie Speicher-Explorer (Vorschau) mit BLOBs arbeiten und Blobs.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die Schritte in diesem Artikel benötigen Sie Folgendes:

- [Herunterladen und Installieren von Speicher-Explorer (Vorschau)](http://www.storageexplorer.com)
- [Verbinden Sie mit einer Azure-Speicher oder](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>BLOB-Container erstellen

Alle Blobs müssen in einem BLOB-Container befinden, ist einfach eine logische Gruppierung von Blobs. Ein Konto kann eine unbegrenzte Anzahl von Containern enthalten, und jeder Container kann eine unbegrenzte Anzahl von Blobs speichern.

Die folgenden Schritte veranschaulichen, wie einen BLOB-Container im Speicher-Explorer (Vorschau) erstellt.

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto BLOB-Container erstellen möchten.
1.  **Blob-Container**Maustaste und wählen Sie aus dem Kontextmenü - **Blob-Container erstellen**.

    ![Kontextmenü für BLOB-Container erstellen][0]

1.  Ein Textfeld wird unter dem Ordner **Blob-Container** angezeigt. Geben Sie den Namen für den BLOB-Container. Finden Sie im Abschnitt [Containerregeln](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) eine Liste von Regeln und Einschränkungen auf Blob Benennungscontainern.

    ![Blob-Container Textfeld erstellen][1]

1.  Drücken Sie die **EINGABETASTE** nach Abschluss den BLOB-Container erstellen oder **Esc** zum Abbrechen. Sobald der BLOB-Container erfolgreich erstellt wurde, wird unter den **Blob-Container** Ordner der ausgewählten Speichergruppe angezeigt.

    ![BLOB-Container erstellt][2]

## <a name="view-a-blob-containers-contents"></a>Ein BLOB-Container-Inhalt anzeigen

BLOB-Container enthalten Blobs und Ordner (die auch Blobs enthalten können).

Die folgenden Schritte veranschaulichen, wie ein BLOB-Container im Speicher-Explorer (Vorschau) anzuzeigen:

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto mit BLOB-Container anzeigen möchten.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Maustaste blobcontainer anzeigen möchten und wählen Sie im Kontextmenü - **Editor öffnen Blob Container**.
Sie können auch den Blob-Container Doppelklicken Sie anzeigen möchten.

    ![Kontextmenü öffnen BLOB-Container-editor][19]

1.  Im Hauptbereich wird der BLOB-Inhalt des Containers angezeigt.

    ![BLOB-Container-editor][3]

## <a name="delete-a-blob-container"></a>Einen BLOB-Container löschen

BLOB-Container können einfach erstellt und ggf. gelöscht. (Löschen Sie einzelne Blobs finden Siehe Abschnitt [Verwalten von Blobs in einem BLOB-Container](./#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie einen BLOB-Container im Speicher-Explorer (Vorschau) löschen:

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto mit BLOB-Container anzeigen möchten.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Maustaste blobcontainer, die, den Sie löschen möchten, und wählen Sie im Kontextmenü - **Löschen**.
Sie können auch ausgewählte Blob wirklich **Löschen** drücken.

    ![BLOB-Container im Kontextmenü Löschen][4]

1.  Wählen Sie **Ja** , um das Bestätigungsdialogfeld.

    ![Blob Container Bestätigung löschen][5]

## <a name="copy-a-blob-container"></a>Einen BLOB-Container kopieren

Speicher-Explorer (Preview) können Sie einen BLOB-Container in die Zwischenablage kopieren und fügen Sie BLOB-Container in einen anderen Speicher-Konto. (Kopieren Sie einzelne Blobs finden Siehe Abschnitt [Verwalten von Blobs in einem BLOB-Container](./#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie BLOB-Container aus einem Konto in ein anderes kopieren.

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich mit der BLOB-Container Speicherkonto kopieren möchten.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Maustaste blobcontainer, die, den Sie kopieren möchten, und wählen Sie aus dem Kontextmenü - **Kopie BLOB-Container**.

    ![BLOB-Container Kontextmenü kopieren][6]

1.  Mit der rechten Maustaste des gewünschte "Target" Speicherkontos in den BLOB-Container eingefügt werden soll und wählen Sie aus dem Kontextmenü - **Blob-Container einfügen**.

    ![BLOB-Container Kontextmenü einfügen][7]

## <a name="get-the-sas-for-a-blob-container"></a>SAS für BLOB-Container abrufen

[SAS (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto.
Dies bedeutet, dass ein Client für einen bestimmten Zeitraum mit einer bestimmten Gruppe von Berechtigungen, Berechtigungen für Objekte in Ihrem Speicherkonto begrenzt, ohne Ihr Konto Zugriffstasten gemeinsam gewähren können.

Die folgenden Schritte veranschaulichen, wie SAS für BLOB-Container erstellen:

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto mit BLOB-Container SAS abgerufen werden soll.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Mit der rechten Maustaste des gewünschten Blob-Containers und wählen Sie aus dem Kontextmenü - **SAS erhalten**.

    ![SAS-Kontext Menü][8]

1.  Klicken Sie im Dialogfeld **SAS** geben Richtlinie, Start- und Enddatum, Zeitzone und Zugriffsebenen für die Ressource ein.

    ![SAS-Optionen][9]

1.  Wenn Sie die SAS-Optionen angegeben sind, wählen Sie **Erstellen**.

1.  Ein zweites **SAS** -Dialogfeld wird angezeigt, die Listen den BLOB-Container sowie die URL Formularwerte mit Speicherressourcen zugreifen.
Wählen Sie **Kopie** neben der URL in die Zwischenablage kopieren möchten.

    ![Kopieren von SAS-URLs][10]

1.  Danach wählen Sie **Schließen**.

## <a name="manage-access-policies-for-a-blob-container"></a>Verwalten von Richtlinien für einen BLOB-container

Die folgenden Schritte veranschaulichen verwalten (hinzufügen und entfernen) Zugriffsrichtlinien für einen BLOB-Container:

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto mit BLOB-Container, dessen Richtlinien Sie verwalten möchten.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Wählen Sie den gewünschten blobcontainer und wählen Sie aus dem Kontextmenü - **Richtlinien verwalten**.

    ![Kontextmenü für Access-Richtlinien verwalten][11]

1.  **Richtlinien** -Dialogfeld listet alle Richtlinien für den ausgewählten blobcontainer angelegt.

    ![Policy-Optionen][12]        

1.  Gehen Sie je nach Access Policy Management folgendermaßen vor:

    - **Hinzufügen einer neuen Zugriffsrichtlinie** - **Hinzufügen**. Generiert, zeigt das Dialogfeld **Richtlinien** die neu hinzugefügte Richtlinie (mit Standardeinstellungen).
    - **Eine Zugriffsrichtlinie bearbeiten** – alle gewünschten bearbeiten und **Speichern**.
    - **Eine Zugriffsrichtlinie entfernen** - Option **Entfernen** neben der Richtlinie, die Sie entfernen möchten.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Festlegen Sie öffentliche Zugriffsebene für einen BLOB-container

Standardmäßig wird jeder BLOB-Container "Kein öffentlicher Zugriff" festgelegt.

Die folgenden Schritte veranschaulichen, wie eine öffentliche Zugriffsebene für einen BLOB-Container.

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto mit BLOB-Container, dessen Richtlinien Sie verwalten möchten.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Wählen Sie den gewünschten blobcontainer und wählen Sie aus dem Kontextmenü - **Legen Sie öffentliche Zugriffsebene**.

    ![Zugang der Öffentlichkeit Kontext Menü][13]

1.  Geben Sie im Dialogfeld **Festgelegt Container öffentliche Zugriffsebene** die gewünschte Zugriffsebene.

    ![Zugang der Öffentlichkeit Optionen festlegen][14]

1.  Wählen Sie **Übernehmen**.

## <a name="managing-blobs-in-a-blob-container"></a>Verwalten von Blobs in einem BLOB-container

Nach dem Erstellen eines BLOB-Containers können einen Blob auf BLOB-Container hochladen, einen Blob auf Ihren lokalen Computer herunterladen, öffnen ein BLOBs auf dem lokalen Computer und vieles mehr.

Die folgenden Schritte veranschaulichen verwalten Blobs (und Ordner) in einem BLOB-Container.

1.  Open Storage Explorer (Vorschau).
1.  Erweitern Sie im linken Bereich das Speicherkonto mit BLOB-Container verwalten möchten.
1.  Erweitern Sie das Speicherkonto **Blob-Container**.
1.  Doppelklicken Sie auf blobcontainer anzeigen möchten.
1.  Im Hauptbereich wird der BLOB-Inhalt des Containers angezeigt.

    ![BLOB-Container anzeigen][3]

1.  Im Hauptbereich wird der BLOB-Inhalt des Containers angezeigt.

1.  Folgendermaßen Sie Aufgaben ausführen möchten:

    - **Hochladen von Dateien auf einen BLOB-container**

        1.  Im Hauptbereich Symbolleiste im Dropdown-Menü Wählen Sie **Hochladen**und **Dateien hochladen aus** .

            ![Dateien hochladen][15]

        1.  Wählen Sie im Dialogfeld **Dateien hochladen** Schaltfläche mit Auslassungszeichen****rechts im Textfeld **Dateien** die Dateien auswählen, die Sie hochladen möchten.

            ![Dateien hochladen][16]

        1.  Geben Sie den **BLOB-Typ**. [Erste Schritte mit Azure BLOB-Speicher mit .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) Artikel erläutert die Unterschiede zwischen den verschiedenen Blob.

        1.  Geben Sie optional einen Zielordner, in dem die ausgewählten Dateien hochgeladen werden. Wenn der Zielordner nicht vorhanden ist, wird sie erstellt.

        1.  Wählen Sie **Hochladen**.

    - **Hochladen eines Ordners mit einem BLOB-container**

        1.  Im Hauptbereich Symbolleiste im Dropdown-Menü Wählen Sie **Hochladen**und **Ordner hochladen aus** .

            ![Menü Ordner hochladen][17]

        1.  Wählen Sie im Dialogfeld **Ordner hochladen** Schaltfläche mit Auslassungszeichen****rechts im Textfeld **Ordner** auf den Ordner, dessen Inhalt Sie hochladen möchten.

            ![Hochladen von Ordneroptionen][18]

        1.  Geben Sie den **BLOB-Typ**. [Erste Schritte mit Azure BLOB-Speicher mit .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) Artikel erläutert die Unterschiede zwischen den verschiedenen Blob.

        1.  Geben Sie optional einen Zielordner, in den Inhalt des ausgewählten Ordners hochgeladen werden. Wenn der Zielordner nicht vorhanden ist, wird sie erstellt.

        1.  Wählen Sie **Hochladen**.

    - **Einen Blob auf Ihren lokalen Computer herunterladen**

        1.  Wählen Sie das Blob, das Sie herunterladen möchten.

        1.  Wählen Sie im Hauptbereich Symbolleiste **herunterladen**.

        1.  Im Dialogfeld **geben an, wo das heruntergeladene Blob speichern** Geben Sie den Speicherort das Blob heruntergeladen und den Namen geben möchten.  

        1.  **Wählen Sie aus.**

    - **Einen Blob auf dem lokalen Computer öffnen**

        1.  Wählen Sie das Blob, das Sie öffnen möchten.

        1.  Wählen Sie im Hauptbereich Symbolleiste **Öffnen**.

        1.  Das Blob wird heruntergeladen und das Blob zugrunde liegende Dateityp zugeordneten Anwendung geöffnet.

    - **Einen Blob in die Zwischenablage kopieren**

        1.  Wählen Sie das Blob, das Sie kopieren möchten.

        1.  Wählen Sie im Hauptbereich Symbolleiste **Kopieren**.

        1.  Im linken Bereich in einen anderen blobcontainer navigieren Sie und doppelklicken Sie im Hauptfenster anzeigen.

        1.  Wählen Sie im Hauptbereich Symbolleiste **Einfügen** eine Kopie des Blob erstellen.

    - **Löschen eines BLOBs**

        1.  Wählen Sie das Blob, das Sie löschen möchten.

        1.  Wählen Sie im Hauptbereich Symbolleiste **Löschen**.

        1.  Wählen Sie **Ja** , um das Bestätigungsdialogfeld.

## <a name="next-steps"></a>Nächste Schritte

- Der [aktuelle Speicher-Explorer (Vorschau) Versionshinweise und Videos](http://www.storageexplorer.com)anzeigen
- Erfahren Sie, wie [Azure-Blobs, Tabellen, Warteschlangen und Dateien erstellen](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
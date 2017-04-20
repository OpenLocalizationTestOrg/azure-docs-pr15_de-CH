<properties 
    pageTitle="Eine fortlaufende Integrationslösung Jenkins Azure-Speicher mit | Microsoft Azure" 
    description="In diesem Lernprogramm anzeigen wie Azure Blob-Dienst als Repository für Artefakte erstellt Jenkins fortlaufende Integration Lösung erstellen." 
    services="storage" 
    documentationCenter="java" 
    authors="dineshmurthy" 
    manager="jahogg" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Mithilfe von Azure Storage Lösung Jenkins fortlaufende Integration

## <a name="overview"></a>Übersicht

Die folgende Informationen gezeigt, wie BLOB-Speicher als Repository für Buildartefakte erstellt eine Projektmappe Jenkins fortlaufende Integration (CI) oder als downloadbare Dateien in einem Buildprozess verwendet werden. Eines Szenarios, in denen Sie dies nützlich ist, ist in eine agile Entwicklungsumgebung (Java oder anderen Sprachen) kodieren Builds ausführen basierend auf fortlaufende Integration und benötigen ein Repository für die Artefakte erstellen konnte, z. B. Mitgliedern der Organisation, Ihre Kunden freigeben oder ein Archiv beizubehalten. Ein anderes Szenario ist wenn die Build-Job selbst andere Dateien, z. B. Abhängigkeiten als Teil der Erstellungseingabe herunterladen.

In diesem Lernprogramm werden Sie Azure Storage Plugin für Jenkins CI bereitgestellt von Microsoft verwenden.

## <a name="overview-of-jenkins"></a>Übersicht über Jenkins ##

Jenkins kann fortlaufende Integration eines, da Entwickler ihre Änderungen integrieren und haben Builds automatisch häufig und damit die Produktivität der Entwickler. Builds bilden und Buildartefakte können in verschiedenen Repositories hochgeladen werden. In diesem Thema zeigen, wie Azure BLOB-Speicher als Repository für die Buildartefakte zu verwenden. Es zeigt auch eine Abhängigkeit von Azure BLOB-Speicher herunterladen.

Informationen über Jenkins finden unter [Jenkins erfüllen](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Vorteile von BLOB-Dienst ##

Agile Entwicklung Buildartefakte host mit BLOB-Dienst bietet folgende Vorteile:

- Hohe Verfügbarkeit Ihrer Buildartefakte bzw. herunterladbare abhängig.
- Leistung beim Projektmappe Jenkins CI Buildartefakte hochgeladen.
- Leistung, wenn Ihre Kunden und Partner Buildartefakte herunterladen.
- Kontrolle über Zugriffsrichtlinien für Benutzer mit anonymer Zugriff, Ablauf-basierten freigegebenen Signatur Zugriff, Private usw.

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Sie benötigen folgende Projektmappe Jenkins CI BLOB-Dienst mit:

- Eine fortlaufende Integration Jenkins-Lösung.

    Wenn Sie derzeit eine Jenkins CI-Lösung haben, können Sie Jenkins CI-Lösung mit dem folgenden Verfahren ausführen:

    1. Auf einem Computer Java aktiviert wird herunterladen Sie jenkins.war von <http://jenkins-ci.org>.
    2. Eine Befehlszeile im Ordner geöffnet wird, jenkins.war ausführen:

        `java -jar jenkins.war`

    3. Öffnen Sie in Ihrem Browser `http://localhost:8080/`. Das Dashboard Jenkins daraufhin wird mit der Installation und Konfiguration von Azure Storage-Plugin.

        Während eine normale Jenkins CI-Lösung eingerichtet werden würde als Dienst Jenkins War in der Befehlszeile ausgeführt werden für dieses Lernprogramm.

- Ein Azure-Konto. Sie können für ein Azure-Konto unter <http://www.azure.com>anmelden.

- Ein Azure Storage-Konto. Haben Sie bereits ein Speicherkonto, können Sie eine mit den Schritten [erstellen ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account)erstellen.

- Vertrautheit mit Jenkins CI-Lösung wird empfohlen, jedoch nicht erforderlich, z. B. der folgende Inhalt ein grundlegendes Beispiel zeigen die Schritte bei Verwendung des BLOB-Diensts als Repository für Jenkins CI Artefakte erstellen.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Verwendung den BLOB-Dienst mit Jenkins CI ##

Verwendung des Dienstes Blob Jenkins müssen Sie Azure Storage-Plugin installieren, das Plugin das Speicherkonto verwenden konfigurieren und Erstellen einer Postbuildaktion, mit dem der Buildartefakte Ihres Speicherkontos hochgeladen. Diese Schritte werden in den folgenden Abschnitten beschrieben.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azure Storage-Plugin installieren ##

1. Klicken Sie im Dashboard Jenkins **Jenkins verwalten**.
2. Klicken Sie auf der Seite **Jenkins verwalten** **Plug-Ins verwalten**.
3. Klicken Sie auf die Registerkarte **verfügbar** .
4. Überprüfen Sie im Abschnitt **Artefakt Uploads** **Microsoft Azure Storage-Plugin**.
5. Klicken Sie auf **Installieren ohne Neustart** oder **jetzt downloaden und installieren Sie nach dem Neustart**.
6. Starten Sie Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>So konfigurieren Sie das Azure-Speicher Plugin das Speicherkonto verwenden ##

1. Klicken Sie im Dashboard Jenkins **Jenkins verwalten**.
2. Klicken Sie auf der Seite **Verwalten Jenkins** **System konfigurieren**.
3. Im **Microsoft Azure Storage Konto** Konfigurationsabschnitt:
    1. Geben Sie Ihren speicherkontonamen, die aus dem [Azure-Portal](https://portal.azure.com)erhalten.
    2. Geben Sie Ihr Konto Speicherschlüssel, auch aus dem [Azure-Portal](https://portal.azure.com)erhältlich.
    3. Verwenden Sie den Standardwert für **BLOB-Dienst-Endpunkt-URL** verwenden öffentliche Azure Cloud. Bei Verwendung eine andere Azure-Cloud verwenden Sie den Endpunkt für das Speicherkonto gemäß der [Azure-Portal](https://portal.azure.com) . 
    4. Klicken Sie auf **Überprüfen Speicher Anmeldeinformationen** Überprüfen Ihres Speicherkontos. 
    5. [Optional] Wenn Sie zusätzlichen Speicher-Konten, die Ihre CI Jenkins zur Verfügung gestellt werden sollen verfügen, klicken Sie auf **Weitere Speicherkonten**.
    6. Klicken Sie auf **Speichern** , um die Einstellung zu speichern.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Erstellen eine Postbuildaktion, mit dem der Buildartefakte Ihres Speicherkontos hochgeladen ##

Aus Gründen der Anweisung zuerst müssen wir einen Auftrag erstellen, der erstellen Sie mehrere Dateien, und fügen Sie Postbuildaktion hochzuladenden Dateien an das Speicherkonto.

1. Klicken Sie im Dashboard Jenkins auf **Neues Element**.
2. **Nennen Sie das Projekt **MyJob**und klicken Sie auf **Erstellen eines frei**.**
3. In der Konfiguration im Abschnitt **Erstellen** auf **Buildschritt hinzufügen** , und wählen Sie **Batchbefehl Windows ausführen**.
4. Verwenden Sie die folgenden Befehle in **Befehl**:

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. Im Abschnitt **Aktionen nach dem Erstellen** der Konfiguration auf **Postbuildaktion hinzufügen** , und wählen Sie **Artefakte Azure BLOB-Speicher hochladen**.
6. **Storage-Kontonamen**wählen Sie das Speicherkonto verwenden.
7. **Containernamen**Geben Sie den Containernamen an. (Container wird erstellt, wenn es nicht bereits vorhanden ist, wenn Buildartefakte hochgeladen werden.) Sie können Umgebungsvariablen verwenden, geben Sie also beispielsweise **${JOB_NAME}** als Container.

    **Tipp**
    
    Unter dem **Befehl** führt, ein Skript für **Windows ausführen Batchbefehl** eingegebene, Abschnitt Umgebungsvariablen Jenkins anerkannt. Klicken Sie auf, die Umgebungsvariablen und Beschreibung. Beachten Sie, dass Umgebungsvariablen, die spezielle, wie die Umgebungsvariable **BUILD_URL Zeichen** nicht als Containername oder gemeinsamen virtuellen Pfad.

8. Klicken Sie auf **neuen Container standardmäßig öffentlich** für dieses Beispiel. (Wenn Sie einen privaten Container verwenden möchten, müssen Sie SAS Zugriff erstellen. Das ist nicht Gegenstand dieses Themas. Sie erhalten weitere Informationen zu SAS unter [Verwendung von gemeinsamen Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Optional] Klicken Sie auf **sauberen Container vor dem Hochladen** der Container Inhalt gelöscht werden, bevor der Buildartefakte hochgeladen werden (lassen sie deaktiviert, wenn nicht der Inhalt des Containers bereinigen möchten).
10. **Liste der Artefakte hochzuladen**, geben Sie * *Text /*.txt**.
11. **Gemeinsame virtuelle Pfad für die hochgeladenen Artefakte**für dieses Lernprogramm eingeben **${Erstellen\_ID} / ${erstellen\_Anzahl}**.
12. Klicken Sie auf **Speichern** , um die Einstellung zu speichern.
13. Klicken Sie im Dashboard Jenkins auf **Jetzt erstellen** **MyJob**ausführen. Prüfen Sie die Konsolenausgabe Status Nachrichten für Azure-Speicher in der Konsolenausgabe enthält beginnt nach dem Buildvorgang Buildartefakte hochladen.
14. Nach erfolgreichem Abschluss des Projekts können Sie durch Öffnen des BLOBs für den öffentlichen Buildartefakte untersuchen.
    1. Anmeldung bei [Azure-Portal](https://portal.azure.com).
    2. Klicken Sie auf **Speichern**.
    3. Klicken Sie auf den speicherkontonamen, den für Jenkins verwendet.
    4. Klicken Sie auf **Container**.
    5. Klicken Sie auf den Container mit dem Namen **Myjob**der Kleinbuchstabe den Auftragsnamen, den beim Erstellen von Jenkins Auftrag zugeordnet. Container und BLOB-Namen sind (Groß- und Kleinbuchstaben beachtet) in Azure-Speicher. Liste von Blobs für den Container mit dem Namen **Myjob** erhalten Sie **hello.txt** und **date.txt**. Kopieren Sie die URL für diese Elemente, und in Ihrem Browser geöffnet. Die Datei, die hochgeladen wurde sehen wie ein Artefakt erstellen.

Pro Projekt kann nur einer Postbuildaktion, die Artefakte in Azure BLOB-Speicher hochgeladen erstellt werden. Beachten Sie, dass einzelne Postbuildaktion hochzuladenden Artefakte in Azure BLOB-Speicher (einschließlich Platzhaltern) unterschiedliche Dateien und Pfade zu Dateien in der **Liste der hochzuladenden Artefakte** mit Semikolons als Trennzeichen angeben kann. Wenn Builds Jenkins JAR-Dateien und TXT-Dateien in den Arbeitsbereich **Erstellen** Ordner erzeugt mit Azure BLOB-Speicher hochladen möchten, z. B. folgende **Liste der hochzuladenden Artefakte** Wert: **Erstellen /\*JAR, Build /\*.txt**. Doppelter Doppelpunkt Syntax können Sie einen Pfad im BLOB-Namen angeben. Beispielsweise die Gläser zum Hochladen mit **Binärdateien** in den BLOB-Pfad und TXT-Dateien zum Hochladen **Hinweise** im BLOB-Pfad verwenden, verwenden Sie folgende **Liste der hochzuladenden Artefakte** Wert: **Erstellen /\*. jar::binaries, Build /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Wie einen Buildschritt erstellt, der von Azure BLOB-Speicher heruntergeladen ##

Die folgenden Schritte zeigen einen Buildschritt zum Herunterladen von Elementen von Azure BLOB-Speicher konfigurieren. Dies wäre ggf. im Build gehören beispielsweise Gläser in Azure halten BLOB-Speicher.

1. In der Konfiguration im Abschnitt **Erstellen** auf **Buildschritt hinzufügen** , und wählen Sie **Azure BLOB-Speicher herunterladen**.
2. **Storage-Kontonamen**wählen Sie das Speicherkonto verwenden.
3. **Containernamen**Geben Sie den Namen des Containers mit Blobs, die Sie herunterladen möchten. Sie können Umgebungsvariablen verwenden.
4. **BLOB-Namen**Geben Sie den Blob. Sie können Umgebungsvariablen verwenden. Außerdem können Sie ein Sternchen als Platzhalterzeichen nach dem ersten Buchstaben des blobnamens angeben. Zum Beispiel **Project\* ** geben alle Blobs, deren Namen mit dem **Projekt**beginnen.
5. [Optional] **Downloadpfad**Geben Sie den Pfad auf dem Jenkins Computer Dateien von Azure BLOB-Speicher werden soll. Umgebungsvariablen können auch verwendet werden. (Wenn Sie keinen Wert für **Downloadpfad**angeben, werden Dateien von Azure BLOB-Speicher die Projekt-Arbeitsbereich heruntergeladen werden.)

Haben Sie weitere Elemente von Azure BLOB-Speicher herunterladen möchten, können Sie zusätzliche Schritte erstellen.

Nach dem Ausführen eines Builds können Sie Geschichte Konsole Buildausgabe überprüfen oder betrachten Sie Ihrem Downloadspeicherort, um festzustellen, ob die Blobs erwartet erfolgreich heruntergeladen wurden.  

## <a name="components-used-by-the-blob-service"></a>Der BLOB-Dienst verwendeten Komponenten ##

Der folgende Überblick Komponenten des Blob.

- **Konto**: Zugriff auf Azure Storage erfolgt über ein Speicherkonto. Dies ist die höchste Namespace auf Blobs. Ein Konto kann eine unbegrenzte Anzahl von Containern enthalten, solange die Gesamtgröße unter 100 TB.
- **Container**: Container stellt eine Gruppierung einer Gruppe von Blobs. Alle Blobs müssen in einem Container befinden. Ein Konto kann eine unbegrenzte Anzahl von Containern enthalten. Ein Container kann eine unbegrenzte Anzahl von Blobs speichern.
- **BLOB**: eine Datei von Typ und Größe. Es gibt zwei Arten von Blobs in Azure-Speicher gespeichert werden können: Block und Seite Blobs. Die meisten Dateien sind Block-Blobs. Ein einzelner Blob kann bis zu 200 GB groß sein. Diese praktische Einführung verwendet Block-Blobs. Seitenblobs, anderen BLOB-Typ kann bis zu 1TB und effizienter sind Bereiche von Bytes in einer Datei häufig geändert werden. Weitere Informationen zu Blobs finden Sie unter [Understanding Block Blobs, Anhängen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-Format**: Blobs mit URL angesprochen werden:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (Das obige Format gilt für Öffentliche Azure Cloud. Bei Verwendung eine andere Azure-Cloud verwenden Sie den Endpunkt in der [Azure-Portal](https://portal.azure.com) URL Endpunkt bestimmt.)

    Im obigen Format `storageaccount` steht für den Namen Ihres Speicherkontos `container_name` steht für den Namen des Containers, und `blob_name` bzw. den Namen der BLOB darstellt. In den Containernamen können Sie mehrere Pfade durch einen Schrägstrich getrennt **/**. Der Containername wird in diesem Lernprogramm wurde **MyJob**und **${Erstellen\_ID} / ${erstellen\_Anzahl}** wurde für den gemeinsamen virtuellen Pfad, was das Blob mit einer URL im folgenden Format verwendet:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Nächste Schritte

- [Jenkins erfüllen](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure-Speicher SDK für Java](https://github.com/azure/azure-storage-java)
- [Azure-Speicher Client SDK-Referenz](http://dl.windowsazure.com/storage/javadoc/)
- [Azure-Speicherdienste REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Weitere Informationen finden Sie auch im [Java Developer Center](https://azure.microsoft.com/develop/java/).
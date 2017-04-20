<properties 
    pageTitle="Azure SDK für Java herunterladen" 
    description="Informationen Sie zum Beispielcode für Maven Projekte und grundlegende Installationsschritte für Azure Toolkit für Eclipse Azure SDK für Java, herunterladen." 
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

# <a name="download-the-azure-sdk-for-java"></a>Azure SDK für Java herunterladen

Dieser Artikel enthält eine Anleitung zum Herunterladen und Installieren der Azure-Bibliotheken für Java.

**Hinweis:** Azure-Bibliotheken für Java verteilen unter [Apache-Lizenz, Version 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure Bibliotheken für Java - Handbuch herunterladen

Klicken Sie auf <http://go.microsoft.com/fwlink/?LinkId=690320> herunterladen eine ZIP-Datei enthält alle Bibliotheken und alle abhängigen um Azure Bibliotheken für Java manuell zu installieren.

Nachdem Sie die Zip-Datei auf Ihren Computer heruntergeladen haben, extrahieren Sie den Inhalt, und verwenden Sie eine der folgenden Optionen JAR-Dateien zum Projekt hinzufügen:

* Das Java-Projekt in Eclipse importieren Sie JAR-Dateien.

* Konfigurieren Sie **Erstellen Pfad** für das Java-Projekt in Eclipse den JAR-Dateien einschließen.

Ausführliche Informationen zum Einrichten des Build-Pfade in Eclipse finden Sie [Java erstellen Pfad] Eclipse-Website.

**Hinweis:** Siehe license.txt und ThirdPartyNotices.txt-Datei im ZIP-Lizenz und andere Informationen.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure Bibliotheken für Java - mit Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Schritt 1: Richten Sie Ihr Projekt Maven für Build

Erstellen Sie Maven Projekte in Eclipse, die Java, Azure Bibliotheken verwenden wie im [Einstieg in Azure Management Bibliotheken für Java] [ maven-getting-started] Artikel. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Schritt 2: Konfigurieren Sie Ihre Maven mit der erforderlichen Abhängigkeit

Sobald das Projekt Maven Build verwenden konfiguriert wurde, können die erforderliche Abhängigkeit der pom.xml Datei mit Syntax wie im folgende Beispiel. Beachten Sie, dass nicht jede Abhängigkeit aufgeführt wird im folgenden Beispiel hinzufügen möchten; Sie müssen nur Ihren spezifischen Abhängigkeiten hinzufügen.

> [AZURE.NOTE] In jedem `<version>` Element im folgenden Beispiel ersetzen Sie die Platzhalter "n.n.n" in diesem Beispiel gültige Versionsnummern von [Azure Bibliotheken Repository Maven]gewonnen.

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure Toolkit installieren für Eclipse

Dieser Abschnitt enthält grundlegende Informationen zur Installation von Azure Toolkit für Eclipse; Detaillierte Informationen finden Sie in der [Azure-Toolkit für Eclipse installieren].

### <a name="prerequisites"></a>Erforderliche Komponenten

1. Windows operating Systems im Artikel [Was ist neu in der Azure-Toolkit für Eclipse] aufgeführt.
1. Macintosh oder Linux operating Systems im Artikel [Was ist neu in der Azure-Toolkit für Eclipse] aufgeführt.
1. IDE für Java EE Entwickler Eclipse Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.

### <a name="basic-installation-steps"></a>Grundlegende Installationsschritte

1. In Eclipse im **Hilfemenü** wählen Sie **Neue Software installieren**.
1. Geben Sie Website Position <http://dl.microsoft.com/eclipse> und drücken Sie die **EINGABETASTE**.
1. Wählen Sie die Elemente und klicken Sie auf **Fertig stellen**.

Azure-Toolkit für Eclipse verwendet die neueste Version von Azure SDK. Dadurch kann <http://go.microsoft.com/fwlink/?LinkID=252838>mit dem Webplattform-Installer (WebPI) heruntergeladen werden. Wenn nicht installiert ist, beim Erstellen Ihrer ersten Azure Bereitstellungsprojekts Azure Toolkit für Eclipse automatisch die entsprechende Version von Azure SDK installiert.

## <a name="see-also"></a>Siehe auch

[Azure-Toolkit für Eclipse]

[Azure Toolkit installieren für Eclipse] 

[Erstellen eine Hello World-Anwendung für Azure in Eclipse]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Maven-Repository Azure Bibliotheken]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java-Erstellungspfad]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Neuigkeiten in Azure Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333

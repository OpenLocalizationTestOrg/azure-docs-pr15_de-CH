<properties
    pageTitle="Installieren von Azure Toolkit für Eclipse | Microsoft Azure"
    description="Informationen Sie zum Azure-Toolkit für Eclipse installieren."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure Toolkit installieren für Eclipse

Azure-Toolkit für Eclipse bietet Vorlagen und, können Sie problemlos erstellen, entwickeln, testen und Bereitstellen von Azure Applications mithilfe des Eclipse Development Environment. Azure-Toolkit für Eclipse ist ein Open Source-Projekt, dessen Quellcode Lizenz MIT der Projektsite auf GitHub unter der folgenden URL verfügbar ist:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Die folgenden Schritte zeigen das Azure-Toolkit für Eclipse installieren.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Azure-Toolkit für Eclipse installieren

1. Starten Sie Eclipse.

1. Wenn Eclipse geöffnet wird, klicken Sie im Menü **Hilfe** und klicken Sie auf **Neue Software installieren**, wie in der folgenden Abbildung dargestellt.

    ![Azure Toolkit installieren für Eclipse][01]

1. Geben Sie im Dialogfeld **Verfügbare Software** im Textfeld **mit** **http://dl.microsoft.com/eclipse** gefolgt von **der EINGABETASTE** .

1. Überprüfen Sie im Bereich **Name** **Azure Toolkit für Eclipse**, und deaktivieren Sie **alle Websites während update installieren erforderlichen Software finden**. Die sollte ähnlich der folgenden angezeigt:

    ![Azure Toolkit installieren für Eclipse][02]

1. Wenn Sie **Azure-Toolkit für Eclipse**erweitern, sehen Sie Folgendes:

    * **Anwendung Einblicke Plugin für Java**: Diese Komponente ermöglicht Azure Telemetrie Protokollierung und Analyse für Ihre Anwendung und Server-Instanzen verwenden.
    * **Azure Access Control Services Filter**: Diese Komponente unterstützt Authentifizierung von Benutzern mit Azure ACS Szenarien einzelne anmelden und das externalisieren Identität Logik aus der Anwendung.
    * **Allgemeine Azure-Plugin**: Diese Komponente stellt die allgemeine Funktionen von anderen Komponenten Toolkit benötigt.
    * **Azure Explorer für Eclipse**: Diese Komponente stellt die allgemeine Funktionen von anderen Komponenten Toolkit benötigt.
    * **Azure-Plugin für Eclipse mit Java**: Diese Komponente bietet Unterstützung für die Entwicklung von Projekten erstellen, testen und Bereitstellen von Java Applikationen für Microsoft Azure Cloud in Eclipse und über die Befehlszeile.
    * **Azure Web Apps-Plugin mit Java**: Diese Komponente unterstützt Java Web Applications Microsoft Azure Web App Container bereitstellen.
    * **Microsoft JDBC Treiber 4.2 für SQL Server**: Diese Komponente bietet JDBC API für Java Platform Enterprise Edition 8 für SQL Server und Microsoft Azure SQL-Datenbank.
    * **Paket für Apache Qpid Clientbibliotheken für JMS**: Diese Komponente stellt die JMS-Clientkomponente von Apache Qpid Projekt die Anwendung AMQP in Microsoft Azure messaging aktivieren.
    * **Paket für Microsoft Azure Bibliotheken für Java**: Diese Komponente stellt APIs für den Zugriff auf Microsoft Azure Services wie Speicher, Servicebus, Service-Laufzeit.

1. Klicken Sie auf **Weiter**. (Wenn Sie ungewöhnliche Verzögerungen bei der Installation des Toolkits, sicherstellen Sie, dass **alle Websites während update Installieren erforderlicher Software zu** deaktiviert.)

1. Klicken Sie im Dialogfeld **Details installieren** auf **Weiter**.

    ![Einzelheiten der Installation][03]

1. Überprüfen Sie im Dialogfeld **Lizenzen überprüfen** die Begriffe der Lizenzverträge. Wenn Sie den Lizenzvertrag zustimmen, klicken Sie auf **den Lizenzvertrag annehmen** , und klicken Sie dann auf **Fertig stellen**. (Die verbleibenden Schritte angenommen Zahlungsbedingungen die Lizenzverträge akzeptieren. Wenn Sie Begriffe im Lizenzvertrag nicht akzeptieren, beenden Sie den Installationsvorgang.)

    ![Lizenzen überprüfen][04]

    Eclipse downloaden und installieren Sie die erforderlichen Pakete.

    ![Installationsstatus][05]

1. Wenn Eclipse zum Abschließen der Installation neu zu starten, klicken Sie auf **Ja**.

    ![Prompt starten][06]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zu Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für Eclipse]
  - *Installieren von Azure Toolkit für Eclipse (dieser Artikel)*
  - [Erstellen Sie Hello World Web App für Azure in Eclipse]
  - [Neuigkeiten in Azure Toolkit für Eclipse]
- [Azure-Toolkit für IntelliJ]
  - [Installieren von Azure Toolkit für IntelliJ]
  - [Erstellen Sie eine Hello World Web App für Azure in IntelliJ]
  - [Neuigkeiten in Azure Toolkit für IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen Sie Hello World Web App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen Sie eine Hello World Web App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren von Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Neuigkeiten in Azure Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuigkeiten in Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png


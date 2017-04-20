<properties
    pageTitle="Neuigkeiten in Azure Toolkit für IntelliJ | Microsoft Azure"
    description="IntelliJ erfahren Sie mehr über die neuesten Funktionen der Azure-Toolkit."
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
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Neuigkeiten in Azure Toolkit für IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure Toolkit IntelliJ Versionen

Dieser Artikel enthält Informationen über die verschiedenen Versionen und Aktualisierungen der Azure-Toolkit für IntelliJ.

> [AZURE.NOTE] Es gibt auch ein Azure-Toolkit für die Eclipse-IDE. Weitere Informationen finden Sie unter [Azure-Toolkit für Eclipse].

### <a name="august-26-2016"></a>26 August 2016

Azure-Toolkit für IntelliJ - August 2016 Version umfasst die folgenden Funktionen:

* **Benutzerdefinierte JDK-Distributionen**. Azure-Toolkit für IntelliJ unterstützt jetzt angeben und eine beliebige JDK-Version Ihre Azure WebApp Container bereitstellen:
  - Neben den JDKs von Azure bereitgestellt können Sie auch Auswahl Zulu OpenJDK Versionen auf Azure durch Azul zur Verfügung gestellt.
  - Sie können auch eigene JDK Verteilung als ZIP-Datei an das Speicherkonto hochladen.
* **Verbesserte Azure Explorer-Ansicht**:
  - Unterstützung für Virtual Machine Azure neue Ressourcenmanager Modell: auflisten, erstellen und Löschen von Resource Manager-basierten virtuellen Maschinen ohne die IDE.
  - Unterstützung für das Speicherkonto BLOB-Management mit Azure Ressourcen-Manager die Funktionalität für die Verwaltung der "klassischen" Speicherkonten ergänzt.
* **Microsoft JDBC-Treiber für SQL Server 6.0**. Dieses Update enthält den neuesten JDBC-Treiber für Microsoft SQL Server (6.0) ist jetzt als Bibliothek, die Ihre Java-Projekte problemlos hinzugefügt werden können, damit die ältere Version ersetzen.

### <a name="june-29-2016"></a>Am 29. Juni 2016

Azure-Toolkit für IntelliJ - Juni 2016 Version umfasst die folgenden Funktionen:

* **Java 8 erforderlich**. Azure-Toolkit für IntelliJ müssen Java 8, obwohl diese Anforderung nur für das Toolkit gilt-Anwendung mithilfe von Azure unterstützt alle Versionen von Java fortfahren können.
* **Unterstützung für die neuesten Java-JDKs**. Die neuesten Versionen der Java-JDKs werden jetzt von Azure-Toolkit für IntelliJ unterstützt.
* **Unterstützung für Azure SDK v2.9.1**. Die neueste Version von Azure SDK ist jetzt mindestens Voraussetzung für die Azure für IntelliJ.
* **Integrierte Beispiele**. Azure-Toolkit für IntelliJ verfügt über verschiedene Anwendungsbeispiele Entwickler beginnen können.
* **Integration von HDInsight-Tools**. Azure HDInsight Tools sind jetzt mit der Azure-Toolkit für IntelliJ gebündelt. Weitere Informationen finden Sie unter [HDInsight Tools Plugin für IntelliJ].
* **Remotedebuggen Java Web Apps**. Azure-Toolkit für IntelliJ unterstützt jetzt das Remotedebuggen Java webapps in Azure App Service.

### <a name="april-12-2016"></a>12 April 2016

Azure-Toolkit für IntelliJ - April 2016 Version umfasst die folgenden Funktionen:

* **Unterstützung für Azure SDK v2.9.0**. Die neueste Version von Azure SDK ist jetzt mindestens Voraussetzung für die Azure für IntelliJ.
* **Sonstige Usability, Reaktionsfähigkeit und Leistung Verbesserungen Azure Web App Support**. Eine Reihe von Performance-Optimierungen in Azure Ergebnis wie das Toolkit kommuniziert reaktionsfähigere Benutzeroberfläche.
* **Möglichkeit, eine vorhandene Anwendung in Azure in IntelliJ wirklich**. Azure-Toolkit für IntelliJ ermöglicht das Löschen eines vorhandenen Containers Azure Web ohne IntelliJ.

## <a name="see-also"></a>Siehe auch ##

Weitere Informationen zu Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für Eclipse]
  - [Azure Toolkit installieren für Eclipse]
  - [Erstellen Sie Hello World Web App für Azure in Eclipse]
  - [Neuigkeiten in Azure Toolkit für Eclipse]
- [Azure-Toolkit für IntelliJ]
  - [Installieren von Azure Toolkit für IntelliJ]
  - [Erstellen Sie eine Hello World Web App für Azure in IntelliJ]
  - *Neuigkeiten in Azure Toolkit für IntelliJ (dieser Artikel)*

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen Sie Hello World Web App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen Sie eine Hello World Web App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Azure Toolkit installieren für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren von Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Neuigkeiten in Azure Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight Tools Plugin für IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md

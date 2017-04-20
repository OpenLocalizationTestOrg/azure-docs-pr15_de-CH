<properties
   pageTitle="Übersicht über See Datenspeicher Azure | Microsoft Azure"
   description="Verstehen Sie, was Azure See Datenspeicher und der Nutzen für andere Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Übersicht über Azure See Datenspeicher

Azure See Datenspeicher ist eine unternehmensweite Großkonzerne Repository für analytische Datenverlustvorfalls Arbeitslasten. Azure Data Lake können Sie jeder Größe, Typ und Aufnahme Geschwindigkeit an einem einzigen Ort für Betriebs- und unsystematische Analytics Datenerfassung.

> [AZURE.TIP] Mit der [See Datenspeicher Lernpfad](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) Azure Datenspeicher See Dienst erkunden.

Azure See Datenspeicher möglich von Hadoop (verfügbar mit HDInsight) mit APIs REST WebHDFS kompatibel. Es ist speziell für Analysen auf die Daten ermöglichen und Leistung für Analytics Szenarien optimiert. Standardmäßig enthält es alle Funktionen der Unternehmensklasse, Sicherheit, Verwaltung, Skalierung, Zuverlässigkeit und Verfügbarkeit – für Unternehmen Anwendungsfälle.


![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Im folgenden sind einige der wichtigsten Funktionen von Azure Data Lake.

### <a name="built-for-hadoop"></a>Speziell für Hadoop

Azure Data Lake-Speicher ist ein Apache Hadoop kompatibel mit Hadoop-Ökosystem mit Hadoop verteilt Datei System bietet.  Die vorhandenen HDInsight Applikationen oder Services, die die WebHDFS-API verwenden können mit See Datenspeicher integrieren. Datenspeicher See macht auch eine WebHDFS-kompatible REST-Schnittstelle für Applikationen

Daten im Datenspeicher See können problemlos mit analytischen Rahmen Hadoop MapReduce oder Struktur analysiert werden. Microsoft Azure HDInsight-Cluster können bereitgestellt und konfiguriert und Daten im Datenspeicher See direkt zugreifen.

### <a name="unlimited-storage-petabyte-files"></a>Unbegrenzte Speicherung Petabyte-Dateien

Azure See Datenspeicher bietet unbegrenzte Speicherung und eignet sich für eine Vielzahl von Daten für Analyse speichern. Es ist nicht beschränkt alle Konto-Größen, Dateigröße oder die Datenmenge in einem See Daten gespeichert werden können. Einzelne Dateien reichen von Kilobyte Petabyte Größe eine hervorragende Wahl Daten speichern. Daten dauerhaft, indem mehrere Kopien und es gibt keine Begrenzung für die Dauer, die Daten im See Daten gespeichert werden können.

### <a name="performance-tuned-for-big-data-analytics"></a>Big Data Analytics optimiert Leistung

Azure See Datenspeicher wird erstellt, zum Ausführen von großen analytischen Systeme, die massive Durchsatz Abfragen und Analysieren große Datenmengen erfordern. Daten-See verbreitet Teile einer Datei über eine Anzahl von einzelnen Speicherserver. Dies verbessert den Lesedurchsatz beim Lesen der Datei parallel zur Durchführung von Datenanalysen.


### <a name="enterprise-ready-highly-available-and-secure"></a>Unternehmenstaugliche: Hoher Verfügbarkeit und

Azure See Datenspeicher bietet Industriestandard-Verfügbarkeit und Zuverlässigkeit. Redundante Kopien gegen unerwartete Ausfälle werden Ihrer Datenbestände dauerhaft gespeichert. Unternehmen können Azure Data Lake in ihre Projektmappen als Bestandteil ihrer vorhandenen Plattform.

See Datenspeicher bietet auch unternehmensweite Sicherheit für gespeicherten Daten. Weitere Informationen finden Sie unter [Sichern von Daten in Azure See Datenspeicher](#DataLakeStoreSecurity).


### <a name="all-data"></a>Alle Daten

Azure See Datenspeicher speichert Daten in ihrem nativen Format ist, ohne vorherigen Transformationen. Einzelne analytischen Rahmen zu interpretieren die Daten definieren, ein Schema zum Zeitpunkt der Analyse überlassen, See Datenspeicher keine Schema vor dem Laden der Daten erforderlich. Dateien mit beliebigen Größen und Formaten speichern ermöglicht See Datenspeicher strukturierten, halbstrukturierten und unstrukturierten Daten.

Azure See Datenspeicher Container für Daten sind im Wesentlichen Ordner und Dateien. Arbeiten Sie mit Daten über SDKs, Azure-Portal und Azure Powershell. Die Daten in den Speicher diese Schnittstellen und den entsprechenden Container einfügen, können Sie beliebige Daten speichern. See Datenspeicher führt keine keine besondere Behandlung der Daten basierend auf den Daten, die darauf gespeichert.


## <a name="DataLakeStoreSecurity"></a>Sichern von Daten in Azure See Datenspeicher

Azure See Datenspeicher Azure Active Directory für die Authentifizierung verwendet und Zugriffssteuerungslisten (ACLs) Zugriff auf Ihre Daten verwalten.

| Funktion                                 | Beschreibung                              |
|-----------------------------------------|------------------------------------------|
| Authentifizierung | Azure See Datenspeicher integriert mit Azure Active Directory (AAD) für Identitäts- und Zugriffsmanagement für alle Daten, die in Azure See Datenspeicher gespeichert. Durch die Integration Azure Data Lake profitieren alle AAD Features wie mehrstufige Authentifizierung, Zugangskontrolle, rollenbasierte Zugriffskontrolle, Überwachung der Verwendung, Sicherheit, Überwachung und Alarmierung, usw. Azure See Datenspeicher unterstützt das OAuth 2.0-Protokoll für die Authentifizierung in der REST-Schnittstelle. |
| Zugriffskontrolle                          | Azure See Datenspeicher Zugriffskontrolle POSIX-Berechtigungen verfügbar gemachte WebHDFS-Protokoll unterstützt. In der aktuellen Version können Zugriffssteuerungslisten auf den Stammordner, Unterordner sowie einzelne Dateien aktiviert werden. Die ACLs in den Stammordner gelten werden auch für alle untergeordneten Ordner/Dateien sowie.|

Möchten Sie mehr über das Sichern von Daten im Datenspeicher See. Die stehenden Sie unten Links.

* Informationen zu im Datenspeicher See finden Sie unter [Sichern von Daten in Azure See Datenspeicher](data-lake-store-secure-data.md).
* Ziehen Sie Videos? [In diesem Video](https://mix.office.com/watch/1q2mgzh9nn5lx) auf Daten im Datenspeicher See schützen.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Applikationen mit Azure See Datenspeicher

Azure See Datenspeicher ist offen Quellkomponenten Hadoop-Ökosystem vereinbar. Außerdem integriert gut mit anderen Azure. Dadurch See Datenspeicher ideal für Ihren Bedürfnissen Speicher. Folgenden Sie unter den Links erfahren wie See Datenspeicher mit open Source-Komponenten sowie andere Azure verwendet werden kann.

* Finden Sie eine Liste mit See Datenspeicher open-Source-Anwendung [auch mit Azure See Datenspeicher](data-lake-store-compatible-oss-other-applications.md) .
* Finden Sie unter [Integration mit anderen Azure](data-lake-store-integrate-with-other-services.md) zu verstehen wie See Datenspeicher mit anderen Azure Services ermöglichen einen größeren Bereich von Szenarios verwendet werden kann.
* Finden Sie unter [Szenarien für See Datenspeicher mit](data-lake-store-data-scenarios.md) Informationen zum Datenspeicher See in Szenarien wie Einnahme Daten, Daten, Daten heruntergeladen und Visualisieren von Daten verwenden.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Was Azure See Datenspeicher ist (Adl: / /)?

Datenspeicher See erfolgt über das neue Dateisystem der AzureDataLakeFilesystem (Adl: / /), Hadoop Umgebung (verfügbar mit HDInsight-Cluster). Programme und Dienste, die Adl: / sind weitere Performance-Optimierung, die nicht derzeit in WebHDFS nutzen. Daher See Datenspeicher können Sie entweder die Leistung der empfohlene Möglichkeit Adl nutzen: / / oder vorhandenen Code weiterhin die WebHDFS-API direkt verwenden. Azure HDInsight nutzt vollständig die AzureDataLakeFilesystem bieten die beste Leistung auf See Datenspeicher.

Die Daten in der See Datenspeicher zugreifen `adl://<data_lake_store_name>.azuredatalakestore.net`. Weitere Informationen zum Zugriff auf die Daten im Datenspeicher See finden Sie unter [Eigenschaften der gespeicherten Daten anzeigen](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Wie benutze ich Azure See Datenspeicher?

Finden Sie in [Erste Schritte mit dem Datenspeicher mithilfe der Azure-Portal](data-lake-store-get-started-portal.md), einen See Datenspeicher Azure-Portal bereitstellen. Nach der Bereitstellung von Azure Data Lake lernen See Datenspeicher Datenverlustvorfalls Angebote Azure Data Lake Analytics oder Azure HDInsight mit Sie. Sie können auch eine Anwendung .NET, Azure See Datenspeicher registrieren und Operationen wie Upload-Daten herunterladen usw. erstellen.

- [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Erste Schritte mit Azure See Datenspeicher mit .NET SDK](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Datenspeicher See videos

Auf Wunsch ansehen von Videos lernen bietet See Datenspeicher Videos an Funktionen.

* [Erstellen Sie ein Konto Azure See Datenspeicher](https://mix.office.com/watch/1k1cycy4l4gen)
* [Verwenden Sie Daten-Explorer, um Daten in Azure See Datenspeicher verwalten](https://mix.office.com/watch/icletrxrh6pc)
* [Azure Data Lake Analytics Azure See Datenspeicher herstellen](https://mix.office.com/watch/qwji0dc9rx9k)
* [Zugriff auf Azure See Datenspeicher über See Datenanalyse](https://mix.office.com/watch/1n0s45up381a8)
* [Azure HDInsight Azure See Datenspeicher herstellen](https://mix.office.com/watch/l93xri2yhtp2)
* [Zugriff auf Azure See Datenspeicher Struktur und Schweine](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Verwenden Sie DistCp (Hadoop verteilt kopieren) Kopieren von Daten und von Azure See Datenspeicher](https://mix.office.com/watch/1liuojvdx6sie)
* [Verwenden Sie zum Verschieben von Daten zwischen relationalen und Azure See Datenspeicher Apache Sqoop](https://mix.office.com/watch/1butcdjxmu114)
* [Daten Orchestrierung Azure Data Factory für Azure See Datenspeicher](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Sichern von Daten im Datenspeicher See Azure](https://mix.office.com/watch/1q2mgzh9nn5lx)




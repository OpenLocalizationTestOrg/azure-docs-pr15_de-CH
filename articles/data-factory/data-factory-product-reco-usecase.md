<properties 
    pageTitle="Daten mit Fall - Produkten" 
    description="Lernen Sie ein Anwendungsfall zusammen mit anderen Diensten mithilfe von Azure Data Factory implementiert." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Anwendungsfall - Produkten 

Azure Data Factory ist einer der vielen Dienste Cortana Intelligence Suite von Solution Accelerators implementiert.  Siehe [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) Seite Weitere Informationen zu dieser Version. Dieses Dokument beschreibt einen allgemeine Anwendungsfall, den Azure Benutzer bereits gelöst und mit Azure Data Factory und andere Cortana Intelligence Komponentendienste implementiert.

## <a name="scenario"></a>Szenario

Online-Händler möchten häufig ihre Kunden kaufen Produkte mit Produkten sind sie wahrscheinlich interessiert und daher wahrscheinlich kaufen. Online-Händler müssen dazu Online-Erfahrung des Benutzers mithilfe von personalisierten Produkten für diesen bestimmten Benutzer anpassen. Diese persönliche Vorschläge werden basierend auf ihrer aktuellen und vergangenen shopping Daten, Informationen, neu eingeführten Marken und Produkt- und Segmentierungsdaten vorgenommen werden.  Darüber hinaus bieten sie Benutzer Produkt eingeführter Analyse insgesamt Nutzungsverhalten aller ihrer Benutzer kombiniert.

Dieser Einzelhändler soll für Benutzer auf Verkauf Umwandlungen optimieren und höhere Umsätze erzielen.  Sie erreichen diese Konvertierung mit kontextbezogenen, Verhalten-basierte eingeführter Interessen und Aktivitäten. Für diesen Fall verwenden wir Onlineeinzelhändlern als Beispiel für Unternehmen, die für ihre Kunden optimieren möchten. Diese Prinzipien gelten jedoch für Unternehmen, die zu seiner Kunden waren und Dienstleistungen ihrer Kunden Einkaufserlebnis mit personalisierten Produkten wünscht.

## <a name="challenges"></a>Herausforderung

Gibt es viele, Onlineeinzelhändlern Gesicht beim Implementieren dieser Anwendungsfall. 

Erstens müssen Daten von Größen und Formen aus mehreren Datenquellen aufgenommen sowohl lokal und in der Cloud. Zu diesen Daten gehören Produktdaten, historisch Kundendaten Verhalten und Daten wie Benutzer online Retail-Site durchsucht. 

Produkten Zweitens personalisierte müssen vernünftigerweise genau berechnet und vorhergesagt werden. Produkt, Marke und Verhalten und Browser Daten müssen Online-Händler auch Kundenfeedback Käufe auf die Bestimmung der empfohlenen Produkte für den Benutzer. 

Empfehlung muss schließlich dem Benutzer eine nahtlose durchsuchen und Einkaufserlebnis und aktuelle und relevante Vorschläge sofort lieferbar. 

Schließlich müssen Einzelhändler messen die Effektivität der Ansatz verfolgen insgesamt Up-Selling und Cross-Selling auf Konvertierung sales-Erfolge und ihre zukünftige Recommendations anpassen.

## <a name="solution-overview"></a>Lösungsübersicht

Dieses Anwendungsbeispiel wurde gelöst und real Azure Benutzer mithilfe von Azure Data Factory und andere Cortana Intelligence Komponentendienste, [HDInsight](https://azure.microsoft.com/services/hdinsight/) und [Power BI](https://powerbi.microsoft.com/)implementiert.

Der Onlinehändler verwendet einen Azure BLOB-Speicher, einer lokalen SQL Server Azure SQL DB und relational Datamart als Speicheroptionen für die Daten im Workflow.  Blob-Speicher enthält Kundeninformationen Verhalten Kundendaten und Produkt-Daten. Produkt Marke Produktdaten enthält und ein Produktkatalog gespeicherte lokalen in einem SQL Datawarehouse. 

Alle Daten wird kombiniert und in ein empfehlungssystem zu personalisierten eingeführter Interessen und Aktivitäten, während der Benutzer Produkte im Katalog auf der Website navigiert. Kunden Siehe auch Produkte, die auf das Produkt beziehen, die, denen Sie suchen, auf allgemeine Website basiert, die nicht für jeden Benutzer.

![Anwendungsfalldiagramm](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabyte unformatierten Webprotokolldateien werden täglich von Online-Händler-Website als teilweise strukturierten Dateien generiert. Die unformatierten Webprotokolldateien und Kataloginformationen Kunden- und Produktinformationen eingenommen regelmäßig in einer Azure Blob-Speicher mit Data Factory Global bereitgestellte Datentransfer als Dienst. Die unformatierten Protokolldateien für den Tag werden im BLOB-Speicher zur Archivierung (nach Jahr und Monat) partitioniert.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) zum Partitionieren von rohen Protokolldateien im Blob-Speicher und die aufgenommenen Protokolle Ebene mit Struktur und Schwein Skripts. Die partitionierte Webprotokolle Daten zum Extrahieren der erforderlichen Eingaben für einen Computer Lernsystem Empfehlung empfohlenen personalisierten Produkt generiert dann verarbeitet.

Das empfehlungssystem Computer lernen in diesem Beispiel ist eine open-Source Empfehlung Plattform von [Apache Mahout](http://mahout.apache.org/).  Das Szenario kann jede [Azure maschinelles lernen](https://azure.microsoft.com/services/machine-learning/) oder benutzerdefinierte Modell angewendet werden.  Mahout-Modell wird die Ähnlichkeit zwischen Elementen auf der Website basierend auf allgemeinen Vorhersagen und personalisierte eingeführter für einzelne Benutzer zu verwendet.

Schließlich wird das Resultset der personalisierten Produkten in relational Datamart für Verbrauch Einzelhändler Website verschoben.  Die Ergebnismenge konnte von BLOB-Speicher direkt von einer anderen Anwendung zugegriffen, oder für andere Verbraucher und Anwendungsfälle in zusätzlichen Speicher verschoben.

## <a name="benefits"></a>Vorteile

Optimieren ihrer Empfehlung Produkt Unternehmensziele ausrichten und erfüllt die Lösung online-Einzelhandelsunternehmen Merchandising- und marketing-Ziele. Darüber hinaus konnten sie sich durchsetzen und die Empfehlung Abläufe eine effiziente, zuverlässige und kostengünstige Weise. Der Ansatz erleichtert das Modell aktualisieren und die Wirksamkeit der Maßnahmen auf Konvertierung Verkaufserfolge optimieren. Mithilfe von Azure Data Factory konnten sie ihre Zeit- und manuelle Cloud-Ressourcenmanagement und Ressourcenmanagement bei Bedarf Cloud verschieben. Daher konnten sie sparen Zeit, Geld und ihre Lösung verkürzen. Linie Datenansichten und betrieblichen Service Health wurde einfach visualisieren und intuitive Data Factory Überwachung und Verwaltung von Azure-Portal Benutzeroberfläche beheben. Die Lösung kann, damit fertig Daten zuverlässig erzeugt und an Benutzer und Daten und Verarbeitung Abhängigkeiten automatisch ohne menschlichen Eingriff verwaltet nun geplant und verwaltet werden.

Personalisierte Einkaufserlebnis, Wettbewerbsfähigkeit, interessante Kunden erstellt Onlinehändler auftreten und daher Vertriebs- und allgemeine Kundenzufriedenheit erhöhen.



  
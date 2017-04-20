Ressource|Standardlimit
---|---
Anzahl der Speicherkonten pro Abonnement|200<sup>1</sup>
TB pro Konto|500 TB
Maximale Anzahl von Blob-Container, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Nachrichten pro Konto|Nur beträgt 500 TB Speicherkapazität Konto
Max. Größe eines einzelnen Blob-Container, Tabelle oder Warteschlange|500 TB
Maximale Anzahl von Blöcken in ein oder Blob Anfügen|50.000
Maximale Größe eines Blocks in ein oder Blob Anfügen|4 MB
Maximale Größe ein, oder fügen Sie blob|50.000 x 4 MB (ca. 195 GB) 
Max. Größe eines Seitenblob |1 TB
Max. Größe einer Tabelle Entität|1 MB
Max. Anzahl von Eigenschaften in einer Tabellenentität|252
Maximale Größe einer Nachricht in einer Warteschlange|64 KB
Max. Größe einer Dateifreigabe|5 TB
Maximale Größe einer Datei in einer Dateifreigabe|1 TB
Maximale Anzahl von Dateien in einem freigegebenen Ordner|Nur ist 5 TB Gesamtkapazität der Dateifreigabe
Max. 8 KB IOPS pro Aktie|1000
Maximale Anzahl von Dateien in einem freigegebenen Ordner|Nur ist 5 TB Gesamtkapazität der Dateifreigabe
Maximale Anzahl von Blob-Container, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Nachrichten pro Konto|Nur beträgt 500 TB Speicherkapazität Konto
Maximale Anzahl von gespeicherten Richtlinien pro Container, Dateifreigabe, Tabelle oder Warteschlange|5
Anforderung Gesamtrate (1KB Größe vorausgesetzt) pro Konto|Bis zu 20.000 IOPS Entitäten pro Sekunde oder Nachrichten pro Sekunde
Zieldurchsatz für einzelnes blob|Bis zu 60 MB pro zweite oder bis zu 500 Anfragen pro Sekunde
Zieldurchsatz für einzelne Warteschlange (1 KB Nachrichten)|Bis zu 2000 Nachrichten pro Sekunde
Zieldurchsatz für einzelne Tabellenpartition (1 KB Entitäten)|Bis zu 2000 Elemente pro Sekunde
Zieldurchsatz für Dateifreigabe auf einem|Bis zu 60 MB pro Sekunde
Max. Eindringen<sup>2</sup> pro Konto (uns Bereiche)|10 Gbit/s, g-ZRS<sup>3</sup> aktiviert, 20 Gbit/s für LRS
Max. Ausgang<sup>2</sup> pro Konto (uns Bereiche)|20 Gbit/s aktivierter RA-GRS/g/ZRS<sup>3</sup> , 30 Gbit/s für LRS
Max. Eindringen<sup>2</sup> pro Speicherkonto (Europäische und asiatische Regionen)|5 Gbps GRS-ZRS<sup>3</sup> aktiviert 10 Gbit/s für LRS
Max. Ausgang<sup>2</sup> pro Speicherkonto (Europäische und asiatische Regionen)|10 Gbit/s RA-GRS/g/ZRS<sup>3</sup> aktiviert 15 Gbit/s für LRS

<sup>1</sup> Dazu gehören Standard- und Premium-Speicherkonten. Benötigen Sie mehr als 200 Speicherkonten können Sie [Azure](https://azure.microsoft.com/support/faq/)unterstützt. Azure Storage-Team wird Ihre beurteilen und kann bis zu 250 Speicherkonten genehmigen. 

<sup>2</sup> *Eindringen* bezieht sich auf alle Daten (Anfragen) an ein Speicherkonto. *Ausgang* bezieht sich auf alle Daten (Antworten) von ein Speicherkonto.  

<sup>3</sup> Azure Optionen Storage Replication:

- **RA-GRS**: Lesezugriff Geo redundanten Speicher. Bei aktiviertem RA-GRS sind Ausgang Ziele für sekundären Standort für den primären Standort identisch.
- **G**: Geo-redundanten Speicher. 
- **ZRS**: Zone redundanten Speicher. Verfügbar nur für Block-Blobs. 
- **LRS**: lokal redundanter Speicher. 


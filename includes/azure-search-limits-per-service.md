Speicher ist Festplattenspeicher oder einen Grenzwert für die *maximale Anzahl* von Indizes oder Dokumente beschränkt, ausstellt. 

Ressource|Frei|Grundlegende|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Vereinbarung zum Servicelevel (SLA)|Nr. <sup>1</sup> |Ja |Ja  |Ja |Ja |Ja
Pro partition|50 MB |2 GB|25 GB|100 GB|200 GB|200 GB
Partitionen pro Dienst|N/A|1|12|12|12|3
Größe der Partition|N/A|2 GB|25 GB|100 GB|200 GB |200 GB
Replikate|N/A|1|12|12|12|12
Maximale Indizes|3|5|50|200|200|1000 pro Partition oder 3000 pro Dienst
Maximale Dokumente|10.000|1 million|15 Millionen pro Partition oder 180 Millionen pro Dienst |60 Millionen pro Partition oder 720 Millionen pro Dienst |120 Millionen pro Partition oder 1,4 Mrd. pro Dienst|1 Million pro Index oder 200 Millionen pro partition |
Geschätzte Abfragen pro Sekunde (QPS)|N/A|3 pro Replikat|~ 15 pro Replikat|60 pro Replikat|60 pro Replikat|> 60 pro Replikat

<sup>1</sup> frei und Vorschau SKUs nicht mit Service Level Agreements (SLAs) kommen. SLAs werden erzwungen, sobald eine SKU erhältlich ist.
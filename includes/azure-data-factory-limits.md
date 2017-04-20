Data Factory ist ein Multi-Tenant-Dienst, der hat die folgenden vorgegebenen Grenzwerte um sicherzustellen, dass die Arbeitslasten Kundenabonnements geschützt sind. Viele der Grenzen können problemlos für Ihr Abonnement bis die maximale ausgelöst werden, vom Kundendienst. 

**Ressource** | **Standardlimit** | **Maximale Anzahl**
-------- | ------------- | -------------
Daten-Factorys in Azure-Abonnement | 50 | [Kontakt zum support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Rohrleitungen in eine Data factory | 2500 | [Kontakt zum support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Datensätze in einer Data factory | 5000 | [Kontakt zum support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
parallele Segmente pro Datensatz | 10 | 10
Bytes pro Objekt für Pipeline Objekte <sup>1</sup> | 200 KB | 2.000 KB
Bytes pro Objekt Dataset und verknüpften Serviceartikel Objekte <sup>1</sup> | 100 KB | 2.000 KB
HDInsight bei Bedarf Cluster Kerne in einem Abonnement <sup>2</sup> | 48 | [Kontakt zum support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Cloud-Daten Bewegung Einheit <sup>3</sup> | 8 | [Kontakt zum support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Anzahl der Wiederholungsversuche für Pipeline Aktivität ausgeführt wird | 1000 | MaxInt (32 Bit)

<sup>1</sup> Pipeline Dataset und verknüpfte-Objekte repräsentieren eine logische Gruppierung Ihrer Arbeit. Grenzwerte für diese Objekte beziehen Datenmenge sich nicht verschieben und Azure Data Factory-Dienst verarbeitet. Data Factory soll skalierbar auf Petabyte Daten behandeln.

<sup>2</sup> bei Bedarf HDInsight Kerne werden aus dem Abonnement zugewiesen, die Werk Daten enthält. Daher ist die obige Beschränkung der Core Grenzwert bei Bedarf HDInsight Kerne erzwungen und von Core Grenzwert der Azure-Abonnement zugeordnet.

<sup>3</sup> Cloud Bewegung Dateneinheit (DMU) wird in einem Kopiervorgang Cloud Cloud verwendet. Es ist eine Maßnahme, die Stromversorgung (eine Kombination aus CPU, Speicher und Netzwerk Ressource: Zuteilung) einer einzelnen Einheit in Data Factory darstellt. Höheren Durchsatz erzielen Sie durch Nutzung mehr Triebzüge für einige Szenarien. Siehe Abschnitt [Cloud Dateneinheiten Bewegung](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) auf Details.

**Ressource** | **Standard-Untergrenze** | **Untergrenze**
-------- | ------------------- | -------------
Intervall für die Planung | 15 Minuten | 15 Minuten
Intervall für Wiederholungsversuche | 1 Sekunde | 1 Sekunde
Timeoutwert wiederholen | 1 Sekunde | 1 Sekunde


### <a name="web-service-call-limits"></a>Web Service-Aufruf Grenzwerte

Azure Ressourcen-Manager hat Grenzen für API-Aufrufe. Sie können API Rate innerhalb der [Grenzen der Azure-Ressourcen-Manager-API](../azure-subscription-service-limits.md#resource-group-limits)aufrufen. 



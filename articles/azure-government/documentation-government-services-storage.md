<properties
    pageTitle="Azure Regierung Dokumentation | Microsoft Azure"
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/13/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-storage"></a>Azure Government Speicher

##  <a name="azure-storage"></a>Azure-Speicher

Ausführliche Informationen über diesen Dienst und dessen Verwendung Dokumentation [Azure Storage public](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Variationen

URLs für Speicherkonten in Azure Government unterscheiden:

Diensttyp|Azure Public|Azure Government
---|---|---
BLOB-Speicher|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Warteschlangenspeicher|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Tabelle speichern|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Alle Skripts und Code muss die entsprechende Endpunkte zu berücksichtigen.  Finden Sie unter [Konfigurieren von Verbindungszeichenfolgen Azure-Speicher](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Weitere Informationen zu APIs finden Sie unter <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Cloud Storage Konto Konstruktor</a>.

Das Suffix Endpunkt mit in diese Überladung ist core.usgovcloudapi.net 

### <a name="considerations"></a>Hinweise

Die folgenden Informationen identifizieren Azure Government Grenze für Azure Storage:

| Reguliert/gesteuert Daten erlaubt | Reguliert/gesteuert Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daten, gespeichert und verarbeitet in Azure Storage kann Produkt Exportkontrolle Daten enthalten. Statische Authentifikatoren Smartcard-PINs auf Azure Plattformkomponenten wie Kennwörter. Private Schlüssel von Zertifikaten zur Verwaltung von Azure Plattformkomponenten. Andere Sicherheit Informationen/Geheimnisse, wie Zertifikate, Verschlüsselungsschlüssel Hauptschlüssel und Speicherschlüssel in Azure Services gespeichert. | Azure Storage Metadata darf nicht Exportkontrolle Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten beim Erstellen und verwalten Ihre Speicherprodukt eingegeben.  Regulierte/gesteuert Daten nicht in die folgenden Felder eingeben: Ressourcengruppen, Bereitstellung, Ressourcennamen, Resource-Tags  

##  <a name="premium-storage"></a>Premium-Speicher

Ausführliche Informationen über diesen Dienst und dessen Verwendung finden Sie unter [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Variationen

Premium-Speicher ist in USGov Virginia erhältlich. Dies schließt VMs DS-Serie. 

### <a name="considerations"></a>Hinweise

Die gleichen Speicher Daten aufgeführten gilt für Premium-Speicherkonten. 

##  <a name="next-steps"></a>Nächste Schritte

Für zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>

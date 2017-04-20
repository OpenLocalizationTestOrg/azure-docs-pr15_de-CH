<properties 
    pageTitle="Importieren und Exportieren von Daten in Azure Redis Cache | Microsoft Azure" 
    description="Informationen Sie zum Importieren und Exportieren von Daten in und aus BLOB-Speicher mit Premium Azure Redis Cache Instanzen" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Importieren und Exportieren von Daten in Azure Redis Cache

Import/Export ist ein Azure Redis Cache Management Datenvorgang, wodurch Sie Daten in Azure Redis Cache importieren oder Exportieren von Daten aus dem Azure Redis Cache durch Importieren und Exportieren von Schnappschüssen Redis-Cache-Datenbank (RDB) aus einem Premium-Cache auf einer Seitenblob in einem Azure Storage-Konto. Import/Export können Sie zwischen verschiedenen Azure Redis Cacheinstanzen migrieren oder Auffüllen mit Daten verwenden.

Dieser Artikel bietet eine Anleitung zum Importieren und Exportieren von Daten mit Azure Redis Cache und bietet Antworten auf häufig gestellte Fragen.

>[AZURE.IMPORTANT] Import/Export ist in der Vorschau und ist nur für [Premium-Tier-](cache-premium-tier-intro.md) Caches.

## <a name="import"></a>Importieren

Import kann verwendet werden, von einem Redis-Server in jeder Umgebung unter Linux, Windows oder Cloud-Anbieter wie Amazon Web Services und andere Redis oder mit Redis kompatibel RDB-Dateien zu. Importieren von Daten ist eine einfache Möglichkeit einen Cache mit vorab aufgefüllten Daten erstellt. Während des Importvorgangs Azure Redis Cache RDB-Dateien von Azure-Speicher in den Arbeitsspeicher geladen und die Schlüssel in den Cache eingefügt.

>[AZURE.NOTE] Sicherstellen Sie bevor Sie den Importvorgang beginnen, dass der Redis Datenbank (RDB) Datei oder Dateien in Seitenblobs in derselben Region und Ihre Azure Redis Cacheinstanz Abonnement, Azure-Speicher hochgeladen werden. Weitere Informationen finden Sie unter [Erste Schritte mit Azure BLOB-Speicher](../storage/storage-dotnet-how-to-use-blobs.md). Die Funktion [Azure Redis Cache exportieren](#export) RDB-Datei exportiert, die RDB-Datei bereits in einer Seitenblob gespeichert und ist bereit für den Import.

1. Weitere exportierte Cache Blobs, [Suchen Sie den Cache](cache-configure.md#configure-redis-cache-settings) in Azure-Portal importieren und auf **Daten importieren** aus dem Blade **Einstellung** der Cacheinstanz.

    ![Daten importieren][cache-import-data]

2. **Angrenzen auswählen** und wählen Sie das Speicherkonto, das die zu importierenden Daten enthält.

    ![Konto auswählen][cache-import-choose-storage-account]

3. Klicken Sie auf den Container, der die zu importierenden Daten enthält.

    ![Wählen Sie container][cache-import-choose-container]

4. Wählen Sie eine oder mehrere Blobs zu importieren, indem Sie auf den Bereich links vom blobnamen und dann auf **auswählen**.

    ![Wählen Sie blobs][cache-import-choose-blobs]

5. Klicken Sie auf **Importieren** , um den Importvorgang zu starten.

    >[AZURE.IMPORTANT] Der Cache kann nicht von cacheclients während des Importvorgangs und alle vorhandenen Daten im Cache gelöscht.

    ![Importieren][cache-import-blobs]

    Sie können den Fortschritt des Importvorgangs Notifizierungen Azure-Portal oder Ereignisse [Protokollieren Audit](cache-configure.md#support-amp-troubleshooting-settings)anzeigen überwachen.

    ![In Arbeit importieren][cache-import-data-import-complete] 


## <a name="export"></a>Exportieren

Können Sie Daten in Azure Redis Cache Redis kompatibel RDB-Dateien exportieren. Diese Funktion können Sie Daten von einer Azure Redis Cache-Instanz in eine andere oder einen anderen Redis-Server verschieben. Während des Exportvorgangs auf dem virtuellen Computer, auf dem die Serverinstanz Azure Redis Cache gehostet wird eine temporäre Datei erstellt und die Datei wird dem designierten Speicher-Konto hochgeladen. Nach Abschluss des Exportvorgangs mit dem Status Erfolg, wird die temporäre Datei gelöscht.

1. Zu den aktuellen Inhalt des Cache Speicher, Azure-Portal [den Cache durchsuchen](cache-configure.md#configure-redis-cache-settings) exportieren **Exportieren** Blatt **Einstellungen** der Cacheinstanz.

    ![Wählen Sie Behälter][cache-export-data-choose-storage-container]

2. **Behälter auswählen** und wählen Sie das gewünschte Speicherkonto. Das Speicherkonto muss in dieselbe Abonnement und Region Cache.

    >[AZURE.IMPORTANT] Exportieren Sie importieren und mit Seitenblobs Classic und Speicherkonten ARM unterstützt, aber nicht von [Speicherkonten Blob](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) zurzeit unterstützt.

    ![Konto][cache-export-data-choose-account]

3. Wählen Sie den gewünschten blobcontainer, und klicken Sie auf **auswählen**. Neuen klicken Sie Container **Container hinzufügen** zuerst hinzufügen und wählen ihn aus der Liste.

    ![Wählen Sie Behälter][cache-export-data-container]

4. Geben Sie ein **Präfix Blob** , und klicken Sie auf **Exportieren** , um den Export zu starten. Das BLOB-Präfix wird verwendet, um die Namen der Dateien von diesen Exportvorgang Präfix.

    ![Exportieren][cache-export-data]

    Sie können den Fortschritt des Exportvorgangs Notifizierungen Azure-Portal oder Ereignisse [Protokollieren Audit](cache-configure.md#support-amp-troubleshooting-settings)anzeigen überwachen.

    ![][cache-export-data-export-complete]

    Caches bleiben während des Exportvorgangs zur Verfügung.


## <a name="importexport-faq"></a>Häufig gestellte Fragen zum Importieren/Exportieren

Dieser Abschnitt enthält häufig gestellte Fragen zum Import/Export-Funktion.

-   [Welche Preise leisten kann Import/Export verwenden?](#what-pricing-tiers-can-use-importexport)
-   [Können Daten von einem Redis-Server werden importiert?](#can-i-import-data-from-any-redis-server)
-   [Werden meine Cache beim Importieren/Exportieren angeboten?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Kann ich mit Redis Import/Export verwenden?](#can-i-use-importexport-with-redis-cluster)
-   [Wie funktioniert die Import/Export mit benutzerdefinierten Datenbanken festlegen?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Wie unterscheidet sich Importieren/Exportieren von Redis Dauerhaftigkeit?](#how-is-importexport-different-from-redis-persistence)
-   [Können Import/Export mit PowerShell, CLI oder anderen Management automatisieren?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Einen Timeoutfehler wurde während meiner Import-/Exportvorgang. Was bedeutet das?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Ich habe einen Fehler beim Exportieren von Daten in Azure BLOB-Speicher. Was ist passiert?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Welche Preise leisten kann Import/Export verwenden?

Import/Export steht nur in der Premium Tarif.

### <a name="can-i-import-data-from-any-redis-server"></a>Können Daten von einem Redis-Server werden importiert?

Ja, zusätzlich zum Importieren von Daten aus Azure Redis Cacheinstanzen importieren RDB-Dateien von einem Redis-Server in jeder Umgebung, wie Linux oder Windows ausgeführt oder cloud-Anbietern wie Amazon Web Services. Hierzu ein Seitenblob in ein Speicherkonto Azure RDB-Datei vom gewünschten Redis-Server hochladen und in Ihrem Premium Azure Redis Cache-Instanz importieren. Angenommen, möchten Sie Daten aus Produktion Cache exportieren und Importieren in den Cache als Teil einer Stagingumgebung zum Testen oder Migration verwendet. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Werden meine Cache beim Importieren/Exportieren angeboten?

-   **Export** - Caches stehen und Sie können mit Ihren Cache beim Exportieren.
-   **Import** - Caches werden zu Beginn ein Importvorgangs nicht verfügbar und werden verfügbar, wenn der Importvorgang abgeschlossen ist.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Kann ich mit Redis Import/Export verwenden?

Ja, und Sie können Import/Export zwischen einem gruppierten und nicht gruppierten Cache. Seit Redis Cluster [unterstützt nur Datenbank 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), werden keine Daten in Datenbanken als 0 importiert. Beim Importieren von gruppierten Cachedaten werden die Schlüssel unter Splitter des Clusters verteilt. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Wie funktioniert die Import/Export mit benutzerdefinierten Datenbanken festlegen?

Einige Tarifen haben unterschiedliche [Datenbanken Grenzen](cache-configure.md#databases)gibt es einige Aspekte beim Importieren konfiguriert einen benutzerdefinierten Wert für die `databases` während der Erstellung des Caches festlegen.

-   Beim Importieren in einen Tarif mit `databases` Limit als die Ebene, aus dem exportiert:
    -   Verwenden Sie die Standardanzahl von `databases` 16 für alle Tarifen ist, keine Daten verloren.
    -   Verwenden Sie eine benutzerdefinierte Anzahl von `databases` , liegt innerhalb der Grenzwerte für die Ebene, die Sie importieren, keine Daten verloren.
    -   Die exportierten Daten in einer Datenbank Daten, die das der neuen Stufe überschreitet, werden die Daten aus diesen höheren Datenbanken nicht importiert.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Wie unterscheidet sich Importieren/Exportieren von Redis Dauerhaftigkeit?

Azure Redis Cache Persistenz können Sie Daten in Redis in den Azure-Speicher gespeichert. Wenn Dauerhaftigkeit konfiguriert ist, behält Azure Redis Cache Snapshot Redis Cache in einem binären Format Redis, festplattenbasierten auf konfigurierbare. Tritt ein schwerwiegendes Ereignis, das die primäre und die Replikat-Cache deaktiviert, werden die Cachedaten automatisch mit den neuesten Snapshot wiederhergestellt. Weitere Informationen finden Sie unter [Datenpersistenz Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).

Import / Export können Sie Daten in oder Exportieren von Azure Redis Cache. Nicht konfigurieren Sie die Sicherung und Wiederherstellung mit Redis Dauerhaftigkeit.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Können Import/Export mit PowerShell, CLI oder anderen Management automatisieren?

Ja, weitere PowerShell Anweisungen [Redis Cache importieren](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) und [Exportieren von Redis-Cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Einen Timeoutfehler wurde während meiner Import-/Exportvorgang. Was bedeutet das?

Wenn weiterhin auf die **Daten importieren** oder **Exportieren von Daten** länger als 15 Minuten vor Beginn des Vorgangs erhalten Sie eine Fehlermeldung ähnlich der folgenden.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Um dies lösen, initiieren importieren oder exportieren 15 Minuten verstrichen ist.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ich habe einen Fehler beim Exportieren von Daten in Azure BLOB-Speicher. Was ist passiert?

Import/Export-funktioniert nur mit RDB-Dateien als Seitenblobs. Andere Blob werden zurzeit einschließlich BLOB-Speicher mit warme und kalte nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr Cache-Premiumfunktionen verwenden.

-   [Einführung in Azure Redis Cache Premium-Ebene](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png









<properties 
    pageTitle="Datenpersistenz Premium Azure Redis Cache konfigurieren" 
    description="Informationen Sie zum Konfigurieren und Verwalten von Datenpersistenz Premium Tier Azure Redis Cache Instanzen" 
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
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Datenpersistenz Premium Azure Redis Cache konfigurieren

Azure Redis Cache hat verschiedene Cache die Flexibilität bei der Wahl der Cachegröße und -Features, einschließlich der neuen Premium-Ebene.

Der Azure Redis Premium Cacheschicht enthält Funktionen wie clustering, Dauerhaftigkeit und virtual Network-Unterstützung. Dieser Artikel beschreibt die Dauerhaftigkeit eine Prämie Azure Redis Cache-Instanz konfigurieren.

Informationen über andere Cache Zusatzfunktionen finden Sie unter [Einführung in Azure Redis Cache Premium-Ebene](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Was ist Datenpersistenz?
Redis-Dauerhaftigkeit können Sie Daten in Redis gespeichert. Außerdem können Sie Momentaufnahmen und Sichern Sie die Daten bei einem Hardwarefehler geladen werden kann. Dies ist ein großer Vorteil gegenüber Basic oder Standard-Ebene, in dem die Daten im Arbeitsspeicher gespeichert und kann Datenverlust bei Ausfall, Cache-Knoten sind. 

Azure Redis Cache bietet Redis Dauerhaftigkeit der [RDB-Modell](http://redis.io/topics/persistence), der Daten in einem Konto Azure-Speicher Speicherort. Wenn Dauerhaftigkeit konfiguriert ist, behält Azure Redis Cache Snapshot Redis Cache in einem binären Format Redis, festplattenbasierten auf konfigurierbare. Tritt ein schwerwiegendes Ereignis, das die primäre und die Replikat-Cache deaktiviert, wird der Cache mit den letzten Snapshot wiederhergestellt.

Dauerhaftigkeit kann während der Erstellung des Caches und auf die **Standardeinstellungen** für vorhandene Premium-Caches aus dem **Neuen Redis Cache** -Blade konfiguriert werden.

## <a name="create-a-premium-cache"></a>Erstellen Sie einen Premium-cache

Erstellen Sie einen Cache konfigurieren Persistenz und zum [Azure-Portal](https://portal.azure.com) anmelden und auf **neu**->**Daten + Speicher**>**Redis Cache**.

![Erstellen einer Redis][redis-cache-new-cache-menu]

Um Dauerhaftigkeit zu konfigurieren, wählen Sie eine **Premium** -Caches Blatt **der Tarif wählen** .

![Wählen Sie die Preisstufe][redis-cache-premium-pricing-tier]

Wählt eine Prämie Tarif klicken Sie auf **Redis Dauerhaftigkeit**.

![Redis Dauerhaftigkeit][redis-cache-persistence]

Im folgenden Abschnitt wird beschrieben, wie Redis Dauerhaftigkeit neue Premium-Cache konfigurieren werden. Klicken Sie konfiguriert Redis Persistenz **Erstellen** , um Ihre neue Premium-Cache mit Redis erstellen.

## <a name="configure-redis-persistence"></a>Konfigurieren von Redis-Dauerhaftigkeit

Redis Persistenz-Blade **Redis Datenpersistenz** konfiguriert. Für neue Caches dieses Blatt während des Erstellungsprozesses Cache erfolgt wie im vorherigen Abschnitt beschrieben. Für bestehende Zwischenspeicher auf Blatt **Einstellungen** für den Cache-Blade **Redis Datenpersistenz** zugreifen.

![Redis Optionen][redis-cache-settings]

Redis-Dauerhaftigkeit klicken Sie auf **aktiviert** , um RDB (Redis) Sicherung aktivieren. Um Redis Dauerhaftigkeit zuvor aktivierte Premium-Cache zu deaktivieren, klicken Sie auf **deaktiviert**.

Konfigurieren Sie das Sicherungsintervall wird wählen Sie eine **Sicherung** aus der Dropdown-Liste aus. Auswahlmöglichkeiten sind **15 Minuten**, **30 Minuten** **60 Minuten**, **6 Stunden**, **12 Stunden**und **24 Stunden**. Dieses Intervall beginnt nach der vorherigen Sicherung erfolgreich abgeschlossen und eine neue Sicherung, wenn es abläuft initiiert wird.

Auf **Speicherkonto** Speicherkonto verwenden aktivieren und wählen Sie die **Primärschlüssel** oder **Sekundärschlüssel** Dropdown **Speicherschlüssel** verwenden. Sie müssen ein Speicherkonto in derselben Region als Cache auswählen und **Premium** Speicherkonto wird empfohlen, da Storage Premium höheren Durchsatz. 

>[AZURE.IMPORTANT] Regeneriert Speicherschlüssel für Ihr Konto Dauerhaftigkeit müssen Sie den gewünschten Schlüssel aus der Dropdownliste **Speicherschlüssel** charakterzusammenstellung.

![Redis Dauerhaftigkeit][redis-cache-persistence-selected]

Klicken Sie auf **OK** , um die Persistenzkonfiguration zu speichern.

Die nächste (oder erste Sicherung neue Caches) wird nach Ablauf der Sicherungsintervall initiiert.



## <a name="persistence-faq"></a>Dauerhaftigkeit FAQ

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Azure Redis Cache Dauerhaftigkeit.

-   [Kann Dauerhaftigkeit eine zuvor erstellte Cache aktivieren?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Kann ich die Sicherung Häufigkeit nach des Caches erstellen ändern?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Warum habe backup Frequenz 60 Minuten gibt es mehr als 60 Minuten zwischen Backups?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Was passiert bei einer neuen Sicherung auf alte Backups?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Was geschieht, wenn ich auf eine andere Größe skaliert haben und eine Sicherung wiederhergestellt, die vor der Skalierung?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kann Dauerhaftigkeit eine zuvor erstellte Cache aktivieren?

Ja, kann Redis Dauerhaftigkeit auf Cache erstellen und auf vorhandene Premium-Caches konfiguriert werden.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Kann ich die Sicherung Häufigkeit nach des Caches erstellen ändern?

Ja, können Sie backup-Häufigkeit-Blade **Redis Datenpersistenz** ändern. Informationen finden Sie unter [Persistenz Redis konfigurieren](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Warum habe backup Frequenz 60 Minuten gibt es mehr als 60 Minuten zwischen Backups?

Das Sicherungsintervall beginnt erst, wenn die vorherige Sicherung erfolgreich abgeschlossen. Wenn backup Frequenz 60 Minuten und einen Sicherungsprozess 15 Minuten abgeschlossen, startet die nächste Sicherung bis 75 Minuten nach der Startzeit der letzten Sicherung nicht.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Was passiert bei einer neuen Sicherung auf alte Backups?

Mit Ausnahme der letzten Backups werden automatisch gelöscht. Dieser Löschvorgang kann nicht sondern ältere Backups nicht unbegrenzt beibehalten.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Was geschieht, wenn ich auf eine andere Größe skaliert haben und eine Sicherung wiederhergestellt, die vor der Skalierung?

-   Wenn eine größere Größe skaliert haben, gibt es keine Auswirkung.
-   Wenn Sie kleiner skaliert haben und eine benutzerdefinierte [Datenbanken](cache-configure.md#databases) festlegen, ist größer als die [Datenbanken beschränken](cache-configure.md#databases) für Ihre neue Größe Daten in Datenbanken nicht wiederhergestellt werden. Weitere Informationen finden Sie unter [Meine benutzerdefinierte Datenbanken während der Skalierung betroffenen festlegen?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Wenn Sie kleiner skaliert haben nicht genügend Platz in der kleineren Größe für alle Daten aus der letzten Sicherung, werden während der Wiederherstellung, üblicherweise über [Lru Allkeys](http://redis.io/topics/lru-cache) Entfernungsrichtlinie Schlüssel entfernt werden.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr Cache-Premiumfunktionen verwenden.

-   [Einführung in Azure Redis Cache Premium-Ebene](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png

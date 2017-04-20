<properties
    pageTitle="Vorgehensweise bei einer Azure service-Unterbrechung, die Azure Key Vault betrifft | Microsoft Azure"
    description="Vorgehensweise, bei Azure Service-Ausfällen, die Azure Key Vault auswirkt."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault Verfügbarkeit und Redundanz

Azure Key Vault bietet mehrere Schichten von Redundanz zu Schlüssel und geheime Schlüssel für Ihre Anwendung verfügbar bleiben, auch wenn einzelne des Dienstes Komponenten.

Der Inhalt des Schlüssels Vault werden im Bereich und einer sekundären Region mindestens 150 Meilen entfernt jedoch innerhalb derselben Region repliziert. Dieser verwaltet Langlebigkeit der Schlüssel und geheime Schlüssel.

Ausfall einzelne Komponenten innerhalb der wichtigsten Tresor Schritt andere Komponenten in der Region zu wenden, um sicherzustellen, dass keine Beeinträchtigung der Funktionalität ist. Sie müssen keinen ergreifen, dadurch ausgelöst. Es geschieht automatisch und transparent werden.

In dem seltenen Fall, dass eine Azure Region nicht verfügbar ist werden Anfragen von Azure Key Vault in diesem Bereich stellen automatisch gerouteten (*Failover*) auf einen zweiten Bereich. Wenn die primäre Region wieder verfügbar ist, werden Anfragen weitergeleitet primäre Region (*wieder fehlgeschlagen*) zurück. Wieder müssen Sie keinen ergreifen, da dies automatisch geschieht.

Es sind ein paar Punkte zu beachten:

* Bei einem Failover Region dauert es einige Minuten für Failover-Dienst. Während dieser Zeit werden Anfragen möglicherweise nach Abschluss des Failovers.
* Nach Abschluss ein Failovers ist der Schlüssel Depot im schreibgeschützten Modus. In diesem Modus unterstützt Anfragen sind:
 * Liste Schlüssel Depots
 * Eigenschaften des Schlüssels Depots
 * Liste Geheimnisse
 * Kennwörter abrufen
 * Liste Schlüssel
 * (Eigenschaften) Schlüssel abrufen
 * Verschlüsseln
 * Entschlüsseln
 * Zeilenumbruch
 * Entpacken
 * Stellen Sie sicher
 * Zeichen
 * Sicherung
* Nach ein Failover wieder nicht, stehen alle Anforderungstypen (auch lesen *und* schreiben).

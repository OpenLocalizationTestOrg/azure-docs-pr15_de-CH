<properties
     pageTitle="Verwenden das Azure-Portal IoT Hub erstellen | Microsoft Azure"
     description="Eine Übersicht zum Erstellen und Verwalten von Azure IoT Hubs durch Azure-portal"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Erstellen Sie einen Azure-Portal mit IoT hub

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Einführung

Dieser Artikel beschreibt IoT Hub im Azure-Portal gefunden und zum Erstellen und Verwalten von IoT Hubs.

## <a name="where-to-find-iot-hubs"></a>Wo finden IoT hubs

Gibt verschiedene IoT Hubs finden.

1. **+ Neu**: **Azure IoT Hub** ist ein IoT und finden Sie in der Kategorie **Internet der Dinge** **+ neu**, wie andere Dienste.

2. IoT Hubs auch über den Markt als Held Dienst unter **Internet der Dinge**möglich.

## <a name="create-an-iot-hub"></a>IoT Hub erstellen

Sie können einen IoT Hub mit den folgenden Methoden erstellen:

- Erstellen eines IoT Hubs über die Option **+ neu** führt zu Blade im nächsten Screenshot dargestellt. Die Schritte zum Erstellen des IoT Hubs dadurch und durch den Markt sind identisch.

- Erstellen einen IoT Hub durch den Markt: **auf** eine-Blade identisch zum vorherigen Blatt **+ Neuer** Erfahrung öffnet. In den nächsten Abschnitten Liste mehrere Schritte IoT Hub erstellen.

### <a name="choose-the-name-of-the-iot-hub"></a>Wählen Sie den Namen des IoT Hubs

Um einen IoT Hub zu erstellen, müssen Sie den Hub benennen. Dieser Name muss über Hubs eindeutig sein. Es wird empfohlen, diesen Hub als eindeutig benannt ist keine Duplizierung von Hubs auf Back-End erlaubt.

### <a name="choose-the-pricing-tier"></a>Wählen Sie die Preisstufe

Sie können aus vier Ebenen: **frei**, **Standard 1** **Standard 2**und **Standard S3**. Freie Ebene kann nur 500 Geräte Verbindung IoT Hub bis 8.000 Nachrichten pro Tag.

**Standard S1**: IoT Hubs S1 Edition ist speziell für IoT Lösungen, die eine große Anzahl von Geräten relativ kleine Datenmengen pro Gerät generiert. Jede Einheit S1 Edition ermöglicht bis zu 400.000 Nachrichten pro Tag über alle angeschlossenen Geräte.

**Standard S2**: IoT Hub S2 Edition ist speziell für IoT Solutions in der Geräte große Datenmengen generieren. Jede Einheit der S2-Edition kann bis zu 6 Millionen Nachrichten pro Tag zwischen allen angeschlossenen Geräten.

**Standard S3**: IoT Hub S3 Edition ist speziell für IoT-Lösungen, die große Datenmengen generieren. Jede Einheit der S3-Edition kann bis zu 300 Millionen Nachrichten pro Tag zwischen allen angeschlossenen Geräten.

![][4]

> [AZURE.NOTE] IoT Hub ermöglicht nur eine freie Hub pro Azure-Abonnement.

### <a name="iot-hub-units"></a>IoT Hub-Einheiten

Eine IoT Hub-Einheit verfügt über eine bestimmte Anzahl von Nachrichten pro Tag. Die Gesamtzahl der Nachrichten für diesen Hub unterstützt ist die Anzahl der Einheiten multipliziert mit der Anzahl von Nachrichten pro Tag für diese Ebene. Wenn IoT Hub Eindringen 700.000 Nachrichten unterstützen soll, wählen Sie S1 Ebene zwei.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Gerät Cloud Partitionen und Ressourcengruppe

Sie können die Anzahl der Partitionen einen IoT Hub ändern. Standardpartitionen sind auf 4 festgelegt. Allerdings können Sie verschiedene Partitionen aus einer Dropdown-Liste.

Für Ressourcengruppen müssen Sie nicht explizit eine leere Ressourcengruppe erstellen. Beim Erstellen einer Ressource können Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden.

![][5]

### <a name="choose-subscriptions"></a>Wählen Sie Abonnements

Azure IoT Hub wird automatisch die Liste der Azure-Abonnements, das Benutzerkonto verknüpft ist. Sie können eine der Optionen hier IoT Hub, Azure-Abonnement zugeordnet.

### <a name="choose-the-location"></a>Speicherort auswählen

Die Speicherorte enthält eine Liste der Regionen IoT Hub angeboten wird. IoT Hub steht an den folgenden Speicherorten bereitstellen: Australien OST Australien Südost Osten, Asien – Südosten, Nordeuropa, Westeuropa, Japan Osten, Japan, uns Osten, Westen der USA.

### <a name="create-the-iot-hub"></a>IoT Hub erstellen

Wenn alle Schritte abgeschlossen sind, ist der IoT Hub erstellt werden. Klicken Sie auf **Erstellen** die Back-End erstellen IoT Hub mit Optionen starten und an dem angegebenen Speicherort bereitstellen.

Es dauert einige Minuten IoT Hub erstellt werden, da es Zeit für die Back-End-Bereitstellung in den entsprechenden Speicherort Servern auftreten.

## <a name="change-the-settings-of-the-iot-hub"></a>Ändern der IoT hub

Einstellung der vorhandenen IoT Hub können Sie nach Erstellung aus dem Blade IoT Hub ändern.

![][8]

**Freigegebene Richtlinien**: Diese Richtlinien definieren, die Berechtigungen für Geräte und Dienste Verbindung IoT Hub. Für den Zugriff auf diese Richtlinien auf **Freigegebenen Richtlinien** unter **Allgemein**. Dieses Blatt vorhandene Richtlinien ändern oder eine neue Richtlinie hinzuzufügen.

### <a name="create-a-policy"></a>Erstellen einer Richtlinie

- Klicken Sie auf **Hinzufügen** , um ein Blatt geöffnet. Hier können Sie den neuen Richtliniennamen und die Berechtigungen, die dieser Richtlinie zugeordnet werden, wie in der folgenden Abbildung dargestellt soll.

    Es gibt verschiedene Berechtigungen, die diese freigegebenen Richtlinien zugeordnet werden können. Die ersten beiden Richtlinien, **Registrierung schreiben**und **Lesen der Registrierung** Lesen erteilen und Schreibzugriffsrechte Identitätsspeicher Gerät oder die identitätsregistrierung. Mit der Option Write automatisch wählt die Option read sowie.

    Der **Dienst hergestellt** Sicherheitsrichtlinie Zugriffsberechtigung für Endpunkte Cloud-Seite wie die Verbraucher für Dienste Verbindung IoT Hub. Die Richtlinie **Gerät** erteilt für senden und Empfangen von Nachrichten auf die Endpunkte geräteseitige IoT Hub.

- Klicken Sie auf **Erstellen** , um diese neu erstellte Richtlinie der vorhandenen Liste hinzugefügt.

![][10]

## <a name="messaging"></a>Messaging

Klicken Sie auf **Messaging** zum Anzeigen einer Liste von Eigenschaften für IoT Hub, der geändert wird. Es gibt zwei Arten von Eigenschaften ändern oder kopieren: **Cloud Gerät** und **Gerät Cloud**.

- **Gerät** cloudeinstellungen: Diese Einstellung hat zwei Subsettings: **Cloud Gerät TTL** (Time-to-live) und die **Retentionszeit** für Nachrichten. Wenn der IoT Hub erstellt wird, werden beide Einstellungen mit einem Standardwert von einer Stunde erstellt. Auf diese Werte mithilfe der Schieberegler, oder geben Sie die Werte.

- **Cloud** Einstellungen: Diese Einstellung hat mehrere Subsettings, die mit dem Namen/werden Wenn IoT Hub erstellt und kann nur auf andere Subsettings, die angepasst werden kopiert. Diese Werte werden im nächsten Abschnitt aufgeführt.

**Partitionen**: Dieser Wert wird festgelegt, wenn IoT Hub erstellt und durch diese Einstellung geändert werden kann.

**Ereignis Hub-kompatible Namen und Endpunkt**: Event Hub wird intern erstellt, sollten Sie unter Umständen auf, wenn der IoT Hub erstellt. Dieses Ereignis Hub-kompatible Namen und Endpunkt kann nicht angepasst werden, jedoch wird über die Schaltfläche **Kopieren** verfügbar.

**Aufbewahrungszeit**: standardmäßig ein Tag aber auf andere Werte mithilfe der Dropdown-Liste angepasst werden. Dieser Wert ist in Tagen Gerät Cloud und nicht in Stunden, wie ähnliche Einstellung für Cloud Gerät.

**Verbrauchergruppen**: Verbrauchergruppen Einstellung ähnelt anderen messaging-Systemen, die verwendet werden können, um Daten an andere Programme oder Dienste Verbindung IoT Hub spezifisch sind. Jede IoT Hub wird mit einer Standardgruppe Consumer erstellt. Sie können jedoch hinzufügen oder Löschen von Verbrauchergruppen zu Ihrem IoT Hubs.

> [AZURE.NOTE] Verbrauchergruppe Standard kann nicht bearbeitet oder gelöscht werden.

![][11]

## <a name="pricing-and-scale"></a>Preise und Skalierung

Die Preise von einem vorhandenen IoT Hub kann Einstellungen **Preise** mit den folgenden Ausnahmen geändert werden:

- In der aktuellen Implementierung kann ein IoT Hub mit freien SKU Ebenen in eine bezahlte SKUs ändern oder umgekehrt.
- In der Azure-Abonnement kann nur eine freie Tier IoT Hub sein.

![][12]

Verschieben von einer höheren Ebene (S2 oder S3) auf niedriger Ebene (S1 oder S2) ist nur zulässig, wenn die Anzahl der Nachrichten für diesen Tag nicht in Konflikt stehen. Beispielsweise überschreitet die Anzahl der Nachrichten pro Tag 400.000, kann die Ebene für den IoT Hub geändert werden. Jedoch wenn Sie S1 Ebene ändern dann Hub für diesen Tag beschränkt.

## <a name="delete-the-iot-hub"></a>IoT Hub löschen

IoT Hub finden Sie löschen, indem Sie auf **Durchsuchen**klicken und dann den entsprechenden Hub löschen möchten. Klicken Sie auf die Schaltfläche **Löschen** unter dem Hubnamen Hub löschen.

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um weitere Informationen zum Verwalten von Azure IoT Hub:

- [BULK IoT-Geräte verwalten][lnk-bulk]
- [Verwendung Metriken][lnk-metrics]
- [Überwachung von Vorgängen][lnk-monitor]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]
- [Sichern Sie Ihre IoT Lösung von Grund auf][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
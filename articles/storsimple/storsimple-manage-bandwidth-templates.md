<properties
   pageTitle="StorSimple bandbreitenvorlagen verwalten | Microsoft Azure"
   description="Beschreibt die StorSimple bandbreitenvorlagen verwalten Bandbreite steuern können."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>Mit der StorSimple Bandbreite Verwalten der StorSimple Manager-Dienst

## <a name="overview"></a>Übersicht

Bandbreitenvorlagen können Sie die Netzwerkbandbreite über mehrere Uhrzeit Zeitpläne für die Datenebene vom StorSimple-Gerät in die Cloud zu konfigurieren.

Mit Bandbreitenschränkung Zeitpläne können Sie:

- Geben Sie individuelle Bandbreite Zeitpläne je nach Arbeitslast Netzwerk Verwendungen.

- Zentrales Management und Zeitpläne auf mehrere Geräte einfach und nahtlos wiederzuverwenden.

> [AZURE.NOTE] Dieses Feature steht nur für StorSimple-Geräten und nicht für virtuelle Geräte.

Die bandbreitenvorlagen für den Dienst in einem Tabellenformat angezeigt und enthalten die folgenden Informationen:

- **Name** einen eindeutigen Namen beim Erstellen der bandbreitenvorlage zugeordnet.

- **Zeitplan** – die Anzahl der Termine in einer bestimmten bandbreitenvorlage enthalten.

- **Verwendet von** – die Anzahl der Volumes Schablonen Bandbreite.

Mithilfe die Seite StorSimple Manager Service **Konfigurieren** im klassischen Azure-Portal Bandbreite verwalten.

Außerdem finden Sie weitere Informationen zu bandbreitenvorlagen konfigurieren:

- Fragen und Antworten zu bandbreitenvorlagen
- Best Practices für die bandbreitenvorlagen

## <a name="add-a-bandwidth-template"></a>Bandbreitenvorlage hinzufügen

Führen Sie die folgenden Schritte zum Erstellen einer neuen bandbreitenvorlage.

#### <a name="to-add-a-bandwidth-template"></a>Hinzufügen eine bandbreitenvorlage

1. Klicken Sie auf der Seite StorSimple Manager **Konfigurieren** auf **bandbreitenvorlage hinzufügen oder bearbeiten**.

2. Klicken Sie im Dialogfeld **Hinzufügen/bearbeiten Bandbreitenvorlage** :

   1. Wählen Sie aus der Dropdown-Liste **Vorlage** **neu erstellen** , um eine neue bandbreitenvorlage hinzufügen.
   2. Geben Sie einen eindeutigen Namen für Ihre bandbreitenvorlage.

3. Definieren Sie einen **Zeitplan Bandbreite**. So erstellen Sie einen Zeitplan

   1. Wählen Sie aus der Dropdown-Liste die Wochentage, denen für der Zeitplan konfiguriert ist. Sie können mehrere Tage auswählen, durch Aktivieren der Kontrollkästchen vor der jeweiligen in der Liste befindet.
   2. Die Option **Ganztägig** Wenn der Zeitplan für den gesamten Tag erzwungen wird. Wenn diese Option aktiviert ist, können Sie keine **Startzeit** oder eine **Endzeit**angeben. Der Zeitplan wird von 12:00 bis 23:59 Uhr ausgeführt.
   3. Wählen Sie aus der Dropdownliste eine **Startzeit**. Dies ist bei der Zeitplan beginnt.
   4. Wählen Sie aus der Dropdownliste eine **Endzeit**. Dies ist bei der Zeitplan beendet wird.

         > [AZURE.NOTE] Sich überschneidende Zeitpläne sind nicht zulässig. Wenn die Start- und Endzeiten überlappende Planung führt, sehen Sie eine Fehlermeldung.

   5. Geben Sie die **Bandbreitenrate**. Dies ist die Bandbreite in Megabit pro Sekunde (Mbps) Geräts StorSimple Operationen Cloud (Uploads und Downloads) verwendet. Geben Sie eine Zahl zwischen 1 und 1.000 für dieses Feld.

   6. Klicken Sie auf Check ![Häkchensymbol](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Die von Ihnen erstellten Vorlage wird der Liste der bandbreitenvorlagen auf der Seite **Konfigurieren** hinzugefügt werden.

    ![Neue bandbreitenvorlage erstellen](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Klicken Sie am unteren Rand der Seite **Speichern** , und klicken Sie auf **Ja,** Wenn Sie zur Bestätigung aufgefordert werden. Dies speichert die geänderte Konfiguration, die Sie erstellt haben.

## <a name="edit-a-bandwidth-template"></a>Bearbeiten einer bandbreitenvorlage

Führen Sie die folgenden Schritte zum Bearbeiten einer bandbreitenvorlage.

### <a name="to-edit-a-bandwidth-template"></a>So bearbeiten Sie eine bandbreitenvorlage

1. Klicken Sie auf **die bandbreitenvorlage hinzufügen oder bearbeiten**.

2. Klicken Sie im Dialogfeld **Hinzufügen/bearbeiten Bandbreitenvorlage** :

   1. Wählen Sie aus der Dropdown-Liste **Vorlage** eine bandbreitenvorlage, die Sie ändern möchten.
   2. Führen Sie die Änderung. (Sie können die vorhandenen Einstellungen ändern.)
   3. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Sie sehen die geänderte Vorlage in der Liste der bandbreitenvorlagen auf der Seite konfigurieren.

3. Klicken Sie ändern am unteren Rand der Seite **Speichern** . Klicken Sie auf **Ja,** Wenn Sie zur Bestätigung aufgefordert werden.

> [AZURE.NOTE] Sie können keine Änderungen speichern, wenn der bearbeitete Zeitplan mit einer vorhandenen Vorlage Bandbreite überlappt, die Sie ändern.

## <a name="delete-a-bandwidth-template"></a>Löschen einer bandbreitenvorlage

Die folgenden Schritte bandbreitenvorlage löschen.

#### <a name="to-delete-a-bandwidth-template"></a>Löschen eine bandbreitenvorlage

1. Tabellarische Liste der bandbreitenvorlagen für Ihren Dienst wählen Sie die Vorlage, die Sie löschen möchten. Ein Symbol "löschen" (**X**) wird ganz rechts in der ausgewählten Vorlage angezeigt. Klicken Sie auf **X** , um die Vorlage zu löschen.

2. Sie werden zur Bestätigung aufgefordert. Klicken Sie auf **OK** , um fortzufahren.

Wenn die Vorlage von alle Volumes verwendet wird, werden Sie löschen nicht zulässig. Sie sehen eine Fehlermeldung, dass die Vorlage verwendet wird. Dialogfeld mit einer Fehlermeldung erscheint darüber informiert, dass alle Verweise auf die Vorlage entfernt.

Sie können alle Verweise auf die Vorlage **Volumecontainer** Seite und volumecontainer, die diese Vorlage verwenden, sodass sie eine andere Vorlage verwenden oder eine benutzerdefinierte oder unbegrenzt Einstellung ändern. Wenn alle Referenzen entfernt haben, können Sie die Vorlage löschen.

## <a name="use-a-default-bandwidth-template"></a>Bandbreite verwenden

Eine Standardvorlage Bandbreite ist und standardmäßig Volume Container Bandbreitenkontrolle erzwingen beim Zugriff auf die Cloud verwendet. Die Standardvorlage dient auch als Referenz bereit für Benutzer, die eigene Vorlagen zu erstellen. Die Details dieser Standardvorlage sind:

- **Name** – unbegrenzt Nächte und Wochenenden

- **Zeitplan** – einen einzelnen Zeitplan von Montag bis Freitag, die eine Bandbreite von 1 Mbit/s zwischen 8 und 17 Uhr Gerätezeit gilt. Die Bandbreite wird für den Rest der Woche auf unbegrenzt festgelegt.

Die Standardvorlage kann bearbeitet werden. Die Verwendung dieser Vorlage (einschließlich bearbeitete Versionen) wird nachverfolgt.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Erstellen Sie eine ganztägige bandbreitenvorlage, die zu einem bestimmten Zeitpunkt beginnt

Gehen Sie folgendermaßen vor, um einen Zeitplan erstellen, der zu einem bestimmten Zeitpunkt beginnt und alle Tag ausgeführt wird. Im Beispiel Zeitplan beginnt um 9 Uhr vormittags und 9 Uhr morgens ausgeführt. Es ist wichtig zu beachten, die Start- und Endzeiten für einen bestimmten müssen beide auf demselben 24-Stunden-Zeitplan enthalten sein und kann nicht über mehrere Tage erstrecken. Benötigen Sie bandbreitenvorlagen einrichten, die mehrere Tage umfassen, müssen Sie mehrere Zeitpläne verwenden (wie im Beispiel gezeigt).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Eine ganztägige bandbreitenvorlage erstellen

1. Erstellen eines Zeitplans, das um 9 Uhr morgens beginnt und bis Mitternacht ausgeführt wird.

2. Einen weiteren Zeitplan hinzufügen. Konfigurieren Sie den zweiten Zeitplan von Mitternacht bis 9 Uhr morgens ausgeführt.

3. Speichern Sie die bandbreitenvorlage.

Zusammengesetzte Zeitplan zu einem Zeitpunkt Ihrer Wahl gestartet und ausgeführt ganztägige klicken.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Fragen und Antworten zu bandbreitenvorlagen

**Q**. Was geschieht mit Bandbreitenkontrolle zwischen den Zeitplänen wird? (Zeitplan beendet und eine noch nicht gestartet).

**A**. In solchen Fällen werden keine Bandbreitenkontrolle eingesetzt. Dies bedeutet, dass das Gerät unbegrenzten Bandbreite verwenden kann, wenn Daten in der Cloud tiering.

**Q**. Können Sie bandbreitenvorlagen auf einem offline-Gerät?

**A**. Sie dürfen bandbreitenvorlagen auf Volumes Container ändern, wenn das entsprechende Gerät offline ist.

**Q**. Können Sie bandbreitenvorlage volumecontainer zugeordnet, wenn die zugeordneten Volumes offline sind?

**A**. Sie können bandbreitenvorlage zugeordneten volumecontainer, dessen Volumes offline sind. Beachten Sie, dass Datenträger offline sind, keine Daten vom Gerät in die Cloud gestuft werden werden.

**Q**. Können Sie eine Standardvorlage löschen?

**A**. Löschen eine Standardvorlage zwar es nicht tun sollten. Die Verwendung der Standardvorlage, einschließlich der bearbeitete Versionen nachverfolgt wird. Die Tracking-Daten analysiert und im Laufe der Zeit zu der Standardvorlage verwendet.

**Q**. Wie bestimmen Sie Ihre bandbreitenvorlagen geändert werden müssen?

**A**. Zeichen müssen Sie die Bandbreite Vorlagen gehört zu Beginn sehen Netzwerk verlangsamt oder Unterfüllen mehrmals an einem Tag. In diesem Fall überwachen Sie das Speicherung und Verwendung Netzwerk anhand der e/a-Leistung und den Netzwerkdurchsatz Diagramme.

Identifizieren Sie von Netzwerkdaten Durchsatz Uhrzeit und volumecontainer in denen Netzwerk-Engpässe auftritt. Wenn dies geschieht, wenn Daten in der Cloud gestuft ist (diese Informationen von i/o-Performance für alle volumecontainer Gerät cloud erhalten), müssen Sie den volumecontainern zugeordneten bandbreitenvorlagen ändern.

Nach geänderten Vorlagen verwendet werden, müssen Sie das Netzwerk für erhebliche Wartezeiten zu überwachen. Wenn noch vorhanden sind, müssen Sie Ihre bandbreitenvorlagen erneut.

**Q**. Was geschieht, wenn mehrere Datenträger auf meinem Gerät Behälter plant diese Überlappung jedoch gelten unterschiedliche Grenzwerte?

**A**. Nehmen wir an, dass ein Gerät mit 3 Volume. Diese Container vollständig zugeordnete Zeitpläne überlappen. Für jeden dieser Container sind verwendet Bandbreitengrenzwerte 5, 10 und 15 Mbit/s. Wenn Ausgaben auf alle Container gleichzeitig auftreten, die mindestens 3 Bandbreitengrenzwerte angewandt werden: in diesem Fall 5 Mbit/s Diese ausgehende e/a-Anfragen die gleiche Warteschlange freigeben.

## <a name="best-practices-for-bandwidth-templates"></a>Best Practices für die bandbreitenvorlagen

Führen Sie diese best Practices für das StorSimple-Gerät:

- Konfigurieren Sie bandbreitenvorlagen auf dem Gerät aktivieren Variable Drosselung der Netzwerkdurchsatz vom Gerät zu verschiedenen Tageszeiten. Bandbreitenvorlagen mit backup-Zeitpläne können zusätzliche Netzwerkbandbreite für Cloud-Betrieb Spitzenzeiten effektiv nutzen.

- Berechnet die tatsächliche Bandbreite für eine bestimmte Bereitstellung anhand der Größe der Bereitstellung und der erforderlichen Recovery Time Objective (RTO) erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)

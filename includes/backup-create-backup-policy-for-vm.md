## <a name="defining-a-backup-policy"></a>Backup-Richtlinie definieren

Backup-Richtlinie definiert eine Matrix als Daten-Snapshots erstellt werden und wie lange diese Snapshots gespeichert werden. Beim Definieren einer Richtlinie für einen virtuellen Computer sichern können Sie einen Sicherungsauftrag *täglich*auslösen. Beim Erstellen einer neuen Richtlinie gilt es für das Depot. Die Sicherungsrichtlinie Schnittstelle sieht folgendermaßen aus:

![Backup-Richtlinie](./media/backup-create-policy-for-vms/backup-policy.png)

So erstellen Sie eine Richtlinie:

1. Geben Sie den **Richtliniennamen ein**.

2. Snapshots der Daten können täglich oder wöchentlich Abständen erstellt werden. Verwenden Sie im Dropdownmenü **Backup-Häufigkeit** , ob Daten Snapshots täglich erstellt werden oder wöchentlich.

    - Verwenden Sie wählen Sie einen täglich das markierte Steuerelement auf die Tageszeit für den Snapshot. Ändern Sie die Stunde aufzuheben Sie Stunde, und wählen Sie neue Stunde.

    ![Tägliche backup-Richtlinie](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Wenn Sie wöchentlichen Intervallen, verwenden Sie die markierten Steuerelemente, wählen Sie die Wochentage und die Uhrzeit der Momentaufnahme. Wählen Sie einen oder mehrere Tage, im Tag. Wählen Sie im Menü Stunde eine Stunde. Ändern Sie die Stunde deaktivieren wählen Sie ausgewählte Stunde und neue Stunde.

    ![Wöchentlicher backup-Richtlinie](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Standardmäßig sind alle **Beibehaltungsdauer** Optionen ausgewählt. Deaktivieren Sie alle Bereichsgrenzen Aufbewahrung, die Sie nicht verwenden möchten. Geben Sie die saaltüren verwenden.

    Monatliche und jährliche Archivierung Bereiche können Sie Snapshots basierend auf wöchentlich oder täglich Inkrement angeben.

    >[AZURE.NOTE] Beim Schützen einer VM führt ein Sicherungsauftrag einmal täglich. Die beim Ausführen der Sicherung dauert für jedes Beibehaltungsdauer.

4. Nachdem alle Optionen für die Richtlinie festlegen, am Anfang des Blades klicken Sie auf **Speichern**.

    Die neue Richtlinie wird auf das Depot sofort angewendet.

Ein Azure Virtual Machine unterstützt eine Anzahl Datenträger anfügen. Für eine optimale Leistung sollten Sie die Anzahl der stark ausgelastete Datenträgern der virtuellen Maschine zu möglichen Drosselung. Wenn alle Festplatten nicht sehr gleichzeitig genutzt werden, kann das Speicherkonto eine größere Anzahl Datenträger unterstützen.

- **Für standard Speicherkonten:** Ein standard Storage-Konto hat eine maximale Gesamtbetrag anfordern 20.000 IOPS. Insgesamt IOPS über alle virtuellen Datenträger im standardspeicherkonto darf diese Grenze nicht überschreiten.

    Sie können etwa Anzahl stark ausgelastete Datenträger unterstützt durch ein Standardverfahren Speicherkonto basierend auf Anforderung Ratenlimit berechnen. Beispiel für eine grundlegende Ebene VM maximal stark ausgelastete Datenträger über 66 (20.000-300 IOPS pro Datenträger) und für Standard-Tier-VM ist ist 40 (20.000-500 IOPS pro Datenträger), wie in der folgenden Tabelle dargestellt. 
 
- **Für Speicherkonten Premium:** Ein Premium-Speicherkonto hat eine maximale Gesamtdurchsatz 50 Gbit/s. Der Gesamtdurchsatz aller Festplatten VM darf diese Grenze nicht überschreiten.
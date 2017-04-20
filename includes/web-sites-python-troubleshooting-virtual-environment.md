Das Bereitstellungsskript wird auf Azure Erstellung der virtuellen Umgebung überspringen, wenn festgestellt wird, dass bereits eine kompatible virtuelle Umgebung.  Dadurch kann Bereitstellung erheblich beschleunigen.  Bereits installierte Pakete werden Pip übersprungen.

In bestimmten Situationen möchten Erzwingen der virtuellen Umgebung löschen.  Sie sollten, wenn eine virtuelle Umgebung als Teil des Repository enthalten.  Sie sollten auch dazu möchten Sie bestimmte Pakete entfernen oder sich requirements.txt testen.

Es gibt einige Optionen vorhandene virtuelle Umgebung auf Azure verwalten:

### <a name="option-1-use-ftp"></a>Option 1: FTP verwenden

Mit einem FTP-Client mit dem Server verbinden, und Sie werden den Env-Ordner löschen.  Beachten Sie, dass einige FTP-Clients (z. B. Webbrowser) möglicherweise schreibgeschützt und nicht die Ordner löschen erlauben, damit diese Funktion einen FTP-Client mit sicherstellen möchten.  Der FTP-Hostname und werden in Ihrem Web app Blade [Azure-Portal](https://portal.azure.com)angezeigt.

### <a name="option-2-toggle-runtime"></a>Option 2: Umschalten runtime

Hier ist eine Alternative, die Tatsache, dass das Bereitstellungsskript Env-Ordner gelöscht werden, wenn sie die gewünschte Version von Python nicht nutzt.  Dadurch wird effektiv die vorhandene Umgebung löschen und eine neue erstellen.

1. Wechseln Sie zu einer anderen Version von Python (über runtime.txt oder **Einstellungen** -Blades im Azure-Portal)
1. Git push (ignorieren Pip Installation Fehler ggf.) ändern
1. Wechseln Sie zurück zur ersten Version von Python
1. Git push erneut ändern

### <a name="option-3-customize-deployment-script"></a>Option 3: Bereitstellungsskript anpassen

Wenn Sie das Bereitstellungsskript angepasst haben, können Sie den Code in "Deploy.cmd" Env Ordner löschen erzwingen ändern.

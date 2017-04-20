<properties
   pageTitle="Anwendung aktualisieren: Erweiterte Themen | Microsoft Azure"
   description="Dieser Artikel behandelt einige erweiterten Themen zur Aktualisierung einer Fabric-Dienst-Anwendung."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Fabric Service Application Upgrade: Erweiterte Themen

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Hinzufügen oder Entfernen von Diensten während einer anwendungsaktualisierung

Ein neuer Dienst zu einer Anwendung hinzugefügt wird, die bereits bereitgestellt und als Aktualisierung veröffentlicht wird der neue Dienst bereitgestellte Anwendung hinzugefügt.  Eine solche Aktualisierung beeinflusst Dienste nicht, die bereits Teil der Anwendung. Für den neuen Dienst aktiv sein muss, eine Instanz des Diensts hinzugefügt wurde gestartet werden (mit der `New-ServiceFabricService` Cmdlet).

Dienste können auch von einer Anwendung als Teil einer Aktualisierung entfernt. Alle aktuelle Instanzen des Dienstes zu-werden gelöschte müssen jedoch vor der Aktualisierung beendet werden (mit der `Remove-ServiceFabricService` Cmdlet). 

## <a name="manual-upgrade-mode"></a>Manuellen Aktualisierungsmodus

> [AZURE.NOTE]  Nicht überwachte manuelle Modus sollte nur für fehlerhafte oder unterbrochene Aktualisierung betrachtet werden. Überwachte Modus ist die empfohlene Upgrade Service Fabric Anwendungsmöglichkeiten.

Azure Service Fabric ermöglicht mehrere Upgrade-Modi um Entwicklung und Produktion Cluster unterstützt. Gewählten Deployment-Optionen können für verschiedene umge bungen abweichen.

Überwachte anwendungsaktualisierung ist die häufigste Aktualisierung in der Produktion verwenden. Aktualisierungsrichtlinien angegeben, sorgt Fabric Service die Anwendung fehlerfrei ist, bevor die Aktualisierung fortgesetzt wird.

 Der Anwendungsadministrator können parallele Anwendung Upgrade Handbetrieb Kontrolle über den Fortschritt der Aktualisierung durch den verschiedenen Upgrade Domänen haben. Dieser Modus ist nützlich, wenn eine benutzerdefinierte oder komplexe Auswertung Integritätsrichtlinie erforderlich oder nicht konventionelle Aktualisierung geschieht (beispielsweise Anwendung ist noch Datenverlust).

Schließlich ist die automatische anwendungsaktualisierung für Entwicklungs- oder testumgebungen ermöglichen eine schnelle Iterationszyklus während der Entwicklung.

## <a name="change-to-manual-upgrade-mode"></a>Zum manuellen Aktualisierungsmodus ändern
**Handbuch**– anwendungsaktualisierung am aktuellen UD beenden und der Aktualisierungsmodus nicht überwachte manuell ändern. Der Administrator muss **MoveNextApplicationUpgradeDomainAsync** um die Aktualisierung fortzusetzen, oder lösen Sie einen Rollback durch Initiieren einer neuen Aktualisierung manuell aufrufen. Nach die Aktualisierung in den manuellen Modus wechselt, bleibt er im manuellen Modus, bis eine neue Aktualisierung. **GetApplicationUpgradeProgressAsync** Befehl gibt FABRIC\_Anwendung\_UPGRADE\_Status\_PARALLELEN\_FORWARD\_ausstehend.

## <a name="upgrade-with-a-diff-package"></a>Diff-Paket aktualisieren

Service Fabric-Anwendung kann durch eine vollständige und unabhängige Anwendung Paket Bereitstellung aktualisiert werden. Eine Anwendung kann auch mithilfe eines Diff-Pakets, das enthält nur die aktualisierten Anwendungsdateien, das Anwendungsmanifest und die Manifestdateien Service aktualisiert werden.

Eine vollständige Anwendung umfasst alle Dateien zum Starten und Ausführen einer Anwendung Service Fabric. Ein Diff-Paket enthält nur die Dateien, die zwischen der letzten und der aktuellen Aktualisierung geändert und vollständige Anwendungsmanifest und dem Dienst Manifestdateien. Jede Bezugnahme im Anwendungsmanifest oder Dienstmanifest im Layout Build gefunden in der Abbildspeicher gesucht.

Vollständige Pakete sind für die erste Installation einer Anwendung für den Cluster erforderlich. Nachfolgende Updates kann entweder einen vollständigen oder Diff-Paket.

Mehrfach mit einem Vergleich hilfreich sein:

* Ein Diff-Paket wird bevorzugt bei großen Anwendungspaket, das verschiedene Service-Manifestdateien oder mehrere Pakete, Config Pakete oder Datenpakete verweist.

* Ein Diff-Paket wird bevorzugt ein Bereitstellungssystem haben, die das Layout erstellen direkt aus der Anwendung Buildprozess generiert. Obwohl der Code geändert hat, erhalten neu erstellten Assemblys in diesem Fall eine andere Prüfsumme. Mit einer vollständigen Anwendung benötigen Sie die Version auf alle Pakete aktualisieren. Verwenden ein Diff-Paket, geben Sie nur die geänderten Dateien und Manifestdateien, die Version geändert wurde.

Beim Aktualisieren einer Anwendung mithilfe von Visual Studio wird das Diff-Paket automatisch veröffentlicht. Um ein Diff-Paket manuell zu erstellen, das Anwendungsmanifest und die Manifeste Service müssen aktualisiert werden, aber nur die geänderten Pakete in das endgültige Anwendungspaket berücksichtigt werden sollen. 

Beispielsweise beginnen wir mit der folgenden Anwendung (Verständlichkeit vorgesehenen Versionsnummern):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Jetzt nehmen Sie nur das Paket von service1 mit einem Diff-Paket mit PowerShell aktualisieren möchten. Die aktualisierte Anwendung hat nun die folgende Ordnerstruktur:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

In diesem Fall aktualisieren Sie das Anwendungsmanifest 2.0.0 und das Manifest für service1 entsprechend dem Paket aktualisieren. Der Ordner für das Anwendungspaket müsste die folgende Struktur:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Sie die Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch ein Upgrade der Anwendung mit Visual Studio.

[Aktualisieren Sie die Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine anwendungsaktualisierung mit PowerShell.

Steuern Sie, wie Ihre Anwendung aktualisiert [Aktualisieren Parameter](service-fabric-application-upgrade-parameters.md).

Machen Sie die Upgrades der Anwendung zu [Daten](service-fabric-application-upgrade-data-serialization.md)Serialisierung kompatibel.

Beheben von Problemen in Anwendungsupgrades auf die Schritte [Zur Problembehandlung Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
 

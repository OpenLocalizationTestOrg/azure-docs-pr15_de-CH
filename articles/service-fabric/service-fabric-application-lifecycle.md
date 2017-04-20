<properties
   pageTitle="Application Lifecycle Service Fabric | Microsoft Azure"
   description="Entwickeln, bereitstellen, testen, aktualisieren, verwalten und Entfernen von Service Fabric-Anwendung beschrieben."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Service Fabric-Anwendungslebenszyklus
Wie mit anderen Plattformen auf Azure Service Fabric normalerweise folgende Phasen: Entwurf, Entwicklung, Tests, Bereitstellung, Aktualisierung, Wartung und entfernen. Service Fabric bietet erstklassige Unterstützung für vollständige Anwendungslebenszyklus Cloudanwendungen von der Entwicklung über Bereitstellung, tägliche Management und Wartung zur endgültigen Stilllegung. Der Dienst ermöglicht verschiedene Rollen an unabhängig im Lebenszyklus Anwendung. Dieser Artikel bietet eine Übersicht über APIs und die Verwendung von Rollen während aller Phasen des Anwendungslebenszyklus Service Fabric.

## <a name="service-model-roles"></a>Modell Rollen
Die Modell Rollen sind:

- **Entwickler**: entwickelt modular und allgemeine Dienste, die neu bestimmt und in mehreren Applikationen gleichen Typ oder verschiedene Typen verwendet werden können. Beispielsweise kann ein Warteschlangendienst für ticketing-Anwendung (Helpdesk) oder e-Commerce-Anwendung (Warenkorb) verwendet werden.

- **Anwendungsentwickler**: Applikationen erstellt, indem eine Sammlung von Diensten Anforderungen bestimmten Szenarien zu integrieren. Beispielsweise kann eine e-Commerce-Website "JSON statusfreien Front-End-Service" Integrieren "Auktion statusbehaftete Dienst" und "Warteschlange statusbehaftete Dienst" Versteigerung Projektmappe.

- **Anwendungsadministrator**: entscheidet (Konfigurationsparameter Vorlage ausfüllen) Konfiguration, Bereitstellung (Zuordnung zu verfügbaren Ressourcen) und Servicequalität. Beispielsweise entscheidet ein Anwendungsadministrator Gebietsschema (Englisch USA) oder Japanisch für Japan beispielsweise der Anwendung. Eine andere Anwendung bereitgestellte haben unterschiedliche Werte.

- **Operator**: basierend auf der Konfiguration und dem Anwendungsadministrator Anforderungen bereitgestellt. Beispielsweise ein Operator Vorschriften und stellt die Anwendung bereit und in Azure ausgeführt wird. Operatoren Anwendungsinformationen Zustand und die Leistung überwachen und verwalten die Infrastruktur nach Bedarf.


## <a name="develop"></a>Entwickeln
1. Ein *Entwickler* entwickelt verschiedene Services mit [Zuverlässigen Akteure](service-fabric-reliable-actors-introduction.md) oder [Zuverlässige Dienste](service-fabric-reliable-services-introduction.md) Programmiermodell.
2. Ein *Entwickler* beschreibt deklarativ entwickelte Typen in einer Manifestdatei Service Code, Konfiguration und Daten Pakete aus.
3. Ein *Anwendungsentwickler* erstellt eine Anwendung mit verschiedenen Diensttypen.
4. Ein *Anwendungsentwickler* beschreibt den Anwendungstyp in einem Anwendungsmanifest deklarativ durch Verweisen auf Service Manifeste der konstituierenden entsprechend überschreiben und Parametrisieren Einstellmöglichkeiten für Konfiguration und Bereitstellung der einzelnen Dienste.

Beispiele finden Sie unter [mit zuverlässigen Akteure](service-fabric-reliable-actors-get-started.md) und [zuverlässige Dienste Einstieg](service-fabric-reliable-services-quick-start.md) .

## <a name="deploy"></a>Bereitstellen
1. Ein *Anwendungsadministrator* Schneider den Anwendungstyp für eine bestimmte Anwendung Service Fabric-Cluster durch Angeben der entsprechenden Parameter des **Anwendungstyps** Elements im Anwendungsmanifest bereitgestellt werden.

2. Ein *Operator* uploadet Anwendungspaket in Abbildspeicher Cluster mithilfe der [ **CopyApplicationPackage** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) oder [ **Copy-ServiceFabricApplicationPackage** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Das Anwendungspaket enthält das Anwendungsmanifest und die Service-Pakete. Fabric Service bringt Applikationen aus dem Anwendungspaket in der Abbildspeicher Azure Blob-Speicher oder der Service Fabric-Dienst gespeichert.

3. Der *Operator* stellt dann den Anwendungstyp im Zielcluster hochgeladene Anwendung Paket mithilfe der [ **ProvisionApplicationAsync** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register-ServiceFabricApplicationType** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125958.aspx)oder die [ **Bereitstellung einer Anwendung** REST-Vorgang](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Nach der Bereitstellung der Anwendungdes startet einen *Operator* die Anwendung mit der *Anwendungsadministrator* die [ **CreateApplicationAsync** Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), [ **Neu-ServiceFabricApplication** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125913.aspx)oder [ **Anwendung erstellen** REST Vorgang](https://msdn.microsoft.com/library/azure/dn707676.aspx)bereitgestellten Parameter.

5. Nach der Bereitstellung der Anwendungdes verwendet *Operator* [ **CreateServiceAsync** Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), [ **Neu-ServiceFabricService** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125874.aspx)oder [ **Create Service** REST-Vorgang](https://msdn.microsoft.com/library/azure/dn707657.aspx) für die Anwendung basierend auf verfügbaren Diensttypen neuen Dienstinstanzen erstellen.

6. Die Anwendung wird nun im Service Fabric-Cluster ausgeführt.

Beispiele finden Sie unter [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Test
1. Nach der Bereitstellung Cluster lokale Entwicklung zu einem Testcluster führt *Entwickler* Testszenario integrierte Failover mit Klassen [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) und [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) oder das [Cmdlet " **Invoke-ServiceFabricFailoverTestScenario** "](https://msdn.microsoft.com/library/azure/mt644783.aspx). Testszenario Failover führt einen angegebenen Dienst über wichtige Übergänge und Failover sicherzustellen, dass es noch aktiv ist.

2. Der *Entwickler* führt dann das integrierte Chaos Testszenario mit Klassen [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) und [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) oder das [Cmdlet " **Invoke-ServiceFabricChaosTestScenario** "](https://msdn.microsoft.com/library/azure/mt644774.aspx). Testszenario Chaos führt mehrere Knoten, Paket und Replikat Fehler zufällig im Cluster.

3. Der *Entwickler* [Tests Dienst - Kommunikation](service-fabric-testability-scenarios-service-communication.md) erstellen Szenarien, die primäre Replikate im Cluster verschoben.

Weitere Informationen finden Sie unter [Einführung in Fault Analysis Services](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Aktualisieren
1. *Entwickler* enthaltenen Dienste instanziierten Anwendung aktualisiert und behebt Fehler und enthält eine neue Version des Servicemanifests.

2. Ein *Anwendungsentwickler* überschreibt und die Konfiguration und Bereitstellung Einstellungen konsistent Dienste parametrisiert und enthält eine neue Version des Anwendungsmanifests. Der Anwendungsentwickler die neuen Versionen der Service Manifeste in die Anwendung integriert und enthält eine neue Version des Anwendungstyps in aktualisierten Antrag.

3. Ein *Anwendungsadministrator* enthält die neue Version des Anwendungstyps in Anwendung aktualisieren die entsprechenden Parameter.

5. Ein *Operator* lädt aktualisierte Anwendungspaket an den Cluster Abbildspeicher mithilfe der [ **CopyApplicationPackage** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) oder [ **Copy-ServiceFabricApplicationPackage** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125905.aspx). Das Anwendungspaket enthält das Anwendungsmanifest und die Service-Pakete.

6. Ein *Operator* stellt die neue Version der Anwendung im Zielcluster mithilfe der [ **ProvisionApplicationAsync** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register-ServiceFabricApplicationType** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125958.aspx)oder der [ **Bereitstellung einer Anwendung** REST-Vorgang](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. Einen *Operator* aktualisiert die Anwendung auf die neue Version der [ **UpgradeApplicationAsync** Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), [ **Start-ServiceFabricApplicationUpgrade** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125975.aspx)oder [ **Aktualisieren einer Anwendung** REST-Vorgang](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. Ein *Operator* überprüft den Status der Aktualisierung mithilfe der [ **GetApplicationUpgradeProgressAsync** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), das [Cmdlet " **Get-ServiceFabricApplicationUpgrade** "](https://msdn.microsoft.com/library/azure/mt125988.aspx)oder [ **Upgrade Application-Status abrufen** REST-Vorgang](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Gegebenenfalls den *Operator* ändert und wendet die Parameter die Aktualisierung der aktuellen Anwendung mit der [Methode **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), [ **Update-ServiceFabricApplicationUpgrade** -Cmdlet](https://msdn.microsoft.com/library/azure/mt126030.aspx)oder [REST **Update Application Upgrade** -Vorgang](https://msdn.microsoft.com/library/azure/mt628489.aspx).

10. Bei Bedarf Rollback *Operator* die Aktualisierung der aktuellen Anwendung mit der [ **RollbackApplicationUpgradeAsync** Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), die [ **Start-ServiceFabricApplicationRollback** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125833.aspx)oder [REST **Rollback Anwendung Upgrade** -Vorgang](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Service Fabric upgrades ohne die Verfügbarkeit der einzelnen Dienste im Cluster ausgeführten Anwendung.

Siehe [Anleitung upgrade Anwendung](service-fabric-application-upgrade-tutorial.md) Beispiele.

## <a name="maintain"></a>Verwalten
1. Betriebssystem-Upgrades und Patches Schnittstellen Service Fabric mit der Azure-Infrastruktur, die im Cluster ausgeführten Programme garantieren.

2. Für Upgrades und Patches Service Fabric-Plattform aufgerüstet Service Fabric ohne Verfügbarkeit von jeder Anwendung auf dem Cluster ausgeführt.

3. Ein *Anwendungsadministrator* genehmigt das Hinzufügen oder Entfernen von Knoten aus einem Cluster nach historisch Kapazitätsdaten und voraussichtlichen Bedarf analysieren.

4. Ein *Operator* hinzugefügt und vom *Anwendungsadministrator*angegebene Knoten entfernt.

5. Wenn neue Knoten hinzugefügt oder vorhandene Knoten aus dem Cluster entfernt, Lastausgleich Fabric Service automatisch ausgeführten Anwendung auf allen Knoten im Cluster eine optimale Leistung zu erzielen.

## <a name="remove"></a>Entfernen
1. *Operator* kann eine bestimmte Instanz eines ausgeführten Diensts im Cluster löschen, ohne die gesamte Anwendung mithilfe der [ **DeleteServiceAsync** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), [ **Remove-ServiceFabricService** -Cmdlet](https://msdn.microsoft.com/library/azure/mt126033.aspx)oder [ **Delete-Service** REST-Vorgang](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. *Operator* kann auch eine Instanz der Anwendung und alle Dienste mithilfe der [ **DeleteApplicationAsync** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), [ **Remove-ServiceFabricApplication** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125914.aspx)oder [ **Anwendung löschen** REST Vorgang](https://msdn.microsoft.com/library/azure/dn707651.aspx)löschen.

3. Sobald die Anwendung und die Dienste beendet wurden, kann *Operator* mithilfe der [ **UnprovisionApplicationAsync** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), [ **Registrierung-ServiceFabricApplicationType** -Cmdlet](https://msdn.microsoft.com/library/azure/mt125885.aspx)oder [ **Aufheben der Bereitstellung einer Anwendung** REST Vorgang](https://msdn.microsoft.com/library/azure/dn707671.aspx)Anwendungstyp aufheben. Den Anwendungstyp bekannt wird Anwendungspaket nicht aus der ImageStore entfernt. Das Anwendungspaket muss manuell entfernt werden.

4. Ein *Operator* entfernt ImageStore mithilfe der [ **RemoveApplicationPackage** -Methode](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) oder [ **Entfernen-ServiceFabricApplicationPackage** -Cmdlet](https://msdn.microsoft.com/library/azure/mt163532.aspx)Anwendungspaket.

Beispiele finden Sie unter [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Entwicklung finden Sie unter Testen und Verwalten von Service Fabric Applikationen und Services:

- [Zuverlässige Akteure](service-fabric-reliable-actors-introduction.md)
- [Zuverlässige Dienste](service-fabric-reliable-services-introduction.md)
- [Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md)
- [Anwendung aktualisieren](service-fabric-application-upgrade.md)
- [Übersicht über die Prüfbarkeit](service-fabric-testability-overview.md)
- [REST-basierten Lifecycle Anwendungsbeispiel](service-fabric-rest-based-application-lifecycle-sample.md)

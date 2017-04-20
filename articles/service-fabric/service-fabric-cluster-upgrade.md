<properties
   pageTitle="Aktualisieren ein Clusters Azure Service Fabric | Microsoft Azure"
   description="Upgrade Service Fabric-Code und Konfiguration, der Service Fabric-Cluster, einschließlich Cluster Aktualisierungsmodus Zertifikate hinzufügen Anwendungsports tun Betriebssystem-Patches aktualisieren ausgeführt wird usw. Was können Sie erwarten, wenn die Aktualisierung ausgeführt werden?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-an-azure-service-fabric-cluster"></a>Aktualisieren eines Clusters Azure Service Fabric

> [AZURE.SELECTOR]
- [Azure-Cluster](service-fabric-cluster-upgrade.md)
- [Eigenständige Cluster](service-fabric-cluster-upgrade-windows-server.md)

Entwerfen für Erweiterbarkeit ist für alle modernen System Schlüssel zur langfristigen Erfolg Ihres Produkts. Ein Azure Service Fabric-Cluster ist eine Ressource, die Sie besitzen jedoch teilweise von Microsoft verwaltet. Dieser Artikel beschreibt, welche automatisch verwaltet wird und was Sie selbst konfigurieren können.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Steuerung der Fabric-Version, die im Cluster ausgeführt wird

Sie können automatische Fabric Upgrades erhalten, wenn Microsoft neue Versionen oder wählen Sie eine unterstützte Fabric möchten Cluster zu Cluster festlegen.

Zu diesem Zweck setzen Clusterkonfiguration "UpgradeMode" im Portal oder Ressourcen-Manager zum Zeitpunkt der Erstellung oder höher auf einem aktiven Cluster verwenden 

>[AZURE.NOTE] Stellen Sie sicher zu Cluster eine unterstützte Fabric-Version immer ausgeführt. Wenn wir der Veröffentlichung einer neuen Version von Service Fabric ankündigen, wird die vorherige Version zum Ende mindestens 60 Tage ab dem markiert. die neue Releases sind angekündigten [Service Fabric-Teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Die neue Version steht wählen. 

14 Tage vor Ablauf der Veröffentlichung des Clusters ausgeführt wird, wird ein Ereignis generiert, der Cluster in einer Warnung Zustand versetzt. Cluster bleibt Aktualisierung auf eine unterstützte Fabric in einen Warnzustand.


### <a name="setting-the-upgrade-mode-via-portal"></a>Festlegen der Aktualisierungsmodus Portal 

Cluster kann beim Erstellen des Clusters automatisch oder manuell festgelegt werden.

![Create_Manualmode][Create_Manualmode]

Sie können Cluster automatisch oder manuell auf einem live-Cluster mit festlegen Erfahrung verwalten. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Upgrade auf eine neue Version in einem Cluster, der manuelle Portal Modus.
 
Um eine neue Version zu aktualisieren, müssen Sie lediglich wählen Sie die Version aus und speichern. Fabric-Aktualisierung wird automatisch gestartet. Clusterintegritätsrichtlinien (aus Knoten und die Gesundheit der Anträge im Cluster) eingehalten wird während der Aktualisierung.

Clusterintegritätsrichtlinien nicht erfüllt, wird die Aktualisierung rückgängig gemacht. Scrollen Sie dieses Dokument mehr wie die benutzerdefinierten Richtlinien. 

Nachdem Sie die Probleme, die Rollback geführt haben behoben, müssen Sie das Upgrade erneut anhand derselben Schritte einleiten.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Festlegen der Aktualisierungsmodus über eine Ressourcen-Manager-Vorlage 

Die Definition der Ressource Microsoft.ServiceFabric/clusters "UpgradeMode" Konfiguration hinzufügen "ClusterCodeVersion" auf einen der unterstützten Fabric-Versionen wie folgt festgelegt und Bereitstellen der Vorlage. Die gültigen Werte für "UpgradeMode" sind "Manuell" oder "Automatisch"
 
![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Upgrade auf eine neue Version in einem Cluster, der manuelle über eine Ressourcen-Manager-Vorlage Modus.
 
Wenn der Cluster im manuellen Modus ist zum Aktualisieren auf eine neue Version ändern Sie "ClusterCodeVersion" auf eine unterstützte Version und bereitstellen. Die Bereitstellung der Vorlage tritt der Fabric-Aktualisierung wird gestartet automatisch. Clusterintegritätsrichtlinien (aus Knoten und die Gesundheit der Anträge im Cluster) eingehalten wird während der Aktualisierung.

Clusterintegritätsrichtlinien nicht erfüllt, wird die Aktualisierung rückgängig gemacht. Scrollen Sie dieses Dokument mehr wie die benutzerdefinierten Richtlinien. 

Nachdem Sie die Probleme, die Rollback geführt haben behoben, müssen Sie das Upgrade erneut anhand derselben Schritte einleiten.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Alle verfügbare Version für alle für das angegebene Abonnement abrufen

Führen Sie folgenden Befehl, und Sie erhalten eine Ausgabe ähnlich.

"SupportExpiryUtc" sagt Sie bei eine bestimmte Version läuft oder ist abgelaufen. Die neueste Version verfügt nicht über ein gültiges Datum - Wert von "9999-12-31T23:59:59.9999999", das bedeutet nur, dass das Ablaufdatum noch nicht festgelegt ist.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Fabric-Aktualisierungsverhalten Cluster aktualisieren-Modus wird automatisch

Microsoft behält die Fabric-Code und Konfiguration, die in Azure-Cluster ausgeführt wird. Wir führen automatische überwachten Upgrades der Software auf einen Bedarf. Diese Upgrades möglicherweise Code und Konfiguration. Um sicherzustellen, dass Ihre Anwendung keine oder minimale durch Upgrades leidet, führen wir die Upgrades in die folgenden Phasen:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Phase 1: Aktualisierung erfolgt über alle clusterintegritätsrichtlinien

Während dieser Phase Upgrades eine Aktualisierungsdomäne jeweils fortgesetzt und die im Cluster ausgeführten Programme weiterhin ohne Ausfallzeiten führen. Clusterintegritätsrichtlinien (aus Knoten und die Gesundheit der Anträge im Cluster) eingehalten wird während der Aktualisierung.

Clusterintegritätsrichtlinien nicht erfüllt, wird die Aktualisierung rückgängig gemacht. Dann wird eine e-Mail an den Eigentümer des Abonnements. Die e-Mail enthält die folgende Informationen:

- Benachrichtigung, dass wir eine Cluster-Aktualisierung zurücksetzen.
- Vorgeschlagene Maßnahmen, sofern vorhanden.
- Die Anzahl der Tage (n), bis wir Phase 2 ausführen.

Wir versuchen, dasselbe Update ein paar Mal ausführen bei Upgrades Infrastruktur Gründen fehlgeschlagen ist. Nach n Tagen ab dem Zeitpunkt der e-Mail fortgesetzt in Phase 2.

Clusterintegritätsrichtlinien wird, die Aktualisierung als erfolgreich und als abgeschlossen markiert. Dies geschieht während der ersten Aktualisierung oder eines Upgrades Wiederholungen in dieser Phase. Es ist keine e-Mail-Bestätigung einer erfolgreichen Ausführung. Dies ist zu vermeiden, dass Sie viele e-Mails – eine e-Mail als Ausnahme normal betrachten. Wir erwarten, dass die meisten Cluster-Upgrades, ohne die Verfügbarkeit der Anwendung beeinträchtigen.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Phase 2: Aktualisierung erfolgt über Standard-Integritätsrichtlinien

Integritätsrichtlinien in dieser Phase werden so festgelegt, dass die Zahl der Anträge, die am Anfang des Upgrades fehlerfrei für die Dauer der Aktualisierung unverändert. In Phase 1 Phase 2 Upgrades eine Aktualisierungsdomäne gleichzeitig fortgesetzt, und die im Cluster ausgeführten Programme weiterhin ohne Ausfallzeiten. Clusterintegritätsrichtlinien (aus Knoten Gesundheit und die Anträge im Cluster) für die Dauer der Aktualisierung eingehalten werden.

Clusterintegritätsrichtlinien wirksam nicht erfüllt, wird die Aktualisierung rückgängig gemacht. Dann wird eine e-Mail an den Eigentümer des Abonnements. Die e-Mail enthält die folgende Informationen:

- Benachrichtigung, dass wir eine Cluster-Aktualisierung zurücksetzen.
- Vorgeschlagene Maßnahmen, sofern vorhanden.
- Die Anzahl der Tage (n), bis wir Phase 3 ausführen.

Wir versuchen, dasselbe Update ein paar Mal ausführen bei Upgrades Infrastruktur Gründen fehlgeschlagen ist. Eine Erinnerung ist e-Mail ein paar Tage vor n Tagen. Nach n Tagen ab dem Zeitpunkt der e-Mail fortgesetzt in Phase 3. E-Mails schicken wir Ihnen in Phase 2 ernst genommen und Abhilfemaßnahmen getroffen werden.

Clusterintegritätsrichtlinien wird, die Aktualisierung als erfolgreich und als abgeschlossen markiert. Dies geschieht während der ersten Aktualisierung oder eines Upgrades Wiederholungen in dieser Phase. Es ist keine e-Mail-Bestätigung einer erfolgreichen Ausführung.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Phase 3: Aktualisierung erfolgt über aggressive Integritätsrichtlinien

Richtlinien in dieser sind Abschluss der Aktualisierung als den Zustand der Anwendung ausgerichtet. In dieser Phase Ende sehr wenige Cluster-Upgrades. Wenn Cluster dieser Phase erreicht, ist wahrscheinlich die Anwendung fehlerhaft bzw. Verfügbarkeit verlieren.

Phase 3-Upgrades ist wie die anderen beiden Phasen fortfahren eine Aktualisierungsdomäne gleichzeitig.

Clusterintegritätsrichtlinien nicht erfüllt, wird die Aktualisierung rückgängig gemacht. Wir versuchen, dasselbe Update ein paar Mal ausführen bei Upgrades Infrastruktur Gründen fehlgeschlagen ist. Ist Cluster fixiert, so dass es nicht mehr Support und Upgrades erhalten.

Eine e-Mail mit diesen Informationen wird zusammen mit den Maßnahmen der Abonnementbesitzer. Wir erwarten keine Cluster zu einem Zustand, in Phase 3 fehlgeschlagen ist.

Clusterintegritätsrichtlinien wird, die Aktualisierung als erfolgreich und als abgeschlossen markiert. Dies geschieht während der ersten Aktualisierung oder eines Upgrades Wiederholungen in dieser Phase. Es ist keine e-Mail-Bestätigung einer erfolgreichen Ausführung.

## <a name="cluster-configurations-that-you-control"></a>Cluster-Konfigurationen, die Sie steuern

Aktualisieren zusätzlich die Möglichkeit, den Cluster Modus sind hier die Konfigurationen, die Sie in einem live-Cluster ändern können.

### <a name="certificates"></a>Zertifikate

Sie können neue hinzufügen oder Zertifikate für die Cluster-Client über das Portal einfach löschen. [Dieses](service-fabric-cluster-security-update-certs-azure.md) Dokument Weitere Informationen

![Screenshot mit dem Zertifikatfingerabdruck in Azure-Portal.][CertificateUpgrade]


### <a name="application-ports"></a>Anwendungsports

Sie können ändern Anwendungsports Lastenausgleich Ressourceneigenschaften, die den Knotentyp zugeordnet sind. Das Portal verwenden, oder Ressourcenmanager PowerShell direkt verwenden.

Führen Sie folgende Schritte aus, um einen neuen Port auf VMs in einem Knoten zu öffnen:

1. Entsprechende Lastenausgleich eine neue Abfrage hinzufügen.

    Wenn Sie den Cluster mithilfe der Portalwebsite bereitgestellt, heißen die Lastenausgleich "LB-Name der Ressource Gruppe-NodeTypename", einen für jeden Knoten. Load Balancer Namen innerhalb einer Ressourcengruppe eindeutig sind, ist es am besten, wenn Sie unter einer bestimmten Ressourcengruppe suchen.

    ![Screenshot zeigt einen Prüfpunkt ein Lastenausgleich im Portal hinzufügen.][AddingProbes]

2. Lastenausgleich eine neue Regel hinzugefügt.

    Fügen Sie eine neue Regel gleichen Lastenausgleich mit Probe hinzu, die Sie im vorherigen Schritt erstellt haben.

    ![Hinzufügen einer neuen Regel Lastenausgleich im Portal.][AddingLBRules]


### <a name="placement-properties"></a>Placement-Eigenschaften

Für jeden Knotentyp können Sie benutzerdefinierte Platzierungseigenschaften hinzufügen, die Sie in Ihrer Anwendung verwenden möchten. NodeType ist eine Standardeigenschaft, die Sie verwenden können, ohne explizit hinzufügen.

>[AZURE.NOTE] Details zur Verwendung der Platzierung Constraints-Eigenschaften und die Definition finden Sie im Abschnitt "Platzierung Integritätsregeln und Knoteneigenschaften" im Dokument Service Fabric Cluster Resource Manager [Beschreiben des](service-fabric-cluster-resource-manager-cluster-description.md)Cluster.

### <a name="capacity-metrics"></a>Kapazität Metriken

Für jeden Knotentyp können Sie benutzerdefinierte Kapazität Metriken hinzufügen, die Sie in Ihrer Anwendung laden Bericht verwenden möchten. Weitere Informationen zur Verwendung der Kapazität Metriken Bericht laden, der Service Fabric Cluster Resource Manager Dokumente [Ihres Clusters beschreiben](service-fabric-cluster-resource-manager-cluster-description.md) und [Metriken laden](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Fabric-Upgradeeinstellungen - Richtlinien Gesundheit

Sie können benutzerdefinierte Integritätsrichtlinien für Fabric Aktualisierung angeben. Festlegen des Clusters auf automatische Fabric Upgrades erhalten diese Richtlinien Phase 1 automatische Fabric Upgrades angewendet.
Wenn Sie Cluster für manuelle Fabric Upgrades eingerichtet, erhalten dann diese Richtlinien jedes Mal angewendet wählen Sie eine neue Version Auslösung der Fabric-Aktualisierung im Cluster starten. Wenn Sie die Richtlinien nicht überschreiben, werden die Standardwerte verwendet.

Benutzerdefinierte Integritätsrichtlinien angeben können oder die aktuellen Einstellungen unter Blade "Fabric Upgrade" Upgrade erweitert auswählen. Überprüfen Sie das folgende Bild zum. 

![Benutzerdefinierte Richtlinien verwalten][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Einstellungen Sie Fabric-für den cluster

Siehe [Service Fabric Fabric Clustereinstellungen](service-fabric-cluster-fabric-settings.md) und deren Anpassung.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>Betriebssystem-Patches auf die VMs, die den Cluster bilden

Diese Funktion wird als automatische Funktion für die Zukunft geplant. Aber Sie derzeit für Ihre VMs patch. Eine VM erforderlich zu einem Zeitpunkt, so, dass Sie nicht mehrere gleichzeitig.

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>BS-Upgrades auf die VMs, die den Cluster bilden

Wenn Sie das Betriebssystemabbild auf den virtuellen Computern im Cluster aktualisieren müssen, muss Sie eine VM gleichzeitig. Sie sind verantwortlich für dieses Upgrade – es gibt keine Automatisierung für diese.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum [Service Fabric Fabric Clustereinstellungen](service-fabric-cluster-fabric-settings.md) anpassen
- Erfahren Sie, wie [Ihr Cluster und](service-fabric-cluster-scale-up-down.md)
- Erfahren Sie mehr über [Upgrades der Anwendung](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
<properties
  pageTitle="Azure IoT Suite FAQ | Microsoft Azure"
  description="Häufig gestellte Fragen zum IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Häufig gestellte Fragen zum IoT Suite

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Was ist der Unterschied zwischen eine Ressourcengruppe im Azure-Portal löschen und klicken auf eine vorkonfigurierte Lösung azureiotsuite.com löschen?

- Löschen die vorkonfigurierte Lösung in [azureiotsuite.com][lnk-azureiotsuite], löschen Sie alle Ressourcen, die bereitgestellt wurden, erstellt die vorkonfigurierte Lösung; Wenn Sie zusätzliche Ressourcen der Ressourcengruppe hinzugefügt haben, werden diese ebenfalls gelöscht. 

- Beim Löschen von Ressourcengruppe in [Azure-Portal][lnk-azure-portal], löschen Sie nur die Ressourcen in dieser Ressourcengruppe; Sie müssen auch vorkonfigurierte Lösung im [klassischen Azure-Portal]zugeordnete Active Directory Azure-Anwendung löschen[lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Wie viele IoT Hub Instanzen können in einem Abonnement werden bereitgestellt? 

Zehn. Ein [Azure support-Ticket] können[ link-azuresupportticket] zu dieser Grenzwert jedoch standardmäßig können Sie nur zehn IoT Hubs pro Abonnement bereitstellen, wie in [Azure-Abonnement wird][link-azuresublimits]. Daher, da jede vorkonfigurierte Lösung eine neue IoT Hub Bestimmungen, können Sie nur bis zu zehn vorkonfigurierte Lösungen eines Abonnements bereitstellen. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Wie viele DocumentDB Instanzen können in einem Abonnement werden bereitgestellt?

50. Ein [Azure support-Ticket] können[ link-azuresupportticket] zu dieser Grenzwert jedoch standardmäßig können nur 50 DocumentDB Instanzen pro Abonnement bereitstellen. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Wie viele kostenlose Bing Maps-APIs können in einem Abonnement werden bereitgestellt?

Zwei. Sie können nur zwei interne Transaktionen Ebene 1 Bing Maps für Pläne in Azure-Abonnement erstellen. Die remote monitoring-Lösung wird standardmäßig mit dem internen Transaktionen Ebene1 bereitgestellt. Daher können Sie nur bis zu zwei remote monitoring-Lösungen in einem Abonnement ohne Modifikationen bereitstellen.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Habe ich eine Überwachung Lösung Remotebereitstellung mit einer statischen Zuordnung, wie füge ich einer Bing-Karte? 
1. Holen Sie die Bing Maps-API für Enterprise QueryKey [Azure-Portal][lnk-azure-portal]: 
 1. Navigieren Sie zu der Ressourcengruppe die Bing Maps-API für Unternehmen in [Azure-Portal ist][lnk-azure-portal].
 2. Klicken Sie auf alle, dann Management. 
 3. Sie sehen zwei Schlüssel: MasterKey und QueryKey. Kopieren Sie den Wert für QueryKey.

     > [AZURE.NOTE] Haben kein Bing Maps-API für Unternehmenskunden? Erstellen Sie in [Azure-Portal] [ lnk-azure-portal] von auf + neue Bing Maps-API für Unternehmen suchen und folgen erstellen.

2. Ziehen Sie den neuesten Code der [Azure-IoT-Remote-Überwachung][lnk-remote-monitoring-github].

3. Führen Sie eine lokale oder Commandline Bereitstellungsrichtlinien im Ordner /docs/ im Repository nach Bereitstellung cloud. 

4. Nachdem Sie haben einen lokalen oder cloud-Bereitstellung suchen im Stammordner für die *. user.config-Datei während der Bereitstellung erstellt. Öffnen Sie diese Datei in einem Texteditor. 

5. Ändern Sie folgende Zeile, um den Wert enthalten, die aus der QueryKey kopiert: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kann ich eine vorkonfigurierte Lösung habe Microsoft Azure für DreamSpark erstellen?
Zu diesem Zeitpunkt kann nicht erstellen eine vorkonfigurierte Lösung mit [Microsoft Azure für DreamSpark] [ lnk-dreamspark] Konto. Sie können jedoch ein [kostenloses Testabo für Azure] erstellen[ lnk-30daytrial] in nur wenigen Minuten können Sie eine vorkonfigurierte Lösung erstellen.

### <a name="how-do-i-delete-an-aad-tenant"></a>Wie wird löschen einen AAD-Mandanten?

Blogbeitrag Eric Golpe der [exemplarischen Vorgehensweise ein Azure AD-Mandanten löschen][lnk-delete-aad-tennant].

## <a name="next-steps"></a>Nächste Schritte

Sie können auch die Features und Funktionen IoT Suite vorkonfigurierte Lösung vertraut machen:

- [Vorbeugende Wartung vorkonfiguriert Lösungsübersicht][lnk-predictive-overview]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx

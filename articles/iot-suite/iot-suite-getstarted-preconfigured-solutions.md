<properties
    pageTitle="Erste Schritte mit vorkonfigurierten Lösungen | Microsoft Azure"
    description="Dieses Lernprogramm ein Azure IoT Suite vorkonfigurierte Lösung erläutert."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Lernprogramm: Erste Schritte mit vorkonfigurierten solutions

## <a name="introduction"></a>Einführung

Azure IoT Suite [vorkonfigurierte Lösung] [ lnk-preconfigured-solutions] Kombination mehrere Azure IoT um End-to-End-Lösungen, die allgemeine Szenarien IoT implementieren. Die *Remoteüberwachung* vorkonfigurierte Lösung verbunden und überwacht Ihre Geräte. Die Lösung können den Datenstrom von Geräten zu analysieren und Ergebnisse verbessern, indem Sie Prozesse auf diesem Datenstrom automatisch reagieren.

Dieses Lernprogramm veranschaulicht die remote monitoring vorkonfigurierte Lösung bereitstellen. Außerdem werden Sie über die grundlegenden Funktionen der remote monitoring-Lösung führt. Sie können viele dieser lösungsdashboard zugreifen, die vorkonfigurierte Lösung bereitstellt:

![Remoteüberwachung vorkonfigurierte lösungsdashboard][img-dashboard]

Um dieses Lernprogramm benötigen Sie ein aktives Azure-Abonnement.

> [AZURE.NOTE]  Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Zeigen Sie das lösungsdashboard

Lösungsdashboard können Sie die bereitgestellte Projektmappe verwalten. Sie können z. B. Telemetrie anzeigen, Geräte und Regeln konfigurieren.

1.  Wenn die Bereitstellung abgeschlossen ist, und die Kachel für vorkonfigurierte Lösung **bereit gibt**, klicken Sie auf **Starten** , um das remote monitoring lösungsportal in einer neuen Registerkarte zu öffnen.

    ![Vorkonfigurierte Lösung starten][img-launch-solution]

2.  Standardmäßig zeigt das lösungsportal *lösungsdashboard*. Sie können andere Ansichten im linken Menü auswählen.

    ![Remoteüberwachung vorkonfigurierte lösungsdashboard][img-dashboard]

Das Dashboard zeigt folgende Informationen:

- Die Karte zeigt den Speicherort der einzelnen Geräte zur Lösung. Beim Ausführen der Projektmappe sind vier simulierte Geräte. Die simulierten Geräten als Azure Webaufträge implementiert und Lösung Bing Maps-API verwendet, um Informationen auf der Karte darstellen.
- **Telemetrie** Protokollfenster zeichnet Temperatur und Feuchtigkeit Telemetrie von einem gewählten Gerät in Echtzeit und zeigt aggregierte Daten wie maximal-, Minimal- und durchschnittliche Luftfeuchtigkeit.
- **Alarm** Protokollbedienfeld zeigt Alarmereignisse Telemetrie Wert einen Schwellenwert überschritten hat. Sie können eigene Alarme neben der vorkonfigurierte Lösung erstellt.

## <a name="view-the-device-list"></a>Anzeigen der Liste

Die Liste zeigt alle registrierten Geräte in der Projektmappe. Sie anzeigen Gerätemetadaten bearbeiten, hinzufügen oder Entfernen von Geräten und Befehle an Geräte senden.

1.  Klicken Sie auf **Geräte** im linken Menü der *Geräteliste* für diese Lösung.

    ![Geräteliste im dashboard][img-devicelist]

2.  Die Liste zeigt, dass es vier simulierte Geräte erstellt der Bereitstellungsprozess.

3.  Klicken Sie auf ein Gerät in der Geräteliste um die Gerätedetails anzuzeigen.

    ![Gerätedetails Dashboard][img-devicedetails]

Das Fenster **Gerät** enthält drei Abschnitte:

- Abschnitt **Aktionen** führt die Aktionen auf dem Gerät ausführen. Wenn Sie das Gerät deaktivieren, darf es nicht mehr Telemetrie senden oder Empfangen von Befehlen. Wenn Sie ein Gerät deaktivieren, können Sie es dann erneut aktivieren. Sie können eine Regel das Gerät, das einen Alarm ausgelöst, wenn ein Telemetrie-Wert überschreitet. Sie können auch einen Befehl an ein Gerät senden. Wenn ein Gerät zunächst herstellt, sagt Lösung können sie Befehle.
- **Geräteeigenschaften** aufgeführt die Gerätemetadaten. Diese Metadaten kommt vom Gerät (wie die) und einige der Lösung (wie die erstellte) generiert. Sie können die Gerätemetadaten hier bearbeiten.
- Abschnitt **Authentifizierungsschlüssel** Listet die Tasten, mit denen das Gerät mit der Lösung authentifizieren.

## <a name="send-a-command-to-a-device"></a>Senden eines Befehls an ein Gerät

Im Detailbereich Gerät zeigt alle Befehle, die ein bestimmtes Gerät unterstützt und ermöglicht das Senden eines Geräts Befehle. Beim ersten Start ein Geräts werden Informationen zu den unterstützten Befehlen zur Lösung sendet.

1.  Klicken Sie auf **Befehle** im Detailbereich Gerät für das ausgewählte Gerät.

    ![Gerätebefehle im dashboard][img-devicecommands]

2.  Wählen Sie **PingDevice** aus der Befehlsliste.

3.  Klicken Sie auf **Befehl**.

4.  Sie können den Status des Befehls im Befehlsverlauf anzeigen.

    ![Befehlsstatus im dashboard][img-pingcommand]

Die Lösung verfolgt den Status jedes Befehls sendet. Zunächst ist **Ausstehend**. Wenn das Gerät meldet, dass der Befehl ausgeführt wurde, wird das Resultset für **Erfolg**.

## <a name="add-a-new-simulated-device"></a>Simuliertes neues Gerät hinzufügen

Wenn Sie vorkonfigurierte Lösung bereitstellen, bereitgestellt die vier Sample-Geräte finden Sie in der Liste automatisch. Diese Geräte sind *simulierten Geräten* in einer Azure Webauftrag. Simulierte Geräten erleichtern die vorkonfigurierte Lösung ohne physikalischer Geräte bereitstellen experimentieren. Wenn Sie der Projektmappe ein reales Gerät herstellen möchten, finden Sie unter [Verbinden Sie Ihr Gerät an die entfernte Datenbank überwachen vorkonfigurierte Lösung] [ lnk-connect-rm] Tutorial.

Die folgenden Schritte veranschaulichen die simuliertes Gerät zur Projektmappe hinzufügen:

1.  Navigieren Sie zu der Liste.

2.  Klicken Sie auf **+ Fügen Sie ein Gerät** in der unteren linken Ecke ein Gerät hinzufügen.

    ![Vorkonfigurierte Lösung ein Gerät hinzufügen][img-adddevice]

3.  Klicken Sie auf die Kachel **Simulierten Gerät** **Hinzufügen** .

    ![Neue Gerätedetails Dashboard festlegen][img-addnew]
    
    Neben Neues simuliertes Gerät, können Sie ein physisches Gerät hinzufügen, wenn Sie ein **Benutzerdefiniertes Gerät**erstellen. Weitere Informationen zum Anschließen von Geräten zur Lösung finden Sie unter [Verbinden Sie Ihr Gerät zur Remoteüberwachung vorkonfigurierte Lösung IoT Suite][lnk-connect-rm].

4.  Wählen Sie **mich definieren Sie eigene Geräte-ID**und geben Sie einen eindeutigen Gerätenamen des ID wie **mydevice_01**.

5.  Klicken Sie auf **Erstellen**.

    ![Speichern eines neuen Geräts][img-definedevice]

6. Klicken Sie in Schritt 3 des **simulierten Gerät hinzufügen**auf die Liste wieder **ausgeführt** .

7. Sie können Ihr Gerät **läuft** in der Liste anzeigen.

    ![Neues Gerät in Liste anzeigen][img-runningnew]

8. Sie können auch simulierte Telemetrie vom neuen Gerät auf dem Dashboard anzeigen:

    ![Anzeigen von Telemetriedaten aus neu][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Gerätemetadaten bearbeiten

Wenn ein Gerät zunächst zur Lösung verbindet, sendet er die Metadaten der Lösung. Gerätemetadaten Lösung Dashboard bearbeiten, neue Metadatenwerte an das Gerät sendet und speichert die neuen Werte in der Lösung DocumentDB Datenbank. Weitere Informationen finden Sie unter [geräteidentitätsregistrierung und DocumentDB][lnk-devicemetadata].

1.  Navigieren Sie zu der Liste.

2.  Wählen Sie das neue Gerät in der **Geräteliste**und klicken Sie dann auf **Bearbeiten** , um die **Eigenschaften**zu bearbeiten:

    ![Gerätemetadaten bearbeiten][img-editdevice]

3. Bildlauf und Täler Breiten- und Längengrad ändern. Klicken Sie auf **Geräte-Registrierung speichern**.

    ![Gerätemetadaten bearbeiten][img-editdevice2]

4. Navigieren Sie zum Dashboard, der Standort des Geräts geändert auf der Karte:

    ![Gerätemetadaten bearbeiten][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Eine Regel für das neue Gerät hinzufügen

Es gibt keine Regeln für das neue Gerät gerade hinzugefügt. In diesem Abschnitt fügen Sie eine Regel, die einen Alarm auslöst, wenn das neue Gerät 47 Grad überschreitet. Bevor Sie beginnen, beachten Sie, dass die Telemetrie Geschichte für das neue Gerät auf dem Dashboard zeigt, Gerät bei 45 Grad nicht überschreiten.

1.  Navigieren Sie zu der Liste.

2.  Wählen Sie das neue Gerät in der **Geräteliste**und klicken Sie dann auf **Regel hinzufügen** , um eine Regel für das Gerät hinzuzufügen.

3. Erstellen Sie eine Regel, die **Temperatur** als Feld verwendet und **AlarmTemp** Ausgabe überschreitet die Temperatur 47 Grad:

    ![Hinzufügen einer Regel][img-adddevicerule]

4. Klicken Sie auf **Speichern und anzeigen** der speichern.

5.  Klicken Sie auf **Befehle** im Detailbereich Gerät für das neue Gerät.

    ![Hinzufügen einer Regel][img-adddevicerule2]

6.  Wählen Sie **ChangeSetPointTemp** aus der Befehlsliste und **SetPointTemp** auf 45 festgelegt. Klicken Sie auf **Befehl senden**:

    ![Hinzufügen einer Regel][img-adddevicerule3]

7.  Navigieren Sie zum lösungsdashboard. Nach kurzer Zeit sehen Sie einen neuen Eintrag im Bereich **Alarmverlauf** durch das neue Gerät gemeldete Temperatur 47 Grad Schwellenwert überschreitet:

    ![Hinzufügen einer Regel][img-adddevicerule4]

8. Sie überprüfen und bearbeiten die Regeln auf die **Regeln** des Dashboards:

    ![Listenregeln][img-rules]

9. Sie überprüfen und bearbeiten alle Aktionen, die als Antwort auf eine Regel auf der Seite **Aktionen** des Dashboards können:

    ![Liste Aktionen][img-actions]

> [AZURE.NOTE] Aktionen, das Senden von e-Mail oder SMS auf eine Regel oder ein LOB System über eine [Logik App]integrieren kann[lnk-logic-apps]. Weitere Informationen finden Sie unter [Verbinden Logik App die Remoteüberwachung von Azure IoT Suite vorkonfigurierte Lösung][lnk-logicapptutorial].

## <a name="other-features"></a>Andere Funktionen

Lösungsportal können Sie verwenden Geräte mit bestimmten Merkmalen wie eine Modellnummer:

![Suche ein][img-search]

Deaktivieren Sie ein Gerät und Deaktivierung ist entfernen:

![Deaktivieren und Entfernen des Geräts][img-disable]

## <a name="behind-the-scenes"></a>Hinter den Kulissen

Wenn Sie eine vorkonfigurierte Lösung bereitstellen, wird im Bereitstellungsprozess mehrere Ressourcen Azure Abonnement ausgewählten. Können diese Ressourcen in Azure- [Portal][lnk-portal]. Verfahren erstellt eine **Ressourcengruppe** mit dem Namen basierend auf dem Namen für die vorkonfigurierte Lösung wählen:

![Vorkonfigurierte Lösung im Azure-portal][img-portal]

Sie können die Einstellungen der einzelnen Ressourcen auswählen in der Liste der Ressourcen in der Ressourcengruppe anzeigen.

Sie können auch den Quellcode für die vorkonfigurierte Lösung anzeigen. Remote monitoring vorkonfigurierte Lösung-Quellcode ist in der [Azure-Iot-Remote-Überwachung] [ lnk-rmgithub] GitHub Repository:

- Der Ordner **DeviceAdministration** enthält den Quellcode für das Dashboard.
- Der **Simulator** -Ordner enthält den Quellcode für das Ausgabegerät.
- Der Ordner **EventProcessor** enthält den Quellcode für den Back-End-Prozess, der eingehende Telemetrie behandelt.

Wenn Sie fertig sind, Sie können vorkonfigurierte Lösung aus dem Azure-Abonnement auf [azureiotsuite.com] [ lnk-azureiotsuite] Website. Hier können Sie die Ressourcen problemlos löschen, die beim Erstellen der vorkonfigurierte Lösung bereitgestellt wurden.

> [AZURE.NOTE] Um sicherzustellen, dass alles vorkonfigurierte Lösung löschen, löschen sie [azureiotsuite.com] [ lnk-azureiotsuite] Website und die Ressourcengruppe im Portal nicht einfach gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Bereitstellung eine vorkonfigurierte Lösung fahren Sie erste Schritte mit IoT Suite durch die folgenden Artikel lesen:

- [Remoteüberwachung vorkonfigurierte Lösung Exemplarische Vorgehensweise][lnk-rm-walkthrough]
- [Verbinden Sie das Gerät remote monitoring vorkonfigurierte Lösung][lnk-connect-rm]
- [Berechtigungen auf der Website azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md

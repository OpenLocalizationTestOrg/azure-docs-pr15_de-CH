<properties 
    pageTitle="Häufige Cloud Service-Verwaltungsaufgaben (Classic) | Microsoft Azure" 
    description="Informationen Sie zum Clouddienste im klassischen Azure-Portal verwalten." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Cloud-Dienste verwalten

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-manage-portal.md)
- [Azure-Verwaltungsportal](cloud-services-how-to-manage.md)

Im Bereich **Cloud-Dienste** des klassischen Azure-Portal können Service-Rolle oder eine Bereitstellung aktualisieren, fördern eine gestaffelte Bereitstellung für die Produktion, verknüpfen Ressourcen zu Ihrem Clouddienst finden Sie unter Resource Dependencies und Ressourcen zusammen skalieren und einen Cloud-Dienst oder einer Bereitstellung löschen.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Gewusst wie: Aktualisieren einer Cloud-Rolle oder Bereitstellung

Benötigen Sie den Anwendungscode für den Clouddienst aktualisieren, verwenden Sie **Aktualisieren** auf der Dashboard, **Cloud-Services** oder **Instanzen** . Sie können eine einzelne Rolle oder allen Rollen aktualisieren. Sie müssen ein neues Servicepaket und Dienst hochzuladen.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf Dashboard, **Cloud-** Seite oder **Instanzen** Seite auf **Aktualisieren**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Bereitstellung Bezeichnung**Geben Sie einen Namen für die Bereitstellung (z. B. mycloudservice4). Die Bezeichnung Bereitstellung unter **Schnellstart** finden Sie im Schaltpult.

3. Verwenden Sie in **Paket** **Durchsuchen** das Service-Paket (.cspkg) hochladen.

4. Verwenden Sie in der **Konfiguration** **Durchsuchen** hochzuladenden Dienstkonfigurationsdatei (.cscfg).

5. **Rolle**wählen Sie **Alle** , wenn alle Rollen im Cloud-Dienst aktualisieren. Wählen Sie zum Aktualisieren einer einzelnen Rolle die Rolle, die Sie aktualisieren möchten. Auch wenn Sie eine bestimmte Rolle aktualisieren auswählen, werden Updates in Dienstkonfigurationsdatei alle Rollen angewendet.

6. Ändert die Aktualisierung der Anzahl der Rollen oder die Größe einer Rolle, aktivieren Sie das Kontrollkästchen **Zulassen aktualisieren Rolle Größe oder Anzahl der Rollen** ermöglichen die Aktualisierung fortgesetzt. 

    Wenn die Größe einer Rolle (die Größe eines virtuellen Computers, der eine Rolleninstanz hostet) oder die Anzahl der Rollen ändern, muss jede Instanz der Rolle (VM) neu abgebildeten und lokalen Daten verloren sein.

7. Haben alle Rollen nur eine Instanz der Rolle, aktivieren Sie das **aktualisieren, auch wenn eine oder mehrere Rollen enthalten ein Kontrollkästchen Instanz** ermöglichen die Aktualisierung fortgesetzt. 

    Azure garantieren nur 99,95 % Verfügbarkeit während ein Cloud-Update, hat jede Rolle mindestens zwei Instanzen (virtuelle Maschinen). Dies ermöglicht ein virtueller Computer Clientanforderungen zu verarbeiten, während der andere aktualisiert wird.

8. Klicken Sie auf **OK** (Häkchen) zum Aktualisieren des Dienstes zu beginnen.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Gewusst wie: Bereitstellung eine gestaffelte Bereitstellung für die Produktion zu tauschen

Verwenden Sie **tauschen** zu staging-Bereitstellung eines Cloud-Diensts für die Produktion. Wenn Sie eine neue Version eines Cloud-Diensts bereitstellen möchten, können bereitstellen und Testen der neuen Version in Cloud Service Stagingumgebung, während Ihre Kunden die aktuelle Version in der Produktion verwenden. Wenn Sie zu der neuen Version für die Produktion bereit sind, können **austauschen** Sie URLs wechseln, zwei Installationen behandelt werden. 

Bereitstellung von **Cloud-** Seite oder das Dashboard tauschen.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf **Clouddienste**.

2. Klicken Sie in der Cloud-Dienste auf Cloud-Dienst, um es auszuwählen.

3. Klicken Sie auf **Ersetzen**.

    Die folgende Aufforderung zur Bestätigung wird angezeigt.

    ![Cloud-Dienste austauschen](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Nachdem Sie die Bereitstellung zu überprüfen, klicken Sie auf **Ja** Swap Bereitstellungen.

    Bereitstellung Swap geschieht schnell, da ändert lediglich die virtuellen IP-Adressen (VIPs) ist für die Bereitstellung.

    Compute Kostengründen können Sie die Bereitstellung in die Stagingumgebung löschen, wenn Sie sicher sind, dass die neuen Produktionsbetrieb erwartungsgemäß.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Gewusst wie: verknüpfen eine Ressource auf einen Clouddienst

Um Ihre Cloud dienstabhängig auf andere Ressourcen anzuzeigen, können Sie die Cloud-Dienst einer Azure SQL-Datenbankinstanz oder ein Speicherkonto verknüpfen. Sie können Verknüpfen Verknüpfung auf der Seite **Verknüpfte Ressourcen** und Überwachen ihrer Verwendung im Schaltpult Cloud-Dienst. Verfügt ein verknüpftes Speicherkonto Überwachung aktiviert ist, können Sie Anfragen insgesamt Cloud Service Dashboard überwachen.

Verwenden Sie **Link** zu Ihrem Clouddienst eine neue oder vorhandene SQL-Instanz oder Storage Konto verknüpfen. Sie können dann die Datenbank zusammen mit Cloud-Dienst Rollen skalieren, die auf der Seite **Skalierung** verwendet. (Ein Speicherkonto skaliert automatisch als Verwendung erhöht.) Weitere Informationen finden Sie unter [wie einen Cloud-Dienst und verknüpfte Ressourcen](cloud-services-how-to-scale.md). 

Auch können Sie überwachen, verwalten, und Skalieren die Datenbank in den Knoten **Datenbanken** klassischen Azure-Portal. 

"Verknüpfen" Ressource in diesem Sinne herstellen nicht Ihre Anwendung mit der Ressource. Wenn Sie eine neue Datenbank mit **Link**erstellen, müssen Sie Anwendungscode Verbindungszeichenfolgen hinzu, und aktualisieren Sie den Cloud-Dienst. Sie müssen auch Verbindungszeichenfolgen hinzufügen, wenn Ihre Anwendung in einem verknüpften Speicherkonto Ressourcen verwendet.

Im folgenden wird beschrieben, wie eine neue SQL-Datenbankinstanz auf einem neuen SQL-Datenbank-Server an einen Clouddienst bereitgestellt.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Verknüpfen eine SQL-Datenbankinstanz auf einen Clouddienst

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Clouddienste**. Klicken Sie dann auf den Namen des Cloud-Dienst, um das Dashboard zu öffnen.

2. Klicken Sie auf **Verknüpfte Ressourcen**.

    **Verknüpfte Ressourcen** angezeigt.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klicken Sie auf **Link Ressource** oder **Link**.

    Der **Link** -Assistent wird gestartet.

    ![Link Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klicken Sie auf **neue Ressource erstellen** oder **eine vorhandene Ressource verknüpfen**.

5. Wählen Sie den Typ der Ressource verknüpfen. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **SQL-Datenbank**. (Vorschau Azure-Verwaltungsportal unterstützt kein Speicherkonto mit Cloud-Dienst verknüpft.)

6. Um die Konfiguration abzuschließen Anweisungen Sie in der Hilfe für den Bereich **SQL-Datenbanken** des klassischen Azure-Portal.

    Sie können den Status des Verknüpfungsvorgangs im Bereich folgen.

    ![Link-Status](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Abschluss verknüpfen, können Sie den Status der verknüpfte Ressource im Schaltpult Cloud Service überwachen. Informationen zum Skalieren einer verknüpften SQL-Datenbank finden Sie unter [wie einen Cloud-Dienst und verknüpfte Ressourcen](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Eine verknüpfte Ressource trennen

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Clouddienste**. Klicken Sie dann auf den Namen des Cloud-Dienst, um das Dashboard zu öffnen.

2. Klicken Sie auf **Verknüpfte Ressourcen**, und wählen Sie die Ressource.

3. Klicken Sie auf **Verknüpfung aufheben**. Klicken Sie dann auf **Ja** zur Bestätigung aufgefordert.

    Aufheben der Verknüpfung einer SQL-Datenbank hat keine Auswirkung auf die Datenbank oder die Anwendung Verbindungen mit der Datenbank. Weiterhin können Sie die Datenbank im Bereich **Datenbanken SQL** Azure-Verwaltungsportal verwalten.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Gewusst wie: Bereitstellung und Cloud-Dienst löschen

Cloud-Dienst löschen zu können, müssen Sie jede vorhandene Bereitstellung löschen.

Compute Kostengründen können staging-Bereitstellung löschen Sie nach dem überprüfen die Bereitstellung in der Produktion wie erwartet funktioniert. Sie sind Rechnung berechnen Kosten für Instanzen, auch wenn ein Cloud-Dienst nicht ausgeführt wird.

Gehen Sie zum Löschen einer Bereitstellung oder dem Cloud-Dienst. 

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Clouddienste**.

2. Wählen Sie Cloud-Dienst aus und dann auf **Löschen**. (Markieren Sie einen Cloud-Dienst ohne Öffnen des Dashboards klicken Sie außer den Namen Cloud Service Eintrag.)

    Haben Sie eine Bereitstellung in Staging oder die Produktion sehen Sie ähnlich dem folgenden unten im Fenster zur Auswahl. Cloud-Dienst löschen zu können, müssen Sie alle vorhandenen Bereitstellungen löschen.

    ![Menü löschen](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Klicken Sie zum Löschen einer Bereitstellung **Löschen Produktionsbetrieb** oder **staging-Bereitstellung**. Um Bestätigung gebeten werden, klicken Sie auf **Ja**. 

4. Wenn Sie Cloud-Dienst löschen möchten, wiederholen Sie Schritt 3, ggf. die Bereitstellung löschen.

5. Klicken Sie Cloud-Dienst **Cloud-Dienst löschen**. Um Bestätigung gebeten werden, klicken Sie auf **Ja**.

> [AZURE.NOTE]
> Wenn ausführliche Überwachung für den Clouddienst konfiguriert ist, wird Azure nicht gelöscht Überwachungsdaten aus Ihrem Speicherkonto Cloud-Dienst löschen. Sie müssen die Daten manuell löschen. Informationen über die Metriken Tabellen finden Sie unter "wie: Zugriff auf ausführliche Überwachungsdaten außerhalb der Azure-Verwaltungsportal" wie [Monitor Clouddienste](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Nächste Schritte

 * [Konfiguration des Cloud-Dienst](cloud-services-how-to-configure.md).
* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name.md).
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate.md).

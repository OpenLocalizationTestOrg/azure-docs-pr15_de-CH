<properties
    pageTitle="Erste Schritte mit Protokollanalyse | Microsoft Azure"
    description="Sie erhalten läuft mit Protokollanalyse in Microsoft Operations Management Suite (OMS) in Minuten."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Erste Schritte mit Protokollanalyse

Sie erhalten läuft mit Protokollanalyse in Microsoft Operations Management Suite (OMS) in Minuten. Bei einen OMS-Arbeitsbereich erstellen, ähnlich einem Konto stehen Ihnen zwei Optionen zur Verfügung:

- Website des Microsoft Operations Management Suite
- Microsoft Azure-Abonnement

Sie können einen kostenlosen OMS Arbeitsbereich OMS-Website erstellen. Oder können Sie ein Microsoft Azure-Abonnement um einen OMS-Arbeitsbereich zu erstellen. Beide Arbeitsbereiche sind funktional äquivalent, aber freier OMS-Arbeitsbereich nur 500 MB an Daten täglich an den OMS-Dienst senden kann. Bei Verwendung von Azure-Abonnement auch können dieses Abonnement Sie andere Azure-Dienste zugreifen. Unabhängig von der Methode, die Sie verwenden, um den Arbeitsbereich zu erstellen, erstellen Sie den Arbeitsbereich mit einem Microsoft-Konto oder Organisationseinheit Konto.

Hier ist eine Betrachtung des Prozesses:

![Onboarding-Diagramm](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Log Analytics Komponenten und Bereitstellung

- Sie benötigen Microsoft Azure Abonnementversion Protokollanalyse nutzen. Haben Sie ein Azure-Abonnement ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen Azure Dienste zugreifen. Oder erstellen Sie ein kostenloses Konto OMS [Operations Management Suite](http://microsoft.com/oms) -Website und klicken Sie auf **ausprobieren**.
- OMS-Arbeitsbereich
- Jedem Windows erfasst Daten führen Windows Server 2008 SP1 oder höher
- [Firewall](log-analytics-proxy-firewall.md) auf der OMS Dienst Webadressen
- Ein [OMS Protokoll Analytics Weiterleitung](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (Gateway) Server Datenverkehr von Servern OMS übermitteln ist Internetzugang nicht von Computern
- Verwenden Sie Operations Manager Protokollanalyse unterstützt UR6 von Operations Manager 2012 SP1 und höher und Operations Manager 2012 R2 UR2 und höher. Proxy wurde in Operations Manager 2012 SP1 UR7 und Operations Manager 2012 R2 UR3 unterstützt. Bestimmen Sie, wie sie OMS integriert werden.
- Feststellen Sie, ob Computer direkten Internetzugriff haben. Wenn nicht, benötigen sie einen Gatewayserver OMS-Dienst Websites zugreifen. Zugriff ist über HTTPS.
- Bestimmen Sie, welche Technologie und Server Daten in OMS sendet. Z. B. Domänencontroller, SQL Server usw.
- OMS und Azure Benutzern erteilen.
- Wenn Sie Daten besorgt sind, jede Projektmappe einzeln bereitstellen und Testen der Leistungseinbußen vor dem Hinzufügen Weitere Lösungsmöglichkeiten.
- Überprüfen Sie Ihre Daten und Leistung Protokollanalyse Lösungen und Features hinzufügen. Dazu gehören Ereignissammlung, Protokollsammlung, Leistungsdaten. Es ist besser mit minimalen Daten-Auflistung oder Leistungseinbußen wurde erkannt.
- Überprüfen Sie Windows-Agenten nicht auch verwaltet mit Operations Manager andernfalls doppelte Daten führt. Dies gilt auch für Azure--Agenten, der Azure-Diagnose aktiviert haben.
- Nach der Installation des Agenten überprüfen Sie, ob der Agent ordnungsgemäß funktioniert. Wenn nicht, überprüfen Sie die Kryptografie-API: Next Generation (CNG) Schlüssel Isolation nicht mit Gruppenrichtlinien deaktiviert.
- Weitere benötigen Lösungsmöglichkeiten Protokollanalyse



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>3 Schritte mit Operations Management Suite anmelden

1. Besuchen Sie die Website [Operations Management Suite](http://microsoft.com/oms) und auf **ausprobieren**. Ihr Microsoft-Konto wie Outlook.com oder organisatorische Konto von Ihrem Unternehmen oder eine Bildungseinrichtung mit Office 365 oder anderen Microsoft-Diensten anmelden.
2. Benennen Sie eindeutige Arbeitsbereich. Ein Workspace ist ein logischer Container, in dem Ihre Daten gespeichert ist. Es bietet eine Möglichkeit zur Partitionierung von Daten zwischen verschiedenen Teams in der Organisation während nur in ihren Arbeitsbereichen werden. Geben Sie eine e-Mail-Adresse und den Bereich der Daten gespeichert werden soll.  
    ![Arbeitsbereich erstellen und Verknüpfen von Abonnements](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Anschließend können Sie ein neues Azure-Abonnement oder eine Verknüpfung auf ein Azure-Abonnement erstellen. Möchten Sie mit der Testversion fortzufahren, klicken Sie auf **Jetzt nicht**.  
  ![Arbeitsbereich erstellen und Verknüpfen von Abonnements](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Sie sind mit dem Portal Operations Management Suite bereit.

Sie erfahren mehr Arbeitsbereich einrichten und Verknüpfen von vorhandenen Azure Arbeitsbereiche mit Operations Management Suite auf [Verwalten Protokollanalyse](log-analytics-manage-access.md)erstellten Konten.

## <a name="sign-up-quickly-using-microsoft-azure"></a>Melden Sie sich schnell mit Microsoft Azure

1. Zum [Azure-Portal](https://portal.azure.com) anmelden, Durchsuchen Sie die Liste der Dienste und wählen Sie **Protokoll Analytics (OMS)**.  
    ![Azure-portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klicken Sie auf **Hinzufügen**, und wählen Sie Optionen für die folgenden Elemente:
    - **OMS** Arbeitsbereichsname
    - **Abonnement** - haben Sie mehrere Abonnements wählen Sie neuer Arbeitsbereich zuordnen möchten.
    - **Ressourcengruppe**
    - **Speicherort**
    - **Preisstufe**  
        ![Schnelles Erstellen](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klicken Sie auf **Erstellen** und Sie sehen die Arbeitsbereich-Details im Azure-Portal.       
    ![Arbeitsbereich-details](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Klicken Sie auf **OMS-Portal** , um die Website Operations Management Suite mit Ihren neuen Arbeitsbereich zu öffnen.

Sie sind jetzt über das Portal Operations Management Suite.

Mehr erfahren über den Arbeitsbereich einrichten und Verknüpfen vorhandener Arbeitsbereiche mit Operations Management Suite Azure Abonnements [Verwalten](log-analytics-manage-access.md)auf Protokollanalyse erstellt.

## <a name="get-started-with-the-operations-management-suite-portal"></a>Erste Schritte mit Operations Management Suite-portal
Lösungen und die zu verwaltenden Server, **Einstellungen** klicken und die Schritte in diesem Abschnitt.  

![Erste Schritte](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Projektmappen hinzufügen** - die installierte Lösungen.  
    ![Solutions](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klicken Sie auf Weitere Lösungen **Galerie** .  
    ![Solutions](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Wählen Sie eine Lösung, und klicken Sie auf **Hinzufügen**.
2. **Verbinden einer Quelle** – wählen Sie, wie Ihre Server-Umgebung Daten herstellen:
    - Werden mit jedem Windows-Server oder Client direkt Installieren eines Agents.
    - Linux-Server mit dem OMS-Agent für Linux anschließen
    - Verwenden Sie ein Azure-Speicher-Konto mit dem Windows oder Linux Azure Diagnose VM konfiguriert.
    - Verwenden Sie System Center Operations Manager, die Gruppen oder die gesamte Bereitstellung von Operations Manager.
    - Aktivieren Sie Windows Telemetrie Analytics aktualisieren verwenden.
        ![verbundene Datenquellen](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Sammeln Sie Daten** Konfigurieren Sie mindestens eine Datenquelle, um Daten im Arbeitsbereich aufzufüllen. Danach klicken Sie auf **Speichern**.    

    ![Sammeln Sie Daten](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Optional Server direkt Verbindung Operations Management Suite durch Installieren eines Agents

Das folgende Beispiel veranschaulicht die Windows-Agent installieren.

1. Klicken Sie **Einstellungen** , klicken Sie auf die Registerkarte **Quellen verbunden** , klicken Sie auf einer Registerkarte für die Herkunftsart hinzufügen möchten und entweder Download Agent oder erfahren Sie Agent aktivieren. Klicken Sie beispielsweise auf **Downloaden Windows-Agent (64 Bit)**. Für Windows-Agenten können Sie nur den Agenten für Windows Server 2008 SP1 oder höher oder Windows 7 SP1 oder höher installieren.
2. Installieren Sie den Agent auf einem oder mehreren Servern. Agenten einzeln installieren oder ein [benutzerdefiniertes Skript](log-analytics-windows-agents.md)oder eine automatisierte Methode mit vorhandenen Software-Verteilung-Lösung, die Sie möglicherweise verwenden.
3. Nachdem Sie dem Lizenzvertrag zustimmen und dem Installationsordner auswählen, wählen Sie **Verbinden Agent auf Azure Protokoll Analytics (OMS)**.   
    ![Agent-setup](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Auf der nächsten Seite müssen Sie für die Arbeitsplatz-ID und Arbeitsbereich Schlüssel. Die Arbeitsplatz-ID und Schlüssel werden auf dem Bildschirm angezeigt, in dem die Agent-Datei heruntergeladen.  
    ![Agent-Schlüssel](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![Schließen Sie Server](./media/log-analytics-get-started/oms-onboard-key.png)
5. Klicken Sie während der Installation **Erweitert** optional den Proxyserver und Authentifizierungsinformationen bereitstellen. Klicken Sie auf **Weiter** , um zu dem Arbeitsbereich zurückzukehren.
6. Klicken Sie auf **Weiter** um die Arbeitsplatz-ID und Schlüssel überprüfen. Wenn Fehler gefunden werden, können Sie **wieder** zum Korrigieren klicken. Die Arbeitsplatz-ID und Schlüssel überprüft werden, klicken Sie auf **Installieren** , um die Agentinstallation abzuschließen.
7. Klicken Sie im Steuerungsbedienfeld auf Microsoft Monitoring Agent > Registerkarte Azure Protokoll Analytics (OMS). Ein grünes Häkchen Symbol Operations Management Suite Service Agents kommunizieren. Zunächst gelangen ca. 5 bis 10 Minuten.

>[AZURE.NOTE] Kapazität und Konfiguration Assessment-Lösungen sind direkt mit Operations Management Suite verbundenen Server derzeit nicht unterstützt.


Sie können auch den Agent auf System Center Operations Manager 2012 SP1 und höher. Hierzu wählen Sie **verbinden die System Center Operations Manager-Agent**. Wenn Sie auswählen, Option, Daten an den Dienst senden, ohne zusätzlichen Hardware oder der Verwaltungsgruppen laden.

Erfahren Sie mehr über Operations Management Suite [Verbinden Windows](log-analytics-windows-agents.md)-Computer zu Protokollanalyse Agenten herstellen.

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Optional, Verbinden von Servern mit System Center Operations Manager

1. Wählen Sie in der Operations Manager-Konsole **Verwaltung**.
2. Erweitern Sie den Knoten **Betriebliche Informationen** und wählen Sie **Operative Einblicke Verbindung**.

  >[AZURE.NOTE] Je nachdem, welche Update Rollup von SCOM Sie verwenden, sehen Sie einen Knoten für *System Center Advisor*, *Betriebliche Informationen*oder *Operations Management Suite*.

3. Klicken Sie auf **registrieren, um betriebliche Informationen** oben rechts und folgen.
4. Klicken Sie nach Abschluss der Registrier-Assistent, **Hinzufügen einer Computergruppe** .
5. Klicken Sie im Dialogfeld **Computer suchen** können Sie Computer oder Gruppen von Operations Manager überwacht suchen. **Computer oder Gruppen auswählen auf integrierte sie auf Protokollanalyse und klicken Sie auf **Hinzufügen**.** Sie können überprüfen, ob der OMS-Dienst Daten empfängt, auf der Kachel **Verwendung** im Portal Operations Management Suite. Daten sollten in etwa 5 bis 10 Minuten angezeigt.

Erfahren Sie mehr über Operations Management Suite zur [Protokollanalyse Operations Manager verbinden](log-analytics-om-agents.md)mit Operations Manager.

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Optional Datenanalyse von Cloud-Diensten in Microsoft Azure

Mit Operations Management Suite können Sie Ereignis- und IIS-Protokolle für Cloud-Dienste und virtuelle Maschinen schnell suchen, Diagnose für Azure Cloud Services aktivieren. Auch erhalten zusätzliche Hinweise für die Azure virtuellen Computer Sie von Microsoft Monitoring Agent installieren. Weitere Informationen zum Konfigurieren der Azure-Umgebung [Verbinden Azure Speicher Protokollanalyse](log-analytics-azure-storage.md)Operations Management Suite verwenden.


## <a name="next-steps"></a>Nächste Schritte

- [Protokollanalyse hinzufügen Lösungen Lösungskatalog](log-analytics-add-solutions.md) Funktionen und Daten.
- Vertraut mit [Protokoll suchen](log-analytics-log-searches.md) Solutions ausführlichen Informationen anzeigen.
- Verwenden Sie [Dashboards](log-analytics-dashboards.md) zu speichern eigene Suchvorgänge.

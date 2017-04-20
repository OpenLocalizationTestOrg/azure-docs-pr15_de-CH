<properties
pageTitle="Verwenden Sie Daten aus einer lokalen SQL Server-Datenbank für maschinelles lernen | Azure"
description="Verwenden Sie Daten aus einer lokalen SQL Server-Datenbank, um erweiterte Analyse mit Azure Computer auszuführen."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Führen Sie erweiterte Analysen mit Azure Computer mit Daten aus einer lokalen SQL Server-Datenbank aus


Häufig mit lokalen Daten arbeiten Unternehmen möchte die Skalierung und Flexibilität der Cloud für ihre Computer lernen Arbeitslasten nutzen. Aber sie möchten ihre aktuellen Geschäftsprozesse und Workflows stören durch ihre lokalen Daten in die Cloud verschieben. Azure Machine Learning unterstützt jetzt die Daten aus einer lokalen SQL Server-Datenbank lesen und training und Bewertung eines Modells mit diesen Daten. Sie müssen nicht mehr manuell kopieren und synchronisieren die Daten zwischen der Cloud und dem lokalen Server. Stattdessen erhalten Moduls **Import** in Azure Machine Learning Studio jetzt direkt von der lokalen SQL Server-Datenbank für die Ausbildung und Bewertung Aufträge. 

Dieser Artikel bietet Eindringen wie SQL Serverdaten in Azure Computer lokal. Es wird vorausgesetzt, dass Sie Azure Machine Learning Konzepte wie *Arbeitsbereiche Module, Datasets, Experimente usw.*kennen...

> [AZURE.NOTE] Dieses Feature ist nicht verfügbar für kostenlose Arbeitsbereiche. Weitere Informationen zu Preisen maschinelles lernen und Ebenen finden Sie unter [Azure Machine Learning Preise](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Installieren Sie Microsoft Data Management Gateway

Zugriff auf eine lokale SQL Server-Datenbank in Azure maschinelles lernen müssen Sie downloaden und installieren Microsoft Data Management Gateway. Beim Konfigurieren der Verbindungs Gateway in Machine Learning Studio haben Sie die Möglichkeit zum Downloaden und installieren das Gateway mit dem **herunterladen und Register datengateway** Dialogfeld beschrieben.

Data Management Gateway voraus können durch Downloaden und Ausführen von msi-Setup-Paket im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Wählen Sie die neueste 32-Bit- oder 64-Bit-Computers entsprechend auswählen. Die MSI-Datei kann auch verwendet werden, erhalten alle Einstellungen eines vorhandenen Daten-Management-Gateways auf die neueste Version aktualisieren.

Das Gateway hat die folgenden Komponenten:

- Die unterstützten Windows-Betriebssystemversionen sind Windows 7, Windows 8 / 8.1, 10 Windows, Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2.
- Die empfohlene Konfiguration für den Gatewaycomputer ist mindestens 2 GHz, 4 Kernen, 8 GB RAM und 80 GB Festplatte.
- Wenn der Hostcomputer im Ruhezustand wird nicht das Gateway auf Anfragen reagieren kann. Konfigurieren Sie daher einen entsprechenden Energiesparplan auf dem Computer vor der Installation des Gateways. Die Gateway-Installation wird eine Meldung angezeigt, wenn der Computer den Ruhezustand konfiguriert ist.
- Da Copy Aktivitäten mit einer bestimmten Häufigkeit auftritt, folgt die Ressourcenverwendung (CPU, Speicher) auf dem Computer auch das gleiche Muster mit Spitzen- und Nebenzeiten. Ressourcenverwendung hängt auch die Menge der Daten verschoben werden. Wenn mehrere Einzelvorgänge werden werden Ressourcenverwendung gehen während der Spitzenzeiten beobachten. Während die Mindestkonfiguration aufgeführten technisch ausreichend ist, sollten Sie eine Konfiguration mit mehr Ressourcen als die Mindestkonfiguration je nach der spezifischen Belastung für Daten.

Sie sollten beim Einrichten und verwenden eine Management-Gateway die folgenden:

- Sie können nur eine Instanz des Data Management Gateway auf einem einzelnen Computer installieren.
- Sie können ein Gateway für mehrere lokale Datenquellen verwenden.
- Sie können lokale Datenquelle mehrere Gateways auf verschiedenen Computern verbinden.
- Sie konfigurieren einen Gateway für nur ein Arbeitsbereich gleichzeitig. Gateways können Arbeitsbereiche hinweg zu diesem Zeitpunkt freigegeben werden.
- Sie können mehrere Gateways für einen Arbeitsbereich. Sie möchten z. B. ein Gateway mit Datenquellen Tests während der Entwicklung und Produktion Gateway verwenden, wenn Sie durchsetzen möchten.
- Das Gateway muss nicht auf demselben Computer wie die Datenquelle aber bleiben näher an der Datenquelle reduziert die Zeit für das Gateway für die Verbindung zur Datenquelle. Wir empfehlen das Gateway auf einem Computer installieren, der anders ist, die lokale Datenquelle hostet, das Gateway und die Datenquelle nicht für Ressourcen konkurrieren.
- Haben Sie bereits ein Gateway auf dem Computer mit Power BI oder Azure Data Factory Szenarien installiert, installieren Sie ein separates Gateway für Azure Machine Learning auf einem anderen Computer. 

    > [AZURE.NOTE] Data Management Gateway und Power BI Gateway kann nicht auf demselben Computer ausgeführt werden.

- Sie müssen Daten Management Gateway für Azure Machine Learning verwenden, auch wenn Sie Azure ExpressRoute für andere Daten verwenden. Die Datenquelle sollte als eine lokale Datenquelle (Firewall) behandeln auch wenn Sie ExpressRoute verwenden und Daten-Management-Gateway Konnektivität zwischen Computerlernen und der Datenquelle hergestellt. 

Ausführliche Informationen zu Installation erforderliche Installationsschritte und Tipps zur Problembehandlung finden im Artikel [Verschieben von Daten zwischen lokalen und Cloud Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway)Sie mit Abschnitt [Aspekte der Verwendung von Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Daten aus der lokalen SQL Server-Datenbank in Azure Computer eindringen


In dieser exemplarischen Vorgehensweise eine Management-Gateway in Azure Machine Learning-Arbeitsbereich einrichten, konfigurieren und Daten aus einer lokalen SQL Server-Datenbank lesen.

> [AZURE.TIP] Deaktivieren Sie zunächst Popupblocker des Browsers für `studio.azureml.net`. Wenn Sie Google Chrome-Browser verwenden, downloaden Sie und installieren Sie eine mehrere Plug-ins auf Google Chrome WebStore [Klicken einmal App-Erweiterung](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Schritt 1: Erstellen eines Gateways

Der erste Schritt ist zu erstellen und das Gateway Zugriff auf Ihre lokalen SQL-Datenbank einrichten.

1. Anmeldung bei [Azure Machine Learning Studio](https://studio.azureml.net/Home/) und wählen Sie den Arbeitsbereich, der Sie arbeiten möchten.

2. Klicken Sie auf Registerkarte **DATENGATEWAYS** oben auf Blatt **EINSTELLUNGEN** auf der linken Seite

3. Klicken Sie auf **Neue DATENGATEWAY** am unteren Bildschirmrand.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. Im Dialogfeld **neue Daten-Gateway** geben den **Gateway-Namen** und optional eine **Beschreibung**hinzufügen. Klicken Sie auf den Pfeil rechts unten fahren mit dem nächsten Schritt der Konfiguration.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Kopieren Sie im Download und Register Daten Gateway Dialogfeld GATEWAY-REGISTRIERUNGSSCHLÜSSEL in die Zwischenablage.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Wenn Sie noch nicht heruntergeladen und installiert Microsoft Data Management Gateway, dann klicken Sie auf **Download Data Management Gateway**. Dadurch gelangen Sie zum Microsoft Download Center, wo Sie die Gateway-Version müssen Sie herunterladen, die wählen und installieren können. Ausführliche Informationen zu Installation erforderliche Installationsschritte und Tipps zur Problembehandlung finden Sie in den Abschnitten Anfang [Verschieben von Daten zwischen lokalen und Cloud Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)Artikel.

7. Nach der Installation von Gateway Data Management Gateway-Konfigurations-Manager wird geöffnet und das Dialogfeld **Gateway registrieren** angezeigt. Fügen Sie den **Gateway-Registrierungsschlüssel** , den Sie in die Zwischenablage kopiert und auf **Registrieren**.

8. Wenn ein Gateway installiert noch führen Sie Data Management Gateway Configuration Manager aus auf **Key ändern**, fügen Sie den  **Gateway-Registrierungsschlüssel** , den Sie in die Zwischenablage kopiert, und klicken Sie auf **OK**.

9. Wenn die Installation abgeschlossen ist, wird das Dialogfeld **Gateway registrieren** für Microsoft Data Management Gateway Configuration Manager angezeigt. Fügen Sie der GATEWAY-REGISTRIERUNGSSCHLÜSSEL ein, die in der obigen Zwischenablage kopiert und auf **Registrieren**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Die Gateway-Konfiguration ist abgeschlossen, wenn auf der Registerkarte **Startseite** in Microsoft Data Management Gateway Configuration Manager die folgenden Werte festgelegt werden:

    - **Gateway-Namen** und den **Instanznamen** werden auf den Namen des Gateways festgelegt.

    - **Registrierte** **Registrierung** fest.

    - **Status** ist **Started**fest.

    - Die Statusleiste am unteren Rand zeigt **verbunden mit Data Management Gateway Cloud Service** zusammen mit einem grünen Häkchen.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure Machine Learning Studio wird auch aktualisiert, wenn die Registrierung erfolgreich ist.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. Klicken Sie im Dialogfeld **herunterladen und registrieren Daten-Gateway** auf das Häkchen, um die Installation abzuschließen. **Die Einstellungsseite** zeigt den Gateway-Status "Online". Im rechten Fensterbereich finden Sie Status und andere Informationen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. In der Microsoft Data Management Gateway Configuration Manager wechseln Sie zur Registerkarte **Zertifikat** . Auf dieser Registerkarte angegebene Zertifikat wird zum Verschlüsseln/Anmeldeinformationen für den lokalen Datenspeicher entschlüsseln, die im Portal angeben. Dies ist das Standardzertifikat generiert. Microsoft empfiehlt dies, Ihr eigenes Zertifikat in Ihrem Verwaltungssystem Zertifikat sichern. Klicken Sie auf **Ändern** , um Ihr eigenes Zertifikat verwenden.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (optional) Ggf. die ausführliche Protokollierung zur Behebung von Problemen mit dem Gateway in Microsoft Data Management Gateway Configuration Manager wechseln Sie zur Registerkarte **Diagnostics** und das Kontrollkästchen Sie **ausführliche Protokollierung für die Problembehandlung** . Die Protokollierungsinformationen finden Sie in der Windows-Ereignisanzeige **Applikationen und Dienstprotokolle**  - &gt; **Datenverwaltungsgateway** Knoten. Die Registerkarte **Diagnose** können zum Testen der Verbindung zu einer lokalen Datenquelle über das Gateway.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Einrichtung in Azure Machine Learning Gateway ist abgeschlossen.
Sie nun können Ihre lokalen Daten.

Sie können erstellen und mehrere Gateways in Studio für jeden Arbeitsbereich einrichten. Beispielsweise müssen Sie eine Verbindung mit Datenquellen Tests während der Entwicklung sollen Gateway und anderen Gateway für Produktionsdatenquellen. Azure Machine Learning Flexibilität mehrerer Gateways je nach Ihrem Firmennetzwerk einzurichten. Derzeit Gateway zwischen Arbeitsbereiche können nicht freigegeben werden, und nur ein Gateway kann auf einem einzelnen Computer installiert werden. Weitere Aspekte bei der Installation des Gateways finden Sie im Artikel [Verschieben von Daten zwischen lokalen und Cloud Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) [Aspekte der Verwendung von Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) .

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Schritt 2: Verwenden Sie das Gateway zum Lesen von Daten aus einer lokalen Datenquelle

Nachdem Sie das Gateway eingerichtet, können Sie ein Modul **Daten importieren** Versuch hinzufügen, die Daten aus der lokalen SQL Server-Datenbank eingibt.

1.  Machine Learning Studio **EXPERIMENTE** Registerkarte, klicken Sie auf **+ neu** in der unteren linken Ecke und **Leere Experiment** wählen (oder wählen Sie einige Beispiel-Experimente verfügbar).

2.  Und ziehen Sie das Modul **Import Data** auf die Leinwand Experiment.

3.  Klicken Sie unterhalb der Leinwand **Speichern** . Geben Sie "Azure Machine Learning lokalen SQL Server Tutorial" Experiment ein, wählen Sie den Arbeitsbereich und klicken Sie **OK** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  **Daten importieren** -Modul, um es auszuwählen klicken Sie im Bereich **Eigenschaften** rechts des Bereichs "Lokale SQL Database" in der Dropdownliste **Datenquelle** .

5.  Wählen Sie den **Daten-Gateway** installiert und registriert. Sie können ein anderes Gateway auswählen "(... Datengateway hinzufügen)" einrichten.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Geben Sie die SQL **Server-Datenbankname** und der **Datenbankname**und der SQL- **Abfrage** auszuführen.

7.  **Geben Sie Werte** unter **Benutzername und Kennwort** und geben Sie Ihre Anmeldeinformationen. Sie können integrierte Windows-Authentifizierung oder SQL Server-Authentifizierung, je nach Konfiguration der lokalen SQL Server.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Ein grünes Häkchen wird die Nachricht "erforderliche Werte" auf"Werte" ändern. Sie müssen nur einmal die Anmeldeinformationen eingegeben werden, wenn die Datenbank oder das Kennwort geändert wird. Azure Machine Learning verwendet das Zertifikat, das Sie bei der Installation von Gateway zum Verschlüsseln von Anmeldeinformationen in der Cloud bereitgestellt. Azure wird niemals für lokale Anmeldeinformationen unverschlüsselt gespeichert.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Klicken Sie zum Ausführen des Experiments **Ausführen** .

Nach Abschluss des Experiments visualisieren ausführen, müssen Sie die Daten, aus der Datenbank importiert werden, indem der Ausgangs-Port des Moduls **Importieren** und **visualisieren**.

Nachdem Sie abschließend das Experiment entwickeln Sie bereitstellen und Modell durchsetzen. Ausführung von Batch nutzen, werden Daten aus der lokalen SQL Server-Datenbank im Modul " **Daten importieren** " Lesen und zum bewerten. Reaktionszeit anfordern kann für lokale Daten bewerten verwenden empfiehlt Microsoft das [Excel-Add-in](machine-learning-excel-add-in-for-web-services.md) verwenden. Derzeit wird Schreiben in einer lokalen SQL Server-Datenbank durch **Daten exportieren** nicht in Ihre Experimente oder veröffentlichten Webdienste unterstützt.


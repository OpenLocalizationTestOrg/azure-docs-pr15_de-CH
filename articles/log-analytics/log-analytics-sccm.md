<properties
    pageTitle="Protokollanalyse Verbindung Configuration Manager | Microsoft Azure"
    description="Dieser Artikel beschreibt Schritte Konfigurationsmanager Protokollanalyse und analysiert Daten."
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
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Protokollanalyse Verbindung Configuration Manager

Sie können System Center Configuration Manager, Protokollanalyse OMS Sync Gerät Auflistung Daten verbinden. Dadurch Daten aus der Bereitstellung von Configuration Manager in OMS verfügbar.

Es gibt eine Anzahl Schritte anschließen Konfigurationsmanager OMS hier also ein kurzer Überblick über den gesamten Prozess:

1. Azure-Verwaltungsportal registrieren Sie Configuration Manager als Web-Anwendung oder Web API-Anwendung und sicherstellen Sie, dass Sie die Client-ID und den geheimen Schlüssel aus der Registrierung von Azure Active Directory Client. Finden Sie [mit Portal Active Directory Anwendung und Service principal, die auf Ressourcen zugreifen können](../resource-group-create-service-principal-portal.md) ausführliche Informationen dafür.
2. In der Azure-Verwaltungsportal, [Zugriffsberechtigung für OMS-Konfigurations-Manager (der registrierten Web app) bieten](#provide-configuration-manager-with-permissions-to-oms).
3. In Configuration Manager [eine Verbindung mit dem Verbindungsassistenten OMS hinzufügen](#add-an-oms-connection-to-configuration-manager).
4. In Configuration Manager können Sie [die Verbindungseigenschaften zu aktualisieren](#update-oms-connection-properties) Wenn Kennwort oder Client geheime Schlüssel nie abläuft oder verloren.
5. Informationen über den OMS-Portal [herunterladen und Installieren von Microsoft Monitoring Agent](#download-and-install-the-agent) auf dem Computer den Konfigurations-Manager-Verbindung Standortsystemrolle zeigen. Der Agent sendet Configuration Manager Daten in OMS.
6. In OMS Computergruppen [Sammlungen in Configuration Manager zu importieren](#import-collections) .
7. Zeigen Sie Daten von Configuration Manager in OMS als [Computergruppen an](log-analytics-computer-groups.md).

Erfahren Sie mehr über OMS [Sync](https://technet.microsoft.com/library/mt757374.aspx)Daten von Configuration Manager Microsoft Operations Management Suite mit Configuration Manager.



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>OMS-Konfigurations-Manager Berechtigungen ermöglichen

Das folgende Verfahren bietet Azure-Verwaltungsportal mit Zugriffsberechtigungen für OMS. Insbesondere müssen Sie Benutzern in der Ressourcengruppe der *Teilnehmerrolle* gewähren. Ermöglicht das Azure-Verwaltungsportal Verbindung Configuration Manager zu OMS.

>[AZURE.NOTE] Sie müssen Berechtigungen für OMS für Configuration Manager angeben. Andernfalls erhalten Sie eine Fehlermeldung bei Verwendung der Konfigurations-Assistent in Configuration Manager.


1. [Azure-Portal](https://portal.azure.com/) zu öffnen, und klicken Sie auf **Durchsuchen** > **Protokoll Analytics (OMS)** Blade Protokoll Analytics (OMS) geöffnet.  
2. Das Blade **Protokoll Analytics (OMS)** klicken Sie auf **Hinzufügen** , um das Blade **OMS-Arbeitsbereich** öffnen.  
  ![OMS-blade](./media/log-analytics-sccm/sccm-azure01.png)
3. Blade **OMS Arbeitsbereich** Angaben Sie folgende und klicken Sie dann auf **OK**.
  - **OMS-Arbeitsbereich**
  - **Abonnement**
  - **Ressourcengruppe**
  - **Speicherort**
  - **Preisstufe**  
    ![OMS-blade](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Im obigen Beispiel erstellt eine neue Ressourcengruppe. Die Ressourcengruppe dient nur Zugriffsrechte für den Arbeitsbereich OMS dabei Configuration Manager bereitstellen.

4. Klicken Sie auf **Durchsuchen** > **Ressourcengruppen** Blade **Ressourcengruppen** öffnen.
5. **Ressourcengruppen** Blatt klicken Sie auf die Ressourcengruppe, die Sie soeben erstellt haben, öffnen Sie die &lt;den Namen&gt; Blatt Einstellungen.  
  ![Gruppe-Einstellungen blade](./media/log-analytics-sccm/sccm-azure03.png)
6. In der &lt;Namen&gt; Blatt Einstellungen klicken Sie auf Access Control (IAM) öffnen den &lt;Namen&gt; Blatt Benutzer.  
  ![Gruppe-Benutzer blade](./media/log-analytics-sccm/sccm-azure04.png)  
7. In den &lt;Namen&gt; Blatt Benutzer klicken Sie auf **Hinzufügen** , um das Blade **Hinzufügen Zugriff** öffnen.
8. Blatt **Hinzufügen Zugriff** auf **Wählen Sie eine Rolle**, und wählen Sie die Rolle **eines Beitragenden** .  
  ![Wählen Sie eine Rolle](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klicken Sie auf **Hinzufügen**, wählen Sie Configuration Manager-Benutzer klicken Sie auf **auswählen**, und klicken Sie auf **OK**.  
  ![Hinzufügen von Benutzern](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Hinzufügen einer OMS-Verbindungs Configuration Manager

Hinzufügen eine OMS-Verbindung muss der Configuration Manager-Umgebung ein [Service-Verbindungspunkt](https://technet.microsoft.com/library/mt627781.aspx) für den Online-Modus konfiguriert.

1. Wählen Sie im Arbeitsbereich **Verwaltung** von Configuration Manager **OMS-Connector**. **OMS-Verbindungsassistenten hinzufügen**wird geöffnet. Wählen Sie **Weiter**.

2. Bildschirm " **Allgemein** " bestätigen Sie Folgendes getan haben und dass Sie für jedes Element Detailinformationen, und wählen Sie dann **Weiter**.
  1. In Azure-Verwaltungsportal angemeldet Sie Configuration Manager App Anwendung oder Web-API und die [Client-ID von der Registrierung](../active-directory/active-directory-integrating-applications.md)haben.
  2. In Azure-Verwaltungsportal haben einen app geheime Schlüssel für die registrierte Anwendung in Azure Active Directory erstellt.  
  3. In Azure-Verwaltungsportal haben die registrierten Web app mit Zugriffsberechtigung für OMS bereitgestellt werden.  
  ![Verbindung mit OMS Assistenten Allgemein](./media/log-analytics-sccm/sccm-console-general01.png)

3. Konfigurieren Sie auf dem Bildschirm **Azure Active Directory** Ihre Verbindung zu OMS Ihre **Mieter** , **Client-ID** , und **Client Geheimschlüssel** , und wählen Sie dann **Weiter**.  
  ![Verbindung zu OMS-Assistenten Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Die anderen Verfahren erfolgreich abgeschlossen wurde, wird die Informationen auf dem Bildschirm **OMS-Verbindungskonfiguration** automatisch auf dieser Seite angezeigt. Informationen für die Verbindungszeichenfolge sollte für **Azure-Abonnement** , **Azure-Ressourcengruppe** und **Operations Management Suite Arbeitsbereich**angezeigt werden.  
  ![Verbindung zu OMS-Assistenten OMS-Verbindung](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. Der Assistent wird mit OMS-Dienst mithilfe der Informationen, die Sie eingegeben haben. Wählen Sie die Geräte, die zu OMS synchronisieren und dann auf **Hinzufügen**.  
  ![Wählen](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Überprüfen Sie die Einstellungen im Fenster **Zusammenfassung** und anschließend **Weiter**. **Der Bildschirm** zeigt den Verbindungsstatus und sollte **abgeschlossen**.

>[AZURE.NOTE] Sie müssen auf der Website der obersten Ebene der Hierarchie OMS anschließen. Eigenständige Primärdatenbank OMS herstellen und Ihre Umgebung fügen Zentraladministrations-Website müssen Sie löschen und erneut in die neue Hierarchie OMS erstellt.

Nachdem Sie Configuration Manager OMS verknüpft haben, können Sie hinzufügen oder Entfernen von Sammlungen und Anzeigen der Eigenschaften eines OMS-Verbindung.

## <a name="update-oms-connection-properties"></a>OMS-Verbindungseigenschaften aktualisieren

Kennwort oder Client geheimer Schlüssel nie abläuft oder verloren, müssen Sie die Verbindungseigenschaften OMS manuell aktualisieren.

1. In Configuration Manager navigieren zu **Cloud-Dienste** und anschließend **OMS-Connector** auf der Seite **Eigenschaften von OMS** .
2. Klicken Sie auf dieser Seite auf die Registerkarte **Azure Active Directory** , um Ihre **Mieter** **Client-ID** **Client geheimen Schlüssel Ablaufdatum**anzeigen. **Überprüfen** der **geheime Schlüssel des Clients** abgelaufen ist.


## <a name="download-and-install-the-agent"></a>Downloaden Sie und installieren Sie den agent

1. Im OMS-Portal [herunterladen Agent-Setupdatei von OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Verwenden Sie eine der folgenden Methoden zum Installieren und konfigurieren Sie den Agent auf dem Computer Configuration Manager Service Connection Point Standortsystemrolle:
  - [Installieren Sie den Agent mithilfe von setup](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Installieren Sie den Agenten über die Befehlszeile](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Installieren Sie den Agenten mit DSC in Azure Automation](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Importinkasso

Nachdem eine OMS-Verbindung Configuration Manager hinzugefügt und installiert den Agenten auf dem Computer den Konfigurations-Manager-Verbindung zeigen Standortsystemrolle, Nächstes Sammlungen in Configuration Manager in OMS als Computergruppen importieren.

Nach dem Import aktiviert ist, wird die Informationen zur Mitgliedschaft alle 3 Stunden abgerufen, um Mitgliedschaften Auflistung auf dem neuesten Stand zu halten. Sie können Import jederzeit deaktivieren.

1. **Klicken Sie auf im OMS-Portal.**
2. Klicken Sie auf der Registerkarte **Computergruppen** , und klicken Sie dann auf die Registerkarte **SCCM** .
3. Wählen Sie **Import Configuration Manager Auflistung Mitgliedschaften** und klicken Sie dann auf **Speichern**.  
  ![Computergruppen – SCCM-Registerkarte](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Daten von Configuration Manager

Nachdem eine OMS-Verbindung Configuration Manager hinzugefügt und installiert den Agenten auf dem Computer Configuration Manager Service Connection Point Standortsystemrolle, werden Daten vom Agent OMS gesendet. Die Configuration Manager-Sammlungen in OMS als [Computergruppen](log-analytics-computer-groups.md)angezeigt. Sie können Gruppen auf der **Configuration Manager** unter **Computergruppen** **Einstellungen**anzeigen.

Nach dem Import der Sammlungen können Sie sehen, wie viele Computer mit Mitgliedschaften Auflistung gefunden wurden. Sie sehen auch die Anzahl der Sammlungen, die importiert wurden.

![Computergruppen – SCCM-Registerkarte](./media/log-analytics-sccm/sccm-computer-groups02.png)

Wenn eine klicken wird geöffnet Suche alle importierten Gruppen oder alle Computer, die den einzelnen Gruppen gehören. [Protokoll-Suche](log-analytics-log-searches.md)können Sie detaillierte Analysen für Configuration Manager Daten starten.

## <a name="next-steps"></a>Nächste Schritte

- Suchfunktion [Protokoll](log-analytics-log-searches.md) detaillierte Informationen über die Configuration Manager-Daten anzeigen.

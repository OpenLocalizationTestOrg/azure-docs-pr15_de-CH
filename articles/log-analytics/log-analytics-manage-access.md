<properties
    pageTitle="Verwalten des Zugriffs auf Protokollanalyse | Microsoft Azure"
    description="Verwalten des Zugriffs auf Protokollanalyse mithilfe verschiedener Verwaltungsaufgaben auf Benutzer, Firmen, OMS Arbeitsbereiche und Azure-Konten."
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
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Verwalten des Zugriffs auf Protokollanalyse

Zum Verwalten des Zugriffs auf Protokollanalyse verwenden Sie eine Vielzahl von administrativen Aufgaben auf Benutzer, Firmen, OMS Arbeitsbereiche und Azure-Konten. Um einen neuen Arbeitsbereich in Operations Management Suite (OMS) erstellen, wählen Sie einen Arbeitsbereichsnamen Ihr Konto zuordnen und Sie einen Standort auswählen. Ein Arbeitsbereich ist im Wesentlichen ein Container, der Kontoinformationen und einfache Konfigurationsinformationen für das Konto enthält. Sie oder andere Mitglieder Ihrer Organisation können mehrere OMS Arbeitsbereiche verwalten unterschiedliche Daten alle oder Teile der IT-Infrastruktur.

[Beginnen mit der Protokollanalyse](log-analytics-get-started.md) Artikel veranschaulicht das schnell und ausführen und den Rest dieses Artikels beschreibt ausführlich einige Aktionen auf OMS verwalten müssen.

Sie müssen möglicherweise alle zuerst ausführen, werden häufig verwendeten Aufgaben besprochen, die Sie in den folgenden Abschnitten verwenden können:

- Die Anzahl der benötigten Arbeitsbereiche
- Verwalten von Konten und Benutzer
- Hinzufügen einer Gruppe zu einem vorhandenen Arbeitsbereich
- Verknüpfen Sie einen vorhandenen Arbeitsbereich mit Azure-Abonnement
- Aktualisieren eines Arbeitsbereichs bezahlte Daten planen
- Ein Plan Datentyp
- Hinzufügen einer Organisation Azure Active Directory auf einem vorhandenen Arbeitsbereich
- Schließen Sie den OMS-Arbeitsbereich

## <a name="determine-the-number-of-workspaces-you-need"></a>Die Anzahl der benötigten Arbeitsbereiche

Ein Arbeitsbereich ist eine Azure-Ressource und Container Daten gesammelt, aggregiert, analysiert, und der OMS-Portal angezeigt.

Es ist möglich, mehrere OMS-Protokollanalyse Arbeitsbereiche erstellen und Benutzer haben Zugriff auf einen oder mehrere Workspaces. Im Allgemeinen möchten Sie die Anzahl von Arbeitsbereichen minimieren diese können Sie Abfragen und die meisten Daten zu korrelieren. Dieser Abschnitt beschreibt, wenn sie mehrere Arbeitsbereiche erstellen hilfreich.

Heute bietet einen Arbeitsbereich für Protokollanalyse:

- Ein Standort für die Speicherung von Daten
- Granularität für die Abrechnung
- Datenisolation

Anhand der oben aufgeführten Merkmale sollten Sie mehrere Arbeitsbereiche erstellen, wenn:

- Sie sind ein globales Unternehmen und benötigen Daten in bestimmten Regionen aus Hoheit oder Compliance.
- Azure verwenden, und Sie möchten ausgehende Übertragung Gebühren mit einer Protokollanalyse Arbeitsbereich im Bereich Azure Ressourcen verwaltet.
- Zuschläge auf verschiedene Kostenstellen und/oder Unternehmensgruppen basierend auf der Verwendung zuweisen möchten. Beim Erstellen eines Arbeitsbereichs für jede Abteilung oder Unternehmensgruppe zeigt Ihre Azure Rechnung und Verwendung Anweisung Gebühren für jeden Arbeitsbereich getrennt.
- Sind Sie ein Dienstleister und müssen zu Protokoll Analysedaten für jeden Kunden Verwalten von anderen Kundendaten.
- Verwalten mehrerer Kunden und soll jeder Debitor oder Abteilung oder Unternehmen an ihre Daten jedoch nicht die Daten für andere Kunden oder Abteilungen oder Unternehmensgruppen.

Wenn Agenten Daten erfassen, können Sie jeden Agent an der erforderliche Arbeitsbereich konfigurieren.

Bei Verwendung von System Center Operations Manager kann jede Verwaltungsgruppe Operations Manager nur ein Arbeitsbereich zugeordnet. Sie installieren Microsoft Monitoring Agent auf Computern mit Operations Manager verwaltet und Agent Bericht Operations Manager und anderen Protokollanalyse-Arbeitsbereich.

### <a name="workspace-information"></a>Informationen zum Arbeitsbereich

Im Portal OMS können Ihr Arbeitsbereich anzeigen und auswählen, ob Sie Informationen von Microsoft.

#### <a name="view-workspace-information"></a>Informationen zum Arbeitsbereich

1. OMS klicken Sie auf die Kachel **Settings** .
2. Klicken Sie auf die Registerkarte **Konten** .
3. Klicken Sie auf der Registerkarte **Arbeitsbereich** .  
  ![Informationen zum Arbeitsbereich](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Verwalten von Konten und Benutzer

Jeder Arbeitsbereich kann mehrere Benutzerkonten zugeordnet und jedes Benutzerkonto (Microsoft organisatorische Konto oder) haben Zugriff auf mehrere OMS-Arbeitsbereiche.

Standardmäßig wird der Microsoft-Konto oder Organisationseinheiten zum Erstellen des Arbeitsbereichs Konto Administrator des Arbeitsbereichs. Der Administrator kann dann laden zusätzliche Microsoft-Konten oder wählen Benutzer aus Active Directory Azure.

Zugang Personen OMS-Arbeitsbereich wird in 2 an:

- Rollenbasierte Zugriffskontrolle können Sie in Azure Azure-Abonnement und die zugehörigen Azure Ressourcen zugreifen. Dies ist auch für PowerShell und REST-API.
- Zugreifen Sie in OMS-Portal, nur das Portal OMS - nicht zugeordneten Azure-Abonnement.

Wenn Sie Benutzern Zugriff gewähren OMS-Portal, nicht jedoch das Azure Abonnement verknüpft ist, dann anzeigen Kacheln Lösung Automatisierung, Backup und Site Recovery nicht Daten Benutzern Wenn sie OMS-Portal anmelden.

Alle Benutzer, die Daten dazu sicherzustellen, dass sie mindestens **Reader** Zugriff für OMS-Arbeitsbereich mit Automatisierungskonto Depot Backup und Site Recovery-Tresor.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Verwalten des Zugriffs auf Protokollanalyse mit Azure-portal

Wenn Sie Benutzern Zugriff auf Protokollanalyse Arbeitsbereich Azure Berechtigungen gewähren, können in Azure-Portal beispielsweise dann dieselben Benutzer Protokollanalyse-Portal zugreifen. Wenn Benutzer in Azure-Portal, können sie durch Klicken auf die **OMS-Portal** Aufgabe Protokollanalyse Arbeitsbereich Ressource anzeigen OMS-Portal.

Einige Punkte zu bedenken Azure-Portal:

- Dies ist keine *Rollenbasierte Zugriffskontrolle*. Haben Sie *Leser* -Zugriffsberechtigungen im Azure-Portal für den Protokollanalyse-Arbeitsbereich können Sie über das Portal OMS Änderungen vornehmen. OMS-Portal ist ein Konzept von Administrator, Teilnehmer und ReadOnly-Benutzer. In Azure Active Directory mit dem Arbeitsbereich verknüpft ist das Konto mit eingeloggt sind Administrator OMS-Portal werden, ansonsten werden Sie einen Beitrag.

- Wenn Sie http://mms.microsoft.com, dann standardmäßig mit OMS-Portal anmelden Siehe Liste **Arbeitsbereich auswählen** . Es enthält nur Arbeitsbereiche mit OMS-Portal hinzugefügt wurden. Darauf Arbeitsbereiche mit Azure-Abonnements verfügen, müssen Sie einen Mieter als Teil der URL angeben. Zum Beispiel:

  `mms.microsoft.com/?tenant=contoso.com`Der Mieter Bezeichner ist häufig das letzte Teil der e-Mail-Adresse, die Sie mit anmelden.

- Ist das Konto mit Anmelden ein Konto in der Mieter Azure Active Directory üblich, außer Sie sind als einen Kryptografiedienstanbieter anmelden, werden Sie *Administrator* des Portals OMS sein. Wenn Ihr Konto nicht Mieter Azure Active Directory ist, werden Sie einem *Benutzer* im Portal OMS sein.

- Möchten Sie navigieren direkt zu einem Portal haben Sie Zugriff auf Azure Berechtigungen, müssen Sie die Ressource als Teil der URL angeben. Es ist möglich, diese URL mithilfe von PowerShell.

  Z. B. `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Die URL wird folgende aussehen:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Verwalten von Benutzern in OMS-portal

Verwalten von Benutzern und Gruppen auf der Registerkarte **Benutzer verwalten** auf der Registerkarte **Konten** auf der Einstellungsseite. Führen Sie die Aufgaben in den folgenden Abschnitten.  

![Verwalten von Benutzern](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich

Gehen Sie einen Benutzer oder eine Gruppe mit OMS Arbeitsbereich hinzufügen. Der Benutzer oder die Gruppe werden angezeigt und alle Alarme, die diesem Arbeitsbereich zugeordnet sind.

>[AZURE.NOTE] Wenn Sie einen Benutzer oder eine Gruppe aus der Organisationseinheit Azure Active Directory-Konto hinzufügen möchten, müssen Sie zunächst sicherstellen, dass Sie Active Directory-Domäne der OMS-Konto zugeordnet. Siehe [Hinzufügen einer Organisation Azure Active Directory auf einem vorhandenen Arbeitsbereich](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. OMS klicken Sie auf die Kachel **Settings** .
2. Klicken Sie auf die Registerkarte **Konten** , und klicken Sie dann auf die Registerkarte **Benutzer verwalten** .
3. Wählen Sie im Abschnitt **Benutzer verwalten** den Kontotyp hinzufügen: **Konto Organisation** **Microsoft-Konto** **Microsoft Support**.
    - Geben Sie die e-Mail-Adresse des Benutzers Microsoft Account zugeordnet, wählt Microsoft Account.
    - Wählt Organisationskonto Teil des Benutzers oder der Gruppe Namen oder e-Mail-Alias eingeben und von Benutzern und Gruppen aufgeführt. Benutzer oder Gruppe auswählen.
    - Verwenden Sie Microsoft Support zu Microsoft Support Ingenieur temporären Zugriff auf den Arbeitsbereich zu Problembehandlung.

    >[AZURE.NOTE] Für die besten Leistungsergebnisse, die Anzahl der Active Directory-Gruppen ein einzelnes OMS Benutzerkonto drei zugeordnet – Administratoren, Mitarbeiter und Gäste. Mehrere Gruppen kann die Leistung der Protokollanalyse auswirken.

5. Wählen Sie den Benutzer oder Gruppe hinzufügen: **Administrator**, **Teilnehmer**oder **Benutzer schreibgeschützt** .  
6. Klicken Sie auf **Hinzufügen**.

  Wenn Sie ein Microsoft-Konto hinzufügen, erhält eine Einladung für den Arbeitsbereich e-Mail angegebene. Nachdem der Benutzer die Informationen in der Einladung für OMS folgt, der Benutzer kann die Hinweise und Informationen für dieses Konto OMS anzeigen und werden die Informationen auf der Registerkarte **Konten** **Einstellungen** anzeigen.
  Hinzufügen einer Organisationseinheit Kontos werden der Protokollanalyse sofort zugreifen.  
  ![Einladung](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Einen vorhandenen Benutzer bearbeiten

Firmenrolle für Ihr Konto OMS Benutzer können geändert werden. Sie haben die folgenden Rollenoptionen:

 - *Administrator*: können Benutzer verwalten, anzeigen wirken sich auf alle Alerts hinzufügen und Entfernen von Servern

 - *Teilnehmer*: können anzeigen wirken sich auf alle Alerts hinzufügen und Entfernen von Servern

 - *ReadOnly-Benutzer*: Benutzer als schreibgeschützt gekennzeichnet, nicht möglich:
   1. Software Solutions. Der Lösungskatalog ist ausgeblendet.
   2. Kacheln auf **Mein Dashboard**hinzufügen/ändern/entfernen.
   3. **Settings** -Seiten anzeigen. Die Seiten werden ausgeblendet.
   4. Aufgaben werden in der Suche anzeigen PowerBI Konfiguration, gespeicherte Suchen und Alarme ausgeblendet.


#### <a name="to-edit-an-account"></a>So bearbeiten Sie ein Konto

1. OMS klicken Sie auf die Kachel **Settings** .
2. Klicken Sie auf die Registerkarte **Konten** , und klicken Sie dann auf die Registerkarte **Benutzer verwalten** .
3. Wählen Sie die Rolle für den Benutzer, den Sie ändern möchten.
2. Klicken Sie im Dialogfeld zur Bestätigung auf **Ja**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Entfernen eines Benutzers aus einer OMS-Arbeitsbereich

Gehen Sie folgendermaßen vor, um einen Benutzer aus einem OMS-Arbeitsbereich entfernen. Beachten Sie, dass dies nicht Arbeitsbereich des Benutzers. Stattdessen entfernt die Zuordnung zwischen Benutzer und dem Arbeitsbereich. Wenn ein Benutzer mehreren Arbeitsbereichen zugeordnet ist werden, noch OMS anmelden und sehen die Arbeitsbereiche.

1. OMS klicken Sie auf die Kachel **Settings** .
2. Klicken Sie auf die Registerkarte **Konten** , und klicken Sie dann auf die Registerkarte **Benutzer verwalten** .
3. **Klicken Sie auf neben den Benutzernamen, den Sie entfernen möchten.**
4. Klicken Sie im Dialogfeld zur Bestätigung auf **Ja**.


### <a name="add-a-group-to-an-existing-workspace"></a>Hinzufügen einer Gruppe zu einem vorhandenen Arbeitsbereich

1.  Führen Sie die Schritte 1 -4 in "Zum Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich verwenden" oben.
2.  Wählen Sie unter **Benutzergruppe auswählen** **aus**.
    ![Hinzufügen einer Gruppe zu einem vorhandenen Arbeitsbereich](./media/log-analytics-manage-access/add-group.png)
3.  Geben Sie den Anzeigenamen oder die e-Mail-Adresse für die Gruppe hinzufügen möchten.
4.  Wählen Sie die Gruppe in der Liste führt, und klicken Sie dann auf **Hinzufügen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Verknüpfen Sie einen vorhandenen Arbeitsbereich mit Azure-Abonnement

Es ist möglich, einen Arbeitsbereich aus der [microsoft.com/oms](https://microsoft.com/oms) -Website erstellen.  Es gibt jedoch bestimmte Grenzwerte für diese Arbeitsbereiche die wichtigsten maximal 500MB pro Tag Datenuploads zu, wenn Sie ein kostenloses Konto verwenden. Um diesem Arbeitsbereich ändern müssen Sie *vorhandenen Arbeitsbereich mit Azure-Abonnement*verknüpft.

>[AZURE.IMPORTANT] Um einen Arbeitsbereich verknüpfen müssen Ihre Azure-Konto Zugriff auf den Arbeitsbereich verknüpfen möchten.  **Also muss das Konto, mit dem Sie Azure-Portal zugreifen als Konto übereinstimmen auf den OMS-Arbeitsbereich verwenden.** Wenn dies nicht der Fall ist, finden Sie unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Einen Arbeitsbereich mit Azure-Abonnement in OMS-Portal verknüpfen

Um einen Arbeitsbereich mit Azure-Abonnement in OMS-Portal verknüpfen müssen angemeldeten Benutzers bezahltes Azure-Konto. Arbeitsbereich aktiv verwenden wird der Azure-Konto verknüpft.

1. OMS klicken Sie auf die Kachel **Settings** .
2. Klicken Sie auf die Registerkarte **Konten** , und klicken Sie dann auf die Registerkarte **Azure-Abonnement und Daten** .
3. Klicken Sie Daten, die Sie verwenden möchten.
4. Klicken Sie auf **Speichern**.  
  ![Abonnement und Daten Pläne](./media/log-analytics-manage-access/subscription-tab.png)

Die neuen Datenplan wird im Portal OMS-Menüband am oberen Rand der Webseite angezeigt.

![OMS-Multifunktionsleiste](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Einen Arbeitsbereich mit Azure-Abonnement im Azure-Portal verknüpfen

1.  [Azure-Portal](http://portal.azure.com)anmelden.
2.  Suchen Sie **Protokoll-Analyse (OMS)** und markieren Sie ihn.
3.  Sie sehen die Liste der vorhandenen Arbeitsbereiche. Klicken Sie auf **Hinzufügen**.  
    ![Liste der Arbeitsbereiche](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  **OMS-Arbeitsbereich**klicken Sie **oder vorhandene**.  
    ![Verknüpfen vorhandener](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klicken Sie auf **Configure Settings erforderlich**.  
    ![erforderliche konfigurieren](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Sie sehen die Liste der Arbeitsbereiche, die noch nicht mit der Azure-Konto verknüpft sind. Arbeitsbereich auswählen.  
    ![Wählen Sie Arbeitsbereiche](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Bei Bedarf können Sie Werte für die folgenden Elemente ändern:
    - Abonnement
    - Ressourcengruppe
    - Speicherort
    - Preisstufe  
        ![Werte ändern](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klicken Sie auf **Erstellen**. Der Arbeitsbereich ist jetzt ein Azure-Konto verknüpft.

>[AZURE.NOTE] Wenn verknüpfen möchte den Arbeitsbereich nicht angezeigt wird, kann dann Azure-Abonnement nicht OMS-Arbeitsbereich zugreifen mit OMS-Website.  Sie benötigen Zugriff auf dieses Konto im Arbeitsbereich OMS OMS-Website verwenden. Hierzu finden Sie unter [Hinzufügen eines Benutzers zu einem vorhandenen Arbeitsbereich](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Aktualisieren eines Arbeitsbereichs bezahlte Daten planen

Es gibt drei Arbeitsbereichsdaten Planen für OMS: ** **frei**, und **Premium****.  *Frei* -Plan finden Sie Ihre Daten maximal 500 MB getroffen haben.  Sie müssen Arbeitsbereich auf ***Bedarfsbasis Plan*** aktualisieren, um Daten darüber hinaus. Jederzeit können Sie Ihr Typ konvertieren.  Weitere Informationen zu Preisen OMS Einzelheiten Sie [Preis](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Arbeitsbereich Pläne können nur geändert werden, werden *verknüpfte* Azure-Abonnement.  Wenn Sie Ihren Arbeitsbereich in Azure erstellt oder bereits Arbeitsbereich *bereits* verknüpft, können Sie diese Meldung ignorieren.  Wenn Sie den Arbeitsbereich [OMS-Website](http://www.microsoft.com/oms)erstellt, müssen Sie Schritte Link [einen vorhandenen Arbeitsbereich an Azure-Abonnement](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Mithilfe von Berechtigungen OMS Add-On für System Center

OMS-Erweiterung für System Center bietet Anspruch für den Zusatzplan OMS Protokollanalyse zu [OMS](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx)beschrieben.

Beim Kauf der OMS-Erweiterung für System Center wird das Add-on OMS als Anspruch auf Ihr System Center-Vertrag hinzugefügt. Jede Azure-Abonnement, die unter dieser Vereinbarung erstellt können mithilfe des Anspruchs. Dadurch können Sie z. B. mehrere OMS Arbeitsbereiche haben, die die Ansprüche von OMS-Add-On verwenden.

Um sicherzustellen, dass die Verwendung eines OMS Workspace auf Ihre Ansprüche von OMS Add-on angewendet wird, müssen Sie:

1. Verknüpfen Sie Arbeitsbereich OMS mit Azure-Abonnement, die Teil der Konzernvertrag, die OMS Add-on Erwerb und Nutzung der Azure-Abonnement umfasst
2. Wählen Sie den Premium-Plan für den Arbeitsbereich

Beim Überprüfen der Verbrauch in Azure oder OMS-Portal wird OMS Add-on Ansprüche nicht angezeigt werden. Allerdings können Berechtigungen in Enterprise Portal angezeigt.  

Benötigen Sie Azure-Abonnement ändern, dem OMS Arbeitsbereich verknüpft ist, können Sie das Cmdlet Azure PowerShell [AzureRmResource verschieben](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Mithilfe von Azure Engagement eine Konzernvertrag

Möchten Sie eigenständige Preisgestaltung für OMS-Komponenten verwenden, Sie Zahlen für jede Komponente des OMS getrennt und die Verwendung Ihrer Azure Rechnung erscheint.

Haben Sie einen Azure monetären Commit auf Enterprise Agreement mit für Ihre Azure-Abonnements belastet beliebige Verwendung der Protokollanalyse automatisch für alle verbleibenden monetären Commit.

Ggf. ändern können Azure-Abonnement, OMS-Arbeitsbereich verknüpft ist Azure PowerShell [Verschieben-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) -Cmdlet.  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Einen Arbeitsbereich bezahlte Datentarif ändern

1.  [Azure-Portal](http://portal.azure.com)anmelden.
2.  Suchen Sie **Protokoll-Analyse (OMS)** und markieren Sie ihn.
3.  Sie sehen die Liste der vorhandenen Arbeitsbereiche. Arbeitsbereich auswählen.  
    ![Liste der Arbeitsbereiche](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Klicken Sie unter **Einstellung**auf **Preisstufe**.  
    ![Preisstufe](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  Wählen Sie unter **Preisstufe**einen aus und dann auf **auswählen**.  
    ![Plan auswählen](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Beim Aktualisieren der Ansicht im Azure-Portal sehen Sie **Preisstufe** für den ausgewählten Plan aktualisiert.  
    ![Update Preisstufe](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Nun können Sie Daten über Daten "frei" Cap sammeln.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Hinzufügen einer Organisation Azure Active Directory auf einem vorhandenen Arbeitsbereich

Sie können Log Analytics (OMS) Arbeitsbereich Azure Active Directory-Domäne zuordnen. Dadurch können Sie Benutzer aus Active Directory direkt Arbeitsbereich OMS hinzufügen ohne separates Microsoft-Konto.

Den Arbeitsbereich von Azure-Portal erstellen oder verknüpfen Arbeitsbereich mit Azure-Abonnement wird Azure Active Directory als Organisations-Konto verknüpft.

Beim Erstellen des Arbeitsbereichs von OMS-Portal werden Sie aufgefordert, eine Azure und organisatorische Konto verknüpfen.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Einen vorhandenen Arbeitsbereich Azure Active Directory Organisation hinzufügen

1. Auf der Seite Einstellungen OMS auf **Konten** , und klicken Sie auf der Registerkarte **Arbeitsbereich** .  
2. Informationen über Organisationseinheiten Konten, und klicken Sie dann auf **Organisation hinzufügen**.  
    ![Organisation hinzufügen](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Geben Sie die Identität für den Administrator von Azure Active Directory-Domäne. Danach sehen Sie eine Bestätigung, dass der Arbeitsbereich Azure Active Directory-Domäne verknüpft ist.
    ![Verknüpfte Arbeitsbereich Bestätigung](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Sobald Ihr Konto eine Organisationseinheit Konto verknüpft ist, kann nicht Verknüpfung entfernt oder geändert werden.

## <a name="close-your-oms-workspace"></a>Schließen Sie den OMS-Arbeitsbereich

Beim Schließen eines OMS-Arbeitsbereichs Arbeitsbereich alle Daten entfällt vom OMS-Dienst binnen 30 Tagen nach Schließen des Arbeitsbereichs.

Wenn Sie Administrator sind und mehrere den Arbeitsbereich zugeordnet Benutzer ist die Zuordnung zwischen Benutzern und dem Arbeitsbereich unterbrochen. Wenn die Benutzer anderen Arbeitsbereichen zugeordnet sind, können sie weiterhin diese Arbeitsbereiche OMS mit. Jedoch nicht anderen Arbeitsbereichen zugeordnet sind müssen sie zum Erstellen eines neuen Arbeitsbereichs Verwendung von OMS.

### <a name="to-close-an-oms-workspace"></a>Zu einem OMS-Arbeitsbereich

1. OMS klicken Sie auf die Kachel **Settings** .
2. Klicken Sie auf die Registerkarte **Konten** , und klicken Sie dann auf der Registerkarte **Arbeitsbereich** .
3. Klicken Sie auf **Schließen Arbeitsbereich**.
4. Wählen Sie eines der Arbeitsbereich schließen oder geben Sie einen anderen Grund in das Textfeld ein.
5. Klicken Sie auf **Schließen Arbeitsbereich**.

## <a name="next-steps"></a>Nächste Schritte

- [Verbinden von Windows-Computern Protokollanalyse](log-analytics-windows-agents.md) Agents und Daten anzeigen
- [Protokollanalyse hinzufügen Lösungen Lösungskatalog](log-analytics-add-solutions.md) Funktionen und Daten.
- [Configure Proxy- und Firewall Settings in Protokollanalyse](log-analytics-proxy-firewall.md) verwendet Ihre Organisation einen Proxyserver oder eine Firewall, dass Agenten mit der Protokollanalyse Dienst kommunizieren können.

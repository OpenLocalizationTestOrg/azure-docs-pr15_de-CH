<properties
    pageTitle="Migrieren von mobilen Diensten eine App Service App"
    description="Erfahren Sie, wie einfach die Anwendung Mobile Dienste einer App Service Mobile Anwendung migrieren"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Migrieren der vorhandenen Azure Mobile Service in Azure App Service

[Allgemeine Verfügbarkeit von Azure App Service]können Azure Mobile Services-Sites einfach direkt zu allen Features von Azure App Service migriert werden.  Dieses Dokument erläutert, wie Sie beim Migrieren Ihrer Website von Azure Mobile Services zu Azure App Service erwartet.

## <a name="what-does-migration-do"></a>Was tut die Migration Ihrer Site

Migration von Azure Mobile Service verwandelt eine [Azure App Service] -app ohne Code Mobile Service.  Die Benachrichtigungshubs, SQL-Verbindung, Authentifizierung, geplanter Aufträge und Domänennamen bleiben unverändert.  Mobile Clients mithilfe von Azure Mobile Service weiterhin normal ausgeführt.  Migration den Dienst gestartet, nachdem in Azure App Service übertragen wird.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Warum sollten Sie Ihre Website migrieren.

Microsoft empfiehlt, dass Sie Ihre Azure Mobile Service zu den Features von Azure App Service, einschließlich migrieren:

  *  Neue Host-Features, einschließlich [Webaufträge] und [benutzerdefinierten Domänennamen].
  *  Verbindung mit [VNet] neben [Hybrid-Verbindungen]mit lokalen Ressourcen.
  *  Überwachung und Problembehandlung mit neuen Relikt oder [Anwendung Einblicke].
  *  Integrierte DevOps-Tools, Rollback, einschließlich [stagingslots]und in Produktion testen.
  *  [Automatische Skalierung], Lastenausgleich und [Performance-Überwachung].

Weitere Informationen zu den Vorteilen von Azure App Service finden Sie auf [Mobile und App Service] .

## <a name="before-you-begin"></a>Bevor Sie beginnen

Vor jeder wichtige Arbeit auf Ihrer Website sollten Sie Skripts [Mobile Service sichern] und SQL-Datenbank.

## <a name="migrating-site"></a>Migrieren der Websites

Der Migrationsprozess migriert alle Websites in einem einzigen Azure-Bereich.

Ihre Website migrieren:

  1.  Das [klassische Azure-Portal]anmelden.
  2.  Wählen Sie einen Dienst Mobile in der Region, die Sie migrieren möchten.
  3.  Klicken Sie auf **Migrieren zu App Service** .

    ![Die Schaltfläche migrieren][0]

  4.  Migrieren in die App Service Dialogfeld lesen.
  5.  Geben Sie den Namen des Mobile Service in das Feld.  Beispielsweise ist der Domänenname contoso.azure mobile.net, geben Sie _Contoso_ in das Feld ein.
  6.  Klicken Sie auf Schaltfläche mit dem Häkchen.

Überwachen Sie den Status der Migration in die Aktivitäts-Anzeige. Ihre Website wird als *Migration* in Azure-Verwaltungsportal aufgeführt.

  ![Aktivitätsmonitor Migration][1]

Jeder Migration dauert von 3 auf 15 Minuten pro Mobilfunkanbieter migrierten.  Ihre Website bleibt während der Migration.
Ihre Website wird am Ende des Migrationsprozesses neu gestartet.  Die Website steht während des Neustarts, die ein paar Sekunden dauern.

## <a name="finalizing-migration"></a>Abschließen der Migrations

Ihre Site von einem mobilen Client nach Abschluss der Migration testen möchten.  Sicherstellen Sie, dass alle gemeinsamen Clientaktionen ohne mobilen Client ausführen können.  

### <a name="update-app-service-tier"></a>Wählen Sie einen entsprechenden Tarif App-Dienst

Sie haben mehr Flexibilität bei der Preisgestaltung nach dem Migrieren zu Azure App Service.

  1.  Auf der [Azure-Portal]anmelden.
  2.  **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3.  Einstellungen-Blades wird standardmäßig geöffnet.
  4.  Klicken Sie im Menü auf **App Service-Plan** .
  5.  Klicken Sie auf nebeneinander **-Tier-Preis** .
  6.  Klicken Sie auf die Kachel der Anforderungen **Wählen**.  Sie müssen auf **Ansicht aller** verfügbaren Preise leisten.

Als Ausgangspunkt empfehlen wir die folgenden Stufen:

| Mobile Preise Dienstebene | App Preise Dienstebene |
| :-------------------------- | :----------------------- |
| Frei                        | F1 kostenlose                  |
| Grundlegende                       | B1 Basic                 |
| Standard                    | S1 Standard              |

Gibt es beträchtliche Flexibilität in der richtigen Tarif für die Anwendung.  Weitere [App Servicepreise] Einzelheiten auf die Preise der neuen App Service.

> [AZURE.TIP]App Service Standard Tier enthält Zugriff auf viele Features, die Sie verwenden möchten, einschließlich [stagingslots], automatische Backups und automatische Skalierung.  Es sind neuen Funktionen testen!

### <a name="review-migration-scheduler-jobs"></a>Überprüfen Sie die migrierten Steuerprogrammaufträge

Scheduler-Jobs werden erst 30 Minuten nach der Migration nicht angezeigt.  Geplante Aufträge weiterhin im Hintergrund ausgeführt.
Geplanter Aufträge anzeigen, nachdem sie wieder angezeigt werden:

  1.  Auf der [Azure-Portal]anmelden.
  2.  Wählen Sie **Durchsuchen >**, geben Sie im Feld _Filter_ **Zeitplan** und **Planer**wählen.

Es gibt eine begrenzte Anzahl von freien Scheduler Aufträge verfügbar nach der Migration.  Überprüfen Sie die Auslastung und [Azure Planer plant].

### <a name="configure-cors"></a>Konfigurieren Sie CORS, falls erforderlich

Cross-Ursprung Ressourcenfreigabe ist ein Verfahren für eine Website auf eine Web-API in einer anderen Domäne zugreifen können.  Bei Verwendung von Azure Mobile Dienste mit der entsprechenden Website müssen Sie CORS als Teil der Migration zu konfigurieren.  Wenn Sie Azure Mobile Services ausschließlich von mobilen Geräten aus zugreifen, muss CORS nicht nur in seltenen Fällen konfiguriert werden.

Migrierten CORS Einstellungen stehen als **MS_CrossDomainWhitelist** App festlegen.  So migrieren Sie Ihre Website an App Service CORS

  1.  Auf der [Azure-Portal]anmelden.
  2.  **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3.  Einstellungen-Blades wird standardmäßig geöffnet.
  4.  Klicken Sie im Menü API auf **CORS** .
  5.  Geben Sie alle zulässigen Ursprünge im bereitgestellt, danach jeweils die EINGABETASTE drücken.
  6.  Sobald Ihre Herkunft zulässig korrekt ist, klicken Sie speichern.

> [AZURE.TIP]Einer der Vorteile der Verwendung von Azure App Service ist Ihre Website und Mobilfunkanbieter auf demselben Standort ausgeführt werden.  Weitere Informationen finden Sie in den Abschnitt " [Nächste Schritte](#next-steps) " fort.

### <a name="download-publish-profile"></a>Ein neues Veröffentlichungsprofil herunterladen

Präsentationsprofil Ihrer Website wird beim Migrieren zu Azure App Service geändert.  Soll Ihre Website innerhalb von Visual Studio veröffentlichen, benötigen Sie ein neues Profil veröffentlichen.  So downloaden Sie das neue Profil veröffentlichen

  1.  Auf der [Azure-Portal]anmelden.
  2.  **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3.  Klicken Sie auf **Get Profil veröffentlichen**.

PublishSettings-Datei wird auf Ihren Computer heruntergeladen.  _Sitename_wird normalerweise aufgerufen. PublishSettings.  Importieren Sie veröffentlichungseinstellungen in Ihr vorhandenes Projekt:

  1.  Öffnen Sie Visual Studio und Azure Mobile Dienstprojekt.
  2.  Maustaste auf Ihr Projekt im **Projektmappen-Explorer** und wählen Sie **veröffentlichen...**
  3.  Klicken Sie auf **Importieren**
  4.  Klicken Sie auf **Durchsuchen** und wählen Sie die heruntergeladene Datei veröffentlichen.  Klicken Sie auf **OK**
  5.  Klicken Sie auf **Verbindung prüfen** veröffentlichen Einstellungen arbeiten.
  6.  Klicken Sie auf **Veröffentlichen** , um Ihre Website veröffentlichen.


## <a name="working-with-your-site"></a>Arbeiten mit der Seite nach der migration

Arbeiten Sie mit der neuen Anwendung in [Azure-Portal] nach der Migration.  Es folgen Hinweise auf bestimmte Operationen verwendet, um im [Klassischen Azure-Portal]mit äquivalenten App Service auszuführen.

### <a name="publishing-your-site"></a>Herunterladen und Veröffentlichen der migrierten site

Die Website steht Git per ftp und kann mit verschiedenen anderen, einschließlich WebDeploy, TFS, Mercurial, GitHub und FTP veröffentlicht.  Anmeldeinformationen für die Bereitstellung sind mit der restlichen Website migriert.  Wenn Ihre Bereitstellung Anmeldeinformationen nicht festgelegt wurde oder Sie sie vergessen, können Sie sie zurücksetzen:

  1. Auf der [Azure-Portal]anmelden.
  2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3. Einstellungen-Blades wird standardmäßig geöffnet.
  4. Klicken Sie auf **Anmeldeinformationen für die Bereitstellung** der Veröffentlichung im Menü.
  5. Geben Sie neue Bereitstellung Anmeldeinformationen in die Textfelder ein und dann auf "Speichern".

Diese Anmeldeinformationen können mit Git clone oder automatisierte Bereitstellung von GitHub, TFS oder Mercurial.  Weitere Informationen finden Sie in der [Dokumentation von Azure App Service-Bereitstellung].

### <a name="appsettings"></a>ApplicationSettings

Die meisten Standardeinstellungen für migrierte Mobilfunkanbieter stehen über App.  Sie erhalten eine Liste der Appeinstellungen [Azure-Portal].
Anzeigen oder Ändern der app:

  1. Auf der [Azure-Portal]anmelden.
  2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3. Einstellungen-Blades wird standardmäßig geöffnet.
  4. Klicken Sie im Menü Allgemein auf **ApplicationSettings** .
  5. Gehen Sie zum Abschnitt App-Einstellungen und Ihre app Einstellung.
  6. Klicken Sie auf den Wert der app-Einstellung den Wert bearbeiten.  Klicken Sie auf **Speichern** , um den Wert zu speichern.

Sie können mehrere Appeinstellungen gleichzeitig aktualisieren.

> [AZURE.TIP]Es gibt zwei Application Settings mit demselben Wert.  Sie sehen z. B. _Anwendungsschlüssel_ und _MS\_Anwendungsschlüssel_.  Aktualisieren Sie beide Einstellungen gleichzeitig.

### <a name="authentication"></a>Authentifizierung

Einstellung für alle sind als App-Einstellungen in der migrierten Site verfügbar.  Aktualisieren Sie Ihre Einstellung für die Formularauthentifizierung müssen Sie die entsprechenden app-Einstellungen ändern.  Die folgende Tabelle zeigt die entsprechende Anwendungseinstellungen für die Authentifizierung:

| Anbieter          | Client-ID                 | Clientschlüssel                | Weitere             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft-Konto | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Hinweis: **MS\_AadTenants** als eine durch Kommas getrennte Liste der Mieter Domänen ("Zulässige Mieter" Felder im Mobile-Portal) gespeichert.

> [AZURE.WARNING] **Verwenden Sie nicht die Authentifizierungsmechanismen im Menü**
>
> Azure App Service bietet ein separates "ohne Code" Authentifizierung und Autorisierung unter der _Authentifizierung / Autorisierung_ Menü und die (abgelehnt) _Mobile_ Authentifizierungsoption im Menü.  Diese Optionen sind nicht kompatibel mit einem migrierten Azure Mobile Service.  Sie können [Ihre Site aktualisieren](app-service-mobile-net-upgrading-from-mobile-services.md) , um Azure App-Authentifizierung nutzen.

### <a name="easytables"></a>Daten

_Daten_ auf Mobile Services wurde durch _Einfache Tabellen_ im Azure-Portal ersetzt.  Einfachen Zugriff auf Tabellen:

  1. Auf der [Azure-Portal]anmelden.
  2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3. Einstellungen-Blades wird standardmäßig geöffnet.
  4. Klicken Sie im Menü "MOBILE" **einfache Tabellen** auf.

Sie können eine Tabelle hinzufügen, indem Sie auf die Schaltfläche **Hinzufügen** oder vorhandenen Tabellen zugreifen, indem Sie auf einen Tabellennamen.  Verschiedene Vorgänge von diesem Blatt einschließlich:

* Table-Berechtigungen ändern
* Betriebsbezogenen Skripts bearbeiten
* Das Schema verwalten
* Löschen der Tabelle
* Löschen des Inhalts der Tabelle
* Löschen von bestimmten Zeilen der Tabelle

### <a name="easyapis"></a>API

_API_ auf Mobile Services wurde durch _Einfache APIs_ in Azure-Portal ersetzt.  Auf einfache APIs zugreifen:

  1. Auf der [Azure-Portal]anmelden.
  2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3. Einstellungen-Blades wird standardmäßig geöffnet.
  4. Klicken Sie im Menü MOBILE **Einfach APIs** auf.

Blatt sind bereits migrierten APIs aufgeführt.  Sie können auch eine API dieses Blatt hinzufügen.  Um eine bestimmte API zu verwalten, klicken Sie auf die API.
Neues Blatt können Berechtigungen anpassen und Bearbeiten der Skripts für die API.

### <a name="on-demand-jobs"></a>Scheduler-Jobs

Alle Steuerprogrammaufträge stehen im Abschnitt Scheduler Job Sammlungen.  Den Planer Zugriff auf Aufträge:

  1. Auf der [Azure-Portal]anmelden.
  2. Wählen Sie **Durchsuchen >**, geben Sie im Feld _Filter_ **Zeitplan** und **Planer**wählen.
  3. Wählen Sie die Sammlung Auftrag für Ihre Website.  Es heißt _Sitename_-Aufträge.
  4. **Klicken Sie auf.**
  5. Klicken Sie unter Verwalten auf **Steuerprogrammaufträge** .

Geplante Aufträge werden mit der Häufigkeit aufgelistet, die vor der Migration angegeben.  Bei Bedarf Aufträge sind deaktiviert.  Einen bedarfsgesteuerter Auftrag ausführen:

  1. Wählen Sie den Einzelvorgang ausführen möchten.
  2. Falls erforderlich, klicken Sie auf **Aktivieren** , um den Auftrag zu aktivieren.
  3. Klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Zeitplan**.
  4. Wählen Sie eine Wiederholung **einmal aus**und dann auf **Speichern**

Ihre Projekte bei Bedarf befinden sich im `App_Data/config/scripts/scheduler post-migration`.  Wir empfehlen [Webaufträge] oder [Funktionen]aller Aufträge bei Bedarf konvertieren.  Schreiben Sie neuen Steuerprogrammaufträge [Webaufträge] oder [Funktionen].

### <a name="notification-hubs"></a>Benachrichtigungshubs

Mobile Dienste verwendet Notification Hubs für Pushbenachrichtigungen.  App Folgendes wird Notification Hub mit Mobile Service nach der Migration zu verknüpfen:

| Anwendung festlegen                    | Beschreibung                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Benachrichtigung Hub Namespace           |
| **MS\_NotificationHubName**             | Namen der Hub                |
| **MS\_NotificationHubConnectionString** | Die Benachrichtigung Hub-Verbindungszeichenfolge   |
| **MS\_NamespaceName**                   | Ein Alias für MS_PushEntityNamespace      |

Benachrichtigung-Hub wird über das [Azure-Portal].  Notieren Sie den Notification Hub (Sie finden die App Einstellungen):

  1. Auf der [Azure-Portal]anmelden.
  2. Wählen Sie **Durchsuchen**>, wählen Sie die **Benachrichtigungshubs**
  3. Klicken Sie auf Benachrichtigungshub des mobilen Diensts zugeordnet.

> [AZURE.NOTE]Die Benachrichtigungshub ist ein "Gemischt", nicht sichtbar.  "Mixed" geben Benachrichtigung Hubs Benachrichtigungshubs und ältere Service Bus-Funktionen nutzen.  [Konvertieren eines gemischten Namespaces] bevor Sie fortfahren.  Sobald die Konvertierung abgeschlossen ist, wird der benachrichtigungshub in [Azure-Portal]angezeigt.

Weitere Informationen finden Sie auf [Benachrichtigungshubs] .

> [AZURE.TIP]Benachrichtigung sind Hubs in [Azure-Portal] weiterhin in der Vorschau.  [Azure-Verwaltungsportal] bleibt für die Verwaltung der Benachrichtigungshubs.

### <a name="legacy-push"></a>Ältere Push-Einstellungen

Wenn Sie Push für den mobilen Dienst vor der Einführung auf Benachrichtigungshubs konfiguriert, verwenden Sie _ältere Push_.  Wenn Sie Push und eine Benachrichtigungshub in der Konfiguration aufgeführt lassen, ist es wahrscheinlich _ältere Push_verwenden.  Dieses Feature wird mit allen Features migriert.  Wir empfehlen jedoch auf Benachrichtigungshubs aktualisieren, sobald die Migration abgeschlossen ist.

In der Zwischenzeit werden alle älteren Push (mit Ausnahme des Zertifikats APN) für App-Einstellungen.  Aktualisieren Sie APN Zertifikat ersetzt die entsprechende Datei im Dateisystem.

### <a name="app-settings"></a>Weitere App

Zusätzliche Anwendung Folgendes sind von Ihrem Mobile migrierten und unter *Einstellungen* > *App-Einstellungen*:

| Anwendung festlegen              | Beschreibung                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Der Name der Anwendung                    |
| **MS\_MobileServiceDomainSuffix** | Domainpräfix. d.h. Azure-mobile.net |
| **MS\_Anwendungsschlüssel**            | Die ANWENDUNGSTASTE                    |
| **MS\_MasterKey**                 | Ihre app-Hauptschlüssel                     |

Anwendungstaste und Hauptschlüssel sind identisch Anwendungsschlüssel aus den ursprünglichen Mobile-Dienst.  Insbesondere wird die ANWENDUNGSTASTE verwenden mobile API Überprüfen von mobilen Clients gesendet.

### <a name="cliequivalents"></a>Befehlszeilen-äquivalente

Mehr können Befehls _Azure mobile_ Ihre Azure Mobile Services-Site verwalten.  Viele Funktionen wurden stattdessen mit dem Befehl _Azure Site_ ausgetauscht.  In der folgenden Tabelle Äquivalente für gemeinsame Befehle zu verwenden:

| _Azure Mobile_ Befehl                     | Entsprechenden Befehl _Azure-Site_            |
| :----------------------------------------- | :----------------------------------------- |
| Mobile Speicherorte                           | Speicherort der Siteliste                         |
| Mobile-Liste                                | Site-Liste                                  |
| Mobile _name_                         | Site _name_                           |
| Mobile Neustart _name_                      | Neustart _name_                        |
| Mobile bereitstellen _name_                     | Bereitstellung bereitstellen _CommitId_ _Namen_ |
| Mobile Schlüsselsatz _ _Name_ _Wert_ _       | Website Appsetting Delete _Key_ _name_ <br/> Website Appsetting _Schlüssel_hinzufügen=_ _Namen_ _ |
| Mobile Config _name_                  | Website Appsetting _name_                |
| Mobile Config _Namen_ _Schlüssel_ abrufen             | Website Appsetting _Key_ _name_          |
| Mobile Config festlegen _Namen_ _Schlüssel_             | Website Appsetting Delete _Key_ _name_ <br/> Website Appsetting _Schlüssel_hinzufügen=_ _Namen_ _ |
| Mobile Domain _name_                  | Site Domain _name_                    |
| Mobile Domain _Name_ _Domain_ hinzufügen          | Standortdomäne _Domain_ _Name_ hinzufügen            |
| Mobile Domäne löschen _name_                | Website-Domäne löschen _Domain_ _name_         |
| Skala _name_                   | Site _name_                           |
| Skala ändern _name_                 | Site skalieren Modus _Modus_ _name_ <br /> Skalierung Instanzen _Instanzen_ _Namen_ |
| Mobile Appsetting _name_              | Website Appsetting _name_                |
| Mobile Appsetting _ _Schlüssel_ _Wert_ _ hinzufügen | Website Appsetting _Schlüssel_hinzufügen=_ _Namen_ _   |
| Mobile Appsetting Delete _Name_ _Schlüssel_      | Website Appsetting Delete _Key_ _name_        |
| Mobile Appsetting Show _Name_ _Schlüssel_        | Website Appsetting Delete _Key_ _name_        |

Aktualisieren Sie Authentifizierung bzw. Push Notification durch die entsprechende Einstellung der Anwendung aktualisieren.
Bearbeiten Sie und veröffentlichen Sie Ihrer Website über ftp oder Git.

### <a name="diagnostics"></a>Diagnose und Protokollierung

Diagnoseprotokoll ist in Azure App Service normalerweise deaktiviert.  Diagnose-Protokollierung:

  1. Auf der [Azure-Portal]anmelden.
  2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3. Einstellungen-Blades wird standardmäßig geöffnet.
  4. Wählen Sie im Menü Funktionen **Diagnoseprotokolle** .
  5. Klicken Sie **auf** die folgenden Protokolle: **Anwendung protokolliert (Dateisystem)**, **detaillierte Fehlermeldungen**und **Fehler Anforderung verfolgen**
  6. Klicken Sie auf **Dateisystem** für Web Server-Protokollierung
  7. Klicken Sie auf **Speichern**

Zum Anzeigen der Protokolle:

  1. Auf der [Azure-Portal]anmelden.
  2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen des migrierten Mobile Service.
  3. Klicken Sie auf die Schaltfläche **Extras**
  4. Wählen Sie **Datenstrom** Menü BEOBACHTEN.

Protokolle werden im Fenster angezeigt, während sie generiert werden.  Sie können auch die Protokolle für die spätere Analyse der Bereitstellung Anmeldeinformationen. Weitere Informationen finden Sie in der Dokumentation zu [Protokollieren] .

## <a name="known-issues"></a>Bekannte Probleme

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Ein Klon migriert Mobile App löschen verursacht eines

Klonen den migrierten mobilen Service mit Azure PowerShell, Sie löschen den Klon, wird der DNS-Eintrag für den Produktionsdienst entfernt.  Die Website ist nicht mehr über das Internet.  

Lösung: Wenn Sie Ihre Website kopieren möchten, dazu über das Portal.

### <a name="changing-webconfig-does-not-work"></a>Ändern von "Web.config" funktioniert nicht

Haben Sie eine ASP.NET Website ändert sich in die `Web.config` Datei werden nicht angewendet.  Azure App Service erstellt eine geeignete `Web.config` Datei während des Startvorgangs die Runtime Mobile Dienste unterstützen.  Sie können bestimmte Eigenschaften (z. B. benutzerdefinierte Header) mithilfe einer XML-Transformationsdatei überschreiben.  Erstellen Sie eine Datei genannt `applicationHost.xdt` -Datei muss am Ende in die `D:\home\site` Verzeichnis auf den Azure-Dienst.  Hochladen der `applicationHost.xdt` über einen benutzerdefinierten Bereitstellungsskripts Datei oder direkt über Kudu.  Im folgenden wird ein Beispieldokument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Weitere Informationen finden Sie in der Dokumentation [XDT transformieren Beispiele] auf GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Migrierte Mobile Dienste kann nicht Traffic Manager hinzugefügt werden

Beim Erstellen eines Profils Traffic Manager kann nicht direkt einen migrierten mobilen Dienst zum Profil auswählen.  Verwenden Sie einen "externen Endpunkt".  Der externe Endpunkt kann nur über PowerShell hinzugefügt werden.  Weitere Informationen finden Sie im [Lernprogramm Traffic Manager](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Nächste Schritte

Damit Ihre Anwendung App Service migriert wird, sind noch mehr Features verwenden können:

  * Bereitstellung [staging Slots] können Sie Änderungen auf Ihrer Website und ein Test B.
  * [Webaufträge] bieten einen Austausch bei Bedarf geplante Aufträge.
  * Können [kontinuierlich bereitstellen] Ihrer Website verknüpfen Ihrer Website GitHub, TFS oder Mercurial.
  * [Anwendung Erkenntnisse] können auf Ihrer Site überwachen.
  * Dienen Sie eine Website und eine Mobile API der gleiche Code.

### <a name="upgrading-your-site"></a>Die Mobile Website aktualisieren Azure Mobile Apps SDK

  * Node.js-basierten Server Projekte bietet neue [Mobile Apps Node.js SDK] einige neue Features. Beispielsweise können Sie jetzt tun, Entwicklung und Debuggen, verwenden alle Node.js-Version über 0,10 und mit jeder Express.js Middleware anpassen.

  * Für. NET-basierten Server-Projekte neue [Mobile Apps SDK NuGet-Pakete](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) flexibler NuGet Abhängigkeiten.  Diese Pakete die neue App-Authentifizierung unterstützen und alle ASP.NET Projekt erstellen. Mehr zum Aktualisieren finden Sie unter [Aktualisieren der vorhandenen .NET Mobile Service App Service](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App Servicepreise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Anwendung Einblicke]: ../application-insights/app-insights-overview.md
[Automatische Skalierung]: ../app-service-web/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Dokumentation der Azure App Service-Bereitstellung]: ../app-service-web/web-sites-deploy.md
[Azure-Verwaltungsportal]: https://manage.windowsazure.com
[Azure-portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler Pläne]: ../scheduler/scheduler-plans-billing.md
[kontinuierlich bereitstellen]: ../app-service-web/app-service-continuous-deployment.md
[Konvertieren eines gemischten namespaces]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[Benutzerdefinierte Domänennamen]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[allgemeine Verfügbarkeit von Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybridverbindungen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Protokollierung]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobiler Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobile und App-Dienste]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Benachrichtigungshubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Performance-Überwachung]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Sichern Sie den Mobile-Dienst]: ../mobile-services/mobile-services-disaster-recovery.md
[Staging-Steckplätze]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[Webaufträge]: ../app-service-web/websites-webjobs-resources.md
[XDT Transformation Samples]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funktionen]: ../azure-functions/functions-overview.md

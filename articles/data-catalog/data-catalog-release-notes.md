<properties
   pageTitle="Azure Data Catalog-Versionshinweise | Microsoft Azure"
   description="Versionsinformationen für Azure Data Catalog."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure Data Catalog-Versionsinformationen

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Versionshinweise für vom 20. November 2015 von Azure Data Catalog

### <a name="opening-data-sources-in-power-bi-desktop"></a>Datenquellen in Power BI Desktop öffnen

Bei Verwendung der Option "Open in Power BI Desktop" **Azure Data Catalog** -Portal können Benutzer zwei Probleme in der Anwendung Power BI Desktop auftreten:

- Ein Dialogfeld mit dem Titel "Nicht zum Öffnen des Dokuments" angezeigt
- Power BI Desktop-Anwendung geöffnet wird, aber die Datei ist leer

Für jede Situation kann das Problem durch Herunterladen und Installieren der neuesten Version von Power BI Desktop vom [PowerBI.com](https://powerbi.com)behoben werden.

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Release Notes für den 13. November 2015 von Azure Data Catalog

### <a name="registering-and-connecting-to-teradata"></a>Registrieren und Teradata herstellen

Beim Herstellen einer Teradata Datenquellen Benutzer müssen den richtigen Teradata ODBC-Treiber installiert haben, die die Bitness der verwendeten Software (32-Bit- oder 64-Bit) entsprechen.

Der aktuelle [Teradata ODBC-Treiber für Windows (Version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) ab Version ADC ist kompatibel mit Office 2013, jedoch nicht mit Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Release Notes für den 13. Juli 2015 von Azure Data Catalog

### <a name="registering-and-connecting-to-oracle-database"></a>Registrieren und eine Verbindung zu Oracle-Datenbank

Beim Verbinden mit müssen Oracle Data Sources Datenbankbenutzer die richtigen Oracle-Treiber installiert, die die Bitness der verwendeten Software (32-Bit- oder 64-Bit) entsprechen.

-   Oracle-Datenquellen auf einem Computer mit 32-Bit Windows registrieren, werden die 32-Bit-Oracle-Treiber verwendet
-   Oracle-Datenquellen auf einem Computer mit 64-Bit Windows registrieren, werden die 64-Bit-Oracle-Treiber verwendet
-   Bei der Verbindung mit Oracle-Datenquellen auf einem Computer mit 32-Bit-Version von Microsoft Office Excel mit werden auf 64-Bit-Windows einschließlich, 32-Bit-Oracle-Treiber verwendet
-   Bei der Verbindung mit Oracle-Datenquellen auf einem Computer mit 64-Bit-Version von Microsoft Office Excel mit werden 64-Bit-Oracle-Treiber verwendet

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registrieren und eine Verbindung zu SQL Server Reporting Services

Unterstützung für SQL Server Reporting Services (SSRS) Datenquellen ist derzeit nur Server im einheitlichen Modus auf. SSRS wird im SharePoint-Modus in einer späteren Version unterstützt werden.

### <a name="opening-data-assets-in-excel"></a>Datenbestand in Excel öffnen

Beim Öffnen von Daten in Microsoft Excel in **Azure Data Catalog** -Portal können Benutzer das Dialogfeld **Sicherheitshinweis für Microsoft Excel** aufgefordert. Standard, erwartet, und Benutzer können weiterhin **ermöglichen** .

Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren von Sicherheitshinweisen Links zu Dateien, die von verdächtigen Websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy und Richtlinie Konfigurations- und Registrierung

Benutzer können eine Situation auftreten, Anmeldung können Azure Data Catalog-Portal, aber beim Data Source-Registrierungstool melden sie eine Fehlermeldung, die Anmeldung verhindert.

Es gibt zwei mögliche Ursachen für dieses Problemverhalten:

**Ursache 1: Konfiguration von Active Directory Federation Services** Das Source-Registrierungstool verwendet Formularauthentifizierung Anmeldeversuche in Active Directory überprüft. Für erfolgreiche Anmeldung muss Formularauthentifizierung von einem Active Directory-Administrator in der globalen Richtlinie Authentifizierung aktiviert.

In einigen Situationen kann dies Fehler auftreten, nur wenn der Benutzer im Unternehmensnetzwerk oder nur, wenn der Benutzer von außerhalb des Unternehmensnetzwerks verbunden ist. Globale Authentifizierungsrichtlinie können Authentifizierungsmethoden für Intranet- und extranet-Verbindungen separat aktiviert werden. Fehler bei der Anmeldung möglicherweise Formularauthentifizierung nicht für das Netzwerk aktiviert ist, aus dem der Benutzer verbunden ist.

Weitere Informationen finden Sie unter [Konfigurieren von Authentifizierungsrichtlinien](https://technet.microsoft.com/library/dn486781.aspx).

**Ursache 2: Netzwerk-Proxy-Konfiguration** Wenn das Unternehmensnetzwerk einen Proxyserver verwendet, das Registrierungstool über den Proxy eine Verbindung zu Active Directory Azure möglicherweise nicht. Benutzer können sicherstellen, dass das Registrierungstool durch Bearbeiten der Konfigurationsdatei das Tool in diesem Abschnitt der Datei hinzufügen:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Suchen die Datei RegistrationTool.exe.config, die Registrierung starten und dann öffnen Sie den Windows Task-Manager. Auf der Registerkarte Details im Task-Manager mit der rechten Maustaste auf RegistrationTool.exe, und wählen Sie im Popupmenü Dateipfad öffnen.

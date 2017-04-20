<properties
    pageTitle="Neuigkeiten in Azure Toolkit für Eclipse"
    description="Erfahren Sie mehr über die neuesten Funktionen der Azure-Toolkit für Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->

# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>Neuigkeiten in Azure Toolkit für Eclipse

## <a name="azure-toolkit-for-eclipse-releases"></a>Azure-Toolkit für Eclipse-Versionen

Dieser Artikel enthält Informationen über die verschiedenen Versionen und Aktualisierungen der Azure-Toolkit für Eclipse.

> [AZURE.NOTE] Es gibt auch ein Azure-Toolkit für die IDE IntelliJ. Weitere Informationen finden Sie unter [Azure-Toolkit für IntelliJ].

### <a name="august-26-2016"></a>26 August 2016

Azure-Toolkit für Eclipse - August 2016 Version umfasst die folgenden Funktionen:

* **Benutzerdefinierte JDK-Distributionen**. Azure-Toolkit für Eclipse unterstützt jetzt angeben und eine beliebige JDK-Version Ihre Azure WebApp Container bereitstellen:
  - Neben den JDKs von Azure bereitgestellt können Sie auch Auswahl Zulu OpenJDK Versionen auf Azure durch Azul zur Verfügung gestellt.
  - Sie können auch eigene JDK Verteilung als ZIP-Datei an das Speicherkonto hochladen.
* **Verbesserte Azure Explorer-Ansicht**:
  - Unterstützung für Virtual Machine Azure neue Ressourcenmanager Modell: auflisten, erstellen und Löschen von Resource Manager-basierten virtuellen Maschinen ohne die IDE.
  - Unterstützung für das Speicherkonto BLOB-Management mit Azure Ressourcen-Manager die Funktionalität für die Verwaltung der "klassischen" Speicherkonten ergänzt.
* **Microsoft JDBC-Treiber für SQL Server 6.0**. Dieses Update enthält den neuesten JDBC-Treiber für Microsoft SQL Server (6.0) ist jetzt als Bibliothek, die Ihre Java-Projekte problemlos hinzugefügt werden können, damit die ältere Version ersetzen.

### <a name="june-29-2016"></a>Am 29. Juni 2016

Azure-Toolkit für Eclipse - Juni 2016 Version umfasst die folgenden Funktionen:

* **Java 8 erforderlich**. Azure-Toolkit für Eclipse müssen Java 8, obwohl diese Anforderung nur für das Toolkit gilt-Anwendung mithilfe von Azure unterstützt alle Versionen von Java fortfahren können.
* **Unterstützung für die neuesten Java-JDKs**. Die neuesten Versionen der Java-JDKs werden jetzt von Azure Toolkit für Eclipse unterstützt.
* **Unterstützung für Azure SDK v2.9.1**. Die neueste Version von Azure SDK ist jetzt die minimale Voraussetzung für Azure Toolkit für Eclipse.
* **Integrierte Beispiele**. Azure-Toolkit für Eclipse verfügt über verschiedene Anwendungsbeispiele Entwickler beginnen können.
* **Integration von HDInsight-Tools**. Azure HDInsight Tools sind jetzt mit dem Azure-Toolkit für Eclipse gebündelt. Weitere Informationen finden Sie unter [HDInsight Tools Plugin für Eclipse].
* **Remotedebuggen Java Web Apps**. Azure-Toolkit für Eclipse unterstützt jetzt das Remotedebuggen Java webapps in Azure App Service.
* **Unterstützung für Eclipse Luna-Version.** Die neue erforderliche Eclipse-IDE Mindestversion ist Luna.

### <a name="april-12-2016"></a>12 April 2016

Azure-Toolkit für Eclipse - April 2016 Version umfasst die folgenden Funktionen:

* **Unterstützung für Azure SDK v2.9.0**. Die neueste Version von Azure SDK ist jetzt die minimale Voraussetzung für Azure Toolkit für Eclipse.
* **Sonstige Usability, Reaktionsfähigkeit und Leistung Verbesserungen Azure Web App Support**. Eine Reihe von Performance-Optimierungen in Azure Ergebnis wie das Toolkit kommuniziert reaktionsfähigere Benutzeroberfläche.
* **Möglichkeit, eine vorhandene Anwendung in Azure in Eclipse wirklich**. Azure-Toolkit für Eclipse ermöglicht das Löschen eines vorhandenen Containers Azure Web ohne Eclipse.

### <a name="march-7-2016"></a>7 März 2016

Azure-Toolkit für Eclipse - März 2016 Version umfasst die folgenden Funktionen:

* **Unterstützung für die schnelle Bereitstellung einfacher Java-Anwendung**. Azure-Toolkit für Eclipse unterstützt jetzt die schnelle Bereitstellung von einfachen Java-Anwendung in Azure Web App Container so statt Minuten Java-Anwendung jetzt bereitstellen Sekunden.
* **Unterstützung für Web App die Azure Explorer-Ansicht**. Azure Explorer-Ansicht im Toolkit unterstützt nun für auflisten, starten und Beenden von Azure Web Apps.
* **Verteilung von Tomcat aktualisiert, Steg und Zulu OpenJDK**. Azure-Toolkit für Eclipse bietet Unterstützung für aktualisierte Versionen der Tomcat Steg und Zulu OpenJDK für Java-Installationen in Azure Cloud Services.

### <a name="january-4-2016"></a>4. Januar 2016

Azure-Toolkit für Eclipse - Januar 2016 Version umfasst die folgenden Funktionen:

* **Zulu OpenJDK Updates unterstützt**. Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Tomcat aktualisiert und Steg Distributionen**. Steg und Tomcat-Distributionen sind Microsoft Azure Azure-Toolkit zur Verfügung Eclipse wurden aktualisiert.
* **Featureparität Eclipse bis IntelliJ Toolkits für Azure**. Azure-Toolkit für Eclipse und [Azure-Toolkit für IntelliJ] unterstützt nun dieselben Funktionen.

### <a name="september-1-2015"></a>1. September 2015

Azure-Toolkit für Eclipse - September 2015 Version umfasst die folgenden Funktionen:

* **Zulu OpenJDK Updates unterstützt**. Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Tomcat aktualisiert und Steg Distributionen**. Steg und Tomcat-Distributionen sind Microsoft Azure Azure-Toolkit zur Verfügung Eclipse wurden aktualisiert. (Diesen Distributionen ermöglicht Entwicklern die schnelle Entwicklung und Testprojekte mit dem Azure-Toolkit für Eclipse.
* **Unterstützung für Tomcat und Steg Verweise automatisch aktualisiert**. Zusätzlich bestimmten Versionen von Tomcat und Steg auf Azure, können Entwickler jetzt als Verteilung verweisen die "neueste (automatische Aktualisierung)", die automatisch zu der neuesten jede Hauptversion Steg oder Tomcat nächsten die Rolleninstanzen aktualisiert werden wiederverwendet. (Wiederverwendung automatisch, aber Entwickler können eine Wiederverwendung durch Azure-Portal manuell auslösen.) Dieses neue Feature bedeutet, dass Entwickler nicht erneut bereitstellen ihrer Anwendung zu ihrer Serversoftware aktualisiert haben. (
*  Diese Funktionalität ist derzeit nur für Entwicklungs- und Testzwecke oder nicht geschäftskritischen Applikationen vorgesehen und für die Produktion nicht empfohlen.)
* **Azure Explorer-Ansicht für Blobs, Queues und Tabellen in Azure-Speicher**. Dadurch können Entwickler eine Reihe von Aufgaben mit ihren Speicher direkt von der Eclipse-IDE ausführen. Beispiel: löschen, hochladen oder Herunterladen von Blobs.

### <a name="august-1-2015"></a>1. August 2015

Azure-Toolkit für Eclipse - August 2015 Version umfasst die folgenden Funktionen:

* **Anwendung Einblicke Instrumentation Schlüsselmanagement**. Dieses Update können Sie erhalten, erstellen und verwalten Anwendung Einblicke Instrumentation direkt Eclipse-IDE.
* **Microsoft JDBC-Treiber für SQL Server 4.1**. Dieses Update unterstützt die neueste JDBC-Treiber für Microsoft SQL Server.
* **Version 2.7 Azure SDK**. Neue Voraussetzung für das Toolkit unter Windows ist Azure SDK diese Aktualisierung (Beachten Sie, dass diese nicht auf nicht-Windows-Betriebssystemen erforderlich ist.)
* **Unterstützung für v7 Zulu OpenJDK aktualisieren**. Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].

### <a name="may-1-2015"></a>1. Mai 2015

Azure-Toolkit für Eclipse - Version 2015 Mai umfasst die folgenden Funktionen:

* **Verbesserte Serverauswahl Benutzeroberfläche**. Vorliegende Version vereinfacht die Verwendung des Toolkits auf nicht-Windows-Betriebssystemen.
* **Unterstützung für Maven Projekte**. Diese Version unterstützt Maven Projekte als Anwendung, die das Toolkit in Azure bereitstellen kann und Application Insights konfigurieren.
* **Version 2.6 von Azure SDK**. Neue Voraussetzung für das Toolkit unter Windows ist Azure SDK diese Aktualisierung (Beachten Sie, dass diese nicht auf nicht-Windows-Betriebssystemen erforderlich ist.)
* **Deployment-Aktualisierung veröffentlichen**. Wenn Sie ein Bereitstellungsprojekt veröffentlichen, wenn die vorherige Version noch aktiv ist, verwendet das Toolkit Azure Bereitstellung aktualisieren Funktionalität jetzt anstelle der vorherigen Bereitstellung heruntergefahren und völlig neu veröffentlichen, wie in der Vergangenheit. Dies ermöglicht dem Cloud-Dienst ohne Unterbrechung möglichst hohe Verfügbarkeit auch bei einer Aktualisierung zu und beschleunigt die erneut veröffentlichen.
* **Unterstützung für die neuesten Zulu OpenJDK v8 - update 40**. Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].

### <a name="march-9-2015"></a>9 März 2015

Azure-Toolkit für Eclipse - Version März 2015 umfasst die folgenden Funktionen:

* **Unterstützung für Mac, Ubuntu und zusätzliche Linux Versionen**. Diese Version des Toolkits Azure für Eclipse unterstützt Mac OS und Unix-Plattformen mehrere Entwickler zum Erstellen, konfigurieren und Java Projekte von Eclipse unter anderen Betriebssystemen als Windows Azure Cloud Services (PaaS) veröffentlichen Toolkit installieren.

>[AZURE.NOTE] Diese Funktion ist in der Vorschau und sollte nicht für die Verwendung in der Produktion. Gibt keine Kunden-Support Service Level Agreement (SLA) jedoch Feedback geschätzt und empfohlen.

* **Neue Anwendung Erkenntnisse Plugin**. Entwickler können jetzt automatisch Telemetrie mit Anwendung Einblicke in Azure konfigurieren.
* **Befehlszeile Ant-basierte Automatisierung**. Dieses Feature ermöglicht Entwicklern die Veröffentlichung neuere Versionen ihrer Bereitstellung mit Ant außerhalb Eclipse automatisieren. Zuvor generierte Skript wird automatisch für ein Projekt konfiguriert, nach der erstmaligen von Eclipse Bereitstellung und nachfolgende Bereitstellungen das Skript um Bereitstellung über die Befehlszeile vollständig zu automatisieren können.
* **Tomcat und Steg Verfügbarkeit Azure für einfachere, schnellere Bereitstellung**. Entwickler können jetzt Tomcat und Steg Varianten verweisen die in Azure direkt stattdessen zu einen Java-Server auf ihre Konten (oder über das Toolkit) so Hochladen keine Notwendigkeit zum Hochladen eines Java Servers für schnelle, Einsteiger-Szenarien besteht.
* **Tastenkombination zum Veröffentlichen Java webapps Azure Cloud Services**. Um die Lernkurve für einfache Szenarien mit Entwicklung und Tests zu reduzieren, können Entwickler jetzt Java-Anwendung direkt in Azure veröffentlichen. Anstatt das Erstellen und Konfigurieren von Azure Bereitstellungsprojekt durchlaufen werden Applikationen mit einer Standardinstanz Tomcat v8 und Zulu JVM (OpenJDK) bereitgestellt.

### <a name="january-30-2015"></a>30. Januar 2015

Azure-Toolkit für Eclipse - Januar 2015 Version umfasst die folgenden Funktionen:

* **Unterstützung für IBM® WebSphere® Application Server Liberty Core**. Diese Version fügt IBM WebSphere Application Server Liberty Core zur Liste der unterstützten Anwendungsserver, aus denen das Toolkit in Azure bereitstellen kann. Diese neueste erweitert die aktuelle Liste der unterstützten Anwendungsserver &quot;Out of Box&quot; durch das Toolkit bereits enthalten verschiedene Versionen von Tomcat, Steg JBoss und GlassFish.
* **Aufnahme von Application Insights SDK**. Diese neu freigegebene Client-API-Bibliothek (v0.9.0) ist jetzt Teil des Pakets für Azure Bibliotheken für Java.
* **Paket für Azure Bibliotheken für Java aktualisiert**. Dieses Update enthält Azure Bibliotheken für Java v0.7.0 und Storage-Client-API Version 2.0.0 sowie v0.9.0 Application Insights-SDK neu veröffentlicht.

### <a name="november-12-2014"></a>12. November 2014

Azure-Toolkit für Eclipse - Version November 2014 umfasst die folgenden Funktionen:

* **Support für Azure SDK 2.5**. Diese neueste Aktualisierung von Azure SDK ist neue Voraussetzung für das Toolkit.
* **Unterstützung für aktualisierte Version Zulu OpenJDK v1. 8, v1. 7 und v1. 6 Pakete**. Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Unterstützung für die neuen Standard D Größen für Cloud-Services**erhöht die bieten Leistung und zusätzliche Speicherressourcen Weitere Informationen finden Sie unter [virtuelle Computer und Azure Cloud Service Größe].

### <a name="october-17-2014"></a>17. Oktober 2014

Azure-Toolkit für Eclipse - Oktober 2014 Version umfasst die folgenden Funktionen:

* **Leistungssteigerungen in Szenarien Cloud veröffentlichen**. Laden von Abonnementinformationen ist schneller, wenn Benutzer mehrere Abonnements und Speicherkonten.
* **Unterstützung für aktualisierte Version des Zulu OpenJDK v1. 8**. Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Unterstützung für ältere Versionen von 3rd Party JDKs veralteter**. Veraltete JDK-Paketen werden nicht mehr für neue Bereitstellungsprojekte im Dropdownmenü angezeigt. Vorhandene Projekte verweisende veraltete JDK-Paketen können dazu Zeit wird, aber es wird empfohlen, solche Projekte auf die neuesten fortgesetzt werden.
* **Aktualisierte Version des Pakets für Azure Bibliotheken für Java-Client-API-Bibliothek**. Weitere Informationen finden Sie unter [Microsoft Azure-Client-API].
* **Fehlerkorrekturen.** Dieses Release enthält zahlreiche verschiedene Fehlerbehebungen, auf Berichte und testen.

### <a name="august-5-2014"></a>5. August 2014

Azure-Toolkit für Eclipse - August 2014 Version umfasst die folgenden Funktionen

* **Support für Azure SDK 2.4.** Ältere Versionen von Eclipse Toolkit funktioniert nicht mit diesem neu freigegebenen SDK.
* **Aktualisierte Versionen der Version 1.6 Zulu OpenJDK 1.7 und v1. 8-Pakete.** Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Aktualisierte Version des Pakets für Azure Bibliotheken für Java-Client-API-Bibliothek.** Weitere Informationen finden Sie unter [Microsoft Azure-Client-API].
* **Unterstützung für aktuelle veröffentlichen Format der Einstellungsdatei.** Unterstützung wurde für Version 2.0 des Dateiformats Veröffentlichungseinstellungen hinzugefügt.
* **Architektonische Änderung hinter veröffentlichen Cloud-Funktion.** Das Toolkit verwendet nun die gerade veröffentlichte Microsoft Azure Client-API für Java für seine Unterstützung Cloud veröffentlichen.
* **Fehlerkorrekturen.** Diese Version enthält eine Reihe von Fehlerbehebungen Benutzer angefordert.

### <a name="june-12-2014"></a>12. Juni 2014

Azure-Toolkit für Eclipse - Juni 2014 Release ist eine Wartung Aktualisierung bietet die folgenden Funktionen:

* **Unterstützung für die Zulu OpenJDK Paket v1. 8.** Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Aktualisierte Versionen der Zulu OpenJDK Version 1.6 und 1.7.** Weitere Informationen finden Sie auf der [Webseite Zulu OpenJDK Azul Systeme].
* **Aktualisierte Version des Pakets für Azure Bibliotheken für Java-Client-API-Bibliothek.** Weitere Informationen finden Sie unter [Microsoft Azure-Client-API].
* **Fehlerkorrekturen.** Diese Version enthält eine Reihe von Fehlerbehebungen Benutzer angefordert.

### <a name="april-4-2014"></a>4. April 2014

Azure-Plugin für Eclipse - Version April 2014 veröffentlicht. Dies ist ein Update begleitenden Release von Azure SDK 2.3, Voraussetzung und wird automatisch heruntergeladen, wenn Sie das Plug-in installieren. Dieses Update enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit Februar 2014 Vorschau:

* **Support für die Version 2.3 von Azure SDK.** Azure-Plugin für Eclipse - Version April 2014 erfordert Azure SDK 2.3. Wenn das neue Plug-in verwenden Sie Azure SDK 2.3 noch nicht, werden Sie aufgefordert, die Installation zuzulassen. Verwenden Sie Azure SDK 2.3 nicht mit früheren Versionen des Plugins.
* **Aktualisierung ohne vollständige Bereitstellung.** Wenn Java-Anwendung bereitstellen, die Teil des Projekts sind, lädt das Plug-in jetzt automatisch sie in Ihrem ausgewählten Speicherkonto damit aktualisieren und die Instanzen der aktuellen Anwendungsbits wiederverwenden ohne und das gesamte Paket erneut bereitstellen.
* **Tomcat 8 ist jetzt erkannten Anwendungsserver.** Wenn Sie Tomcat 8-Installationsverzeichnis auf Ihrem Computer in der Registerkarte **Server** des Dialogfelds **Azure Bereitstellungsprojekt** auswählen, werden das Plug-in jetzt automatisch erkannt und Tomcat 8 automatisiert, ähnlich den älteren Versionen der Tomcat bereits in der Liste bereitstellen.
* **Azul Zulu OpenJDK Paket-Updates: V1. 7 Update 51 und v1. 6 update 47.** Effektiv mit dieser Version ist Azul System Zulu Open JDK v7 Aktualisierungspaket 51 verfügbar. Außerdem starten Zulu Open JDK V6-Pakete verfügbar, beginnend mit Update 47. Diese Updates sind neben den zuvor geöffneten JDK Zulu v7 update 45 update 40 und 25 aktualisieren.
* **Unterstützung für A8 und A9 Microsoft Azure Virtual Machine.** Sie können nun einen Cloud-Dienst Speicher A8 und A9 Virtual Machine Größen bereitstellen. Weitere Informationen zu diesen Größen VM finden Sie unter [virtuelle Computer und Azure Cloud Service Größe].
* **Automatische Umleitung von HTTP auf HTTPS für Rollen SSL aktiviert.** Wenn die Benutzeranfrage HTTP gibt der Cloud-Dienst nur HTTPS Rollen enthält, wird es automatisch HTTPS umgeleitet. Gibt es eine separate Rolle der HTTP-Anfragen erstellen müssen.
* **Express-Emulator für lokale Emulation verwendet.** Azure Express Emulator jetzt als Emulator dient die Anwendung lokal zu debuggen.
* **Azure hat als Microsoft Azure umbenannt.** Benutzeroberflächenbildschirme geben jetzt Azure umbenannt und Azure nicht aufgerufen wurde.

### <a name="february-6-2014"></a>6. Februar 2014

Azure-Plugin für Eclipse - Februar 2014 Vorschau zur Verfügung. Dieses Update enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit Oktober 2013 Vorschau:

* **Unterstützung für SSL-Abladung.** Secure Sockets Layer (SSL) Verschiebung wurde als Feature ermöglicht einfache Hypertext Transfer Protocol Secure (HTTPS) in der Java-Bereitstellung in Azure-Unterstützung ohne SSL in Ihren Java-Anwendungsserver konfigurieren hinzugefügt. Dies ist besonders wichtig in Sitzungsaffinität bzw. Kommunikationsszenarien authentifiziert. Beispielsweise bei Filter Access Control Service (ACS) die vom Toolkit bereits unterstützt wird. Weitere Informationen finden Sie unter [SSL-Abladung] und [zum SSL-Abladung verwenden].
* **GlassFish 4 ist jetzt erkannten Anwendungsserver.** Bei Auswahl von GlassFish 4-Installationsverzeichnis auf Ihrem Computer in der Registerkarte **Server** des Dialogfelds **Azure Bereitstellungsprojekt** werden Plug-in jetzt automatisch erkannt und GlassFish OSE 4 automatisiert, ähnelt der GlassFish OSE 3 Version bereits in der Liste bereitstellen.
* **Azul Zulu OpenJDK Aktualisierungspaket 45.** Effektive dieser Version steht Azul System Zulu (Open JDK v7 Paket) Update 45; Dies ist neben der zuvor verfügbare Update 40 und Update 25.
* **Unterstützung für 'Auto' für Private Endpunkt-Ports.** Privaten Port lassen sich automatisch für input Endpunkte und interne Endpunkte Azure diesen Endpunkt einen Anschluss automatisch zuweisen zu lassen. Zuvor konnten Sie nur eine bestimmte Portnummer zuweisen.
* **Unterstützung der Name (CN) des Zertifikats selbst signiertes Zertifikat erstellen Benutzeroberfläche anpassen.** Zuvor wurde dieselbe hartcodierte Name für alle neuen Zertifikate verwendet. Jetzt können Sie Ihre eigenen Zertifikat unter mehrere Zertifikate im Azure-Portal für verschiedene Zwecke unterscheiden angeben.
* **Azure Symbolleiste:** Die Azure-Symbolleiste enthält eine aktualisierte wie folgt geändert: 
    * ![][ic710876]Dieses Symbol wurde für das **Neue Azure-Bereitstellungsprojekt**hinzugefügt.
    * ![][ic710877]Dieses Symbol wurde als Verknüpfung selbstsigniertes Zertifikat erstellen Dialogfeld hinzugefügt.
* **Unterstützung für Größe A5 Azure Virtual Machine.** Sie können nun einen Cloud-Dienst in den hohen Speicherbereich Größe A5 virtuellen Computer bereitstellen. Weitere Informationen zu dieser VM-Größe finden Sie unter [virtuelle Computer und Azure Cloud Service Größe].
* **Unterstützung für Microsoft Windows Server 2012 R2.** Sie können nun Windows Server 2012 R2 als Cloud-Betriebssystem auswählen.

### <a name="october-22-2013"></a>22 Oktober 2013

Azure-Plugin für Eclipse - Oktober 2013 Preview veröffentlicht. Dieses Update enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit September 2013 Vorschau:

* **Unterstützung für Azure SDK 2.2 Version.** Azure-Plugin für Eclipse - Vorschau Oktober 2013 Azure SDK 2.2 unterstützt. Das Plug-in funktioniert auch mit Azure SDK 2.1 und Azure SDK 2.2 automatisch installiert, wenn Sie nicht bereits mindestens Azure SDK 2.1 installiert haben.
* **Azul Zulu OpenJDK Aktualisierungspaket 40.** Wie angekündigt September 2013 Vorschau der Plugin jetzt können über Drittanbieter bereitgestellten JDK direkt Azure ohne eigene JDK hochladen. Im Oktober 2013 Release steht Azul System Zulu (Open JDK v7 Paket) Update 40; Dies ist neben der ursprünglich veröffentlichten Update 25.
* **Cloud-Bereitstellung Link im Aktivitätsprotokoll.** Im Aktivitätsprotokoll Azure bei die Bereitstellung den Status **veröffentlicht**hat klicken Sie **veröffentlicht** jetzt eine Verknüpfung mit der Bereitstellung ist. die Bereitstellung wird im Browser geöffnet. (Der Status **veröffentlicht** wurde zuvor **ausgeführt**bezeichnet.)
* **Ziel-OS-Auswahl zur Veröffentlichung.** Das Dialogfeld **in Azure veröffentlichen** enthält ein neues Feld **Ziel OS**bietet leichter können Sie Ihr Ziel-Betriebssystem festgelegt.
* **Vorherige Bereitstellung automatisch überschrieben.** **Veröffentlichen in Azure** -Dialogfeld enthält ein neues Kontrollkästchen **Vorherige Bereitstellung überschreiben**. Wenn diese Option aktiviert ist, wenn die neue Bereitstellung erscheint überschreiben automatisch die vorherige Bereitstellung; nicht auftreten &quot;409 Conflict&quot; ausgibt, wenn dort veröffentlichen, ohne die erste Veröffentlichung der vorherigen Bereitstellung.
* **Steg 9 ist jetzt erkannten Anwendungsserver.** Bei Auswahl von Steg 9-Installationsverzeichnis auf Ihrem Computer in der Registerkarte **Server** des Dialogfelds **Azure Bereitstellungsprojekt** werden Plug-in jetzt automatisch erkannt und Steg 9 automatisiert, ähnlich den älteren Versionen der Anleger bereits in der Liste bereitstellen.
* **Rolle im Projekt-Kontextmenü hinzufügen.** Das Kontextmenü der **Azure** -Projekt enthält nun ein neues Menüelement **Funktion hinzufügen**, eine schnellere und mehr sichtbar können Sie Ihre Azure-Projekt eine neue Rolle hinzu.
* **Aktualisieren des Pakets für die Azure-Bibliotheken für Java-Bibliothek.** Diese Version 0.4.6 [Microsoft Azure-Client-API]basiert.

### <a name="september-25-2013"></a>25 September 2013

Azure-Plugin für Eclipse - September 2013 Preview veröffentlicht. Dieses Update enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit August 2013 Vorschau:

* **Möglichkeit, das Paket Azul Zulu OpenJDK auf Azure bereitgestellt.** Eine neue Option wurde hinzugefügt, wenn Sie Ihre Azure-Bereitstellung mit JDK angeben. Mit dieser Option können Sie ein Drittanbieter JDK-Paket direkt in Azure Cloud ohne eigene hochladen bereitstellen. Azul Systeme ist die erste, z. B. Paket namens Zulu basierend auf OpenJDK, die jetzt diese Option bereitgestellt werden kann.
* **Aktualisieren des Pakets für die Azure-Bibliotheken für Java-Bibliothek.** Diese Version 0.4.5 [Microsoft Azure-Client-API]basiert.

### <a name="august-1-2013"></a>1. August 2013

Azure-Plugin für Eclipse - August 2013 Preview veröffentlicht. Dies ist ein Update begleitenden Release von Azure SDK 2.1, Voraussetzung und wird automatisch heruntergeladen, wenn Sie das Plug-in installieren. Dieses Update enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit Juli 2013 Vorschau:

* **Entfernen der Optionen, um die lokalen JDK und lokalen Anwendungsserver des Bereitstellungspakets enthalten.** JDK herunterladen und Anwendungsserver aus Cloud-Speicher während der Bereitstellung ist vorzuziehen, diese Komponenten im Paket seit Ergebnissen Elemente kleiner Bereitstellung Paket Bereitstellung schneller und einfacher herunterladen einbetten. Infolgedessen JDK enthalten Optionen und Anwendungsserver im Bereitstellungspaket entfernt wurden. Projekte, die konfiguriert wurden, um die lokalen JDK und lokalen Server als Teil des Bereitstellungspakets automatisch konvertiert, Auto-JDK hochladen und Anwendungsserver, Cloud-Speicher.
* **Support für die Version 2.1 von Azure SDK.** Azure-Plugin für Eclipse - August 2013 Preview erfordert Azure SDK 2.1. Verwenden Sie August 2013 Preview nicht mit früheren Versionen von Azure SDK und nicht mit Azure SDK 2.1 Vorgängerversionen von Azure-Plugin für Eclipse.
* **Unterstützung für Eclipse Kepler Version.** In diesem Zusammenhang erforderlichen neue Minimum Eclipse-IDE Version Indigo. Azure-Plugin für Eclipse wird nicht offiziell auf Helios getestet.

### <a name="july-3-2013"></a>3 Juli 2013

Azure-Plugin für Eclipse - Juli 2013 Preview veröffentlicht. Dieses Update enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit Mai 2013 Vorschau:

* **Möglichkeit, ein neues Speicherkonto erstellen.** **Speicherkonto hinzufügen** Dialogfeld wurde eine **neue** Schaltfläche hinzugefügt. So können Sie ein Speicherkonto in Eclipse Plugin erstellen ohne Azure-Verwaltungsportal anmelden. (Sie Azure-Abonnement für diese Funktion müssen.) Weitere Informationen zum Erstellen eines neuen Speicherkontos finden Sie unter [Erstellen eines neuen Speicherkontos].
* **Neue &quot;(Auto)&quot; Option Speicherkonto für die automatische Bereitstellung von JDK und Server und für das Zwischenspeichern verwendet.** Bei Verwendung der Option **automatisch laden** für JDK und Anwendungsserver jetzt angeben **(Auto)** für die URL und Speicher mit JDK übertragen und Anwendungsserver oder Azure Caching verwenden. Dann verwendet diese Funktionen automatisch dieselbe Speicherkonto als, die Sie im Dialogfeld **Veröffentlichen Azure** auswählen. [Erstellen einer Hello World-Anwendung für Azure in Eclipse] Tutorial aktualisiert die neue Option **(Auto)** verwenden.
* **Fähigkeit, Ihre Azure-Endpunkte.** Geben Sie die Dienstendpunkte, die bestimmen, ob Ihre Anwendung bereitgestellt und die globale Azure-Plattform verwaltet Azure 21Vianet in China oder Private Azure-Plattform betrieben. Weitere Informationen finden Sie unter [Azure-Endpunkte].
* **Große Installationen können lokale Speicherressourcen.** Die Bereitstellung zu groß, um in den Standardordner Approot enthalten ist, können lokale Speicherressourcen jetzt als Bereitstellungsziel für Ihre JDK angeben und Anwendungsserver. Weitere Informationen finden Sie in [Großen Bereitstellungen bereitstellen].
* **Unterstützung für Größe A6 und A7 Azure Virtual Machine.** Sie können nun einen Cloud-Dienst Speicher A6 und A7 Virtual Machine Größen bereitstellen. Weitere Informationen zu diesen Größen finden Sie unter [virtuelle Computer und Azure Cloud Service Größe].
* **Aktualisieren des Pakets für die Azure-Bibliotheken für Java-Bibliothek.** Diese Version 0.4.4 [Microsoft Azure-Client-API]basiert.

### <a name="may-1-2013"></a>1. Mai 2013

Azure-Plugin für Eclipse - möglicherweise 2013 Preview veröffentlicht. Dies ist ein wesentliches Update begleitenden Release von Azure SDK 2.0, Voraussetzung und wird automatisch heruntergeladen, wenn Sie das Plug-in installieren. Diese Version enthält neue Features, Updates und Nutzbarkeit Feedback gesteuert wurde seit Februar 2013 Vorschau:

* **Automatische Hochladen JDK, Anwendungsserver und Bereitstellung von Azure-Speicher.** Eine neue Option, die automatisch lädt ausgewählte JDK und Anwendungsserver gegebenenfalls angegebenen Azure Storage-Konto und stellt diese Komponenten dort in das Bereitstellungspaket einbetten oder mit Upload Benutzer dann manuell. Diese häufig angeforderte Funktion kann einfach die JDK und Server-Komponenten, insbesondere für Anfänger verbessern. Eine exemplarische Vorgehensweise, die diese Optionen verwendet, finden Sie unter [Erstellen einer Hello World-Anwendung für Azure in Eclipse].
* **Zentrale Speicherkonto verfolgen und Möglichkeit, Speicher Referenzkunden mehr einfach (über ein Dropdownsteuerelement).** Dies gilt für mehrere Features, die auf Speicher JDK Bereitstellung von Komponenten und Zwischenspeichern. Weitere Informationen finden Sie in der [Liste Azure Storage].
* **Vereinfachtes Setup RAS Cloud-Assistenten veröffentlichen.** Müssen Sie lediglich Geben Sie einen Benutzernamen und ein Kennwort, Remotezugriff oder RAS deaktiviert bleiben leer.
* **Aktualisieren des Pakets für die Azure-Bibliotheken für Java-Bibliothek.** Diese Version 0.4.2 [Microsoft Azure-Client-API]basiert.
* **Unterstützung für Kurznotizen Sitzung mit Windows Server 2012.** Arbeitete sticky Sessions auf Windows Server 2008 R2 jetzt beide-Betriebssystem Ziele Support-sitzungsaffinität Cloud.
* **Paket-Upload Leistungssteigerungen.** Auch wenn das JDK und Anwendungsserver in das Bereitstellungspaket eingebettet sind, Upload-Teil des Bereitstellungsprozesses etwa doppelt so schnell im Vergleich zu früheren Versionen.

### <a name="february-8-2013"></a>8. Februar 2013

Azure-Plugin für Eclipse - Februar 2013 Preview veröffentlicht. Dies ist ein kleineres Update mit Bugfixes, Feedback-gesteuerte einfachen Verwendbarkeit verbessert und einige neue Features seit November 2012 Vorschau:

* Unterstützung für die Bereitstellung von JDKs, Anwendungsserver und andere Komponenten von öffentlichen oder privaten Azure BLOB-Speicher Downloads, statt sie im Bereitstellungspaket bei der Bereitstellung in der Cloud.
* Möglichkeit zum Ändern der Reihenfolge, in der benutzerdefinierte Komponenten einer Rolle durch Hinzufügen von Schaltflächen **Nach oben** und **Nach unten** im Abschnitt " **Komponenten** " **Azure Eigenschaften**verarbeitet werden.
* Ein Update **-Paket für Azure Bibliotheken für Java** -Bibliothek, basierend auf Version 0.4.0 [Microsoft Azure-Client-API].

### <a name="november-5-2012"></a>5. November 2012

Azure-Plugin für Eclipse - November 2012 Vorschau zur Verfügung. Dies ist ein wesentliches Update umfasst eine Reihe neuer Features sowie zusätzliche Fehlerbehebungen und verbesserte Nutzbarkeit Feedback gesteuert seit September 2012 Vorschau:

* Unterstützung für Microsoft Windows Server 2012 als Cloud-Betriebssystem.
* Unterstützung für Azure zusammengestellten caching Unterstützung für Memcached Kunden.
* Aufnahme von Apache Qpid JMS-Clientbibliotheken für Azure AMQP-basierten messaging nutzen.
* Eine verbesserte Assistenten **Neues Projekt** eine neue Seite am Ende das bietet die Möglichkeit, mehrere allgemeine wichtige Funktionen in ihrem Projekt schnell aktivieren: sticky Sessions, Cache und Remotedebuggen.
* Automatische Reduzierung der Rolleninstanzen 1 im Serveremulator Serverinstanzen Port-Bindungskonflikte zu vermeiden.

### <a name="september-28-2012"></a>28 September 2012

Azure-Plugin für Eclipse - September 2012 Vorschau zur Verfügung. Dieses Update Service umfasst eine Reihe von zusätzlichen Fehlerbehebungen seit August 2012 Vorschau sowie einige Feedback gesteuert einfachen Verwendbarkeit verbessert vorhandene Funktionen:

* Unterstützung für Microsoft Windows 8 und Microsoft Windows Server 2012 als Entwicklung Probleme, die zuvor das Plugin ordnungsgemäß auf diesen Betriebssystemen verhindert.
* Verbesserte Unterstützung für Portbereiche Endpunkt angeben.
* Fehlerkorrekturen Bezug auf Pfade, die Leerzeichen enthalten.
* Rolle Kontext Menü Verbesserungen für schnelleren Zugriff auf rollenspezifische Konfigurationen.
* Kleinere Weiterentwicklungen im Assistenten **in der cloud veröffentlichen** und eine Anzahl zusätzlicher Fehlerbehebungen.

### <a name="august-28-2012"></a>28. August 2012

Azure-Plugin für Eclipse - August 2012 Vorschau zur Verfügung. Dieses Update Service enthält zusätzliche Fehlerkorrekturen seit Juli 2012 Vorschau sowie mehrere Feedback gesteuert einfachen Verwendbarkeit verbessert vorhandene Funktionen:

* Im Dialogfeld Azure Access Control Services Filter:
    * **Das Signaturzertifikat Einbettungsoption** WAR-Datei der Anwendung, Cloudbereitstellung vereinfachen.
    * **Option zum Erstellen eines selbstsignierten Zertifikats** ACS Filterbenutzeroberfläche. Weitere Informationen über Azure Access Control Services Filter finden Sie unter [Authentifizieren von Webbenutzern mit Azure Access Control Service verwenden Eclipse].
* Azure Weitergabeprojekt-Assistenten (gilt auch für die Rolle Serverkonfiguration Eigenschaftenseite):
    * **Automatische Erkennung von JDK-Speicherort** auf Ihrem Computer (Sie überschreiben können, falls gewünscht).
    * **Automatische Erkennung des Servertyps** beim Auswählen des Application Server-Installationsverzeichnis.

### <a name="july-15-2012"></a>Am 15. Juli 2012

Azure-Plugin für Eclipse - Juli 2012 Vorschau, welche Adressen Anzahl der höchsten Priorität Fehler gefunden oder von Benutzern gemeldet nach Version vom Juni 2012 veröffentlicht. Dies ist nur ein Update, keine neuen Features sind enthalten.

### <a name="june-7-2012"></a>7. Juni 2012

Azure-Plugin für Eclipse - Juni 2012 CTP-Version wurde veröffentlicht. Neue Funktionen:

* **Neue Azure Weitergabeprojekt-Assistenten:** Auswahl der JDK Java-Anwendungsserver und Java-Programme auf die verbesserte Assistenten-Benutzeroberfläche. In der Liste der vordefinierten Konfigurationen zur Auswahl enthalten sind Tomcat 6, Tomcat 7, GlassFish OSE 3, Steg 7, 8 Steg, JBoss 6 und JBoss 7 (eigenständig). Darüber hinaus können Sie die Liste der Server-Konfigurationen anpassen. Diese Verbesserung der Benutzeroberfläche ist eine Alternative ziehen und Ablegen von komprimierten Dateien Startskripts kopieren, die vorher die wichtigsten Ansatz. Diese Methode funktioniert, noch wahrscheinlich nur für erweiterte Szenarien verwendet.
* **Serverkonfiguration Rolle Eigenschaftenseite:** Können Sie problemlos JDKs Java-Anwendungsserver und Anwendung der Bereitstellung zugeordnet, nachdem das Projekt erstellt haben. Weitere Informationen finden Sie unter [Server-Konfigurationseigenschaften].
* **&quot;Cloud veröffentlichen&quot; Assistent:** bietet eine einfache Möglichkeit, Ihr Projekt direkt von Eclipse automatisieren die manuelle hohe-Aufhebung Abrufen von Anmeldeinformationen anmelden bei der Azure-Verwaltungsportal hochladen Paket usw. in Azure bereitstellen. Ein Beispiel direkt bereitzustellende Projekt in Azure finden Sie unter [Erstellen einer Hello World-Anwendung für Azure in Eclipse].
* **Azure Symbolleiste:** Eine Azure Symbolleiste steht in Eclipse enthält Schaltflächen, die Folgendes aufrufen:
    * ![][ic710879]**Azure-Emulator ausführen**: Projekt im Emulator ausgeführt.
    * ![][ic710880]**Azure-Emulator zurücksetzen**: setzt den Emulator.
    * ![][ic710881]**Azure Cloud Paket erstellen**: das Paket für die Bereitstellung kompiliert.
    * ![][ic710876]**Neues Bereitstellungsprojekt Azure**: erstellt ein neues Projekt Azure-Bereitstellung.
    * ![][ic710882]**In Azure Cloud veröffentlichen**: veröffentlicht das Projekt in Azure.
    * ![][ic710883]**Veröffentlichung**: Löscht die Bereitstellung.
    * Viele dieser Azure Symbolleistenschaltflächen werden Erstellung [Einer Hello World-Anwendung für Azure in Eclipse]verwendet.
* **Für Java Azure Bibliotheken:** Verfügbar als Teil des einzelnen Pakets Azure Bibliotheken für Java-Bibliothek in Eclipse Plugin-Installation begleiten und enthält alle erforderlichen Abhängigkeiten. Nur einen Verweis auf die Bibliothek in das Java-Projekt hinzufügen und Sie brauchen separat herunterladen. Weitere Informationen finden Sie in der [Azure-Toolkit für Eclipse installieren].
* **Microsoft JDBC-Treiber für SQL Server während der Plugin-Installation 4.0:** Während der Installation von neuen Plug-in kann die neueste Version der Microsoft JDBC-Treiber für SQL Server installiert werden.
* **Azure Access Control Service Filter während der Plugin-Installation:** Diese neue Komponente als ein Eclipse-Bibliothek im Toolkit ermöglicht der Anwendung Java Web nahtlos mit verschiedenen Identitätsanbieter wie Google, Live.com und Yahoo! Azure Access Control Service (ACS) Authentifizierung nutzen. Sie müssen selbst Authentifizierungslogik schreiben, einige Optionen konfigurieren und den Filter die Schwerarbeit der Anwender mit ACS anmelden können. Sie können nur auf den Code den Benutzern Zugriff auf Ressourcen entsprechend ihrer Identität der Anwendung des Filters in das Anforderungsobjekt zurückgegeben konzentrieren. Ein Lernprogramm zur Verwendung des ACS-Filters finden Sie unter [Authentifizieren von Webbenutzern mit Azure Access Control Service verwenden Eclipse].
* **Automatische Erkennung von Azure SDK 1.7 Voraussetzung:** Beim Erstellen einer neuen Azure-Bereitstellungsprojekt wird Azure SDK 1.7 automatisch heruntergeladen, wenn nicht installiert.
* **Instanz Endpunkte:** Zugriff direkten Anschluss Endpunkt mit Lastenausgleich Instanzen. Instanz Endpunkte können über die Benutzeroberfläche, über die Seite [Endpunkte] Endpunkte hinzugefügt werden. Dadurch Remotedebuggen und JMX Diagnose für bestimmte Instanzen in der Cloud in Szenarien mit mehreren compute-Instanz Bereitstellungen. 
* **Komponenten UI:** Erleichtert die fortgeschrittene projektabhängigkeiten zwischen Azure Rollen im Projekt und andere externen Ressourcen wie Java-Anwendungsprojekte einrichten; Außerdem vereinfacht Bereitstellungslogik beschreiben. Weitere Informationen finden Sie unter [Komponenteneigenschaften].
* **Automatische Aktualisierung früherer Versionen von Projekten:** Wenn Sie einen Arbeitsbereich, Azure Projekt mit einer früheren Version des Plugins öffnen, alte Projekte in Eclipse erscheint als geschlossen, da frühere Versionen von Projekten nicht mit der neuen Version kompatibel sind. Wenn Sie versuchen, eine alte Projekte öffnen, wird eine Update-Assistent gestartet. Stimmen Sie die Aktualisierung, wird ein neues Projekt mit **_Upgraded** an den Namen angehängt erstellt und automatisch mit der neuen Version aktualisiert. Sie können das neue Projekt nach Bedarf umbenennen. Als Teil der Aktualisierung Ihre nicht geändert (und bleibt geschlossen).

### <a name="december-10-2011"></a>10. Dezember 2011

Azure-Plugin für Eclipse - Dezember 2011 CTP-Version wurde veröffentlicht. Neue Funktionen:

* **Sitzungsaffinität (&quot;sticky Sessions&quot;) unterstützt:** Statusbehaftete können helfen, gruppiert Java-Programme mit nur einem Kontrollkästchen. Weitere Informationen finden Sie unter [Sitzungsaffinität].
* **Start Skriptbeispiele vorgefertigte:** Für die beliebtesten Java Server (Tomcat Steg, JBoss, GlassFish), die Sie nur aus dem Projekt Beispielverzeichnis in Ihr Startskript kopieren können.
* **Emulator starten Ausgabe in Echtzeit:** Sie können jetzt ansehen Ausführung alle aus dem Startskript in einem dedizierten Konsolenfenster zeigt den Fortschritt und den Fehler im Skript während der Ausführung von Azure.
* **Automatische, leicht java.exe Überwachung:** Wenn java.exe beendet, mit einem einfachen, vorgefertigte Skript automatisch in die Bereitstellung eingeschlossen wird eine Rolle Wiederverwendung Kraft.
* **Remote Java-Anwendung Debuggen Konfiguration UI:** Können Sie problemlos Aktivieren des Eclipse Remotedebugger auf Ihre Java-Anwendung in den Emulator oder Azure Cloud durchgehen und Debuggen Sie den Java-Code in Echtzeit ausgeführt. Weitere Informationen finden Sie unter [Debuggen von Azure Applications in Eclipse].
* **Lokale Ressource Speicherkonfiguration UI:** So haben Sie mehr lokale Ressourcen konfigurieren, indem Sie die XML direkt bearbeiten. Dieses Feature ermöglicht Zugriff auf effektive Dateipfad der lokalen Ressource über eine Umgebungsvariable bereitgestellt wird, direkt von Ihrem Startskript verwiesen. Weitere Informationen finden Sie unter [Eigenschaften von lokalen Speicher].
* **Variable Umgebungskonfiguration UI:** So haben Sie mehr Umgebungsvariablen über manuelle Bearbeitung der XML-Konfigurationsdatei. Weitere Informationen finden Sie unter [Variablen Eigenschaften].
* **JDBC-Treiber für SQL Azure:** Über das Plug-in als nahtlos integrierte Eclipse Bibliothek aktivieren einfacher Programmieren von SQL Azure installiert. 
* **Schnelle Kontextmenü auf Konfiguration UI**: nur mit der rechten Maustaste auf den Ordner Rollen, und klicken Sie auf **Eigenschaften**.
* **Benutzerdefinierte Azure-Projekt und Rolle Ordnersymbole:** Für eine bessere Transparenz und einfachere Navigation im Arbeitsbereich und Projekt.

## <a name="see-also"></a>Siehe auch ##

Weitere Informationen zu Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für Eclipse]
  - [Azure Toolkit installieren für Eclipse]
  - [Erstellen Sie Hello World Web App für Azure in Eclipse]
  - *Neuigkeiten in Azure Toolkit für Eclipse (dieser Artikel)*
- [Azure-Toolkit für IntelliJ]
  - [Installieren von Azure Toolkit für IntelliJ]
  - [Erstellen Sie eine Hello World Web App für Azure in IntelliJ]
  - [Neuigkeiten in Azure Toolkit für IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen Sie Hello World Web App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen Sie eine Hello World Web App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Azure Toolkit installieren für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren von Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuigkeiten in Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[Azul Systeme Webseite Zulu OpenJDK]: http://go.microsoft.com/fwlink/?LinkId=402457
[Azure-Endpunkte]: http://go.microsoft.com/fwlink/?LinkID=699526
[Liste Azure-Speicher]: http://go.microsoft.com/fwlink/?LinkID=699528
[Komponenteneigenschaften]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debuggen von Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Große Bereitstellung bereitstellen]: http://go.microsoft.com/fwlink/?LinkID=699536
[Endpunkte-Eigenschaften]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[Variablen Eigenschaften]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[HDInsight Tools Plugin für Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[Webbenutzer mit Azure Access Control Service mit Eclipse Authentifizierung]: http://go.microsoft.com/fwlink/?LinkID=264703
[Wie SSL-Abladung verwenden]: http://go.microsoft.com/fwlink/?LinkID=699545
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Eigenschaften lokaler Speicher]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Microsoft Azure-Client-API]: http://go.microsoft.com/fwlink/?LinkId=280397
[Eigenschaften von Server-Konfiguration]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Sitzungsaffinität]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-Abladung]: http://go.microsoft.com/fwlink/?LinkID=699549
[Neue Speicher-Konto erstellen]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[Virtueller Computer und Azure Cloud Service Größe]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png

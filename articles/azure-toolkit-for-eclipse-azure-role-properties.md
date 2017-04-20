<properties
    pageTitle="Eigenschaften der Azure-Rolle"
    description="Erfahren Sie, wie mithilfe der Azure-Toolkit für Eclipse Azure-Rolle konfigurieren."
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Eigenschaften der Azure-Rolle #

Verschiedene Konfigurationen für Ihre Azure-Rolle können in Azure Toolkit für Eclipse festgelegt werden.

## <a name="configuring-azure-role-properties"></a>Konfigurieren der Eigenschaften der Azure-Rolle ##

Konfigurieren der Eigenschaften der Azure erfolgt über die Eigenschaft Dialogfelder für die Worker-Rolle. Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und wählen Sie im Untermenü " **Azure** ". (Wenn bei Eclipse Projekt-Explorer nicht angezeigt wird, erweitern Sie Ihre Azure-Projekt im Projekt-Explorer.)

![][ic789599]

Eigenschaften, die in den Dialogfeldern **Eigenschaften** festgelegt werden können, werden in diesem Thema beschrieben. Beachten Sie, dass viele Eigenschaften automatisch ausgefüllt werden, wenn ein neues Azure Weitergabeprojekt erstellen.

Folgende Eigenschaftenseiten stehen für Azure-Rollen.

* [Eigenschaften für virtuelle Computer](#virtual_machine_properties)
* [Zwischenspeichern von Eigenschaften](#caching_properties)
* [Zertifikate-Eigenschaften](#certificates_properties)
* [Komponenteneigenschaften](#components_properties)
* [Debuggen](#debugging_properties)
* [Endpunkte-Eigenschaften](#endpoints_properties)
* [Variablen Eigenschaften](#environment_variables_properties)
* [Lastenausgleich / Sitzungseigenschaften Affinität (AKA "sticky Sessions")](#session_affinity_properties)
* [Eigenschaften lokaler Speicher](#local_storage_properties)
* [Eigenschaften von Server-Konfiguration](#server_configuration_properties)
* [SSL-Abladung Eigenschaften](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>Eigenschaften für virtuelle Computer ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse **Azure**auf und klicken Sie dann auf **Eigenschaften**, und Sie haben die Möglichkeit zum Ändern der Größe des virtuellen Computers und auch ändern der Anzahl der Instanzen, wie in der folgenden Abbildung dargestellt.

![][ic719499]

>[AZURE.NOTE] Nur Windows: Wenn Sie die Anzahl der Instanzen auf einen Wert größer 1 festgelegt und auch Anwendungsserver, ermöglichen das Toolkit 1 Rolleninstanz ausführen im Emulator unabhängig von dieser Einstellung. Dies ist zu Port Bindungskonflikte zwischen verschiedenen Serverinstanzen (z. B. alle an Anschluss 8080 Binden) Wenn sie auf demselben Computer ausgeführt werden. Festlegen der gewünschten Instanzenzahl erhalten, aber es wirksam erst in der Cloud bereitstellen.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>Zwischenspeichern von Eigenschaften ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse, **Azure**auf und klicken Sie dann auf **Zwischenspeichern**. In diesem Dialogfeld können benannten zusammengestellten Memcache-kompatible Caches ermöglicht einer Anwendung beschleunigen.

![][ic719483]

In der Eigenschaftenseite **Zwischenspeichern** können Sie globale Einstellungen für Folgendes angeben:

* ob am selben Standort das Zwischenspeichern aktiviert ist.
* die Cachegröße als Prozentsatz des Arbeitsspeichers.
* der Speicher-Kontoname für das Speichern der Zwischenspeicherstatus beim Ausführen der Anwendung als Clouddienst oder none, wenn kein Cache speichern möchten. (Speicherkontoname wird nicht verwendet, beim Ausführen der Anwendung im Serveremulator.) Wenn Sie speicherkontoname **(** Auto) (Standard) festgelegt, verwendet die Zwischenspeicherungskonfiguration gleichen Speicherkonto wie automatisch im Dialogfeld **Azure veröffentlichen** wählen.

>[AZURE.NOTE] Die Einstellung **(Auto)** haben den gewünschten Effekt nur, wenn Sie die Bereitstellung mit dem Eclipse Toolkit Veröffentlichen Assistenten veröffentlichen. Stattdessen die .cspkg-Datei manuell mithilfe eines externen Mechanismus wie [Azure-Verwaltungsportal][]veröffentlichen wird die Bereitstellung nicht ordnungsgemäß.

Das folgende Dialogfeld zeigt die Eigenschaften für einen Cache.

![][ic719501]

* **Name:** Der Name des Caches am selben Standort.
* **Port-Nummer:** Die Anschlussnummer für den Cache.
* **Ablaufrichtlinie:** Die folgenden Werte, die angibt, wenn ein Schlüssel im Cache abläuft.
    * **Absolute:** Der Schlüssel läuft ab, wenn die angegebenen **Minuten** Zeit erreicht wird.
    * **NeverExpires:** Der Schlüssel muss eine Ablaufzeit nicht.
    * **SlidingWindow:** Der Schlüssel läuft ab, wenn es nicht für den angegebenen **Minuten**Zeit zugegriffen wurde; jedes Mal darauf zugegriffen wird, wird die Ablauf Uhr zurückgesetzt.
* **Minuten:** Maximale Anzahl von Minuten eine Memcached Taste live unter der Ablaufrichtlinie.
* **Hohe Verfügbarkeit mit replizierten Backups auf verschiedene Instanzen:** Wenn aktiviert, bieten hilft auf verschiedenen Instanzen mit hoher Verfügbarkeit repliziert werden. Beachten Sie, dass mindestens zwei Instanzen für die Bereitstellung dieser Funktion gelten müssen.

Um einen neuen Cache hinzuzufügen, klicken Sie auf der Eigenschaftenseite **Zwischenspeichern** **Hinzufügen** und ein Dialogfeld **Mit dem Namen Cache konfigurieren** geöffnet. Geben Sie Werte für die Eigenschaften, die oben beschrieben wurden.

Ändern Sie einen benannten Cache aktivieren Sie Cache und klicken Sie **Bearbeiten** der Seite **Zwischenspeichern** . Ein Dialogfeld wird geöffnet ermöglicht die Cacheeigenschaften ändern. Drücken Sie **OK** , um die Cache-Werte speichern.

Cache löschen Wählen Sie Cache aus klicken Sie auf **Entfernen** auf der Eigenschaftenseite **Zwischenspeichern** und klicken Sie auf **Ja,** um den Löschvorgang zu bestätigen.

Weitere Informationen zum Zwischenspeichern von Informationen Sie [zum Zwischenspeichern der Taste verwenden][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Zertifikate-Eigenschaften ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse, **Azure**auf und klicken Sie auf **Zertifikate**.

![][ic710964]

In diesem Dialogfeld können Sie hinzufügen oder Entfernen von Zertifikate das Eclipse-Projekt. Beachten Sie, dass die hier aufgelisteten Zertifikate nicht automatisch in alle Java-Schlüsselspeicher gespeichert werden und sind daher nicht automatisch für die Verwendung von Java-Anwendung verfügbar. Sie werden nur bei Azure registriert, dass sie vorab können in Windows auf den virtuellen Computern geladen werden, die Bereitstellung zu speichern und später von anderen Windows-Software. Derzeit ist das einzige Feature des Toolkits, die Zertifikate so im Dialogfeld **Zertifikate** verwendet [SSL-Abladung][]aufgrund der Verwendung von Internet Information Services (IIS) und Anwendung anfordern Routing (ARR) erfordern das richtige Zertifikat auf diese Weise bereitgestellt werden.

Bei Bereitstellung des Projekts in Azure mithilfe des Veröffentlichen-Assistenten werden Sie aufgefordert, zeigen Sie auf Personal Informationsaustausches (PFX) die entsprechenden Dateien für diese Zertifikate und ihre Kennwörter um automatisch Azure Service, aber nur, wenn sie es zuvor wurden nicht hochgeladen hochladen.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Komponenteneigenschaften ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie dann auf **Komponenten**auf **Azure**. In diesem Dialogfeld können Sie hinzufügen, ändern oder entfernen Sie die Komponenten Ihrer Rolle sowie Ändern der Reihenfolge, in der sie verarbeitet werden.

![][ic719502]

Komponenten-Feature können Sie Azure Bereitstellungsprojekt, Java-Anwendungsprojekte spezielle Dateien und ausführbaren Befehlszeile-Anweisungen, die von der Bereitstellung benötigt Abhängigkeiten hinzufügen.

Für jede Komponente können Sie Folgendes angeben:

* Der Schritt bei Azure Bereitstellungsprojekt Komponente importieren, wenn es erstellt wird.
* Der Schritt beim Bereitstellen der Komponente in der Azure-Cloud.

>[AZURE.NOTE] Beim Angeben von Dateien oder Befehlszeilen Denken Sie daran, dass die Bereitstellung einer virtuellen Maschine Windows erscheint daher Ihre benutzerdefinierten Schritte für ein Windows-Betriebssystem müssen. 

Komponenten verfügen über folgenden Eigenschaften:

* **Import:** Methode, die angibt, wie die Komponente in das Projekt importiert werden, wenn das Projekt erstellt wird. Die folgenden Werte sind möglich:
    * **Kopieren:** Die Komponente wird aus den lokalen Pfad **der Eigenschaft** in der Rolle **Approot** Verzeichnis festgelegten kopiert.
    * **Ohr:** Die Komponente ist ein Java Enterprise-Archiv (EAR) von Enterprise-Anwendung durch **die Eigenschaft** angegebenen lokalen Pfad Projekt importiert. (Dies wird automatisch basierend auf der Art des Projekts an diesem Speicherort Toolkit erkannt).
    * **Glas:** Die Komponente ist ein Java-Archiv (JAR) und ein Java-Projekt durch **die Eigenschaft** angegebenen lokalen Pfad importiert. (Dies wird automatisch basierend auf der Art des Projekts an diesem Speicherort Toolkit erkannt).
    * **keine:** Keine Aktion wird die Komponente importieren. Dies gilt bei die Komponente angenommen wird, bereits in der Rolle **Approot** Verzeichnis oder nur einer ausführbaren Befehlszeile Anweisung in der **als** -Eigenschaft wird **die Methode** **Exec**ist.
    * **WAR:** Die Komponente Anwendung Java Webarchiv (WAR) und von einem dynamischen Webprojekt durch **die Eigenschaft** angegebenen lokalen Pfad importiert. (Dies wird automatisch basierend auf der Art des Projekts an diesem Speicherort Toolkit erkannt).
    * **PLZ:** Die Komponente ist eine Zip-Datei und durch Komprimieren das Verzeichnis oder durch **die Eigenschaft** angegebenen Datei importiert.
* **Von:** Pfad auf dem lokalen Computer in den Ordner oder die Datei, zu der Bereitstellung importiert. Windows-Umgebungsvariablen können in dieser Eigenschaft verwendet werden. Importiert alle werden beim Erstellen des Projekts in der Rolle **Approot** Verzeichnis importiert.
    
    Beachten Sie, dass Sie die Möglichkeit, eine Komponente aus einem Download bereitstellen, bei der Bereitstellung in der Cloud (nicht Serveremulator). Anzeigen Sie verwandte Informationen über eine Komponente    
    
* **As:** Dateiname, unter dem die Komponente in der Rolle **Approot** Verzeichnis importiert und schließlich in der Azure-Cloud bereitgestellt werden. Lassen Sie diese Eigenschaft leer, der Name beibehalten auf dem lokalen Computer. (Für ausführbare Komponenten, die **deren Methode** **Exec**festgelegt ist dies eine beliebige Windows-Befehlszeile Anweisung kann.)

    >[AZURE.IMPORTANT] Wenn Sie Leerzeichen für diesen Wert verwenden, werden sie je nach Bereitstellung anders behandelt. Ist die Methode **Ausführen**, werden Leerzeichen als Argumenttrennzeichen Befehlszeile und nicht als Bestandteil des Dateinamens interpretiert. Für alle anderen Methoden bereitstellen, Leerzeichen werden als Teil des Dateinamens interpretiert.
    
* **Bereitstellung:** Methode, die beim Start der Bereitstellung auf die Komponente angewendete Aktion angibt. Die folgenden Werte sind möglich:
    * **Kopieren:** Die Komponente wird in der **To** -Eigenschaft angegeben Zielpfad kopiert.
    * **Exec:** Die Komponente ist eine ausführbare Windows Befehlszeile Anweisung im Kontext des Pfads festgelegten Eigenschaft **,** zum Zeitpunkt die Bereitstellung wird ausgeführt.
    * **keine:** Keine Aktion wird zu Beginn die Bereitstellung der Komponente angewendet.
    * **PLZ:** Die Komponente wird extrahiert die **To** -Eigenschaft angegeben Zielpfad. Diese Methode steht nur beim **Importvorgang** **Zip**wohnen.
* **To:** Zielpfad auf dem virtuellen Computer, die Komponente bereitgestellt wird. Windows-Umgebungsvariablen können in dieser Eigenschaft verwendet und Dateipfade sind relativ **Approot**.
    
Um eine neue Komponente hinzuzufügen, klicken Sie auf der Eigenschaftenseite **Komponenten** **Hinzufügen** und eine **Komponente der Azure-Rolle** -Dialogfeld geöffnet. Geben Sie Werte für die Eigenschaften, die oben beschrieben wurden. 

Im folgenden wird ein Beispiel für das Hinzufügen einer neuen Komponente WAR.

![][ic719503]

Bei der Bereitstellung von Cloud (nicht Serveremulator), wenn Sie die Komponente herunterladen bereitstellen möchten, sicher, dass **im Cloud statt im Paket Bereitstellen von** aktiviert ist. Möchten Sie Ihre Azure-Speicherkonto auswählen das Speicherkonto in **Speicherkonto** Dropdown-Liste (Sie können Klicken die **Konten** ändern, was in der Liste ist) die teilweise im **URL** -Feld füllen und füllen Sie die verbleibenden Teil der URL herunterladen. Nicht Azure-Speicher verwenden möchten, **Wählen Sie aus der Dropdownliste **Speicherkonto** ** und geben Sie die URL im Feld **URL** der Komponente. Geben Sie eine der folgenden Methoden:

* **Kopieren:** Download-Komponente wird durch den Pfad **Zum Verzeichnis** angegeben Zielpfad kopiert.
* **gleichen:** Dieselbe Methode zum **Bereitstellen von Downloads** für **Paket bereitstellen**.
* **PLZ:** Download-Komponente wird extrahiert **,** Verzeichnispfad angegeben Zielpfad.

Um eine Komponente zu ändern, wählen Sie die Komponente, und klicken Sie auf die Schaltfläche **Bearbeiten** in der Eigenschaftenseite **Komponenten** . Ein Dialogfeld wird geöffnet, können Sie die Eigenschaften ändern. Drücken Sie **OK** , um den Werten zu speichern.

Um eine Komponente zu löschen, wählen Sie die Komponente und klicken Sie auf die Schaltfläche **Entfernen** in der Eigenschaftenseite **Komponenten** und klicken Sie auf **Ja,** um den Löschvorgang zu bestätigen.

Komponenten werden in der aufgeführten Reihenfolge verarbeitet. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten** die Reihenfolge anordnen.

>[AZURE.NOTE] Die Konfigurationsfunktion Server basiert auf Komponenten. Diese Komponenten werden nicht entfernt oder ohne die entsprechende Serverkonfiguration bearbeitet. Sie werden dazu aufgefordert beim Komponenten ändern.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Debuggen ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie dann auf **Debuggen**auf **Azure**. In diesem Dialogfeld Sie können aktivieren oder Deaktivieren des Remotedebuggens sowie Debugkonfigurationen erstellen, wie in der folgenden Abbildung dargestellt.

![][ic719504]

Informationen zum Debuggen finden Sie unter [Debuggen von Azure Applications in Eclipse][].

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Endpunkte-Eigenschaften ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken **Endpunkte**auf **Azure**. In diesem Dialogfeld können Sie eine erstellen und bearbeiten oder entfernen einen Endpunkt wie in der folgenden Abbildung dargestellt.

![][ic719505]

Um einen Endpunkt hinzufügen, klicken Sie auf der Eigenschaftenseite **Endpunkte** **Hinzufügen** und einen **Endpunkt hinzufügen** Dialogfeld geöffnet.

![][ic710897]

Geben Sie einen Namen für den Endpunkt wählen Sie aus ( **Eingabe**, **interne**oder **InstanceInput**) und geben Sie den öffentlichen und privaten Port. Drücken Sie **OK** , um die neuen Endpunkt Werte speichern.

Je nach Endpunkt können Sie Portbereiche folgendermaßen verwenden:

* Für einen Endpunkt Eingabeinstanz kann öffentliche Port einen Portbereich (z. B. **2000 2010**) und private Port ist ein fester Wert.
* Interne Endpunkt öffentliche Port nicht verwendet wird, leer oder auf ein Sternchen zeigt automatisch von Azure festgelegt und private Port kann einen Bereich.
* Für Eingabe Endpunkte öffentliche Port kann nur einen festen Wert und private Port kann einen festen Wert oder leer oder ein Sternchen festgelegt, um anzugeben, dass es automatisch von Azure festgelegt ist.

Wenn Sie eine einzelne Portnummer statt einer verwenden möchten, lassen Sie das Textfeld für das Ende des Bereichs leer.

Für Ports auf automatisch gesetzt, wenn Sie werden bestimmen, welcher Anschluss tatsächlich während der Laufzeit verwendet wird, können die Anwendung die Azure Laufzeit-API, [com.microsoft.windowsazure.serviceruntime Paketübersicht][]dokumentiert.

Wie Instanz input Endpunkte unterstützen Sie beim Debuggen einer Bereitstellung mit mehreren Instanzen verwendet werden können, finden Sie unter [Debuggen spezifische Rolleninstanz in einer Bereitstellung mit mehreren Instanzen][].

Um einen Endpunkt zu ändern, wählen Sie den Endpunkt und klicken Sie auf die Schaltfläche **Bearbeiten** der Seite **Endpunkte** . Ein Dialogfeld wird geöffnet ermöglicht Endpunktname, Typ und öffentliche und private Ports ändern. Drücken Sie **OK** die geänderte Endpunkt Werte gespeichert.

Um einen Endpunkt zu löschen, wählen Sie den Endpunkt klicken Sie **Entfernen** der Seite **Endpunkte** und klicken Sie auf **Ja,** um den Löschvorgang zu bestätigen.

Um einige Features (z. B. Caching Remotedebuggen, Sitzungsaffinität oder SSL-Verschiebung) aktiviert, indem der Benutzer eine Rolle konfigurieren, kann das Toolkit automatisch besonderen Endpunkte konfigurieren, die mit benutzerdefinierten Endpunkten aufgelistet werden. Das Toolkit verhindert, dass Benutzer bearbeiten oder Endpunkte löschen z. B. automatisch generiert werden, als die zugeordnete Funktion aktiviert ist.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Variablen Eigenschaften ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie dann auf **Umgebungsvariablen**auf **Azure**. In diesem Dialogfeld können Sie eine Umgebungsvariable erstellen ändern oder entfernen Sie eine Umgebungsvariable wie in der folgenden Abbildung dargestellt.

![][ic719506]

Umgebungsvariablen stehen die Startskripts bei gestartet wird.

>[AZURE.NOTE] Beim Festlegen von Umgebungsvariablen, beachten Sie, dass die Bereitstellung auf einem Windows-Computer veröffentlicht wird, müssen also die Umgebungsvariablen für ein Windows-basiertes Betriebssystem gültig.

Als Beispiel einer Umgebungsvariablen wird verfügbar, wenn die Rolle gestartet wird erstellen Sie eine neue Umgebungsvariable durch Klicken auf die Schaltfläche **Hinzufügen** . Im folgenden wird eine Umgebungsvariable mit dem Namen **MyRoleVersion** erstellt und der Wert **1,0**zugewiesen.

![][ic659268]

Innerhalb des Codes Jsp konnte anzeigen den Wert mithilfe der `System.getenv` Methode:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Was diese Ausgabe der Anwendung:

![][ic552233]

Um eine Umgebungsvariable zu ändern, wählen Sie die Umgebungsvariable und klicken Sie **Bearbeiten** Eigenschaftenseite **Umgebungsvariablen** . Ein Dialogfeld wird geöffnet können Sie die Variablen Eigenschaften ändern. Drücken Sie **OK** , um die Umgebung Werte speichern.

Um eine Umgebungsvariable zu löschen, wählen Sie die Umgebungsvariable klicken Sie **Entfernen** der **Umgebungsvariablen** Seite und klicken Sie auf **Ja,** um den Löschvorgang zu bestätigen.

Um einige Funktionen (z. B. Serverkonfiguration Remotedebuggen und lokalen Speicher) aktivierte Benutzer eine Rolle konfigurieren, kann das Toolkit automatisch spezielle Umgebungsvariablen konfigurieren, die mit benutzerdefinierten Umgebungsvariablen aufgelistet werden. Das Toolkit verhindert, dass Benutzer bearbeiten oder Umgebungsvariablen löschen z. B. automatisch generiert werden, als die zugeordnete Funktion aktiviert ist.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Lastenausgleich / Sitzungseigenschaften Affinität (AKA "sticky Sessions") ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie dann auf **Den Lastenausgleich**auf **Azure**. In diesem Dialogfeld können Sie aktivieren oder Deaktivieren der sitzungsaffinität, wie in der folgenden Abbildung dargestellt.

![][ic719492]

Weitere Informationen finden Sie unter [Sitzungsaffinität][]. Beachten Sie diese Funktion Verhalten im Kontext der SSL-Abladung wie [SSL-Abladung][]beschrieben.

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Eigenschaften lokaler Speicher ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie dann auf **Lokalem Speicher**auf **Azure**. In diesem Dialogfeld können Sie erstellen, ändern oder Entfernen von lokalen Zwischenspeicher für den virtuellen Computer, der die Anwendung ausgeführt wird. Bestimmte Werte können für die Größe des lokalen Speicher festgelegt werden und ob der Inhalt beibehalten werden, wenn die Rolle recycelt wird, wie in der folgenden Abbildung dargestellt.

![][ic719508]

Sie können optional auch eine Umgebungsvariable, die lokalen Speicher entspricht.

Standardmäßig was in Azure bereitstellen platziert (im Ordner **Approot** die Rolleninstanz und entpackt). Einfachste Bereitstellung werden zwar passt es auch nach dem Entpacken des Speicherplatzes für das Verzeichnis **Approot** beschränkt und klar definierte (weniger als 1 GB angemessenen Faustregel). Daher sollten sicherzustellen, Azure in größeren Systemen ausreichend Speicherplatz reserviert, die nicht im Ordner **Approot** passen Sie eine lokale Speicherressourcen mithilfe des Dialogfelds **Lokaler Speicher** einrichten. Eine einfache Möglichkeit hierzu finden Sie in [Großen Bereitstellungen bereitstellen][].

Sie können problemlos Speicherressourcen von Startskripts (z. B. die **startup.cmd**) über automatisch zugeordneten Eclipse Toolkit die Ressource im **Lokalen Speicher** -Dialogfeld angezeigten Umgebungsvariable verweisen. Diese Umgebungsvariable enthält den vollständigen Pfad der lokalen Ressource konfigurierten Zeitpunkt haben Ihre Startskript ausgeführt wird. 

Ändern für lokale Speicherressourcen wählen Sie lokale Speicherressourcen und klicken Sie **Bearbeiten** **Lokaler Speicher** -Eigenschaftenseite. Ein Dialogfeld wird damit Sie ändern die Eigenschaften lokaler Speicher geöffnet. Drücken Sie **OK** , um die Ressourcenwerte lokalen Speicher speichern.

Um eine lokale Speicherressourcen zu löschen, wählen Sie lokale Speicherressourcen klicken Sie auf **Entfernen** auf der Eigenschaftenseite des **Lokalen Speicher** und klicken Sie auf **Ja,** um den Löschvorgang zu bestätigen.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Eigenschaften von Server-Konfiguration ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie dann auf **Server-Konfiguration**auf **Azure**. In diesem Dialogfeld hinzufügen, entfernen und ändern den JDK und Java-Anwendungsserver von der Bereitstellung verwendeten können Sie sowie hinzufügen oder entfernen Applications (wie WAR, Glas oder Ohr) Bereitstellung verwendet.

### <a name="jdk-configuration"></a>JDK-Konfiguration ###

In diesem Dialogfeld können Sie das JDK-Paket für Ihre Bereitstellung angeben. Wenn Sie Eclipse unter Windows verwenden, können das JDK-Paket können lokal in Azure-Emulator ausgeführt und haben die Möglichkeit, die lokale Installation in Azure bereitstellen. Nicht-Windows-Betriebssystemen Emulator JDK-Einstellung ist nicht anwendbar und lokal installierte JDK kann nicht bereitgestellt werden, da es nicht mit Windows kompatibel. Jedoch unabhängig vom Betriebssystem, das Sie verwenden, können Sie immer unter der 3. JDK Pakete in Azure bereitstellen, oder Ihre eigenen Windows-kompatiblen JDK-Paket eine alternative Download aus.

Nachfolgend ein Beispiel, wie Sie unter Windows eine JDK angeben können:

![][ic780647]

Wenn Sie Eclipse unter Windows verwenden, können Sie eine JDK Serveremulator mit; dazu sicher, dass im Abschnitt **Bereitstellung Emulator** **verwenden JDK aus diesem Pfad für lokal testen** aktiviert ist. Geben Sie den lokalen Pfad der JDK; andere JDK finden Sie die gewünschten nicht automatisch ausgewählt. Sie können auch die JDK auf den Azure-Cloud-Dienst bereitstellen; Hierzu wählen Sie im Abschnitt **Bereitstellung Cloud** Bereitstellungsoption **Meine lokale JDK (Auto-Upload, Cloud-Speicher)** .

Hinweis: Auf nicht-Windows-Betriebssystemen sind **Emulator** Bereitstellungseigenschaften und **Bereitstellen meiner lokalen JDK** -Option nicht verfügbar. Das folgende Beispiel veranschaulicht eine JDK auf einem Mac oder anderen unterstützten Windows-Betriebssystem angeben:

![][ic789643]

Unabhängig vom Betriebssystem auf sind, müssen Sie die folgenden zwei **Cloud** Bereitstellungsoptionen für die Quelle und das JDK-Paket:

* **Ein 3rd Party JDK Paket auf Azure bereitgestellt** 
* **Bereitstellen von benutzerdefinierten herunterladen** 

Verwenden Sie die Option **3. JDK-Paket von Azure Partei bereitstellen** :

1. Aktivieren Sie das Kontrollkästchen mit dem Namen **Deploy 3. JDK-Paket von Azure Partei**.
1. Wählen Sie aus der Dropdown-Liste 3rd Party JDK Paket auf Azure.
1. Die **JDK** -Registerkarte sieht ähnlich dem folgenden Windows:  ![][ic780648] 
    und sieht etwa wie folgt unter Mac OS oder anderen unterstützten Windows-Betriebssysteme: ![][ic789643]
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.
1. Aufforderung den Lizenzvertrag von 3rd Party JDK-Provider des Pakets überprüfen Sie den Lizenzvertrag. Wenn Sie zustimmen, klicken Sie auf **Ja,** um das Dialogfeld **Lizenzvertrag akzeptieren** schließen.
    Beachten Sie, dass die zugrunde liegende Logik für die Elemente in der Dropdownliste die Option **Bereitstellen einer 3. JDK-Paket von Azure Partei** angezeigt angepasst werden kann. Klicken Sie auf **Anpassen** , um die Elemente im Dialogfeld **JDK** anzupassen. Dies wird die **JDK** -Eigenschaftenseite schließen und öffnen Sie die Datei **componentsets.xml** in Eclipse dann nach Bedarf ändern können. Dokumentation für die **componentsets.xml** ist in der Datei **componentsets.xml** enthalten.

Bei Verwendung der Bereitstellungsoption **JDK benutzerdefinierte Download** :

1. Erstellen Sie eine ZIP-Datei des Installationsverzeichnisses JDK Verzeichnisknoten selbst untergeordnete ZIP-Struktur und nicht der Inhalt ist. Notieren Sie den Namen des Verzeichnisses, wird es später, und beachten Sie die JDK-Installation auf einem Windows-Computer bereitgestellt werden.
1. Hochladen Sie ZIP in Ihr Konto Azure-Speicher als Blob. Sie können dazu eine extern verfügbaren Tools für Blobs Azure-Speicher hochgeladen. Es wird empfohlen, einen privaten Blob verwenden. Beachten Sie die BLOB-URL des Inhalts ZIP.
1. Aktivieren Sie das Kontrollkästchen mit dem Namen **Deploy JDK benutzerdefinierte herunterladen**.
    Möchten Sie Ihre Azure-Speicherkonto auswählen das Speicherkonto in **Speicherkonto** Dropdown-Liste (Sie können Klicken die **Konten** ändern, was in der Liste ist) die teilweise im **URL** -Feld füllen und füllen Sie die verbleibenden Teil der URL herunterladen. Wenn Sie keine Azure-Speicher verwenden möchten, **Wählen Sie aus der Dropdownliste **Speicherkonto** ** und geben Sie die URL im Feld **URL** zu der JDK. Verwenden der Azure-Speicher müssen BLOB-Namen in der URL klein sein.
1. Sicherstellen Sie, dass der richtige Verzeichnisname **JAVA_HOME** Textfeld bezieht. Standardmäßig verweist es JDK Verzeichnis gleichnamigen als Wert für die lokale Verwendung wählte. Jedoch hat das ZIP-Verzeichnis einen anderen Namen (z. B. durch eine andere Version), den Verzeichnisnamen im Textfeld **JAVA_HOME** entsprechend aktualisieren, da diese Einstellung in der Cloud (nicht im Serveremulator) verwendet werden.
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Das wars. Jetzt beim Cloud erstellen, sehen Sie die Paketgröße werden viel kleiner, der Buildprozess dauert normalerweise weniger Zeit und Bereitstellung selbst in der Cloud veröffentlichen sollten auch schneller. Beachten Sie, dass die Optionen **Bereitstellen meiner lokalen JDK (Auto-Upload, Cloud-Speicher)** oder **JDK benutzerdefinierte Download bereitstellen** gelten nur, wenn die Anwendung in der Cloud bereitgestellt wird. Sie haben keine Auswirkung auf die Compute-Emulator; die lokale Version der Komponenten wird weiterhin bei der Serveremulator Bereitstellung verwendet. 

### <a name="server-configuration"></a>Server-Konfiguration ###

Folgendes ist ein Beispiel, wie Sie Anwendungsserver angeben können.

![][ic796926]

Stellen Sie sicher, dass das Kontrollkästchen **Bereitstellen ein Server dieses Typs** ausgewählt ist, und wählen Sie dann den Anwendungsserver zu verwenden.

Für einen Server angeben, Cloud-Bereitstellung verwenden, können Sie die folgenden Optionen nutzen:

1. **Bereitstellen einer 3. Partei auf Azure Server** - Dies gilt insbesondere für Test-/Szenarien, Bereitstellungseffizienz und Einfachheit ist und der Server erfordert eine Konfiguration. Oder wenn Sie einen dieser Server als Ausgangspunkt verwenden aber aufnehmen Anpassung der entsprechenden Server in der Bereitstellung Startlogik.
1. **Bereitstellen von benutzerdefinierten Download** - Dies gilt insbesondere in Produktionsszenarien bei speziell vorbereitete und konfigurierten Server, die Sie in der Cloud verwenden möchten.
1. **Meine lokale Serverinstallation bereitstellen** - Dies gilt insbesondere für ist die lokale Server-Installation bereits benutzerdefinierte für Ihre Verwendung konfiguriert. Wenn Sie diese Option auswählen, müssen Sie dem lokalen Server Pfad im Textfeld **Lokaler Pfad** unten angeben.

Verwenden Sie die Option **3. auf Azure Server von Drittanbietern bereitstellen** :

1. Aktivieren Sie das Kontrollkästchen mit dem Namen **einer 3. Partei auf Azure Server bereitstellen**.
1. Wählen Sie im Dropdownmenü die gewünschten Serversoftware Bereitstellung in der Cloud. Beachten Sie: Wenn Sie bereits einen Server verwenden angegeben, werden Sie auf nur einen Cloudserver der gleichen Familie als dieser Servertyp auswählen. Wenn jedoch ein Server nicht ausgewählt haben, können von Servern, die auf Azure Servertyp automatisch für Sie ausgewählt.
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Wenn die Option **benutzerdefinierte Download bereitstellen** :

1. Stellen Sie sicher, dass Sie bereits ein Server nach den vorhergehenden Schritten ausgewählt haben. Veranlaßt dem Plug-in zum Server von Ihrem benutzerdefinierten Download bereitstellen muss aus der Familie der ausgewählten Typ haben.
1. Aktivieren Sie das Kontrollkästchen mit dem Namen **Deploy benutzerdefinierte herunterladen**.
    Wenn Sie Ihr Konto Azure-Speicher herunterladen möchten, herunterladen wählen das Speicherkonto in **Speicherkonto** Dropdown-Liste (Sie können Klicken die **Konten** ändern, was in der Liste ist) die teilweise im **URL** -Feld füllen und füllen Sie die verbleibenden Teil der URL zum Server ZIP (bei Azure-Speicher, BLOB-Namen in der URL mit Kleinbuchstaben) Wenn nicht Azure-Speicher verwenden möchten, **Wählen Sie aus der Dropdownliste **Speicherkonto** ** und geben Sie die URL zu Ihrem Server Download ZIP im **URL** -Feld. ZIP enthält Unterordner, der die Application Server-Installationsverzeichnis darstellt. Beispielsweise verwenden Sie eine ZIP-Datei für wäre Apache Tomcat 7.0.35, innerhalb der Zip untergeordneter Ordner Installationsverzeichnis **Apache Tomcat 7.0.35**darstellt. 
1. Geben Sie den Wert für die Umgebungsvariable home-Verzeichnis. Es wird standardmäßig auf den Wert für den lokalen Server ggf., aber Sie können einen anderen Wert angeben, wenn Ihre Cloud-Anwendungsserver vom lokalen Server unterscheidet. Sie müssen jedoch sicherstellen, dass Ihre Cloud-Anwendungsserver derselben Familie als Server ist zuvor ausgewählten Typ.
    Wenn Ihre Postleitzahl Cloud Application Server zukünftig aktualisieren, können Sie manuell Einstellung Basisverzeichnis ändern oder es erneut entsprechen Ihrem lokalen festlegen (Wenn Sie den lokalen Server zu geändert).
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Die zugrunde liegende Logik für die Elemente in der Registerkarte **Server** die **Serverkonfiguration** Eigenschaftenseite angezeigt kann angepasst werden. Dies ist eine erweiterte Funktion, die Sie benötigen, wenn Ihre Bedürfnisse, die Standardwerte hinausgehen oder andere Server hinzufügen möchten. Klicken Sie auf **Anpassen** , um die Logik im Dialogfeld **Server** anzupassen. Dies wird die **Serverkonfiguration** Eigenschaftenseite schließen und öffnen Sie die Datei **componentsets.xml** in Eclipse Sie dann ändern können, um die Konfigurationsvorlage Server erweitern. Dokumentation für die **componentsets.xml** ist in der Datei **componentsets.xml** enthalten.

Wenn Sie die Option **Bereitstellen meinem lokalen Server (Auto-Upload, Cloud-Speicher)** verwenden:

1. Aktivieren Sie das Kontrollkästchen mit dem Namen **Deploy meinem lokalen Server (Auto-Upload, Cloud-Speicher)**.
1. Wählen Sie mit der Dropdownliste **Speicherkonto** **(Auto)**. Wenn **(Auto)** hier angeben verwendet Eclipse Toolkit gleichen Speicherkonto für Ihren Server als für die Bereitstellung im Dialogfeld **Azure veröffentlichen** wählen.
1. Klicken Sie auf **OK** , um die Änderungen zu speichern.

Wählen Sie einen Server-Installationspfad auf dem Computer im Textfeld **Lokaler Pfad** , wenn Folgendes zutrifft:

* Möchten die Bereitstellung im Emulator testen (nur Windows).
* Lokal installierten Server in der Cloud bereitstellen möchten.
* Sie möchten verwenden einen benutzerdefinierten Download selbst in der Cloud in dem Fall, außerdem sicherstellen, dass die **Bereitstellung meinem lokalen Server (Auto-Upload, Cloud-Speicher)** über Option.

Wenn keine der obigen Optionen auf Ihre Situation zutrifft, ist die lokale Einstellung optional.

### <a name="applications-configuration"></a>Anwendungskonfiguration ###

Folgendes ist ein Beispiel, wie Sie eine Anwendung angeben.

![][ic719512]

Klicken Sie auf **Hinzufügen** zu einer anderen Anwendung hinzufügen oder **Entfernen** Sie eine Anwendung entfernen. Aus Effizienzgründen können einen Download für die Quelle einer Anwendung in der Cloud bereitstellen, verwenden Sie [Komponenteneigenschaften](#components_properties) eine URL Speicherkonto usw. angeben. 

Ab April 2014 Release werden Ihre Anwendung automatisch im gleichen Speicherkonto (unter **Eclipsedeploy** Container) wie für die Bereitstellung hochgeladen. Die Startlogik der Bereitstellung enthält einen Schritt, der zuerst diese Anträge von diesem Storage-Konto heruntergeladen. Dies bedeutet, Sie können Ihre Anwendung in Ihrer Bereitstellung ohne und das gesamte Paket manuell hochladen neuere Versionen der Anwendung direkt in diesem Storage-Konto (beispielsweise unter Verwendung von Azure-Portal) erneut aktualisieren, ersetzen Dateien WAR ursprünglich vom Toolkit es hochgeladen. Dann nur initiieren Sie die Wiederverwendung aller Instanzen dieser Rolle mithilfe der Azure-Verwaltungsportal erneut oder Befehlszeilen-Dienstprogramme. (Auslösen Rolle direkt in Eclipse Toolkit recycling ist nicht aktuell unterstützt.)

### <a name="notes-about-server-configuration"></a>Hinweise zur Serverkonfiguration ###

Änderungen, die über die Eigenschaftenseite **Serverkonfiguration** der `<component>` -Elemente der Datei package.xml.

Wenn Sie **automatisch hochladen...** verwenden oder **Bereitstellen von Download** -Optionen für JDK oder Anwendungsserver und Sie für die Cloud (nicht Serveremulator) erstellen und Sie mit dem Netzwerk verbunden sind, bemerken Sie Nachrichten wie folgt in die Konsolenausgabe erstellen wie Ant-Generator den Download Verfügbarkeit überprüft:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Wenn die Option **Bereitstellen aus herunterladen...** möglicherweise die folgende Warnung angezeigt, aber der Buildprozess fortgesetzt:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Diese Warnung ist der einzige Hinweis darauf, dass der Download Verfügbarkeit bestätigt wurde. Also wenn eine Bereitstellung in der Cloud aus irgendeinem Grund fehlschlägt, überprüfen Sie, ob Sie diese Warnung erhalten.

Möchten Sie deaktivieren Download Überprüfung (z. B. Meinung unnötig verlangsamt den Build), die `verifydownloads` -Attribut `false` in der `<windowsazurepackage>` Bestandteil package.xml: 

`<windowsazurepackage verifydownloads="false" ...>` 

Bei Auswahl die Option **automatisch hochladen...** dann in der Konsole sehen Sie Buildmeldungen der Fortschritt des Uploads alle 5 Sekunden, wenn ein Upload erforderlich ist.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL-Abladung Eigenschaften ###

Das Kontextmenü für die Rolle im Projekt-Explorer des Eclipse und klicken Sie anschließend auf **SSL-Abladung**auf **Azure**. 

![][ic719481]

In diesem Dialogfeld können Sie SSL-Abladung ermöglicht einfache Hypertext Transfer Protocol Secure (HTTPS) in der Java-Bereitstellung in Azure-Unterstützung ohne SSL in Ihren Java-Anwendungsserver konfigurieren. Weitere Informationen finden Sie unter [SSL-Abladung][] und [zum SSL-Abladung verwenden][].

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Azure Toolkit installieren für Eclipse][]

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

[Azure-Eigenschaften][]

[Liste Azure-Speicher][]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure-Eigenschaften]: http://go.microsoft.com/fwlink/?LinkID=699524
[Liste Azure-Speicher]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.Microsoft.windowsazure.ServiceRuntime Paket Zusammenfassung]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debuggen einer bestimmten Rolleninstanz in einer Bereitstellung mit mehreren Instanzen]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Debuggen von Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Große Bereitstellung bereitstellen]: http://go.microsoft.com/fwlink/?LinkID=699536
[Wie Sie zusammengestellten Zwischenspeichern]: http://go.microsoft.com/fwlink/?LinkID=699542
[Wie SSL-Abladung verwenden]: http://go.microsoft.com/fwlink/?LinkID=699545
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Sitzungsaffinität]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-Abladung]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png

<properties
   pageTitle="Konfigurieren Ihre Azure-Projekt mehrere Dienstkonfigurationen | Microsoft Azure"
   description="Informationen Sie zum Azure-Cloud-Dienstprojekt konfigurieren, indem Sie die Dateien ServiceDefinition.csdef und ServiceConfiguration.cscfg ändern."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurieren Ihre Azure-Projekt mehrere Dienstkonfigurationen

Ein Azure-Cloud-Dienst-Projekt enthält zwei Dateien: ServiceDefinition.csdef und ServiceConfiguration.cscfg. Diese Dateien sind mit der Azure-Cloud-Dienst-Anwendung gepackt und in Azure bereitgestellt.

- Die **ServiceDefinition.csdef** -Datei enthält Metadaten, die von der Azure-Umgebung bei der Cloud-Dienst Anwendung enthält Rollen erforderlich ist. Diese Datei enthält auch Konfigurationen, die für alle Instanzen gelten. Diese Konfigurationen können zur Laufzeit mithilfe von Azure Service Hosting Laufzeit-API gelesen werden. Diese Datei kann nicht aktualisiert werden, während der Dienst in Azure ausgeführt wird.

- Die Datei **ServiceConfiguration.cscfg** Werte für Konfigurationen in der Service-Definitionsdatei definiert und gibt die Anzahl der Instanzen für jede Rolle ausführen. Diese Datei kann aktualisiert werden, während in Azure Cloud-Dienst ausgeführt wird.

Azure Tools for Microsoft Visual Studio bieten Eigenschaftenseiten, mit denen Sie in diesen Dateien gespeicherten Einstellungen festgelegt. Doppelklicken Sie auf die Rolle Referenz unter Azure Cloud Service-Projekt im Projektmappen-Explorer Zugriff auf Eigenschaftenseiten Rolle Verweis Maustaste und wählen Sie **Eigenschaften**, wie in der folgenden Abbildung dargestellt.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informationen über die zugrunde liegenden Schemas für die Dienstdefinition und Service-Konfigurationsdateien finden Sie [Referenzinformationen](https://msdn.microsoft.com/library/azure/dd179398.aspx). Weitere Informationen zur Konfiguration finden Sie unter [Konfigurieren von Cloud-Diensten](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurieren von Eigenschaften

Eigenschaftenseiten für Web-Rolle und eine Worker-Rolle sind ähnlich, jedoch einige Unterschiede in den folgenden Abschnitten hingewiesen bestehen.

Konfigurieren Sie auf der Seite **Zwischenspeichern** Azure caching Services.

### <a name="configuration-page"></a>Seite "Konfiguration"

Auf der Seite **Konfiguration** können Sie diese Eigenschaften festlegen:

**Instanzen**

**Instanz** Count-Eigenschaft auf die Anzahl der Instanzen, die den Betrieb dieser Rolle, festgelegt.

Die **VM Size** -Eigenschaft auf **Extra Small**, **klein**, **Mittel**, **Groß**oder **Extragroß**festgelegt.  Weitere Informationen finden Sie unter [Größen für Cloud-Services](./cloud-services/cloud-services-sizes-specs.md).

**Startaktion** (Nur Webrolle)

Legen Sie diese Eigenschaft festlegen, dass Visual Studio Webbrowser für Endpunkte HTTP oder HTTPS Endpunkte oder beides starten soll, wenn Sie das Debuggen starten.

Die HTTPS-Endpunkt-Option ist nur verfügbar, wenn Sie bereits einen HTTPS-Endpunkt für Ihre Rolle definiert haben. Sie können einen HTTPS-Endpunkt auf der Eigenschaftenseite **Endpunkte** definieren.

Wenn Sie bereits einen HTTPS-Endpunkt hinzugefügt haben, die HTTPS-Endpunkt-Option ist standardmäßig aktiviert und Visual Studio startet einen Browser für diesen Endpunkt beim Starten des Debuggens neben einem Browser HTTP-Endpunkt. Dies setzt voraus, dass beide Startoptionen aktiviert sind.

**Diagnose**

Standardmäßig ist die Diagnose für die Web-Rolle aktiviert. Auf lokalen Speicheremulator verwenden das Azure-Cloud-Projekt und Speicher-Dienstkonto festgelegt. Wenn Sie in Azure bereitstellen möchten, können Sie die Generator-Schaltfläche (**...**) um Azure-Speicher in der Cloud verwenden das Speicherkonto aktualisiert auswählen. Sie können bei Bedarf oder automatisch regelmäßig die Diagnosedaten an das Speicherkonto übertragen. Weitere Informationen zu Azure Diagnostics finden Sie unter [Aktivieren der Diagnose in Azure Cloud Services und virtuellen Maschinen](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Einstellungsseite

Fügen Sie auf **der Einstellungsseite** Konfiguration für Ihren Dienst. Konfigurationen sind Name-Wert-Paaren. Code, der in der Rolle kann die Werte der Konfigurationen zur Laufzeit mithilfe von [Azure verwaltet Library](http://go.microsoft.com/fwlink?LinkID=171026)bereitgestellte Klassen lesen. Insbesondere gibt die [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) -Methode den Wert einer benannten Einstellung zur Laufzeit.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfigurieren der Verbindungszeichenfolge für ein Speicherkonto

Eine Verbindungszeichenfolge ist eine Einstellung, die Verbindungs-und Authentifizierung Speicheremulator oder einem Azure-Speicher bietet. Wenn Code Azure Storage Services Daten – also Blob, Warteschlange oder Tabellendaten – von Code in einer Rolle zugreifen muss, müssen Sie eine Verbindungszeichenfolge für das Storage-Konto definieren.

Eine Verbindungszeichenfolge, die auf ein Konto Azure-Speicher verwenden ein definiertes Format. Informationen zum Erstellen von Verbindungszeichenfolgen finden Sie unter [Azure Storage-Verbindungszeichenfolgen konfigurieren](./storage/storage-configure-connection-string.md).

Wenn Sie bereit sind, den Service für Azure-Speicherdienste zu testen oder Sie den Cloud-Dienst in Azure bereitstellen möchten, können Sie den Wert alle Verbindungszeichenfolgen auf der Azure-Speicher-Konto ändern. Wählen Sie****(...), **Storage-Anmeldeinformationen eingeben**. Geben Sie Ihre Kontoinformationen, die Ihren Benutzernamen und kontoschlüssel enthält. Im Dialogfeld **Storage Konto Verbindungszeichenfolge** können Sie auch angeben, ob HTTPS Standardendpunkte (Standardeinstellung), Standard-HTTP-Endpunkte oder benutzerdefinierte Endpunkte verwenden möchten. Sie können benutzerdefinierte Endpunkte verwenden, wenn Sie einen benutzerdefinierten Domänennamen für den Dienst registriert haben, wie unter [Konfigurieren einer benutzerdefinierten Domänennamen für BLOB-Daten in Azure Storage-Konto](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Ändern Sie die Verbindungszeichenfolgen auf Azure Storage-Konto, bevor Sie den Dienst bereitstellen. Andernfalls kann Ihre Rolle nicht oder Zustände initialisieren, beschäftigt und beenden zu durchlaufen.

## <a name="endpoints-page"></a>Seite

Eine Worker-Rolle können eine beliebige Anzahl von HTTP, HTTPS oder TCP-Endpunkte. Endpunkte kann input Endpunkte für externe Clients verfügbar sind oder interne Endpunkte, die anderen Rollen, die der Dienst ausgeführt werden.

- Um einen HTTP-Endpunkt für externe Clients und Webbrowser verfügbar ändern Sie Endpunkttyp Eingabe, und geben Sie einen Namen und eine öffentliche Portnummer.

- Um einen HTTPS-Endpunkt für externe Clients und Webbrowser verfügbar ändern Sie Endpunkttyp auf **input**, und geben Sie einen Namen, eine öffentliche Portnummer und ein Zertifikat Management.

    Beachten Sie, dass vor der Angabe Verwaltungszertifikat das Zertifikat auf der Eigenschaftenseite **Zertifikate** definieren müssen.

- Um einen Endpunkt für den internen Zugriff von anderen Rollen im Cloud-Dienst verfügbar ändern Sie Endpunkttyp internen, und geben Sie einen Namen und kann private Ports für diesen Endpunkt.

## <a name="local-storage-page"></a>Lokaler Speicher-Seite

**Lokaler Speicher** -Eigenschaftenseite können Sie um eine oder mehrere lokale Speicherressourcen für eine Rolle zu reservieren. Lokale Speicherressourcen ist ein reservierter Verzeichnis im Dateisystem der Azure Virtual Machine in der Instanz einer Rolle ausgeführt wird.

## <a name="certificates-page"></a>Seite Zertifikate

Auf der Seite **Zertifikate** können Sie Zertifikate mit Ihrer Rolle zuordnen. Zertifikate, die Sie hinzufügen können verwendet werden, die HTTPS-Endpunkte auf den **Endpunkten** konfigurieren.

Die Eigenschaftenseite **Zertifikate** hinzugefügt der Dienstkonfiguration Informationen Ihre Zertifikate. Beachten Sie, dass Ihre Zertifikate-Dienst nicht verpackt werden. Sie müssen Ihre Zertifikate separat in Azure [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen.

Um Ihre Rolle ein Zertifikat zuzuordnen, geben Sie einen Namen für das Zertifikat. Sie verwenden diesen Namen des Zertifikats auf einen HTTPS-Endpunkt auf der Eigenschaftenseite **Endpunkte** konfigurieren. Als Nächstes geben Sie an, ob Zertifikatspeicher des **Lokalen** oder **Aktuelle Benutzer** und den Namen des Informationsspeichers. Schließlich geben Sie den Fingerabdruck des Zertifikats. Ist das Zertifikat in Aktueller Benutzer\Persönlich Speicher (My), können Sie den Zertifikatfingerabdruck aufgefüllte Liste auswählen das Zertifikat eingeben. Wenn sie an einem anderen Speicherort befindet, geben Sie Fingerabdruck manuell.

Wenn Sie ein Zertifikat aus dem Zertifikatspeicher hinzufügen, werden alle Zwischenzertifikate Konfigurationsinformationen für Sie automatisch hinzugefügt. Diese Zwischenzertifikate müssen auch in Azure hochgeladen werden, um den Dienst für SSL ordnungsgemäß konfigurieren.

Management Zertifikate, die Ihrem Dienst zugeordnet gelten für Ihren Dienst erst in der Cloud ausgeführt wird. Der Dienst in der lokalen verwendet ein standard Zertifikat Serveremulator verwaltet.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfiguration von Azure-Cloud-Dienstprojekt

Zum Konfigurieren, die für eine gesamte Azure-Cloud-Dienstprojekt gelten zuerst öffnen Sie das Kontextmenü für den Projektknoten, und wählen Sie Eigenschaften, um die Eigenschaftenseiten öffnen. Die folgende Tabelle zeigt die Eigenschaftenseiten.

|Eigenschaftenseite|Beschreibung|
|---|---|
|Anwendung|Auf dieser Seite zeigen Sie Informationen zur Version der Azure-Tools können Sie auf die aktuelle Version der Tools aktualisieren, die dieser Cloud-Dienstprojekt verwendet.|
|Buildereignisse|Auf dieser Seite können Sie Präbuild-und Postbuildereignisse festlegen.|
|Entwicklung|Auf dieser Seite können Sie Buildkonfigurationsanweisungen und die Umstände, unter denen alle Postbuildereignisse ausgeführt werden.|
|Web|Auf dieser Seite können Sie konfigurieren, die auf dem Webserver.|

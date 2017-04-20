<properties
    pageTitle="Zum Erstellen und Bereitstellen eines Cloud-Diensts | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Bereitstellen eines Methode der Firma in Azure Cloud-Diensts."
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Zum Erstellen und Bereitstellen eines Cloud-Diensts

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure-Verwaltungsportal](cloud-services-how-to-create-deploy.md)

Klassische Azure-Portal bietet zwei Methoden zum Erstellen und Bereitstellen eines Cloud-Diensts: **Schnell erstellen** und **Benutzerdefinierte**.

In diesem Thema erläutert, wie mit Methode erstellen Sie schnell einen neuen Clouddienst erstellen und verwenden dann hochladen und Bereitstellen eines Pakets Cloud-Dienst in Azure **Hochladen** . Bei Verwendung dieser Methode wird der Azure-Verwaltungsportal verfügbar Anbindung für alle Vorschriften abschließen, wie Sie. Wenn Sie beim Erstellen Ihrer Cloud-Dienst bereitstellen sind, möglich auf die Verwendung **Benutzerdefinierter**.

> [AZURE.NOTE] Möchten Sie den Clouddienst von Visual Studio Team Services (VSTS) veröffentlichen, schnell erstellen und dann VSTS Veröffentlichung von **Quick Start** oder Dashboard einrichten. Weitere Informationen finden Sie in der [Kontinuierlichen Bereitstellung in Azure mithilfe von Visual Studio Team Services][TFSTutorialForCloudService], oder in der Seite **Quick Start** -Hilfe.

## <a name="concepts"></a>Konzepte
Drei Komponenten sind erforderlich, um eine Anwendung als Cloud-Dienst in Azure bereitstellen:

- **Dienstdefinition**  
  Die Cloud-Definitionsdatei (.csdef) definiert das Service-Modell sowie die Rollen.

- **Konfiguration**  
  Die Cloud-Konfigurationsdatei (.cscfg) bietet Konfigurationen für die Cloud Service und einzelne Rollen, einschließlich der Anzahl der Instanzen.

- **Service-Paket**  
  Das Service-Paket (.cspkg) enthält den Anwendungscode und Konfigurationen und Service-Definitionsdatei.
  
Erfahren Sie mehr über diese und erstellen Sie ein Paket [hier](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Bereiten Sie Ihrer app vor
Vor der Bereitstellung eines Cloud-Diensts müssen Sie aus Ihrem Anwendungscode und einer Cloud-Konfigurationsdatei (.cscfg) das Cloud Service-Paket (.cspkg) erstellen. Azure SDK enthält Tools für diese Dateien erforderliche Bereitstellung vorbereiten. Sie können das SDK auf der Seite [Downloads Azure](https://azure.microsoft.com/downloads/) in der Sprache in denen Anwendungscode entwickeln möchten.

Drei Cloud Servicefunktionen erfordern spezielle Konfigurationen, bevor Sie ein Service-Paket exportieren:

- Cloud-Dienst bereitgestellt werden soll, die Secure Sockets Layer (SSL) zum Verschlüsseln von Daten, [Konfigurieren Sie Ihre Anwendung](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) für SSL verwendet.

- Wenn Sie Remotedesktopverbindungen Rolleninstanzen für Remotedesktop [Rollen konfigurieren](cloud-services-role-enable-remote-desktop.md) konfigurieren möchten.

- Ggf. Konfigurieren ausführlicher für den Clouddienst aktivieren Sie Azure Diagnostics für Cloud-Dienst. *Minimale Überwachung* (die Überwachungsstufe) verwendet Leistungsindikatoren von Betriebssystemen für Instanzen (virtuelle Maschinen) gesammelt. "Ausführliche Überwachung * zusätzliche Statistiken basierend auf Daten innerhalb der Rolleninstanzen ermöglicht genaueren Analyse von Problemen bei der Anwendung erfasst. Aktivieren der Azure-Diagnose finden finden Sie unter [Aktivieren der Diagnose in Azure](cloud-services-dotnet-diagnostics.md).

Um einen Cloud-Dienst mit der Bereitstellung von Webrollen oder Worker-Rollen erstellen, müssen Sie [das Service-Paket erstellen](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Bevor Sie beginnen

- Azure SDK installiert haben, klicken Sie auf zum Öffnen der [Azure-Downloadseite](https://azure.microsoft.com/downloads/) **Azure SDK installieren** und Laden Sie das SDK für die Sprache, in der Sie Code zu arbeiten. (Sie müssen eine Verkaufschance dazu später.)

- Wenn alle Instanzen ein Zertifikat benötigen, erstellen Sie die Zertifikate. Cloud-Dienste erfordern eine PFX-Datei mit einem privaten Schlüssel. Sie können [Zertifikate in Azure hochladen](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) erstellen und Cloud-Dienst bereitstellen.

- Wenn Sie Cloud-Dienst zu einer Gruppe bereitstellen möchten, erstellen Sie die Gruppe. Eine Gruppe können Sie dem Cloud-Dienst und andere Dienste Azure an in einer Region bereitstellen. Sie können die Gruppe im Bereich **Netzwerke** klassischen Azure-Portal, auf der Seite **Gruppen** erstellen.


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Gewusst wie: erstellen einen Cloud-Dienst mit schnellen Erstellen

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/) **neu**>**berechnen**>**Cloud-Dienst**>**Schnell erstellen**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. **URL**Geben Sie eine Subdomäne mit der öffentlichen URL für den Zugriff auf die Cloud-Dienst im Praxiseinsatz. Das URL-Format für den Praxiseinsatz ist: http://*MyURL*. cloudapp.net.

3. **Region oder Gruppe**wählen Sie geografische Region oder Gruppe den Clouddienst bereitgestellt. Wählen Sie eine Gruppe, wenn Sie den Cloud-Dienst am gleichen Speicherort wie andere Azure-Dienste innerhalb eines Bereichs bereitstellen.

4. Klicken Sie auf **Cloud-Dienst erstellen**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Sie können den Status des Prozesses im Bereich unten im Fenster überwachen.

    Bereich **Cloud Services** öffnet mit neuen Cloud-Dienst angezeigt. Ändert der Status erstellt, wurde Cloud erstellen erfolgreich abgeschlossen.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Gewusst wie: Hochladen ein Zertifikats für einen Clouddienst

1. Klicken Sie auf **Clouddienste**im [klassischen Azure-Portal](http://manage.windowsazure.com/)klicken Sie auf den Namen des Cloud-Dienst, und klicken Sie auf **Zertifikate**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Klicken Sie auf **ein Zertifikat laden** oder **Hochladen**.

3. Verwenden Sie in **Datei** **Durchsuchen** das Zertifikat (PFX-Datei) auswählen.

4. Geben Sie im Feld **Kennwort**den privaten Schlüssel für das Zertifikat.

5. Klicken Sie auf **OK** (Häkchen).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Den Fortschritt des Uploads im Nachrichtenbereich unten überwachen können. Wenn der Upload abgeschlossen ist, wird das Zertifikat zur Tabelle hinzugefügt. Klicken Sie in den Bereich auf OK, um die Meldung zu schließen.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Gewusst wie: Bereitstellen einen Cloud-Dienst

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Clouddienste**klicken Sie auf den Namen des Cloud-Dienst, und klicken Sie auf **Dashboard**.

2. Klicken Sie auf **Hochladen eine neuen Bereitstellung in der Produktion** oder **Hochladen**.

3. **Bereitstellung Bezeichnung**Geben Sie einen Namen für die neue Bereitstellung – z. B. MyCloudServicev4.

3. **Paket**mithilfe **Durchsuchen** die Service-Paket-Datei (.cspkg) aus.

4. Verwenden Sie in der **Konfiguration** **Durchsuchen** können Sie den Dienst konfigurieren (.cscfg) verwenden.

5. Wenn Cloud-Dienst Rollen nur eine Instanz enthalten soll, aktivieren Sie das Kontrollkästchen **bereitstellen, auch wenn eine oder mehrere Rollen eine einzelne Instanz enthalten** , ermöglichen die Bereitstellung fortgesetzt.

    Azure kann nur garantieren 99,95 % Cloud-Dienst bei der Wartung und Aktualisierung hat jede Rolle mindestens zwei Instanzen. Bei Bedarf können Sie weitere Instanzen nach der Bereitstellung von Cloud-Dienst **Skalierung** auf Hinzufügen. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

6. Klicken Sie auf **OK** (Häkchen), um die Cloudbereitstellung-Dienst starten.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Sie können den Status der Bereitstellung wird im Mitteilungsbereich überwachen. Klicken Sie auf OK, um die Meldung auszublenden.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Überprüfen der Bereitstellung erfolgreich abgeschlossen

1. Klicken Sie auf **Dashboard**.

    Der Status sollte anzeigen, dass der Dienst **ausgeführt**.

2. Klicken Sie unter **Blick**auf den URL der Site, um Ihre Cloud-Dienst in einem Webbrowser öffnen.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Nächste Schritte

* [Konfiguration des Cloud-Dienst](cloud-services-how-to-configure.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name.md).
* [Verwalten der Cloud-Dienst](cloud-services-how-to-manage.md).
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate.md).
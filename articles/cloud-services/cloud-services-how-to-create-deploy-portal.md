<properties
    pageTitle="Zum Erstellen und Bereitstellen eines Cloud-Diensts | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Bereitstellen eines Cloud-Diensts mithilfe des Azure-Portals."
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Zum Erstellen und Bereitstellen eines Cloud-Diensts

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-create-deploy-portal.md)
- [Azure-Verwaltungsportal](cloud-services-how-to-create-deploy.md)

Azure-Portal bietet zwei Methoden zum Erstellen und Bereitstellen eines Cloud-Diensts: *Schnell erstellen* und *Benutzerdefinierte*.

Erläutert, wie mit der Methode schnell erstellen neuen Clouddienst erstellen und verwenden Sie dann hochladen und Bereitstellen eines Pakets Cloud-Dienst in Azure **Hochladen** . Wenn Sie diese Methode verwenden, wird Azure-Portal verfügbar Anbindung für alle unterwegs abschließen. Wenn Sie beim Erstellen Ihrer Cloud-Dienst bereitstellen sind, möglich auf die Verwendung benutzerdefinierter.

> [AZURE.NOTE] Möchten Sie den Clouddienst von Visual Studio Team Services (VSTS) veröffentlichen, schnell erstellen und dann VSTS Veröffentlichung von Azure Schnellstart oder Dashboard einrichten. Weitere Informationen finden Sie in der [Kontinuierlichen Bereitstellung in Azure mithilfe von Visual Studio Team Services][TFSTutorialForCloudService], oder in der Seite **Quick Start** -Hilfe.

## <a name="concepts"></a>Konzepte
Bereitstellen eine Anwendung als Cloud-Dienst in Azure sind drei Komponenten erforderlich:

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

- Cloud-Dienst bereitgestellt werden soll, die Secure Sockets Layer (SSL) zum Verschlüsseln von Daten, [Konfigurieren Sie Ihre Anwendung](cloud-services-configure-ssl-certificate-portal.md#modify) für SSL verwendet.

- Wenn Sie Remotedesktopverbindungen Rolleninstanzen für Remotedesktop [Rollen konfigurieren](cloud-services-role-enable-remote-desktop.md) konfigurieren möchten. Dies kann nur im klassischen Portal erfolgen.

- Ggf. Konfigurieren ausführlicher für den Clouddienst aktivieren Sie Azure Diagnostics für Cloud-Dienst. *Minimale Überwachung* (die Überwachungsstufe) verwendet Leistungsindikatoren von Betriebssystemen für Instanzen (virtuelle Maschinen) gesammelt. *Verbose Überwachung* sammelt zusätzliche Statistiken basierend auf Daten innerhalb der Rolleninstanzen ermöglicht genaueren Analyse von Problemen bei der Anwendung. Aktivieren der Azure-Diagnose finden finden Sie unter [Aktivieren der Diagnose in Azure](cloud-services-dotnet-diagnostics.md).

Um einen Cloud-Dienst mit der Bereitstellung von Webrollen oder Worker-Rollen erstellen, müssen Sie [das Service-Paket erstellen](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Bevor Sie beginnen

- Azure SDK installiert haben, klicken Sie auf zum Öffnen der [Azure-Downloadseite](https://azure.microsoft.com/downloads/) **Azure SDK installieren** und Laden Sie das SDK für die Sprache, in der Sie Code zu arbeiten. (Sie müssen eine Verkaufschance dazu später.)

- Wenn alle Instanzen ein Zertifikat benötigen, erstellen Sie die Zertifikate. Cloud-Dienste erfordern eine PFX-Datei mit einem privaten Schlüssel. Als Sie [die Zertifikate in Azure hochladen können]() erstellen und Bereitstellen von Cloud-Dienst.

- Wenn Sie Cloud-Dienst zu einer Gruppe bereitstellen möchten, erstellen Sie die Gruppe. Eine Gruppe können Sie dem Cloud-Dienst und andere Dienste Azure an in einer Region bereitstellen. Sie können die Gruppe im Bereich **Netzwerke** klassischen Azure-Portal, auf der Seite **Gruppen** erstellen.


## <a name="create-and-deploy"></a>Erstellen und bereitstellen

1. Auf der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **Neu > virtuellen Computer**, blättern Sie abwärts, und klicken Sie auf **Cloud-Dienst**.

    ![Cloud-Dienst veröffentlichen](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Klicken Sie auf " **Erstellen**" am unteren Rand der Informationsseite angezeigt. 
4. Geben Sie einen Wert in das neue **Cloud Service** -Blade für den **DNS-Namen**.
5. Eine neue **Ressourcengruppe** erstellen oder ein vorhandenes Profil auszuwählen.
6. Wählen Sie einen **Speicherort**.
7. Klicken Sie auf **Paket**. Das Blade **ein Paket hochladen** wird geöffnet. Füllen Sie die Pflichtfelder aus.  

     Wenn Ihre Rollen eine einzelne Instanz enthalten, sicherstellen Sie, dass **bereitstellen, enthält eine oder mehrere Rollen eine einzelne Instanz** ausgewählt ist.

8. Stellen Sie sicher, dass **Start Bereitstellung** ausgewählt ist.
9. Klicken Sie auf **OK** um das Blade **ein Paket hochladen** schließen.
10. Wenn Sie keine Zertifikate hinzugefügt haben, klicken Sie auf **Erstellen**.

    ![Cloud-Dienst veröffentlichen](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Hochladen eines Zertifikats

Wenn das Bereitstellungspaket [Zertifikate konfiguriert](cloud-services-configure-ssl-certificate-portal.md#modify)wurde, können Sie das Zertifikat jetzt hochladen.

1. Wählen Sie **Zertifikate**Blade **Hinzufügen Zertifikate** wählen Sie die SSL-Zertifikat PFX-Datei aus und geben Sie das **Kennwort** für das Zertifikat
2. Auf **Anhängen Zertifikat**, und klicken Sie auf die **Zertifikate hinzufügen** auf **OK** .
3. Klicken Sie auf die **Cloud-Dienst** **Erstellen** . Wenn die Bereitstellung **betriebsbereit** erreicht hat, können Sie mit den nächsten Schritten fortfahren.

    ![Cloud-Dienst veröffentlichen](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Überprüfen der Bereitstellung erfolgreich abgeschlossen

1. Klicken Sie auf die Cloud-Dienstinstanz.

    Der Status sollte anzeigen, dass der Dienst **ausgeführt**.

2. Klicken Sie unter **Essentials**auf **Website-URL** zum Cloud-Dienst in einem Webbrowser öffnen.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Nächste Schritte

* [Konfiguration des Cloud-Dienst](cloud-services-how-to-configure-portal.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name-portal.md).
* [Verwalten der Cloud-Dienst](cloud-services-how-to-manage-portal.md).
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md).
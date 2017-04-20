<properties 
    pageTitle="Erstellen eine cloudsammlung von Azure RemoteApp | Microsoft Azure" 
    description="Erfahren Sie, wie eine Bereitstellung von Azure RemoteApp zu erstellen, die Daten in der Azure-Cloud gespeichert." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Erstellen eine cloudsammlung von Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Es gibt zwei Arten von [Azure RemoteApp-Sammlungen](remoteapp-collections.md): 

- Cloud: befindet sich vollständig in Azure. Sie können alle Daten in der Cloud speichern (so eine Auflistung nur Cloud) oder zu Ihrer Sammlung an ein VNET Daten speichern.   
- Hybrid: enthält ein virtuelles Netzwerk für lokalen Zugriff – dies erfordert die Verwendung von Azure AD und eine lokale Active Directory-Umgebung.

Dieses Lernprogramm führt Sie durch den Prozess der Erstellung einer Cloud-Sammlung. Umfasst vier Schritte: 

1.  Erstellen einer Azure RemoteApp-Auflistung.
2.  Konfigurieren Sie Verzeichnissynchronisation optional. Bei Verwendung von Azure AD + Active Directory müssen Sie Benutzer, Kontakte und Kennwörter für lokale Active Directory Azure AD-Mandanten zu synchronisieren.
5.  Apps zu veröffentlichen.
6.  Konfigurieren Sie Benutzerzugriff.


**Bevor Sie beginnen**

Sie müssen vor dem Erstellen der Sammlung folgendermaßen:

- [Melden Sie sich](https://azure.microsoft.com/services/remoteapp/) für Azure RemoteApp. 
- Informationen Sie über den Benutzer Zugriff zu gewähren. Dies kann Microsoft-Kontoinformationen oder Active Directory arbeiten Kontoinformationen für Benutzer.
- Es wird vorausgesetzt, dass Sie werden entweder eine Vorlage Bilder als Teil Ihres Abonnements oder bereits das Vorlagenbild hochgeladen, das Sie verwenden möchten. Benötigen Sie eine andere Vorlage Bild, möglich das auf der Vorlage Bilder. Nur auf **Vorlagenbild hochladen** , und folgen Sie den Schritten im Assistenten. 
- Office 365 ProPlus Bild verwenden? Überprüfen Sie Informationen [hier](remoteapp-officesubscription.md).
- Benutzerdefinierte apps oder Programme LOB möchten? Neues [Bild](remoteapp-imageoptions.md) erstellen und in der Cloud-Auflistung verwenden.
- Abbildung, ob Sie mit einem VNET verbinden. Eine VNET herstellen möchten, vergewissern sie [Richtlinien anpassen](remoteapp-vnetsizing.md) und es entspricht [RemoteApp können](remoteapp-vnet.md). [VNET Planung Artikel ](remoteapp-planvnet.md)Weitere Informationen anzeigen
- Wenn Sie ein VNET verwenden, entscheiden Sie, ob der lokalen Active Directory-Domäne hinzufügen möchten.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Schritt 1: Erstellen einer Auflistung Cloud - mit oder ohne ein VNET##


Gehen Sie zum **Erstellen einer Auflistung nur Cloud**:

1. Gehen Sie im Verwaltungsportal RemoteApp-Seite.
2. Klicken Sie auf **Neu > Erstellen Sie schnell**.
3. Geben Sie einen Namen für die Sammlung, und wählen Sie Ihre Region.
4. Wählen Sie den Plan - standard oder Standard verwenden möchten.
5. Wählen Sie die Vorlage für diese Auflistung. 

    **Tipp:** Ihr Abonnement für RemoteApp enthält [Vorlage Bilder](remoteapp-images.md) mit Office 365 oder Office 2013 (Testphase) Programme (z.B. Word) veröffentlicht und veröffentlichen. Sie können auch ein neues [Bild](remoteapp-imageoptions.md) erstellen und in der Cloud-Auflistung verwenden.


1. Klicken Sie auf **Sammlung RemoteApp zu erstellen**.
    
    **Wichtig:** Es dauert bis zu 30 Minuten die Auflistung bereitstellen.

Nach dem Erstellen der Auflistung Azure RemoteApp Doppelklicken Sie auf den Namen der Auflistung. **Quick Start** -Seite bringt -, in dem Sie die Auflistung konfiguriert haben.

Gehen Sie folgendermaßen vor, eine **Cloud + VNET Auflistung**erstellen:

1. Gehen Sie im Verwaltungsportal Azure RemoteApp-Seite.
2. **Klicken Sie auf** > **vnet erstellen**.
3. Geben Sie einen Namen für die Auflistung.
4. Wählen Sie den Plan - standard oder Standard verwenden möchten.
5. Wählen Sie das erstellten VNET. Weiß nicht dazu? Jetzt sind die Schritte im [Hybrid](remoteapp-create-hybrid-deployment.md) -Thema.
6. Entscheiden Sie, ob Ihre Auflistung der Domäne beitreten. Falls Ja, müssen Sie mit Active Directory verbinden Azure AD integriert und Active Directory-Umgebung. Wird unten in **Schritt2**behandelt.
6. Klicken Sie auf **Sammlung RemoteApp zu erstellen**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Schritt 2: Konfigurieren von Active Directory Directory-Synchronisierung (optional) ##

Wenn Sie Active Directory verwenden möchten, muss Azure RemoteApp Verzeichnissynchronisation zwischen Azure Active Directory und Active Directory für lokale Benutzer, Kontakte und Kennwörter in Ihrem Mandanten Azure Active Directory synchronisieren. Anzeigen Sie [Konfigurieren von Active Directory für Azure RemoteApp](remoteapp-ad.md) Planungsinformationen Sie können auch direkt mit [Active Directory verbinden](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) Informationen wechseln.

## <a name="step-3-publish-apps"></a>Schritt 3: Veröffentlichen Sie apps ##

Azure RemoteApp app ist die app oder Programm, das Sie für Ihre Benutzer. Das Vorlagenbild befindet sich im hochgeladenen für die Auflistung. Wenn ein Benutzer eine Anwendung, die Anwendung in ihrer Umgebung ausgeführt wird, aber tatsächlich in einem virtuellen Computer in Azure ausgeführt wird. 

Bevor Benutzer apps zugreifen können, müssen Sie veröffentlichen-Veröffentlichung apps können Benutzer apps über Remote Desktop Client zugreifen.
 
Sie können mehrere apps Ihrer Azure RemoteApp-Sammlung veröffentlichen. Die Veröffentlichungsseite klicken Sie auf **Veröffentlichen** , um ein Programm hinzuzufügen. Sie können entweder über **das Startmenü des Vorlagenbilds oder die Pfadangabe für die Anwendung auf dem** veröffentlichen. Wählen Sie aus **dem Startmenü** hinzugefügt werden soll die app veröffentlichen. Möchten Sie den Pfad für die Anwendung anzugeben, geben Sie einen Namen für die Anwendung und den Installationsort auf dem Pfad.

## <a name="step-4-configure-user-access"></a>Schritt 4: Konfigurieren von Benutzerzugriff ##

Ihre Sammlung erstellt haben, müssen Sie die Benutzer hinzufügen, die Sie remote Ressourcen verwenden möchten. Bei Verwendung von Active Directory zum Benutzer bereit zu dem Abonnement zugeordneten Active Directory-Mieter sein müssen Sie dieser Auflistung erstellen.

1.  Klicken Sie auf Schnellstart auf **Benutzerzugriff konfigurieren**. 
2.  Geben Sie Arbeit Konto (Active Directory) oder Microsoft-Konto, das Sie Zugriff gewähren möchten.

    **Hinweise:** 

    Stellen Sie sicher, dass die “user@domain.com” Format.

    Wenn Sie Office 365 ProPlus in der Auflistung verwenden, müssen Sie die Active Directory-Identität für Ihre Benutzer verwenden. Dadurch werden Lizenzen überprüfen. 

3.  Nachdem der Benutzer überprüft werden, klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte ##

Das ist alles - erfolgreich erstellt und Ihre Azure RemoteApp Cloud-Sammlung bereitgestellt haben. Im nächste Schritt soll die Benutzer Remote Desktop Client herunterladen und installieren. Sie finden die URL für den Client auf Azure RemoteApp Quick Start. Dann haben Sie Benutzer der Client anmelden und Zugriff auf apps, die Sie veröffentlicht.

### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen helfen 
Wussten Sie, dass neben diesen Artikel Bewertung und Kommentare unten unten, Sie Artikel selbst ändern können? Fehlt etwas? Etwas? Habe ich etwas schreiben, die nur verwirrend? Bildlauf nach oben und auf Änderungen **auf GitHub bearbeiten** - die Überprüfung zu uns kommen und danach, sobald wir sie abzeichnen, sehen Sie Ihre Änderung und Verbesserung hier.
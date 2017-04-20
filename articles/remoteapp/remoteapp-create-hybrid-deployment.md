<properties
    pageTitle="Erstellen Sie eine Hybrid-Sammlung für Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie eine Bereitstellung von RemoteApp zu erstellen, die mit dem internen Netzwerk verbindet."
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

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Erstellen Sie eine Hybrid-Sammlung für Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Es gibt zwei Arten von Azure RemoteApp-Sammlungen:

- Cloud: befindet sich vollständig in Azure. Sie können alle Daten in der Cloud speichern (so eine Auflistung nur Cloud) oder zu Ihrer Sammlung an ein VNET Daten speichern.   
- Hybrid: enthält ein virtuelles Netzwerk für lokalen Zugriff – dies erfordert die Verwendung von Azure AD und eine lokale Active Directory-Umgebung.

Müssen nicht wissen? Überprüfen Sie, [welche Art von Auflistung benötigen Sie für Azure RemoteApp](remoteapp-collections.md).

Dieses Lernprogramm führt Sie durch den Prozess der Erstellung einer Hybrid-Auflistung. Es gibt acht Schritte:

1.  Entscheiden Sie, welche [Bild](remoteapp-imageoptions.md) für die Auflistung. Sie können ein benutzerdefiniertes Bild erstellen oder Verwenden eines Microsoft Bilder in Ihrem Abonnement enthalten.
2. Virtuelles Netzwerk einrichten. Überprüfen Sie die Informationen [VNET planen](remoteapp-planvnet.md) und [Größe](remoteapp-vnetsizing.md) .
2.  Erstellen Sie eine Auflistung.
2.  Die Auflistung der lokalen Domäne beitreten.
3.  Fügen Sie ein Vorlagenbild-Auflistung hinzu.
4.  Konfigurieren Sie Synchronisierung. Azure RemoteApp muss integrieren Sie Azure Active Directory entweder 1) Konfiguration von Azure Active Directory-Synchronisierung mit Kennwort synchronisieren oder 2) Konfiguration von Azure Active Directory-Synchronisierung ohne Kennwort Synchronisierungsoption einer Domäne, die AD FS verbunden ist. Checken Sie die [Konfigurationsinformationen für Active Directory RemoteApp](remoteapp-ad.md).
5.  RemoteApp-apps zu veröffentlichen.
6.  Konfigurieren Sie Benutzerzugriff.

**Bevor Sie beginnen**

Sie müssen vor dem Erstellen der Sammlung folgendermaßen:

- [Melden Sie sich](https://azure.microsoft.com/services/remoteapp/) für Azure RemoteApp.
- Erstellen Sie ein Benutzerkonto in Active Directory als Azure RemoteApp-Dienstkonto verwenden. Schränken Sie die Berechtigungen für dieses Konto so ein, dass nur Computer in der Domäne beitreten können.
- Informationen über das lokale Netzwerk: IP-Adresse, Informationen und VPN-Geräte.
- [Azure PowerShell](../powershell-install-configure.md) -Modul zu installieren.
- Informationen Sie über den Benutzer Zugriff zu gewähren. Sie benötigen den Azure Active Directory-Benutzerprinzipalnamen (z. B. name@contoso.com) für jeden Benutzer. Stellen Sie sicher, dass der UPN zwischen Azure entspricht und Active Directory.
- Wählen Sie Ihre Vorlagenbild. Ein Azure RemoteApp Vorlagenbild enthält apps und Programme, die Sie für Ihre Benutzer veröffentlichen möchten. Weitere Informationen finden Sie in [Azure RemoteApp Bildoptionen](remoteapp-imageoptions.md) .
- Office 365 ProPlus Bild verwenden? Überprüfen Sie Informationen [hier](remoteapp-officesubscription.md).
- [Konfigurieren von Active Directory für RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Schritt 1: Richten Sie Ihr virtuelles Netzwerk
Eine Hybrid-Auflistung, die eine vorhandene Azure virtuelle Netzwerk bereitstellen oder ein neues virtuelles Netzwerk erstellen. Ein virtuelles Netzwerk kann Benutzer Access-Daten in Ihrem lokalen Netzwerk über RemoteApp Remoteressourcen. Ein Azure virtuelles Netzwerk gibt den direkten Netzwerkzugriff Auflistung zu anderen Azure und virtuelle Computer, virtuelle Netzwerk bereitgestellt.

Stellen Sie sicher, [VNET planen](remoteapp-planvnet.md) und [VNET Größe](remoteapp-vnetsizing.md) Informationen überprüfen, bevor Sie Ihr VNET erstellen.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Erstellen Sie ein Azure-VNET und verknüpfen Sie ihn mit der Active Directory-Bereitstellung

Zunächst erstellen ein [virtuelles Netzwerk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Dies geschieht auf der Registerkarte **Netzwerk** in Azure-Portal. Sie müssen Ihrem Mandanten Azure Active Directory synchronisiert die Active Directory-Bereitstellung virtuelle Netzwerk herstellen.

Weitere Informationen finden Sie unter [erstellen ein virtuelles Netzwerks mit Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Sicherstellen Sie, dass Ihr virtuelle Netzwerk für Azure RemoteApp bereit ist
Ihre Sammlung Wir stellen sicher, dass Ihr neue virtuelle Netzwerk bereit ist. Sie können dies folgendermaßen überprüfen:

1. Erstellen eines Azure virtuellen Computers im Subnetz des virtuellen Netzwerks erstellten für RemoteApp.
2. Verwenden Sie Remotedesktop Verbindung mit dem virtuellen Computer. (Klicken Sie auf **Verbinden**.)
3. Verknüpfen Sie ihn mit der gleichen Active Directory-Bereitstellung für RemoteApp verwenden möchten.

Funktioniert das? Virtuelles Netzwerk und Subnetz sind für Azure RemoteApp bereit!

Weitere Informationen zu Azure virtuelle Computer erstellen und eine Verbindung zu ihnen Remote Desktop [hier](https://msdn.microsoft.com/library/azure/jj156003.aspx)finden.

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Schritt 2: Erstellen einer Azure RemoteApp-Auflistung ##



1. Wechseln Sie in [Azure-Portal](http://manage.windowsazure.com)zu Azure RemoteApp-Seite.
2. Klicken Sie auf **Neu > vnet erstellen**.
3. Geben Sie einen Namen für die Auflistung.
4. Wählen Sie den Plan - standard oder Standard verwenden möchten.
5. Wählen Sie das VNET aus der Dropdown-Liste und die Subnetzmaske.
6. Wählen Sie Ihre Domäne hinzufügen.
5. Klicken Sie auf **Sammlung RemoteApp zu erstellen**.

Nach dem Erstellen der Auflistung Azure RemoteApp Doppelklicken Sie auf den Namen der Auflistung. **Quick Start** -Seite bringt -, in dem Sie die Auflistung konfiguriert haben.

Gehen etwas schief? Überprüfen Sie die [Problembehandlung Hybrid-Informationen](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Schritt 3: Verknüpfen Sie Ihre Sammlung mit der lokalen Domäne ##


1. Klicken Sie auf der Seite **Quick Start** **Mitglied einer lokalen Domäne**.
2. Azure RemoteApp-Dienstkonto der lokalen Active Directory-Domäne hinzufügen. Sie benötigen den Domänennamen, Organisationseinheit Dienst Benutzernamen und Kennwort.

    Dies ist die Informationen gesammelt, wenn die Schritte in [Active Directory für Azure RemoteApp konfigurieren](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Schritt 4: Link zu einem Bild Azure RemoteApp ##

Ein Azure RemoteApp Vorlagenbild enthält Programme, die Sie für Benutzer freigeben möchten. Neues [Vorlagenbild](remoteapp-imageoptions.md) erstellen oder ein vorhandenes Bild (ein bereits importiert oder in Azure RemoteApp hochgeladen) verknüpfen. Sie können auch eines der Azure RemoteApp [Vorlage Bilder](remoteapp-images.md) mit Office 365 oder Office 2013 (Testphase) Programme verknüpfen.

Wenn Sie das neue Bild hochladen, müssen Sie den Namen und Speicherort für das Bild auswählen. Auf der nächsten Seite des Assistenten Sie PowerShell-Cmdlets - Kopie finden und führen diese Cmdlets aus einer erweiterten Windows PowerShell gefragt werden, ob das angegebene Bild.

Wenn Sie eine vorhandene Vorlagenbild verknüpfen, geben Sie einfach Bildname, Speicherort und zugehörigen Azure-Abonnement.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Schritt 5: Konfigurieren von Active Directory Verzeichnissynchronisation ##

Azure RemoteApp muss integrieren Sie Azure Active Directory entweder 1) Konfiguration von Azure Active Directory-Synchronisierung mit Kennwort synchronisieren oder 2) Konfiguration von Azure Active Directory-Synchronisierung ohne Kennwort Synchronisierungsoption einer Domäne, die AD FS verbunden ist.

Auschecken [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) - dieser Artikel hilft Ihnen beim Einrichten Verzeichnisintegration in 4 Schritten.

Anzeigen Sie [Directory Synchronization Roadmap](http://msdn.microsoft.com//library/azure/hh967642.aspx) für die Planung von Informationen sowie detaillierte Schritte

## <a name="step-6-publish-apps"></a>Schritt 6: Apps veröffentlichen ##

Azure RemoteApp app ist die app oder Programm, das Sie für Ihre Benutzer. Das Vorlagenbild befindet sich im hochgeladenen für die Auflistung. Wenn ein Benutzer eine Anwendung zugreift, wird in ihrer Umgebung ausgeführt, aber wirklich in Azure ausgeführt wird.

Bevor Benutzer apps zugreifen können, Sie diese veröffentlichen müssen können Zugriff auf Benutzer apps über Remote Desktop Client.

Sie können die Auflistung mehrere apps veröffentlichen. Wählen Sie die Veröffentlichungsseite **Veröffentlichen** eine Anwendung hinzufügen. Sie können entweder über **das Startmenü des Vorlagenbilds oder die Pfadangabe für die Anwendung auf dem** veröffentlichen. Wenn Sie aus **dem Startmenü** hinzufügen möchten, wählen Sie hinzugefügt. Möchten Sie den Pfad für die Anwendung anzugeben, geben Sie einen Namen für die Anwendung und den Installationsort auf dem Pfad.

## <a name="step-7-configure-user-access"></a>Schritt 7: Benutzerzugriff konfigurieren ##

Ihre Sammlung erstellt haben, müssen Sie die Benutzer hinzufügen, die Sie remote Ressourcen verwenden möchten. Benutzer, Zugriff in dem Abonnement zugeordneten Active Directory-Mandanten vorhanden sein müssen, den Sie dieser Azure RemoteApp-Sammlung verwendet.

1.  Klicken Sie auf Schnellstart auf **Benutzerzugriff konfigurieren**.
2.  Geben Sie Arbeit Konto (Active Directory) oder Microsoft-Konto, das Sie Zugriff gewähren möchten.

    **Hinweise:**

    Stellen Sie sicher, dass die “user@domain.com” Format.

    Wenn Sie Office 365 ProPlus in der Auflistung verwenden, müssen Sie die Active Directory-Identität für Ihre Benutzer verwenden. Dadurch werden Lizenzen überprüfen.


3.  Sobald der Benutzer überprüft werden, klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte ##
Das ist alles - erfolgreich erstellt und Ihre Azure RemoteApp Hybrid-Sammlung bereitgestellt haben. Im nächste Schritt soll die Benutzer Remote Desktop Client herunterladen und installieren. Sie finden die URL für den Client auf Azure RemoteApp Quick Start. Dann haben Sie Benutzer der Client anmelden und Zugriff auf apps, die Sie veröffentlicht.



### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen helfen
Wussten Sie, dass neben diesen Artikel Bewertung und Kommentare unten unten, Sie Artikel selbst ändern können? Fehlt etwas? Etwas? Habe ich etwas schreiben, die nur verwirrend? Bildlauf nach oben und auf Änderungen **auf GitHub bearbeiten** - die Überprüfung zu uns kommen und danach, sobald wir sie abzeichnen, sehen Sie Ihre Änderung und Verbesserung hier.

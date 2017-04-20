
<properties 
    pageTitle="Verwendung der Office 365-Abonnement mit Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Ihr Office 365-Abonnement in Azure RemoteApp verwenden, Office apps freigeben."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Verwendung der Office 365-Abonnement mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wussten Sie, dass Sie Ihre Office 365-Abonnement in Azure RemoteApp gemeinsam nutzen können Office-apps in die Cloud? Lesen Informationen über Ihre Office 365 + Azure RemoteApp-Optionen, stellen einschließlich Links zu Artikeln über Office 365 von Abonnements.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Verwendung Office 365 Konten für Azure RemoteApp?
Peter neue Artikel alle Informationen: [wie Azure RemoteApp mit Office 365 Benutzerkonten](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Kann ich mein Office 365-Abonnement zur Ausführung von Office Anwendung in Azure RemoteApp verwenden?

Ja! Mit Office 365-Abonnement ist sogar die einzige Möglichkeit zu einer Office-Anwendung zu Azure RemoteApp.

(Hinweis: wird die Bereitstellung Azure RemoteApp Hosting-Partner, möglicherweise mit einem [Service Provider Licensing Agreement](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)Office Lizenzen bereitstellen)


Der große Vorteil von Office 365-Abonnement ist können Sie die gleichen Benutzerlizenz auf vielen verschiedenen Plattformen und einschließlich der Azure-Cloud-Umgebung verwenden. Bei Verwendung von Office-Anwendung in Azure RemoteApp müssen Sie zusätzliche Lizenzen erwerben oder Ihre vorhandenen Lizenzen besondere Weise konfigurieren. Alles ist ein Office 365-Abonnement, die [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx)enthält.

Office 365 ProPlus ermöglicht [freigegebenen Computer](https://technet.microsoft.com/library/Dn782860.aspx) – dieses Leistungsmerkmal temporäre-basierte Aktivierung für Office in virtuellen und Cloud-Umgebungen wie Azure RemoteApp (Remote Desktop Services).

Die Office 365-Pläne enthalten Office 365 ProPlus? Die [Verfügbarkeit von Diensten innerhalb jeder](https://technet.microsoft.com/library/office-365-plan-options.aspx) Tabelle anzeigen Beachten Sie, dass nicht alle Office 365 ProPlus (z. B. Office 365 Business Plan). Ihr Plan nicht in Erwägung einen Plan (z. B. Office 365 Education E3) ist.

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, so wie mein Office 365 ProPlus Lizenzen Azure RemoteApp verwendet?

Jede Benutzerlizenz für Office 365 ProPlus kann einen einzelnen Benutzer Office-Clientanwendungen auf bis zu 5 Computern und Tablets und Telefone aktivieren. Jede Aktivierung ist für den Benutzer registriert, bis sie Office auf dem Gerät deaktivieren. (Benutzer können ihre Geräte im [Office 365-Portal](https://portal.office365.com/)verwalten.)

Azure RemoteApp kann ein einzelner Benutzer mehrere Computer am selben Tag Anmeldung ohne Ihr Wissen. Deshalb, weil der Dienst automatisch verwaltet und Ressourcen in der Cloud skaliert, während der Benutzer sieht nur apps und Programme, die Sie freigegeben haben. Für dieses Szenario Office 365 ProPlus einen freigegebenen Computer Aktivierungsmodus bietet-dies bedeutet, dass Benutzer für alle Lizenzmanagement Zugriff auf diese Ressourcen und, muss nicht zählen Computer nicht gegen die maximal 5 Aktivierung.

Als Sie (Admin) Benutzer Office 365 ProPlus Lizenzen zuweisen, können sie Office auf ihren persönlichen Geräten sowie Ihre Azure RemoteApp-Auflistung verwenden.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Die Office-Programme können mit Office 365 und Azure RemoteApp werden verwendet?

Ihr Office 365-Abonnement können Sie aktivieren und Office 2013 in Azure RemoteApp-Bereitstellung. Wir unterstützen derzeit nicht die Verwendung anderer Versionen von Office Azure RemoteApp. Dies umfasst Office 2003, Office 2007, Office 2010 und Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Visio Pro oder Projekt Pro?

Office 365 ProPlus Bild in Ihrem Abonnement RemoteApp enthalten enthält Visio Pro und Pro Projekt. Aber können Sie Ihr Office 365 ProPlus Abonnement dieser Programme aktivieren - sie haben jeweils eigene Lizenz. Sie können sie in [Office 365-Portal](https://portal.office365.com/)aktivieren. 

Sie müssen diese Programme lizenzieren, wenn Sie sie verwenden möchten. Aktivieren Sie nur zu verwenden - und andere Programme. Sie sehen sie im Bild, jedoch können Sie nicht. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Wie mit Office 365 und Azure RemoteApp beginne ich?

Jetzt wissen die Details der Office 365-Lizenzierung, lassen Sie die Schritte in Azure RemoteApp mit - ist sehr einfach:

Beim Erstellen von Azure RemoteApp-Auflistung verwenden Sie **Office 365 ProPlus (Abonnement erforderlich)** Bild.

![Azure RemoteApp-Bild mit Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Dieses Bild enthält die neueste Version von Windows Server und Office 365 ProPlus. Nachdem Sie Ihre Sammlung (einschließlich publishing apps), die Benutzer einfach konfiguriert Azure RemoteApp (mithilfe ihrer RemoteApp-Client) anmelden und ihre Office 365-Anmeldeinformationen für alle Office-apps. Lizenzen werden automatisch übertragen ohne jede oder Management erforderlich.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Kann ich ein benutzerdefiniertes Bild mit Office 365 ProPlus erstellen?

Sie können ein benutzerdefiniertes Bild für die Auflistung erstellen, die Office 365 ProPlus enthält. 2 Auswahlmöglichkeiten vorhanden sind - Azure Galeriebild bieten wir verwenden oder ein eigenes benutzerdefinierte Bild erstellen und installieren es Office 365 ProPlus.

### <a name="use-the-azure-gallery-image"></a>Azure Galeriebild verwenden

Die einfachste Methode zum Bereitstellen von Office 365 ProPlus eine Auflistung ist [eine Azure Galeriebilder](remoteapp-image-on-azurevm.md) in Azure RemoteApp-Abonnement enthalten. Stellen Sie sicher, dass **Windows Server Remote Desktop Session Host mit Office 365 ProPlus vorinstalliert** Bild auswählen. Installieren gewünschten apps auf das Bild und Sie können loslegen.

### <a name="use-a-custom-image"></a>Verwenden Sie ein benutzerdefiniertes Bild

Immer können ein benutzerdefiniertes Bild - ein [Azure VM](remoteapp-image-on-azurevm.md) oder [erstellen Bild lokal](remoteapp-create-custom-image.md) und in Azure hochladen. Stellen Sie in beiden Fällen installieren Sie Office 365 ProPlus Knoten Aktivierung gemeinsam genutzten Computer verwenden. Verwenden Sie das [Office-Bereitstellungstool](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) und folgen Sie der [Anleitung](https://technet.microsoft.com/library/Dn782858.aspx) für die Installation.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Deaktivieren von automatischen Updates für Office 365 ProPlus in Ihr benutzerdefiniertes Abbild - wichtig

Das benutzerdefinierte Abbild Azure RemoteApp als Vorlage dient zum Hinzufügen von zusätzlichen Ressourcen Nachfrage aus der Benutzer. Um Verzögerungen und Probleme zu vermeiden, deaktivieren Sie automatische Updates für Office im Bild. Wenn Sie nicht aktualisiert jede Ressource mit dieser Vorlage erstellte automatisch Wenn es gestartet wird. Verwenden Sie stattdessen Azure RemoteApp Standardprozess für das benutzerdefinierte Abbild aktualisieren. So aktualisieren die Office-Programme auf das Vorlagenbild und lassen Sie Azure RemoteApp kümmern Aktualisierungen an Ihre Benutzer.

Deaktivieren Sie automatische Updates Office Deployment Tool-Konfigurationsdatei fügen Sie Folgendes hinzu:

        <Updates Enabled="FALSE" />

Die Konfigurationsdatei sollte nun folgenden Zeilen enthalten:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Wie kann ich ein Bild mit Office 365 ProPlus aktualisieren?

Es gibt viele Gründe, um das Bild in Ihrer Sammlung zu aktualisieren. Hier sind ein paar:

- Abrufen der neuesten Windows-updates 
- Erhalten Sie die neuesten Updates für Office 365 ProPlus-Anwendung
- Aktualisieren Sie Ihre benutzerdefinierte Anwendung
- Andere Konfigurationen für das eigentliche Bild ändern

Die End-to-End-Schritte zum Aktualisieren Ihrer Sammlung, benutzen Sie aktualisiert, finden Sie [hier](remoteapp-update.md). Aber finden Sie folgende Informationen zum Bild und Office 365 ProPlus aktualisieren.

Sie haben zwei Optionen für die Aktualisierung der Image - ersetzen Sie Ihr Bild mit einer vollständig neuen oder manuell aktualisieren das vorhandene Bild.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Ersetzen Sie das Bild mit neuesten Azure Galeriebild + fügen Anpassungen hinzu
Mit dieser Option können Sie Microsoft Windows Server und Office 365 ProPlus-Updates kümmern. Statt das vorhandene Bild aktualisieren, erstellen Sie ein vollständig neues Bild basierend auf dem aktuellen Bild. Wiederholen Sie alle Schritte zum Anpassen des Image zuvor-Installation benutzerdefinierte apps Bild Konfiguration usw. ändern.

Galeriebilder werden regelmäßig aktualisiert, so einfach können Sie wissen, dass Ihre Windows Server und Office 365 ProPlus apps sind. Natürlich ist der Nachteil, dass Anpassungen anwenden, jedes Mal ein neues Bild erhalten haben. Skripts zum Automatisieren von Anpassungen festlegen können.

### <a name="manually-update-your-existing-image"></a>Das vorhandene Bild manuell aktualisieren

Mit dieser Option verwenden Sie standard Windows Updates auf das Bild angewendet. Verwenden Sie für Office 365 ProPlus Office Deployment Tool herunterladen und Installieren der neuesten Updates oder Versionen von Office 365 ProPlus.

> [AZURE.IMPORTANT] Denken Sie daran: Office 365 ProPlus automatische Updates deaktivieren.

Informationen Weitere zur Verwendung des Office-Bereitstellungstools für Updates?

- [Bereitstellen Sie Klick-und-Los für Office 365-Produkte mithilfe des Office-Bereitstellungstools](https://technet.microsoft.com/library/JJ219423.aspx)
- [Bereitstellung und Aktualisierung von Office 365 ProPlus Office Deployment Tool mit](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (Video)
- [Update für Office 365 ProPlus konfigurieren](https://technet.microsoft.com/library/dn761708.aspx)

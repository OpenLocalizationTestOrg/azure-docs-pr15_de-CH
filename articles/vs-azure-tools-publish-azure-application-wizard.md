<properties 
   pageTitle="Azure-Assistenten veröffentlichen | Microsoft Azure"
   description="Webpublishing-Assistent Azure-Anwendung"
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

# <a name="publish-azure-application-wizard"></a>Webpublishing-Assistent Azure-Anwendung

## <a name="overview"></a>Übersicht

Nach dem Entwickeln einer Webanwendung in Visual Studio können Sie Anwendung leichter Azure Cloud-Dienst mit dem **Azure-Anwendung veröffentlichen** -Assistenten veröffentlichen. Im erste Abschnitt werden die Schritte erläutert, die Sie ausführen müssen, bevor Sie den Assistenten verwenden und die übrigen Abschnitte die Funktionen des Assistenten erläutern.

>[AZURE.NOTE] Dieses Thema wird Cloud-Dienste nicht auf Websites bereitstellen. Informationen zum Bereitstellen von Websites finden Sie unter [Azure-Website bereitstellen](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor dem Veröffentlichen der Webanwendung in Azure benötigen Sie ein Microsoft-Konto und Azure-Abonnement und Azure-Clouddienst Web-Anwendung zugeordnet sind. Wenn Sie diese Aufgaben bereits abgeschlossen haben, können Sie mit dem nächsten Abschnitt überspringen.

1. Ein Microsoft-Konto und Azure-Abonnement zu erhalten. Versuchen Sie ein kostenloses Azure kostenlos einen Monat Abonnement [hier](https://azure.microsoft.com/pricing/free-trial/)

1. Erstellen Sie einen Cloud-Dienst und ein Speicherkonto in Azure. Dies ist im Server-Explorer in Visual Studio oder mit dem [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)möglich.

1. Aktivieren Sie die Webanwendung für Azure. Zum Aktivieren von Webtests aus Visual Studio Azure veröffentlicht werden soll, müssen Sie ein Azure Cloud Service-Projekt in Visual Studio zugeordnet. Erstellen Sie zugeordnete Cloud-Dienstprojekt öffnen Sie im Kontextmenü des Projekts für die Webanwendung, und wählen Sie konvertieren, **Konvertieren in Azure Cloud Service Project**.

1. Nachdem Sie der Projektmappe das Projekt für den Cloud-Dienst hinzugefügt, öffnen Sie das Kontextmenü erneut und dann **Veröffentlichen**. Weitere Informationen zum Aktivieren der Anträge Azure finden Sie [wie: Migrieren und veröffentlichen Sie eine Anwendung von Azure Cloud Service von Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Achten Sie darauf, starten Sie Visual Studio mit Administratorrechten (als Administrator ausführen).

1. Wenn Sie eine Anwendung veröffentlichen möchten, öffnen Sie das Kontextmenü für das Azure-Cloud-Dienstprojekt und wählen Sie dann **Veröffentlichen**. Die folgenden Schritte zeigen den Azure-Anwendung Veröffentlichen-Assistenten.

## <a name="choosing-your-subscription"></a>Ihr Abonnement auswählen

### <a name="to-choose-a-subscription"></a>Wählen Sie ein Abonnement

1. Bevor Sie den Assistenten zum ersten Mal verwenden, müssen Sie sich anmelden. Wählen Sie den Link **Anmelden** . Aufforderung Azure-Portal anmelden und Ihr Azure Benutzernamen und Kennwort. 

    ![Dies ist eine publishing-Assistenten](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    Die Liste der Abonnements werden Ihrem Konto zugeordneten Abonnements. Möglicherweise auch Abonnements Abonnementdateien angezeigt, die Sie zuvor importiert.

1. Wählen Sie in der Liste zu **Ihrem Abonnement** das Abonnement für diese Bereitstellung.

   Wählen Sie **< verwalten... >** **Abonnements verwalten** -Dialogfeld wird angezeigt und können das Abonnement und das Benutzerkonto, das Sie verwenden möchten. Zeigt die Registerkarte **Konten** alle Konten, und die Registerkarte **Abonnements** werden alle Konten zugeordneten Abonnements. Sie können auch einen Bereich aus dem Azure Ressourcen, erstellen oder importieren Sie Zertifikate für Ihr Abonnement von Azure-Portal. Abonnements von Abonnementdatei importiert wird, erscheint die zugeordneten Zertifikate Registerkarte **Zertifikate** . Wenn Sie fertig sind, wählen Sie die Schaltfläche **Schließen** .

    ![Abonnements verwalten](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Abonnementdatei kann mehrere Abonnements enthalten.

1. Wählen Sie **Weiter** , um fortzufahren. 

    Ihr Abonnement nicht Cloud-Services, müssen Sie einen Cloud-Dienst in Azure Ihr Projekt erstellen. **Cloud-Dienst erstellen und Storage** -Dialogfeld wird angezeigt.

    Geben Sie einen neuen Namen für den Clouddienst. Der Name muss in Azure eindeutig sein. Geben Sie eine Region oder eine Gruppe für ein Rechenzentrum, die Sie oder die meisten Ihrer Kunden. Dieser Name wird auch ein neues Storage-Konto verwendet, Azure für den Clouddienst erstellt.

1. Ändern Sie eine Einstellung für die Bereitstellung und das **Veröffentlichen** klicken (im nächste Abschnitt enthält weitere Informationen zu den verschiedenen) veröffentlichen. Überprüfen Sie die Einstellungen vor der Veröffentlichung wählen Sie **Weiter** .

    >[AZURE.NOTE] Wenn Sie in diesem Schritt veröffentlichen ausgewählt haben, können Sie den Status dieser Bereitstellung in Visual Studio überwachen.

Allgemeine und erweiterte Einstellungen für eine Bereitstellung können mithilfe des Assistenten zum **Veröffentlichen von Azure-Anwendung** . Beispielsweise können Sie eine Einstellung, um die Anwendung in einer Umgebung bereitstellen, bevor Sie freigeben. Die folgende Abbildung zeigt die Registerkarte **Einstellungen** für Azure-Bereitstellung.

![Allgemeine Einstellung](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Konfigurieren der Einstellungen veröffentlichen

### <a name="to-configure-the-publish-settings"></a>Konfigurieren der Veröffentlichung

1. Führen Sie in der Liste **Cloud-Dienst** einen der folgenden Schritte aus:

   1. Wählen Sie im Dropdown-Listenfeld einen vorhandenen Cloud-Dienst. Data Center-Speicherort für den Dienst wird angezeigt. Notieren Sie sich diesen Speicherort, und stellen sicher, dass Ihr Konto Speicherort im selben Rechenzentrum.

    1. Wählen Sie **Neu erstellen** , einen Clouddienst Azure erstellen. Im Dialogfeld **Cloud-Dienst erstellen** einen Namen für den Dienst, und geben Sie dann eine Region oder eine Gruppe an den Speicherort des Rechenzentrums, die zum Cloud-Dienst hosten. Der Name muss in Azure eindeutig sein.

1. Wählen Sie in der **Umgebung** **Produktion** oder **Staging**. Wählen Sie die Stagingumgebung, wenn Sie die Anwendung in einer Umgebung bereitstellen. Sie können Ihre Anwendung später in dieser Umgebung verschieben.

1. Wählen Sie in der **Buildkonfiguration** **Debug** oder **Release**.

1. Wählen Sie in der **Dienstkonfiguration** **Cloud** oder **lokal**.

    Wählen Sie das Kontrollkästchen **Remotedesktop für alle Rollen aktivieren** möchten Sie Remote mit dem Dienst herstellen können. Diese Option wird hauptsächlich zur Problembehandlung verwendet. Wenn Sie dieses Kontrollkästchen aktivieren, wird das Dialogfeld **Konfiguration des Remotedesktop** . Wählen Sie der Hyperlink zum Ändern der Konfiguration aus

    Aktivieren Sie das Kontrollkästchen **Aktivieren Web Deploy für alle Webrollen** Webentwicklung für den Dienst aktivieren. Sie müssen Remotedesktop diese Funktion aktivieren. Weitere Informationen finden Sie unter [[Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). Weitere Informationen zu Web Deploy finden Sie unter [[Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx).

1. Wählen Sie die Registerkarte **Advanced Settings** . Übernehmen Sie den Standardnamen, oder geben Sie einen Namen Ihrer Wahl im Feld **Bezeichnung Bereitstellung** . Das Datum der Bereitstellung Bezeichnung an lassen Sie das Kontrollkästchen aktiviert

    ![Dritter Bildschirm des Veröffentlichen-Assistenten](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Wählen Sie in der **Speicherkonto** Speicherkonto für die Bereitstellung verwenden. Vergleichen Sie die Standorte der Rechenzentren für den Clouddienst und das Speicherkonto. Im Idealfall sollten diese Speicherorte übereinstimmen.

    >[AZURE.NOTE] Azure Speicherkonto speichert das Paket für die Bereitstellung der Anwendung. Nach der Bereitstellung wird das Paket aus dem Speicherkonto entfernt.

1. Aktivieren Sie das Kontrollkästchen **Bereitstellung aktualisieren** , ggf. aktualisierte Komponenten bereitstellen. Diese Art der Bereitstellung kann schneller als eine vollständige Bereitstellung. Wählen Sie den Link **Einstellungen** auf **Bereitstellung update Standardeinstellungen** im Dialogfeld in der folgenden Abbildung dargestellt. 

    ![Bereitstellungseigenschaften](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Sie können eine der beiden Optionen für die Bereitstellung von inkrementellen oder gleichzeitig. Inkrementelle Bereitstellung aktualisiert einer bereitgestellte Instanz gleichzeitig, sodass die Anwendung online und für Benutzer verfügbar bleiben. Gleichzeitige Bereitstellung aktualisiert aller bereitgestellte Instanzen gleichzeitig. Bei Auswahl dieser Option die Anwendung möglicherweise nicht zur Verfügung während des Aktualisierungsvorgangs gleichzeitiger Aktualisierung ist schneller als inkrementelle Aktualisierung

    Wenn Bereitstellung aktualisiert werden kann, aktivieren Sie das Kontrollkästchen für sollten tun eine vollständige Bereitstellung die vollständige Bereitstellung zu automatisch schlägt eine Bereitstellung. Eine vollständige Implementierung setzt die virtuelle IP-Adresse (VIP) Adresse für Cloud-Dienst. Weitere Informationen finden Sie unter [wie: beibehalten Konstante virtuelle IP-Adresse für einen Cloud-Dienst](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Um den Dienst zu debuggen, **IntelliTrace aktivieren** das Kontrollkästchen oder **Debug** Configuration bereitstellen und Ihre Azure-Clouddienst Debuggen Kontrollkästchen Sie das **Remotedebugger für alle Rollen aktivieren** remote Debugdienste bereitstellen.

2. Um das Anwendungsprofil **Profiler-Lauf ermöglichen** das Kontrollkästchen und wählen Sie **der Hyperlink zeigt die Profilerstellungsdaten Optionen** . 


    >[AZURE.NOTE] Verwenden Sie Visual Studio Ultimate um IntelliTrace oder Tier Interaktion Profiling (TIP) aktivieren und Sie nicht beide gleichzeitig aktivieren.

    Weitere Informationen finden Sie in [einem Clouddienst veröffentlicht IntelliTrace mit Visual Studio debuggen](https://msdn.microsoft.com/library/azure/ff683671.aspx) und [Testen der Leistung eines Cloud-Diensts](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Wählen Sie **Weiter** die Zusammenfassungsseite für die Anwendung anzuzeigen.

## <a name="publishing-your-application"></a>Veröffentlichen der Anwendung

1. Sie können ein Veröffentlichungsprofil Einstellungen erstellen, die Sie ausgewählt haben. Sie können beispielsweise ein Profil für eine und eine für die Produktion erstellen. Um dieses Profil zu speichern, wählen Sie das Symbol **Speichern** . Der Assistent erstellt das Profil und im Visual Studio-Projekt gespeichert. Um den Profilnamen zu ändern, öffnen Sie die Liste **Zielprofil** und wählen Sie **< verwalten... >**.

    ![Zusammenfassung des Assistenten](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Publishing Profil erscheint im Projektmappen-Explorer in Visual Studio und die Einstellungen in eine Datei mit der Erweiterung .azurePubxml geschrieben. Als Attribute von XML-Tags werden gespeichert.

1. Wählen Sie **Veröffentlichen** , veröffentlichen die Anwendung. Sie können den Prozessstatus in **das Ausgabefenster in Visual Studio** überwachen.

## <a name="see-also"></a>Siehe auch

[Gewusst wie: Migrieren und Veröffentlichen einer Webanwendung ein Azure-Cloud-Dienst von Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Veröffentlichen einer Cloud-Dienst mithilfe der Azure-Tools](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Veröffentlichte Clouddienst Visual Studio mit IntelliTrace Debuggen](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Testen der Leistung eines Cloud-Diensts](https://msdn.microsoft.com/library/azure/hh369930.aspx)


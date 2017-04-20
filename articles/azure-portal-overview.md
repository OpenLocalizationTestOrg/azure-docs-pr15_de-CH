<properties
    pageTitle="Übersicht über Microsoft Azure-portal"
    description="Informationen Sie zum Microsoft Azure-Portal verwenden."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Übersicht über Microsoft Azure-portal

Microsoft Azure-Portal ist ein zentraler Ort bereitstellen und Azure Ressourcen.  In diesem Lernprogramm im Portal vertraut und zeigen, wie Sie einige wichtige Funktionen verwenden:
- Eine **umfassende Marketplace** , mit dem Sie durchsuchen Sie Tausende von Elementen von Microsoft und anderen Anbietern, die gekauft und/oder bereitgestellt.
- Ein **einheitliches und skalierbares durchsuchen Erfahrung** , der leicht zu Ressourcen kümmern und verschiedene Management-Vorgänge durchführen.
- **Konsistente Management-Seiten** (oder Blätter), mit denen Sie das Verwalten von Azure verschiedener Dienste über ein konsistentes Einstellungen, Aktionen, Rechnungsadresse Informationen, Überwachung und Nutzung Daten, und viele mehr.
- Eine **Persönliche Erfahrung** , mit der Sie eine benutzerdefinierte Startseite erstellen, die die Informationen angezeigt, die Sie bei jedem anzeigen möchten, Sie angemeldet.  Sie können auch eine Management-Blades anpassen, die Kacheln enthalten.

 ![Azure Portal UI-Ausrichtung][UIOrientation]

## <a name="before-you-get-started"></a>Bevor Sie beginnen

Sie benötigen ein gültiges Azure Abonnement für dieses Lernprogramm durchgehen.  Haben Sie eine dann [Melden Sie sich für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) noch heute.  Haben Sie ein Abonnement, können Sie das Portal unter [https://portal.azure.com] zugreifen.

## <a name="how-to-create-a-resource"></a>Zum Erstellen einer Ressource

Azure hat einen Markt mit Tausenden von Elementen, die von einem Ort zu erstellen.  Angenommen, eine neue Windows Server 2012 VM erstellen möchten.  Der + neue Hub ist der Einstiegspunkt in kuratierte mehrere besondere Kategorien vom Marketplace.  Jede Kategorie hat wenigen Artikel mit einem Link zum vollständigen Markt, der alle Kategorien und Suche angezeigt. Erstellen Sie die neue Windows Server 2012 VM Aktionen:  

1.  Windows Server 2012 ist vorhanden, der Compute-Kategorie auswählen.  
2.  Füllen Sie einige grundlegenden Eingaben in einem Formular.
3.  Klicken Sie auf 'Erstellen' und die VM startet sofort bereitstellen.

Hub Benachrichtigung informiert, wenn die Ressource erstellt wurde und Management Blade öffnet (immer finden Sie Ressourcen weiter unten).

![Portal Kategorien][PortalCategories]


## <a name="how-to-find-your-resources"></a>So finden Sie Ihre Ressourcen

Genutzten Ressourcen kann immer zu Ihrem Startmenü angeheftet, aber möglicherweise etwas zu suchen, die Sie häufig zugreifen.  Unten durchsuchen-Hub ist zu allen Ressourcen.  Sie können Abonnements Filtern Spalten auswählen/Größe und navigieren zu der Management-Blades für einzelne Elemente.

![Hub durchsuchen][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Wie verwalten und Zugriff auf eine Ressource

Dieses Blatt Verbinden mit dem virtuellen Computer mithilfe von Remotedesktop wichtige Leistungsindikatoren überwachen, Steuern des Zugriffs auf diesem virtuellen Computer mit rollenbasierter Zugriff (RBAC) Konfigurieren der VM und andere wichtige Aufgaben.  Delegieren des Zugriffs basierend auf Rolle ist Ebene verwalten.  Klicken Sie [hier](./active-directory/role-based-access-control-configure.md) Weitere Informationen. Um Zugriff auf eine Ressource zu delegieren, durchführen Sie die folgenden Aktionen:

1.  Wechseln Sie zu der Ressource.
2.  Klicken Sie auf "alle" im Abschnitt Essentials.
3.  Klicken Sie auf 'Benutzer' in der Liste.
4.  Klicken Sie auf 'Hinzufügen' in der Befehlszeile.
5.  Wählen Sie einen Benutzer und eine Rolle.

![Verwalten einer Ressource][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Eine Ressource Blade anpassen

Azure voreinstellt Blades für Ressourcen, aber auf diese sind Ihr Steuerelement.  Sie können problemlos wechseln in Modus zum Hinzufügen, entfernen, ändern Sie die Größe anpassen oder Kacheln anpassen. Gehen Sie zum Anpassen einer-Blades:

1.  Wechseln Sie zu der Ressource.
2.  Klicken Sie auf die "..." am Anfang des Blades möchten Sie anpassen.
3.  Klicken Sie auf "Elemente hinzufügen".
4.  Ziehen und Ablegen von Webparts zu starten.  

![Anpassen von Blades][CustomizeBlades]

## <a name="how-to-get-help"></a>So erhalten Sie Hilfe

Wenn Sie ein Problem haben, sind wir für Sie.  Das Portal hat eine Seite Hilfe und Support, die Sie in die richtige Richtung zeigen.  Je nach Ihrem [unterstützen](https://azure.microsoft.com/support/plans/)können Sie Supportanfragen auch direkt im Portal erstellen.  Support-Ticket erstellen, können Sie den Lebenszyklus des Tickets im Portal verwalten. Sie können Hilfe und Supportseite durchsuchen navigieren -> Hilfe und Support.  

![Hilfe und support][HelpSupport]

## <a name="summary"></a>Zusammenfassung

Betrachten Sie in diesem Lernprogramm haben gelernt:
- Sie haben gelernt, wie anmelden, erhalten und auf das Portal durchsuchen
- Sie mit dem Portal Benutzeroberfläche und gelernt erstellen und Ressourcen
- Haben Sie gelernt, erstellen Sie eine Ressource und Ressourcen
- Haben Sie Informationen über die Struktur oder Management-Blades und wie Sie verschiedene Ressourcen konsistent verwalten können
- Sie haben gelernt, wie das Portal die Informationen anpassen interessiert an der Vorderseite und
- Haben Sie gelernt, Steuerung des Zugriffs auf Ressourcen mit rollenbasierter Zugriff (RBAC)
- Haben Sie gelernt, Hilfe und support

Microsoft Azure-Portal vereinfacht radikal erstellen und verwalten Ihre Anwendung in der Cloud.  Betrachten Sie [Management Blog](https://azure.microsoft.com/blog/topics/management/) Stand wie wir ständig [Feedback überwachen](https://feedback.azure.com/forums/223579-azure-preview-portal/) und verbessern.  [ScottGu Blog](http://weblogs.asp.net/scottgu) lädt eine andere Azure Updates suchen.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png

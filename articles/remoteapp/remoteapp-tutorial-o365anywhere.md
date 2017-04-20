<properties
   pageTitle="Erleben Sie dieselbe Office 365 Gerät Azure RemoteApp | Microsoft Azure"
   description="Erfahren Sie, wie Benutzer mithilfe von Azure RemoteApp alle Office 365-app freigeben."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Erleben Sie dieselbe Office 365 Gerät Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Dieser Artikel umfasst Office 365 auf alle Geräte in Ihrem Unternehmen bereitstellen. Die Benutzer können die Funktionen und die Benutzeroberfläche für Android, Apple und Windows abrufen.

Wir erreichen dies mit Azure RemoteApp mit Office 365 für Skalierung können virtuelle Computer in Azure Benutzer zugreifen können. Diese Gruppe von virtuellen Computern rufen wir eine cloudsammlung"".

## <a name="create-a-cloud-collection"></a>Erstellen Sie eine cloudsammlung

Nachdem Sie ein Azure-Konto erstellt haben, navigieren Sie zuerst zu **RemoteApp** durch Klicken auf den Link auf der linken Seite.
![Anzeigen von Azure RemoteApp Azure-Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Fahren Sie durch Klicken auf **neue** der unteren und "schnell" eine Auflistung. Geben Sie einen Namen, Bereich, Abonnements, Plan und das Bild "Office Professional 2013", das wir bereitstellen.
![Dialogfeld erstellen](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Nach Abschluss sollte der Erstellungsprozess Auflistung beginnen. Dies kann bis zu einer Stunde dauern.

![Warten](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Sobald der Vorgang abgeschlossen ist, wird es wie folgt aussehen. Wir **Veröffentlichen** klicken, sehen wir, dass die meisten Office-Programme für uns bereits veröffentlicht wurden.
![Erstellt wurde](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Veröffentlichte apps](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Zu diesem Zeitpunkt können Sie weitere Benutzer hinzufügen, die Zugriff auf diese Auflistung von **Benutzerzugriff**auf.
![Benutzerzugriff konfigurieren](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Nun versuchen wir, mit Office 365!

## <a name="connect-to-office-365"></a>Verbinden mit Office 365

Wir gehen Sie zu [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), scrollen Sie nach unten und klicken Sie auf Azure RemoteApp-Client auf dem Gerät installieren Sie **Clients herunterladen** . Die folgenden Screenshots sind für Windows.

Nach der Anwendung Start werden Sie aufgefordert, melden Sie sich mit Ihrem Microsoft-Konto (früher eine "Live ID" genannt) verwenden dasselbe als Azure-Konto jetzt. Wenn Sie angemeldet sind sollten in Sie eine Benachrichtigung über neue Einladung klicken und sollten angezeigt wie unten. Die Einladung, die Ihre Azure-Konto Besitzer e-Mail übereinstimmt.

![Neue Einladung](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

So sieht es bei neue Einladungen.

![Akzeptieren einer Anwendung](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Nachdem Sie die Einladung annehmen sollte alle Office-apps in Azure RemoteApp-Client angezeigt werden.

![Liste der apps](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Wenn Sie auf sollte diese Anwendung auf Azure VM starten sollten alle! Viel Spaß!

![Starten](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)

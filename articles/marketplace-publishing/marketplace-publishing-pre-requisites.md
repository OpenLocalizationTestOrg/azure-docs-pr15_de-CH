<properties
   pageTitle="Technischen Komponenten für das Erstellen eines Angebots für den Azure | Microsoft Azure"
   description="Verstehen der Anforderungen zum Erstellen und Bereitstellen eines Angebots Azure Marketplace für andere erwerben."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="08/18/2016"
  ms.author="hascipio"/>

# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Allgemeine Komponenten zum Erstellen eines Angebots für Azure Marketplace
Verstehen Sie allgemeinen, Business-prozessorientierten erforderlichen Komponenten, die erforderlich sind, mit den Erstellungsprozess bieten.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Stellen Sie sicher, dass Sie als Verkäufer bei Microsoft registriert
Detaillierte Informationen zum ein Verkäuferkonto bei Microsoft registrieren zur [Erstellung und Registrierung](marketplace-publishing-accounts-creation-registration.md).

- **Wenn Ihr Unternehmen bereits als Verkäufer im Developer Center registriert und ein neues Angebot erstellen möchten** und dann bei der Veröffentlichung Portal mit derselben e-Mail-Id mit der Entwicklungscenter Registrierung erfolgt. Dieser Schritt ist erforderlich, damit Portal Developer Center und Veröffentlichung sind miteinander verknüpft.
- **Wenn Ihr Unternehmen bereits als Verkäufer im Developer Center registriert und ein vorhandenes Angebot bearbeiten möchten** und klicken Sie dann entweder bei der Veröffentlichung Portal mit dem Administratorkonto oder ein Konto als co-Administrator die Veröffentlichung hinzugefügt Portal. Hinzufügen der co-Administratorkonto wird unten angegeben.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Co-Administrator im Veröffentlichungsportal hinzufügen
Administratoren der Veröffentlichung Portal können andere Mitglieder des Unternehmens in der Anwendung arbeiten, als co-Administrator die Veröffentlichung Portal. **Vorausgesetzt, dass Sie der Administrator** unter werden die Schritte zum Hinzufügen einer co-Admin

>[AZURE.NOTE] Für neue Benutzer vor dem Hinzufügen einer co-Administrator die Veröffentlichung Portal, sicherzustellen, dass Sie mindestens eine Anwendung, die Veröffentlichung erstellt haben Portal. Dies ist erforderlich, da die Registerkarte **Verleger** erscheinen erst mindestens eine Anwendung erstellen, die Veröffentlichung Portal.

1. Sicherstellen Sie, dass die co-Admin e-Mail-Id ein Microsoft account(MSA). Wenn dies nicht der Fall ist, wie eine MSA mit diesem [Link](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1)registrieren.
2. Sicherstellen Sie, dass mindestens eine Anwendung unter dem Administratorkonto vor dem Hinzufügen einer co-Admin.
3. Nachdem die Schritte erfolgen bei der Veröffentlichung der co-Admin e-Mail-Id und Abmelden Portal.
4. Jetzt melden die Veröffentlichung mit der Id Admin e-Mail-Portal.
5. Navigieren Sie zum Herausgeber wählen Sie Ihr Konto -> ->-Administratoren hinzufügen co-Admin (Abbildung untenstehende) >

    ![Zeichnen](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)

6. Sicherstellen Sie, dass die verschiedenen Phasen der Veröffentlichungsvorgang (z.B. Entwicklungscenter, Portal veröffentlichen) zur e-Mail-Ids für die Kommunikation von Microsoft überwacht werden.
7. Entwicklungscenter Registrierung verwenden Sie ein Konto mit einer Person. Dies ist empfehlenswert, für eine einzelne Abhängigkeit entfernt.
8. Wenn Sie mit Entwicklungscenter Registrierung Probleme lösen Sie ein Ticket mit diesem [Link](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Co-Administrator die Veröffentlichung löschen Portal
**Vorausgesetzt, dass Sie der Administrator sind** nachstehend sind die Schritte zum Löschen einer co-Admin

1. Bei der Veröffentlichung mit der Id Admin e-Mail-Portal.
2. Navigieren Sie zu **Publisher** -wählen Sie Ihr Konto- **Administratoren**> > -> **Co-Administratoren**.
3. Klicken Sie auf die Schaltfläche **X** Tot löschen (Abbildung untenstehende) soll co-Admin.

    ![Zeichnen](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [AZURE.IMPORTANT] Sie müssen keinen steuern und Firmendaten abschließen, wenn Sie planen, nur Angebote (oder eine eigene Lizenz).

> Die Eintragung als Unternehmen muss zunächst abgeschlossen werden. Allerdings arbeitet Ihr Unternehmen Informationen steuern und im Microsoft Developer-Konto Entwickler können beginnen im [Veröffentlichungsportal](https://publish.windowsazure.com)darin zertifiziert, VM-Image Erstellen und in die Stagingumgebung Azure testen. Sie benötigen vollständige Verkäufer Konto Genehmigung nur für den letzten Schritt veröffentlichen Sie Ihr Angebot in Azure Marketplace.

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>"Pay" Azure-Abonnement erwerben
Dies ist das Abonnement mit der VM-Abbilder erstellen und die Bilder an [Azure Marketplace](https://azure.microsoft.com/marketplace/). Wenn Sie nicht über ein Abonnement verfügen, dann melden Sie sich unter https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Verkauf von" Ländern
> [AZURE.WARNING]
Um Ihre Dienste auf Azure zu verkaufen, müssen sicherstellen, dass die registrierte Entität aus der genehmigten "Verkaufen von" ist. Diese Einschränkung ist für die Auszahlung und steuern. Wir suchen aktiv erweitern dieser Länder in naher Zukunft, so bleiben Sie dran. Eine vollständige Liste finden Sie in Abschnitt 1 b [Azure Marketplace Beteiligung Richtlinien](http://go.microsoft.com/fwlink/?LinkID=526833).

## <a name="next-steps"></a>Nächste Schritte
Nach die erforderlichen technischen Komponenten vorliegen sind das Angebot bestimmten technischen Komponenten. Klicken Sie auf den Artikel für den jeweiligen Angebot ein, dem für den Azure erstellen möchten.

- [VM technischen erforderlichen Komponenten](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Lösung Vorlage technischen erforderlichen Komponenten](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md)

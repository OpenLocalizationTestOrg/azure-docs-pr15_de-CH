<properties
   pageTitle="Anleitung zum Erstellen einer Lösungsvorlage für den Markt | Microsoft Azure"
   description="Ausführliche Informationen zum Erstellen, Zertifizierung und Multi-VM-Vorlagenbild Lösung für die Bereitstellung auf Azure Marketplace erwerben."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Anleitung zum Erstellen einer Lösungsvorlage für Azure
Nach Schritt 1, [Erstellung und Registrierung][link-acct-creation], leiten wir Sie bei der Erstellung einer Vorlage Azure-kompatible Lösung zur [technischen Komponenten zum Erstellen einer Lösungsvorlage](marketplace-publishing-solution-template-creation-prerequisites.md). Nachdem wir Sie durch die Schritte zum Erstellen einer Lösungsvorlage für mehrere VMs [Veröffentlichungsportal] gehen[ link-pubportal] für Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Erstellen Sie Ihre Vorlage Lösungsangebots im Portal veröffentlichen
Gehen Sie zu [https://publish.windowsazure.com](http://publish.windowsazure.com). Bei der Anmeldung zum ersten Mal [Veröffentlichungsportal](https://publish.windowsazure.com/)verwenden Sie dasselbe Konto, das Ihr Firmenprofil Verkäufer registriert wurde. Später können Sie als co-Administrator im Veröffentlichungsportal Mitarbeiter Ihres Unternehmens hinzufügen.

### <a name="1-select-solution-templates"></a>1. Wählen Sie "Lösungsvorlagen"

  ![Zeichnen][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. erstellen Sie eine neue Projektmappenvorlage

  ![Zeichnen][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Starten Sie mit Topologien
Eine Projektmappenvorlage ist "Parent" aller seiner Topologien. Sie können mehrere Topologien in einem Angebot-Lösung Vorlage definieren. Ein Angebot zum Staging abgelegt, mit dessen Topologien verschoben. Gehen Sie folgendermaßen vor, um Ihr Angebot zu definieren:     

- Erstellen einer Topologie: "Topologie Bezeichner" ist in der Regel den Namen der Topologie für die Lösungsvorlage. Topologie-ID wird in der URL wie folgt:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure-Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Neue Version hinzufügen.

### <a name="4-get-your-topology-versions-certified"></a>4. beziehen Sie Ihre Topologie Versionen zertifiziert
Laden Sie eine Zip-Datei, die alle erforderliche Dateien zum Bereitstellen dieser speziellen Version der Topologie enthält. Diese Zip-Datei muss Folgendes enthalten:

- *mainTemplate.json* und *createUiDefinition.json* Datei Root-Verzeichnis.
- Alle verknüpften Vorlagen und alle erforderlichen Skripts.

  > [AZURE.TIP] Während Entwickler auf Vorlage Topologien die Projektmappe erstellen und sie zertifizierte Unternehmen, können Marketing- und rechtlichen Inhalt marketing und juristische Abteilungen Ihres Unternehmens arbeiten.

## <a name="next-steps"></a>Nächste Schritte
Ihre Lösungsvorlage erstellt und die Zip-Datei hochgeladen führen Sie bitte die in den [Markt Marketinginformationen führen](marketplace-publishing-push-to-staging.md) vor Angebot bereitstellen. Den vollständigen Satz von Marketplace Artikel finden Sie auf [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md).

Sie können auch diesen Artikeln interessiert.

- VM-Bilder: [Zu virtuellen Computerimages in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- VM-Erweiterungen: [VM und VM Extensions Overview](https://msdn.microsoft.com/library/azure/dn832621.aspx) und [Azure VM und Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure Ressourcenmanager: [Azure ARM-Vorlagen erstellen](../resource-group-authoring-templates.md) und [Beispiele für einfache ARM-Vorlage](https://github.com/rjmax/ArmExamples)
- Speicherkonto Steuerung: [How to Monitor für Storage-Konto Drosselung](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) und [Premium-Speicher](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com

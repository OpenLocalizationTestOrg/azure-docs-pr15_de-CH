<properties
   pageTitle="Schnellstartübersicht Azure Security Center | Microsoft Azure"
   description="Dieser Artikel hilft Ihnen schnell mit Azure Security Center beginnen, führen Sie durch die Überwachung und Policy Management Sicherheitskomponenten und verknüpfen Sie mit nächste Schritte."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Azure Security Center quick Start-Handbuch

Dieser Artikel hilft Ihnen schnell mit Azure Security Center zunächst führen Sie durch die Überwachung und Policy Management Sicherheitskomponenten Security Center.

> [AZURE.NOTE] Dieser Artikel stellt den Dienst mithilfe einer beispielbereitstellung. Dieser Artikel ist nicht Schritt.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst mit Sicherheit müssen Sie ein Abonnement für Microsoft Azure. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)anmelden.

Die kostenlose Sicherheitscenter automatisch mit Ihrem Abonnement aktiviert und bietet einen Einblick in den Sicherheitsstatus Ihrer Azure-Ressourcen. Bietet grundlegende Verwaltung der Sicherheitsrichtlinien, Sicherheitsaspekte und Integration Sicherheitsprodukte und-Services von Azure Partnern.

Das Sicherheitscenter wird aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. Weitere Informationen zu Azure-Portal finden Sie unter [Portal-Dokumentation](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Datensammlung

Sicherheitscenter sammelt Daten von der virtuellen Maschinen (VMs) ihres Sicherheitsstatus bewerten, Sicherheitsaspekte und Sie auf Gefahren hinweisen. Beim Aufrufen von Security Center VMs in Ihrem Abonnement Datensammlung aktiviert. Datensammlung wird empfohlen, aber Teilnahme Datensammlung in der Sicherheitscenter deaktivieren.

Die folgenden Schritte beschreiben, wie und die Komponenten des Security Center. In diesen Schritten zeigen wir Ihnen Daten deaktivieren Sie kündigen möchten.

## <a name="access-security-center"></a>Access Security Center

Gehen Sie im Portal Security Center zugreifen:

1. Wählen Sie im **Microsoft Azure** **Security Center**.
![Azure-Menü][1]

2. Wenn Sie Security Center zum ersten Mal zugreifen, wird das **Willkommen** Blatt geöffnet. Wählen Sie Ja **! Ich möchte starten Azure Security Center** Blatt **Security Center** zu öffnen und Daten.
![Willkommenseite][10]

3. Nach dem Starten Sie Security Center über Willkommensseite Blatts oder Menü Sicherheitscenter Microsoft Azure **Security Center** Blade geöffnet Einfachen Zugriff auf das **Security Center** -Blade in Zukunft die Option **Pin Blade Dashboard** (oben rechts).
![PIN-Blade Dashboard-Option][2]

## <a name="use-security-center"></a>Verwenden Sie das Sicherheitscenter

Sie können Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen. Wir Konfigurieren einer Sicherheitsrichtlinie für Ihr Abonnement:

1. Blatt **Security Center** wählen Sie die Kachel **Richtlinie** .
![Sicherheitsrichtlinien][3]

2. Wählen Sie ein Abonnement-Blade **Sicherheitsrichtlinien - Politik pro Abonnement oder Ressource** .
3. Blade **-Sicherheitsrichtlinie** ist **Sammlung** aktiviert Protokolle automatisch sammeln. Die Überwachung Erweiterung wird auf alle aktuellen und neuen VMs im Abonnement bereitgestellt. (Datensammlung deaktivieren, indem **Daten** auf **Off**, aber dadurch Sicherheitscenter Sicherheitshinweise und Vorschläge zu geben.)
4. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** **ein Speicherkonto pro Region aus**. Für jede Region, in der virtuelle Computer ausgeführt haben, wählen Sie das Speicherkonto Daten die VMs Speicherort. Wenn Sie ein Speicherkonto für jede Region nicht auswählen, wird sie für Sie erstellt. Die gesammelten Daten ist logisch von anderen Kunden Daten aus Sicherheitsgründen isoliert.

     > [AZURE.NOTE] Wir empfehlen Datensammlung aktivieren und ein Speicherkonto auf Abonnementebene zuerst auswählen. Sicherheitsrichtlinien können auf der Azure-Abonnement und Ressource Gruppenebene festlegen, Konfiguration der Datensammlung und Speicherkonto tritt allerdings am Abonnement-Level.

5. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** **Prevention-Richtlinie aus** Daraufhin wird das Blatt **Prevention** .
![Prevention-Richtlinien][4]

6. Aktivieren Sie auf Blade **Intrusionspräventions-Richtlinie** Vorschläge, die Sie als Teil der Sicherheitsrichtlinie anzeigen möchten. Beispiele:

 - Einrichten von **System-Updates** **auf** scannt alle unterstützten virtuellen Computer auf fehlende Updates OS.
 - Festlegen von **Betriebssystem-Sicherheitslücken** **auf** scannt alle unterstützten virtuellen Computer OS Konfigurationen identifizieren, die den virtuellen Computer anfällig für Angriffe machen.

### <a name="view-recommendations"></a>Vorschläge anzeigen

1. Blatt **Security Center** zurück und wählen Sie das Musterelement **Recommendations** . Sicherheitscenter analysiert regelmäßig den Sicherheitsstatus Ihrer Azure-Ressourcen. Beim Sicherheitscenter Sicherheitslücken bezeichnet, wird auf die **Empfehlung** Recommendations.
![Recommendations in Azure Security Center][5]

2.  Wählen Sie eine Empfehlung **empfohlenen** Blade mehr Informationen anzuzeigen oder Maßnahmen zur Behebung des Problems.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Ansichtszustand Gesundheit und Sicherheit der Ressourcen

1.  Zurück zum Blatt **Security Center** . Die **Sicherheitsstatus Ressourcen** Kachel enthält Indikatoren für den Sicherheitsstatus für virtuelle Computer, Netzwerke, Daten und Programme.
2.  Wählen Sie **virtuellen Maschinen** um weitere Informationen anzuzeigen. **Virtuelle Computer** Blade wird geöffnet und zeigt eine statuszusammenfassung Antimalware-Programme, System-Updates, Neustart und OS Schwachstellen Ihrer virtuellen Computer.
![Ressourcen Health Kachel in Azure Security Center][6]

3.  Wählen Sie eine Empfehlung unter **VIRTUAL MACHINE RECOMMENDATIONS** Informationen anzuzeigen oder Maßnahmen erforderlichen Steuerelemente konfigurieren.
4.  Wählen Sie eine VM unter **virtuellen Maschinen** um zusätzliche Details anzuzeigen.

### <a name="view-security-alerts"></a>Anzeigen von Sicherheitshinweisen

1.  Blatt **Security Center** zurück und wählen Sie das Musterelement **Sicherheitshinweise** . **Sicherheitshinweise** Blade wird geöffnet und zeigt eine Liste der Warnungen. Sicherheitscenter Analyse der Sicherheitsprotokolle und Netzwerkaktivität wird diese Warnung generiert. Alerts von integrated Partner Solutions sind enthalten.
![Sicherheitshinweise in Azure Security Center][7]

    > [AZURE.NOTE] Sicherheitshinweise sind nur verfügbar, wenn die Standard-Stufe des Sicherheitscenters aktiviert ist. Eine 90-Tage-Testversion von Standard Ebene steht. Weitere Informationen wie die Standard-Stufe [Weiter](#next-steps) zu.

2.  Wählen Sie eine Warnung, um weitere Informationen anzuzeigen. In diesem Beispiel wählen wir **binäre geändert-System erkannt**. Blades, die weitere Details zur jeweiligen Warnung bereitstellen wird geöffnet.
![Sicherheit Warnungsdetails in Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Zeigen Sie den Zustand von Ihrem Partner solutions

1. Zurück zum Blatt **Security Center** . Die Kachel **Partner Solutions** können Sie auf einen Blick den Status Ihrer Partner-Projektmappen integriert Azure-Abonnement überwachen.
2. Wählen Sie das Musterelement **Partner Solutions** . Eine-Blade wird geöffnet und zeigt eine Liste der Partner Solutions Security Center verbunden.
![Partner solutions][9]

3. Wählen Sie eine partnerlösung. In diesem Beispiel wählen wir **F5 WAF** Lösung.  Eine-Blade wird geöffnet und zeigt den Status der Projektmappe zugeordneten Ressourcen und Partner. Wählen Sie **Projektmappe Konsole** Erfahrung der Partner für diese Lösung öffnen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel vorgestellt Überwachung und Policy Management Sicherheitskomponenten von Security Center. Da Sie Security Center kennen, führen Sie die folgenden Schritte aus:

- Konfigurieren Sie eine Sicherheitsrichtlinie für Ihr Azure-Abonnement. Informationen finden Sie unter [Einrichten von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md).
- Mithilfe der Empfehlung im Sicherheitscenter Azure Ressourcen schützen. Informationen finden Sie unter [Managing Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md).
- Überprüfen Sie und verwalten Sie Ihre aktuelle Sicherheitshinweise. Weitere finden Sie unter [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md).
- Weitere Informationen über die [erweiterten Features zur Erkennung von Bedrohung](security-center-detection-capabilities.md) , die mit [Standard-Stufe](security-center-pricing.md) des Sicherheitscenters. Eine 90-Tage-Testversion von Standard Ebene steht.
- Haben Sie Fragen zur Verwendung von Security Center finden Sie [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png

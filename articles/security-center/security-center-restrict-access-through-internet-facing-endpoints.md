<properties
   pageTitle="Einschränken des Zugriffs über Internetzugriff Endpunkte in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Empfehlung Azure Security Center **schränken Sie den Zugriff über Internet gerichteten Endpunkt**implementiert."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Einschränken des Zugriffs über Internetzugriff Endpunkte in Azure Security Center

Azure Security Center wird empfohlen, Zugriff über Internetzugriff Endpunkte verfügt Ihr Netzwerk-Sicherheitsgruppen (NSGs) eine oder mehrere eingehende Regeln, mit denen Zugriff von 'alle' Quell-IP-Adresse zu beschränken. Auf "" öffnen, können Angreifer Zugriff auf Ihre Ressourcen. Sicherheitscenter wird empfohlen, Sie bearbeiten diese eingehenden Regeln zum Einschränken des Zugriffs auf die Quell-IP-Adressen, die wirklich Zugriff benötigen.

Diese Empfehlung ist für alle nicht-Port generiert, "alle" als Quelle enthält.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung. Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. **Empfohlene Blade**wählen Sie **schränken Sie den Zugriff über Internet gerichteten Endpunkt aus**
![Einschränken des Zugriffs über Endpunkt mit Internetzugriff][1]

2. Das Blade **schränken Sie den Zugriff über Internet gerichteten Endpunkt**wird geöffnet. Dieses Blatt enthält virtuelle Maschinen (VMs) eingehende Regeln, die ein mögliches Sicherheitsproblem erstellen. Wählen Sie einen virtuellen Computer.
![Wählen Sie einen virtuellen Computer][2]

3. **NSG** Blade zeigt Network Security Group, verwandte eingehende Regeln und zugeordnete VM an. Wählen Sie für eine eingehende Regel bearbeiten **Eingehende Regeln bearbeiten** .
![Netzwerk-Sicherheitsgruppe blade][3]

4. Wählen Sie auf der **eingehenden Sicherheitsregeln** eingehende Regel bearbeiten. In diesem Beispiel wählen wir **AllowWeb**.
![Eingehende Sicherheitsregeln][4]

  Beachten Sie auch **Standardregeln** für die Standardregeln enthalten alle NSGs angezeigt auswählen können. Standardregeln können nicht gelöscht werden, da sie eine niedrige Priorität zugewiesen, sie können jedoch überschrieben werden die Regeln, die Sie erstellen. Erfahren Sie mehr über [Standardregeln](../virtual-network/virtual-networks-nsg.md#default-rules).
![Standardregeln][5]

5. Bearbeiten Sie die Eigenschaften der eingehende Regel-Blade **AllowWeb** so, dass die **Quelle** eine IP-Adresse oder IP-Adressen blockieren. Erfahren Sie mehr über die Eigenschaften der eingehenden Regel anzeigen Sie [NSG Regeln](../virtual-network/virtual-networks-nsg.md#nsg-rules)

  ![Eingehende Regel bearbeiten][6]

## <a name="see-also"></a>Siehe auch

Dieser Artikel wurde gezeigt, wie das Sicherheitscenter Empfehlung "Beschränken Zugriff über Internet mit Endpunkt". Erfahren Sie mehr über das Aktivieren der NSGs und Regeln finden Sie hier:

- [Was ist Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [NSGs mit der Azure-Portal verwalten](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md)– Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md)– erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md)erfahren Sie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md)- Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md)- finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)- Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png

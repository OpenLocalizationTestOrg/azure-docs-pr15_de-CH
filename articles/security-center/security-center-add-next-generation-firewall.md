<properties
   pageTitle="Fügen Sie nächste Generation in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Empfehlung Azure Security Center **Hinzufügen einer Firewall der nächsten Generation** und **Route mit Eingangsport durch nur NGFW**implementieren."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Fügen Sie nächste Generation in Azure Security Center

Azure Security Center empfiehlt eine Firewall der nächste Generation (NGFW) von einem Microsoft-Partner hinzufügen, um Ihre Sicherheitsmaßnahmen zu erhöhen. Dieses Dokument führt Sie durch ein Beispiel dazu.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blatt **Recommendations** **Hinzufügen einer Firewall der nächsten Generation**.
![Hinzufügen einer Firewalls der nächsten Generation][1]

2. Wählen Sie einen Endpunkt Blatt **Hinzufügen einer Firewall der nächsten Generation** .
![Wählen Sie einen Endpunkt][2]

3. Eine zweite **Firewall der nächsten Generation hinzufügen** -Blatt wird geöffnet. Sie können eine vorhandene Projektmappe verfügbaren verwenden oder eine neue erstellen. In diesem Beispiel gibt keine vorhandenen Projektmappen, wir eine neue NGFW erstellen.
![Erstellen Sie neue Firewall der nächsten Generation][3]

4. Erstellen Sie eine neue NGFW wählen Sie eine Projektmappe aus der Liste der integrierten Partner. In diesem Beispiel wählen wir **Check Point**.
![Wählen Sie Next Generation Firewall-Lösung][4]

5. **Check Point** -Blade öffnet Informationen über partnerlösung. Wählen Sie in der Blade-Informationen **Erstellen** .
![Firewall Informationen blade][5]

6. **Erstellen virtueller Computer** -Blatt wird geöffnet. Hier können Sie Informationen, einen virtuellen Computer (VM) drehen, die die NGFW ausgeführt wird. Führen Sie die Schritte und Informationen Sie den NGFW benötigt. Wählen Sie OK anwenden.
![Virtuelle Maschine ausführen NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Datenverkehr über NGFW nur

Zurück zu Blade **Recommendations** . Ein neuer Eintrag wurde erstellt, nachdem **Datenverkehr durch NGFW nur**hinzugefügt NGFW Security Center aufgerufen werden. Diese Empfehlung wird nur erstellt, wenn Sie Ihre NGFW Security Center installiert. Haben Internetzugriff Endpunkte werden Security Center empfehlen Network Security Regeln konfigurieren, die eingehenden Datenverkehr für die VM durch Ihre NGFW erzwingen.

1. Wählen Sie im **empfohlenen Blade** **Datenverkehr durch NGFW nur**.
![Datenverkehr über NGFW nur][7]

2. VMs aufgeführt, die zum Datenverkehr kann **Datenverkehr durch NGFW nur** Blade wird geöffnet. Wählen Sie einen virtuellen Computer aus.
![Wählen Sie einen virtuellen Computer][8]

3. Eine Blade zur ausgewählten VM wird geöffnet und verwandte eingehende Regeln. Eine Beschreibung enthält weitere Informationen über mögliche nächste Schritte. Wählen Sie für eine eingehende Regel bearbeiten **Eingehende Regeln bearbeiten** . Erwartung ist die **Quelle** nicht auf **Alle** für Internetzugriff Endpunkte mit der NGFW verknüpft ist. Erfahren Sie mehr über die Eigenschaften der eingehenden Regel anzeigen Sie [NSG Regeln](../virtual-network/virtual-networks-nsg.md#nsg-rules)
![Konfigurieren von Regeln, um den Zugriff einzuschränken][9]
![eingehende Regel bearbeiten][10]

## <a name="see-also"></a>Siehe auch

Dieses Dokument wurde gezeigt, wie das Sicherheitscenter Empfehlung "Hinzufügen einer Firewall der nächsten Generation". Erfahren Sie mehr über NGFWs und Check Point partnerlösung finden Sie hier:

- [Next-Generation Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge über Azure Sicherheit und Compliance.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png

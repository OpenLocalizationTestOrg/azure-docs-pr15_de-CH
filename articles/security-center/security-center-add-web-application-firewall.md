<properties
   pageTitle="Fügen Sie Web-Anwendung in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Empfehlung Azure Security Center **Hinzufügen eine Web Application Firewall** und **Finalize Schutz**implementieren."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Fügen Sie Web-Anwendung in Azure Security Center

Azure Security Center empfiehlt, eine Web Application Firewall (WAF) von einem Microsoft-Partner Sichern einer Anwendung hinzuzufügen. Dieses Dokument führt Sie durch ein Beispiel dazu.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie im Blade **Recommendations** **Secure Webanwendung Web Application Firewall**.
![Sichere Web-Anwendung][1]

2. Wählen Sie Blatt **schützen Ihre Web-Anwendung mit Web Application Firewall** eine Anwendung. **Hinzufügen einer Web Application Firewall** -Blatt wird geöffnet.
![Fügen Sie Web-Anwendung][2]
3. Sie können eine vorhandene Web Application Firewall ggf. verwenden oder eine neue erstellen. In diesem Beispiel gibt keine vorhandenen WAFs, wir eine neue WAF erstellen.

4. Erstellen Sie eine neue WAF wählen Sie eine Projektmappe aus der Liste der integrierten Partner. In diesem Beispiel wählen wir **Barracuda Web Application Firewall**.
5. Das Blade **Barracuda Web Application Firewall** öffnet Informationen über partnerlösung. Wählen Sie in der Blade-Informationen **Erstellen** .
![Firewall Informationen blade][3]

6. Das **Neue Web Application Firewall** Blade geöffnet wird, können Sie Schritte **VM-Konfiguration** und **WAF**Informationen. **VM-Konfiguration**auswählen

7. Blatt **VM-Konfiguration** Geben Sie die erforderlichen virtuellen Computer hochfahren, die die WAF ausgeführt wird.
![VM-Konfiguration][4]
8. Um das **Neue Web Application Firewall** Blade und aktivieren Sie **WAF Informationen**. Konfigurieren Sie **WAF Informationen** Blade WAF selbst. Schritt 7 können Sie den virtuellen Computer konfigurieren, auf dem die WAF ausgeführt und Schritt 8 können Sie die WAF selbst bereitstellen.

## <a name="finalize-application-protection"></a>Anwendungsschutz abschließen

1. Zurück zu Blade **Recommendations** . Ein neuer Eintrag wurde erstellt, nachdem WAF **Anwendungsschutz Finalize**aufgerufen erstellt. Diesen Eintrag können Sie wissen, dass Sie den Prozeß tatsächlich Verkabelung von WAF in Azure Virtual Network, damit die Anwendung schützen können.
![Anwendungsschutz abschließen][5]

2. Wählen Sie **Finalize Anwendungsschutz**. Ein neues Blatt wird geöffnet. Sie sehen, es ist eine Anwendung, die den Datenverkehr umgeleitet haben.
3. Wählen Sie die Anwendung. Eine-Blade wird mit der Schritte zu erhalten, für das Web Application Firewall-Setup wird abgeschlossen. Führen Sie die Schritte aus, und wählen Sie **Datenverkehr einschränken**. Sicherheitscenter führen Sie dann die Verkabelung auf Sie.
![Einschränken des Datenverkehrs][6]

> [AZURE.NOTE] Diese Programme Bereitstellung Ihrer vorhandenen WAF hinzufügen, um mehrere ASP.NET-Webanwendungen im Sicherheitscenter zu schützen. WAF Geräte (erstellt mit dem Ressourcen-Manager-Bereitstellungsmodell) müssen mit einem separaten virtuellen Netzwerk bereitgestellt werden. WAF Geräte (erstellt mit dem klassischen Bereitstellungsmodell) sind mit einer Netzwerk-Sicherheitsgruppe auf. Diese Unterstützung wird in Zukunft individuell Bereitstellung einer WAF Einheit (classic) erweitert. Erfahren Sie mehr über die [klassischen und Ressourcenmanager Bereitstellungsmodelle](../azure-classic-rm.md) für Azure-Ressourcen.

Die Protokolle, WAF sind nun vollständig integriert. Das Sicherheitscenter kann automatisch erfassen und Analysieren der Protokolle, damit es wichtige Sicherheitshinweise Oberfläche kann starten.

## <a name="see-also"></a>Siehe auch

Dieses Dokument wurde gezeigt, wie das Sicherheitscenter Empfehlung "Hinzufügen einer Web-Anwendung". Um weitere Informationen zum Konfigurieren einer Web Application Firewall finden Sie hier:

- [Konfigurieren von Web Application Firewall (AAF) für App Service-Umgebung](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge über Azure Sicherheit und Compliance.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png

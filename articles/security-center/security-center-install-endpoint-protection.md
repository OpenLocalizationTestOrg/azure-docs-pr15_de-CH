<properties
   pageTitle="Endpoint Protection in Azure Security Center installieren | Microsoft Azure"
   description="Dieses Dokument veranschaulicht der Azure Security Center Empfehlung **Endpoint Protection installieren**."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Endpoint Protection in Azure Security Center installieren

Azure Security Center wird empfohlen, dass Antimalware-Programm Ihren Azure virtuellen Maschinen (VMs) bereitstellen, wenn Malware nicht bereits aktiviert ist. Diese Empfehlung gilt nur für Windows-VMs.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blatt **Recommendations** **Endpoint Protection installieren**.
![Wählen Sie Endpoint Protection installieren][1]

2. **Endpoint Protection installieren** Blade wird eine Liste von virtuellen Maschinen ohne Antimalware aktiviert. Wählen Sie aus der Liste der VMs Antimalware auf installieren, und klicken **auf virtuellen Computern installiert**werden soll.
![Wählen Sie VMs Antimalware auf Installieren][2]

3. **Wählen Sie Endpoint Protection** Blade wird geöffnet Antimalware-Lösung auswählen, die Sie verwenden möchten. In diesem Beispiel wählen wir **Microsoft Antimalware**.
![Wählen Sie Endpoint Protection][3]

4. Weitere Informationen über die Antimalware-Lösung wird angezeigt. Wählen Sie **Erstellen**.
![Antimalware-Lösung erstellen][4]

5. Geben Sie die erforderliche Konfiguration auf die **Erweiterung hinzufügen** , und klicken Sie auf **OK**. Erfahren Sie mehr über die Konfigurationsdateien finden Sie unter [standardmäßige und benutzerdefinierte Antimalware-Konfiguration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft Antimalware](../azure-security-antimalware.md) ist jetzt auf ausgewählten VMs.

## <a name="see-also"></a>Siehe auch

Dieser Artikel wurde gezeigt, wie das Sicherheitscenter Empfehlung "Endgeräteschutz installieren". Erfahren Sie mehr über ein Antimalware Programm in Azure finden Sie hier:

- [Microsoft Antimalware für Cloud-Dienste und virtuelle Maschinen](../azure-security-antimalware.md) – Informationen zum Microsoft Antimalware bereitstellen.

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge über Azure Sicherheit und Compliance.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png

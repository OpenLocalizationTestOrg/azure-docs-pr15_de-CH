<properties
   pageTitle="Machen Sie Sicherheit in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Sicherheit Kontaktdaten in Azure Security Center."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="provide-security-contact-details-in-azure-security-center"></a>Machen Sie Sicherheit in Azure Security Center

Azure Security Center wird Sicherheit Kontaktdetails für Ihre Azure-Abonnement bereitzustellen, falls noch nicht empfehlen. Diese Information wird von Microsoft kontaktieren, wenn Microsoft Security Response Center (MSRC) feststellt, dass Ihre Daten durch eine rechtswidrige oder nicht autorisierte zugegriffen wurde verwendet. MSRC Auswahl von Azure-Netzwerk und Infrastruktur Überwachung durchführt und Threat Intelligence und Missbrauch Beschwerden von Dritten erhält.

Eine Benachrichtigung wird vom ersten täglich auftreten einer Warnung und nur für Warnungen mit hoher Priorität gesendet. E-Mail-Optionen können nur für Richtlinien konfiguriert werden. Ressourcengruppen innerhalb eines Abonnements erben diese Einstellung.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blatt **Recommendations** **bieten Sicherheit Kontakt**.
![Sicherheit Kontakt][1]

2. Das Blade **bieten Sicherheit Kontakt**wird geöffnet. Auswählen des Azure-Abonnements auf Kontaktinformationen.
![Machen Sie Sicherheit][2]

3. Eine zweite **Sicherheit Kontaktinformationen** Blatt wird geöffnet.

  - Geben Sie die Sicherheit e-Mail-Adressen durch Kommas getrennt. Es ist nicht auf die Anzahl der e-Mail-Adressen, die Sie eingeben.
  - Geben Sie eine Sicherheit internationale Telefonnummer.
  - Um e-Mails zu hohem Schweregrad Alerts Option **meiner e-Mails über Warnungen**aktivieren.
  - In Zukunft müssen Sie e-Mail-Benachrichtigungen Abonnementbesitzer senden. Diese Option ist derzeit deaktiviert.
  - Wählen Sie **OK** Sicherheitsinformationen zu Ihrem Abonnement angewendet.

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) - Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png

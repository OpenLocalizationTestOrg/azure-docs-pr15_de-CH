<properties
   pageTitle="Transparente Verschlüsselung in Azure Security Center aktivieren | Microsoft Azure"
   description="Dieses Dokument veranschaulicht der Azure Security Center Empfehlung **Transparente Verschlüsselung aktivieren**."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparente Verschlüsselung in Azure Security Center aktivieren

Azure Security Center wird empfohlen, Transparent Data Encryption (DSA) auf SQL-Datenbanken aktivieren, wenn TDE noch nicht aktiviert ist. TDE schützt Ihre Daten und hilft Ihnen Auflagen durch Verschlüsselung der Datenbank zugeordneten Backups und Transaktionsprotokolldateien in Ruhe, ohne Änderung der Anwendung. Finden Sie mehr [Transparente Verschlüsselung in Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096).

Diese Empfehlung gilt für den Azure SQL-Dienst. SQL auf die virtuellen Computer nicht eingeschlossen werden.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie das Blade **Recommendations** **Transparente Verschlüsselung aktivieren**.
![Transparente Verschlüsselung aktivieren][1]

2. Blades **Können transparente Verschlüsselung auf SQL-Datenbanken** wird geöffnet. Wählen Sie eine SQL-Datenbank TDE aktivieren.
![Wählen Sie SQL-DB TDE aktivieren][2]
3. -Blade **transparente Verschlüsselung** wählen **auf** unter Verschlüsselung und **Speichern** im oberen Menüband des Blades.
![TDE aktivieren][3]

  Sobald TDE für die ausgewählte SQL-Datenbank aktiviert ist, wird der **Verschlüsselungsstatus** **verschlüsselt**geändert.    

  ![Verschlüsselung][4]

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie der Security Center-Empfehlung implementiert "Transparente Verschlüsselung aktivieren". Erfahren Sie mehr über SQL TDE finden Sie hier:

- [Transparente Verschlüsselung mit SQL Azure-Datenbank](https://msdn.microsoft.com/library/dn948096)
- [Erste Schritte mit Transparent Data Encryption (DSA)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) - Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png

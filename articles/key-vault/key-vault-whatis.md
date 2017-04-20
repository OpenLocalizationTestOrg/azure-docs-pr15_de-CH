<properties
    pageTitle="Was ist Azure Key Vault? | Microsoft Azure"
    description="Azure Key Vault können kryptografische Schlüssel schützen und Geheimnisse von Cloudanwendungen und Diensten verwendet. Mithilfe von Azure Key Vault können Kunden Schlüssel und geheime Schlüssel (wie Authentifizierungsschlüssel, speicherkontoschlüssel, Verschlüsselungsschlüssel, verschlüsselt. PFX-Dateien und Kennwörter) Tasten, die Hardwaresicherheitsmodule (HSMs) geschützt sind."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Was ist Azure Key Vault?

Azure Key Vault ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter [Key Vault Preisseite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung

Azure Key Vault können kryptografische Schlüssel schützen und Geheimnisse von Cloudanwendungen und Diensten verwendet. Mithilfe von Key Vault können Sie Schlüssel und geheime Schlüssel (wie Authentifizierungsschlüssel, speicherkontoschlüssel, Verschlüsselungsschlüssel, verschlüsseln. PFX-Dateien und Kennwörter) Tasten, die Hardwaresicherheitsmodule (HSMs) geschützt sind. Für zusätzliche Sicherheit können Sie importieren oder in HSMs Schlüssel generieren. Wenn Sie möchten, Ihre Schlüssel im FIPS 140-2 Level 2 überprüfte HSMs (Hardware und Firmware) Microsoft-Prozesse.  

Key Vault rationalisiert die Schlüsselmanagement und ermöglicht Ihnen die Kontrolle über Tasten, die Zugriff auf Ihre Daten verschlüsseln. Entwickler können Schlüssel für Entwicklung und testing in Minuten erstellen und dann nahtlos auf Produktion migrieren. Administratoren können erteilt Berechtigung auf Bedarf und widerrufen.

Verwenden Sie die folgende Tabelle besser verstehen helfen Schlüssel Depot auf die Bedürfnisse von Entwicklern und Administratoren.





| Rolle        | Problemstellung           | Azure-Tresor gelöst  |
| ------------- |-------------|-----|
| Entwickler für eine Azure-Anwendung      | "Schreiben eine Anwendung für Azure, der Schlüssel zum Signieren und Verschlüsseln verwendet werden soll, aber diese Schlüssel externe von meiner Anwendung, so dass die Lösung für die Anwendung geeignet ist, die geografisch verteilt werden soll. <br/><br/>Ich möchte diese Schlüssel und Kennwörter zu ohne den Code selbst schreiben. Ich möchte diese Schlüssel und Kennwörter einfach für mich, meine Anwendung mit optimaler Leistung." | √ Schlüssel in einem Tresor gespeichert und bei Bedarf URI aufgerufen.<br/><br/> √ Schlüssel von Azure mit Industrie-Standard-Algorithmen und Schlüssellängen Hardwaresicherheitsmodule (HSMs) sicher.<br/><br/> √ Schlüssel werden HSMs verarbeitet, die in derselben Azure Rechenzentren als der Anwendung befinden. Dadurch höhere Zuverlässigkeit und geringere Latenz als, wenn die Schlüssel in einem anderen Speicherort, wie lokal vorhanden.|
| Entwickler für Software as a Service (SaaS)      |"Keine Verantwortung oder potentielle Kunden Mieter und Geheimnisse möchte. <br/><br/>Soll die Kunden und ihre Schlüssel verwalten, sodass ich, was am besten tun die Kern-Softwarefunktionen bietet konzentrieren können." | √ Kunden können eigene Schlüssel in Azure importieren und verwalten. Wenn eine SaaS Anwendung kryptografische Operationen mit ihren Kunden, ist Key Vault diese Vorgänge für die Anwendung. Die Anwendung wird nicht der Kunden Schlüssel angezeigt.|
| Chief Security Officer (CSO) | "Ich möchte wissen entsprechen unserer FIPS 140-2 Level 2 HSMs für sichere Schlüsselmanagement. <br/><br/>Ich möchte sicherstellen, dass mein Unternehmen der Lebenszyklus ist und Schlüsselverwendung überwachen kann. <br/><br/>Und obwohl wir mehrere Azure Services und Ressourcen verwenden, die Schlüssel von einem einzigen Ort in Azure verwalten möchte."     |√-HSMs FIPS 140-2 Level 2 überprüft.<br/><br/>√ Schlüssel Depot ist so konzipiert, dass Microsoft nicht sehen oder Ihre Schlüssel zu extrahieren.<br/><br/>√ In Echtzeit Protokollierung verwendet.<br/><br/>√ Das Depot bietet eine Oberfläche, unabhängig davon, wie viele Tresore Sie in Azure, welche Bereiche sie Support und die Anwendung verwenden. |


Jeder Azure-Abonnement erstellen und Verwenden von wichtigen Depots. Obwohl Schlüssel Depot Entwickler und Administratoren profitieren, konnte Sie implementiert und ein Organisations-Administrator, andere Azure-Dienste für eine Organisation verwaltet, verwaltet werden. Erstellen Sie ein Depot für die Organisation, Schlüssel, und dann betriebliche Aufgaben wie Administrator ein Azure-Abonnement anmelden möchten beispielsweise:

+ Erstellen Sie oder importieren Sie eines Schlüssels oder geheimen
+ Sperren Sie oder löschen Sie eines Schlüssels oder Schlüssel
+ Autorisieren von Benutzern oder Anwendungszugriffs auf die wichtigsten Tresor dann verwalten oder ihre Schlüssel und geheime Schlüssel
+ Konfigurieren der Schlüsselverwendung (z. B. signieren oder verschlüsseln)
+ Monitor Schlüsselverwendung

Dieser Administrator würde dann bieten Entwicklern mit URIs ihrer Anwendung aufzurufen und ihren Sicherheitsadministrator Schlüsselverwendung Informationen bieten. 

   ![Übersicht über Azure-Tresor][1]

Entwickler können auch die Tasten direkt verwalten über APIs. Weitere Informationen finden Sie unter [Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).

## <a name="next-steps"></a>Nächste Schritte

Ein immer gestartet Administrator finden Sie unter [Erste Schritte mit Azure Schlüssel](key-vault-get-started.md).

Weitere Informationen zur Verwendung des Key Vault Protokollierung finden Sie unter [Azure Key Vault-Protokollierung](key-vault-logging.md).

Weitere Informationen über Schlüssel und geheime Schlüssel mit Azure finden Sie unter [über Schlüssel, Kennwörter, und Zertifikate](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png

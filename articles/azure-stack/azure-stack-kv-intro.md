<properties
    pageTitle="Einführung in Azure Stack Schlüssel Depot | Microsoft Azure"
    description="Erfahren Sie, wie Azure Stack Schlüssel Depot Schlüssel und Kennwörter verwaltet"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Einführung in die wichtigsten Tresor in Azure Stapel #

## <a name="before-you-start"></a>Bevor Sie beginnen

In diesem Artikel wird Folgendes vorausgesetzt:

- Der Reader greift auf ein Abonnement, das Azure Key Vault aktiviert wurde.
- Azure PowerShell SDK ist konfiguriert und verfügbar.
- TP2 Version können alle Mieter gerichteten Operationen von PowerShell ausgeführt werden.

## <a name="key-vault-basics"></a>Key Vault-Grundlagen

Key Vault in Azure Stapel können kryptografische Schlüssel sichern und Kennwörter, die cloud-Programme und-Dienste verwenden. Mithilfe von Key Vault können Sie Schlüssel und geheime Schlüssel (z. B. Authentifizierungsschlüssel, speicherkontoschlüssel Verschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln.

Key Vault rationalisiert die Schlüsselmanagement und ermöglicht Ihnen die Kontrolle über Tasten, die Zugriff auf Ihre Daten verschlüsseln. Entwickler können Schlüssel für Entwicklung und testing in Minuten erstellen und dann nahtlos auf Produktion migrieren. Administratoren können erteilt Berechtigung auf Bedarf und widerrufen.

Jeder Stapel Azure-Abonnement erstellen und Verwenden von wichtigen Depots. Obwohl Schlüssel Depot Entwickler und Administratoren profitieren kann, andere Azure Stapel Dienste für eine Organisation verwaltet, werden sie implementiert und verwaltet werden. Beispielsweise erstellen Sie dieser Administrator kann ein Stapel Azure-Abonnement anmelden ein Depot für die Organisation, Schlüssel, und dann diese betrieblichen Aufgaben:

- Erstellen Sie oder importieren Sie eines Schlüssels oder geheimen
- Sperren Sie oder löschen Sie eines Schlüssels oder Schlüssel
- Autorisieren von Benutzern oder Anwendungszugriffs auf die wichtigsten Tresor dann verwalten oder ihre Schlüssel und geheime Schlüssel
- Konfigurieren der Schlüsselverwendung (z. B. signieren oder verschlüsseln)

Dieser Administrator dann Entwickler URIs ihrer Anwendung aufzurufen, enthalten, und einen Sicherheitsadministrator Schlüsselverwendung Informationen bieten.

Entwickler können auch die Tasten direkt verwalten über APIs. Weitere Informationen finden Sie unter Key Vault-Entwicklerhandbuch.

## <a name="scenarios"></a>Szenarien

Die folgende Tabelle zeigt einige Szenarien, wo Schlüssel Depot helfen können Entwickler und Administratoren:


### <a name="developer-for-an-azure-stack-application"></a>Entwickler für eine Anwendung Azure Stapel

**Problem**: für Azure eine Anwendung schreiben, die Schlüssel für die Signierung und Verschlüsselung verwendet werden soll, aber ich möchte diese externen von meiner Anwendung so, dass die Lösung für eine Anwendung, die geografisch verteilt ist.

**Anweisung**: Schlüssel im Tresor gelagert und URI bei Bedarf aufgerufen.


### <a name="developer-for-software-as-a-service-saas"></a>Entwickler für Software als Service (SaaS)

**Problem:** Verantwortung oder potentielle soll Kunden Mieter und Geheimnisse nicht.

**Anweisung:** Kunden können ihre eigenen Schlüssel in Azure importieren und verwalten. Ich möchte Kunden und ihre Schlüssel verwalten, sodass ich, was am besten tun die Kern-Softwarefunktionen bietet konzentrieren können.


### <a name="chief-security-officer-cso"></a>Chief Security Officer (CSO)

**Problem:** Ich möchte sicherstellen, dass mein Unternehmen der Lebenszyklus ist und Schlüsselverwendung überwachen kann.

**Anweisung** Key Vault ist so konzipiert, dass Microsoft nicht sehen oder der Schlüssel extrahieren.  Wenn eine Anwendung für kryptografische Operationen mit Kunden, dies Schlüssel Depot für die Anwendung. Die Anwendung wird nicht der Kunden Schlüssel angezeigt.  Obwohl wir mehrere Stapel Azure Services und Ressourcen verwenden, möchten die Tasten von einem einzigen Ort in Azure Stapel verwalten werden. Die Vault bietet eine Oberfläche, unabhängig davon, wie viele Tresore Sie in Azure Stapel, welche Bereiche sie Support und die Anwendung verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte mit Schlüssel](azure-stack-kv-getting-started.md)

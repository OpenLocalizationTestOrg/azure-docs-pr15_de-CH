<properties
   pageTitle="Sicherheit von Azure Automatisierung | Microsoft Azure"
   description="In diesem Artikel Überblick einen Automatisierung Sicherheits- und die verschiedenen Authentifizierungsmethoden für Automatisierung in Azure Automation."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Automatisierung der Sicherheit Sichere Automatisierung" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Sicherheit von Azure Automatisierung
Azure Automation können Sie Aufgaben für Ressourcen in Azure lokal und andere Cloud-Anbietern wie Amazon Web Services (AWS) automatisieren.  Ein Runbook erforderlichen Aktionen ausführen muss er Berechtigungen sicheren Zugriff auf Ressourcen mit minimalen Berechtigungen innerhalb des Abonnements erforderlich sein.  
Dieser Artikel behandelt die verschiedenen Authentifizierungsszenarien Azure Automation unterstützt und erfahren Sie, wie auf die Umgebung oder Umgebung verwalten müssen.  

## <a name="automation-account-overview"></a>Übersicht über Automatisierung
Wenn Sie Azure Automatisierung zum ersten Mal starten, müssen Sie mindestens ein Automation-Konto erstellen. Automation-Konten können Sie die Automatisierung (Runbooks, Ressourcen, Konfigurationen) Isolieren von Ressourcen in anderen automatisierungskonten. Automation-Konten können Ressourcen in separate logische Umgebung getrennt. Beispielsweise können Sie ein Konto für Entwicklung, weitere Produktion und einen anderen für Ihre lokale Umgebung.  Ein Azure Automation-Konto unterscheidet sich von Ihrem Microsoft-Konto oder die Konten in der Azure-Abonnement.

Die Automatisierung Ressourcen für jedes Konto Automatisierung eine Azure Region zugeordnet sind, aber Automation-Konten Ressourcen in jeder Region. Der Hauptgrund automatisierungskonten in verschiedenen Regionen erstellen wäre Sie Richtlinien, die Daten und Ressourcen in einer bestimmten Region isoliert werden.

>[AZURE.NOTE]Automatisierungskonten und Ressourcen enthalten sie in Azure-Portal erstellt werden, kann im klassischen Azure-Portal zugegriffen werden. Wenn diese Konten oder ihre Ressourcen mit Windows PowerShell verwaltet werden soll, verwenden Sie Azure-Ressourcen-Manager-Module.

Alle Aufgaben, die Sie für Ressourcen mit Azure Ressourcenmanager Azure Cmdlets in Azure Automation ausführen muss in Azure Azure Active Directory Organisationseinheiten Identität Anmeldeinformationen basierende Authentifizierung authentifizieren.  Zertifikatbasierte Authentifizierung wurde die ursprüngliche Authentifizierungsmethode für Azure Service-Modus, aber es war kompliziert Setup.  Authentifizierung bei Azure Azure AD-Benutzer wurde eingeführten wieder 2014 nicht nur vereinfachen zum Konfigurieren eines Kontos für die Authentifizierung, sondern auch die Möglichkeit, nicht interaktiv in Azure mit einem einzelnen Benutzerkonto zu authentifizieren, die mit Azure Resource Manager und classic Ressourcen.   

Derzeit beim Erstellen eines neuen Kontos Automatisierung in Azure-Portal wird automatisch erstellt:

-  Ausführen als Konto erstellt einen neuen Dienstprinzipalnamen in Azure Active Directory ein Zertifikat weist die Teilnehmer rollenbasierte Zugriffskontrolle (RBAC) die Ressourcenmanager Ressourcen mit Runbooks verwendet wird.
-  Klassische Ausführen als Konto hochladen Verwaltungszertifikat, mit Azure Service oder classic Ressourcen Runbooks verwalten.  

Rollenbasierte Zugriffskontrolle ist mit Azure Ressourcenmanager erlaubte Aktionen Azure AD-Benutzerkonto als Konto ausführen als, und authentifizieren, Dienstprinzipalnamen verfügbar.  Lesen Sie [Rollenbasierte Zugriffskontrolle in Azure Automation Artikel](../automation/automation-role-based-access-control.md) Weitere Informationen zum Modell für die Verwaltung von Berechtigungen Automatisierung entwickeln.  

Auf eine hybride Runbook Arbeitskraft im Datencenter oder computing Services in AWS Runbooks kann nicht dieselbe Methode verwenden, die normalerweise für die Authentifizierung von Runbooks Azure Ressourcen verwendet wird.  Ist diese Ressourcen von Azure ausgeführt und erfordert daher ihre eigenen Anmeldeinformationen Automatisierung Ressourcen authentifizieren, der auf sie lokal definiert.  

## <a name="authentication-methods"></a>Authentifizierungsmethoden

In der folgenden Tabelle werden die verschiedenen Authentifizierungsmethoden für jede Umgebung von Azure Automation und die Artikel zur Authentifizierung für Ihre Runbooks setup unterstützt zusammengefasst.

-Methode  |  Umgebung  | Artikel
----------|----------|----------
Azure AD-Benutzerkonto | Azure Ressourcenmanager und Azure Servicemanagement | [Runbooks mit Azure AD authentifizieren](../automation/automation-sec-configure-aduser-account.md)
Azure ausführen als Konto | Azure Ressourcenmanager | [Runbooks mit Azure ausführen als Konto authentifizieren](../automation/automation-sec-configure-azure-runas-account.md)
Azure klassische Ausführen als Konto | Azure Servicemanagement | [Runbooks mit Azure ausführen als Konto authentifizieren](../automation/automation-sec-configure-azure-runas-account.md)
Windows-Authentifizierung | Lokalen Datencenter | [Authentifizieren Sie Runbooks Hybrid Runbook Arbeitskräfte](../automation/automation-hybrid-runbook-worker.md)
AWS-Anmeldeinformationen | Amazon WebServices | [Authentifizieren von Runbooks mit Amazon WebServices (AWS)](../automation/automation-sec-configure-aws-account.md)




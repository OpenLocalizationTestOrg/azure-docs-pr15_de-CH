<properties
   pageTitle="Konfigurieren Sie die Authentifizierung mit Amazon WebServices | Microsoft Azure"
   description="Dieser Artikel beschreibt das Erstellen und Überprüfen einer AWS-Anmeldeinformationen für Runbooks in Azure Automation AWS-Ressourcen verwalten."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="AWS Authentifizierung Aws konfigurieren"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Authentifizieren von Runbooks mit Amazon WebServices
Automatisieren häufiger Aufgaben mit Amazon Web Services (AWS) erfolgt mit Automatisierung Runbooks in Azure.  Sie können viele Aufgaben mithilfe von Automatisierung Runbooks, wie Ressourcen in Azure AWS automatisieren.  Nur Dinge zwei:

* Ein Abonnement AWS und einen Satz von Anmeldeinformationen.  Speziell die AWS zugreifen und geheime Schlüssel.  Weitere Informationen finden Sie im Artikel [AWS-Anmeldeinformationen verwenden](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Ein Azure-Abonnement und Automation-Konto.  Weitere Informationen zum Einrichten von Azure Automation-Konto lesen Sie den Artikel [Konfigurieren Azure ausführen als Konto](../automation/automation-sec-configure-azure-runas-account.md).  

Authentifizierung mit AWS Geben Sie AWS Anmeldeinformationen authentifizieren Ihre Runbooks von Azure Automation. Wenn Sie bereits ein automatisierungskonto erstellt und AWS Authentifizierung verwenden möchten, können Sie im folgenden Abschnitt Schritte.  Ggf. ein Konto für Runbooks Anwendung AWS Ressourcen gewidmet sollten Sie zunächst ein neues [Automatisierung ausführen als Konto](../automation/automation-sec-configure-azure-runas-account.md) (überspringen einen Dienstprinzipal zu erstellen) und führen Sie die folgenden Schritte aus.

## <a name="configure-automation-account"></a>Automation-Konto konfigurieren
Azure-Automatisierung mit AWS kommunizieren müssen Sie zunächst die AWS-Anmeldeinformationen abrufen und als Anlagen in Azure Automation.  Gehen Sie im AWS Dokument [Verwalten von Zugriffstasten für Ihr Konto AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) Zugriffstaste erstellen und kopieren **Zugriff Schlüssel-ID** und **Geheimschlüssel** dokumentiert (optional Schlüsseldatei zum sicheren Speichern herunterladen).

Nachdem Sie erstellt und Ihr Sicherheitsschlüssel AWS kopiert haben, müssen Sie eine Anlage Anmeldeinformationen mit einem Azure Automation-Konto sicher speichern und mit der Runbooks erstellen.  Führen Sie die Schritte **zum Erstellen einer neuen Anmeldeinformationen** im Abschnitt [Anmeldeinformationen Anlagen in Azure Automation](../automation/automation-certificates.md/###To create a new credential with the Azure portal) Artikel und geben Sie folgende Informationen:

1. Geben Sie im Feld **Name** **AWScred** oder einen entsprechenden Wert der Benennungsstandards.  
2. Geben Sie im Feld **Benutzername** Ihre **Zugriffs-ID** und den **Geheimschlüssel** im Feld **Kennwort** und **Kennwort bestätigen** .   

## <a name="next-steps"></a>Nächste Schritte

- Reivew Lösung Artikel [automatisieren Bereitstellung eines virtuellen Computers in Amazon Web Services](../automation/automation-scenario-aws-deployment.md) zum Automatisieren von Aufgaben in AWS Runbooks erstellen.

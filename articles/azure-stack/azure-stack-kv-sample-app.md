<properties
    pageTitle="Anwendung Revtrieve Azure Stack Schlüssel Depot Geheimnisse | Microsoft Azure"
    description="Eine Beispiel-app mit Azure Stack Schlüssel zu verwenden"
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

# <a name="run-the-sample-application-for-key-vault"></a>Führen Sie die beispielanwendung für Schlüssel Depot 

Eine Beispiel-Anwendung verwenden Sie in diesem Handbuch Geheimnisse und Kennwörter von Key Vault abrufen.

## <a name="download-the-samples-and-prepare"></a>Beispiele downloaden und Vorbereiten

Downloaden Sie Azure Key Vault Client Beispiele von [Azure Key Vault Client Beispielseite](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Extrahieren Sie den Inhalt der ZIP-Datei auf dem lokalen Computer.

Der **README.md** -Datei (Dies ist eine Textdatei), und gehen Sie.

## <a name="run-sample-1--hellokeyvault"></a>Beispiel #1 – HelloKeyVault ausführen
HelloKeyVault ist ein Konsolenanwendungsprojekt geht über wichtige Szenarios, die von Schlüssel unterstützt werden:

  1. Erstellen/Import eine Taste (HSM oder Software)
  2. Verschlüsselt einen Schlüssel mit einem symmetrischen Schlüssel
  3. Umschließen Sie mit einem Schlüssel Schlüssel Depot Inhaltsschlüssel
  4. Den Inhaltsschlüssel Entpacken
  5. Entschlüsseln des geheimen Schlüssels
  6. Festlegen eines Kennwortes

Dieser Konsolenanwendung führen ohne Änderungen, die entsprechenden Konfigurationsdateien in App.Config folgendermaßen aktualisiert:

1. Aktualisieren Sie app-Konfigurationen in HelloKeyVault\App.config mit Vault-URL, Anwendung principal-ID und Schlüssel. Die Informationen kann optional mit **scripts\GetAppConfigSettings.ps1**generiert werden.
2. Aktualisieren Sie die Werte der obligatorischen Variablen im GetAppConfigSettings.ps1.
3. Starten Sie das Windows PowerShell-Fenster.
4. Das Skript GetAppConfigSettings.ps1 im PowerShell-Fenster.
5. Kopieren Sie die Ergebnisse des Skripts in der Datei HelloKeyVault\App.config.


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen einer VM mit einem Schlüssel Vault-Kennwort](azure-stack-kv-deploy-vm-with-secret.md)

[Bereitstellen einer VM mit einem Schlüssel Depot](azure-stack-kv-push-secret-into-vm.md)
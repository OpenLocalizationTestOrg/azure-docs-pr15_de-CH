<properties
    pageTitle="Bereitstellen der Ressourcenanbieter MySQL Azure Stapel | Microsoft Azure"
    description="Detaillierte Schritte Ressourcenanbieter MySQL Azure Stapel bereitstellen."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>Bereitstellen Sie MySQL Ressourcenanbieter Azure Stapel mit WebApps

> [AZURE.NOTE] Folgendes gilt nur für Azure Stapel TP1 Bereitstellungen.

Mit diesem Artikel ausführliche Anleitung zum Einrichten der Ressourcenanbieter MySQL auf Azure Stapel Prüfung des Konzepts (POC) folgen, sodass Azure Stapel einschließlich MySQL als Backend WordPress Sites [mit MySQL-Datenbanken](azure-stack-mysql-rp-deploy-short.md) starten können mit [Azure Web Apps](azure-stack-webapps-deploy.md)erstellt.

## <a name="set-up-steps-before-you-deploy"></a>Schritte vor der Bereitstellung einrichten

Vor dem Bereitstellen des Ressourcenanbieters müssen:

- Haben Sie Standard Windows Server-Abbilds mit .NET 3.5
- Deaktivieren Sie verstärkte Sicherheitskonfiguration von Internet Explorer (IE)
- Installieren Sie die neueste Version von Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Erstellen Sie ein Abbild von Windows Server, einschließlich .NET 3.5

Sie können diesen Schritt überspringen, wenn Bits Azure Stapel nach 23/2/2016 heruntergeladen, da das standardmäßige Windows Server 2012 R2 Basisabbild .NET 3.5 Framework in diesem Download und höher enthalten.

Wenn 23/2/2016 heruntergeladen müssen Sie eine Windows Server 2012 R2 Datacenter VHD mit .NET 3.5 erstellen und Standardbild im Bildspeicher Plattform ist.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Deaktivieren Sie IE verstärkte Sicherheit und Aktivieren von cookies

Um ein Ressourcenanbieter bereitzustellen, führen Sie PowerShell Integrated Scripting Environment (ISE) als Administrator müssen Sie Cookies und JavaScript in Internet Explorer Profil ermöglichen Azure Active Directory Administrator und Benutzer anmelden bei verwenden.

**IE deaktivieren verbesserte Sicherheit:**

1. Melden Sie sich bei Azure Stack Proof of Concept (PoC) Computer als AzureStack/Administrator und öffnen Sie Server-Manager.

2. **Verstärkte Sicherheitskonfiguration für Internet Explorer** für Administratoren und Benutzer deaktivieren.

3. **ClientVM.AzureStack.local** virtuellen Computer als Administrator melden Sie an und öffnen Sie Server-Manager.

4. **Verstärkte Sicherheitskonfiguration für Internet Explorer** für Administratoren und Benutzer deaktivieren.

**So aktivieren Sie Cookies:**

1. Startbildschirm klicken Sie auf **Alle apps**, klicken Sie auf **Windows-Zubehör**, Maustaste **Internet Explorer**, auf **mehr**und wählen Sie **als Administrator ausführen**.

2. Ggf. **empfohlen Sicherheit**überprüfen Sie und klicken Sie dann auf **OK**.

3. Klicken Sie in Internet Explorer auf **Extras (gear) Symbol** &gt; **Optionen** &gt; Registerkarte **Datenschutz** .

4. Klicken Sie auf **Erweitert**, stellen Sie sicher, dass beide **akzeptieren** Schaltflächen ausgewählt sind, klicken Sie auf **OK**und klicken Sie erneut auf **OK** .

5. Schließen Sie Internet Explorer und starten Sie PowerShell ISE als Administrator.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Installieren Sie eine Azure-Stapel-kompatible Version von Azure PowerShell

1. Deinstallieren Sie alle vorhandenen Azure PowerShell von VM-Client.

2. Melden Sie sich bei Azure Stapel POC-Maschine als AzureStack/Administrator an.

3. Mit Remote Desktop melden Sie **ClientVM.AzureStack.local** virtuellen Computer als Administrator an.

4. Öffnen Sie das Bedienfeld, klicken Sie auf **Programm deinstallieren** &gt; klicken Sie auf **Azure PowerShell** &gt; klicken Sie auf **Deinstallieren**.

5. [Herunterladen der neuesten Azure PowerShell, der Azure-Stapel unterstützt](http://aka.ms/azstackpsh) , und installieren Sie es.

    Nach der Installation von PowerShell führen Sie diese Überprüfung PowerShell-Skript, um sicherzustellen, dass Ihre Azure Stack-Instanz hergestellt werden kann (eine Login-Webseite erscheint).

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Bootstrap-Bereitstellung der Ressource PowerShell

1. ClientVm.AzureStack.Local mit Azure Stapel POC-Remotedesktop und melden Sie sich als Azurestack\\Azurestackuser.

2. [MySQL-RP-Binärdateien herunterladen](http://aka.ms/masmysqlrp) Datei und extrahieren Sie sie auf Laufwerk D:\\MySQLRP.

3. D: Ausführen\\MySQLRP\\Bootstrap.cmd Datei als Administrator (Azurestack\administrator).

    Dadurch wird die Datei Bootstrap.ps1 in PowerShell ISE.

4. Nach Abschluss der Windows PowerShell ISE laden, klicken Sie auf die Schaltfläche "Wiedergabe" oder drücken Sie F5.

    Zwei wichtige Registerkarten lädt, jeweils alle Skripts und Dateien Sie Ihre MySQL-Ressourcenproviders bereitstellen müssen.

## <a name="prepare-prerequisites"></a>Erforderliche Vorbereitung

Klicken Sie auf die Registerkarte **Komponenten vorbereiten** :

- Erstellen der erforderlichen Zertifikate
- MySQL-Binärdateien auf dem Stapel Azure herunterladen
- Hochladen Sie Artefakte auf ein Speicherkonto Stapel Azure
- Galerie-Elemente veröffentlichen

### <a name="create-the-required-certificates"></a>Erstellen Sie die erforderlichen Zertifikate
Dieses **Neue SslCert.ps1** fügt die \_. AzureStack.local.pfx SSL-Zertifikat D:\\MySQLRP\\Komponenten\\BlobStorage\\Containerordner. Das Zertifikat sichert die Kommunikation zwischen dem Ressourcenanbieter und die lokale Instanz von Azure-Ressourcen-Manager.

1. **Erforderliche Vorbereitung** großen Registerkarte klicken Sie auf die Registerkarte **Neu SslCert.ps1** und ausführen.

2. Geben Sie bei der Aufforderung, die angezeigt wird, ein PFX-Kennwort, das den privaten Schlüssel und **Notieren Sie dieses Kennwort**schützt. Sie werden ihn später benötigen.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>MySQL-Binärdateien auf dem Stapel Azure herunterladen

1. Wählen Sie die Registerkarte **MySqlServer.ps1 herunterladen** und ausführen.
2. Wenn Sie aufgefordert werden, klicken Sie auf **Ja** im Dialogfeld bestätigen den EULA anzunehmen.

    Dieser Befehl fügt zwei Zip-Dateien in den Ordner D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Hochladen Sie alle Elemente in ein Speicherkonto Stapel Azure

1. Klicken Sie auf **Upload-Microsoft.MySql-RP.ps1** und ausgeführt werden.

2. Geben Sie im Dialogfeld Anmeldeinformationen Anforderung Windows PowerShell die Anmeldeinformationen des Dienstadministrators Azure Stapel.

3. Für die Azure Active Directory Mandanten-ID geben den Azure Active Directory Mieter vollqualifizierten Domänennamen ein: zum Beispiel microsoftazurestack.onmicrosoft.com.

    Ein Popup-Fenster fordert Anmeldeinformationen.

    > [AZURE.TIP] Wenn das Popup-Fenster angezeigt wird, Sie, die entweder noch nicht Internet Explorer deaktiviert, erweiterte Sicherheit aktivieren Sie JavaScript auf diesem Computer und Benutzer oder Cookies in Internet Explorer noch nicht akzeptiert. Finden Sie die [Schritte vor der Bereitstellung einrichten](#set-up-steps-before-you-deploy).

4. Geben Sie Ihre Anmeldeinformationen Azure Stapel Dienstadministratoren und klicken Sie auf **Anmelden**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Galerieartikel für spätere Ressource erstellen veröffentlichen

Wählen Sie die Registerkarte **Veröffentlichen GalleryPackages.ps1** und ausgeführt werden. Dieses Skript hinzugefügt Azure Stapel POC Portal Marketplace, mit denen Sie Ressourcen wie Marketplace Elemente bereitstellen zwei Marketplace Elemente.

## <a name="deploy-the-mysql-resource-provider-vm"></a>Bereitstellen des MySQL Ressourcenanbieters VM

Azure Stapel PoC mit der erforderlichen Zertifikate und Marketplace vorbereitet haben, können Sie eine SQL Server-Ressourcenproviders bereitstellen. Klicken Sie auf die Registerkarte **Provider MySQL bereitstellen** :

   - Geben Sie Werte in einer JSON-Datei, die der Bereitstellungsprozess verweist
   - Den Ressourcenanbieter bereitstellen
   - Der lokale DNS-Aktualisierung
   - Der SQL Server-Anbieter-Ressourcenadapter registrieren

### <a name="provide-values-in-the-json-file"></a>Geben Sie Werte in der JSON-Datei

Klicken Sie auf **Microsoft.MySqlprovider.Parameters.JSON**. Diese Datei enthält Parameter die Azure-Ressourcen-Manager-Vorlage ordnungsgemäß Stapel Azure bereitgestellt werden muss.

1. Füllen Sie die **leeren** Parameter in der JSON-Datei:

    - Stellen Sie sicher, dass die **Adminusername** und **Adminpassword** für MySQL Ressource Anbieter VM.

    - Stellen Sie sicher, dass Sie das Kennwort für den **SetupPfxPassword** -Parameter angeben, die eine [Prepare Prequisites](#prepare-prerequisites) Schritt vorgenommen.

    - Stellen Sie sicher, dass **BasicAuthUserName** und **BasicAuthPassword** -Parameter angeben. **Notieren Sie sich diese Werte.** Sie benötigen sie später registrieren den Ressourcenanbieter.

2. Klicken Sie auf **Speichern**.

### <a name="deploy-the-resource-provider"></a>Den Ressourcenanbieter bereitstellen

1. Klicken Sie auf **Deploy-Microsoft.Mysql-provider.PS1** , und führen Sie das Skript.
2. Geben Sie Ihren Namen Mieter in Azure Active Directory Aufforderung.
3. Senden Sie im Popupfenster Ihre Azure Stapel Admin Dienstanmeldeinformationen.

Die vollständige Bereitstellung dauert zwischen 15 und 45 Minuten auf einige stark ausgelastete Azure Stapel Machbarkeitsstudien.

### <a name="update-the-local-dns"></a>Der lokale DNS-Aktualisierung

1. Klicken Sie auf der Registerkarte **Journal-Microsoft.MySQL-fqdn.ps1** , und führen Sie das Skript.
2. Aufforderung zur Azure Active Directory-Mandanten-ID Eingabe Azure Active Directory Mieter vollqualifizierten Domänennamens: z. B. **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Registrieren Sie SQL-RP-Ressourcenproviders

1. Klicken Sie auf der Registerkarte **Journal-Microsoft.My-provider.ps1** , und führen Sie das Skript.

2. Aufforderung für Anmeldeinformationen verwenden Sie als die Parameter **BasicAuthUserName** und **BasicAuthPassword** notiert haben.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Überprüfen Sie die Bereitstellung mithilfe von Azure Stack-Portal

1. Die ClientVM melden, und melden Sie sich erneut als **AzureStack\User**.

2. Auf dem Desktop auf **Azure Stapel POC-Portal** und das Portal Dienstadministrators anmelden

3. Stellen Sie sicher, dass die Bereitstellung war erfolgreich. Klicken Sie auf **Durchsuchen** &gt; **Ressourcengruppen einzusehen**, klicken Sie auf die Ressourcengruppe, die Sie verwendet (Standard ist **MySQLRP**), und stellen sicher, dass der wesentliche Teil Blade (obere Hälfte) **Bereitstellung erfolgreich**gelesen.


4. Stellen Sie sicher, dass die Registrierung erfolgreich war. Klicken Sie auf **Durchsuchen** &gt; **Ressourcenprovider**, und dann für **MySQL**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Erstellen Sie Ihre erste MySQL-Datenbank zum Testen der Bereitstellung

1. Als Dienstadministrators Azure Stapel POC-Portal anmelden

2. Klicken Sie auf die **+** Schaltfläche &gt; **benutzerdefinierte** &gt; **MySQL Server und Datenbanken**.

3. Füllen Sie das Formular mit den Details der Datenbank.

    **Notieren Sie sich die Eingabe "Servername".** Die Zeichenfolge Verbindungen für Ihre Datenbank enthält "Servername" als Teil des Benutzernamens: z. B. ** "user@ <ServerName>"**. Sie müssen einen Benutzernamen im folgenden Format eingeben, beim Verbinden mit der Datenbank: Wenn Sie zum Beispiel eine MySQL-Website mithilfe des Ressourcenproviders Azure-Website bereitstellen


## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie andere [PaaS-Dienste](azure-stack-tools-paas-services.md) wie [SQL Server Ressourcenanbieter](azure-stack-sql-rp-deploy-short.md) und [Web Apps Ressourcenanbieter](azure-stack-webapps-deploy.md).

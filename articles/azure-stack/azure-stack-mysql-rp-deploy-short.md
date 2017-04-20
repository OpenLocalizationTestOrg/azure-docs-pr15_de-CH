<properties
    pageTitle="Verwenden von MySQL-Datenbanken als PaaS Azure Stapel | Microsoft Azure"
    description="Verstehen Sie QuickSteps Ressourcenanbieter MySQL bereitstellen und MySQL als Dienst Azure Stapel."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>MySQL-Datenbanken als PaaS Azure Stapel verwenden

> [AZURE.NOTE] Folgendes gilt nur für Azure Stapel TP1 Bereitstellungen.

Sie können ein Ressourcenanbieter MySQL Azure Stapel bereitstellen. Nach der Bereitstellung des Ressourcenproviders können MySQL Server und Datenbanken über Anwendungsvorlagen Azure-Ressourcen-Manager erstellen und MySQL-Datenbanken als Dienst bereitzustellen. MySQL-Datenbanken, die auf Websites unterstützen viele Website-Plattformen. Nach der Bereitstellung des Ressourcenproviders können WordPress Websites von Azure Web Apps-Plattform als Service (PaaS) Add-on für Azure erstellen.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Schritte zum Bereitstellen des Ressourcenanbieters
Verwenden Sie folgendermaßen, wenn Sie bereits mit Azure vertraut sind. Wenn Sie mehr Details wünschen, Hyperlinks in jedem Abschnitt oder direkt zum [Bereitstellen der MySQL Datenbank Anbieter Ressourcenadapter auf Azure Stapel POC](azure-stack-mysql-rp-deploy-long.md).

1.  Stellen Sie sicher, Sie [alle Schritte umfassende](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) vor den Ressourcenanbieter bereitstellen:

    - 3.5 von .NET Framework ist bereits im Windows Server-Basisabbild festgelegt. (Wenn Bits Azure Stapel nach Februar 23, 2016 heruntergeladen können Sie diesen Schritt überspringen.)
    - [Eine Version von Azure PowerShell, die kompatibel mit Azure ist, installiert](http://aka.ms/azStackPsh).
    - In Internet Explorer Einstellungen auf ClientVM [Internet Explorer aus Sicherheitsgründen deaktiviert und Cookies aktiviert sind](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [MySQL-Anbieter Binärdateien Ressourcendatei herunterladen](http://aka.ms/masmysqlrp) und extrahieren Sie sie auf die ClientVM in der Azure-Stapel Machbarkeitsstudie (PoC).

3. [Bootstrap.cmd und Skripts ausführen](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Eine Gruppe von Skripts, die zwei wichtigsten Registerkarten gruppiert öffnet in der PowerShell Integrated Scripting Environment (ISE). Führen Sie die geladenen Skripts nacheinander von links nach rechts auf jeder Registerkarte.

    1. Skripts im Register **Vorbereiten** von links nach rechts, um ausführen:

        - Erstellen Sie ein Platzhalterzertifikat zum Sichern der Kommunikation zwischen dem Ressourcenanbieter und Azure-Ressourcen-Manager.
        - MySQL-EULA zustimmen und MySQL-Binärdateien herunterladen.
        - Hochladen Sie Zertifikate und andere Artefakte auf ein Speicherkonto Stapel Azure.
        - Veröffentlichen Sie Gallery-Pakete bereitstellen MySQL-Ressourcen in der Sammlung.

        > [AZURE.IMPORTANT] Wenn Skripts ohne ersichtlichen Grund hängen, nachdem Sie Ihre Azure Active Directory Mieter übermittelt, blockieren die Sicherheitsstufe eine DLL, die für die Bereitstellung ausführen muss. Zum Beheben dieses Problems Microsoft.AzureStack.Deployment.Telemetry.Dll im Ordner Anbieter Ressource suchen Sie, rechts klicken Sie auf klicken Sie auf **Eigenschaften**, und überprüfen Sie **Zulassen** auf der Registerkarte **Allgemein** .

    2. Skripts in die Registerkarte **Bereitstellen** von links nach rechts, um ausführen:

        - [Bereitstellen einer virtuellen Maschine (VM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , die der Ressourcenanbieter, MySQL Server und Datenbanken, die Sie instanziieren hostet. Dieses Skript verweist auf ein JSON-Parameterdatei, müssen Sie einige Werte aktualisieren, bevor Sie das Skript ausführen.
        - [Registrieren Sie einen lokalen DNS-Datensatz](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) , die dem Ressourcenanbieter VM zugeordnet wird.
        - [Registrieren der Ressourcenanbieter](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) mit lokalen Azure Resource Manager.

        > [AZURE.IMPORTANT] Alle Skripts angenommen, Betriebssystem-Image erforderlichen Komponenten (.NET 3.5 installiert, JavaScript und Cookies aktiviert die ClientVM und die neueste Version von Azure PowerShell installiert) erfüllt. Wenn Sie beim Ausführen des Skripts Fehler erhalten, überprüfen Sie die Voraussetzung erfüllt.

5. [Testen Sie die neue MySQL Ressourcenanbieter](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment)eine MySQL-Datenbank in Azure Stapel Portal bereitstellen. Klicken Sie auf **Erstellen** &gt; **benutzerdefinierte** &gt; **MySQL Server und Datenbank**.

Dies sollte dem Ressourcenanbieter MySQL, etwa 25 Minuten wiederherstellt.

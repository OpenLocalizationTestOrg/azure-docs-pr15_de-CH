<properties
    pageTitle="Das Generieren und übertragen HSM-geschützte Azure Key Vault | Microsoft Azure"
    description="Anhand dieses Artikels können Sie planen, erstellen und übertragen eigener HSM-geschützten Schlüssel mit Azure Schlüssel."
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
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Generieren und übertragen HSM-geschützten Schlüssel für Azure Key Vault

##<a name="introduction"></a>Einführung

Für zusätzliche Sicherheit bei der Verwendung von Azure Key Vault können importieren oder Schlüssel in Hardwaresicherheitsmodule (HSMs), die Grenze HSM niemals verlassen. Dieses Szenario wird häufig als *eigene Schlüssel*oder BYOK bezeichnet. Die HSMs werden FIPS 140-2 Level 2 überprüft Azure Key Vault verwendet Thales nShield-Produktfamilie HSMs Schlüssel schützen.

Anhand der Informationen in diesem Thema können Sie planen, erstellen und übertragen eigener HSM-geschützten Schlüssel mit Azure Schlüssel.

Diese Funktion ist nicht verfügbar für Azure China. 

>[AZURE.NOTE] Weitere Informationen zu Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](key-vault-whatis.md)  
>
>Ein immer gestartet, umfasst ein Schlüssel Depot für HSM-geschützten Schlüssel erstellen, finden Sie unter [Erste Schritte mit Azure Schlüssel](key-vault-get-started.md).

Weitere Informationen über das Generieren und Übertragen eines HSM-geschützter Schlüssels über das Internet:

- Generieren des Schlüssels ein offline-Workstation die Angriffsfläche reduziert.

- Der Schlüssel wird mit einem Schlüssel Exchange Schlüssel (KEK) verschlüsselt die verschlüsselte bis Azure Key Vault erläutert werden. Die verschlüsselte Version des Schlüssels bleibt die ursprüngliche Arbeitsstation.

- Das Toolset legt Eigenschaften für Ihre Mieter Schlüssel, der den Schlüssel Azure Key Vault Security World gebunden. So nach Azure Key Vault HSMs erhalten und Ihre Schlüssel entschlüsseln können dieser HSMs. Der Schlüssel kann nicht exportiert werden. Diese Bindung wird durch Thales HSMs erzwungen.

- Verschlüsselt den Schlüssel Key Exchange Schlüssel (KEK) in Azure Key Vault HSMs generiert und kann nicht exportiert werden. Die HSMs erzwingen, dass keine klare Version des KEK außerhalb der HSMs. Darüber hinaus enthält das Toolset Bescheinigung Thales, die KEK ist nicht exportierbar in eine echte HSM von Thales hergestellt wurde.

- Das Toolset enthält Bescheinigung Thales Azure Key Vault Security World auch auf eine echte HSM hergestellten Thales generiert wurde. Diese Bescheinigung zeigt Sie, dass Microsoft genuine Hardware verwenden.

- Microsoft verwendet separate KEKs und getrennte Welten, die Sicherheit in jeder geografischen Region. Diese Trennung wird sichergestellt, dass der Schlüssel nur in Rechenzentren in der Region verwendet werden kann, in dem Sie verschlüsselt. Beispielsweise kann ein Schlüssel von einem europäischen Kunden in Rechenzentren in Nordamerika oder Asien verwendet werden.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Weitere Informationen zu Thales HSMs und Microsoft services

Thales e-Security ist ein führender Anbieter von Verschlüsselung und Cyber Security Solutions, Finanzdienstleistungen, Hightech, Fertigung, Regierung und Technologie. Mit einem 40 Jahre Schutz Unternehmen Informationen Thales Solutions vier der fünf größten Energie und Luftfahrt dienen. Lösungsvorschläge auch von 22 NATO-Ländern verwendet, und mehr als 80 Prozent der weltweit Transaktionen sichern.

Microsoft arbeitete mit Thales HSMs zu dem Stand der Technik. Diese Optimierungen können Sie normale Vorteile der gehosteten Dienste ohne jedoch Kontrolle über Ihre Schlüssel. Insbesondere können diese Erweiterungen verwalten die HSMs auf keinen Microsoft. Als Cloud-Dienst skaliert Azure Key Vault kurzfristig zu Auslastungsspitzen Ihrer Organisation. Zur gleichen Zeit Ihre Schlüssel in Microsofts HSMs geschützt: Sie behalten die Kontrolle über wichtige Lebenszyklus den Schlüssel generieren und Übertragen von Microsoft erläutert.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementiert einen eigenen Schlüssel (BYOK) Azure Key Vault bringen

Die folgenden Informationen und Verfahren verwenden, wenn Sie eigene HSM-geschützten Schlüssel generieren und übertragen Sie sie in Azure Key Vault – der Schlüssel (BYOK)-Szenario.


##<a name="prerequisites-for-byok"></a>Erforderliche Komponenten für BYOK

Finden Sie in der folgenden Tabelle eine Liste der erforderlichen Komponenten für einen eigenen Schlüssel (BYOK) für Azure Key Vault bringen.

|Anforderung|Weitere Informationen|
|---|---|
|Ein Azure-Abonnement|Erstellen Sie eine Azure Key Vault Azure-Abonnement erforderlich: [Melden Sie sich für kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)|
|Azure Key Vault Premium-Service-Tier HSM-geschützten Schlüssel unterstützt|Weitere Informationen über die Dienstebenen und Funktionen für Azure Key Vault anzeigen Sie Website [Azure Key Vault Preise](https://azure.microsoft.com/pricing/details/key-vault/)|
|Thales HSM, Smartcards und Support-software|Sie müssen zu einem Hardwaresicherheitsmodul Thales Grundkenntnisse Thales HSMs betriebliche. Liste der kompatiblen Modelle oder eine HSM zu erwerben, haben Sie nicht eine finden Sie unter [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) .|
|Die folgende Hardware und Software:<ol><li>Offline X64 Workstation mit einer minimalen Windows-Betriebssystem von Windows 7 und Thales nShield Software mindestens Version 11,50.<br/><br/>Wenn diese Arbeitsstation Windows 7 ausgeführt wird, müssen Sie [Microsoft.NET Framework 4.5 installieren](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Eine Arbeitsstation, die mit dem Internet verbunden und verfügt über eine minimale Windows-Betriebssystem von Windows 7.</li><li>Ein USB-Laufwerk oder anderen mobilen Gerät mit mindestens 16 MB Speicherplatz.</li></ol>|Aus Sicherheitsgründen wird empfohlen, die erste Arbeitsstation nicht mit einem Netzwerk verbunden ist. Diese Empfehlung ist allerdings nicht programmgesteuert erzwungen.<br/><br/>Beachten Sie, dass in die folgende Anleitung dieser Arbeitsstation als getrennte Arbeitsstation bezeichnet wird.</p></blockquote><br/>Darüber hinaus ist der Tenant-Schlüssel ein, empfehlen wir eine zweite, separate Arbeitsstation das Toolset herunterladen und Hochladen des Tenant-Schlüssels verwenden. Aber zu Testzwecken können Sie dieselbe Arbeitsstation als den ersten.<br/><br/>Beachten Sie, dass in die folgende Anleitung dieses zweite Workstation Workstation Internetzugang genannt.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generieren und Ihre Schlüssel in Azure Key Vault HSM übertragen

Sie verwenden die folgenden fünf Schritte generieren und den Schlüssel auf einer Azure Key Vault HSM:

- [Schritt 1: Vorbereiten der Arbeitsstation Internetzugang](#step-1-prepare-your-internet-connected-workstation)
- [Schritt 2: Vorbereiten der getrennte Arbeitsstation](#step-2-prepare-your-disconnected-workstation)
- [Schritt 3: Ihre Schlüssel generieren](#step-3-generate-your-key)
- [Schritt 4: Vorbereiten des Schlüssels für die Übertragung](#step-4-prepare-your-key-for-transfer)
- [Schritt 5: Übertragen Sie Ihre Schlüssel Azure Schlüssel Tresor](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Schritt 1: Vorbereiten der Arbeitsstation Internetzugang
Führen Sie für diesen ersten Schritt die folgenden Verfahren auf der Arbeitsstation, die mit dem Internet verbunden ist.


###<a name="step-11-install-azure-powershell"></a>Schritt 1.1: Azure PowerShell installieren

Von der Arbeitsstation Internetzugang herunter, und installieren Sie Azure PowerShell-Modul, das die Cmdlets zum Verwalten von Azure Schlüssel Depot enthält. Dies erfordert eine minimale Version von 0.8.13.

Installationshinweise Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>Schritt 1.2: Die Azure-Abonnement-ID abrufen

Starten Sie ein Azure PowerShell und melden Sie an, um Ihre Azure-Konto mithilfe des folgenden Befehls:

        Add-AzureAccount
Geben Sie im Popupmenü Browserfenster Ihre Azure Benutzernamen und Kennwort. Verwenden Sie den Befehl [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Finden Sie aus der Ausgabe die ID für das Abonnement für Azure Key Vault verwenden. Sie benötigen dieses Abonnement-ID später.

Schließen Sie Azure PowerShell-Fenster nicht.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Schritt 1.3: BYOK Toolset für Azure Key Vault herunterladen

Besuchen Sie die Microsoft Download Center und für Ihre geografische Region oder Azure-Instanz [Laden Azure Key Vault BYOK Toolset](http://www.microsoft.com/download/details.aspx?id=45345) . Mit dem Paketnamen herunterladen und die entsprechenden SHA-256-Paket Hash bestimmt Folgendes:

---

**Nordamerika:**

Schlüsseltresor-BYOK-Tools-Vereinigtes States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Europa:**

Schlüsseltresor BYOK-Tools Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Asien:**

Schlüsseltresor BYOK-Tools AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Lateinamerika:**

Schlüsseltresor BYOK-Tools LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japan:**

Schlüsseltresor BYOK-Tools Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australien:**

Schlüsseltresor BYOK-Tools Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure Government:**](https://azure.microsoft.com/features/gov/)

Schlüsseltresor BYOK-Tools USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

Schlüsseltresor BYOK-Tools Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Deutschland:**

Schlüsseltresor BYOK-Tools Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Indien:**

Schlüsseltresor BYOK-Tools India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Überprüfen die Integrität der heruntergeladenen BYOK Toolset Azure PowerShell-Sitzung verwenden Sie das Cmdlet " [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) ".

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Das Toolset enthält Folgendes:

- Ein Key Exchange Schlüssel (KEK)-Paket, das einen Namen beginnend mit **BYOK-KEK - Pkg-.**
- Ein Security World-Paket, das einen Namen beginnend mit **BYOK-SecurityWorld - Pkg-.**
- Ein Python-Skript namens v**erifykeypackage.py.**
- Ein Befehlszeilen-Dienstprogramm Datei benannten **KeyTransferRemote.exe** und zugehörigen DLLs.
- Eine Visual C++ Redistributable Package, namens **vcredist_x64.exe.**

Kopieren Sie das Paket auf einem USB-Laufwerk oder einem anderen Wechseldatenträger.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Schritt 2: Vorbereiten der getrennte Arbeitsstation

Führen Sie für diesen zweiten Schritt die folgenden Verfahren auf der Arbeitsstation, die nicht mit einem Netzwerk (Internet oder Ihrem internen Netzwerk) verbunden ist.


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Schritt 2.1: Getrennte Workstation mit Thales HSM vorbereiten

Installieren Sie (Thales) von nCipher-Support-Software auf einem windowscomputer, und fügen Sie ein Thales HSM für diesen Computer.

Sicherstellen Sie, dass die Thales Tools im Pfad (**%nfast_home%\bin** und **%nfast_home%\python\bin**). Geben Sie beispielsweise Folgendes ein:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Weitere Informationen finden Sie in der Bedienungsanleitung Thales HSM.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Schritt 2.2: Installieren Sie BYOK Toolset getrennte Arbeitsstation

BYOK Toolset Paket über das USB-Laufwerk oder einem anderen Wechseldatenträger kopieren und dann folgendermaßen vor:

1. Extrahieren Sie die Dateien aus dem heruntergeladenen Paket in einem Ordner.
2. Führen Sie in diesem Ordner vcredist_x64.exe.
3. Anleitung zu Visual C++-Laufzeitkomponenten für Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Schritt 3: Ihre Schlüssel generieren

Dieser dritte Schritt werden Sie die folgenden Verfahren auf getrennte Arbeitsstation.

###<a name="step-31-create-a-security-world"></a>Schritt 3.1: Erstellen einer Security world

Eine Befehlszeile starten das Programm und führen Sie Thales neue Welt.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Dieses Programm erstellt eine **Sicherheit** % NFAST_KMDATA%\local\world in den Ordner C:\ProgramData\nCipher\Key Management-Data\local entspricht. Können unterschiedliche Werte für das Quorum, aber in unserem Beispiel aufgefordert, jeweils drei leere Karten und Pins eingeben. Dann geben zwei Karten vollen Zugriff auf die Security World. Diese Karten werden **Administrator Card Set** für die neue Security World.

Führen Sie dann folgende Schritte aus:

- Sichern Sie die World-Datei. Sichern Sie und stellen Sie sicher, dass keine einzelne Person Zugriff auf mehr als eine Karte schützen Sie World-Datei, die Administrator-Karten und ihre Pins zu.

###<a name="step-32-validate-the-downloaded-package"></a>Schritt 3.2: Überprüfen des heruntergeladenen Pakets

Dieser Schritt ist optional, aber empfohlen, dass Sie Folgendes überprüfen können:

- Der Schlüssel, der im Toolset enthalten Exchange wurde von einem echten Thales HSM generiert.
- Der Hash Security World, die im Toolset enthalten wurde in einem echten Thales HSM generiert.
- Exchange-Schlüssel kann nicht exportiert werden.

>[AZURE.NOTE]Überprüfen Sie das heruntergeladene Paket muss die HSM werden angeschlossen, eingeschaltet und eine Security World auf (z. B. den gerade erstellten).

Überprüfen Sie das heruntergeladene Paket

1.  Das Skript verifykeypackage.py folgenden je nach Regionen oder Azure-Instanz binden:
    - Nordamerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Europa:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Asien:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Lateinamerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Für Japan:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Für Australien:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - [Azure Government](https://azure.microsoft.com/features/gov/)verwendet die US-Regierung Instanz von Azure:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Für Kanada:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Deutschland:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Indien:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Die Thales-Software umfasst Python am %NFAST_HOME%\python\bin

2.  Bestätigen Sie, dass im folgenden finden Sie die erfolgreichen Validierung angibt: **Ergebnis: Erfolg**

Dieses Skript überprüft die Signaturgeber Kette bis Thales Stammschlüssels. Der Hash dieses Stammschlüssel im Skript eingebettet, und sein Wert sollte **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Sie können diesen Wert auch separat der [Thales-Website](http://www.thalesesec.com/)überprüfen.

Sie nun können einen neuen Schlüssel erstellen.

###<a name="step-33-create-a-new-key"></a>Schritt 3.3: Erstellen eines neuen Schlüssels

Generieren Sie einen Schlüssel mit dem **Generatekey** Thales.

Führen Sie den folgenden Befehl zum Generieren des Schlüssels:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Wenn Sie diesen Befehl ausführen, gehen Sie folgendermaßen vor:

- Wert **Modul**müssen *schützen* Parameter festgelegt werden, wie gezeigt. Dies erstellt einen Schlüssel Modul geschützt. OCS-geschützten Schlüssel unterstützt BYOK Toolset nicht.

- Ersetzen Sie den Wert von *Contosokey* für **Ident** und **Plainname** mit einem String-Wert. Verwaltungsaufwand minimiert das Risiko von Fehlern und wird empfohlen, den gleichen Wert für beide verwenden. **Ident** -Wert darf nur Zahlen, Bindestrichen und Kleinbuchstaben enthalten.

- Der Pubexp bleibt leer (Standardeinstellung) dabei aber bestimmte Werte angeben. Weitere Informationen finden Sie in der Dokumentation zu Thales.

Dieser Befehl erstellt eine Token Schlüsseldatei im Ordner %NFAST_KMDATA%\local mit einem Namen beginnend mit **Key_simple_**, gefolgt von **Ident** , die im Befehl angegeben wurde. Beispiel: **Key_simple_contosokey**. Diese Datei enthält einen verschlüsselten Schlüssel.

Sichern Sie diese Tokenschlüsseldatei an einem sicheren Ort.

>[AZURE.IMPORTANT] Beim später Ihre Schlüssel Azure Schlüssel Tresor übertragen kann nicht Microsoft dieser Schlüssel Sie exportieren, so wird es äußerst wichtig, dass Ihre Schlüssel und Security World problemlos sichern. Erhalten Sie Thales Leitfäden und bewährte Methoden für das Sichern des Schlüssels.

Sie können nun Ihren Schlüssel in Azure Key Vault übertragen.

##<a name="step-4-prepare-your-key-for-transfer"></a>Schritt 4: Vorbereiten des Schlüssels für die Übertragung

Dieser vierte Schritt führen Sie die folgenden Verfahren auf getrennte Arbeitsstation.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Schritt 4.1: Erstellen Sie eine Kopie Ihres Schlüssels mit eingeschränkten Berechtigungen

Um die Berechtigungen für den Schlüssel über eine Befehlszeile zu reduzieren, führen Sie folgenden je nach Regionen oder Azure-Instanz:

- Nordamerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Europa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Asien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Lateinamerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Für Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Für Australien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- [Azure Government](https://azure.microsoft.com/features/gov/)verwendet die US-Regierung Instanz von Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Für Kanada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Deutschland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Indien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Wenn Sie diesen Befehl ausführen, ersetzen Sie *Contosokey* mit demselben Wert im angegebenen **Schritt 3.3: Erstellen Sie einen neuen Schlüssel** Schritt [den Schlüssel generieren](#step-3-generate-your-key) .

Sie müssen die Security World Admin Karten einstecken.

Wenn der Befehl abgeschlossen ist, sehen Sie **Ergebnis: Erfolg** und Kopie Ihres Schlüssels mit eingeschränkten Berechtigungen in der Datei Key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Schritt 4.2: Überprüfen Sie die neue Kopie des Schlüssels

Gegebenenfalls führen Sie die Thales Dienstprogramme die Mindestberechtigungen für den neuen Schlüssel bestätigen:

- aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- Kmfile-Dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Beim Ausführen dieser Befehle ersetzen Contosokey mit demselben Wert im angegebenen **Schritt 3.3: Erstellen Sie einen neuen Schlüssel** Schritt [den Schlüssel generieren](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Schritt 4.3: Verschlüsseln Sie den Schlüssel mithilfe von Microsoft Exchange Schlüssel

Führen Sie einen der folgenden Befehle je nach Regionen oder Azure-Instanz:

- Nordamerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Europa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Asien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Lateinamerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Australien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- [Azure Government](https://azure.microsoft.com/features/gov/)verwendet die US-Regierung Instanz von Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Für Kanada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Deutschland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Indien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Wenn Sie diesen Befehl ausführen, gehen Sie folgendermaßen vor:

- Ersetzen Sie *Contosokey* mit der ID, die zum Generieren des Schlüssels in verwendet **Schritt 3.3: Erstellen Sie einen neuen Schlüssel** Schritt [den Schlüssel generieren](#step-3-generate-your-key) .

- Die ID der Azure-Abonnement, das die wichtigsten Depot enthält ersetzen Sie *SubscriptionID* . Abgerufene Wert zuvor in **Schritt 1.2: Get your Azure-Abonnement-ID** aus dem [Vorbereiten Ihrer Arbeitsstation Internetzugang](#step-1-prepare-your-internet-connected-workstation) Schritt.

- Ersetzen Sie *ContosoFirstHSMKey* durch eine Bezeichnung für die Dateinamen verwendet wird.

Nach erfolgreichem Abschluss zeigt **Ergebnis: Erfolg** und eine neue Datei im aktuellen Ordner mit dem folgenden Namen: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Schritt 4.4: Kopieren Sie Key Transfer-Paket an die Arbeitsstation Internetzugang

Verwenden Sie ein USB-Laufwerk oder einem anderen Wechseldatenträger Ausgabedatei aus dem vorherigen Schritt (KeyTransferPackage ContosoFirstHSMkey.byok) auf der Arbeitsstation Internetzugang kopieren.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Schritt 5: Übertragen Sie Ihre Schlüssel Azure Schlüssel Tresor

Verwenden Sie für diesen letzten Schritt auf der Arbeitsstation Internetzugang [Add-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) -Cmdlet Key Transfer Paket hochladen, das in Azure Key Vault HSM von getrennten Workstation kopiert:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Wenn der Upload erfolgreich war, finden Sie die Eigenschaften der soeben hinzugefügte Schlüssel angezeigt.


##<a name="next-steps"></a>Nächste Schritte

Jetzt können dieser HSM-geschützten Schlüssel in den Schlüssel Tresor. Weitere Informationen finden Sie im Lernprogramm für [Erste Schritte mit Azure Schlüssel](key-vault-get-started.md) Abschnitt **ein Hardwaresicherheitsmodul (HSM) verwendet werden soll** .

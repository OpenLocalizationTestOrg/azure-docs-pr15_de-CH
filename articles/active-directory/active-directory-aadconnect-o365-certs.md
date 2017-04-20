<properties
    pageTitle="Zertifikat Erneuerung Anleitung für Office 365 und Azure Active Directory | Microsoft Azure"
    description="Dieser Artikel erläutert Office 365-Benutzer Probleme mit e-Mails, die sie zum Erneuern eines Zertifikats informieren."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Bei Office 365 und Azure Active Directory erneuern Sie Federation Zertifikate

##<a name="overview"></a>Übersicht

Für die erfolgreiche Föderation zwischen Azure Active Directory (Azure AD) und Active Directory-Verbunddienste (AD FS) übereinstimmen Sicherheitstoken Azure AD signiert von AD FS verwendeten Zertifikaten Azure AD Konfiguration. Jede Abweichung führt zu fehlerhaften Vertrauensstellung. Azure AD wird sichergestellt, dass diese Informationen beim Bereitstellen von AD FS und Web Application Proxy (für Extranetzugriff) synchronisiert.

Dieser Artikel enthält zusätzliche Informationen, die Ihr Token Signaturzertifikate verwalten und synchronisieren sie mit Azure AD in folgenden Fällen:

* Web Application Proxy nicht bereitstellen und daher die Verbundmetadaten steht nicht im extranet.
* Sie verwenden nicht die Standardkonfiguration von AD FS für Zertifikatssignaturen Token.
* Einen Drittanbieter-Identitätsanbieter verwenden.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Standardkonfiguration von AD FS für Zertifikatssignaturen token

Die Tokensignatur Token entschlüsseln Zertifikate werden normalerweise selbstsignierte Zertifikate und werden für ein Jahr. AD FS enthält standardmäßig einen automatische Erneuerung Prozess namens **AutoCertificateRollover**. Wenn Sie AD FS 2.0 oder höher, Office 365 und Azure AD das Zertifikat aktualisiert vor Ablauf.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Erneuerung von Office 365-Portal oder eine e-Mail benachrichtigen

>[AZURE.NOTE] Wenn Sie eine e-Mail oder ein Portal Benachrichtigung gefragt, Office das Zertifikat erneuern, finden Sie unter [Verwalten von Änderungen Signaturzertifikate Token](#managecerts) zu überprüfen, ob Sie Maßnahmen ergreifen müssen. Microsoft kennt ein mögliches Problem zu Benachrichtigungen Schlüsselsätze gesendet, auch wenn keine Aktion erforderlich ist.

Azure AD versucht die Verbundmetadaten überwachen und aktualisieren das Token Signaturzertifikate durch diese Metadaten. 30 Tage vor Ablauf des Tokens, Signieren Zertifikate überprüft Azure AD liegen neue Zertifikate durch die Verbundmetadaten abrufen.

* Wenn es kann erfolgreich die Verbundmetadaten abrufen und neue Zertifikate abrufen, wird keine e-Mail-Benachrichtigung oder Warnung in Office 365-Portal an den Benutzer ausgegeben.
* Wenn sie das neue Token Signaturzertifikate abrufen kann, gibt entweder die Verbundmetadaten nicht erreichbar oder automatischen Rollover ist nicht aktiviert, Azure AD Warnung in Office 365-Portal und eine e-Mail-Benachrichtigung.

![Office 365-Portal-Benachrichtigung](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Wenn Sie AD FS verwenden, um Business Continuity sicherzustellen, überprüfen Sie Ihre Server die folgenden Updates, sodass Authentifizierungsfehler für bekannte Probleme nicht auftreten. Dadurch werden AD FS Proxy Serverprobleme Erneuerung und zukünftige Erneuerungszeiträume:
>
>Server 2012 R2 - [Windows Server Mai 2014 Rollup](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 und 2012 - [Authentifizierungsfehler über Proxy in Windows Server 2012 oder Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Überprüfen Sie, ob die Zertifikate müssen aktualisiert werden<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Schritt 1: Überprüfen Sie den Status AutoCertificateRollover

Öffnen Sie auf dem Server AD FS PowerShell. Überprüfen Sie, dass AutoCertificateRollover auf True festgelegt ist.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Wenn AD FS 2.0 verwenden, führen Sie zuerst Add-Pssnapin-Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Bestätigen Sie Schritt 2: AD FS und Azure AD synchronisieren

Öffnen Sie auf dem AD FS-Server Azure AD PowerShell Prompt und Azure AD an.

>[AZURE.NOTE] Sie können Azure AD PowerShell [hier](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Überprüfen Sie die Zertifikate in AD FS und Azure AD Eigenschaften für die angegebene Domäne vertrauen.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![MsolFederationProperty abrufen](./media/active-directory-aadconnect-o365-certs/certsync.png)

Wenn die Fingerabdrücke in den Ausgaben entsprechen, werden Ihre Zertifikate mit Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Schritt 3: Überprüfen Sie, ob Ihr Zertifikat läuft bald ab

In der Ausgabe von Get-MsolFederationProperty oder AdfsCertificate erhalten Sie unter "Nicht nach" Datum überprüfen Wenn das Datum 30 Tage entfernt, sollten Sie Maßnahmen ergreifen.

| AutoCertificateRollover | Zertifikate mit Azure AD | Verbundmetadaten ist zugänglich | Gültigkeit | Aktion |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Ja | Ja | Ja | - | Keine Maßnahme erforderlich. [Erneuern Tokensignaturzertifikat automatisch](#autorenew)anzeigen |
| Ja | Nein  | - | Weniger als 15 Tage | Erneuern Sie sofort. [Erneuern Tokensignaturzertifikat manuell](#manualrenew)anzeigen |
| Nein | - | - | Weniger als 30 Tage | Erneuern Sie sofort. [Erneuern Tokensignaturzertifikat manuell](#manualrenew)anzeigen |

\[-Spielt keine Rolle.

## Erneuern Sie das Tokensignaturzertifikat automatisch (empfohlen)<a name="autorenew"></a>

Sie müssen manuelle Schritte ausführen, wenn Folgendes zutrifft:
- Sie haben Web Application Proxy eingesetzt, die Verbundmetadaten aus dem extranet ermöglichen können.
- Sie verwenden die AD FS-Standardkonfiguration (AutoCertificateRollover aktiviert).

Überprüfen Sie Folgendes, um sicherzustellen, dass das Zertifikat automatisch aktualisiert werden kann.

**1. die AD FS-Eigenschaft AutoCertificateRollover muss auf True festgelegt.** Dies bedeutet, dass AD FS wird automatisch token signieren und token Entschlüsselung Zertifikate, abläuft, bevor die alten.

**2. die Verbundmetadaten AD FS ist öffentlich zugänglich.** Überprüfen Sie, ob die Verbundmetadaten öffentlich durch Navigieren zur folgenden URL von einem Computer im Internet (aus dem Firmennetzwerk):


https:// (Your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

wo `(your_FS_name) `Ihr Unternehmen, wie fs.contoso.com verwendet Federation Service Hostnamen ersetzt.  Wenn Sie beide überprüfen können Einstellungen, Sie müssen nicht fortfahren.  

Beispiel: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Erneuern Sie das Tokensignaturzertifikat manuell<a name="manualrenew"></a>

Sie können das Token Signaturzertifikate manuell zu erneuern. Die folgenden Szenarien funktioniert beispielsweise besser für manuelle Erneuerung:
* Token Signaturzertifikate sind selbst signierte Zertifikate. Der häufigste Grund dafür ist Ihre Organisation AD FS registrierte Organisation Zertifizierungsstelle Zertifikate verwaltet.
* Sicherheit lässt nicht die Verbundmetadaten öffentlich verfügbar sein.

In diesen Fällen bei jeder Änderung Token Signieren von Zertifikaten müssen Sie auch Ihre Office 365-Domäne aktualisieren mit PowerShell-Befehl Update MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Schritt 1: Sicherstellen Sie, dass AD FS neues Token Signaturzertifikate hat

**Nicht-Standardkonfiguration**

Wenn Sie eine nicht standardmäßige Konfiguration von AD FS verwenden (in **AutoCertificateRollover** auf **False**festgelegt ist), verwenden Sie wahrscheinlich benutzerdefinierte Zertifikate (nicht selbstsigniert). Weitere Informationen zu AD FS-Sicherheitstoken Signaturzertifikate erneuern anzeigen Sie [Anleitung für Kunden nicht AD FS selbstsignierte Zertifikate](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert)

**Verbundmetadaten ist nicht öffentlich verfügbar**

Andererseits, wenn **AutoCertificateRollover** auf **True**festgelegt ist, aber die Verbundmetadaten nicht öffentlich ist, vergewissern Sie sich, dass neue Tokensignaturzertifikate von AD FS generiert wurde. Bestätigen Sie haben neues Token Signieren Zertifikate die folgenden Schritte:

1. Überprüfen Sie, ob Sie die primären AD FS-Server angemeldet sind.
2. Überprüfen der aktuellen Signaturzertifikate in AD FS ein PowerShell-Befehlsfenster öffnen und den folgenden Befehl ausführen:

    PS C:\>Token Signieren von Get-ADFSCertificate-CertificateType

    >[AZURE.NOTE] Wenn Sie AD FS 2.0 verwenden, sollten Sie zuerst Add-Pssnapin-Microsoft.Adfs.Powershell ausführen.

3. Sehen Sie sich die Ausgabe des Befehls in aufgeführten Zertifikate. Wenn AD FS ein neues Zertifikat generiert, sollten Sie zwei Zertifikate in der Ausgabe sehen: für den **IsPrimary** -Wert ist **true,** und **nicht nach** Datum innerhalb von 5 Tagen und für die **IsPrimary** ist **falsch** und **nicht nach** einem Jahr in der Zukunft.

4. Wenn ein Zertifikat nur angezeigt und **nicht nach** Datum innerhalb von 5 Tagen, müssen Sie ein neues Zertifikat generieren.

5. Um ein neues Zertifikat generieren, führen Sie den folgenden Befehl an der Befehlszeile PowerShell: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Überprüfen Sie die Aktualisierung erneut mit dem folgenden Befehl: PS C:\>Token Signieren von Get-ADFSCertificate-CertificateType

Zwei Zertifikate sollte jetzt angezeigt werden, hat ungefähr ein Jahr in der Zukunft **nicht nach** Datum und für den **IsPrimary** -Wert ist **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Schritt 2: Aktualisieren Sie das neue Token Signaturzertifikaten für Office 365 vertrauen

Aktualisieren Sie Office 365 mit dem neuen Token Signieren von Zertifikaten für die Vertrauensstellung wie folgt.

1.  Öffnen Sie Microsoft Azure Active Directory-Moduls für Windows PowerShell.
2.  Ausführen von $cred = Get-Credential. Wenn dieses Cmdlet Anmeldeinformationen aufgefordert werden, geben Sie Ihre Cloud Service Administrator-Anmeldeinformationen.
3.  Verbindung MsolService führen – $cred Anmeldeinformationen. Dieses Cmdlet stellt eine Verbindung zum Cloud-Dienst her. Erstellen einen Kontext, der mit dem Clouddienst herstellt muss vor der Ausführung zusätzliche Cmdlets vom Tool installiert.
4.  Diese Befehle auf einem Computer ausgeführt, der nicht der primäre Verbundserver AD FS ist, führen Sie Set-MSOLAdfscontext-Computer <AD FS primary server>, wobei <AD FS primary server> der interne Vollqualifizierten Domänennamen AD FS Primärserver ist. Dieses Cmdlet erstellt einen Kontext, der mit AD FS verbindet.
5.  Ausführen von Update-MSOLFederatedDomain-DomainName <domain>. Dieses Cmdlet die Einstellungen von AD FS Cloud-Dienst aktualisiert und konfiguriert die Vertrauensstellung zwischen den beiden.


>[AZURE.NOTE] Benötigen Sie Unterstützung mehrerer Domänen der obersten Ebene, z. B. contoso.com und fabrikam.com, müssen Sie **SupportMultipleDomain** Switch mit Cmdlets verwenden. Weitere Informationen finden Sie unter [Unterstützung für mehrere Top Level Domains](active-directory-aadconnect-multiple-domains.md).

## Reparieren Sie Azure AD Trust mithilfe von Azure AD-Verbindung<a name="connectrenew"></a>

Wenn Ihre AD FS-Farm und Azure AD Trust konfiguriert mit Azure AD verbinden, können Azure AD Connect Sie feststellen, ob für das Token Signaturzertifikate Maßnahmen ergreifen müssen. Benötigen Sie Zertifikate erneuern, können Azure AD Connect Sie dies tun.

Weitere Informationen finden Sie unter [Reparieren die Vertrauensstellung](./active-directory-aadconnect-federation-management.md#repairing-the-trust).

<properties 
    pageTitle="PhoneFactor-Agent aktualisieren Azure Multi-Factor Authentication Server"
    description="Dieses Dokument beschreibt Einstieg in Azure MFA-Server und Aktualisieren von älteren Phonefactor-Agent."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>PhoneFactor-Agent aktualisieren Azure Multi-Factor Authentication Server

Aktualisieren von PhoneFactor Agent v5.x oder älter Azure mehrstufige Authentifizierungsserver PhoneFactor Agent und verbundenen Komponenten deinstalliert werden, bevor der kombinierten Authentifizierungsserver erfordert und verbundenen Komponenten installiert werden.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>PhoneFactor-Agent Azure mehrstufige Authentifizierungsserver aktualisieren
<ol>
<li>Zuerst sichern Sie die Datei PhoneFactor. Der Standardspeicherort ist c:\Programme\Microsoft Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Wenn User Portal installiert ist:</li>
<ol>
<li>Navigieren Sie zum Installationsordner und Sichern Sie die Datei web.config. Das Standardverzeichnis ist C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Wenn Sie das Portal benutzerdefinierten Designs hinzugefügt haben, Sichern Sie benutzerdefinierte Ordner unterhalb des Verzeichnisses C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Deinstallieren Sie das Portal Benutzer durch den PhoneFactor-Agent (nur verfügbar, wenn auf demselben Server wie der PhoneFactor-Agent) oder Windows-Programme und Funktionen.</li></ol>




<li>Wenn die Mobile Anwendung Webdienst installiert ist:
<ol>
<li>Zum Installationsordner, und Sichern Sie die Datei web.config. Das Standardverzeichnis ist C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Deinstallieren Sie Mobile App-Webdienst über Windows Programme und Funktionen.</li></ol>

<li>Wenn Web Service SDK installiert ist, deinstallieren Sie es über den PhoneFactor-Agenten oder Windows-Programme und Funktionen.

<li>Deinstallieren Sie den Agent PhoneFactor Fenster Programme und Funktionen.

<li>Installieren Sie Server mehrstufige Authentifizierung. Beachten Sie, dass der Installationspfad abgeholt wird aus der Registrierung der vorherigen PhoneFactor Agent-Installation so am selben Speicherort (z.B. C:\Program Files\PhoneFactor) installieren sollten. Neue Installationen müssen verschiedene Standard Pfad (z.B. C:\Program Files\Multi-Factor Authentication Server). Links vom vorherigen PhoneFactor Agent Datendatei sollten Benutzer und Einstellungen noch es soll nach der Installation neue mehrstufige Authentifizierung während der Installation aktualisiert werden.

<li>Wenn aufgefordert, aktivieren Sie mehrstufige Authentifizierungsserver und sicherstellen Sie, dass der richtige Replikationsgruppe zugewiesen ist.

<li>Wenn der Web Service SDK installiert wurde, installieren Sie neue Web Service SDK über die mehrstufige Authentifizierung Server-Benutzeroberfläche. Beachten Sie, dass der standardmäßige virtuelle Verzeichnisname nun "MultiFactorAuthWebServiceSdk" statt "PhoneFactorWebServiceSdk". Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Gestatten die Installation neuer Standardnamen verwenden müssen Sie andernfalls die URL in Programme ändern, die Web Service SDK wie User Portal und Mobile App-Webdienst auf den richtigen Speicherort verweisen.

<li>Wenn User Portal auf dem Server PhoneFactor-Agent installiert wurde, installieren Sie die neue mehrstufige Authentifizierung User Portal über mehrstufige Authentifizierung Server-Benutzeroberfläche. Beachten Sie, dass der standardmäßige virtuelle Verzeichnisname nun "MultiFactorAuth" statt "PhoneFactor". Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Andernfalls sollten Sie gestatten die Installation neuer Standardnamen verwenden klicken Sie auf dem Authentifizierungsserver mehrstufige User Portal und User Portal-URL auf der Registerkarte aktualisieren.

<li>Wenn User Portal oder Mobile App-Webdienst wurde installiert auf einem anderen Server in der PhoneFactor-Agent:
<ol>
<li>Zum Installationsort (z.B. C:\Program Files\PhoneFactor) und die entsprechenden mitgelieferten auf den Server kopieren. 32-Bit- und 64-Bit-Installer für den User Portal und Mobile App-Dienst sind vorhanden. Sie sind MultiFactorAuthenticationUserPortalSetupXX.msi und MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi genannt.</li>
<li>User Portal auf dem Webserver installieren, öffnen Sie ein Eingabeaufforderungsfenster als Administrator, und führen Sie die MultiFactorAuthenticationUserPortalSetupXX.msi. Beachten Sie, dass der standardmäßige virtuelle Verzeichnisname nun "MultiFactorAuth" statt "PhoneFactor". Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Andernfalls sollten Sie gestatten die Installation neuer Standardnamen verwenden klicken Sie auf dem Authentifizierungsserver mehrstufige User Portal und User Portal-URL auf der Registerkarte aktualisieren. Kunden müssen die neue URL mitgeteilt.</li>
<li>Gehen Sie an die User Portal Installation (z.B. C:\inetpub\wwwroot\MultiFactorAuth) und die Datei web.config bearbeiten. Kopieren Sie die Werte im Abschnitt AppSettings und ApplicationSettings aus der ursprünglichen web.config-Datei, die in die neue web.config-Datei vor der Aktualisierung gesichert wurde. Befindet sich der neue Name des virtuellen Standardverzeichnisses wurde beim Web Service SDK installieren, ändern Sie die URL im ApplicationSettings-Abschnitt auf den richtigen Speicherort zeigen. Wenden Sie anderen Standardeinstellungen in der Datei web.config geändert wurden, die gleiche Änderung in die neue web.config-Datei.</li>
<li>Mobile App-Webdienst auf dem Webserver installieren, öffnen Sie ein Eingabeaufforderungsfenster als Administrator und führen Sie der MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi aus. Beachten Sie, dass der standardmäßige virtuelle Verzeichnisname nun "MultiFactorAuthMobileAppWebService" statt "PhoneFactorPhoneAppWebService". Wenn Sie den vorherigen Namen verwenden möchten, müssen Sie den Namen des virtuellen Verzeichnisses während der Installation ändern. Möglicherweise möchten einen kürzeren Namen für Endbenutzer geben auf ihren mobilen Geräten erleichtert. Andernfalls gestatten die Installation neuer Standardnamen verwenden sollten klicken Sie auf dem Authentifizierungsserver mehrstufige Mobile-Anwendung und die Mobile Anwendung Webdienst-URL aktualisiert.</li>
<li>Gehen Sie an den Webdienst Mobile App Installation (z.B. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) und die Datei web.config bearbeiten. Kopieren Sie die Werte im Abschnitt AppSettings und ApplicationSettings aus der ursprünglichen web.config-Datei, die in die neue web.config-Datei vor der Aktualisierung gesichert wurde. Befindet sich der neue Name des virtuellen Standardverzeichnisses wurde beim Web Service SDK installieren, ändern Sie die URL im ApplicationSettings-Abschnitt auf den richtigen Speicherort zeigen. Wenden Sie anderen Standardeinstellungen in der Datei web.config geändert wurden, die gleiche Änderung in die neue web.config-Datei.</li></ol>

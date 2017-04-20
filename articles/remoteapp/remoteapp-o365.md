
<properties
    pageTitle="Verwendung von Office mit Azure RemoteApp | Microsoft Azure" 
    description="Erfahren Sie, wie Office und Azure RemoteApp zusammenarbeiten"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Verwendung von Office mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Sie haben zwei Optionen für das Hosten von Office-Anwendung in Azure RemoteApp: Office 365 ProPlus oder Testversion von Office 2013 Professional Plus.

**Wussten Hey, Sie haben wir neue Artikel besser, die bald diese ersetzt werden? Überprüfen Sie, [wie Ihr Office 365-Abonnement Azure RemoteApp](remoteapp-officesubscription.md). Alle Informationen für mit Office 365 + Azure RemoteApp behandelt.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Sie können mit Office 365 ProPlus Vorlagenbild eine RemoteApp-Auflistung erstellen. Mit dieser Option können Sie Ihre Office 365-Dienst RemoteApp erweitern. Müssen Sie ein vorhandenes Abonnement und Ihre Benutzer zugelassen für Office 365 ProPlus Dienst entweder eigenständig oder über Office 365 Dienst Pläne.

RemoteApp unterstützt Office 365 freigegebene Aktivierung. Wenn Sie freigegebene Computer Aktivierung aktivieren und [Office-Bereitstellungstools](http://www.microsoft.com/download/details.aspx?id=36778) für die Installation verwenden, installiert Office 365 ProPlus, ohne aktiviert wird. Wenn ein Benutzer in eine Auflistung mit Office 365, überprüft Office der Benutzer für Office 365 ProPlus bereitgestellt wurde. Daher vorübergehend Office Office 365 ProPlus - aktiviert bleibt diese Aktivierung bis dieser Benutzer Zeichen aus dem Dienst.

Um Office 365 freigegebene Aktivierung verwenden, müssen Sie eine [benutzerdefinierte Vorlage](remoteapp-create-custom-image.md) erstellen und installieren Sie Office 365 ProPlus, nach [dieser Anleitung](https://technet.microsoft.com/library/dn782858.aspx).

Sie können die Benutzer Office 365-Lizenzen im [Verwaltungsportal von Office 365](https://portal.office365.com/)verwalten. Informationen Sie weitere zu [Office 365 Servicepläne](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Testversion von Office 2013 Professional Plus
Professional Plus (Testversion) Vorlagenbild Office 2013 können Sie während einer 30-Tage-Testversion von RemoteApp um RemoteApp-Sammlung zu erstellen. Sie können Benutzer dieser Testversion Auflistung mit ihren Azure Active Directory arbeiten oder Microsoft Konten zuweisen. Kein zusätzliches Abonnement ist erforderlich.

Dies ist eine hervorragende Option Reifen und gut für Office in RemoteApp erhalten. Ist jedoch diese Option für Evaluierung und Tests nur. RemoteApp Sammlungen mit Office 2013 Professional Plus (Testversion) vorlagenimage können nicht Produktionsmodus gewechselt und am Ende des Testzeitraums deaktiviert.

## <a name="switching-from-trial-to-production"></a>Wechseln von der Testversion zur Produktion
Beginnen der 30-Tage-Testversion wird eine Notiz im Abschnitt RemoteApp Portal erfahren Sie wie lange Sie Versuch verlassen haben vor dem Übergang zu einem gebührenpflichtigen Konto. Aktivieren Sie Ihr Konto, und über den Link in diesem Produktions-Modus wechseln.

Wenn Sie Ihr Konto aktivieren, wirkt sich dies auf die RemoteApp-Sammlungen in Ihrem Konto aus.

- Sammlungen, die mit Windows Server 2012 R2 oder Office 365 ProPlus Vorlage Bilder geht nahtlos in die Produktion. Alle Benutzerdaten und Einstellungen laufende Sitzungen bleiben erhalten.
- Wenn Sie benutzerdefinierte Vorlage Bilder hochgeladen haben, werden diese Bilder Sammlungen auch nahtlosen Übergang.
- Professional Plus (Testversion) Vorlagenbild Office 2013 dient nur zu Testzwecken. Sammlungen mit dieser Vorlagenbild können nicht für die Produktion umgestellt werden. Sie setzen im Status "deaktiviert".


Wenn Sie nicht Produktionsmodus nach Ablauf des Testzeitraums übertragen werden, werden Ihre Sammlungen RemoteApp deaktiviert. Keine Sorge - Einstellungen und Daten werden weitere 90 Tage weiterhin den Dienst aktivieren und wechseln Sie in die Produktionsmodus ohne Datenverlust.

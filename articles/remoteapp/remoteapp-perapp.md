<properties
   pageTitle="Anwendung für einzelne Benutzer in Azure RemoteApp-Auflistung (Vorschau) veröffentlichen | Microsoft Azure"
   description="Erfahren Sie, wie Sie apps für einzelne Benutzer anstelle von je nach Gruppen in Azure RemoteApp veröffentlichen können."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Veröffentlichen Sie Anwendung für einzelne Benutzer in Azure RemoteApp-Auflistung (Vorschau)

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Erläutert, wie Applikationen für einzelne Benutzer in Azure RemoteApp-Auflistung an. Neue Funktionen in Azure RemoteApp ist in "private Vorschau" und verfügbar nur für Evaluierungszwecke Erstanwendern

Ursprünglich aktiviert Azure RemoteApp "Veröffentlichen" Anwendung nur eine Möglichkeit: Administrator apps aus dem Bild veröffentlichen und sie sichtbar für alle Benutzer in der Auflistung.

Häufig wird häufig in einem Bild und einer Auflistung um Managementkosten bereitstellen. Oft nicht alle Programme für alle Benutzer – Administratoren Benutzer apps veröffentlichen, damit sie nicht benötigte Programme in ihrer Anwendung feed sehen möchten.

Dies ist jetzt möglich in Azure RemoteApp – aktuell als Vorschau beschränkt. Hier ist eine Zusammenfassung der neuen Funktionen:

1. Eine Auflistung kann in zwei Modi festgelegt werden:
 
  - die ursprüngliche "Sammlungsmodus" alle Benutzer in einer Auflistung finden veröffentlicht alle Applikationen. Dies ist der Standardmodus.
  - neue "Application Mode", Benutzer nur Programme finden, die Sie explizit zugewiesen

2. Derzeit kann der Anwendungsmodus nur aktiviert werden, mithilfe von Azure RemoteApp-PowerShell-Cmdlets wurde.

  - Auf Anwendungsmodus festgelegt, in der Auflistung von Benutzerrechten Azure Portal verwaltet werden kann. Benutzer hat über PowerShell-Cmdlets verwaltet werden.

3. Benutzer sehen nur die Programme direkt veröffentlicht. Jedoch kann immer noch für einen Benutzer andere Programme auf das Bild direkt im Betriebssystem auf Starten möglich.
  - Dieses Feature bietet keine Sicherheit Sperren von Applikationen. Sichtbarkeit der Anwendung feed nur beschränkt.
  - Benötigen Sie Benutzer Applikationen zu isolieren, müssen Sie getrennte Auflistungen verwenden.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Wie man Azure RemoteApp-PowerShell-cmdlets

Gehen Sie neue Vorschaufunktion müssen Sie Azure PowerShell-Cmdlets verwenden. Kann derzeit nicht mit der Azure-Verwaltungsportal neue Veröffentlichungsmodus Anwendung aktiviert.

Vergewissern Sie sich, [Azure PowerShell-Modul](../powershell-install-configure.md) installiert haben.

Starten Sie die PowerShell-Konsole im Administratormodus und führen Sie das folgende Cmdlet:

        Add-AzureAccount

Sie werden für Ihren Azure Benutzernamen und Kennwort aufgefordert. Nach der Anmeldung werden Sie Azure RemoteApp Cmdlets für Ihre Azure-Abonnements ausführen.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Wie Sie überprüfen, welchen Modus eine Auflistung

Führen Sie das folgende Cmdlet:

        Get-AzureRemoteAppCollection <collectionName>

![Der Sammlungsmodus überprüfen](./media/remoteapp-perapp/araacllelvel.png)

AclLevel-Eigenschaft kann die folgenden Werte aufweisen:

- Sammlung: die ursprünglichen Veröffentlichungsmodus. Alle Benutzer sehen alle apps veröffentlicht.
- Anwendung: die neue Veröffentlichungsmodus. Benutzer sehen nur die apps direkt veröffentlicht.

## <a name="how-to-switch-to-application-publishing-mode"></a>Veröffentlichungsmodus Anwendung wechseln

Führen Sie das folgende Cmdlet:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Anwendung veröffentlichen Zustand beibehalten: zunächst alle Benutzer sehen die ursprünglichen veröffentlichten Apps.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Wie Benutzer, die eine bestimmte Anwendung anzeigen können

Führen Sie das folgende Cmdlet:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Hier werden alle Benutzer der Anwendung sehen können.

Hinweis: Sie sehen Anwendung Aliase (in der obigen Syntax "app Alias" genannt) durch Ausführen von Get-AzureRemoteAppProgram - CollectionName <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Zuweisen eine Anwendung zu einem Benutzer

Führen Sie das folgende Cmdlet:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Der Benutzer sehen nun die Anwendung in Azure RemoteApp-Client und herstellen können.

## <a name="how-to-remove-an-application-from-a-user"></a>Eine Anwendung von einem Benutzer entfernen

Führen Sie das folgende Cmdlet:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Abgeben von feedback
Wir schätzen Ihr Feedback und Vorschläge zu diesem Vorschaufunktion. Füllen Sie die [Umfrage](http://www.instant.ly/s/FDdrb) , teilen Sie uns Ihre Meinung.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Habe nicht die Vorschaufunktion ausprobieren?
Verwenden Sie die Vorschau noch nicht teilgenommen haben, diese [Umfrage](http://www.instant.ly/s/AY83p) Zugriff anfordern.

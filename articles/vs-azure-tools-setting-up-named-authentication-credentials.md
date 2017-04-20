<properties
   pageTitle="Einrichten von Authentifizierungsinformationen namens | Microsoft Azure"
   description="Hier erfahren Sie, wie auf Anmeldeinformationen, Visual Studio kann in Azure veröffentlichen Sie eine Anwendung in Azure von Visual Studio oder einen vorhandenen Clouddienst überwachen authentifizieren... "
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="setting-up-named-authentication-credentials"></a>Einrichten von benannten Anmeldeinformationen

Veröffentlichen Sie eine Anwendung in Azure von Visual Studio oder einen vorhandenen Cloud-Dienst zu überwachen, müssen Sie Anmeldeinformationen, mit denen Visual Studio in Azure authentifizieren. Es gibt mehrere Orte in Visual Studio, in dem Sie sich anmelden können, diese Anmeldeinformationen. Beispielsweise können Sie vom Server-Explorer öffnen Sie das Kontextmenü für den **Azure** -Knoten und **mit Azure**auswählen. Bei der Anmeldung, Abonnementinformationen Azure-Konto steht in Visual Studio und nichts mehr möchten.

Azure Tools unterstützt auch eine ältere Möglichkeit, Anmeldeinformationen mit dem (publishsettings-Abonnementdatei). Dieses Thema beschreibt diese Methode weiterhin in Azure SDK 2.2 unterstützt wird.

Die folgenden Elemente sind für die Authentifizierung in Azure erforderlich.

- Die Abonnement-ID

- Ein gültiges x. 509 v3-Zertifikat

>[AZURE.NOTE] Der x. 509 v3-Zertifikat Schlüssel muss mindestens 2048 Bits bestehen. Azure lehnt alle Zertifikate, die diese Anforderung nicht erfüllen oder gilt nicht.

Visual Studio verwendet die Abonnement-ID, mit der Daten als Anmeldeinformationen. Die entsprechenden Anmeldeinformationen wird in der Abonnementdatei (PUBLISHSETTINGS-Datei) enthält einen öffentlichen Schlüssel für das Zertifikat. Die Abonnementdatei kann Anmeldeinformationen für mehrere Abonnements enthalten.

Abonnementinformationen im Dialogfeld **New/Edit-Abonnement** können, wie weiter unten in diesem Thema erläutert.

Möchten Sie ein Zertifikat zu erstellen, können die Anweisungen in [Erstellen und Verwaltungszertifikat für Azure hochladen](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) und uploaden das Zertifikat manuell auf den [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Diese Visual Studio Cloud-Dienste verwalten müssen Anmeldeinformationen nicht dieselben Anmeldeinformationen, die zum Authentifizieren einer Anforderung für Azure-Speicherdienste erforderlich sind.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Ändern Sie oder exportieren Sie Authentifizierungsinformationen in Visual Studio

Sie können auch einrichten, ändern oder exportieren Ihre Anmeldeinformationen im Dialogfeld **Neues Abonnement** wird angezeigt, wenn Sie eine der folgenden Aktionen ausführen:

- Öffnen Sie im **Server-Explorer**das Kontextmenü für den Knoten **Azure** , wählen Sie **Abonnements verwalten**, wählen Sie die Registerkarte **Zertifikate** und wählen Sie **neu** oder **Bearbeiten** .

- Beim Veröffentlichen eines Azure Cloud Service aus dem **Azure-Anwendung veröffentlichen** -Assistenten wählen Sie in der Liste **Abonnements** **Verwalten** und wählen Sie die Registerkarte Zertifikate und wählen Sie dann **neu** oder **Bearbeiten** .

Vorausgesetzt, dass das **Neue Abonnement** -Dialogfeld geöffnet ist.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Einrichten von Authentifizierungsinformationen in Visual Studio

1. **Wählen Sie ein vorhandenes Zertifikat** für die Authentifizierungsliste wählen Sie ein Zertifikat.

1. Wählen Sie **den vollständigen Pfad kopieren** . Der Pfad für das Zertifikat (CER-Datei) wird in die Zwischenablage kopiert.

    >[AZURE.IMPORTANT] Um Ihre Azure-Anwendung von Visual Studio zu veröffentlichen, müssen Sie dieses Zertifikat [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen.

1. Das Zertifikat mit dem [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen:

    1. Wählen Sie den Link Azure-Portal.

         Der [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885) wird geöffnet.

    1. [Klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)melden Sie an und wählen Sie dann auf **Clouddienste** .

    1. Wählen Sie den Cloud-Dienst, der Sie interessieren.

        Für den Dienst geöffnet.

    1. Wählen Sie auf der Registerkarte **Zertifikate** **Hochladen** .

    1. Fügen Sie den vollständigen Pfad der CER-Datei, die Sie gerade erstellt haben, und geben Sie das Kennwort, das Sie angegeben.

<properties
   pageTitle="Hinzufügen und Verwalten mehrerer Verzeichnisse Azure Active Directory | Microsoft Azure"
   description="Informationen und empfohlene Vorgehensweisen für das Hinzufügen und Verwalten der Azure Active Directory Verzeichnisse erläutert Verzeichnisse als unabhängige Ressourcen"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Fügen Sie hinzu und verwalten Sie mehrerer Azure Active Directory-Verzeichnisse

In Azure Active Directory (Azure AD) jedes Verzeichnis ist eine unabhängige Ressource: ein Peer voll ausgestattete und logisch unabhängig von anderen Verzeichnissen, die Sie verwalten. Keine hierarchische Beziehung besteht zwischen Verzeichnissen. Diese Unabhängigkeit zwischen Verzeichnissen enthält Ressourcen Unabhängigkeit, administrative Unabhängigkeit und Synchronisierung Unabhängigkeit.

##<a name="resource-independence"></a>Ressource Unabhängigkeit

Erstellen oder Löschen einer Ressource in einem Verzeichnis hat keine Auswirkung auf eine Ressource in einem anderen Verzeichnis, mit der Ausnahme von externen Benutzern beschrieben. Wenn Sie eine benutzerdefinierte Domäne "contoso.com" mit einem Verzeichnis verwenden, kann mit einem beliebigen anderen Verzeichnis verwendet werden.

##<a name="administrative-independence"></a>Administrative Unabhängigkeit

Wenn ein Benutzer ohne Administratorrechte Verzeichnis "Contoso" dann ein Testverzeichnis "Test" erstellt:
- Standardmäßig ein Verzeichnis erstellt Benutzer als externen Benutzer in diesem neuen Verzeichnis hinzugefügt und im Verzeichnis die globalen Administratorrolle zugewiesen.
- Die Administratoren des Verzeichnisses "Contoso" Administratorrechte keine direkte Verzeichnis "Test", wenn ein Administrator 'Test' speziell ihnen diese Berechtigungen erteilt. Administratoren von Contoso' steuern Zugriff auf Verzeichnis "Test", wenn sie steuern das Benutzerkonto "Test" erstellt
- Wenn Sie ändern (hinzufügen oder entfernen) einer Administratorrolle für einen Benutzer in einem Verzeichnis die Änderung wirkt sich keine Benutzer in einem anderen Verzeichnis möglicherweise Administratorrolle.

##<a name="synchronization-independence"></a>Synchronisierung Unabhängigkeit

Sie können jede Azure AD-Verzeichnis unabhängig, um Daten aus einer Instanz einer synchronisiert:
  - Das Tool Verzeichnissynchronisation (DirSync) Daten mit einer AD-Gesamtstruktur synchronisieren.
  - Azure Active Directory Connector für Forefront Identity Manager Daten eine oder mehrere lokale Gesamtstrukturen oder nicht Azure AD-Datenquellen synchronisieren.

##<a name="add-an-azure-ad-directory"></a>Ein Azure AD-Verzeichnis hinzufügen

Azure AD-Verzeichnis im klassischen Azure-Portal hinzufügen, wählen Sie die Erweiterung Azure Active Directory auf der linken Seite und tippen Sie auf **Hinzufügen**.

> [AZURE.NOTE]   Im Gegensatz zu anderen Ressourcen Azure sind die Verzeichnisse nicht untergeordneten Ressourcen der Azure-Abonnement. Oder zulassen der Azure-Abonnement abläuft, können Sie weiterhin die Verzeichnisdaten mithilfe von Azure PowerShell API Graph Azure oder anderen Schnittstellen wie Office 365 Admin Center zugreifen. Sie können das Verzeichnis auch ein weiteres Abonnement zuordnen.

Eine allgemeine Übersicht über Azure AD Lizenzierungsfragen und Vorgehensweisen finden Sie unter [Was ist Azure Active Directory Lizenzierung?](active-directory-licensing-what-is.md).

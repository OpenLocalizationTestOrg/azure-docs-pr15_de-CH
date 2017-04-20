<properties
    pageTitle="Zuweisen von Benutzern zu einer benutzerdefinierten Domäne in Active Directory Azure | Microsoft Azure"
    description="Ausfüllen eine benutzerdefinierte Domäne in Azure Active Directory mit Benutzerkonten"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Zuweisen von Benutzern zu einer benutzerdefinierten Domäne

Nach dem Hinzufügen der benutzerdefinierten Domäne in Azure Active Directory müssen Sie Benutzerkonten für diese Domäne hinzufügen, sodass Sie beginnen können, authentifizieren.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Benutzer von einem Verzeichnis in Ihrem Unternehmensnetzwerk synchronisiert

Wenn Sie bereits eine Verbindung zwischen der lokalen Active Directory und Active Directory Azure eingerichtet haben, kann die Synchronisierung Konten auffüllen. Weitere Informationen zum Synchronisieren von Azure Active Directory mit lokalen Active Directory finden Sie unter [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Benutzer hinzugefügt und in der Cloud verwaltet

Die Domäne für ein Benutzerkonto zu ändern:

1.  Öffnen Sie mit einem Konto, ein globaler Administrator oder einen das klassische Azure-portal

2.  Öffnen Sie das Verzeichnis.

3.  Wählen Sie die Registerkarte **Benutzer** .

4.  Wählen Sie den Benutzer aus der Liste.

5.  Ändern Sie die Domäne des Benutzers und dann **Speichern**.

Dies erreichen Sie auch mit [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) oder [Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Wählen Sie eine benutzerdefinierte Domäne beim Erstellen eines neuen Benutzers

1.  Öffnen Sie mit einem Konto, ein globaler Administrator oder einen das klassische Azure-portal

2.  Öffnen Sie das Verzeichnis.

3.  Wählen Sie die Registerkarte **Benutzer** .

4.  Wählen Sie in der Befehlszeile **Hinzufügen**.

5.  Beim Hinzufügen der Benutzername wählen Sie benutzerdefinierte Domäne aus der Domänenliste.

6.  **Wählen Sie aus.**

## <a name="next-steps"></a>Nächste Schritte

-   [Die Anmeldung für Benutzer vereinfachen mithilfe von benutzerdefinierten Domänennamen](active-directory-add-domain.md)

-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)

-   [Erfahren Sie mehr über Managementkonzepte in Azure AD](active-directory-add-domain-concepts.md)

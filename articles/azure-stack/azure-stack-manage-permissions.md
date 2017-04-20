<properties
    pageTitle="Verwalten von Berechtigungen zu Ressourcen pro Benutzer in Azure Stapel (Dienstadministratoren und Mieter) | Microsoft Azure"
    description="Als Dienstadministrator oder Mieter Informationen Sie zum Verwalten von Berechtigungen für Ressourcen pro Benutzer."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Benutzerberechtigungen verwalten

Ein Benutzer in Azure Stapel kann ein Leser, Besitzer oder Mitwirkender für jede Instanz eines Abonnements, Ressourcengruppe oder Service. Angenommen, kann Benutzer A Leseberechtigungen Abonnement 1, haben aber Berechtigungen für Virtual Machine 7.

-   Reader: Benutzer kann alles anzeigen, aber nicht verändern.

-   Teilnehmer: Benutzer kann alles Zugriff auf Ressourcen verwalten.

-   Besitzer: Benutzer kann auch Zugriff auf Ressourcen verwalten.


## <a name="set-access-permissions-for-a-user"></a>Festlegen von Zugriffsberechtigungen für einen Benutzer

1.  Melden Sie sich mit einem Konto mit Berechtigungen für die Ressource, die Sie verwalten möchten.

2.  Blatt für die Ressource klicken Sie **Zugriff** ![](media/azure-stack-manage-permissions/image1.png).

3.  Klicken Sie im Blatt **Benutzer** **Rollen**.

4.  Blatt **Rollen** klicken Sie auf **Hinzufügen** , um Berechtigungen für den Benutzer hinzuzufügen.

## <a name="next-steps"></a>Nächste Schritte

[Eine Mieter Azure Stapel hinzufügen](azure-stack-add-new-user-aad.md)

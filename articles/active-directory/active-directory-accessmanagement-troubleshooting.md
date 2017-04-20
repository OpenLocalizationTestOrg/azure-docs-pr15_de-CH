
<properties
    pageTitle="Problembehandlung bei dynamischen Mitgliedschaft für Gruppen | Microsoft Azure"
    description="Problembehandlung für dynamische Mitgliedschaft für Gruppen in Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Problembehandlung bei dynamischen Mitgliedschaften für Gruppen

**Ich eine Regel auf eine Gruppe konfiguriert, aber keine Mitgliedschaft in der Gruppe aktualisiert**<br/>Überprüfen Sie, ob die Einstellung **Enable delegierte Verwaltung** der Registerkarte **Konfigurieren** auf **Ja** festgelegt ist. Sie sehen diese Einstellung nur, wenn Sie als Benutzer angemeldet sind, die eine Azure Active Directory Premium-Lizenz zugewiesen ist. Überprüfen Sie die Werte für Benutzerattribute in der Regel: Gibt es Benutzer, die der Regel entsprechen?

**Ich eine Regel konfiguriert, aber die vorhandenen Mitglieder der Regel werden entfernt**<br/>Dies ist normal. Mitglieder der Gruppe werden entfernt, wenn eine Regel aktiviert oder geändert wird. Die Auswertung der Regel zurückgegebenen Benutzer werden als Mitglieder der Gruppe hinzugefügt.     

**Ich sehe Mitgliedschaft sofort beim Hinzufügen oder Ändern einer Regel nicht ändert?**<br/>Dedizierte Mitgliedschaft Auswertung erfolgt regelmäßig eine asynchrone Hintergrundprozess. Wie lange dauert der Prozess die Anzahl der Benutzer im Verzeichnis und die Größe der Gruppe erstellt durch die Regel bestimmt. In der Regel sehen Verzeichnisse mit wenigen Benutzern die Gruppenmitgliedschaft ändert in wenigen Minuten. Verzeichnisse mit einer großen Anzahl von Benutzern dauert 30 Minuten oder länger zu füllen.

Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<properties
    pageTitle="Azure Active Directory-Cmdlets für die Gruppe konfigurieren | Microsoft Azure"
    description="Verwaltung die Einstellungen für Gruppen mit Active Directory Azure-Cmdlets."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-Cmdlets für die Gruppe konfigurieren

Im Verzeichnis können Folgendes für einheitliche Gruppen konfiguriert werden:

1.  Klassifizierung: eine kommagetrennte Liste Klassifikationen Benutzer für eine Gruppe festlegen können. Beispiele wären "Als", "Geheim" und "Streng geheim".

2.  Verwendung Richtlinien-URL: ein URL Benutzer für die Vereinbarung zur Verwendung von Unified Gruppen von Ihrer Organisation definiert. Diese URL erscheinen in der Benutzeroberfläche, wo Benutzer Gruppen verwenden.

3.  Gruppe erstellen aktiviert: ob keine, einige oder alle Benutzer können Unified Gruppen erstellen. Wenn aktiviert, können alle Benutzer erstellen. Wenn festgelegt, können keine Benutzer erstellen. Wenn, Sie können auch eine Sicherheitsgruppe Gruppen, deren Benutzer noch erstellen dürfen.

Dazu werden mit einer Einstellung konfiguriert und SettingsTemplate. Zunächst sehen Sie keine Einstellungsobjekte im Verzeichnis. Dies bedeutet, dass das Verzeichnis mit den Standardeinstellungen konfiguriert ist. Um die Standardeinstellungen zu ändern, erstellen Sie ein neues Einstellungsobjekt mithilfe einer Vorlage. Vorlagen werden von Microsoft definiert.

Sie können das Modul mit der Cmdlets für diese Vorgänge von der [Microsoft Connect-Website](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)verwendet.

## <a name="create-settings-at-the-directory-level"></a>Einstellungen auf der Verzeichnisebene erstellen

Erstellen Einstellungen auf Verzeichnisebene, die für alle Office-Gruppen im Verzeichnis gelten.

1. Wenn die SettingTemplate mit nicht bekannt ist, gibt dieses Cmdlet die Liste der Vorlagen:

    `Get-MsolAllSettingTemplate`

    ![Liste der Vorlagen](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Um eine Verwendung Richtlinie URL hinzuzufügen, müssen zuerst SettingsTemplate Objekt abzurufen, das der Durchschnittswert URL Verwendung definiert; d. h. die Group.Unified Vorlage:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Als Nächstes erstellen Sie ein neues Einstellungsobjekt basierend auf der Vorlage:

    `$setting = $template.CreateSettingsObject()`

4. Dann aktualisieren Sie Richtwert Verwendung:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Wenden Sie anschließend die Einstellung:

    `New-MsolSettings –SettingsObject $setting`

    ![Verwendung Richtlinie URL hinzufügen](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Hier werden in der Group.Unified SettingsTemplate definiert.

 **Einstellung**                          | **Beschreibung**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Typ: Zeichenfolge<li>Standard: ""                  | Eine durch Kommas getrennte Werteliste gültige Klassifizierung, die Unified Gruppen angewendet werden können.                
 <ul><li>EnableGroupCreation<li>Typ: Boolean<li>Standard: True              | Das Flag, ob Unified Erstellung im Verzeichnis zulässig ist.                               
 <ul><li>GroupCreationAllowedGroupId<li>Typ: Zeichenfolge<li>Standard: ""         | GUID der Sicherheitsgruppe, die Unified Gruppen erstellen, selbst wenn EnableGroupCreation == False.
 <ul><li>UsageGuidelinesUrl<li>Typ: Zeichenfolge<li>Standard: ""                  | Ein Link zu der Gruppe Richtlinien.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Lesen Sie auf Verzeichnisebene

Diese Schritte lesen Einstellungen auf Verzeichnisebene, die für alle Office-Gruppen im Verzeichnis gelten.

1. Lesen Sie alle vorhandenen verzeichniseinstellungen:

    `Get-MsolAllSettings`

2. Lesen Sie alle Einstellungen für eine bestimmte Gruppe:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Verzeichniseinstellungen, SettingId GUID zu lesen:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Settings-ID-GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Aktualisieren Sie auf Verzeichnisebene

Folgendermaßen update Standardeinstellungen auf Verzeichnisebene, die für alle Office-Gruppen im Verzeichnis gelten.

1. Vorhandene Einstellung-Objekt abrufen:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Rufen Sie den Wert, den Sie aktualisieren möchten:

    `$value = $Setting.GetSettingsValue()`

3. Aktualisieren Sie den Wert:

    `$value["AllowToAddGuests"] = "false"`

4. Aktualisieren Sie die Einstellung:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Entfernen Sie auf Verzeichnisebene

Dieser Schritt entfernt Einstellungen auf Verzeichnisebene, die für alle Office-Gruppen im Verzeichnis gelten.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Referenz für die Filtersyntax Cmdlet

Weitere Azure Active Directory PowerShell-Dokumentation finden unter [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>Der Objektverweis (Group.Unified-SettingsTemplate-Objekt) SettingsTemplate

- "Name": "EnableGroupCreation", "Typ": "System.Boolean", "Standardwert": "true", "Beschreibung": "Boolean Kennzeichnung, ob die Unified Gruppe erstellen-Funktion aktiviert ist."

- "Name": "GroupCreationAllowedGroupId", "Typ": "System.Guid", "Standardwert": "", "Beschreibung": "GUID der Sicherheitsgruppe, die White Unified erstellen."

- "Name": "ClassificationList", "Typ": "System.String", "Standardwert": "", "Beschreibung": "Eine kommagetrennte Liste von gültigen Klassifizierung Werte, Unified Gruppen angewendet werden können."

- "Name": "UsageGuidelinesUrl", "Typ": "System.String", "Standardwert": "", "Beschreibung": "Eine Verknüpfung zu der Gruppe Richtlinien."

Name | Typ | Standardwert | Beschreibung
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Boolean Kennzeichnung, ob die Unified Gruppe erstellen-Funktion aktiviert ist."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | -GUID der Sicherheitsgruppe, die White Unified erstellen."
"ClassificationList"  | "System.String"  | ""  | "Eine kommagetrennte Liste von gültigen Klassifizierung Werte, die Unified Gruppen angewendet werden können."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Eine Verknüpfung zu der Gruppe Richtlinien."

## <a name="next-steps"></a>Nächste Schritte

Weitere Azure Active Directory PowerShell-Dokumentation finden unter [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Zusätzliche Anweisung Programmmanager bei Microsoft Rob de Jong steht [Gruppen](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad)Blog von Rob Young.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

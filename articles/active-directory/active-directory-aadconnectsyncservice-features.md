<properties
    pageTitle="Azure AD Connect Synchronisierungsfunktionen Service und Konfiguration | Microsoft Azure"
    description="Beschreibt Features des Service für Azure AD Connect Synch Service."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect Service Synchronisierungsfunktionen

Synchronisierung von Azure AD-Verbindung besteht aus zwei Komponenten:

- Lokale Komponente namens **Azure AD verbinden synchronisieren**, auch als **Synchronisierungsmodul**.
- Der Dienst im Azure AD auch **Azure AD Connect-Synchronisierungsdienst**

Dieses Thema erläutert, wie die folgenden Funktionen von **Azure AD Connect-Synchronisierungsdienst** funktionieren und sie mit Windows PowerShell konfigurieren.

Diese Werte werden von [Azure Active Directory Modul für Windows PowerShell](http://aka.ms/aadposh)konfiguriert. Downloaden Sie und installieren Sie es separat von Azure AD Connect. In diesem Thema dokumentierten Cmdlets [2016 März freigeben (Build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1)eingeführt. Wenn Sie keinen Cmdlets in diesem Thema dokumentierten dasselbe Ergebnis erzeugen, stellen Sie sicher, dass Sie die neueste Version ausführen.

Führen Sie die Konfiguration in Azure AD-Verzeichnis finden `Get-MsolDirSyncFeatures`.  
![Führen Get-MsolDirSyncFeatures](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Diese Einstellungen können nur von Azure AD-Verbindung geändert werden.

Folgendes können konfiguriert werden, indem `Set-MsolDirSyncFeature`:

DirSyncFeature | Kommentar
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Ein Attribut wird ein Duplikat eines anderen Objekts statt das gesamte Objekt während des Exports Fehler isoliert werden können.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Ermöglicht Objekten auf UserPrincipalName neben primäre SMTP-Adresse.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Ermöglicht das Synchronisierungsmodul das UserPrincipalName-Attribut für verwaltete/lizenzierte Benutzer (nicht verbunden) aktualisieren.

Nachdem Sie eine Funktion aktiviert haben, kann es wieder deaktiviert werden.

>[AZURE.NOTE] 24 August 2016 Feature *Duplikat Attribut Stabilität* standardmäßig für neue Azure AD-Verzeichnissen. Dieses Feature wird auch eingeführt und vor diesem Datum erstellten Verzeichnisse aktiviert. Sie erhalten eine e-Mail-Benachrichtigung bei Ihrem Verzeichnis wird dieses Feature aktiviert.

Die folgenden Eigenschaften von Azure AD-Verbindung konfiguriert und kann nicht geändert werden, indem `Set-MsolDirSyncFeature`:

DirSyncFeature | Kommentar
--- | ---
DeviceWriteback | [Azure AD Connect: Gerät Rückschreiben aktivieren](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Verbindung synchronisieren: Verzeichnis Extensions](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Implementieren von Kennwortsynchronisation mit Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Vorschau: Gruppenrückschreiben](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Derzeit nicht unterstützt.

## <a name="duplicate-attribute-resiliency"></a>Doppeltes Attribut Stabilität
Statt Bereitstellung nicht Objekte mit doppelten Benutzerprinzipalnamen und die doppelten Attributs "isoliert" ProxyAddresses ein temporärer Wert zugewiesen. Wenn der Konflikt aufgelöst wird, wird automatisch temporäre UPN auf den richtigen Wert geändert. Dieses Verhalten kann für Benutzerprinzipalnamen und ProxyAddress separat aktiviert werden. Weitere Informationen finden Sie unter [Identität Synchronisierung und Stabilität Doppeltes Attribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName soft Übereinstimmung
Wenn dieses Feature aktiviert ist, ist weich Übereinstimmung für UPN neben [primäre SMTP-Adresse](https://support.microsoft.com/kb/2641663)aktiviert immer aktiviert ist. Soft-Übereinstimmung wird verwendet, um lokale Benutzer vorhandenen Cloud-Benutzer in Azure AD entsprechen.

Bei Bedarf Übereinstimmung lokale AD-Konten mit vorhandenen Konten in der Cloud nicht verwendetem Exchange Online, und diese Funktion ist nützlich. In diesem Szenario müssen Sie normalerweise keinen Grund für die SMTP-Attributsatz in der Cloud.

Dieses Feature wird auf Standardeinstellung für neu Azure AD-Verzeichnissen erstellt. Sie sehen, ob dieses Feature für Sie mit aktiviert ist:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Wenn diese Funktion nicht für Ihr Azure AD-Verzeichnis aktiviert ist, können Sie es mit aktivieren:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName Updates synchronisieren
In der Vergangenheit Updates mithilfe der Synchronisierungsdienst lokal UserPrincipalName-Attribut wurde blockiert, wenn beides zutrifft:

- Der Benutzer wird (nicht verbunden).
- Der Benutzer wurde keine Lizenz zugewiesen.

Weitere Informationen finden Sie unter [Benutzernamen in Office 365 und Azure Intune lokalen UPN oder alternativen Benutzernamen übereinstimmen](https://support.microsoft.com/kb/2523192).

Aktivieren dieses Features ermöglicht das Synchronisierungsmodul UserPrincipalName aktualisieren, wenn es lokal geändert und Sie Kennwort synchronisieren. Verwenden Sie Föderation, wird diese Funktion nicht unterstützt.

Dieses Feature wird auf Standardeinstellung für neu Azure AD-Verzeichnissen erstellt. Sie sehen, ob dieses Feature für Sie mit aktiviert ist:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Wenn diese Funktion nicht für Ihr Azure AD-Verzeichnis aktiviert ist, können Sie es mit aktivieren:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nach dem Aktivieren dieser Funktion Trend UserPrincipalName bleibt-ist. Nächste Änderung der UserPrincipalName-Attribut lokalen aktualisiert normalen Delta-Synchronisierung Benutzer UPN.  

## <a name="see-also"></a>Siehe auch

- [Azure AD verbinden synchronisieren](active-directory-aadconnectsync-whatis.md)
- [Ihre lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

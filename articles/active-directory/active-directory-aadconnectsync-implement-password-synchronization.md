<properties
    pageTitle="Implementieren Kennwortsynchronisation Azure AD Connect Sync | Microsoft Azure"
    description="Enthält Informationen über die Funktionsweise Kennwortsynchronisation und aktivieren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Implementieren von Kennwortsynchronisation mit Azure AD-Verbindung synchronisieren
Dieses Thema enthält Informationen, müssen Sie die Kennwörter von lokalen Active Directory (AD) zu einem cloudbasierten Azure Active Directory (Azure AD) synchronisieren.

## <a name="what-is-password-synchronization"></a>Was ist Synchronisierung von Kennwörtern
Die Wahrscheinlichkeit, dass Sie Ihre Arbeit aufgrund eines vergessenen Kennworts gehindert bezieht sich auf die Anzahl der Passwörter merken müssen. Sie die höheren Wahrscheinlichkeit vergessen müssen mehr Kennwörter. Fragen und Anrufe über das Zurücksetzen von Kennwörtern und anderen verwandten Problemen erfordern die meisten Helpdesk-Ressourcen.

Synchronisierung von Kennwörtern ist Kennwörter von lokalen Active Directory eine cloudbasierte Azure Active Directory (Azure AD) synchronisieren.
Dieses Feature können Sie Azure Active Directory Services (z. B. Office 365, Windows Intune CRM Online und Azure Active Directory-Domänendienste) anmelden mit demselben Kennwort, die Sie mit Ihrem lokalen Active Directory anmelden.

![Was ist Azure AD verbinden](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Durch Reduzieren der Anzahl der Kennwörter müssen Benutzer auf einmal verwalten kann Kennwortsynchronisation Sie:

- Verbesserung der Produktivität der Benutzer
- Ihre Helpdesk-Kosten reduzieren  

Auch wenn Sie [**mit AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)verwenden, können Sie optional Kennwortsynchronisation als Sicherung aktivieren bei Ausfall die AD FS-Infrastruktur.

Synchronisierung von Kennwörtern ist eine Erweiterung von Azure AD Connect Sync implementiert Directory-Synchronisierung. Um die Synchronisierung von Kennwörtern in Ihrer Umgebung verwenden, müssen Sie:

- Installation Azure AD verbinden  
- Konfigurieren der Verzeichnissynchronisation zwischen Ihrem Firmensitz AD und Azure Active Directory
- Kennwortsynchronisation aktivieren

Weitere Informationen finden Sie in [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md)

> [AZURE.NOTE] Weitere Informationen zu Active Directory Domain Services, die für die Synchronisierung von FIPS und Kennwort konfiguriert sind, finden Sie unter [Kennwort und FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Funktionsweise der Synchronisierung von Kennwörtern
Der Active Directory-Domänendienst speichert Kennwörter in Form einer Hash Wert Darstellung des tatsächlichen Benutzerkennworts. Ein Hashwert ist ein Ergebnis einer einseitigen mathematischen Funktion (die "*Hashalgorithmus*"). Es gibt keine Methode, das Ergebnis einer unidirektionalen Funktion Textversion Kennwort wiederherzustellen. Ein Kennworthash können mit Ihrem lokalen Netzwerk anmelden.

Um Ihr Kennwort zu synchronisieren, extrahiert Azure AD Connect Sync Ihr Kennworthash für lokale Active Directory. Zusätzliche Sicherheit Verarbeitung wird den Kennworthash vor der Synchronisierung auf den Azure Active Directory-Authentifizierung. Kennwörter werden pro Benutzer und chronologisch synchronisiert.

Tatsächlichen Datenflusses Synchronisierungsprozess Kennwort ähnelt der Synchronisierung von Benutzerdaten DisplayName oder e-Mail-Adressen. Kennwörter werden jedoch häufiger als Standardverzeichnis Synchronisierungsfenster für andere Attribute synchronisiert. Der Synchronisierungsvorgang Kennwort wird alle zwei Minuten ausgeführt. Die Häufigkeit dieses Vorgangs kann nicht geändert werden. Beim Synchronisieren eines Kennworts überschreibt vorhandenen Cloud-Kennwort.

Beim ersten Aktivieren der Kennwort-Synchronisation, führt eine erste Synchronisation der Kennwörter für alle Benutzer im Bereich. Eine Teilmenge der Kennwörter definieren synchronisieren möchten keine explizit.

Beim Ändern eines lokalen Kennworts wird das aktualisierte Kennwort, meist in wenigen Minuten synchronisiert.
Kennwort-Synchronisierung versucht automatisch fehlgeschlagene Synchronisierung versucht. Tritt ein Fehler bei dem Versuch, ein Kennwort zu synchronisieren, wird ein Fehler in der Ereignisanzeige protokolliert.

Die Synchronisierung eines Kennworts hat keine Auswirkung auf den aktuell angemeldeten Benutzer.
Die aktuelle Sitzung des Cloud-Dienst ist nicht sofort eine synchronisierte Änderung betroffen, das auftritt, während Sie auf einen Clouddienst angemeldet sind. Cloud-Dienst erneut authentifizieren muss, müssen Sie jedoch das neue Kennwort bereitstellen.

> [AZURE.NOTE] Kennwortsynchronisierung wird nur für Objekt geben Benutzer in Active Directory unterstützt. Es wird für iNetOrgPerson-Objekttyp nicht unterstützt.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Funktionsweise der Synchronisierung von Kennwörtern mit Azure Active Directory-Domänendienste
Kennwort-Synchronisierung können Sie Kennwörter für lokale [Azure Active Directory-Domänendienste](../active-directory-domain-services/active-directory-ds-overview.md)synchronisieren. Dieses Szenario ermöglicht Azure Active Directory-Domänendienste authentifiziert Benutzer in der Cloud mit allen Methoden in Ihrem lokalen AD. Zur Darstellung von diesem Szenario ähnelt der Active Directory-Migrationsprogramm (ADMT) in einer lokalen Umgebung.

### <a name="security-considerations"></a>Sicherheitsaspekte
Beim Synchronisieren von Kennwörtern ist die Textversion des Kennworts Synchronisierung Kennwort Azure AD oder zugehörige Dienste nicht verfügbar.

Außerdem ist nicht erforderlich für lokale Active Directory das Kennwort in einer umkehrbaren verschlüsselten Format gespeichert. Eine Zusammenfassung der Active Directory-Kennworthash für die Übertragung zwischen den Standorten verwendet AD und Azure Active Directory. Der Digest den Kennworthash kann Zugriff auf Ressourcen in der lokalen Umgebung verwendet werden.

### <a name="password-policy-considerations"></a>Kennwort-Belange
Es gibt zwei Arten von Kennwortrichtlinien betroffen Kennwortsynchronisation aktivieren:

1. Kennwortrichtlinie zur Komplexität
2. Richtlinie für den Kennwortablauf

**Kennwortrichtlinie zur Komplexität**  
Beim Aktivieren von Kennwortsynchronisation Außerkraftsetzungsrichtlinien Komplexität Kennwortrichtlinien in Ihrem lokalen Active Directory Komplexität in der Cloud für synchronisierte Benutzer. Alle gültige Kennwörtern für lokale Active Directory können Sie Azure AD-Dienste zugreifen.

> [AZURE.NOTE] Kennwörter für Benutzer, die direkt in der Cloud erstellt sind noch unter Kennwortrichtlinien in der Cloud.

**Richtlinie für den Kennwortablauf**  
Ist ein Benutzer im Bereich der Kennwortsynchronisation wird das Kontokennwort Cloud*Läuft nie ab"*festgelegt.
Sie können weiterhin anmelden, um Ihre Cloud-Diensten mit einer synchronisierten Kennwort in Ihrer lokalen Umgebung abgelaufen ist. Ihr Kennwort Cloud ist das nächste Mal aktualisiert das Kennwort in der lokalen Umgebung zu ändern.

### <a name="overwriting-synchronized-passwords"></a>Kennwörter synchronisiert überschreiben
Ein Administrator kann Ihr Kennwort mit Windows PowerShell manuell zurücksetzen.

In diesem Fall überschreibt das neue Kennwort Passwort synchronisiert und alle in der Cloud definierte Kennwortrichtlinien gelten für das neue Kennwort.

Wenn das lokale Kennwort ändern erneut das neue Kennwort in die Cloud synchronisiert und überschreibt das Kennwort manuell aktualisierte.

## <a name="enabling-password-synchronization"></a>Aktivieren der Synchronisierung von Kennwörtern
Kennwortsynchronisation wird beim Installieren von Azure AD Connect **Express Einstellungen**automatisch aktiviert. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Connect express Einstellungen](./connect/active-directory-aadconnect-get-started-express.md).

Wenn Sie beim Installieren von Azure AD Connect Standardeinstellungen verwenden, können Sie Kennwortsynchronisation auf der Benutzer-Seite. Weitere Informationen finden Sie unter [benutzerdefinierte Installation von Azure AD verbinden](./connect/active-directory-aadconnect-get-started-custom.md).

![Aktivieren der Synchronisierung von Kennwörtern](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronisierung von Kennwörtern und FIPS
Wenn Ihre Server nach FIPS Federal Information Processing Standard () gesperrt wurde, wurde MD5 deaktiviert.

**So aktivieren Sie MD5 für Synchronisierung von Kennwörtern:**

1. Gehen Sie zu **%programfiles%\Azure AD Sync\Bin**.
2. Öffnen Sie **miiserver.exe.config**.
3. Wechseln Sie zum Knoten **Konfiguration-Laufzeit** (am Ende der Datei).
4. Fügen Sie den folgenden Knoten:`<enforceFIPSPolicy enabled="false"/>`
5. Speichern.

Zur Referenz wird dieser Ausschnitt wie es aussehen sollte:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Informationen zur Sicherheit und FIPS finden Sie unter [AAD Kennwort Sync, Verschlüsselung und FIPS-compliance](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Problembehandlung bei Kennwortsynchronisation
Wenn Kennwörter nicht synchronisiert werden, wie erwartet, kann es für Benutzer oder für alle Benutzer.

- Haben Sie ein Problem mit den einzelnen Objekten, finden Sie Sie unter [Problembehandlung bei einem Objekt, die Kennwörter nicht synchronisiert werden](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Wenn Sie ein Problem haben, keine Kennwörter synchronisiert, finden Sie unter [Problembehandlung Probleme, keine Kennwörter synchronisiert werden](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Problembehandlung bei einem Objekt, das nicht Synchronisieren von Kennwörtern
Sie können problemlos Kennwort Synchronisierung Problemen überprüfen Sie den Status eines Objekts.

Starten Sie in **Active Directory-Benutzer und -Computer**. Suchen Sie den Benutzer und dass **Benutzer muss Kennwort bei der nächsten Anmeldung ändern** deaktiviert ist.
![Active Directory produktiv Kennwörter](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Wenn es aktiviert ist, bitten Sie den Benutzer anmelden und das Kennwort ändern. Temporäre Kennwörter werden nicht in Azure Active Directory synchronisiert.

Wenn sie in Active Directory richtig aussieht, besteht der nächste Schritt der Benutzer in das Synchronisierungsmodul. Indem der Benutzer auf lokale Active Directory Azure AD, finden Sie unter liegt eine beschreibende Fehlermeldung für das Objekt.

1. Starten der **[Synchronisierung Service Manager](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Klicken Sie auf **Anschlüsse**.
3. Wählen Sie den **Active Directory Connector** der Benutzer befindet.
4. Wählen Sie **Suche Connectorspace**.
5. Suchen Sie den Benutzer, den Sie suchen.
6. Wählen Sie die Registerkarte **Linie** und sicherstellen Sie, dass mindestens eine Synchronisierungsregel **Kennwort Sync** **true**zeigt. In der Standardkonfiguration wird der Name der Regel Sync **In vom AD - Benutzer AccountEnabled**.  
    ![Herkunfts-Informationen zu einem Benutzer](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. [Führen Sie den Benutzer](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) durch das Metaverse Azure AD-Connectorspace. Das Connectorspace-Objekt haben eine ausgehende Regel **Kennwort synchronisieren** auf **True**festgelegt. In der Standardkonfiguration ist der Name der Synchronisierungsregel **zum AAD - Benutzer hinzufügen**.  
    ![Connector Space-Eigenschaften eines Benutzers](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Klicken Sie Kennwort Sync Details des Objekts über die vergangene Woche **Protokoll...**.  
    ![Details zum Objekt](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Die Statusspalte kann folgende Werte haben:

Status | Beschreibung
---- | -----
Erfolg | Kennwort wurde erfolgreich synchronisiert.
FilteredByTarget | Kennwort ist auf **Benutzer muss Kennwort bei der nächsten Anmeldung ändern**festgelegt. Kennwort wurde nicht synchronisiert.
NoTargetConnection | Kein Objekt im Metaverse oder in Azure AD-Connectorspace.
SourceConnectorNotPresent | Kein Objekt in der lokalen Active Directory-Connectorspace gefunden.
TargetNotExportedToDirectory | Das Azure AD-Connectorspace-Objekt wurde nicht exportiert.
MigratedCheckDetailsForMoreInfo | Protokolleintrag wurde vor Build 1.0.9125.0 erstellt und in ihrem alten Zustand angezeigt.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Problembehandlung, keine Kennwörter synchronisiert werden
Starten Sie das Skript [den Status der Synchronisierung-Kennwort](#get-the-status-of-password-sync-settings)Abschnitt ausgeführt. Es enthält eine Übersicht der Konfiguration Sync Kennwort.  
![PowerShell-Skriptausgabe-Kennwort synchronisieren](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Führen Sie Wenn das Feature in Azure AD nicht aktiviert ist oder der Synchronisierungsstatus Kanal nicht aktiviert ist, den Installationsassistenten verbinden. Wählen Sie **Synchronisierungsoptionen anpassen** und deaktivieren Sie Kennwort synchronisieren. Diese Änderung deaktiviert vorübergehend die Funktion. Dann führen Sie den Assistenten erneut aus, und aktivieren Sie Kennwort synchronisieren. Führen Sie das Skript erneut aus, um sicherzustellen, dass die Konfiguration richtig ist.

Das Skript zeigt wird kein Takt, führen Sie das Skript im [Trigger eine vollständige Synchronisierung aller Kennwörter](#trigger-a-full-sync-of-all-passwords). Dieses Skript kann auch für andere Szenarien verwendet, die Konfiguration jedoch Kennwörter werden nicht synchronisiert.

#### <a name="get-the-status-of-password-sync-settings"></a>Ruft den Status der Synchronisierung-Kennwort

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Trigger eine vollständige Synchronisierung aller Kennwörter
Sie können eine vollständige Synchronisierung aller Kennwörter mithilfe des folgenden Skripts auslösen:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Nächste Schritte

* [Azure AD Verbindung synchronisieren: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

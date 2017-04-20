<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen Directory Synchronization Anforderungen | Microsoft Azure"
    description="Identifizieren, welche Vorschriften erforderlich sind, für alle Benutzer zwischen Synchronisierung on = lokal und Cloud für das Unternehmen."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-directory-synchronization-requirements"></a>Directory Synchronization Anforderungen
Synchronisierung ist darum Benutzer eine Identität in der Cloud entsprechend ihrer lokalen Identität. Ob sie synchronisierte Konto für die Authentifizierung oder Verbundauthentifizierung verwenden, müssen die Benutzer noch eine Identität in der Cloud haben.  Diese Identität müssen verwaltet und regelmäßig aktualisiert werden.  Die Updates können viele Formen von Titel ändern Kennwörter geändert werden.  

Organisationen für lokale Identität Lösung und Benutzer erforderlichen Bewertung starten. Dabei ist wichtig, die technischen Vorschriften für wie Benutzeridentitäten erstellt und in der Cloud aufrechterhalten.  Für die meisten Organisationen Active Directory ist für lokale und Verzeichnisses für lokale Benutzer werden synchronisiert aus, jedoch in einigen Fällen dies nicht der Fall sein wird.  

Stellen Sie sich folgende Fragen:


- Haben einer AD-Gesamtstruktur, mehrere oder keine?
 - Wie viele Azure AD-Verzeichnissen können Sie zu synchronisieren?
 
    1. Verwenden Sie filtern?
    2. Haben Sie mehrere Azure AD Connect Server geplant?
  
- Haben Sie derzeit eine Synchronisierung vor-Ort-tool?
  - Wenn Ja, ist Ihre Benutzer haben Benutzer eine virtuelle Verzeichnis/Integration von Identitäten?
- Haben Sie alle anderen Verzeichnis lokale die zu synchronisieren (z.B. LDAP Directory Personaldatenbank usw.)?
  - Werden alle GALSync tun?
  - Was ist der aktuelle Zustand des UPNs in Ihrer Organisation? 
  - Haben Sie ein anderes Verzeichnis, dem Benutzerauthentifizierung?
  - Verwendet Ihr Unternehmen Microsoft Exchange?
    - Planen sie mit einer hybriden Exchange-Bereitstellung?

Jetzt haben Sie eine Idee zu der Synchronisierung müssen Sie bestimmen, welches Tool für diese Anforderungen richtig ist.  Microsoft bietet mehrere Tools Directory Integration und Synchronisierung ausführen.  Siehe [Hybrid Identity Integration Tools Vergleichstabelle](active-directory-hybrid-identity-design-considerations-tools-comparison.md) Weitere Informationen. 
   
Synchronisierung Bedarf und das Tool, das dazu für Ihr Unternehmen haben, müssen Sie Anträge auswerten, die diese Verzeichnisdienste verwenden. Dabei ist wichtig, die technischen Vorschriften Anwendung in die Cloud zu integrieren. Stellen Sie sich folgende Fragen:

- Werden diese Programme zur Cloud verschoben und das Verzeichnis verwenden?
- Gibt es spezielle Attribute, die in die Cloud synchronisiert werden, damit die Anwendung erfolgreich einsetzen können?
- Müssen diesen neu geschrieben werden, Cloud-Authentifizierung nutzen?
- Weiterhin diesen lokalen Zugriff mit cloudidentität Leben?

Sie sollen die Sicherheit Vorschriften und Integritätsregeln Verzeichnissynchronisation bestimmen. Dabei ist wichtig, eine Liste der Anforderungen, die zur Erstellung und Verwaltung von Benutzeridentitäten in der Cloud. Stellen Sie sich folgende Fragen:

- Wo werden Dirsync-Server befindet?
- Werden Domäne hinzugefügt?
- Befinden der Server in einem eingeschränkten Netzwerk hinter einem Firewall wie einer DMZ?
  - Die Unterstützung der Synchronisierung erforderlichen Firewallports geöffnet können Sie?
- Haben Sie einen Wiederherstellungsplan für den Dirsync-Server?
- Haben Sie ein Konto mit Berechtigungen für alle Gesamtstrukturen mit synchronisieren möchten?
 - Wenn Ihr Unternehmen die Antwort auf diese Frage nicht kennen, lesen Sie den Abschnitt "Berechtigungen für die Synchronisierung von Kennwörtern" in Artikel [Azure Active Directory-Synchronisierungsdienst installieren](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) und ermitteln Sie haben Sie bereits ein Konto mit diesen Berechtigungen oder erstellen möchten.
- Haben Sie kann für mehrteilige Gesamtstruktur Synchronisierung Sync-Server in jeder Gesamtstruktur zu?
 
>[AZURE.NOTE]
Stellen Sie sicher zu Notizen jeder Antwort die Gründe für die Antwort. [Bestimmen Vorfällen Anforderungen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gehen über die verfügbaren Optionen. Durch die Beantwortung dieser Fragen wählen Sie welche Option am besten Ihr Unternehmen passt benötigt.

## <a name="next-steps"></a>Nächste Schritte
[Mehrstufige Authentifizierung ermitteln](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)

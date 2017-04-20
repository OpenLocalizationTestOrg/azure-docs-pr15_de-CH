<properties
    pageTitle="Azure Active Directory Identität Schutz Glossar | Microsoft Azure"
    description="Azure Active Directory Identität Schutz Glossar"
    services="active-directory"
    keywords="Azure active Directory Schutz, Cloud app Discovery, Verwalten von Applikationen, Sicherheit, Risiken, Risiko, Schwachstelle, Sicherheitsrichtlinien, Glossar"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory Identität Schutz Glossar 


### <a name="at-risk-user"></a>Risiko (Benutzer)  
Ein Benutzer mit mindestens aktives Risiko. 

### <a name="atypical-sign-in-location"></a>Untypische Speicherort   
Ein Anmelden von einem geografischen Standort für den Benutzer, ähnlich wie Benutzer oder Pächter.

### <a name="azure-ad-identity-protection"></a>Azure AD-Schutz    
Ein Sicherheitsmodul Azure Active Directory, die eine konsolidierte Ansicht Risiken und potenzielle Schwachstellen Identitäten einer Organisation bereitstellt.

### <a name="conditional-access"></a>Zugangskontrolle  
Eine Richtlinie zum Sichern des Zugriffs auf Ressourcen. Bedingte Regeln in Azure Active Directory gespeichert und von Azure AD vor dem Zugriff auf die Ressource ausgewertet.  Beispielregeln enthalten Zugriff basierend auf Speicherort-Gerät Gesundheit oder Benutzer Authentifizierungsmethode.

### <a name="credentials"></a>Anmeldeinformationen     
Informationen Identifizierung und einen Identifizierungsnachweis, mit dem Zugriff auf lokale und Netzwerkressourcen zu erhalten. Beispiele für Anmeldeinformationen sind Benutzernamen und Kennwörter, Smartcards und Zertifikate.

### <a name="event"></a>Ereignis   
Ein Datensatz einer Aktivität in Azure Active Directory.

### <a name="false-positive-risk-event"></a>Falsch-Positive (Risikoereignis) 
Ein Risiko Ereignisstatus ein Schutz Benutzer Risikoereignis untersucht und einem Risikoereignis falsch gekennzeichnet wurde manuell festlegen.

### <a name="identity"></a>Identität    
Eine Person oder Entität, die durch die Authentifizierung basierend auf Kriterien wie Kennwörtern oder Zertifikaten überprüft werden muss.

### <a name="identity-risk-event"></a>Identität Risikoereignis     
AAD-Ereignis, das Flag außergewöhnlicher von Schutz und kann bedeuten, dass eine Identität beschädigt wurde.

### <a name="ignored-risk-event"></a>Ignoriert (Risikoereignis)    
Ein Risiko Ereignisstatus ein Schutz Benutzer, dass das Risikoereignis geschlossen wird, ohne eine Reparaturaktion manuell festlegen.

### <a name="impossible-travel-from-atypical-locations"></a>Reisen untypische Speicherorte   
Ein Risikoereignis ausgelöst, wenn zwei Anmeldungen für denselben Benutzer erkannt werden, wo mindestens eine aus einem untypische anmelden und ist die Zeit zwischen den Anmeldungen kürzer als die minimale, physisch zwischen diesen Standorten Reisen würde.  

###<a name="investigation"></a>Untersuchung    
Verstehen des Prozess der Überprüfung der Aktivitäten, Protokolle und andere relevante Informationen zu einem Risikoereignis entscheiden oder eindämmende Maßnahmen notwendig sind, wenn und wie die Identität wurde beschädigt und verstehen, wie die betroffenen Identität verwendet wurde.

### <a name="leaked-credentials"></a>Verlorene Anmeldeinformationen  

Ein Risikoereignis bei aktuellen Benutzeranmeldeinformationen (Benutzername und Kennwort) öffentlich im Web dunkel unserer Forscher.

### <a name="mitigation"></a>Minderung  
Eine Aktion, beschränken oder eliminieren können Angreifer eine gefährdete Identität oder Gerät nutzen, ohne die Identität oder das Gerät in einen sicheren Zustand wiederherzustellen. Eine Abschwächung löst keine vorherigen Risikoereignisse Identität oder Gerät zugeordnet.

### <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung 
Eine Authentifizierungsmethode, die erfordert mindestens zwei Authentifizierungsmethoden, z. B. ein Benutzer hat ein Zertifikat; etwas weiß der Benutzer, z. B. Benutzernamen, Kennwörter oder Passphrasen; physische Attribute wie ein Fingerabdruck; und persönliche Attribute wie eine persönliche Signatur.

### <a name="offline-detection"></a>Offline-Erkennung   
Die Erkennung von Anomalien und Bewertung des Risikos eines Ereignisses wie Anmeldeversuch danach für ein Ereignis, das bereits geschehen ist.

### <a name="policy-condition"></a>Richtlinienbedingung    
Ein Teil einer Sicherheitsrichtlinie bestimmt die Entitäten (Gruppen Benutzer apps, Plattformen, Gerätezustände, IP-Adressbereiche, Clienttypen) in die Richtlinie einbezogen oder ausgeschlossen.

### <a name="policy-rule"></a>Richtlinienregel     
Der Teil einer Sicherheitsrichtlinie beschreibt die Umstände, die die Richtlinie und die Aktionen auslösen die Richtlinie auslösen.

### <a name="prevention"></a>Prevention  
Eine Aktion, die der Organisation durch Missbrauch einer Identität oder Gerät beschädigt verdächtigen oder gefährdet kennen. Prevention-Aktion bietet keinen Schutz für das Gerät oder die Identität und vorherigen Risikoereignisse wird nicht aufgelöst.

### <a name="privileged-user"></a>Berechtigungen (Benutzer)
Benutzer, die bei einem Risikoereignis permanente oder temporäre Administratorberechtigungen für mindestens eine Ressource in Azure Active Directory wie ein globaler Administrator Abrechnungsadministrators, Dienstadministrator, Benutzer und Kennwort-Administrator. 

###<a name="real-time"></a>In Echtzeit    
Echtzeit-Erkennung anzeigen

###<a name="real-time-detection"></a>Echtzeit-Erkennung  
Die Erkennung von Anomalien und das Risiko eines Ereignisses wie Anmeldeversuch bevor das Ereignis fortgesetzt.

### <a name="remediated-risk-event"></a>Wiederhergestellt (Risikoereignis)     
Ein Risiko Ereignisstatus automatisch festgelegten Schutz, dass Eintreten des standardmäßigen Reparaturaktion für diesen Risiken mit wiederhergestellt wurde. Z. B. werden das Kennwort zurückgesetzt wird, automatisch viele Risikoereignisse, die angeben, dass das vorherige Kennwort gefährdet wurde behoben.

### <a name="remediation"></a>Behebung     
Eine Aktion, eine Identität oder ein Gerät, das zuvor vermutet oder gefährdet bekanntermaßen sichern. Eine Reparaturaktion stellt die Identität oder das Gerät in einen sicheren Zustand und löst vorherigen Risikoereignisse Identität oder Gerät zugeordnet.

### <a name="resolved-risk-event"></a>(Risikoereignis) aufgelöst   
Ein Risiko Ereignisstatus ein Schutz Benutzer manuell festlegen, geschlossen, hat der Benutzer eine geeignete Behebungsaktion außerhalb Schutz und berücksichtigen die Risikoereignis angibt.

###<a name="risk-event-status"></a>Risikostatus-Ereignis    
Eine Eigenschaft von einem Risikoereignis, der angibt, ob das Ereignis aktiv sein und geschlossen, den Grund dafür.

###<a name="risk-event-type"></a>Risiko-Ereignistyp  
Eine Kategorie für das Risikoereignis den Typ Anomalie, die das Ereignis gefährlich angesehen hat.

###<a name="risk-level-risk-event"></a>Risikostufe (Risikoereignis)  
Ein Hinweis auf den Schweregrad des Risikoereignisses Schutz Benutzer Aktionen zu priorisieren (hoch, Mittel oder niedrig) nehmen sie das Risiko für ihre Organisation. 

###<a name="risk-level-sign-in"></a>Risiko-Level (Anmelden) 
Angabe (hoch, Mittel oder niedrig) der Wahrscheinlichkeit, dass für eine bestimmte Anmeldung, jemand versucht, die Identität des Benutzers zu verwenden.

###<a name="risk-level-user-compromise"></a>Risiko-Level (Benutzer Kompromiss) 
Angabe (hoch, Mittel oder niedrig) der Wahrscheinlichkeit, dass eine Identität beschädigt wurde.

### <a name="risk-level-vulnerability"></a>Risikostufe (Sicherheitsrisiko)  
Der Schweregrad der Identitätsschutz Benutzer Aktionen priorisieren Angabe (hoch, Mittel oder niedrig) nehmen sie das Risiko für ihre Organisation.

### <a name="secure-identity"></a>Sichern (Identität)   
Ergreifen Sie Behebung oder eine Änderung des Kennworts Computer Reinstallation um potenziell gefährdete Identität eine uneingeschränkte Zustand wiederherzustellen.

### <a name="security-policy"></a>Sicherheitsrichtlinien
Eine Sammlung von Regeln und Zustand. Eine Richtlinie kann Entitäten wie Benutzer, Gruppen, apps, Geräte, Plattformen, Gerätezustände, IP-Adressbereiche und Clienttypen Auth2.0 angewendet. Wenn eine Richtlinie aktiviert ist, wird sie immer eine Entität in der Richtlinie enthalten Token für eine Ressource ausgegeben wird ausgewertet.

### <a name="sign-in-v"></a>Melden Sie an (V) 
Eine Identität in Azure Active Directory authentifizieren.

### <a name="sign-in-n"></a>Anmelden (n) 
Prozesse oder Aktionen Authentifizieren einer Identität in Azure Active Directory und das Ereignis, das diesen Vorgang erfasst.

###<a name="sign-in-from-anonymous-ip-address"></a>Anonyme IP-Adresse anmelden    
Ein Risikoereignis ausgelöst, nach einem erfolgreichen Anmeldung von IP-Adresse, die als anonyme Proxy IP-Adresse identifiziert.

###<a name="sign-in-from-infected-device"></a>Infizierte Gerät anmelden 
Ein Risikoereignis ausgelöst, wenn ein Zeichen in eine IP-Adresse stammt von gefährdeten Geräten verwendet werden, die Kommunikation mit einem Server Bot aktiv versuchen bekannt ist.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Anmelden von IP-Adresse mit verdächtigen Aktivitäten 
Ein Risikoereignis wird ausgelöst, nachdem eine erfolgreiche Anmeldung aus einer IP-Adresse mit einer großen Anzahl von fehlgeschlagenen Anmeldeversuchen über mehrere Konten in kurzer Zeit.

###<a name="sign-in-from-unfamiliar-location"></a>Unbekannte aus anmelden 
Ein Risikoereignis ausgelöst, wenn ein Benutzer erfolgreich von einem neuen Speicherort (IP, Breiten-und Längengrad und ASN) signiert.

###<a name="sign-in-risk"></a>Anmelden Risiko     
Risiko Ebene (Anmelden)

###<a name="sign-in-risk-policy"></a>-In Risiko-policy  
Eine bedingte Richtlinie, die überprüft das Risiko einer bestimmten anmelden und Gegenmaßnahmen basierend auf vordefinierten Startbedingungen und Regeln.

###<a name="user-compromise-risk"></a>Risiko der Kompromittierung eines Benutzers     
Risiko auf (Benutzer Kompromiss)

###<a name="user-risk"></a>Risiko    
Risiko auf (Benutzer Kompromiss).

###<a name="user-risk-policy"></a>Benutzerrichtlinie Risiko     
Eine bedingte Richtlinie, die die Anmeldung betrachtet und Gegenmaßnahmen basierend auf vordefinierten Startbedingungen und Regeln.

###<a name="users-flagged-for-risk"></a>Benutzer für Risiko gekennzeichnet   
Benutzer, die Risiken sind aktiv oder Beseitigung

###<a name="vulnerability"></a>Sicherheitsrisiko    
Konfigurations- oder Bedingung in Azure Active Directory macht das Verzeichnis anfällig für Angriffe oder.


## <a name="see-also"></a>Siehe auch

- [Azure Active Directory-Schutz](active-directory-identityprotection.md)

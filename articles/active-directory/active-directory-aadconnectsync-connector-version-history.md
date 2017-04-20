<properties
   pageTitle="Connector-Version Versionsverlauf | Microsoft Azure"
   description="Dieses Thema enthält alle Versionen der Anschlüsse für Forefront Identity Manager (FIM) und Microsoft Identity Manager (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Connector-Version Versionsverlauf
Anschlüsse für Forefront Identity Manager (FIM) und Microsoft Identity Manager (MIM) werden regelmäßig aktualisiert.

>[AZURE.NOTE]
Dieses Thema ist nur FIM und MIM. Diese Connectors werden auf Azure AD Connect nicht unterstützt.

Dieses Thema listet alle Versionen von Connectors, die veröffentlicht wurden.

Weitere Links:

- [Herunterladen der neuesten Connectors](http://go.microsoft.com/fwlink/?LinkId=717495)
- Dokumentation der [Generischen LDAP-Anschluss](active-directory-aadconnectsync-connector-genericldap.md)
- Dokumentation der [Generischen SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md)
- [Webdienstkonnektor](http://go.microsoft.com/fwlink/?LinkID=226245) Dokumentation
- Dokumentation der [PowerShell-Connector](active-directory-aadconnectsync-connector-powershell.md)
- Dokumentation zur [Lotus Domino-Connector](active-directory-aadconnectsync-connector-domino.md)

## <a name="111170"></a>1.1.117.0
Veröffentlicht: 2016 März

**Neue Verbindung**  
Erstveröffentlichung des [Generischen SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md)

**Neue Funktionen:**

- Generische LDAP-Anschluss:
    - Unterstützung für deltaimport mit Isode.
- Webdienstkonnektor:
    - Aktualisiert die CsEntryChangeResult Aktivität und SetImportErrorCode Aktivität Fehler auf Objekt an das Synchronisierungsmodul zurückgegeben werden sollen.
    - Aktualisiert die SAP6 und SAP6User Vorlagen, um das neue Objekt Fehler Funktionalität.
- Connector für Lotus Domino:
    - Für den Export benötigen Sie eine Zulassungsstelle pro-Adressbuch. Dasselbe Kennwort für alle Zertifizierer können jetzt um die Verwaltung zu vereinfachen.

**Behobene Probleme:**

- Generische LDAP-Anschluss:
    - Einige Attribute sind für IBM Tivoli DS nicht richtig erkannt.
    - Leerzeichen am Anfang und Ende von Zeichenfolgen wurden für offenes LDAP während ein deltaimport abgeschnitten.
    - Novell und NetIQ umbenannt, der Verschieben eines Objekts zwischen Organisationseinheiten-Containern und den Export Objekts fehlgeschlagen
- Webdienstkonnektor:
    - Wenn der Webdienst mehrere Endpunkte für die gleiche Bindung hatten, dann entdecken der Connector nicht richtig diese Endpunkte.
- Connector für Lotus Domino:
    - FullName Attribut Mail in Datenbank exportieren funktioniert nicht.
    - Ein Export hinzugefügt und entfernt Member aus einer Gruppe exportiert nur hinzugefügte Mitglieder.
    - Wenn ein Notes-Dokument ungültig (die IsValid Attribut auf False gesetzt), dann schlägt die Verbindung fehl.

## <a name="older-releases"></a>Ältere Versionen
Die Connectors wurden vor März 2016 als Support-Themen veröffentlicht.

**Generische LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, September 2015
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, März 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, Januar 2015
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, September 2014
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, März 2014

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, September 2014

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, September 2014

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, September 2015
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, März 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712 2014 August
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, Februar 2014  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, Oktober 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, August 2013

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

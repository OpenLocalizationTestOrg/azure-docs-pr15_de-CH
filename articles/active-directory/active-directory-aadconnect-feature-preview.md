<properties
   pageTitle="Azure AD Connect: Funktionen in Vorschau | Microsoft Azure"
   description="Dieses Thema beschreibt im Detail mehr in Vorschau in Azure AD verbinden."
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
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Weitere Informationen zu den Funktionen in der Vorschau
Dieses Thema beschreibt, wie Features derzeit Vorschau verwenden.

## <a name="group-writeback"></a>Gruppenrückschreiben
Die Option gruppenrückschreiben in Zusatzoptionen können Rückschreiben **Office 365-Gruppen** in einer Gesamtstruktur mit Exchange installiert ist, Sie. Dies ist eine Gruppe, die immer in der Cloud beherrschen. Wenn Sie lokale Exchange dann Sie diese Gruppen zu lokalen zurückschreiben können so Benutzer mit lokalen Exchange-Postfach können senden und e-Mails aus diesen Gruppen.

Weitere Informationen zu Office 365-Gruppen und deren Verwendung finden Sie [hier](http://aka.ms/O365g).

Diese Gruppe vertreten wie Verteilergruppen in lokalen AD DS. Der lokalen Exchange-Server muss Exchange 2013 kumulative Update 8 (veröffentlicht März 2015) oder Exchange 2016 dieser neuen Gruppentyp erkennen.

**Notizen während der Vorschau**

- Address Book-Attribut wird derzeit in der Vorschau nicht aufgefüllt. Ohne dieses Attribut wird die Gruppe nicht in der GAL sichtbar. Dieses Attribut aufgefüllt am einfachsten mit Exchange PowerShell-Cmdlet `update-recipient`.
- Nur Gesamtstrukturen mit Exchange-Schema sind gültige Ziele für Gruppen. Wenn kein Exchange erkannt wurde, wird gruppenrückschreiben nicht aktiviert werden.
- Nur einzelne Gesamtstruktur Exchange-Organisation-Installationen werden unterstützt. Haben Sie mehr als eine Exchange-Organisation lokal, müssen Sie eine lokale GALSync Lösung für diese Gruppen in anderen Gesamtstrukturen angezeigt werden.
- Das Rückschreiben KE behandelt nicht aktuell Sicherheitsgruppen oder Verteilergruppen.

>[AZURE.NOTE] Ein Abonnement für Azure AD Premium ist erforderlich für das gruppenrückschreiben.

## <a name="user-writeback"></a>Benutzer Rückschreiben
> [AZURE.IMPORTANT] Das Rückschreiben Vorschau Feature entfernt wurde im August 2015 Update Azure AD verbinden. Wenn Sie es aktiviert haben, sollten Sie dieses Feature deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
Fortzusetzen Sie die [benutzerdefinierte Installation von Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

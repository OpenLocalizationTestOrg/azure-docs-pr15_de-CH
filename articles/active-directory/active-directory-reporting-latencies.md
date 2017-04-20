<properties
   pageTitle="Azure Active Directory Wartezeiten Reporting | Microsoft Azure"
   description="Wie lange es dauert für Ereignisse in Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Azure Active Directory Bericht Wartezeiten

*Diese Dokumentation ist Teil der [Azure Active Directory-Berichte-Handbuch](active-directory-reporting-guide.md).*

Bericht                                                  | Mindestens  | Durchschnitt    | Maximale
------------------------------------------------------- | -------- | ---------- | ----------
**Sicherheitsberichte**                                    |          |            |
Ungewöhnliche Aktivität                              | 2 Stunden  | 4 Stunden    | 8 Stunden
Anmelden von unbekannten Quellen                           | 2 Stunden  | 4 Stunden    | 8 Stunden
Anmeldungen nach mehreren Fehlern                        | 2 Stunden  | 4 Stunden    | 8 Stunden
Anmelden über mehrere Regionen                      | 2 Stunden  | 4 Stunden    | 8 Stunden
Anmelden von IP-Adressen mit verdächtigen Aktivitäten     | 2 Stunden  | 4 Stunden    | 8 Stunden
Anmelden von möglicherweise infizierten Geräte                 | 2 Stunden  | 4 Stunden    | 8 Stunden
Benutzer mit außergewöhnlicher Aktivität                   | 2 Stunden  | 4 Stunden    | 8 Stunden
Benutzer mit verlorene                           | 2 Stunden  | 4 Stunden    | 8 Stunden
**Berichte**                                 |          |            |
Konto Bereitstellung Aktivität                           | 2 Stunden  | 4 Stunden    | 8 Stunden
Berücksichtigen Sie Fehler-Bereitstellung                             | 2 Stunden  | 4 Stunden    | 8 Stunden
Anwendung                                       | 2 Stunden  | 4 Stunden    | 8 Stunden
Kennwortstatus rollover                                | 2 Stunden  | 4 Stunden    | 8 Stunden
**Audit- und Berichte**                            |          |            |
Prüfbericht                                            | 1 minute | 15 Minuten | 30 Minuten
Passwort-Aktivität (Azure AD)                      | 2 Stunden  | 4 Stunden    | 8 Stunden
Passwort-Aktivität (Identity Manager)              | 2 Stunden  | 4 Stunden    | 8 Stunden
Passwort Registrierungsaktivität (Azure AD)         | 2 Stunden  | 4 Stunden    | 8 Stunden
Passwort Registrierungsaktivität (Identity Manager) | 2 Stunden  | 4 Stunden    | 8 Stunden
Selbst Serviceaktivität Gruppen (Azure AD)                 | 2 Stunden  | 4 Stunden    | 8 Stunden
Selbst Serviceaktivität Gruppen (Identity Manager)         | 2 Stunden  | 4 Stunden    | 8 Stunden
**RMS-Berichte**                                         |          |            |
Aktivste RMS-Benutzer                                   | 2 Stunden  | 4 Stunden    | 8 Stunden
RMS-Verwendung                                               | 2 Stunden  | 4 Stunden    | 8 Stunden
RMS Nutzung                                        | 2 Stunden  | 4 Stunden    | 8 Stunden
RMS-fähigen Anwendungsverwendung                           | 2 Stunden  | 4 Stunden    | 8 Stunden
**Private Berichtsvorschau**                             |          |            |
Aktivität für alle Benutzer anmelden                               | 2 Stunden  | 4 Stunden    | 8 Stunden

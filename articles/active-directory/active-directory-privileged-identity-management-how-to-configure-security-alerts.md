<properties
   pageTitle="Sicherheitshinweise konfigurieren | Microsoft Azure"
   description="Informationen Sie zum Konfigurieren von Sicherheitshinweise für Azure privilegierten Identitätsmanagement-Erweiterung."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Sicherheitshinweise in Azure AD privilegierten Identitätsmanagement konfigurieren

## <a name="security-alerts"></a>Sicherheitshinweise
Azure privilegierten Identity Management (PIM) löst Alarme, wenn es verdächtige oder unsichere Aktivitäten in Ihrer Umgebung. Wenn eine Warnung ausgelöst wird, wird sie auf PIM-Dashboard. Wählen Sie die Warnung einen Bericht anzuzeigen, der Benutzer oder Rollen, die die Warnung enthält.

![Sicherheitshinweise für PIM Dashboard - screenshot][1]



| Warnung | Trigger | Empfehlung |
| ----- | ------- | -------------- |
| **Rollen werden außerhalb PIM zugeordnet** | Administrator wurde eine Funktion außerhalb der PIM-Schnittstelle dauerhaft zugewiesen. | Überprüfen Sie die neue rollenzuweisung. Da andere Dienste nur permanente Administratoren zuweisen können, einer Aufgabe können bei Bedarf geändert. |
| **Rollen werden auch häufig aktiviert** | Innerhalb der in den Einstellungen wurden zu viele Reaktivierungen dieselbe Rolle. | Wenden Sie sich an den Benutzer darauf, warum die Funktion einmal aktiviert. Vielleicht ist die Frist für ihre Aufgaben, oder vielleicht sie Skripts verwenden eine Rolle automatisch aktiviert. |
| **Rollen erforderlich nicht für die Aktivierung mehrstufige Authentifizierung.** | Ohne MFA in den Einstellungen aktiviert werden | Wir MFA höchst privilegierte Funktionen erfordern jedoch empfehlen, MFA für die Aktivierung aller Rollen aktivieren. |
| **Administratoren verwenden nicht ihre privilegierten Rollen.** | Können Administratoren, die ihre Rollen nicht zuletzt aktiviert werden | Starten einer Überprüfung Access Ermitteln der Benutzer, die Zugriff nicht mehr benötigen. |
| **Es gibt zu viele globale Administratoren** | Es gibt weitere globale Administratoren als. | Haben eine hohe Anzahl von globalen Administratoren ist es wahrscheinlich, dass Benutzer mehr Berechtigungen immer als notwendig. Weniger privilegierten Rollen verschieben Sie Benutzer oder stellen Sie einige für die Rolle dauerhaft zugewiesen. |

## <a name="configure-security-alert-settings"></a>Warnung Sicherheit konfigurieren

Sie können einige Sicherheitshinweise PIM mit Ihrer Umgebung und Ziele anpassen. Gehen Sie die Einstellungen-Blades erreichen:

1. [Azure-Portal](https://portal.azure.com/) anmelden und **Azure AD privilegierten Identitätsmanagement** -Kachel aus dem Dashboard auswählen.
2. Wählen Sie **privilegierte Rollen verwaltet** > **Settings** > **Alarme Settings**.

    ![Navigieren Sie zur Sicherheitsrichtlinie alerts][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Warnung "Rollen werden auch häufig aktiviert"

Diese Warnung auslöst, wenn ein Benutzer die gleiche privilegierte Funktion mehrmals innerhalb eines bestimmten Zeitraums aktiviert. Sie können den Zeitraum und die Anzahl der Vorgänge.

- **Aktivierung Verlängerungszeitraum**: angegeben in Tagen, Stunden, Minuten und zweite Zeitraum mit verdächtigen Erneuerung verfolgen möchten.

- **Anzahl der Aktivierung Erneuerung**: Geben Sie die Anzahl der Vorgänge zwischen 2 und 100, die Sie der Warnung Zeitrahmen berücksichtigen Sie gewählt haben. Sie können dies ändern, durch Verschieben des Schiebereglers oder eine Zahl in das Textfeld eingeben.


### <a name="there-are-too-many-global-administrators-alert"></a>Warnung "Sind zu viele globale Administratoren"

PIM auslöst diese Warnung, wenn zwei verschiedene Kriterien erfüllt sind und beide zu konfigurieren. Zunächst müssen Sie eine bestimmte globale Administratoren Schwelle. Zweitens muss ein bestimmten Prozentsatz Ihrer Aufträge insgesamt Rolle globale Administratoren sein. Wenn Sie nur eine dieser Maßnahmen treffen, wird die Warnung nicht angezeigt.  

- **Mindestanzahl von globalen Administratoren**: globale Administratoren von 2 100 halte unsicheren Betrag angeben.

- **Prozentsatz der globalen Administratoren**: Geben Sie Administratoren, globale Administratoren sind, zwischen 0 % und 100 %, unsichere in Ihrer Umgebung.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Warnung "Administratoren nicht ihre privilegierten Rollen verwenden"

Diese Warnung auslöst, wenn ein Benutzer eine bestimmte Zeitspanne geht ohne eine Rolle.

- **Anzahl Tage**: Geben Sie die Anzahl von Tagen zwischen 0 und 100, die ein Benutzer wechseln kann, ohne eine Rolle.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png

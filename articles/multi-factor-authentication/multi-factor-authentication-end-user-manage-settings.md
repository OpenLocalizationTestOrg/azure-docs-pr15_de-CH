<properties
    pageTitle="Verwalten der zweistufigen Überprüfung | Microsoft Azure"
    description="Verwendung von Azure mehrstufige Authentifizierung einschließlich Kontaktinformationen ändern oder konfigurieren die Geräte verwalten."
    services="multi-factor-authentication"
    keywords = "die kombinierte Authentifizierung-Client Authentifizierungsproblem Korrelations-ID"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Verwalten der zweistufigen Überprüfung

Dieser Artikel beantwortet Fragen erneut für zwei Überprüfung oder kombinierte Authentifizierung. Bei Fragen zu Ihrem Konto anmelden weitere [Probleme mit Überprüfung in zwei Schritten](multi-factor-authentication-end-user-troubleshoot.md) zur Fehlerbehebung.


## <a name="where-to-find-the-settings-page"></a>Wo finden Sie die Einstellungsseite
Je nach Ihrem Unternehmen Azure mehrstufige Authentifizierung sind einige Orte können Sie Einstellungen wie Ihre Telefonnummer ändern.

Wenn Ihren IT-Administrator eine bestimmte URL oder Schritte zu zweistufigen Authentifizierung gesendet, Anweisungen Sie die ausführen. Andernfalls sollte die folgenden für alle anderen funktionieren. Wenn Sie folgendermaßen jedoch nicht die gleichen Optionen, also Ihre Arbeit oder Schule eigenen Portal angepasst. Muss der Administrator für die Verknüpfung auf das Portal Azure mehrstufige Authentifizierung.


1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Wählen Sie oben **Profil**.  
3. Wählen Sie **zusätzliche Überprüfung**.  

    ![MyApps](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Zusätzliche Überprüfungsseite lädt mit Einstellungen.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Ich möchte meine Telefonnummer ändern oder hinzufügen eine sekundäre Nummer

Es ist wichtig, eine sekundäre authentifizierungstelefonnummer konfigurieren.  Da Ihre primäre Telefonnummer und der mobilen Anwendung wahrscheinlich auf dem gleichen Telefon sind, ist die sekundäre Telefonnummer die einzige Möglichkeit, Sie werden in Ihrem Konto zurück, wenn Ihr Telefon verloren geht oder gestohlen wird.

> [AZURE.NOTE]
> Wenn Sie nicht Zugriff auf Ihre primäre Telefonnummer haben und Hilfe Ihrem Konto benötigen, finden Sie unter unsere Hilfethemen [Probleme mit zwei Überprüfung](multi-factor-authentication-end-user-troubleshoot.md).

**Primäre Telefonnummer zu ändern:**  

1. Wählen Sie auf der Seite zusätzliche Überprüfung das Textfeld mit der Telefonnummer und Ihre neue Telefonnummer bearbeiten.  
2. **Wählen Sie aus.**  
3. Ist dies die Nummer, die Sie für Ihre bevorzugten Überprüfungsfunktion verwenden, müssen Sie die neue Nummer überprüfen, bevor Sie sie speichern können.  


**Sekundäre Telefonnummer hinzu**  

1. Auf der Seite Additional Security Überprüfung das Kontrollkästchen neben **alternativen Authentifizierung Telefon.**  
2. Sekundäre Telefonnummer in das Textfeld eingeben.  
3. Wählen Sie **Speichern** und die Änderungen abgeschlossen sind.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Wie Microsoft Authenticator von meinem alten Gerät bereinigen und eine neue verschieben?
Beim Deinstallieren der Anwendung auf dem Gerät oder das Gerät wird nicht die Aktivierung auf dem Back-End entfernt. Sie sollten die Schritte auf [ein neues Gerät](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app)verwenden.

## <a name="next-steps"></a>Nächste Schritte
- Tipps zur Problembehandlung und auf [Probleme mit zwei Überprüfung](multi-factor-authentication-end-user-troubleshoot.md)
- Richten Sie [app Kennwörter](multi-factor-authentication-end-user-app-passwords.md) für apps, die zweistufigen Authentifizierung nicht unterstützen.

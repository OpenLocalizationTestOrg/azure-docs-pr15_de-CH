<properties
    pageTitle="Azure Active Directory B2C: Mehrstufige Authentifizierung | Microsoft Azure"
    description="Mehrstufige Authentifizierung in Azure Active Directory B2C gesichert kundenorientierter Applikationen aktivieren"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Kundenorientierter Anwendung aktivieren Sie mehrstufige Authentifizierung

Azure Active Directory (Azure AD) B2C Integration direkte mit [Azure mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md) , Anmeldung und Anmeldung Erfahrung in kundenorientierter Clientanwendungen eine zweite Sicherheitsebene hinzugefügt werden können. Und Sie können dies tun, ohne eine einzige Codezeile schreiben zu müssen. Derzeit unterstützt Telefonanruf und Text Nachricht überprüfen. Wenn Sie Richtlinien für Anmelde und Anmeldung bereits erstellt haben, können Sie weiterhin mehrstufige Authentifizierung aktivieren.

> [AZURE.NOTE]
Mehrstufige Authentifizierung kann auch beim Anmelden und einloggen Richtlinien erstellen nicht nur Bearbeiten von vorhandenen Richtlinien aktiviert werden.

Dadurch können Programme wie die folgenden Szenarios:

- Benötigen mehrstufige Authentifizierung auf eine Anwendung zugreifen, aber Sie benötigen ein Zugriff auf. Z. B. Consumer eine Versicherung Auto-Anwendung mit einem sozialen oder lokalen Konto anmelden kann, jedoch muss die Telefonnummer überprüfen, bevor Zugriff auf private Versicherung Anwendung im selben Verzeichnis registriert.
- Sie benötigen mehrstufige Authentifizierung im Allgemeinen auf eine Anwendung zugreifen, aber Sie benötigen sie Zugriff auf die vertraulichen Teile darin. Z. B. Consumer einer Banking-Anwendung mit sozialen oder lokalen Konto Kontostand Kontrollkästchen anmelden kann, aber muss die Rufnummer vor einer Überweisung überprüfen.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Ändern der Registrierungsrichtlinie um mehrstufige Authentifizierung

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmeldung**.
3. Klicken Sie auf die Registrierungsrichtlinie (z. B. "B2C_1_SiUp") zu öffnen.
4. **Mehrstufige Authentifizierung** auf und aktivieren Sie den **Zustand** **auf**. Klicken Sie auf **OK**.
5. Klicken Sie am oberen Rand der Blade **Speichern** .

Die Funktion "Jetzt ausführen" können in der Richtlinie Sie das Kundenerlebnis überprüfen. Überprüfen Sie Folgendes:

Ein Kundenkonto wird im Verzeichnis erstellt, bevor der kombinierten Authentifizierung Schritt. Während des Schritts wird der Verbraucher aufgefordert seine Telefonnummer, und bestätigen es. Wenn die Überprüfung erfolgreich ist, ist die Telefonnummer Kundenkonto für die spätere Verwendung zugeordnet. Auch wenn der Verbraucher abbricht oder ausfällt, können Sie aufgefordert, eine Rufnummer erneut bei der nächsten Anmeldung (mit aktivierter Mehrfaktor-Authentifizierung) überprüfen.

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Ändern Sie die Richtlinie um mehrstufige Authentifizierung

1. [Folgendermaßen B2C Features Blade Azure-Portal navigieren](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klicken Sie auf **Anmelden**.
3. Klicken Sie auf die anmelden (z. B. "B2C_1_SiIn") zu öffnen. Klicken Sie am oberen Rand der Blade **Bearbeiten** .
4. **Mehrstufige Authentifizierung** auf und aktivieren Sie den **Zustand** **auf**. Klicken Sie auf **OK**.
5. Klicken Sie am oberen Rand der Blade **Speichern** .

Die Funktion "Jetzt ausführen" können in der Richtlinie Sie das Kundenerlebnis überprüfen. Überprüfen Sie Folgendes:

Wenn der Verbraucher anmeldet (mit einem sozialen oder lokalen Konto), wenn das Kundenkonto verifizierte Telefonnummer zugeordnet ist, er wird zur Bestätigung aufgefordert. Wenn keine Telefonnummer zugeordnet ist, ist der Verbraucher aufgefordert eine und bestätigen es. Nach erfolgreicher Überprüfung auf gehört die Telefonnummer Kundenkonto für die spätere Verwendung.

## <a name="multi-factor-authentication-on-other-policies"></a>Mehrstufige Authentifizierung auf andere Richtlinien

Anmeldung & Anmeldung Richtlinien oben beschrieben, kann auch mehrstufige Authentifizierung auf Anmeldung aktivieren oder Richtlinien Anmeldename und Kennwort zurücksetzen Richtlinien. Sie werden bald auf Profil Bearbeiten von Richtlinien.

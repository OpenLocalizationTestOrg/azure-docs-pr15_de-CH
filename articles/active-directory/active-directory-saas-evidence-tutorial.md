<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Evidence.com | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Evidence.com."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Lernprogramm: Azure Active Directory-Integration mit Evidence.com

Das Ziel dieses Lernprogramms ist einrichten einmaliges Anmelden zwischen Azure Active Directory (AAD) und Evidence.com anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:
    
* Ein gültiges Microsoft Azure-Abonnement
* Ein Evidence.com Abonnement für einmaliges Anmelden aktiviert (e-Mail earlyaccess@evidence.com Wenn SAML-basierte einmaliges Anmelden nicht aktiviert ist)

Am Ende dieses Lernprogramms werden AAD Benutzer, die Zugriff Evidence.com zugeordnet haben, einmaliges in der Anwendung die Abdeckung AAD können.

## <a name="add-evidencecom-to-your-directory"></a>Das Verzeichnis Evidence.com hinzufügen

In diesem Abschnitt wird beschrieben, wie Evidence.com als integrierte Anwendung in Azure Active Directory hinzufügen.

**So aktivieren Sie die Anwendungsintegration Beweise**

1.  Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

4.  Um der Galerie öffnen, klicken Sie auf **Hinzufügen**und dann auf **eine Anwendung aus dem Katalog hinzufügen**.

5.  Geben Sie im Suchfeld **Evidence.com**.

6.  Wählen Sie im Ergebnisbereich **Evidence.com aus**und dann auf **vollständig** die Anwendung hinzufügen.


## <a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt beschreibt, wie Benutzer Evidence.com mit seinem Konto in Azure Active Directory Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

**So konfigurieren einmaliges Anmelden:**

1.  Klicken Sie nach Evidence.com im klassischen Azure-Portal hinzufügen auf **Konfigurieren Sie einmaliges Anmelden**. 
 
2.  Auf dem nächsten Bildschirm **Azure AD einmaliges**wählen Sie aus und dann auf **Weiter**.

3.  Fenster App-URL konfigurieren, geben Sie den URL, in dem Benutzer in der URL Evidence.com Mieter signieren (Beispiel: https://yourtenant.evidence.com, und klicken Sie dann auf **Weiter**. 

4.  Klicken Sie auf **Zertifikat herunterladen** und auf Ihrem lokalen Laufwerk speichern. Das Zertifikat und die Metadaten-URLs (Entity ID und SSO-Zeichen In URL Zeichen, URL) werden zum SSO auf Evidence.com Website einrichten. 

5.  In separaten Webbrowserfenster bei Ihrem Evidence.com Mieter als Administrator und navigieren Sie zur Registerkarte " **Admin** "
      
6.  Klicken Sie auf **Agentur für einmaliges Anmelden**
 
7.  Wählen Sie **SAML einmaliges**
 
8.  Kopieren Sie **Aussteller URL** **Single Sign-On** und **Einzelne Abmelden** Werte im klassischen Azure-Portal und in die entsprechenden Felder im Evidence.com.

9.  Öffnen des Zertifikats in Schritt 4 mit einem Texteditor wie Notepad.exe, heruntergeladene kopieren und fügen den Inhalt im **Sicherheitszertifikat** . 

10. Speichert die Konfiguration in Evidence.com.
 
11. Überprüfen Sie in der Azure-Verwaltungsportal **bestätigen Sie einmaliges Anmelden wie oben beschrieben konfiguriert haben**. Überprüfen wird das aktuelle Zertifikat für diese Anwendung Kontrollkästchen aktivieren.
 
12. Klicken Sie auf der Bestätigungsseite für einzelne Zeichen auf **abgeschlossen**.  


## <a name="creating-an-evidencecom-test-user"></a>Erstellen eines Benutzers Evidence.com test

Azure AD Benutzer anmelden müssen sie für den Zugriff in der Evidence.com-Anwendung bereitgestellt werden. Dieser Abschnitt beschreibt die Azure AD in Evidence.com erstellen

**Bereitstellung von einem Benutzerkonto in Evidence.com**

1.  Melden Sie in einem Webbrowserfenster als Administrator der Website Ihres Unternehmens Evidence.com.

2.  Navigieren Sie zum Register " **Admin** ".

3.  Klicken Sie auf **Benutzer hinzufügen**.

4.  Klicken Sie auf die Schaltfläche **Hinzufügen** .

5.  Die **E-Mail-Adresse** des hinzugefügten Benutzers muss den Benutzernamen des Benutzers in Azure AD zugreifen möchten. Wenn Benutzername und e-Mail-Adresse nicht den gleichen Wert in Ihrer Organisation sind, können Sie die **Evidence.com > Attribute > Single Sign-On** Abschnitt der klassischen Azure-Portal Nameidenitifer an Evidence.com gesendet, um die e-Mail-Adresse ändern.


## <a name="assigning-users-to-evidencecom"></a>Zuweisen von Benutzern zu Evidence.com

Bereitgestellte AAD Benutzer Zugriff auf ihre Evidence.com sehen können müssen sie Zugang klassischen Azure-Portal zugeordnet.

**Evidence.com Benutzer zuweisen:**

1.  Klicken Sie den Schnellstart für Evidence.com im klassischen Azure-Portal auf **Evidence.com Benutzern zuweisen**.
 
2.  Wählen Sie im Menü **Anzeigen** möchten Evidence.com einen Benutzer oder eine Gruppe zuweisen, und klicken Sie auf das Kontrollkästchen.
 
3.  Wählen Sie in der Liste **Benutzer** die Benutzer gruppieren die gewünschte Evidence.com.
 
4.  Klicken Sie in der Fußzeile **zuweisen** .


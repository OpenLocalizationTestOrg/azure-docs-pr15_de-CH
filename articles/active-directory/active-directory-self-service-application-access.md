<properties
    pageTitle="Self-service-Anwendung und delegierte Verwaltung Azure Active Directory | Microsoft Azure"
    description="Dieser Artikel beschreibt das Self-service-Anwendung und delegierte Verwaltung Azure Active Directory."
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
    ms.date="02/09/2016"
    ms.author="asmalser"/>

#<a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Self-service-Anwendung und delegierte Verwaltung Azure Active Directory

Aktivieren von Self-service-Funktionen für Endbenutzer ist ein gängiges Szenario für Unternehmen. Viele Benutzer und viele Programme Person am besten informierten zu Zugriff gewähren möglicherweise nicht die Verzeichnisadministrator. Häufig ist zu entscheiden, wer eine Anwendung zugreifen kann ein Teamleiter oder andere delegierter Administrator. Aber am Ende des Tages ist der Benutzer, die Anwendung verwendet, und der Benutzer weiß, was sie müssen, ihre Aufgaben auszuführen.

Self-service-Anwendung ist ein Feature von [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) , die Directory-Administratoren zulassen:

* Benutzer fordern Zugriff auf Programme mit einer Kachel "Get Anwendung" [Azure AD-Abdeckung](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Festlegen, welche Benutzer Programme auf anfordern können
* Festlegen Sie, ob eine Genehmigung für Benutzer Zugriff auf eine Anwendung selbst zuweisen
* Festlegen die Anfragen genehmigen und Verwalten des Zugriffs für jede Anwendung

Heute ist diese Funktion für alle integrierten und benutzerdefinierten apps unterstützt, Föderations- oder Passwort einmaliges Anmelden im [Anwendungskatalog Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/)einschließlich apps wie Salesforce, Ablage und Google Apps unterstützen.
Dieser Artikel beschreibt, wie Sie:

* Konfigurieren von Self-service-Anwendung für Endbenutzer, einschließlich ein optionalen Genehmigungsworkflows konfigurieren 
* Verwaltung spezifische Anwendung am besten geeigneten Personen in Ihrer Organisation delegieren und aktivieren, Azure AD-Abdeckung auf Anfragen genehmigen, Zugriff auf ausgewählte Benutzer direkt zugewiesen oder Anmeldeinformationen für Zugriff (optional) festgelegt, wenn kennwortbasierte einmaliges Anmelden konfiguriert ist


##<a name="configuring-self-service-application-access"></a>Konfigurieren von Self-service-Anwendung

Aktivieren von Self-service-Anwendung und die Anwendungsmöglichkeiten können oder Ihre Endbenutzer verlangten gehen.

**1:** Das [klassische Azure-Portal](https://manage.windowsazure.com/)anmelden.

**2:**  Abschnitt **Active Directory** wählen Sie das Verzeichnis, und wählen Sie die Registerkarte **Applications** . 

**3:** Wählen Sie die Schaltfläche **Hinzufügen** und die Option Katalog auswählen und eine Anwendung hinzufügen.

**4:** Nach dem Hinzufügen Ihrer app erhalten Sie die Anwendungsseite Quick Start. **Konfigurieren Sie einmaliges Anmelden**auf, wählen Sie den gewünschten einzelnen anmelden und speichern Sie die Konfiguration. 

**5:** Wählen Sie die Registerkarte **Konfigurieren** . Damit Benutzer Zugriff auf diese Anwendung von Azure AD-Abdeckung anfordern können, legen Sie **Self-service-Anwendungszugriff** auf **Ja**.

![][1]

**6:** Konfigurieren Sie optional einen Genehmigungsworkflow für Zugriffe auf **Ja**festgelegt **vor dem Zugriff erforderlich** . Eine oder mehrere genehmigende Personen können dann mithilfe der Schaltfläche **genehmigenden Personen** ausgewählt werden.

Ein Genehmiger kann jeder Benutzer in der Organisation Azure Konto und nannte Konto bereitstellen, Lizenzierung oder anderen Geschäftsprozess der Organisation erforderlich ist, bevor Sie Zugriff auf eine Anwendung. Die genehmigende Person könnte sogar den Besitzer der Gruppe einer oder mehr Gruppen freigegeben und können die Benutzer zu diesen Gruppen Zugriff über ein gemeinsames Benutzerkonto. 

Wenn keine Genehmigung erforderlich ist, erhalten Benutzer sofort die Anwendung ihre Azure AD-Abdeckung hinzugefügt. Diese geeignet, wenn die Anwendung wurde für [automatisiertes provisioning](active-directory-saas-app-provisioning.md)oder ["Benutzer verwaltete" Kennwort SSO-Modus](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) der Benutzer bereits verfügt über ein Benutzerkonto und das Kennwort weiß eingerichtet wurde.

**7:** Wenn die Anwendung konfiguriert wurde, um kennwortbasierte einmaliges Anmelden und dann eine Option ermöglicht die genehmigende Person Festlegen der SSO-Anmeldeinformationen für jeden Benutzer steht. Abschnitt folgenden Delegaten Zugriffsmanagement für Weitere Informationen.

**8:** Schließlich wird die **Gruppe zugewiesene Benutzer** den Namen der Gruppe, die zum Speichern der Benutzer gewährt oder Zugriff auf die Anwendung zugewiesen. Genehmigende Person Zugriff Eigentümer dieser Gruppe. Wenn der Gruppenname angezeigt nicht vorhanden ist, wird es automatisch erstellt. Der Gruppenname kann optional den Namen einer vorhandenen Gruppe festgelegt werden.

**9:** Klicken Sie am unteren Bildschirmrand **Speichern** , um die Konfiguration zu speichern. Jetzt werden Benutzern die Abdeckung Zugriff auf diese Anwendung anfordern können.

**10:** Zum Testen des Endbenutzers in der Organisation Azure AD-Abdeckung am https://myapps.microsoft.com, vorzugsweise mit einem anderen Konto, das genehmigende Person app nicht anmelden. 

**11:** Registerkarte **Applications** klicken Sie **Weitere Programme zu erhalten** . Dadurch wird eine Sammlung von Komponenten, die für Self-service-Anwendung auf das Verzeichnis mit der Möglichkeit zum Suchen und Filtern von app-Kategorie auf der linken Seite aktiviert wurden. 

**12:** Auf eine Anwendung startet den Prozess. Wenn kein Genehmigungsprozess erforderlich ist, wird die Anwendung sofort Registerkarte **Programme** nach einer kurzen Bestätigung hinzugefügt. Genehmigung wird dann ein Dialogfeld dies sehen und e-Mail werden an die Genehmiger gesendet. (Kurzer Hinweis: müssen Sie die Abdeckung als nicht Genehmiger angemeldet werden zu diesem Prozess finden Sie unter).

**13:** E-Mail weist den Genehmiger die Abdeckung Azure AD anmelden und Genehmigung. Nachdem die Anforderung genehmigt und spezielle Prozesse definieren durch die genehmigende Person durchgeführt sehen Benutzer dann die Anwendung unter der Registerkarte **Programme** angezeigt werden, können sie sich wieder anmelden.

##<a name="delegated-application-access-management"></a>Anwendung delegierte Verwaltung

Genehmigende Person Anwendung Zugriff kann jeder Benutzer in der Organisation, der am besten geeignete Person genehmigen oder Verweigern des Zugriffs auf die Anwendung. Dieser Benutzer konnte für Bereitstellung, Lizenzierung oder anderen Geschäftsprozess der Organisation erforderlich ist, bevor Sie Zugriff auf eine Anwendung.
 
Beim Konfigurieren von Self-service-Anwendung beschrieben, sehen eine zugewiesene Anwendung Genehmiger ein zusätzliche **Anwendung verwalten** untereinander Abdeckung Azure AD sie die Anwendungsmöglichkeiten, die sie der Administrator Zugriff. Auf eine Anwendung zeigt einen Bildschirm mit mehreren Optionen.

![][2]

###<a name="approve-requests"></a>Genehmigen von Anfragen

Kachel **Anfragen genehmigen** kann Genehmiger an ausstehende Genehmigung für die Anwendung, und zur Registerkarte Genehmigung, die Anfragen bestätigt oder verweigert werden kann. Beachten Sie, dass die genehmigende Person auch automatisierten e-Mails empfängt, wenn eine Anfrage erstellt wird angewiesen, was zu tun.

###<a name="add-users"></a>Hinzufügen von Benutzern

Die Kachel **Benutzer** kann Genehmiger direkt gewähren Sie ausgewählten Benutzern Zugriff auf die Anwendung. Auf diese Kachel, sieht die genehmigende Person, ein Dialogfeld anzeigen und nach Benutzern im Verzeichnis suchen können. Einem Benutzerergebnisse hinzufügen in der Anwendung angezeigt wird, in die Benutzer Azure AD Access Bereiche oder Office 365 Wenn alle manuellen Benutzerbereitstellungsprozess die App erforderlich, ist bevor der Benutzer anmelden kann, sollten die genehmigende Person dabei führen vor dem Zuweisen des Zugriffs.  

###<a name="manage-users"></a>Verwalten von Benutzern

Die Kachel **Benutzer verwalten** kann Genehmiger direkt aktualisieren oder Entfernen der Benutzer auf die Anwendung zugreifen. 

###<a name="configure-password-sso-credentials-if-applicable"></a>Konfigurieren von SSO-Anmeldeinformationen (falls zutreffend)

**Konfigurieren** Kachel wird nur angezeigt, wenn die Anwendung vom IT-Administrator mit einmaliges Kennwort basierend auf konfiguriert wurde und der Administrator des Genehmigers SSO-Anmeldeinformationen festlegen gewährt, wie zuvor beschrieben. Bei Auswahl dieser Option wird der Genehmiger mehrere Optionen für wie Anmeldeinformationen weitergegeben werden, um Benutzern angezeigt:

![][3]

* **Benutzer ihre eigenen Kennwörter Anmelden** – In diesem Modus zugewiesene Benutzer kennen ihren Benutzernamen und Kennwörter für die Anwendung, und geben sie bei ihrer ersten Anmeldung der Anwendung aufgefordert. Das Kennwort SSO-Fall entspricht, in dem der [Benutzer Anmeldeinformationen verwalten](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

* **Benutzer werden automatisch angemeldet separate Konten verwenden, die ich verwalten** – In diesem Modus zugewiesene Benutzer nicht erforderlich oder anwendungsspezifische Anmeldeinformationen kennen, beim Anmelden bei der Anwendung Stattdessen wird der Genehmiger die Anmeldeinformationen für jeden Benutzer nach dem Zuweisen des Zugriffs über das Anmeldebild für **Benutzer hinzufügen** . Klickt der Benutzer auf die Anwendung in ihrer Abdeckung oder Office 365, werden sie automatisch angemeldet werden die Anmeldeinformationen durch die genehmigende Person festlegen. Das Kennwort SSO-Fall entspricht, in dem [Administratoren Anmeldeinformationen verwalten](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

* **Benutzer werden automatisch mit einem einzigen Konto ich leite angemeldet** : Dies ist ein Sonderfall und wird verwendet, wenn alle zugewiesenen Benutzer verwenden ein gemeinsames Benutzerkonto zugreifen. Der häufigste Anwendungsfall dafür ist sozialer Mediaprogramme, in eine Organisation ist ein einzelnes "Unternehmenskonto" und mehrere Benutzer müssen Updates für dieses Konto. Auch dies Kennwort SSO-Fall, in dem [Administratoren Anmeldeinformationen verwalten](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Jedoch werden nach Auswahl dieser Option die genehmigende Person aufgefordert, den Benutzernamen und das Kennwort für das gemeinsame Konto einzugeben. Abschluss, werden alle zugeordneten Benutzer über dieses Konto, wenn die Anwendung ihre Azure AD Access Bereiche oder Office 365 angemeldet.

##<a name="additional-resources"></a>Zusätzliche Ressourcen
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG

<properties
    pageTitle="Attribut-basierte Anwendung mit Filter Bereiche bereitstellen | Microsoft Azure"
    description="Informationen Sie zum Bereichsfilter verwenden, um Objekte in apps zu verhindern, die unterstützen automatisierte benutzerbereitstellung von tatsächlich bereitgestellt werden, wenn ein Objekt nicht Ihre Bedürfnisse erfüllen."
    services="active-directory"
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Attribut-basierte Anwendung mit Filter Bereiche bereitstellen

Dieser Abschnitt soll Bereichsfilter Verwendung Attribut Regeln definieren, die bestimmt, welche Benutzer der Anwendung bereitgestellt werden.





## <a name="clauses-and-scope-groups"></a>Klauseln und Bereichsgruppen


![Festlegen des Gültigkeitsbereichs von Filtern][1] 




Bereichsfilter definiert eine oder mehrere **Bereichsgruppen**, von denen jede eine oder mehrere **Klauseln**enthalten. Finden Sie die Klauseln für einen bestimmten Bereichsgruppe erweitern Sie auf den Pfeil links neben dem Gruppennamen.

Eine **Klausel** bestimmt, welche Benutzer die Bereichsfilter passieren durch Auswerten des Benutzers Attribute. Beispielsweise müssen Sie eine Klausel, die erfordert, dass 'State'-Attribut des Benutzers New York gleich, sodass nur die Benutzer in New York in der Anwendung bereitgestellt werden.

![Festlegen des Gültigkeitsbereichs von Gruppenname][2] 



Jede **Bereichsgruppe** beginnt mit einem obligatorischen **Klausel**wie in der Abbildung oben dargestellt. Diese Klausel gibt einfach an, dass der Benutzer zunächst die Anwendung zugewiesen werden muss vor der Auswertung durch die Bereichsfilter. Diese Klausel kann nicht gelöscht oder geändert werden.

Neue Klauseln oder neue Bereichsgruppen können durch Betätigen der entsprechenden Schaltfläche. Sie können jede Bereichsgruppe benennen die Eigenschaft **Gruppennamen** bearbeiten.





## <a name="how-scoping-filters-are-evaluated"></a>Wie werden Bereiche Filter ausgewertet

Während der Bereitstellung testen wir alle zugewiesenen Benutzer gegen die Bereichsfilter um festzustellen, ob der Benutzer Zugriff auf die Anwendung verdient. Sie können sich vorstellen jede Klausel als einen Test in der Benutzer zu der erste herausgefiltert übergeben werden muss. 

Haben mehrere Bereichsgruppen definiert muss jeder Benutzer mindestens eine übergeben, um die Anwendung zugreifen. Innerhalb jeder Bereichsgruppe jedoch muss Benutzer jede Klausel bestimmten Bereichsgruppe bestehen. 

Das heißt, Sie können sich vorstellen Bereichsgruppen als zusammengefügt und die Klauseln darin als sozusagen und zusammengefügt. Betrachten Sie beispielsweise die folgenden Bereichsfilter:


![Festlegen des Gültigkeitsbereichs von Gruppenname][2]  


Nach dieser Bereichsfilter müssen Benutzer die folgenden Kriterien erfüllen, um bereitgestellt werden:

1. Sie müssen die Anwendung zugeordnet.

2. Sie müssen die Engineering-Abteilung arbeiten.

3. Arbeit muss in San Francisco oder Kanada.


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Benutzer Bereitstellung und Entfernung SaaS-Anwendung zu automatisieren](active-directory-saas-app-provisioning.md)
- [Anpassen der Attribute Mappings für Benutzer bereitstellen](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Attribute Mappings](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereitstellen von Benachrichtigungen Konto](active-directory-saas-account-provisioning-notifications.md)
- [So aktivieren Sie die automatische Bereitstellung von Benutzern und Gruppen aus dem Active Directory Azure Applications verwenden SCIM](active-directory-scim-provisioning.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png

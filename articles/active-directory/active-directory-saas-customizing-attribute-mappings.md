<properties
    pageTitle="Anpassen der Attribute Mappings | Microsoft Azure"
    description="Erfahren Sie, welche Attribute Mappings für SaaS-apps in Azure Active Directory wie sie Ihre geschäftlichen Bedürfnisse ändern."
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


# <a name="customizing-attribute-mappings"></a>Anpassen der Attribute Mappings


Microsoft Azure AD bietet Unterstützung für Benutzer bereitstellen, SaaS Drittanbietern wie Salesforce und Google Apps. Haben benutzerbereitstellung für Dritte SaaS-Anwendung aktiviert wird, steuert der Azure-Verwaltungsportal Attributwert in Form einer Testkonfiguration "attributzuordnung".

Gibt eine vorkonfigurierte Attribut Zuordnungen zwischen Azure AD Benutzer- und jede SaaS-app Benutzerobjekte. Einige apps Verwalten anderer Typen von Objekten, Gruppen oder Kontakte. <br> 
Sie können das Attribut Standardmappings Ihren geschäftlichen Bedürfnissen anpassen. Dies bedeutet, Sie können ändern Attribut Zuordnung löschen oder neue Attribut Zuordnung erstellen.

In Azure AD-Portal können Sie dieses Feature zugreifen, indem Sie Attribute auf eine SaaS-Anwendung auf.

> [AZURE.NOTE] **Attribute** Link ist nur verfügbar, wenn benutzerbereitstellung für eine SaaS-Anwendung aktiviert. 


![Salesforce][1] 


Wenn Sie Attribute in der Symbolleiste der Liste der aktuellen Zuordnungen klicken, die für eine SaaS-Anwendung konfiguriert.

Der folgende Screenshot zeigt ein Beispiel dafür:



![Salesforce][2]  


Im obigen Beispiel können Sie sehen, dass **FirstName** -Attribut eines verwalteten Objekts in Salesforce **GivenName** Wert des enthält Azure AD-Objekt.

Wenn Sie Zuordnungen Attribut anpassen oder zurücksetzen möchten auf die Konfiguration angepasste, können Sie dazu zugehörige Schaltfläche auf der Symbolleiste am unteren Rand einer Anwendung.


![Salesforce][3]  


Attribut-Zuordnung, die eine SaaS-Anwendung ordnungsgemäß erforderlich sind. In der Attributtabelle haben Zuordnung verknüpfte Attribut **Ja** als Wert für das Attribut **erforderlich** . Wenn eine attributzuordnung erforderlich ist, können Sie nicht löschen. In diesem Fall ist **Löschen** -Funktion nicht verfügbar.

Um ein attributzuordnung zu ändern, wählen Sie einfach die Zuordnung und dann auf **Bearbeiten**. Dadurch wird ein dialogfeldseite, die die ausgewählte attributzuordnung ändern kann.


![Attributzuordnung bearbeiten][4]  



## <a name="understanding-attribute-mapping-types"></a>Grundlegendes zum Attribut Mapping-Typen


Mit Attribut Mappings Partei Steuerelement gefüllt wie Attribute in einem Drittland SaaS-Anwendung. Es gibt vier verschiedene Zuordnungstypen unterstützt:

- **Direkte** – das Zielattribut wird durch den Wert eines Attributs des verknüpften Objekts in Azure AD gefüllt.


- **Konstanten** – das Zielattribut aufgefüllt, mit einer bestimmten Zeichenfolge angegebene.


- **Ausdruck** - das Zielattribut wird basierend auf dem Ergebnis eines Ausdrucks, wie Skripts gefüllt. Weitere Informationen finden Sie unter [Schreiben von Expressions für Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Keine** - das Zielattribut links unverändert ist. Wenn das Zielattribut immer leer ist, wird es mit dem Standardwert aufgefüllt, die Sie angeben.



Neben diesen vier grundlegende Zuordnung Attributtypen unterstützen benutzerdefinierte Attribut Zuordnungen das Konzept eine **Standard** -Zuweisung. Die Wert Zuordnung wird sichergestellt, dass Zielattribut mit einem Wert aufgefüllt wird, wenn kein Wert in Azure AD noch für das Zielobjekt.

Microsoft Azure AD bietet eine sehr effiziente Implementierung eine Synchronisierung. In einer Umgebung initialisiert werden nur Objekte ausnutzen Zyklus Synchronisierung verarbeitet. Attribut Mappings Aktualisierung wirkt sich auf die Leistung der Synchronisierung ab. Ist eine Aktualisierung Zuordnungskonfiguration Attribut erfordert alle verwalteten Objekte zu. Aus diesem Grund ist es empfiehlt es sich die Anzahl der aufeinander folgenden Zuordnungen Attribut mindestens zu.


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Benutzer Bereitstellung/aufheben der SaaS-Apps zu automatisieren](active-directory-saas-app-provisioning.md)
- [Schreiben von Ausdrücken für Attribute Mappings](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsfilter für Benutzer bereitstellen](active-directory-saas-scoping-filters.md)
- [So aktivieren Sie die automatische Bereitstellung von Benutzern und Gruppen aus dem Active Directory Azure Applications verwenden SCIM](active-directory-scim-provisioning.md)
- [Bereitstellen von Benachrichtigungen Konto](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png

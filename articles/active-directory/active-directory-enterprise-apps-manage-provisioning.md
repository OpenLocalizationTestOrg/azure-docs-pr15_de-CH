<properties
    pageTitle="Benutzer-Management für Unternehmen apps in der Vorschau Azure Active Directory-Bereitstellung | Microsoft Azure"
    description="Verwalten Sie benutzerbereitstellung für Enterprise-apps mithilfe von Azure Active Directory-Vorschau"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Vorschau: Benutzerkonto bereitstellen für Enterprise-apps im neuen Azure-Portal verwalten

Dieser Artikel beschreibt, wie mit der [Azure-Portal](https://portal.azure.com) verwalten automatische Benutzerkonto erteilen und entziehen für Applikationen, die insbesondere unterstützen, die aus der Kategorie "Top" [Azure Active Directory Anwendungskatalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)hinzugefügt wurden. Dieser Artikel beschreibt die neuen Funktionen sowie einige temporäre Einschränkungen während der Probezeit sind diese Verwaltungsoption im neuen Azure-Portal ist derzeit public Preview-Version [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md)

Automatisiertes Bereitstellung und deren Funktionsweise finden Sie unter [automatisieren Benutzer und Deprovisioning SaaS-Anwendung in Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Suchen Ihre apps in das neue portal

Ab September 2016 können Anträge für einzelne konfiguriert wurden in einem Verzeichnis ein Verzeichnis Administrator [Azure Active Directory Anwendungskatalog](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) im [klassischen Azure-Portal](https://manage.windowsazure.com)anmelden, jetzt angezeigt und verwaltet werden im neuen Azure-Portal.

Diesen Abschnitt **Anwendung** neue Azure-Portal finden Sie die über das Menü **Mehr Dienste** im linken Navigationsbereich zugegriffen werden kann. Enterprise-apps sind apps, die bereitgestellt wurden und von Benutzern in Ihrer Organisation verwendet werden.

![NTD-blade][0]

**Alle** diese Verknüpfung auf der linken Seite zeigt eine Liste aller Apps konfiguriert wurden einschließlich apps, die aus der Galerie hinzugefügt wurde. Auswählen einer Anwendung lädt Ressourcen Blade App, wo Berichte können für die Anwendung und Einstellungen verwaltet werden kann.

**Bereitstellung** auf der linken Seite auswählen kann Benutzerkonto Einstellungen Bereitstellung verwaltet werden.

![Anwendung Ressource blade][1]


##<a name="provisioning-modes"></a>Modi-Bereitstellung

**Provisioning** -Blade beginnt mit einem **Modus** , der angezeigt wird, welche Bereitstellung Modi für eine Enterprise-Anwendung unterstützt werden, und kann konfiguriert werden. Die Optionen sind verfügbar:

* **Automatische** – diese Option wird angezeigt, wenn Azure AD automatische API-basierte Bereitstellung bzw. Aufheben der Bereitstellung von Benutzerkonten dieser Anwendung unterstützt. Eine Schnittstelle, die führt Administratoren durch Konfigurieren von Azure AD Verbindung mit der Anwendung Benutzermanagement-API, Kontenzuordnungen und Workflows, die definieren, wie Benutzer Konto zwischen Azure Datenfluss sollten diesen Modus auswählen angezeigt und die Anwendung und Verwalten von Azure AD Service-Bereitstellung.

* **Handbuch** – diese Option wird angezeigt, wenn Azure AD automatische Bereitstellung von Benutzerkonten in dieser Anwendung nicht unterstützt. Diese Option bedeutet, dass Benutzer in der Anwendung gespeicherte Datensätzen einen externen Prozess basierend auf den Benutzer-Management und provisioning Funktionen dieser Anwendung (SAML Just-in-Time-Bereitstellung enthalten kann) verwaltet werden müssen.


##<a name="configuring-automatic-user-account-provisioning"></a>Automatisiertes Bereitstellung konfigurieren

Die **Automatische** Option zeigt ein Fenster an, die in vier Abschnitte unterteilt:

###<a name="admin-credentials"></a>Administratorinformationen
Dies ist die Anmeldeinformationen für Azure AD Verbindung der Anwendung Verwaltung API eingegeben, erforderlich. Der Aufwand hängt von der Anwendung ab. Über die Anmeldeinformationen und für bestimmte Applikationen finden Sie unter [Konfiguration Tutorial für die spezifische Anwendung](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Schaltfläche **Verbindung testen** können Sie die Anmeldeinformationen mit Azure testen AD Verbindungsversuch mit der Anwendung der Anwendung mit den angegebenen Anmeldeinformationen bereitstellen.

###<a name="mappings"></a>Zuordnung
Wo Administratoren können anzeigen und welche Benutzer Attribute Flow zwischen Azure bearbeiten und die Anwendung, wenn Benutzerkonten bereitgestellt oder aktualisiert werden.

Gibt eine vorkonfigurierte Zuordnungen zwischen Azure AD Benutzer- und jede SaaS-app Benutzerobjekte. Einige apps Verwalten anderer Typen von Objekten, Gruppen oder Kontakte. Auswählen eines diese Zuordnung in der Tabelle zeigt den Editor auf der rechten Seite, wo sie angezeigt und angepasst werden können.

![Anwendung Ressource blade][2]

Unterstützte Anpassungen während der ersten Vorschau einschließen:

* Aktivieren und Deaktivieren von Mappings für bestimmte Objekte wie Azure AD-Benutzerobjekt Benutzerobjekt SaaS-app.

* Bearbeiten die Attribute von Azure AD-Benutzerobjekt in der app-Benutzerobjekt fließen. Weitere Informationen zum Attribut zuordnen finden Sie unter [Understanding Attributtypen Zuordnung](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Filteraktionen Sie welche Bereitstellung Azure AD auf das Zielprogramm ist ein neues Feature in Azure-Portal durchführen soll. Anstatt Azure AD Objekte synchronisieren, können Sie die Aktionen einschränken. Beispielsweise nur auswählen von **Update**Azure AD nur Updates vorhandene Benutzerkonten in einer Anwendung und erstellt keine neue. Wählen Sie nur **Erstellen**, Azure erstellt neue Benutzerkonten nur nicht vorhandene zu aktualisieren. Dieses Feature ermöglicht Administratoren zu anderen Zuordnungen für Erstellung Workflows. Vollständige können mehrere Schemen pro Anwendung erstellen wird später in der Probezeit vorgesehen.

###<a name="settings"></a>Einstellungen
In diesem Abschnitt kann Administratoren starten Azure AD provisioning Service für die ausgewählte Anwendung beenden sowie optional Bereitstellung Cache löschen und den Dienst neu starten.

Bereitstellung einer Anwendung erstmals aktiviert wird, aktivieren Sie den Dienst durch Ändern des **Status Bereitstellung** **auf**. Dadurch wird Azure AD Service bereitstellen einen anfänglichen Synchronisierung ausführen, die **Benutzer** und Gruppen zugewiesenen Benutzer liest, fragt die Anwendung für sie und führt dann die Bereitstellung im Abschnitt Azure AD **Mappings** definierten Aktionen. Dabei speichert provisioning Service zwischengespeicherte Daten über welche Benutzerkonten verwaltet, damit nicht verwaltete Konten innerhalb der nie in Bereich für Zuweisung Zielbereiche Entziehen von Operationen betroffen sind nicht. Nach der anfänglichen Synchronisierung synchronisiert provisioning Service automatisch Benutzer- und Gruppenkonten auf zehn Minuten.

Ändern des **Status Bereitstellung** **zu** hält einfach provisioning Service. In diesem Zustand Azure nicht erstellen, aktualisieren oder entfernen Sie alle Benutzer oder Objekte in der Anwendung. Ändern des Zustands, in wird Dienst, wo sie aufgehört.

**Deaktivieren Sie Stand und Synchronisierung starten** das Kontrollkästchen aktivieren und Speichern beendet den Dienst Bereitstellung speichert zwischengespeicherten Daten über welche Konten Azure AD ist, startet die Dienste neu und führt die anfängliche Synchronisierung erneut. Mit dieser Option können Administratoren den Bereitstellung Bereitstellungsprozess wiederholen.

###<a name="synchronization-details"></a>Synchronisierungsdetails
Dieser Abschnitt enthält Details hinzufügen den Betrieb der Bereitstellung, einschließlich der ersten und letzten Lief provisioning Service der Anwendung und wie viele Benutzer- und Gruppenkonten verwaltet werden.

Links sind und **Aktivitätsbericht Provisioning**bietet ein Protokoll aller Benutzer und Gruppen erstellt, aktualisiert und entfernt zwischen Azure AD Target Anwendung und **Provisioning Bericht** bietet detaillierte Fehlermeldungen für Benutzer- und Gruppenobjekte, die nicht gelesen, erstellt, aktualisiert oder entfernt. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG

<properties
    pageTitle="Azure AD Verbindung synchronisieren: Grundlegendes zu Benutzern und Kontakten | Microsoft Azure"
    description="Erläutert Benutzern und Kontakten in Azure AD-Verbindung synchronisieren."
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
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Verbindung synchronisieren: Grundlegendes zu Benutzern und Kontakten

Es gibt verschiedene Gründe Warum mehrere Active Directory-Gesamtstrukturen müssen und es gibt mehrere verschiedene Bereitstellungstopologien. Allgemeine Modelle sind eine Ressource Bereitstellung und GAL sync'ed Gesamtstrukturen eine Fusion und Akquisition. Aber auch wenn reine Modelle vorhanden sind, Hybrid-Modelle sind ebenfalls. Die Standardkonfiguration in Azure AD Connect Sync übernimmt ein bestimmtes Modell jedoch je nach wie übereinstimmenden Benutzer im Installationshandbuch ausgewählt wurde, können unterschiedliche Verhaltensweisen beobachtet werden.

In diesem Abschnitt gehen wir durch die Standardkonfiguration in bestimmten Topologien Verhalten. Wir gehen über die Konfiguration und Synchronisierung Regel-Editor kann sich die Konfiguration verwendet werden.

Es gibt einige allgemeine Regeln, die der Konfiguration:

- Unabhängig von der Reihenfolge wir von der Quelle aus Active Directory importieren, sollte das Ergebnis immer übereinstimmen.
- Ein aktives Konto trägt immer Anmeldeinformationen, einschließlich **UserPrincipalName** und **SourceAnchor**.
- Ein deaktiviertes Konto trägt UserPrincipalName und SourceAnchor, es sei ein verknüpftes Postfach ist kein aktives Konto gefunden werden.
- Ein Konto mit einer verknüpften Postfachs wird nie UserPrincipalName und SourceAnchor verwendet werden. Es wird angenommen, dass ein aktives Konto später finden.
- Ein Kontaktobjekt kann als Kontakt oder als Azure AD bereitgestellt. Sie wissen nicht wirklich, bis alle Active Directory-Gesamtstrukturen mit Quelle verarbeitet wurden.

## <a name="contacts"></a>Kontakte

Kontakte, die einen Benutzer in einer anderen Gesamtstruktur darstellt gilt eine Fusion und Akquisition, GALSync Lösung zwei oder mehrere Exchange-Gesamtstrukturen überbrückt. Das Kontaktobjekt tritt immer aus dem Connectorspace in das Metaversum das Mail-Attribut verwenden. Wenn bereits ein Kontaktobjekt oder Benutzerobjekt mit derselben e-Mail-Adresse ist, werden die Objekte zusammengefügt. Dies wird in der Regel konfiguriert **In aus AD-Kontakt verknüpfen**. Ist auch eine Regel namens **In aus AD-Kontakt gemeinsamen** mit dem Metaverse Attribute-Attribut **Quellobjekttyp** mit ständiger **Kontakt**. Diese Regel ist sehr niedrige Priorität Wenn aller Benutzerobjekte mit derselben Metaverse-Objekt und die Regel verknüpft ist **In aus AD-Benutzer häufig** trägt den Benutzer Wert dieses Attributs. Mit dieser Regel müssen dieses Attributs den Kontakt, wenn kein Benutzer verbunden ist und dem Wert der Benutzer mindestens einen Benutzer gefunden wurde.

Für ein Objekt in Azure AD Bereitstellung erstellt ausgehende Regel **zum AAD – Kontakt verknüpfen** eines, wenn Metaverse-Attribut **Quellobjekttyp** **an**festgelegt ist. Wenn **Benutzer**dieses Attribut festgelegt ist, dann die Regel **zum AAD – Benutzer beitreten** ein Benutzerobjekts stattdessen erstellt.
Es ist möglich, dass ein Objekt hochgestuft wird Kontakt Benutzer weitere Quelle Active Directory importiert und synchronisiert werden.

Beispielsweise in einer Topologie GALSync finden wir Kontaktobjekte für alle in der zweiten Gesamtstruktur beim Importieren von der ersten Gesamtstruktur. Dies wird neuen Kontaktobjekte in AAD Connector bereitstellen. Wenn wir später importieren und die zweite Gesamtstruktur synchronisieren, wir die tatsächlichen Benutzer und mit vorhandenen Metaverse-Objekten verknüpfen. Wir werden das Kontaktobjekt in AAD löschen und stattdessen ein neues Benutzerobjekt zu erstellen.

Haben Sie eine Topologie, Benutzer und als Kontakte wählen Benutzer das Attribut Mail im Installationshandbuch übereinstimmen. Wenn Sie eine andere Option auswählen, müssen eine abhängige Konfiguration Sie. Kontaktobjekte treten immer auf das Mail-Attribut, aber Benutzerobjekte treten nur auf das Mail-Attribut dieser Option wurde im Installationshandbuch. Sie könnten dann am Ende zwei verschiedene Objekte im Metaverse in dasselbe e-Mail-Attribut das Kontaktobjekt vor dem Benutzerobjekt importiert. Beim Exportieren in Azure AD wird ein Fehler ausgelöst. Dieses Verhalten ist beabsichtigt und würde fehlerhafte Daten oder die Topologie während der Installation nicht erkannt wurde.

## <a name="disabled-accounts"></a>Deaktivierte Konten

Deaktivierte Konten werden auch Azure AD synchronisiert. Deaktivierte Konten können Ressourcen in Exchange beispielsweise Konferenzräume darstellen werden. Benutzer mit einem Postfach verknüpften gilt; Wie bereits erwähnt werden diese nie ein Konto Azure AD bereitstellen.

Die Annahme ist, wenn ein deaktiviertes Benutzerkonto befindet, und wir ein anderes Aktives Konto später nicht finden das Objekt Azure AD mit UserPrincipalName gefunden SourceAnchor bereitgestellt. Bei einem anderen aktiven Konto dieselbe Metaverse-Objekt hinzugefügt werden soll, werden die UserPrincipalName und SourceAnchor verwendet werden.

## <a name="changing-sourceanchor"></a>SourceAnchor ändern

Wenn ein Objekt in Azure AD exportiert wurde, dann darf der SourceAnchor nicht mehr ändern. Wenn das Objekt exportiert wurde ist Metaverse-Attribut **CloudSourceAnchor** anerkannten Azure AD **SourceAnchor** -Wert festgelegt. Wenn **SourceAnchor** geändert und nicht mit **CloudSourceAnchor**überein, löst die Regel **zum AAD – Benutzer beitreten** Fehler **SourceAnchor Attribut geändert**. In diesem Fall müssen die Konfiguration oder Daten korrigiert werden also dieselbe SourceAnchor in das Metaverse vorhanden, bevor das Objekt wieder synchronisiert werden kann.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure AD Verbindung synchronisieren: Anpassen von Synchronisierungsoptionen](active-directory-aadconnectsync-whatis.md)
* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

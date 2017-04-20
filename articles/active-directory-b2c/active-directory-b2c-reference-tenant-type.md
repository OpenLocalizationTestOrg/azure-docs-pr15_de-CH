<properties
    pageTitle="Azure Active Directory B2C: Produktions-Skalierung und Vorschau B2C Mieter | Microsoft Azure"
    description="Auf welche Azure Active Directory B2C Mieter"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Produktions-Skalierung und Vorschau B2C Mieter

Falls Sie Azure Active Directory (Azure AD) B2C produktionsanwendung schreiben, müssen Sie bestimmte sein, dass Sie den richtigen Mieter "Typ" Live auf. Sehen, folgendermaßen navigieren [B2C Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure-Portal und suchen unter **Tenant-Typ**.

## <a name="summary"></a>Zusammenfassung

Azure AD B2C unterstützt nur auf **Produktion Skala** B2C in Nordamerika Mieter Produktion apps.

| Tenant-Typ | Länder/Regionen | Erhältlich? |
| ----------- | -------------- | --------------------- |
| **Produktion-Skala Mieter** | Nordamerikanischen Ländern | Ja |
| **Produktion-Skala Mieter** | Alle Länder/Regionen außer Nordamerika | Nein |
| **Vorschau Mieter** | Alle Länder/Regionen | Nein |

> [AZURE.NOTE]
Azure AD B2C Mieter (für Verbraucher) sind in einigen Ländern oder Regionen, in Azure AD Mieter (Mitarbeiter) verfügbar sind, derzeit nicht verfügbar. Lesen Sie weitere Informationen in den folgenden Abschnitten.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Produktion-Skala B2C Mieter in Nordamerika

Wenn [Ihrem B2C-Mandanten erstellt](active-directory-b2c-get-started.md) in Nordamerika, z.B. in einem der folgenden Länder oder Regionen: Vereinigte Staaten, Kanada, Costa Rica, Ecuador, El Salvador, Guatemala, Mexiko, Panama, Puerto Rico Trinidad und Tobago, **Mieter Typ** B2C-Administratoroberfläche sagt **Produktion skalieren**und Ihrem Mandanten für Produktions-apps verwendet werden kann.

> [AZURE.NOTE]
Produktion-Skala Mieter können auf 100 Millionen Kundenidentitäten pro Mandant s.

![Screenshot der Produktion Skala Mieter](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Vorschau B2C Mieter in jedem Land

B2C Mieter bei Azure AD B2C Probezeit erstellt haben, ist wahrscheinlich der **Mieter geben** sagt **Mieter Vorschau**. Wenn dies der Fall ist, müssen Sie Ihrem Mandanten nur für Entwicklungs- und Testzwecke und nicht für Produktions-apps verwenden.

> [AZURE.IMPORTANT]
Es ist kein Migrationspfad von einer Vorschau B2C Mieter Mieter B2C Produktion skalieren. Hinweis es bekannte Probleme bei Vorschau B2C-Mandanten löschen und neu Produktion Skala B2C Mieter mit dem gleichen Domänennamen erstellen. Sie müssen mit einem anderen Domänennamen Produktion Skala B2C Mieter erstellen.

![Screenshot der Vorschau Mieter](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Produktion-Skala B2C Mieter außerhalb von Nordamerika

Azure AD B2C ist derzeit nicht allgemein verfügbare außerhalb von Nordamerika. Jedoch können Sie erstellen und Verwenden von Produktion Skala Mieter für Entwicklung und testing Zwecke in einem der folgenden Länder oder Regionen: Algerien, Österreich, Aserbaidschan, Bahrain, Belarus, Belgien, Bulgarien, Kroatien, Zypern, Tschechien, Dänemark, Ägypten, Estland, Finnland, Frankreich, Deutschland, Griechenland, Ungarn, Island, Irland, Israel, Italien, Jordanien, Kasachstan, Kenia, Kuwait, Lettland, Libanon, Liechtenstein, Litauen, Luxemburg, ehemalige jugoslawische Republik Mazedonien, Malta, Montenegro, Marokko, Niederlande, Nigeria, Norwegen , Oman, Pakistan, Polen, Portugal, Katar, Rumänien, Russland, Saudi-Arabien, Serbien, Slowakei, Slowenien, Südafrika, Spanien, Schwedens, Schweiz, Tunesien, Türkei, Ukraine, Vereinigte Arabische Emirate und Großbritannien.

Nach Azure AD B2C allgemeinen Verfügbarkeit in Ländern oder Regionen gibt, können Sie weiterhin diese Produktion Skala Mieter und Go live mit Ihrer Produktion apps ohne Datenverlust.

## <a name="availability-of-b2c-tenants"></a>Verfügbarkeit der B2C-Mandanten

B2C Mieter stehen derzeit in folgenden Ländern oder Regionen: Afghanistan, Argentinien, Australien, Brasilien, Chile, Kolumbien, Ecuador, Hongkong, Indien, Indonesien, Irak, Japan, Korea, Malaysia, Neuseeland, Paraguay, Peru, Philippinen, Singapur, Sri Lanka, Taiwan, Thailand, Uruguay und Venezuela. Wir möchten in Zukunft einschließen.

<properties
pageTitle="MailChimp | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. MailChimp ist ein SaaS-Dienst, der können Unternehmen verwalten und automatisieren e-Mail-marketing-Aktivitäten, einschließlich Newsletter, automatisierte Nachrichten und gezielte Kampagnen senden."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>Erste Schritte mit MailChimp-Anschluss

MailChimp ist ein SaaS-Dienst, der können Unternehmen verwalten und automatisieren e-Mail-marketing-Aktivitäten, einschließlich Newsletter, automatisierte Nachrichten und gezielte Kampagnen senden.


>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion.

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

MailChimp Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten.

 MailChimp Konnektor hat folgende Aktionen und Triggern verfügbar:

### <a name="mailchimp-actions"></a>MailChimp Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Erstellen Sie eine neue Kampagne ein Kampagnentyp Empfängerliste und Einstellungen (Betreff, Titel, From_name und Reply_to)|
|[NewList](connectors-create-api-mailchimp.md#newlist)|Erstellen Sie eine neue Liste in Ihrem Konto MailChimp|
|[AddMember](connectors-create-api-mailchimp.md#addmember)|Hinzufügen oder Aktualisieren eines Mitglieds|
|[RemoveMember](connectors-create-api-mailchimp.md#removemember)|Löschen Sie ein Element aus einer Liste.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Informationen für ein bestimmtes Mitglied|
### <a name="mailchimp-triggers"></a>MailChimp Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn eine Liste ein Element hinzugefügt wurde|Einen Workflow auslöst, wenn eine Liste ein neues Element hinzugefügt wurde|
|Erstellung eine neue Liste|Einen Workflow ausgelöst wird, wenn eine neue Liste erstellt|


## <a name="create-a-connection-to-mailchimp"></a>Erstellen einer Verbindung mit MailChimp
Um Logik apps mit MailChimp erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Anmeldeinformationen für MailChimp|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="reference-for-mailchimp"></a>Referenz für MailChimp
Gilt für Version: 1.0

## <a name="newcampaign"></a>newcampaign
Neue Kampagne: Erstellen einer neuen Kampagne basierend auf ein Kampagnentyp Empfängerliste und Einstellungen (Betreff, Titel, From_name und Reply_to)

```POST: /campaigns```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|newCampaignRequest| |Ja|Körper|keine|JSON-Objekt mit der neuen Kampagnenparameter Anforderung senden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="newlist"></a>NewList
Neue Liste: Erstellen einer neuen Liste in Ihrem Konto MailChimp

```POST: /lists```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|newListRequest| |Ja|Körper|keine|JSON-Objekt mit der neuen Kampagnenparameter Anforderung senden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="addmember"></a>AddMember
Mitglied hinzufügen: Hinzufügen oder aktualisieren ein Mitglieds

```POST: /lists/{list_id}/members```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|
|newMemberInList| |Ja|Körper|keine|JSON-Objekt mit den neuen Mitgliedstaaten Informationen senden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="removemember"></a>RemoveMember
Mitglied aus Liste entfernen: Löschen Sie ein Element aus einer Liste.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|
|member_email|Zeichenfolge|Ja|Pfad|keine|Die e-Mail-Adresse des zu löschenden Members|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="updatemember"></a>updatemember
Informationen aktualisieren: Aktualisieren Sie Informationen für einen bestimmten Member

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|
|member_email|Zeichenfolge|Ja|Pfad|keine|Eindeutige e-Mail-Adresse des Mitglieds aktualisieren|
|updateMemberInListRequest| |Ja|Körper|keine|JSON-Objekt mit den aktualisierten Informationen senden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Wenn ein Element einer Liste hinzugefügt wurde: einen Workflow auslöst, wenn eine Liste ein neues Element hinzugefügt wurde

```GET: /trigger/lists/{list_id}/members```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|Zeichenfolge|Ja|Pfad|keine|Die eindeutige Id für die Liste|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="oncreatelist"></a>OnCreateList
Erstellung eine neue Liste: löst einen Workflow beim Erstellen eine neue Liste

```GET: /trigger/lists```

Es gibt keine Parameter für diesen Aufruf
#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Typ|Zeichenfolge|Ja |
|Empfänger|nicht definiert|Ja |
|Einstellungen|nicht definiert|Ja |
|variate_settings|nicht definiert|Nein |
|Überwachung|nicht definiert|Nein |
|rss_opts|nicht definiert|Nein |
|social_card|nicht definiert|Nein |



### <a name="recipient"></a>Empfänger


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list_id|Zeichenfolge|Ja |
|segment_opts|nicht definiert|Nein |



### <a name="settings"></a>Einstellungen


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|subject_line|Zeichenfolge|Ja |
|Titel|Zeichenfolge|Nein |
|from_name|Zeichenfolge|Ja |
|reply_to|Zeichenfolge|Ja |
|use_conversation|boolescher Wert|Nein |
|to_name|Zeichenfolge|Nein |
|folder_id|ganze Zahl|Nein |
|Authentifizierung|boolescher Wert|Nein |
|auto_footer|boolescher Wert|Nein |
|inline_css|boolescher Wert|Nein |
|auto_tweet|boolescher Wert|Nein |
|auto_fb_post|Array|Nein |
|fb_comments|boolescher Wert|Nein |



### <a name="variatesettings"></a>Variate_Settings


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|winner_criteria|Zeichenfolge|Nein |
|Wartezeit|ganze Zahl|Nein |
|test_size|ganze Zahl|Nein |
|subject_lines|Array|Nein |
|send_times|Array|Nein |
|from_names|Array|Nein |
|reply_to_addresses|Array|Nein |



### <a name="tracking"></a>Überwachung


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Öffnet|boolescher Wert|Nein |
|html_clicks|boolescher Wert|Nein |
|text_clicks|boolescher Wert|Nein |
|goal_tracking|boolescher Wert|Nein |
|ecomm360|boolescher Wert|Nein |
|google_analytics|Zeichenfolge|Nein |
|clicktale|Zeichenfolge|Nein |
|Salesforce|nicht definiert|Nein |
|Hochhaus|nicht definiert|Nein |
|"Kapseln"|nicht definiert|Nein |



### <a name="rssopts"></a>RSS_Opts


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|feed_url|Zeichenfolge|Nein |
|Häufigkeit|Zeichenfolge|Nein |
|constrain_rss_img|Zeichenfolge|Nein |
|Zeitplan|nicht definiert|Nein |



### <a name="socialcard"></a>Social_Card


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|image_url|Zeichenfolge|Nein |
|Beschreibung|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="segmentopts"></a>Segment_Opts


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|saved_segment_id|ganze Zahl|Nein |
|Übereinstimmung|Zeichenfolge|Nein |



### <a name="salesforce"></a>Salesforce


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Kampagne|boolescher Wert|Nein |
|Notizen|boolescher Wert|Nein |



### <a name="highrise"></a>Hochhaus


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Kampagne|boolescher Wert|Nein |
|Notizen|boolescher Wert|Nein |



### <a name="capsule"></a>"Kapseln"


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Notizen|boolescher Wert|Nein |



### <a name="schedule"></a>Zeitplan


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Stunde|ganze Zahl|Nein |
|daily_send|nicht definiert|Nein |
|weekly_send_day|Zeichenfolge|Nein |
|monthly_send_date|Anzahl|Nein |



### <a name="dailysend"></a>Daily_Send


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Sonntag|boolescher Wert|Nein |
|Montag|boolescher Wert|Nein |
|Dienstag|boolescher Wert|Nein |
|Mittwoch|boolescher Wert|Nein |
|Donnerstag|boolescher Wert|Nein |
|Freitag|boolescher Wert|Nein |
|Samstag|boolescher Wert|Nein |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|create_time|Zeichenfolge|Nein |
|archive_url|Zeichenfolge|Nein |
|Status|Zeichenfolge|Nein |
|emails_sent|ganze Zahl|Nein |
|send_time|Zeichenfolge|Nein |
|content_type|Zeichenfolge|Nein |
|Empfänger|Array|Nein |
|Einstellungen|nicht definiert|Nein |
|variate_settings|nicht definiert|Nein |
|Überwachung|nicht definiert|Nein |
|rss_opts|nicht definiert|Nein |
|ab_split_opts|nicht definiert|Nein |
|social_card|nicht definiert|Nein |
|report_summary|nicht definiert|Nein |
|delivery_status|nicht definiert|Nein |
|_links|Array|Nein |



### <a name="absplitopts"></a>AB_Split_Opts


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|split_test|Zeichenfolge|Nein |
|pick_winner|Zeichenfolge|Nein |
|wait_units|Zeichenfolge|Nein |
|Wartezeit|ganze Zahl|Nein |
|split_size|ganze Zahl|Nein |
|from_name_a|Zeichenfolge|Nein |
|from_name_b|Zeichenfolge|Nein |
|reply_email_a|Zeichenfolge|Nein |
|reply_email_b|Zeichenfolge|Nein |
|subject_a|Zeichenfolge|Nein |
|subject_b|Zeichenfolge|Nein |
|send_time_a|Zeichenfolge|Nein |
|send_time_b|Zeichenfolge|Nein |
|send_time_winner|Zeichenfolge|Nein |



### <a name="reportsummary"></a>Report_Summary


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Öffnet|ganze Zahl|Nein |
|unique_opens|ganze Zahl|Nein |
|open_rate|Anzahl|Nein |
|klickt|ganze Zahl|Nein |
|subscriber_clicks|Anzahl|Nein |
|click_rate|Anzahl|Nein |



### <a name="deliverystatus"></a>Delivery_Status


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|aktiviert|boolescher Wert|Nein |
|can_cancel|boolescher Wert|Nein |
|Status|Zeichenfolge|Nein |
|emails_sent|ganze Zahl|Nein |
|emails_canceled|ganze Zahl|Nein |



### <a name="link"></a>Link


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|rel|Zeichenfolge|Nein |
|href|Zeichenfolge|Nein |
|-Methode|Zeichenfolge|Nein |
|targetSchema|Zeichenfolge|Nein |
|Schema|Zeichenfolge|Nein |



### <a name="newlistrequest"></a>NewListRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Ja |
|Wenden Sie sich an|nicht definiert|Ja |
|permission_reminder|Zeichenfolge|Ja |
|use_archive_bar|boolescher Wert|Nein |
|campaign_defaults|nicht definiert|Ja |
|notify_on_subscribe|Zeichenfolge|Nein |
|notify_on_unsubscribe|Zeichenfolge|Nein |
|email_type_option|boolescher Wert|Ja |
|Sichtbarkeit|Zeichenfolge|Nein |



### <a name="contact"></a>Kontakt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Unternehmen|Zeichenfolge|Ja |
|Adresse1|Zeichenfolge|Ja |
|Adresse2|Zeichenfolge|Nein |
|Ort|Zeichenfolge|Ja |
|Zustand|Zeichenfolge|Ja |
|ZIP|Zeichenfolge|Ja |
|Land|Zeichenfolge|Ja |
|Telefon|Zeichenfolge|Ja |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|from_name|Zeichenfolge|Ja |
|from_email|Zeichenfolge|Ja |
|Betreff|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Ja |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|Name|Zeichenfolge|Ja |
|Wenden Sie sich an|nicht definiert|Ja |
|permission_reminder|Zeichenfolge|Ja |
|use_archive_bar|boolescher Wert|Nein |
|campaign_defaults|nicht definiert|Ja |
|notify_on_subscribe|Zeichenfolge|Nein |
|notify_on_unsubscribe|Zeichenfolge|Nein |
|date_created|Zeichenfolge|Nein |
|list_rating|ganze Zahl|Nein |
|email_type_option|boolescher Wert|Ja |
|subscribe_url_short|Zeichenfolge|Nein |
|subscribe_url_long|Zeichenfolge|Nein |
|beamer_address|Zeichenfolge|Nein |
|Sichtbarkeit|Zeichenfolge|Nein |
|Module|Array|Nein |
|Statistiken|nicht definiert|Nein |
|_links|Array|Nein |



### <a name="stats"></a>Statistiken


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|member_count|ganze Zahl|Nein |
|unsubscribe_count|ganze Zahl|Nein |
|cleaned_count|ganze Zahl|Nein |
|member_count_since_send|ganze Zahl|Nein |
|unsubscribe_count_since_send|ganze Zahl|Nein |
|cleaned_count_since_send|ganze Zahl|Nein |
|campaign_count|ganze Zahl|Nein |
|campaign_last_sent|ganze Zahl|Nein |
|merge_field_count|ganze Zahl|Nein |
|avg_sub_rate|Anzahl|Nein |
|avg_unsub_rate|Anzahl|Nein |
|target_sub_rate|Anzahl|Nein |
|open_rate|Anzahl|Nein |
|click_rate|Anzahl|Nein |
|last_sub_date|Zeichenfolge|Nein |
|last_unsub_date|Zeichenfolge|Nein |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Listen|Array|Nein |
|total_items|ganze Zahl|Nein |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Ja |
|merge_fields|nicht definiert|Nein |
|Interessen|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|boolescher Wert|Nein |
|Speicherort|nicht definiert|Nein |
|email_address|Zeichenfolge|Ja |



### <a name="firstandlastname"></a>FirstAndLastName


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|FNAME|Zeichenfolge|Nein |
|LNAME|Zeichenfolge|Nein |



### <a name="location"></a>Speicherort


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Latitude|Anzahl|Nein |
|Länge|Anzahl|Nein |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|email_address|Zeichenfolge|Nein |
|unique_email_id|Zeichenfolge|Nein |
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Nein |
|merge_fields|nicht definiert|Nein |
|Interessen|Zeichenfolge|Nein |
|Statistiken|nicht definiert|Nein |
|ip_signup|Zeichenfolge|Nein |
|timestamp_signup|Zeichenfolge|Nein |
|ip_opt|Zeichenfolge|Nein |
|timestamp_opt|Zeichenfolge|Nein |
|member_rating|ganze Zahl|Nein |
|last_changed|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|boolescher Wert|Nein |
|email_client|Zeichenfolge|Nein |
|Speicherort|nicht definiert|Nein |
|last_note|nicht definiert|Nein |
|list_id|Zeichenfolge|Nein |
|_links|Array|Nein |



### <a name="lastnote"></a>Last_Note


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|note_id|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|created_by|Zeichenfolge|Nein |
|Hinweis|Zeichenfolge|Nein |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Mitglieder|Array|Nein |
|list_id|Zeichenfolge|Nein |
|total_items|ganze Zahl|Nein |



### <a name="object"></a>Objekt






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|email_address|Zeichenfolge|Nein |
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Ja |
|merge_fields|nicht definiert|Nein |
|Interessen|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|boolescher Wert|Nein |
|Speicherort|nicht definiert|Nein |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Mitglieder|Array|Nein |
|list_id|Zeichenfolge|Nein |
|total_items|ganze Zahl|Nein |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|email_address|Zeichenfolge|Ja |
|unique_email_id|Zeichenfolge|Nein |
|email_type|Zeichenfolge|Nein |
|Status|Zeichenfolge|Nein |
|merge_fields|nicht definiert|Ja |
|Interessen|Zeichenfolge|Nein |
|Statistiken|nicht definiert|Nein |
|ip_signup|Zeichenfolge|Nein |
|timestamp_signup|Zeichenfolge|Nein |
|ip_opt|Zeichenfolge|Nein |
|timestamp_opt|Zeichenfolge|Nein |
|member_rating|ganze Zahl|Nein |
|last_changed|Zeichenfolge|Nein |
|Sprache|Zeichenfolge|Nein |
|VIP|boolescher Wert|Nein |
|email_client|Zeichenfolge|Nein |
|Speicherort|nicht definiert|Nein |
|last_note|nicht definiert|Nein |
|list_id|Zeichenfolge|Nein |
|_links|Array|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)

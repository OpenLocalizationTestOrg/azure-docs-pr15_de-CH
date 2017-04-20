<properties
    pageTitle="Verwenden Sie skalieren Aktionen e-Mails und Webhook-Benachrichtigung senden. | Microsoft Azure"
    description="Finden Sie unter Skalieren Aktionen Web-URLs oder e-Mail-Benachrichtigung in Azure Monitor verwenden. "
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Verwenden Sie skalieren Aktionen e-Mails und Webhook in Azure Monitor-Benachrichtigung senden

Dieser Artikel zeigt, wie Trigger eingerichtet, damit bestimmte Web-URLs aufrufen oder e-Mails Autoscale Aktionen in Azure basiert.  

## <a name="webhooks"></a>Webhooks
Webhooks können Sie den Azure-Benachrichtigung an andere Systeme Nachbearbeitung oder benutzerdefinierte Benachrichtigung weiterleiten. Beispielsweise routing Warnung zu Diensten, die eine eingehende Webanfrage senden SMS Protokoll Fehler meldet ein Team chatten oder messaging services verarbeiten kann. Webhook URI muss einen gültigen HTTP oder HTTPS-Endpunkt.

## <a name="email"></a>E-Mail
E-Mail kann eine beliebige gültige e-Mail-Adresse gesendet werden. Und Co-Administratoren des Abonnements, in dem die Regel ausgeführt, werden auch benachrichtigt.


## <a name="cloud-services-and-web-apps"></a>Cloud-Services und webapps
Sie können aus dem Azure-Portal für Cloud-Services und Serverfarmen (Web Apps) anmelden.

- Wählen Sie die **Skalierung von** Metrik.

![Skalieren](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtual Machine Maßstab legt
Für neuere erstellte virtuelle Maschinen mit Ressourcen-Manager (Virtual Machine Scale Sets) können Sie dies konfigurieren REST API, Ressourcenmanager Vorlagen PowerShell und CLI verwenden. Eine Portal-Oberfläche ist noch nicht verfügbar.
Wenn die REST-API oder Ressourcen-Manager-Vorlage verwenden, enthalten Sie Benachrichtigungen Element mit den folgenden Optionen.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Feld                              |Obligatorisch? |Beschreibung|
|---                                |---        |---|
|Vorgang                          |Ja        |Wert muss "Skalieren"|
|sendToSubscriptionAdministrator    |Ja        |Wert muss "true" oder "false" sein.|
|sendToSubscriptionCoAdministrators |Ja        |Wert muss "true" oder "false" sein.|
|customEmails                       |Ja        |[null] oder Zeichenfolgenarray e-Mails möglich|
|webhooks                           |Ja        |Wert kann null oder gültiger Uri sein.|
|serviceUri                         |Ja        |eine gültige Https Uri|
|Eigenschaften                         |Ja        |Wert muss leer {oder Schlüssel-Wert-Paare enthalten|


## <a name="authentication-in-webhooks"></a>Authentifizierung webhooks
Es gibt zwei Arten der Authentifizierung-URI:

1. Token-Basis Authentifizierung, wo Webhook URI mit der token-ID als Abfrageparameter speichern. Beispielsweise https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Standardauthentifizierung, wo Sie einen Benutzernamen und Kennwort verwenden. Zum Beispielhttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Skalierungsgröße Benachrichtigungsschema Webhook Nutzlast
Wenn skalieren-Benachrichtigung generiert wird, ist die folgende Metadaten in der Webhook-Nutzlast enthalten:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Feld  |Obligatorisch?|    Beschreibung|
|---|---|---|
|Status |Ja    |Der Status, der bedeutet, dass eine Aktion Autoscale generiert wurde|
|Vorgang| Ja |Eine Erhöhung der Instanzen werden "Scale Out" und einem Rückgang Instanzen werden "Skalieren"|
|Kontext|   Ja |Kontextmenü Aktion skalieren|
|Zeitstempel| Ja |Zeitstempel skalieren-Aktion ausgelöst wurde|
|ID |Ja|   Ressourcen-Manager-ID der Einstellung skalieren|
|Name   |Ja|   Der Name der Einstellung skalieren|
|Details|   Ja |Erläuterung der Aktion die Skalierungsgröße Dienst und der Anzahl der Instanzen|
|subscriptionId|    Ja |Abonnement-ID der Zielressource skaliert werden|
|resourceGroupName| Ja|    Name der Ressourcengruppe der Zielressource skaliert werden|
|resourceName   |Ja|   Name der Ressource, die skaliert werden|
|resourceType   |Ja|   Die drei unterstützten Werte: "microsoft.classiccompute/domainnames/slots/roles" - Cloud-Dienst Rollen "microsoft.compute/virtualmachinescalesets" - VM Maßstab legt und "Microsoft.Web/serverfarms" - Web App|
|resourceId |Ja|Ressourcen-Manager-ID der Zielressource skaliert werden|
|portalLink |Ja    |Azure Portal Link auf der Seite Zusammenfassung der Zielressource|
|oldCapacity|   Ja |Die aktuelle (alte) Instanzenzahl bei Skalieren einer Aktion|
|newCapacity|   Ja |Die neue Skalierungsgröße Ressource skaliert Instanzenzahl|
|Eigenschaften|    Nein| Optional. Satz von < Schlüssel, Wert > Paare (z. B. Dictionary < String, String >). Das Eigenschaften-Feld ist optional. In einer benutzerdefinierten Benutzeroberfläche oder Logik app-basierte Workflow können Sie eingeben, Schlüssel und Werte mit den Nutzdaten übergeben werden können. Eine alternative Möglichkeit, benutzerdefinierte Eigenschaften wieder an die ausgehende Webhook übergeben wird mit Webhook (als Abfrageparameter) URI|

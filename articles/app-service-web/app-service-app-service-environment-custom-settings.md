<properties
    pageTitle="Standardeinstellungen für App-Dienst"
    description="Benutzerdefinierte Konfigurationen für App-Dienst"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Benutzerdefinierte Konfigurationen für App-Dienst

## <a name="overview"></a>Übersicht ##
Da App Service-Umgebungen für einen einzelnen Debitor isoliert sind, sind bestimmte Konfigurationen App Service-Umgebungen exklusiv zugewiesen werden können. Dieser Artikel beschreibt verschiedene bestimmten Anpassungen, die für App-Dienst verfügbar sind.

Wenn Sie keinen App Service-Umgebung finden Sie unter [Erstellen einer App Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md).

App Service-Umgebung anpassen können mit einem Array in das neue **ClusterSettings** -Attribut gespeichert werden. Dieses Attribut befindet sich im Wörterbuch "Eigenschaften" *HostingEnvironments* Ressourcenmanager Azure Entität.

Die folgende abgekürzte Ressourcenmanager Vorlage Ausschnitt zeigt das **ClusterSettings** -Attribut:


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Das **ClusterSettings** -Attribut kann in einer Vorlage Ressourcenmanager die App Service-Umgebung aktualisieren enthalten.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Verwenden Sie Azure-Ressourcen-Explorer, um eine App Service-Umgebung aktualisieren
Alternativ können Sie die App Service-Umgebung mithilfe von [Azure Resource Explorer](https://resources.azure.com)aktualisieren.  

1. Im Ressourcen-Explorer rufen Sie den Knoten für die App Service-Umgebung (**Abonnements** > **ResourceGroups** > **Provider** > **Micrososft.Web** > **HostingEnvironments**). Klicken Sie auf die spezifischen App Service-Umgebung, die Sie aktualisieren möchten.

2. Klicken Sie im rechten Bereich in der oberen Symbolleiste Interaktive Bearbeitung im Ressourcen-Explorer zu **Lese-/Schreibzugriff** .  

3. Klicken Sie auf Blau **Bearbeiten** zu Ressourcenmanager Vorlage bearbeitet werden.

4. Scrollen Sie nach unten im rechten Bereich. Das **ClusterSettings** -Attribut ist ganz unten in dem eingeben oder aktualisieren den Wert.

5. Eingeben (oder kopieren und Einfügen) Array Konfigurationswerte im **ClusterSettings** -Attribut.  

6. Klicken Sie auf die grüne Schaltfläche **setzen** befindet sich oben rechts auf die Änderung der App Service-Umgebung anwenden.

Aber Einreichen einer Änderung dauert ungefähr 30 Minuten multipliziert mit der Anzahl der front-Ends in der App Service-Umgebung für die Änderung wirksam wird.
Beispielsweise weist eine App Service-Umgebung vier front-Ends, dauert etwa zwei Stunden für die Aktualisierung der Konfiguration abgeschlossen. Beim Rollout der Änderung der Konfiguration können keine anderen skalieren oder Änderung Konfigurationsvorgänge in App Service-Umgebung erfolgen.

## <a name="disable-tls-10"></a>TLS 1.0 deaktivieren ##
Eine wiederkehrende Frage Kunden besonders für Kunden, die mit PCI-Compliance überwacht, wie TLS 1.0 für ihre apps explizit deaktivieren.

TLS 1.0 kann durch den folgenden **ClusterSettings** Eintrag deaktiviert werden:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>TLS Cipher Suite Reihenfolge ändern ##
Eine weitere Frage Kunden ändern sie die Liste der Ziffern ihrer Server ausgehandelt und ist dies kann durch Ändern der **ClusterSettings** wie folgt. Die Liste der verfügbaren Verschlüsselungssammlungen [diesem MSDN-Artikel] abgerufen werden (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Wenn falsche Werte für die Verschlüsselungssammlung, die SChannel nicht verstehen festgelegt sind, TLS-Kommunikation mit dem Server funktioniert möglicherweise nicht. In diesem Fall müssen Sie den Eintrag *FrontEndSSLCipherSuiteOrder* aus **ClusterSettings** und Senden der aktualisierten Ressourcenmanager Vorlage um die Chiffre-Suite Standardeinstellungen wiederherzustellen.  Verwenden Sie diese Funktion mit Vorsicht.

## <a name="get-started"></a>Erste Schritte
Websitevorlage Azure Schnellstart-Ressourcen-Manager enthält eine Vorlage mit der Basis zum [Erstellen einer App Service-Umgebung](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->

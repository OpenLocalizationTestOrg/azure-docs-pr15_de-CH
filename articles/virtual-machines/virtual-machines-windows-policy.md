<properties
    pageTitle="Anwenden von Richtlinien auf Azure Resource Manager virtuelle Computer | Microsoft Azure"
    description="Anwenden eine Richtlinie auf ein Azure Ressourcenmanager virtuellen Windows-Maschine"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Anwenden von Richtlinien auf Azure Resource Manager virtuelle Computer

Mithilfe von Richtlinien kann eine Organisation verschiedene Konventionen und Regeln im gesamten Unternehmen erzwingen. Umsetzung des gewünschten Verhaltens können Risiken und den Erfolg des Unternehmens. In diesem Artikel wird erläutert, Verwendung Azure-Ressourcen-Manager-Richtlinien das gewünschte Verhalten für virtuelle Maschinen der Organisation definieren.

Die Gliederung für die Schritte hierzu ist unten

1. Azure-Ressourcen-Manager-Richtlinie 101
2. Definieren Sie eine Richtlinie für den virtuellen Computer
3. Erstellen Sie die Richtlinie
4. Anwenden der Richtlinie

## <a name="azure-resource-manager-policy-101"></a>Azure-Ressourcen-Manager-Richtlinie 101

Erste Schritte mit Azure-Ressourcen-Manager empfohlen unten lesen und danach mit den Schritten in diesem Artikel. Der Artikel beschreibt grundlegende Definition und Struktur einer Richtlinie wie Richtlinien ausgewertet und verschiedene Beispiele für Policy-Definitionen.

* [Verwenden von Gruppenrichtlinien zu Ressourcen Zugriff](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Definieren Sie eine Richtlinie für den virtuellen Computer

Eine häufige Szenarien für ein Unternehmen möglicherweise ihren Benutzern nur von bestimmten Betriebssystemen erstellen, die für eine LOB-Anwendung getestet wurden. Mit einer Azure-Ressourcen-Manager-Richtlinie kann diese Aufgabe in wenigen Schritten erfolgen. In diesem Beispiel Richtlinie gehen wir nur Windows Server 2012 R2 Datacenter virtuellen Computer erstellt werden können. Policy-Definition aussieht unten

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "MicrosoftWindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "WindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "2012-R2-Datacenter"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Die oben genannten Richtlinie kann problemlos für ein Szenario, in denen möglicherweise soll alle Windows Server Datacenter Bild für die Bereitstellung eines virtuellen Computers mit, geändert die unten ändern

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*Datacenter"
}
```

#### <a name="virtual-machine-property-fields"></a>Virtual Machine Felder

Folgende Tabelle beschreibt die Virtual Machine-Eigenschaften, die als Felder in der Richtliniendefinition. Weitere Bereiche finden Sie im folgenden Artikel:

* [Felder und Quellen](../resource-manager-policy.md#fields-and-sources)


| Feldname     | Beschreibung                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Gibt den Herausgeber des Bildes               |
| imageOffer     | Gibt das Angebot für den Verleger ausgewählte image |
| imageSku       | Gibt die SKU für das ausgewählte Angebot             |
| imageVersion   | Gibt die Bildversion für die gewählte SKU     |

## <a name="create-the-policy"></a>Erstellen Sie die Richtlinie

Eine Richtlinie kann problemlos über die REST API direkt oder die PowerShell-Cmdlets erstellt werden. Erstellen der Richtlinie finden Sie im folgenden Artikel:

* [Erstellen einer Richtlinie](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Anwenden der Richtlinie

Nach Erstellung der Richtlinie müssen Sie auf einen definierten Bereich anwenden. Der Bereich kann ein Abonnement, Ressourcengruppe oder sogar der Ressource. Die Richtlinien finden Sie im folgenden Artikel:

* [Erstellen einer Richtlinie](../resource-manager-policy.md#applying-a-policy)

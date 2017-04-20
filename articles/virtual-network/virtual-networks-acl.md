<properties
   pageTitle="Was ist ein Netzwerk Zugriffssteuerungsliste (ACL)?"
   description="Erfahren Sie mehr über ACLs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Was ist ein Endpunkt Zugriffssteuerungsliste (ACLs)?

Ein Endpunkt Zugriffssteuerungsliste (ACL) ist Ihre Azure-Bereitstellung zur Optimierung der Sicherheit. Eine ACL ermöglicht erlauben oder verweigern Datenverkehr für einen virtuellen Endpunkt. Dieses Paket Filterfunktion stellt eine zusätzliche Sicherheitsebene. Sie können Netzwerk-ACLs für nur Endpunkte angeben. Sie können keine ACL für ein virtuelles Netzwerk oder einem bestimmten Subnetz enthalten in einem virtuellen Netzwerk angeben.

> [AZURE.IMPORTANT] Wird empfohlen, mithilfe der Netzwerk-Sicherheitsgruppen (NSGs) statt ACLs möglichst. Weitere Informationen zu NSGs finden Sie unter [Was ist eine Netzwerk-Sicherheitsgruppe?](virtual-networks-nsg.md).

ACLs können mithilfe von PowerShell oder Verwaltungsportal konfiguriert werden. Konfigurieren eines ACL mithilfe von PowerShell finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](virtual-networks-acl-powershell.md). Konfigurieren eines ACL mit dem Management-Portal finden Sie unter [Festlegen von Endpunkten einer virtuellen Maschine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Netzwerk-Zugriffssteuerungslisten verwenden, können Sie Folgendes:

- Erlauben Sie oder verweigern Sie eingehenden Datenverkehr anhand Remotesubnetz IPv4-Adressbereich mit einem virtuellen Computer input-Endpunkt.

- Schwarze Liste IP-Adressen

- Mehrere Regeln pro virtuellen Endpunkt erstellen

- Geben Sie bis zu 50 ACL Regeln pro virtuellen Endpunkt

- Reihenfolge der Verwendung Regeln darauf den richtigen Satz von Regeln gelten auf einem bestimmten virtuellen Endpunkt (aufsteigend)

- Geben Sie eine Zugriffssteuerungsliste für einen bestimmten Remotesubnetz IPv4-Adresse.

## <a name="how-acls-work"></a>Funktionsweise von ACLs

Eine ACL ist ein Objekt, das eine Liste von Regeln enthält. Beim Erstellen einer ACL und virtuellen Endpunkt wenden erfolgt Paketfilter auf dem Hostknoten VM. Dies bedeutet, dass der Datenverkehr von remote-IP-Adressen Hostknoten ACL Zuordnungsregeln statt auf Ihrem virtuellen Computer gefiltert werden. Dadurch wird verhindert, dass die VM Ausgaben wertvollen CPU-Zyklen für Paketfilter.

Erstellung eine virtuellen Maschine wird eine Standard-ACL zu jeglichen eingehenden Datenverkehr zu blockieren. Allerdings wird ein Endpunkt für (Port 3389) dann standardmäßig ACL modifiziert erstellt, um alle eingehenden Datenverkehr für diesen Endpunkt zuzulassen. Eingehender Datenverkehr von remote-Subnetz darf diesen Endpunkt und keine Firewall bereitstellen muss. Alle anderen Ports sind für eingehenden Datenverkehr blockiert, sofern Endpunkte für diese Ports erstellt werden. Standardmäßig ist ausgehender Datenverkehr zugelassen.

**Standard-ACL Beispieltabelle**

| **Regel Nr.** | **Remote-Subnetz** | **Endpunkt** | **Zulassen/Verweigern** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Zulassen      |

## <a name="permit-and-deny"></a>Zulassen und verweigern

Sie können gewähren bzw. Verweigern des Netzwerkverkehrs für einen virtuellen Computer input Endpunkt Regeln, die angeben, "zulassen" oder "Verweigern". Es ist wichtig zu beachten, dass standardmäßig beim Erstellen ein Endpunkt der gesamte Datenverkehr an den Endpunkt zugelassen ist. Aus diesem Grund ist es wichtig zu Zulassen/Verweigern Regeln erstellen und in der richtigen Reihenfolge auf Wunsch detaillierte Kontrolle über den Datenverkehr, der zu den virtuellen Endpunkt zu wählen.

Zu berücksichtigende Punkte

1. **Keine ACL-** Standardmäßig beim Erstellen ein Endpunkts ermöglichen wir für den Endpunkt.

1. **Zulassen-** Beim Hinzufügen von mindestens einem "Bereiche zulassen" werden alle Bereiche standardmäßig verweigert werden. Nur Pakete aus zugelassene IP-Bereich können mit dem virtuellen Computer kommunizieren.

1. **Deny-** Beim Hinzufügen von mindestens einem "Bereiche verweigern" erlauben Sie anderen Bereichen Verkehr standardmäßig.

1. **Aus zulassen und verweigern-** Sie können eine Kombination aus "zulassen" und "Verweigern" Wenn festzulegen, einen bestimmten IP-Adressbereich zugelassen oder verweigert werden soll.

## <a name="rules-and-rule-precedence"></a>Regel Vorrang und Regeln

Netzwerk-ACLs können auf bestimmten virtuellen Endpunkte eingerichtet. Beispielsweise können Sie ein Netzwerk ACL für eine RDP-Endpunkt erstellt auf einem virtuellen Computer die sperrt Access für bestimmte IP-Adressen angeben. Die Tabelle zeigt öffentliche virtuelle IPs (VIPs) eines bestimmten Bereichs RDP gestatten Zugriff gewähren können. Alle anderen remote IP-Adressen wird verweigert. Wir verfolgen einen Regelfolge *niedrigsten Vorrang* .

### <a name="multiple-rules"></a>Mehrere Regeln

Im folgenden Beispiel möchten Sie Zugriff auf den RDP-Endpunkt aus zwei öffentliche IPv4-Adressbereiche (65.0.0.0/8 und 159.0.0.0/8) können Sie dazu zwei *Zulassen* Regeln angeben. Da RDP standardmäßig für einen virtuellen Computer erstellt wird, sollten Sie in diesem Fall auf den RDP-Port basierend auf einem Remotesubnetz sperren. Das folgende Beispiel zeigt eine Möglichkeit, öffentliche virtuelle IPs (VIPs) eines bestimmten Bereichs RDP gestatten Zugriff gewähren. Alle anderen remote IP-Adressen wird verweigert. Dies funktioniert, da Netzwerk ACLs für einen bestimmten virtuellen Endpunkt eingerichtet kann und Zugriff standardmäßig verweigert.

**Beispiel – mehrere Regeln**

| **Regel Nr.** | **Remote-Subnetz** | **Endpunkt** | **Zulassen/Verweigern** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Zulassen      |
| 200    | 159.0.0.0/8   | 3389     | Zulassen      |

### <a name="rule-order"></a>Reihenfolge der Regeln

Da mehrere Regeln für einen Endpunkt angegeben werden können, muss eine Möglichkeit zu Regeln, um zu bestimmen, welche Regel Vorrang hat sein. Die Reihenfolge der Regeln gibt Vorrang. Netzwerk-Zugriffssteuerungslisten Reihenfolge ein *niedrigsten Vorrang* Regel. Im folgenden Beispiel erhält der Endpunkt auf Port 80 selektiv auf bestimmte IP-Adressbereiche. Hierfür haben wir eine Deny-Regel (Regel \# 100) für Adressen im Bereich 175.1.0.1/24. Dann wird eine zweite Regel mit 200 festgelegt, die Zugriff auf alle anderen Adressen unter 175.0.0.0/8 ermöglicht.

**Beispiel-Regel Vorrang**

| **Regel Nr.** | **Remote-Subnetz** | **Endpunkt** | **Zulassen/Verweigern** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Verweigern        |
| 200    | 175.0.0.0/8   | 80       | Zulassen      |

## <a name="network-acls-and-load-balanced-sets"></a>Netzwerk-ACLs und ausgewogene laden

Netzwerk-ACLs können für einen Lastenausgleich festlegen (LB festgelegt) Endpunkt angegeben werden. Eine ACL für ein Pfund angegeben ist, gilt der Netzwerk-ACL für alle virtuellen Computer in dieser LB. Beispielsweise erstellt Pfd festgelegt mit "Port 80" erstellt und die LB enthält 3 VMs, Netzwerk-ACL Endpunkt "Port 80" einer VM auf die VMs automatisch angewendet wird.

![Netzwerk-ACLs und ausgewogene laden](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Nächste Schritte

[Zugriffssteuerungslisten (ACLs) für Endpunkte verwalten, mithilfe von PowerShell](virtual-networks-acl-powershell.md)

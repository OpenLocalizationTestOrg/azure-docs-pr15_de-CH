<properties
    pageTitle="Bereitstellen eine Internetschnittstelle Lastenausgleich Lösung mit IPv6 mithilfe einer Vorlage | Microsoft Azure"
    description="Wie IPv6-Unterstützung für Lastenausgleich Azure und VMs mit Lastenausgleich bereitgestellt."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6 Azure Lastenausgleich dual-Stack, öffentliche IP-Adresse, nativen IPv6-, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Bereitstellen einer Internetschnittstelle Lastenausgleich-Projektmappe mit IPv6 mithilfe einer Vorlage

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Vorlage](./load-balancer-ipv6-internet-template.md)

Ein Azure Lastverteiler ist ein Layer-4 (TCP, UDP)-Lastenausgleich. Lastenausgleich bietet hohe Verfügbarkeit durch verteilen eingehenden Datenverkehr fehlerfrei Dienstinstanzen mit Cloud-Services oder virtuelle Computer in einer Load Balancer. Azure Lastenausgleich können auch diese Dienste auf mehrere Ports oder mehrere IP-Adressen darstellen.

## <a name="example-deployment-scenario"></a>Beispielszenario für die Bereitstellung

Das folgende Diagramm veranschaulicht den Lastenausgleich Lösung mithilfe der in diesem Artikel beschriebene Beispielvorlage bereitgestellt wird.

![Load Balancer Szenario](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

In diesem Szenario erstellen Sie Azure Folgendes:

- eine virtuelle Netzwerkschnittstelle für jede VM mit IPv4- und IPv6-Adressen
- ein Internetzugriff zum Lastenausgleich mit einer IPv4- und IPv6-öffentliche Adresse
- zwei laden Netzwerklastenausgleich Regeln für private Endpunkte öffentliche VIPs zuordnen
- eine Verfügbarkeit festlegen, zwei VMs enthält
- zwei virtuelle Maschinen (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>Bereitstellen der Vorlage mithilfe des Azure-Portals

Dieser Artikel verweist auf eine Vorlage, die in [Azure Schnellstart](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) Vorlagensammlung veröffentlicht. Sie downloaden die Vorlage aus der Galerie oder starten die Bereitstellung in Azure direkt aus der Galerie. Es wird vorausgesetzt, dass Sie die Vorlage auf den lokalen Computer heruntergeladen haben.

1. Öffnen der Azure-Portal und melden Sie sich mit einem Konto mit Berechtigungen zum Erstellen von virtuellen Computern und Netzwerkressourcen in Azure-Abonnement. Auch wenn Sie vorhandene Ressourcen verwenden, berücksichtigen die Berechtigung zum Erstellen einer Ressourcengruppe und ein Speicherkonto.

2. Klicken Sie auf "+ Neu" im Menü dann Typ "Vorlage" in das Suchfeld. Wählen Sie "vorlagenbereitstellung" aus den Suchergebnissen aus.

    ![Pfd-ipv6-Portal-Schritt 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. In das Blatt klicken Sie auf "vorlagenbereitstellung."

    ![Pfd-ipv6-Portal-Schritt 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klicken Sie auf "Erstellen".

    ![Pfd-ipv6-Portal-Schritt 4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klicken Sie auf "Vorlage bearbeiten". Löschen Sie den vorhandenen Inhalt kopieren in den gesamten Inhalt der Vorlagendatei (enthalten Beginn und Ende {}), und klicken Sie auf "Speichern".

    > [AZURE.NOTE] Wenn Sie Microsoft Internet Explorer verwenden, wenn Sie fügen ein Dialogfeld werden Sie aufgefordert, den Zugriff auf die Windows-Zwischenablage. Klicken Sie auf "Zugriff gewähren".

    ![lb ipv6 Portal Schritt 5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klicken Sie auf "Parameter bearbeiten". Blatt Parameter geben Sie Werte pro der Anleitung im Abschnitt Parameter Vorlage an und dann auf "Speichern" Blade Parameter schließen. Blatt benutzerdefinierte Bereitstellung Ihres Abonnements eine vorhandene Ressourcengruppe auswählen oder erstellen. Wenn Sie eine Ressourcengruppe erstellen, wählen Sie einen Speicherort für die Ressourcengruppe. Als Nächstes **rechtlich**klicken **Einkauf** für die Vertragsbedingungen. Azure beginnt die Ressourcen bereitstellen. Es dauert einige Minuten alle Ressourcen bereitstellen.

    ![Pfd-ipv6-Portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Weitere Informationen zu diesen Parametern finden Sie im Abschnitt [Vorlagenparameter und Variablen](#template-parameters-and-variables) in diesem Artikel.

7. Um von der Vorlage erstellten Ressourcen anzuzeigen, klicken Sie auf Durchsuchen, blättern Sie in der Liste "Ressourcengruppen" angezeigt wird, und klicken sie.

    ![Pfd-ipv6-Portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Klicken Sie auf Blade Gruppen Ressourcen der Ressourcengruppe, die Sie in Schritt 6 angegeben. Finden Sie eine Liste aller Ressourcen, die bereitgestellt wurden. Wenn alles gut geht, sollte "Erfolgreich" unter "Letzte Bereitstellung" angezeigt Nicht sicher, dass das Konto, das Sie verwenden Berechtigungen zum Erstellen der erforderlichen Ressourcen verfügt.

    ![lb ipv6 Portal Schritt 8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Wenn Sie Ressourcengruppen durchsuchen nach Schritt 6 zeigt "Letzte Bereitstellung" den Status von "Bereitstellen" Ressourcen bereitgestellt werden.

9. Klicken Sie auf "myIPv6PublicIP" in der Liste der Ressourcen. Hat eine IPv6-Adresse unter IP-Adresse und DNS-Namen der für den Parameter dnsNameforIPv6LbIP in Schritt 6 angegebenen Wert angezeigt. Diese Ressource ist der öffentlichen IPv6-Adresse und Name für Internet-Clients.

    ![Pfd-ipv6-Portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Wenn die Vorlage erfolgreich bereitgestellt wurde, können Sie Konnektivität überprüfen, die folgenden Aufgaben:

1. Azure-Portal anmelden und Verbinden mit einzelnen VMs vorlagenbereitstellung erstellt. Wenn Sie einen Windows Server-VM führen Sie Ipconfig bereitgestellt/von einer Befehlszeile aus. Sie sehen, dass die VMs IPv4- und IPv6-Adressen. Wenn Sie Linux VMs bereitgestellt, müssen Sie das Linux-Betriebssystem um dynamische IPv6-Adressen der Anleitung für Ihre Linux-Distribution.
2. Ein Client IPv6-Internet-Verbindung initiieren Sie eine Verbindung mit öffentlichen IPv6-Adresse des Lastenausgleich. Bestätigen der Lastenausgleich zwischen zwei VMs Netzwerklastenausgleich, konnte Sie Webserver wie Microsoft-Internetinformationsdienste (IIS) auf jedem virtuellen Computer installieren. Die Standardwebseite auf jedem Server kann den Text "Server0" oder "Server1" eindeutig identifizieren enthalten. Dann öffnen Sie einen Internet-Browser auf einem Client IPv6-Internet verbunden und suchen auf der Hostname angegeben dnsNameforIPv6LbIP als Parameter für den Lastenausgleich, End-to-End-IPv6-Konnektivität für jeden VM zu bestätigen. Wird nur die Webseite mit nur einem Server, müssen Sie den Browsercache löschen. Mehrere private Browsersitzungen zu öffnen. Eine Antwort von jedem Server sollte angezeigt werden.
3. Ein Client IPv4-Internet-Verbindung initiieren Sie eine Verbindung mit der öffentlichen IPv4-Adresse des Lastenausgleich. Um sicherzustellen, dass das System zum Lastenausgleich Lastenausgleich zwei VMs können Sie mit IIS, wie in Schritt2 testen.
4. Jede VM initiieren Sie eine ausgehende Verbindung zu einem Gerät IPv6- oder IPv4-Verbindung Internet. In beiden Fällen ist das Zielgerät gesehen Quell-IP der öffentlichen IPv4- oder IPv6-Adresse Lastenausgleich.

>[AZURE.NOTE]
ICMP für IPv4 und IPv6 ist in Azure Netzwerk blockiert. ICMP-Tools wie ping immer daher fehl. Verwenden Sie eine Alternative TCP-TCPing oder Test-NetConnection PowerShell-Cmdlet zum Testen der Konnektivität. Beachten Sie, dass die IP-Adressen in der Abbildung dargestellten Beispiele für Werte, die angezeigt werden. Da IPv6-Adressen dynamisch zugewiesen werden, die Adressen erhalten Sie variiert und können je nach Region variieren. Es ist auch für die öffentlichen IPv6-Adresse auf dem Lastenausgleich durch ein anderes Präfix als privaten IPv6-Adressen im Back-End-Pool zu.

## <a name="template-parameters-and-variables"></a>Vorlagenparameter und Variablen

Eine Azure-Ressourcen-Manager-Vorlage enthält mehrere Variablen und Parameter, die an Ihre Bedürfnisse anpassen können. Variablen werden feste Werte, die Sie nicht ändern. Parameter dienen Werte, dass Benutzer bereitstellen, wenn die Vorlage bereitstellen möchten. Die Beispielvorlage ist in diesem Artikel beschriebenen Szenario konfiguriert. Sie können dies zu Ihrer Umgebung anpassen.

In diesem Artikel verwendeten Beispielvorlage enthält die folgenden Variablen und Parameter:

| Parameter / Variable | Notizen |
|-----------|-------|
| adminUsername | Geben Sie den Namen des Administratorkontos mit den virtuellen Computern anmelden. |
| adminPassword | Geben Sie das Kennwort für das Administratorkonto für die virtuellen Maschinen mit angemeldet. |
| dnsNameforIPv4LbIP | Geben Sie den DNS-Hostnamen als den öffentlichen Namen der Lastenausgleich zuweisen möchten. Dieser Name wird in der Load-Balancer öffentlichen IPv4-Adresse aufgelöst. Der Name muss Kleinbuchstaben und dem regulären Ausdruck überein: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Geben Sie den DNS-Hostnamen als den öffentlichen Namen der Lastenausgleich zuweisen möchten. Dieser Name in der Load-Balancer öffentlichen IPv6-Adresse aufgelöst. Der Name muss Kleinbuchstaben und dem regulären Ausdruck überein: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Dies kann den gleichen Namen wie die IPv4-Adresse. Sendet ein Client eine DNS-Abfrage für diesen Namen Azure zurück Datensätze A- und AAAA bei der Name freigegeben ist. |
| vmNamePrefix | Geben Sie das Namenspräfix VM. Die Vorlage fügt eine Zahl (0, 1, usw.) den Namen beim Erstellen von VMs. |
| nicNamePrefix | Geben Sie das Netzwerk-Schnittstelle Präfix. Die Vorlage fügt eine Zahl (0, 1, usw.) den Namen beim Erstellen von Netzwerkschnittstellen. |
| storageAccountName | Geben Sie den Namen eines bestehenden Speicherkontos, oder geben Sie den Namen eines neuen der Vorlage erstellt werden. |
| availabilitySetName | Geben Sie dann die Verfügbarkeit für VMs einrichten |
| addressPrefix | Das Adresspräfix definiert den Adressbereich des virtuellen Netzwerks |
| subnetName | Der Name der im Subnetz erstellt für das VNet |
| subnetPrefix | Das Adresspräfix definiert den Adressbereich des Subnetzes |
| vnetName | Geben Sie den Namen für das VNet von VMs verwendet. |
| ipv4PrivateIPAddressType | Zuordnungsmethode für die private IP-Adresse (statisch oder dynamisch) |
| ipv6PrivateIPAddressType | Zuordnungsmethode für private IP-Adresse (dynamisch). IPv6 unterstützt nur dynamische Zuweisung. |
| numberOfInstances | Die Anzahl der Lastenausgleich Instanzen der Vorlage bereitgestellt |
| ipv4PublicIPAddressName | Geben Sie den DNS-Namen mit der öffentlichen IPv4-Adresse Lastenausgleich kommunizieren möchten. |
| ipv4PublicIPAddressType | Zuordnungsmethode für die öffentliche IP-Adresse (statisch oder dynamisch) |
| Ipv6PublicIPAddressName | Geben Sie den DNS-Namen mit Lastenausgleich die öffentlichen IPv6-Adresse kommunizieren möchten. |
| ipv6PublicIPAddressType | Zuordnungsmethode für die öffentliche IP-Adresse (dynamisch). IPv6 unterstützt nur dynamische Zuweisung. |
| lbName | Geben Sie den Namen des zum Lastenausgleich. Dieser Name wird im Portal oder verwendet es mit einem CLI oder PowerShell-Befehl. |

Die restlichen Variablen in der Vorlage enthalten abgeleiteten Werte zugewiesen werden, wenn Azure Ressourcen erstellt. Ändern Sie die Variablen.

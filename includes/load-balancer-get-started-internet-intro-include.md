Ein Azure Lastverteiler ist ein Layer-4 (TCP, UDP)-Lastenausgleich. Lastenausgleich bietet hohe Verfügbarkeit durch verteilen eingehenden Datenverkehr fehlerfrei Dienstinstanzen mit Cloud-Services oder virtuelle Computer in einer Load Balancer. Azure Lastenausgleich können auch diese Dienste auf mehrere Ports oder mehrere IP-Adressen darstellen.

Sie können einen Lastenausgleich zu konfigurieren:

* Saldo Internetverkehr virtuellen Maschinen (VMs) zu laden. Wir bezeichnen ein Lastenausgleich in diesem Szenario als ein [Lastenausgleich Internetzugriff](../articles/load-balancer/load-balancer-internet-overview.md).
* Load Balance Datenverkehr zwischen VMs in einem virtuellen Netzwerk (VNet), zwischen VMs in der Cloud-Services oder zwischen auf lokalen Computern und VMs in einem standortübergreifenden virtuellen Netzwerk Wir bezeichnen ein Lastenausgleich in diesem Szenario als eine [interne Lastenausgleich (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Externe Verkehr an eine bestimmte VM-Instanz.

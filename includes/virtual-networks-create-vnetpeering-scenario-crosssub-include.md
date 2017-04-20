## <a name="peering-across-subscriptions"></a>Über Abonnements Peering

In diesem Szenario erstellen Sie ein peering zwischen zwei verschiedenen Subskriptionen für VNets.

![Cross Subszenario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

VNet peering verwendet rollenbasierte Zugriffskontrolle (RBAC) für die Autorisierung. Cross-Abonnements Szenario müssen Sie zunächst die Peeringverbindung erstellen Benutzer ausreichende Berechtigungen gewähren:

> [AZURE.NOTE] Wenn derselbe Benutzer beide Daueraufträge die Berechtigung hat, können Sie Schritt 1 bis 4 unten überspringen.

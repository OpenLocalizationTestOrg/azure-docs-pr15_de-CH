<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>FAQ - Reverse DNS für Ihre Azure zugewiesene IP-Adresse

### <a name="how-much-do-reverse-dns-records-cost"></a>Wie viel DNS-Datensätze Kosten storniert?
Sie sind kostenlos!  Es ist ohne zusätzliche Kosten für reverse-DNS-Einträge oder Abfragen.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Lösen reverse DNS-Datensätze aus dem Internet?
Ja. Nach die reverse DNS-Eigenschaft für den Cloud-Dienst festlegen verwaltet Azure die DNS-Delegierung und stellt sicher, dass für alle Internetbenutzer reverse DNS-Eintrag löst DNS-Zonen.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Werden standardmäßige reverse DNS-Datensatz für Cloud-Dienste erstellt?
Nein. Reverse DNS ist anmelden. Keine standardmäßige reverse DNS-Datensätze werden erstellt, wenn Sie nicht konfigurieren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Was ist das Format für den vollqualifizierten Domänennamen (FQDN)?
FQDNs werden in der Reihenfolge angegeben und müssen durch einen Punkt (z. B. "app1.contoso.com.") beendet werden.

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Was passiert bei der Überprüfung wird der reverse DNS angegebenen fehlschlagen?
Reverse DNS, überprüft nicht, der Service Management-Vorgang fehl. Reverse DNS-Wert entsprechend zu korrigieren, und wiederholen.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Kann ich für Meine Website Azure reverse-DNS verwalten?
Reverse-DNS-wird für Azure Websites nicht unterstützt. Reverse-DNS-wird für Azure PaaS-Rollen und IaaS virtuelle Maschinen unterstützt.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Kann ich mehrere reverse DNS-Datensätze für Cloud-Dienst konfigurieren?
Nein. Azure unterstützt einen umgekehrten DNS-Datensatz für jede Azure-Cloud-Dienst. Jede Azure-Cloud-Dienst kann jedoch eigene reverse DNS-Datensatz verfügen.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kann ich von meinem Azure Compute-Services e-Mails an externe Domänen senden?
Nein. [Azure Compute-Dienste unterstützen keine e-Mails an externe Domänen senden](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).

Eine DNS-Zone dient die DNS-Datensätze für eine bestimmte Domäne hosten. Um Hosten Ihrer Domäne zu starten, müssen Sie eine DNS-Zone erstellen. Alle DNS-Einträge für eine bestimmte Domäne erstellt werden in einer DNS-Zone für die Domäne. 

Beispielsweise enthalten die Domäne "contoso.com" eine Anzahl von DNS-Datensätzen wie "mail.contoso.com" (für einen e-Mail-Server) und "www.contoso.com" (für eine Website). 


## <a name="names"></a>Über DNS-Namen
 
- Der Name der Zone muss innerhalb der Ressourcengruppe und die Zone muss nicht bereits vorhanden. Andernfalls schlägt der Vorgang fehl.

- Gleichnamige Zone kann in einer anderen Ressourcengruppe oder ein anderes Azure Abonnement erneut verwendet werden. 

- Wenn mehrere Zonen denselben Namen haben, jeweils verschiedene Namensserveradressen zugewiesen und kann nur eine Instanz von der übergeordneten Domäne delegiert. Weitere Informationen finden Sie unter [Delegaten Azure DNS Domäne](../articles/dns/dns-domain-delegation.md).
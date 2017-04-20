| Ressource                                    | Grenzwert                                  |
|---------------------------------------------|----------------------------------------|
| Cache-Größe                                  | 530 GB ([Kontakt](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase) Weitere)                                  |
| Datenbanken                                   | 64                                     |
| Max. verbundene clients                       | 40.000                                 |
| Redis Cache Replikate (hohe Verfügbarkeit) | 1 |
| Splitter in einem Premium-Cache mit clustering    | 10 |

Azure Redis Cache beschränkt und Größen für jeden Tarif. Die Tarifen sowie deren zugeordnete Größe finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/pricing/details/cache/).

Weitere Informationen zu Azure Redis Konfiguration Cachelimits finden Sie unter [Serverkonfiguration Standard Redis](redis-cache/cache-configure.md#default-redis-server-configuration).

Da die Konfiguration und Verwaltung von Azure Redis Cacheinstanzen erfolgt durch Microsoft, werden nicht alle Redis Befehle in Azure Redis Cache unterstützt. Weitere Informationen finden Sie unter [Redis Befehle nicht unterstützt in Azure Redis Cache]((redis-cache/cache-configure.md#redis-commands-not-supported-in-azure-redis-cache).
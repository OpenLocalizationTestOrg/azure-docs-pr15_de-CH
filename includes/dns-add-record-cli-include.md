#### <a name="create-an-a-record-set-with-single-record"></a>Erstellen Sie einen A-Datensatz Datensatz festgelegt

Verwenden Sie zum Erstellen einer Datensatzgruppe `azure network dns record-set create`. Geben Sie die Ressourcengruppe Zonenname Rekord relative Namen Datensatztyp und Zeit Live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Nach dem Erstellen der ein Datensatz festlegen, den Eintrag mit die IPv4-Adresse hinzufügen `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Erstellen Sie einen AAAA-Datensatz mit einem einzelnen Datensatz festlegen

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Erstellen Sie einen CNAME-Eintrag mit einem einzelnen Datensatz festlegen

CNAME-Einträge können nur einen einzelnen Zeichenfolgenwert.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Erstellen eines MX-Eintrags mit einem einzelnen Datensatz festlegen

In diesem Beispiel verwenden wir den Namen des Datensatzes "@" zu den MX-Eintrag der Zone Spitze (in diesem Fall "contoso.com"). Dies ist für MX-Einträge.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Erstellen Sie einen NS-Datensatz mit einem einzelnen Datensatz festlegen

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Erstellen Sie einen PTR-Datensatz mit einem einzelnen Datensatz festlegen  
In diesem Fall "Meine-Arpa-Zone.com" ARPA Zeitzone darstellt IP-Bereich darstellt.  Jede PTR-Eintrag in dieser Zone festgelegt entspricht einer IP-Adresse innerhalb dieses IP-Bereichs.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Erstellen Sie einen SRV-Eintrag mit einem einzelnen Datensatz

Wenn Sie einen SRV-Eintrag im Stamm der Zone erstellen, können Sie "Service" und "_protocol" in dessen Namen angeben. Besteht keine Notwendigkeit, "@" in den Namen des Datensatzes.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Erstellen eines TXT-Datensatzes Datensatz festgelegt

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"

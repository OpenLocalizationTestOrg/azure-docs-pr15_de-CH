<properties
    pageTitle="Rechenintensive Java-Anwendung auf einer VM | Microsoft Azure"
    description="Informationen Sie zum Azure virtuellen Computer erstellen, der eine rechenintensive Java-Anwendung ausgeführt wird, die von einer anderen Anwendung Java überwacht werden können."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Einen rechenintensiver Vorgang in Java auf einem virtuellen Computer ausführen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Einen virtuellen Computer können Sie mit Azure rechenintensive Aufgaben. Ein virtueller Computer kann z. B. Aufgaben und Ergebnisse auf Client-Computern oder mobilen Anwendung. Diese Artikel müssen Sie wie einen virtuellen Computer erstellen, der eine rechenintensive Java-Anwendung ausgeführt wird, die von einer anderen Anwendung Java überwacht werden können.

Diese praktische Einführung geht wissen, wie Java-Konsole erstellt, können Bibliotheken für die Java-Anwendung und generieren einen Java-Archiv (JAR). Keine Kenntnis von Microsoft Azure wird angenommen.

Lernen Sie Folgendes:

* Zum Erstellen eines virtuellen Computers mit einem Java Development Kit (JDK) installiert werden.
* Wie Sie Remote mit dem virtuellen Computer anmelden.
* Erstellen Sie einen Service Bus-namespace
* Eine Java-Anwendung erstellen, die eine rechenintensive Aufgabe ausführt
* Eine Java-Anwendung erstellen, die den Fortschritt der rechenintensiven Aufgabe überwacht
* Die Java-Programme ausführen
* Zum Beenden der Java-Anwendung.

In diesem Lernprogramm verwenden Sie Handlungsreisendenproblems für rechenintensive Aufgabe. Nachfolgend ein Beispiel für die Java Anwendung rechenintensiver Vorgang.

![Traveling Salesman Problem solver][solver_output]

Folgendes ist ein Beispiel der rechenintensiven Überwachungsaufgabe Java-Anwendung.

![Traveling Salesman Problem client][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Zum Erstellen eines virtuellen Computers

1. Melden Sie sich im [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **neu** **berechnen**klicken Sie, klicken Sie auf **virtuelle Computer**und klicken Sie dann auf **Aus Galerie**.
3. Wählen Sie im Dialogfeld **virtueller Computer auswählen** **JDK 7 Windows Server 2012**.
Beachten Sie, dass **JDK 6 Windows Server 2012** bei Legacyanwendungen, die noch nicht in JDK 7 ausgeführt werden.
4. Klicken Sie auf **Weiter**.
4. Im Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Geben Sie einen Namen für den virtuellen Computer.
    2. Geben Sie die Größe des virtuellen Computers.
    3. Geben Sie einen Namen für den Administrator im Feld **Benutzername** ein. Beachten Sie diese Namen und das Kennwort, das Sie anschließend eingeben, Sie verwenden beim Remote auf dem virtuellen Computer anmelden.
    4. Geben Sie ein Kennwort im Feld **Neues Kennwort** und geben in das Feld **bestätigen** . Dies ist das Kennwort des Administratorkontos.
    5. Klicken Sie auf **Weiter**.
5. Die nächsten virtuellen Computer im Dialogfeld **Konfiguration** :
    1. **Cloud-Dienst**verwenden Sie unter **Erstellen einer neuen Cloud-Dienst**.
    2. Der Wert für **Cloud-Dienst DNS-Namen** muss über cloudapp.net eindeutig sein. Bei Bedarf diesen Wert ändern, damit diese Azure zeigt an, dass er eindeutig ist.
    2. Geben Sie einen Bereich, Gruppe oder virtuelles Netzwerk. Geben Sie für dieses Lernprogramm einer Region wie **Westen der USA**.
    2. **Konto**wählen Sie **mit einer automatisch generierten Speicherkonto aus**
    3. **Für **Verfügbarkeit**wählen.**
    4. Klicken Sie auf **Weiter**.
5. Im abschließenden Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Akzeptieren Sie die Standardeinträge Endpunkt.
    2. Klicken Sie auf **abgeschlossen**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Mit dem virtuellen Computer remote anmelden

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **virtuelle Computer**.
3. Klicken Sie auf den Namen des virtuellen Computers, die Sie sich anmelden möchten.
4. Klicken Sie auf **Verbinden**.
5. Antworten Sie auf die Aufforderung nach Bedarf für die Verbindung mit dem virtuellen Computer. Verwenden Sie nach dem Namen und Kennwort gefragt die Werte, die Sie beim Erstellen der virtuellen Computer bereitgestellt.

Beachten Sie, dass Azure Service Bus Funktionalität Baltimore CyberTrust Root Zertifikat als Teil der JRE **Cacerts** Speicher installiert werden muss. Dieses Zertifikat wird automatisch in der Java Runtime Environment (JRE) verwendet, die für dieses Lernprogramm aufgenommen. Wenn Sie dieses Zertifikat im Speicher **Cacerts** JRE nicht verfügen, finden Sie unter [Hinzufügen eines Zertifikats zum Java CA Zertifikatspeicher] [ add_ca_cert] Informationen über sie (sowie Informationen zum Anzeigen der Zertifikate im Speicher Cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Erstellen Sie einen Service Bus-namespace

Zunächst in Azure Service Bus-Warteschlangen verwenden, muss einen Dienstnamespace erstellen. Dienstnamespace bietet eine Bereichseinheit Service Bus Ressourcen in der Anwendung.

So erstellen Sie einen Service-namespace

1.  Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com).
2.  Klicken Sie im unteren linken Navigationsbereich des klassischen Azure-Portal auf **Service Bus, Zugriffskontrolle und Zwischenspeichern**.
3.  Im linken Bereich des klassischen Azure-Portal auf **Service Bus** -Knoten und klicken Sie dann auf die Schaltfläche **neu** .  
    ![Bus-Dienstknoten screenshot][svc_bus_node]
4.  Geben Sie im Dialogfeld **neue Service-Namespace erstellen** einen **Namespace**und um sicherzustellen, dass er eindeutig ist, klicken Sie **Verfügbarkeit prüfen** .  
    ![Erstellen Sie einen neuen Namespace-screenshot][create_namespace]
5.  Wenn Namespacenamen verfügbar ist, wählen Sie Land / Region, in dem der Namespace gehostet werden soll und klicken **Namespace erstellen** .  

    Der Namespace erstellten erscheinen dann in der Azure-Verwaltungsportal und dauert einen Moment aktivieren. Warten Sie, bis der Status **aktiv** ist, bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Stellen Sie die Standard-Management Anmeldeinformationen für den namespace

Zur Durchführung von Verwaltungsoperationen wie Erstellen einer Warteschlange für den neuen Namespace müssen Sie Anmeldeinformationen Management für den Namespace erhalten.

1.  Klicken Sie im linken Navigationsbereich auf **Service Bus** -Knoten, um die Liste der verfügbaren Namespaces anzuzeigen.
    ![Verfügbare Namespaces screenshot][avail_namespaces]
2.  Wählen Sie den Namespace erstellten aus der Liste.
    ![Namespace Liste screenshot][namespace_list]
3.  Rechten Bereich **Eigenschaften** Listet die Eigenschaften für den neuen Namespace.
    ![Eigenschaften im Bereich screenshot][properties_pane]
4.  Der **Standardschlüssel** ist ausgeblendet. Klicken Sie auf **Ansicht** , um die Anmeldeinformationen anzuzeigen.
    ![Standard-Taste screenshot][default_key]
5.  Notieren Sie **Standard Aussteller** und **Standardschlüssel** verwendet diese Informationen unten für Operationen mit dem Namespace.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Eine Java-Anwendung erstellen, die eine rechenintensive Aufgabe ausführt

1. Auf dem Entwicklungscomputer (der keinen virtuellen Computer handeln, die Sie erstellt haben), laden Sie das [Azure SDK für Java](https://azure.microsoft.com/develop/java/).
2. Erstellen Sie eine Java mit den folgenden Code am Ende dieses Abschnitts. In diesem Tutorial verwenden wir **TSPSolver.java** als Java-Dateinamen. Ändern der **der\_Service\_Bus\_Namespace**, **der\_Service\_Bus\_Besitzer**, und **der\_Service\_Bus\_Schlüssel** Platzhalter mit Ihrem Service bus **Namespace**und **Aussteller Standard** **Standard** Schlüsselwerte.
3. Nach der Codierung die Anwendung eine ausführbare Java Archive (JAR) exportieren und Bibliotheken in generierten JAR-Datei packen. In diesem Tutorial verwenden wir erstellten JAR-Namen **TSPSolver.jar** .

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Eine Java-Anwendung erstellen, die den Fortschritt der rechenintensiven Aufgabe überwacht

1. Erstellen Sie auf dem Entwicklungscomputer eine Java mit den folgenden Code am Ende dieses Abschnitts. In diesem Tutorial verwenden wir **TSPClient.java** als Java-Dateinamen. Ändern Sie wie oben beschrieben die **Ihre\_Service\_Bus\_Namespace**, **der\_Service\_Bus\_Besitzer**, und **der\_Service\_Bus\_Schlüssel** Platzhalter mit Ihrem Service bus **Namespace**und **Aussteller Standard** **Standard** Schlüsselwerte.
2. Anwendung ausführbarer JAR exportieren und Bibliotheken in generierten JAR-Datei packen. In diesem Tutorial verwenden wir erstellten JAR-Namen **TSPClient.jar** .

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Die Java-Programme ausführen
Führen Sie die Anwendung rechenintensive zunächst zu der Warteschlange lösen Saleseman Problem Reisen Service Bus-Warteschlange aktuelle beste Route hinzugefügt wird. Während die rechenintensive Anwendung läuft (oder später) führen Sie die Ergebnisse aus der Service Bus-Warteschlange anzeigen.

### <a name="to-run-the-compute-intensive-application"></a>Rechenintensive Anwendung ausführen

1. Melden Sie sich beim virtuellen Computer an.
2. Erstellen Sie einen Ordner, in dem die Anwendung ausgeführt wird. Beispielsweise **c:\TSP**.
3. Kopieren Sie **TSPSolver.jar** in **c:\TSP**,
4. Erstellen Sie eine Datei namens **c:\TSP\cities.txt** mit dem folgenden Inhalt.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Befehlszeile wechseln Sie zu c:\TSP.
6. Sicherstellen Sie, dass die JRE Bin-Ordner in der PATH-Umgebungsvariable.
7. Sie müssen Service Bus-Warteschlange vor der TSP Solver Permutationen erstellen. Führen Sie den folgenden Befehl zum Erstellen der Service Bus-Warteschlange.

        java -jar TSPSolver.jar createqueue

8. Erstellen der Warteschlange können Sie TSP Solver Permutationen ausführen. Beispielsweise führen Sie den folgenden Befehl Solver für 8 Städte ausgeführt.

        java -jar TSPSolver.jar 8

 Wenn Sie eine Zahl angeben, wird für 10 Städten ausgeführt. Solver aktuelle kürzeste Route findet, werden sie der Warteschlange hinzugefügt.

> [AZURE.NOTE]
> Je größer die Zahl, die Sie, desto länger angeben der Gleichungslöser ausführen. Zum Beispiel ausgeführt 14 Städte können einige Minuten dauern und Ausführung für 15 Städten mehrere Stunden dauern. Auf 16 oder mehr Orten führen Tagen Laufzeit (schließlich Wochen, Monate und Jahre). Dies ist aufgrund der schnell die Anzahl der Permutationen Städte mit steigender Anzahl an der Gleichungslöser bewertet.

### <a name="how-to-run-the-monitoring-client-application"></a>Die Überwachung Clientanwendung ausführen
1. Melden Sie sich bei Ihrem Computer, in dem die Clientanwendung ausgeführt wird. Dies muss nicht unbedingt den gleichen Computer mit der Anwendung **TSPSolver** sein.
2. Erstellen Sie einen Ordner, in dem die Anwendung ausgeführt wird. Beispielsweise **c:\TSP**.
3. Kopieren Sie **TSPClient.jar** in **c:\TSP**,
4. Sicherstellen Sie, dass die JRE Bin-Ordner in der PATH-Umgebungsvariable.
5. Befehlszeile wechseln Sie zu c:\TSP.
6. Führen Sie den folgenden Befehl ein.

        java -jar TSPClient.jar

    Legen Sie gegebenenfalls die Anzahl der Minuten zwischen überprüft die Warteschlange übergeben Befehlszeilenargument schlafen. Der Standbymodus Standardzeitraum für die Überprüfung der Warteschlange beträgt 3 Minuten wird kein Befehlszeilenargument an **TSPClient**übergeben. Wenn Sie einen anderen Wert für das Ruheintervall verwenden möchten, z. B. Führen Sie eine Minute den folgenden Befehl.

        java -jar TSPClient.jar 1

    Der Client läuft, bis eine Meldung Warteschlange "Vollständig" sieht. Beachten Sie, dass wenn Sie mehrfach Solver ausführen, ohne den Client, können dem Client mehrere Male vollständig leeren die Warteschlange ausführen. Alternativ können Sie die Warteschlange löschen und erneut erstellen. Führen Sie den folgenden **TSPSolver** (nicht **TSPClient**) Befehl, um die Warteschlange zu löschen.

        java -jar TSPSolver.jar deletequeue

    Solver wird ausgeführt, bis es beendet alle Routen untersuchen.

## <a name="how-to-stop-the-java-applications"></a>Die Java-Anwendung beenden
Für die Solver und Clientanwendungen drücken Sie **STRG + C** zum beenden möchten Sie vor dem normalen Abschluss beendet.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md

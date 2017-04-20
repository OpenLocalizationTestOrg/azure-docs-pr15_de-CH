<properties 
    pageTitle="Cluster-Modell | Microsoft Azure" 
    description="Cluster-Modell" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Cluster-Modell    

Wie können wir Gruppen haben Karteninhaber Verhalten, um die Belastung Kreditkartenunternehmen verringern? Wie können wir Gruppen von Eigenschaften der Mitarbeiter definieren, um die Leistungsfähigkeit zu verbessern? Wie können Ärzte Patienten in Gruppen anhand der Merkmale ihrer Krankheiten einordnen? Grundsätzlich können alle diese Fragen durch clusteranalyse beantwortet werden.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Cluster-Analyse klassifiziert eine Gruppe von Messwerte in mindestens zwei sich gegenseitig ausschließende unbekannte Gruppen basierend auf Kombinationen von Variablen. Cluster-Analyse ist zu einem System Messwerte normalerweise Personen und deren Eigenschaften in Gruppen organisieren, haben Mitglieder der Gruppen Eigenschaften gemeinsam. Dieser [Dienst](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) verwendet die K-Means-Methode einen häufig verwendeten clustering-Technik, Cluster beliebiger Daten in Gruppen. Dieser Webdienst die Daten und die Anzahl k Cluster als Eingabe und erzeugt Vorhersagen der k Gruppen jeder Messwerte gehört. 

>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer verwendet werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes   
Dieser Webdienst werden Daten in Gruppen k und gibt die Zuordnung für jede Zeile. Der Webdienst erwartet die Endbenutzer Daten als Zeichenfolge eingeben, wobei Zeilen werden durch Kommas (,) getrennt und Spalten werden durch ein Semikolon (;) getrennt. Der Webdienst erwartet 1 Zeile gleichzeitig. Ein Beispieldataset könnte wie folgt aussehen:

![Beispieldaten][1]

Nehmen wir an, dass der Benutzer diese Daten in 3 sich gegenseitig ausschließende Gruppen aufteilen. Die Daten für das obige Dataset folgende wäre: Wert = "10 5 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Die Ausgabe ist die vorhergesagte Gruppenmitgliedschaft für jede Zeile.

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




##<a name="creation-of-web-service"></a>Erstellen des Webdiensts  
>Dieser Webdienst wurde mit Azure Machine Learning erstellt. Eine kostenlose Testversion sowie einführende Videos Experimente und [publishing-Webdienste](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Unten ist ein Screenshot des Versuchs, die Web Service und Beispielcode für jedes der Module innerhalb des Experiments erstellt.

Aus in Azure Machine Learning ein neues leeres Experiment erstellt wurde und zwei [R-Skript ausführen] [ execute-r-script] Module auf den Arbeitsbereich gezogen. Das Schema wurde mit einem einfachen [R-Skript ausführen][execute-r-script]. Dann Abschnitt Modell Cluster erneut mit einem [Execute R Script]erstellt das Schema verknüpft war[execute-r-script]. [R-Skript ausführen] [ execute-r-script] für Cluster-Modell verwendet, nutzt der Webdienst dann "k-Means"-Funktion, die vordefinierte in [Execute R Script] ist[ execute-r-script] von Azure maschinelles lernen.    
   

     
![Experiment Fluss][3]

####<a name="module-1"></a>Modul 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Modul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Grenzen
Dies ist ein sehr einfaches Beispiel eines Webdiensts clustering. Wie aus den oben stehenden Beispielcode ersichtlich, wird keine Fehler abgefangen und der Dienst geht alles eine kontinuierliche Variable (keine kategorischen Funktionen zulässig), als der Dienst nur Eingaben numerische Werte zum Zeitpunkt der Erstellung dieser Webdienst. Außerdem behandelt der Dienst derzeit beschränkt Datengröße fällig Anforderung/Antwort Art des Web Service-Aufruf und die Tatsache, dass das Modell immer entspricht wird der Webdienst aufgerufen wird. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

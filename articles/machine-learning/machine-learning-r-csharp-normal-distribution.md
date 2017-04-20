<properties 
    pageTitle="Normale Verteilung Web Service Suite | Microsoft Azure" 
    description="Normale Verteilung Web Service Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 

#<a name="normal-distribution-suite"></a>Normale Verteilung Suite



Normalverteilung Suite ist eine Gruppe erstellen und behandeln normale Verteilung bei Beispiel Web Services ([Generator]( https://datamarket.azure.com/dataset/aml_labs/ndg7) [Quantil Rechner]( https://datamarket.azure.com/dataset/aml_labs/ndq5) [Wahrscheinlichkeit Rechner]( https://datamarket.azure.com/dataset/aml_labs/ndp5)). Den Diensten können nacheinander normale Verteilung beliebiger Länge Quantile einer bestimmten Wahrscheinlichkeit berechnen und Berechnung der Wahrscheinlichkeit von bestimmten Quantil generieren. Die Dienste gibt verschiedene Ergebnisse basierend auf den ausgewählten Dienst aus (siehe unten). Normalverteilung Suite basiert auf der R Funktionen Qnorm, Rnorm und Pnorm, R Statistik-Paket enthalten sind.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer verwendet werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.  
 

##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes
Normalverteilung Suite beinhaltet 3.

###<a name="normal-distribution-quantile-calculator"></a>Normale Verteilung Quantil Rechner
Dieser Dienst 4 Argumente einer normalen Verteilung und zugeordnete Quantil berechnet.

Die Eingabeargumente sind:

* p - einzelne Wahrscheinlichkeit eines Ereignisses mit normalen Verteilung. 
* Mittel - Mittel normale Verteilung.
* SD - normale Verteilung der Standardabweichung. 
* Seite - L für den unteren Rand der Verteilung und U für den oberen Rand der Verteilung.

Die Ausgabe des Diensts ist das berechnete Quantil angegebenen Wahrscheinlichkeit zugeordnet ist.

###<a name="normal-distribution-probability-calculator"></a>Normale Verteilung Wahrscheinlichkeit Rechner
Dieser Dienst 4 Argumente einer normalen Verteilung und zugeordnete Wahrscheinlichkeit berechnet.

Die Eingabeargumente sind:

* Q - ein einzelnes Quantil eines Ereignisses mit normalen Verteilung. 
* Mittel - Mittel normale Verteilung.
* SD - normale Verteilung der Standardabweichung. 
* Seite - L für den unteren Rand der Verteilung und U für den oberen Rand der Verteilung.

Die Ausgabe des Dienstes ist die berechnete Wahrscheinlichkeit angegebenen Quantil zugeordnet ist.

###<a name="normal-distribution-generator"></a>Normale Verteilung Generator
Dieser Dienst 3 Argumente einer normalen Verteilung und generiert eine zufällige Folge von Zahlen, die normalerweise verteilt werden. Die folgenden vorgesehen, innerhalb der Anforderung:

* n – die Anzahl der Messwerte. 
* Mittel - Mittel normale Verteilung.
* SD - normale Verteilung der Standardabweichung. 

Die Ausgabe des Diensts ist eine Sequenz der Länge n mit einer normalen Verteilung anhand der Mittelwert und sd-Argumente.

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (Beispiel apps sind: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx) [Wahrscheinlichkeit Rechner](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx) [Quantil-Rechner](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

###<a name="normal-distribution-quantile-calculator"></a>Normale Verteilung Quantil Rechner
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normale Verteilung Wahrscheinlichkeit Rechner
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Normale Verteilung Generator
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Dieser Webdienst wurde mit Azure Machine Learning erstellt. Eine kostenlose Testversion sowie einführende Videos Experimente und [publishing-Webdienste](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). 

Unten ist ein Screenshot des Versuchs, die Web Service und Beispielcode für jedes der Module innerhalb des Experiments erstellt.

###<a name="normal-distribution-quantile-calculator"></a>Normale Verteilung Quantil Rechner

Flow Experiment:

![Experiment Fluss][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normale Verteilung Wahrscheinlichkeit Rechner
Flow Experiment:

![Experiment Fluss][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Normale Verteilung Generator
Flow Experiment:

![Experiment Fluss][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Grenzen 

Sehr einfache Beispiele der normalen Verteilung umgeben sind. Wie aus den oben stehenden Beispielcode ersichtlich, kleine Fehler abfangen erfolgt.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 

<properties 
    pageTitle="Binomialverteilung Suite | Microsoft Azure" 
    description="Binomialverteilung Suite" 
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


#<a name="binomial-distribution-suite"></a>Binomialverteilung Suite




Binomialverteilung Suite ist eine Reihe von generieren und Umgang mit Binomialverteilung bei Beispiel Web Services ([Binomial Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5) [Wahrscheinlichkeit Rechner]( https://datamarket.azure.com/dataset/aml_labs/bdp4) [Quantil-Rechner]( https://datamarket.azure.com/dataset/aml_labs/bdq5)). Den Diensten können nacheinander Binomialverteilung längere Berechnung Quantile generieren, bestimmten Wahrscheinlichkeit und Berechnung Wahrscheinlichkeit von bestimmten Quantil. Die Dienste gibt verschiedene Ergebnisse basierend auf den ausgewählten Dienst aus (siehe unten). Binomialverteilung Suite basiert auf der R Funktionen Qbinom, Rbinom und Pbinom, R-Statistik-Paket enthalten sind. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Diese Webdienste können von Benutzer möglicherweise direkt auf dem Markt über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer beispielsweise genutzt werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann dann Azure Marketplace veröffentlicht und von Benutzern und Geräten weltweit genutzt – keine Infrastruktur Setup vom Autor des Webdiensts erforderlich ist.

##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes
Binomialverteilung Suite beinhaltet 3.

###<a name="binomial-distribution-quantile-calculator"></a>Binomialverteilung Quantil Rechner
Dieser Dienst 4 Argumente einer normalen Verteilung und zugeordnete Quantil berechnet.
Die Eingabeargumente sind:

- p - einem aggregierten Wahrscheinlichkeit mehrere Versuche.  
- Größe - die Anzahl der Versuche.
- Prob - Wahrscheinlichkeiten in eine Testversion.
- Seite - L für den unteren Rand der Verteilung, U für den oberen Rand der Verteilung. 

Die Ausgabe des Diensts ist das berechnete Quantil angegebenen Wahrscheinlichkeit zugeordnet ist.

###<a name="binomial-distribution-probability-calculator"></a>Binomialverteilung Wahrscheinlichkeit Rechner
Dieser Dienst nimmt 4 Argumente einer Binomialverteilung und zugeordnete Wahrscheinlichkeit berechnet.
Die Eingabeargumente sind:

- Q - ein einzelnes Quantil eines Ereignisses mit Binomialverteilung. 
- Größe - die Anzahl der Versuche.
- Prob - Wahrscheinlichkeiten in eine Testversion.
- Seite - L für den unteren Rand der Verteilung, U für den oberen Rand der Verteilung oder E eine einzelne Zahl der Erfolge übereinstimmt.

Die Ausgabe des Dienstes ist die berechnete Wahrscheinlichkeit angegebenen Quantil zugeordnet ist.

###<a name="binomial-distribution-generator"></a>Binomialverteilung Generator
Dieser Dienst akzeptiert 3 Argumente einer Binomialverteilung und generiert eine zufällige Folge von Zahlen, die binomially verteilt werden. Die folgenden vorgesehen, innerhalb der Anforderung:

- n – Anzahl der Messwerte. 
- Größe - die Anzahl der Versuche.
- Prob - Wahrscheinlichkeit des Erfolgs.

Die Ausgabe des Diensts ist eine Sequenz der Länge n mit einer Binomialverteilung anhand der Größe und Probleme Argumente.

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (Beispiel apps sind: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx) [Wahrscheinlichkeit Rechner](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx) [Quantil-Rechner](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

###<a name="binomial-distribution-quantile-calculator"></a>Binomialverteilung Quantil Rechner
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Binomialverteilung Wahrscheinlichkeit Rechner
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Binomialverteilung Generator
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Binomialverteilung Quantil Rechner

![Arbeitsbereich erstellen][4]

####<a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Modul 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Binomialverteilung Wahrscheinlichkeit Rechner

![Arbeitsbereich erstellen][5]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Binomialverteilung Generator

![Arbeitsbereich erstellen][6]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Grenzen 
Sehr einfache Beispiele das Binomialverteilung umgeben sind. Wie aus den oben stehenden Beispielcode ersichtlich, kleine Fehler abfangen erfolgt.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 

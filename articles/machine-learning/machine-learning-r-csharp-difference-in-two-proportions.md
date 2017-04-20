<properties 
    pageTitle="Unterschied zwischen Proportionen Test | Microsoft Azure" 
    description="Unterschied zwischen Proportionen Test" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Unterschied zwischen Proportionen Test


Sind beiden statistisch verschieden Angenommen, ein Benutzer möchte vergleichen zwei Filme zu bestimmen, ob ein Film einen wesentlich höheren Anteil der "mag" Wenn die andere. Mit einem großen Beispiel könnte ein statistisch signifikanter Unterschied zwischen den Proportionen 0,50 und 0,51. Mit einem kleinen Beispiel gibt es möglicherweise nicht genügend Daten, um festzustellen, ob diese Anteile tatsächlich unterscheiden. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/prop_test) führt eine Hypothese Unterschied in zwei Abschnitte, die Benutzereingaben die Anzahl der Erfolge und die Gesamtzahl der Versuche für die Gruppen 2 Vergleich. In einem möglichen Szenario konnte diesen Webdienst von einem Film Vergleich App dem Benutzer mitteilt, ob einer der Filme "gefallen ist" häufiger als andere, basierend auf Bewertungen aufgerufen werden.

>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer verwendet werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.


##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes

Dieser Dienst 4 Argumente akzeptiert und testet eine Hypothese Proportionen.

Die Eingabeargumente sind:

* Successes1 - Anzahl der Erfolgsereignisse in Beispiel 1.
* Successes2 - Anzahl der Erfolgsereignisse in Beispiel 2.
* Total1 - Größe von Beispiel 1.
* Summe2 - Größe von Beispiel 2.

Die Ausgabe des Diensts ist das Ergebnis die Hypothese testen mit der Chi-Quadrat-Statistik, df, p-Wert und Anteil in Beispiel 1/2 und Konfidenzintervall Grenzen.

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

Von in Azure Machine Learning ein neues leeres Experiment wurde mit zwei [R-Skript ausführen] [ execute-r-script] Module. Im ersten Modul definiert das Schema verwendet das zweite Modul des Befehls prop.test R die Hypothese 2 Proportionen ausführen. 


###<a name="experiment-flow"></a>Flow Experiment:

![Experiment Fluss][2]


####<a name="module-1"></a>Modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Modul 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Grenzen 

Dies ist ein sehr einfaches Beispiel für einen Test der Unterschied zwischen 2 Proportionen. Wie aus den oben stehenden Beispielcode ersichtlich, wird keine Fehler abgefangen und der Dienst geht davon aus, dass alle Variablen sind.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 

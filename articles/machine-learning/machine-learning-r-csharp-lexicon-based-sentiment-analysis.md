<properties 
    pageTitle="Lexikon basierend Stimmungsanalyse | Microsoft Azure" 
    description="Lexikon basierend Stimmungsanalyse" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Lexikon basierend Stimmungsanalyse 

Wie messen Sie Benutzer Stellungnahmen und Haltung gegenüber Marken oder Themen soziale Netzwerke wie Facebook-Beiträge, Tweets, Kritiken usw..? Stimmungsanalyse bietet eine Methode zum Analysieren von Fragen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Im Allgemeinen werden zwei Methoden für die stimmungsanalyse. Beaufsichtigte lernalgorithmus verwendet und die andere als unbeaufsichtigte lernen behandelt werden. Beaufsichtigte lernalgorithmus erstellt ein klassifizierungsmodell im Allgemeinen auf eine große kommentierte trennen. Seine Genauigkeit ist hauptsächlich auf die Qualität der Anmerkung und normalerweise das Training dauert sehr lange. Außerdem beim Anwenden des Algorithmus auf eine andere Domäne ist das Ergebnis in der Regel nicht gut. Im Vergleich zu lernen, Lexikon-basierte unbeaufsichtigte lernen verwendet Stimmung Wörterbuch dar, die Speichern von großen Datenmengen Corpus und Schulung erfordert überwacht –, wodurch des gesamten Prozess deutlich schneller. 

Unser [Service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) basiert auf MPQA Subjektivität Lexikon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), eines der am häufigsten verwendeten Subjektivität Lexika. In MPQA gibt 5097 negativ und 2533 positive Wörter. Und alle Wörter werden als stark oder schwach Polarität. Die gesamte Sammlung wird unter der GNU General Public License. Der Webdienst kann eine kurze Sätze Tweets wie Facebook Beiträge angewendet werden. 

>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer genutzt werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.

##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes

Die eingegebenen Daten können Text aber der Webdienst arbeitet besser mit kurzen Sätzen. Die Ausgabe ist eine Zahl zwischen-1 und 1. Jeder Wert unter 0 gibt an, dass die Absicht des Textes negativ ist; positive über 0. Der Absolute Wert des Ergebnisses gibt die Stärke der zugehörigen Stimmung. 

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Die Eingabe ist "heute einen schönen Tag." Die Ausgabe ist '1', womit eine positive Stimmung der Eingabesatz zugeordnet. 

##<a name="creation-of-web-service"></a>Erstellen des Webdiensts
>Dieser Webdienst wurde mit Azure Machine Learning erstellt. Eine kostenlose Testversion sowie einführende Videos Experimente und [publishing-Webdienste](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Unten ist ein Screenshot des Versuchs, die Web Service und Beispielcode für jedes der Module innerhalb des Experiments erstellt.


Aus in Azure Machine Learning wurde ein neues leeres Experiment erstellt. Die Abbildung zeigt den Fluss Experiment Stimmung Lexikon-basierte Analyse. Die Datei "sent_dict.csv" ist MPQA Subjektivität Lexikon und als Eingaben [Ausführen R Skript]festgelegt[execute-r-script]. Eine andere Eingangsquelle ist aufgenommenen Überprüfung von Amazon Überprüfung Dataset für Test, wo wir Auswahl Spalte Namensänderung und teilen Sie die Arbeitsgänge. Wir verwenden ein Hash-Paket Subjektivität Lexikon Speicher und Bewertung Berechnung beschleunigen. Der Text "tm" Paket zerlegt und gegenüber dem Wort im Wörterbuch Stimmung. Schließlich eine Bewertung berechnet das Gewicht jedes subjektive Wortes im Text hinzufügen. 

###<a name="experiment-flow"></a>Flow Experiment:

![Experimentieren Sie Fluss][2]


####<a name="module-1"></a>Modul 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Hash-Paket installieren
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Grenzen

Aus Sicht der Algorithmus ist Lexikon-basierte stimmungsanalyse eine allgemeine Stimmung, die besser Klassifizierungsmethode für bestimmte Felder nicht ausführen kann. Negation Problem nicht auch behandelt. Wir hartcodieren mehrere Negation Wörter im Programm aber besser eine Negation Wörterbuch und einige Regeln erstellen. Der Webdienst eine bessere auf kurze und einfache Sätze Tweets wie Facebook Beiträge als auf lange und komplexe Sätze wie Amazon-Bewertungen. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 

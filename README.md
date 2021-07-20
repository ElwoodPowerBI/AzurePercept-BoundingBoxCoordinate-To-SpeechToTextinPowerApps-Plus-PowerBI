# AzurePercept-BoundingBoxCoordinate-To-SpeechToTextinPowerApps-Plus-PowerBI
Using the Camera/Vision module, extracting the X coordinate for the bounding box, sending over Stream Analytics to chart in PowerBI, and then sending to PowerApps to connect with TextToSpeech to play audio

# Architecture
![image](https://user-images.githubusercontent.com/79670628/126335307-5dec7208-e4f4-42ca-9318-6bf55318f978.png)

## I now plan on using this process and code to :
### Run an Azure Percept in ArtPrize GrandRapids for this project.  
This project will recognize ballet steps, convert to a mapped emotion and then display that as abstract digital art. 
https://www.artprize.org/70976
### Run an Azure Percept for the SuperHeroes Foundation in Puerto Rico to help children create their own sign language
link TBD

# Overview
This repository will document and have instructions on how to :
     - Use the Azure Percept Dev Kit to detect and draw a bounding box around a person
     - Parse the bounding box coordinates in Azure Stream Analytics and send that to both blob storage and to PowerBI
     - Plot the x coordinate in PowerBI to watch in real time the size of the bounding box increase/decrease as your arm moves in and out.
     - Create a PowerApp that plays back audio to read back Microsoft Translater API call to read back x coordinate.
     - Map x coordinates to music notes 
# Azure Percept Dev Kit
- https://azure.microsoft.com/en-us/services/azure-percept/
- Percept Setup instructions will be here.  TBD

# Events to Storage Queue
- In a parallel path to Stream Analytics, I will be creating a parallel route using Events to Storage Queue to Logic Apps.

I was able to :
1) feed IoT Hub messages into a storage queue
2) pull each message and parse out the "label" from the JSON packet
3) Push the "label" back onto the queue (may need to put this on a seperate que)
4) delete message from the original queue.
- However, all the transactions and the large number of messages put me over my spending limit.  
Plan : I am looking at adding Stream Analytics on the Edge device to reduce the number of messages before they get to a queue or blob storage.

![image](https://user-images.githubusercontent.com/79670628/126335781-4fb92995-d798-450f-941e-9008893079b4.png)

# Stream Analytics
- Stream Analytics code to parse out JSON and identify X coordinate will be here.

## Stream Anlytics Query 
      
- When I first passed all the data from my Percept into PowerBI over streamanalytics I would get "array" instead of the bounding box coordinates.
- I then tried the following code in the Stream Analytics query which worked nicely to parse out the individual bounding box coordinate values.

```
SELECT
    Percept.ArrayValue.label,
    Percept.ArrayValue.confidence,
    GetArrayElement(Percept.ArrayValue.bbox, 0) AS bbox0,
    GetArrayElement(Percept.ArrayValue.bbox, 1) AS bbox1,
    GetArrayElement(Percept.ArrayValue.bbox, 2) AS bbox2,
    GetArrayElement(Percept.ArrayValue.bbox, 3) AS bbox3,
    Percept.ArrayValue.bbox,
CAST (udf.main(Percept.ArrayValue.timestamp) as DateTime) as DETECTION_TIMESTAMP,
    Percept.ArrayValue.timestamp
INTO
    "PowerBI Report Name"
FROM
    "IOTHUB connection" as event
    CROSS APPLY GetArrayElements(event.Neural_Network) AS Percept
WHERE
    CAST(Percept.ArrayValue.confidence as Float) > 0.6
```  

## Stream Analytics Add Function (User Defined Function or UDF)

Here is the code that is used to convert the time date stamp into something usable.
Anybody now how to convert it into your timezone?

```
function main(nanoseconds) {

    var epoch = nanoseconds * 0.000000001;

    var d = new Date(0);

    d.setUTCSeconds(epoch);

    return (d.toISOString());

}
```

Plan for this section
- Document output in PowerBI, and upload a PBIX file of output example.
- Document steps I used and documentation I read to come up with the Stream Analytics query.
- Document how to create inputs from IOT HUB and output to PowerBI stream analytics - and some tips to make it work if changes are made to the query.

# PowerBI setup
Instructions to connect to PowerBI to monitor data in visuals will be here.

# PowerApps Setup
Instructions to setup PowerApps and connect to Microsoft Translator Speech to Text will be here.

# A fun adventure!!
I will update daily as I learn more 😁

# AzurePercept-BoundingBoxCoordinate-To-SpeechToTextinPowerApps-Plus-PowerBI
Using the Camera/Vision module, extracting the X coordinate for the bounding box, sending over Stream Analytics to chart in PowerBI, and then sending to PowerApps to connect with TextToSpeech to play audio
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
# Stream Analytics
      - Stream Analytics code to parse out JSON and identify X coordinate will be here.
# PowerBI setup
      - Instructions to connect to PowerBI to monitor data in visuals will be here.
# PowerApps Setup
      - Instructions to setup PowerApps and connect to Microsoft Translator Speech to Text will be here.

# A fun adventure!!
    - I will update daily as I learn more 😁

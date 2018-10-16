# Speech-to-Text Custom Language Model
1.  Background
Teams working to train Speech-to-Text Models for Custom Language Models and Acoustic Models will need to do the following steps in Development as well as Production as currently there is not a supported vehicle for moving fully trained models from Development to Production.   

Replace {username} and {password} in the commands below with the appropriate credentials for the Speech-To-Text service instance.  If using Identity and Access Management (IAM) API Key, replace {username}:{password} with “apikey:{apikey}” where {apikey} is the IAM API Key value for the Speech-To-Text service instance.
    	

2.  Create a Custom Speech-to-Text Model
curl -X POST -u "{username}:{password}"
  --header "Content-Type: application/json"
  --data "{\"name\": \"Custom model name \",
    \"base_model_name\": \"en-US_BroadbandModel\", \
    \"description\": \"Custom Model Description\"}" \
  "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations"

[Note:  returns a unique "customization_id" ]

3.  Add Training Data to Custom Language Model using a Corpus File

Example corpus file
  What days does United Postal Service delivery Amazon Packages?
  How many packages can be delivered in a single day?
  How do I contact customer service?
  
Command to upload data from Corpus File  

CUSTOMIZATION_ID=<customization id returned when creating custom model>
CORPUS_FILE=<text file containing training data>
CORPUS_NAME=<naming convention used to identify training data - i.e. trainingDataV1 ]
URL=”https://stream.watsonplatform.net/speech-to-text/api/v1” 
      


curl -X POST $INSECURE -i -u "{username}:{password}" \
  --data-binary "{<path>/CorpusFileName}" \
  "$URL/customizations/{customization_id}/corpora/{CorpusName}"

Example of commands to upload single words (if not using Corpus File)

Note:  Example uses TCP/IP 
 
 curl -X PUT  -u "{username}:"${password}" \
  --header "Content-Type: application/json" \
  --data "{\"sounds_like\": [\"T. C. P. I. P.\"], \"display_as\": \"TCP/IP\"}" \
  "$URL/customizations/$CUSTOMIZATION_ID/words/tcpip"
 
 
 4.  Train Custom Language Model
This step is required to run after any upload of data from a Corpus File or Single-word upload.  If this is not run, the data uploaded will not be available for use by the Custom STT model.
 
 curl -X POST -u "{username}":"{password}" \
  "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/train"
  
  Note:  depending upon amount of data provided for training, can take mins or several hours


5.  Validate Training Completion

curl -X GET  -u "<username>":"{password} \
      "https://stream/watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}/corpora/{CorpusName}

6.  Delete Custom Language Model

curl -X DELETE  -u "{username}":"{password" \
  "https://stream.watsonplatform.net/speech-to-text/api/v1/customizations/{customization_id}"



  
  

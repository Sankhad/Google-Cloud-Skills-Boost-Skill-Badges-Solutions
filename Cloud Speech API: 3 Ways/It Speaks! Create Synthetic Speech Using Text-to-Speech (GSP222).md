# It Speaks! Create Synthetic Speech Using Text-to-Speech || [GSP222](https://www.cloudskillsboost.google/focuses/1290?parent=catalog) ||

### Run the following Commands in CloudShell

```
#!/bin/bash
# Define color variables

BLACK=`tput setaf 0`
RED=`tput setaf 1`
GREEN=`tput setaf 2`
YELLOW=`tput setaf 3`
BLUE=`tput setaf 4`
MAGENTA=`tput setaf 5`
CYAN=`tput setaf 6`
WHITE=`tput setaf 7`

BG_BLACK=`tput setab 0`
BG_RED=`tput setab 1`
BG_GREEN=`tput setab 2`
BG_YELLOW=`tput setab 3`
BG_BLUE=`tput setab 4`
BG_MAGENTA=`tput setab 5`
BG_CYAN=`tput setab 6`
BG_WHITE=`tput setab 7`

BOLD=`tput bold`
RESET=`tput sgr0`
#----------------------------------------------------start--------------------------------------------------#

echo "${BG_MAGENTA}${BOLD}Starting Execution${RESET}"

export PROJECT_ID=$(gcloud config get-value project)

gcloud services enable texttospeech.googleapis.com

gcloud iam service-accounts create tts-qwiklab

gcloud iam service-accounts keys create tts-qwiklab.json --iam-account tts-qwiklab@$PROJECT_ID.iam.gserviceaccount.com

export GOOGLE_APPLICATION_CREDENTIALS=tts-qwiklab.json

echo "${RED}${BOLD}Congratulations${RESET}" "${WHITE}${BOLD}for${RESET}" "${GREEN}${BOLD}Completing the Lab !!!${RESET}"

echo "${CYAN}${BOLD}With Regards Himadri${RESET}"

#-----------------------------------------------------end----------------------------------------------------------#

```

# Congratulations 🎉 for completing the Challenge Lab !

##### *You Have Successfully Demonstrated Your Skills And Determination.*

#### *Well done!*

# APIs Explorer: Cloud Storage || [GSP421](https://www.cloudskillsboost.google/focuses/3632?parent=catalog) ||


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

echo "${YELLOW}${BOLD}Starting${RESET}" "${GREEN}${BOLD}Execution${RESET}"

gsutil mb gs://$DEVSHELL_PROJECT_ID

gsutil mb gs://$DEVSHELL_PROJECT_ID-2

curl -LO raw.githubusercontent.com/Himadri8991/Google-Arcade-Skill-Badges-Solutions/main/Get%20Started%20with%20Cloud%20Storage/APIs%20Explorer%3A%20Cloud%20Storage/demo-image1.png

curl -LO raw.githubusercontent.com/Himadri8991/Google-Arcade-Skill-Badges-Solutions/main/Get%20Started%20with%20Cloud%20Storage/APIs%20Explorer%3A%20Cloud%20Storage/demo-image2.png

curl -LO raw.githubusercontent.com/Himadri8991/Google-Arcade-Skill-Badges-Solutions/main/Get%20Started%20with%20Cloud%20Storage/APIs%20Explorer%3A%20Cloud%20Storage/demo-image1-copy.png

gsutil cp demo-image1.png gs://$DEVSHELL_PROJECT_ID/demo-image1.png

gsutil cp demo-image2.png gs://$DEVSHELL_PROJECT_ID/demo-image2.png

gsutil cp demo-image1-copy.png gs://$DEVSHELL_PROJECT_ID-2/demo-image1-copy.png

echo "${RED}${BOLD}Congratulations${RESET}" "${WHITE}${BOLD}for${RESET}" "${GREEN}${BOLD}Completing the Lab !!!${RESET}"
echo "${CYAN}${BOLD}With Regards Himadri${RESET}"
#-----------------------------------------------------end----------------------------------------------------------#
```

# Congratulations 🎉 for completing the Challenge Lab !

##### *You Have Successfully Demonstrated Your Skills And Determination.*

#### *Well done!*

#### Don't Forget to Join the [WhatsApp Group](https://chat.whatsapp.com/Cxmw4DvCwEHCqU8qzTpv6r) 

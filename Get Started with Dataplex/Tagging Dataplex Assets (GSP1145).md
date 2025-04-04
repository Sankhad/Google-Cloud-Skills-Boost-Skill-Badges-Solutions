# Tagging Dataplex Assets || [GSP1145](https://www.cloudskillsboost.google/course_templates/726/labs/461570) ||

### Run the following Commands in CloudShell
### From "Task 1" you can find your REGION,
```
export REGION=
```
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

gcloud services enable dataplex.googleapis.com

gcloud services enable datacatalog.googleapis.com

gcloud dataplex lakes create orders-lake \
  --location=$REGION \
  --display-name="Orders Lake"

gcloud dataplex zones create customer-curated-zone \
    --location=$REGION \
    --lake=orders-lake \
    --display-name="Customer Curated Zone" \
    --resource-location-type=SINGLE_REGION \
    --type=CURATED \
    --discovery-enabled \
    --discovery-schedule="0 * * * *"

gcloud dataplex assets create customer-details-dataset \
--location=$REGION \
--lake=orders-lake \
--zone=customer-curated-zone \
--display-name="Customer Details Dataset" \
--resource-type=BIGQUERY_DATASET \
--resource-name=projects/$DEVSHELL_PROJECT_ID/datasets/customers \
--discovery-enabled

gcloud data-catalog tag-templates create protected_data_template --location=$REGION --field=id=protected_data_flag,display-name="Protected Data Flag",type='enum(YES|NO)' --display-name="Protected Data Template"

echo "${CYAN}${BOLD}Click here: "${RESET}""${BLUE}${BOLD}"https://console.cloud.google.com/dataplex/search?project=$DEVSHELL_PROJECT_ID&qSystems=DATAPLEX""${RESET}"

echo "${CYAN}${BOLD}With Regards Himadri${RESET}"

#-----------------------------------------------------end----------------------------------------------------------#

```
# ```Click``` here: (https://console.cloud.google.com/dataplex/search?project=qwiklabs-gcp-00-e3e1a050e3ec)
### Systems :> *Select* ```DATAPLEX```
### Click :> ```customer_details``` > ```Attach Tags``` 
### For Choose what to tag, enable the checkboxes for the following columns:
* zip
* state
* last_name
* country
* email
* latitude
* first_name
* city
* longitude
### Click ```OK```
### Choose the tag templates
### Protected Data Template ```YES```
### Save

## Congratulations 🎉 for completing the Challenge Lab !

##### *You Have Successfully Demonstrated Your Skills And Determination.*

#### *Well done!*

### Don't Forget to Join the [WhatsApp Group](https://chat.whatsapp.com/Cxmw4DvCwEHCqU8qzTpv6r) 

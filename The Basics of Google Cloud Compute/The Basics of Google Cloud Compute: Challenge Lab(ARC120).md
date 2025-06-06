# The Basics of Google Cloud Compute: Challenge Lab || [ARC120](https://www.cloudskillsboost.google/course_templates/754/labs/461563) ||

* Go to `Create a bucket` from [here](https://console.cloud.google.com/storage/create-bucket?)
* Go to `TASK-1` > find your `Bucket Name` > `CREATE` > `Continue`

## Run the following Commands in CloudShell

### From "Challenge Scenario" you can find your Zone,

```
export ZONE=
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

gcloud compute instances create my-instance \
    --machine-type=e2-medium \
    --zone=$ZONE \
    --image-project=debian-cloud \
    --image-family=debian-11 \
    --boot-disk-size=10GB \
    --boot-disk-type=pd-balanced \
    --create-disk=size=100GB,type=pd-standard,mode=rw,device-name=additional-disk \
    --tags=http-server

gcloud compute disks create mydisk \
    --size=200GB \
    --zone=$ZONE

gcloud compute instances attach-disk my-instance \
    --disk=mydisk \
    --zone=$ZONE

sleep 15

cat > prepare_disk.sh <<'EOF_END'

sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx

EOF_END

gcloud compute scp prepare_disk.sh my-instance:/tmp --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --quiet

gcloud compute ssh my-instance --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --quiet --command="bash /tmp/prepare_disk.sh"

echo "${BG_RED}${BOLD}Congratulations For Completing The Lab !!!${RESET}"
echo "${CYAN}${BOLD}With Regards Himadri${RESET}"
#-----------------------------------------------------end----------------------------------------------------------#

```

### Please Ignore all the Errors

# Congratulations 🎉 for completing the Challenge Lab !

##### *You Have Successfully Demonstrated Your Skills And Determination.*

#### *Well done!*

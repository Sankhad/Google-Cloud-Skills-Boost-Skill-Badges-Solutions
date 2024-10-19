# Cloud Monitoring: Qwik Start || [GSP089](https://www.cloudskillsboost.google/focuses/10599?parent=catalog) ||

### Run the following Commands in CloudShell

###  From "Setup and requirements" you can find your ZONE. [Setup and requirements > Set your region and zone]

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

export REGION="${ZONE%-*}"

gcloud config set compute/zone $ZONE
gcloud config set compute/region $REGION

# Create the instance with the necessary metadata and tags
gcloud compute instances create lamp-1-vm \
    --project=$DEVSHELL_PROJECT_ID \
    --zone=$ZONE \
    --machine-type=e2-small \
    --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default \
    --metadata=enable-oslogin=true \
    --maintenance-policy=MIGRATE \
    --provisioning-model=STANDARD \
    --tags=http-server \
    --create-disk=auto-delete=yes,boot=yes,device-name=lamp-1-vm,image=projects/debian-cloud/global/images/debian-10-buster-v20230629,mode=rw,size=10,type=projects/$DEVSHELL_PROJECT_ID/zones/$ZONE/diskTypes/pd-balanced \
    --no-shielded-secure-boot \
    --shielded-vtpm \
    --shielded-integrity-monitoring \
    --labels=goog-ec-src=vm_add-gcloud \
    --reservation-affinity=any

# Create firewall rule to allow incoming HTTP traffic on port 80
gcloud compute firewall-rules create allow-http \
    --project=$DEVSHELL_PROJECT_ID \
    --direction=INGRESS \
    --priority=1000 \
    --network=default \
    --action=ALLOW \
    --rules=tcp:80 \
    --source-ranges=0.0.0.0/0 \
    --target-tags=http-server

cat > prepare_disk.sh <<'EOF_END'

sudo apt-get update

sudo apt-get install -y apache2 php7.0

sudo service apache2 restart

EOF_END

gcloud compute scp prepare_disk.sh lamp-1-vm:/tmp --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --quiet

gcloud compute ssh lamp-1-vm --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --quiet --command="bash /tmp/prepare_disk.sh"

cat > alert_config.json <<EOF
{
  "displayName": "Inbound Traffic Alert",
  "userLabels": {},
  "conditions": [
    {
      "displayName": "VM Instance - Network traffic",
      "conditionThreshold": {
        "filter": "resource.type = \"gce_instance\" AND metric.type = \"agent.googleapis.com/interface/traffic\"",
        "aggregations": [
          {
            "alignmentPeriod": "300s",
            "crossSeriesReducer": "REDUCE_NONE",
            "perSeriesAligner": "ALIGN_RATE"
          }
        ],
        "comparison": "COMPARISON_GT",
        "duration": "60s",
        "trigger": {
          "count": 1
        },
        "thresholdValue": 500
      }
    }
  ],
  "alertStrategy": {
    "autoClose": "604800s"
  },
  "combiner": "OR",
  "enabled": true
}
EOF

gcloud alpha monitoring policies create --policy-from-file=alert_config.json

echo "${CYAN}${BOLD}With Regards Himadri${RESET}"
#-----------------------------------------------------end----------------------------------------------------------#
```

### Go to `Create Uptime Check` from [here](https://console.cloud.google.com/monitoring/uptime/create?)

### For Resource Type `Instance` > Click `Instance*` > Select `lamp-1-vm` > Click `Continue` > Click `Continue` > Click `Continue` > For Title, type `Lamp Uptime Check` > Create

# Congratulations 🎉 for completing the Challenge Lab !

### *You Have Successfully Demonstrated Your Skills And Determination.*

## *Well done!*

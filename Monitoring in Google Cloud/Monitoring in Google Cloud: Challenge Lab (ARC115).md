# Monitoring in Google Cloud: Challenge Lab || [ARC115](https://www.cloudskillsboost.google/focuses/63855?parent=catalog) ||

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

cat > prepare_disk.sh <<'EOF_END'

curl -sSO https://dl.google.com/cloudagents/add-logging-agent-repo.sh
sudo bash add-logging-agent-repo.sh --also-install

curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
sudo bash add-monitoring-agent-repo.sh --also-install

(cd /etc/stackdriver/collectd.d/ && sudo curl -O https://raw.githubusercontent.com/Stackdriver/stackdriver-agent-service-configs/master/etc/collectd.d/apache.conf)

sudo service stackdriver-agent restart

timeout 120 bash -c -- 'while true; do curl localhost; sleep $((RANDOM % 4)) ; done'

EOF_END

export ZONE=$(gcloud compute instances list apache-vm --format 'csv[no-heading](zone)')

gcloud compute scp prepare_disk.sh apache-vm:/tmp --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --quiet

gcloud compute ssh apache-vm --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --quiet --command="bash /tmp/prepare_disk.sh"

sleep 60

cat > email-channel.json <<EOF_END
{
  "type": "email",
  "displayName": "quickgcplab",
  "description": "Awesome",
  "labels": {
    "email_address": "$USER_EMAIL"
  }
}
EOF_END

gcloud beta monitoring channels create --channel-content-from-file="email-channel.json"

# Get the channel ID
email_channel_info=$(gcloud beta monitoring channels list)
email_channel_id=$(echo "$email_channel_info" | grep -oP 'name: \K[^ ]+' | head -n 1)

# Create an email notification channel

# Create the alert policy

cat > stopped-vm-alert-policy.json <<EOF_END
{
  "displayName": "quick gcp lab",
  "userLabels": {},
  "conditions": [
    {
      "displayName": "VM Instance - Traffic",
      "conditionThreshold": {
        "filter": "resource.type = \"gce_instance\" AND metric.type = \"agent.googleapis.com/apache/traffic\"",
        "aggregations": [
          {
            "alignmentPeriod": "60s",
            "crossSeriesReducer": "REDUCE_NONE",
            "perSeriesAligner": "ALIGN_RATE"
          }
        ],
        "comparison": "COMPARISON_GT",
        "duration": "0s",
        "trigger": {
          "count": 1
        },
        "thresholdValue": 3072
      }
    }
  ],
  "alertStrategy": {},
  "combiner": "OR",
  "enabled": true,
  "notificationChannels": [
    "$email_channel_id"
  ],
  "severity": "SEVERITY_UNSPECIFIED"
}


EOF_END

# Create the alert policy
gcloud alpha monitoring policies create --policy-from-file=stopped-vm-alert-policy.json

echo "${CYAN}${BOLD}With Regards Himadri${RESET}"

#-----------------------------------------------------end----------------------------------------------------------#
```

### Go to `Create Uptime Check` from [here](https://console.cloud.google.com/monitoring/uptime/create?)

### 1. Resource Type: `Instance`
### 2. Select Instance*: `apache-vm`
### 3. `Continue` > `Continue` > `Continue`
### 4. For Title: enter anything eg: `Pentaverse-India`
### 5. Create

### Go to `Dashboards` from [here](https://console.cloud.google.com/monitoring/dashboards?)

### 1. Go to `+ Create Dashboard` > Click `+ Add Widget` > Select `Line`

### 2. Go to `Select a metric`, Search `CPU load (1m)`

### 3. Under Popular resources click `VM Instance` > `Cpu` > `CPU load (1m)` > Apply > Apply

### 4. Click `+ Add Widget` > Select `Line`

### 5. Go to `Select a metric`, Search `Requests`

### 6. Under Popular resources click `VM Instance` > `Apache` > `Requests` > Apply > Apply



### Go to `Create log-based metric` from [here](https://console.cloud.google.com/logs/metrics/edit?)

### 1. Select Metric Type: `Distribution` > For Log-based metric name: enter Anything, eg: `Pentaverse-India`

### 2. Paste The Following in `Build filter` & Replace PROJECT_ID
```
resource.type="gce_instance"
logName="projects/PROJECT_ID/logs/apache-access"
textPayload:"200"
```
### 3. Field name: `textPayload`

### 4. Paste The Following in `Regular Expression` field:
```
execution took (\d+)
```
### 5. Create Metric

# Congratulations 🎉 for completing the Challenge Lab !

##### *You Have Successfully Demonstrated Your Skills And Determination.*

#### *Well done!*

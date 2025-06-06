# Monitoring and Logging for Cloud Functions || [GSP092](https://www.cloudskillsboost.google/focuses/1833?parent=catalog) ||


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

gcloud services enable \
  artifactregistry.googleapis.com \
  cloudfunctions.googleapis.com \
  cloudbuild.googleapis.com \
  eventarc.googleapis.com \
  run.googleapis.com \
  logging.googleapis.com \
  pubsub.googleapis.com

mkdir ~/hello-http && cd $_
touch index.js && touch package.json

cat > index.js <<EOF
/**
 * Responds to any HTTP request.
 *
 * @param {!express:Request} req HTTP request context.
 * @param {!express:Response} res HTTP response context.
 */
exports.helloWorld = (req, res) => {
    let message = req.query.message || req.body.message || 'Hello World!';
    res.status(200).send(message);
  };
  
EOF

cat > package.json <<EOF
{
    "name": "sample-http",
    "version": "0.0.1"
  }
  
EOF



deploy_function() {
  gcloud functions deploy helloWorld \
--trigger-http \
--runtime nodejs20 \
--allow-unauthenticated \
--region $REGION \
--max-instances 5
}

deploy_success=false

while [ "$deploy_success" = false ]; do
  if deploy_function; then
    echo "Cloud Run service is created. Exiting the loop."
    deploy_success=true
  else
    echo "Waiting for Cloud Run service to be created..."
    sleep 60
  fi
done

echo "Running the next code..."

curl -LO 'https://github.com/tsenart/vegeta/releases/download/v6.3.0/vegeta-v6.3.0-linux-386.tar.gz'

tar xvzf vegeta-v6.3.0-linux-386.tar.gz

gcloud logging metrics create CloudFunctionLatency-Logs \
    --project=$DEVSHELL_PROJECT_ID \
    --description="awesome lab" \
    --log-filter='resource.type="cloud_function" AND resource.labels.function_name="helloWorld" AND log_name="projects/$DEVSHELL_PROJECT_ID/logs/cloudaudit.googleapis.com%2Factivity" AND resource.labels.region="$REGION"'

echo "${RED}${BOLD}Congratulations${RESET}" "${WHITE}${BOLD}for${RESET}" "${GREEN}${BOLD}Completing the Lab !!!${RESET}"

echo "${CYAN}${BOLD}With Regards Himadri${RESET}"
#-----------------------------------------------------end----------------------------------------------------------#
```

* Note ***If you are still facing problems in `Task 1`, then go through with following these steps.***
  
  1. First Search `Cloud Run Functions` in the `Cloud Console` > Open `Cloud Run Functions`
  2. `Delete` the Function, named `helloWorld`
  3. Click `CREATE FUNCTION` > Function name `helloWorld` > Select your Region > Check once your Trigger Type is `HTTP` > In Authentication `Allow unauthenticated invocations` > Save
  4. Expand `Runtime, build, connections and security settings` > In Autoscaling `Maximum number of instances` set it to `5` > Next > Deploy


# Congratulations 🎉 for completing the Challenge Lab !

##### *You Have Successfully Demonstrated Your Skills And Determination.*

#### *Well done!*

# RaspberryPi-AzureDevopsAgent


My starting point is:

- Raspberry Pi 4 Model B Starter Kit
- 16gb SD card (if you intend to have large docker images then go bigger)
- Ubuntu 20.04 installed on the Rasparry Pi
- Raspberry Pi Imager v1.2
- PuTTY for connecting remotely to the Pi from a laptop (Optional)

## Azure Devops Setup
Check this link for details on creating your PAT token
  
  https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page
  
  Check this link for creating an App Pool
  
  https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser#capabilities

## Install Docker

```sudo apt install docker.io```

```sudo systemctl enable --now docker```

Check that it installed

```docker --version```


## Create Docker Image

1. Create a directory
  ```makdir agent```
  
2. Create Dockerfile
  ```nano Dockerfile```
  
3. Copy and past the contents of the Dockerfile from this repo (it's in the **linux-ARM directory**)

4. Create start.sh file
  ```nano start.sh```
  
5. Copy and past the contents of the start.sh from this repo (it's in the **linux-ARM directory**)

6. Build the image
  ```docker build -t dockeragent:latest .```
  
7. Run the image to create your container updayting the 2 variables for Project and PAT (Note my App pool is called SelfHosted so you many need to change that too)

  ```sudo docker run --restart=always  -e AZP_URL=https://dev.azure.com/<YourProject> -e AZP_TOKEN=<PAT TOKEN> -e AZP_POOL=SelfHosted -e AZP_AGENT_NAME=pi-agent1 --name DevopsAgent1 dockeragent:latest```
  
I have used the ```--restart=always``` so the container(s) will start automatically after a reboot. You can run multiple containers and get multiple agents if you wish, just change the ```AZP_AGENT_NAME``` and ```--name``` variables in step 7 above.
  
  

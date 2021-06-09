# shinyproxy_pycon21

Please feel free to clone and use this repo, the general idea of this is to be able to deploy shiny or dash applications using shinyproxy (https://www.shinyproxy.io/).

If you want you can watch this video for a more detailed explanation (TODO)

## Initial set up

0. Create your own github repo after this repo. On that repo you should also have at least 2 branches (main and development)

1. Create a cluster on GKE

2. Create 2 different namespaces running the following commands in your cluster

    - kubectl create namespace development
    - kubectl create namespace main

3. Create a key for the Default compute service account	and upload the value of the json as a secret to your own repo (name it GKE_SA_SECRET)

4. Create an additional secret with the value of the name of your project (name it GKE_PROJECT)


5. Push any change, the github actions should create the deployment of your app (i recommend that you do this step on the development branch)... Check the workloads to verify that the deployment is running ok.

6. Replace xxbranchxx (you should make this step twice, one for each namespace/branch) of the sp-authorization.yaml file and the run the command: kubectl apply -f sp-authorization.yaml 

7. Expose the deployment (you should make this step twice, one for each namespace/branch):  kubectl expose deployment shinyproxy --type=LoadBalancer --name=shinyonline --namespace=[development, main] ... After a few minutes you will be able to see the external ip running the command: kubectl get services

8. Access your app on externalIP:8080


## How to add a new dash application

0. Try to understand lines 37-42 of application.yml

1. Save the .py into Dash/apps folder

2. Please read the Dash/apps/app1.py file and add the lines that are required (lines 10-20 and 61-62)

3. Add the app to application.yml under specs

    - id should be unique and without spaces
    - container-cmd just change "app1" to the name .py file of your new app  (["gunicorn", "--bind", "0.0.0.0:8050","app1:server"])
    - leave port as 8050
    - container-image: leave it as  gcr.io/xxprojidxx/xxdashappsxx:xxbranchxx

4. Check the Dash/Dockerfile to add any dependency


## How to add a new dash application

0. Try to understand lines 31-35 of application.yml

1. Save folder that contains the app in the folder Shiny/

2. Modify the Shiny/Dockerfile to install al your dependencies 

3. Modify the Shiny/Dockerfile to copy the folder of your app to the container (save it in /root/yourappfolder)

3. Add the app to application.yml under specs

    - id should be unique and without spaces
    - container-cmd just change "pokedex" to "yourappfolder"  ["R", "-e", "shiny::runApp('/root/pokedex')"]
    - container-image: leave it as  gcr.io/xxprojidxx/xxshinyappsxx:xxbranchxx


## About github actions

The idea of this part is to be able to easily deploy the apps to GKE, the general idea of the github actions is to authenticate into the k8s cluster, change some variable names according to the branch and then deploy the new version of your apps. The general steps are:

1. log into the cluster
2. create the images
3. Change the files using the sed command to specify the namespace and branch
4. Deploy the app using kubectl













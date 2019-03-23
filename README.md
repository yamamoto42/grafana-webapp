# grafana-webapp
[![Docker Automated build](https://img.shields.io/docker/automated/yamamoto42/grafana-webapp.svg)](https://hub.docker.com/r/yamamoto42/grafana-webapp)

How to run Grafana with plug-in added with Azure App Service
Make App Service mount Azure Storage for persistence.

## Getting Started
1. Prepare
```
myloc="eastus"
myrg="gfrg"
myasp="gfasp"
myapp="gfapp"
mysta="gfsta"
mystc="gfstc"
az login
az account set --subscription <subscription_id>
az configure --defaults $myloc
```
2. Create App Service Plan
```
az group create -n $myrg -l $myloc
az appservice plan create -n $myasp -g $myrg --is-linux --sku B1
```
3. Create App Service and Deploy Container Image
```
az webapp create  \
-g $myrg  \
-p $myasp \
-n $myapp \
-i yamamoto42/grafana-webapp
```
4. Create Storage and Storage Container
```
az storage account create -n $mysta -g $myrg
az storage container create -n $mystc --account-name $mysta
```
5. Create Storage and Storage Container
```
a
```

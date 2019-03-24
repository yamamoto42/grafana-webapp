# grafana-webapp
[![Docker Automated build](https://img.shields.io/docker/automated/yamamoto42/grafana-webapp.svg)](https://hub.docker.com/r/yamamoto42/grafana-webapp)

How to run Grafana with plug-in added with Azure App Service
Make App Service mount Azure Storage for persistence.

## Getting Started
1. Prepare, set environment
```
myloc="eastus"
myrg="gfrg"
myasp="gfasp"
myapp="gfapp"
mysta="gfsta"
mystc="gfstc"
mysti="gfsti"
az login
az account set --subscription <subscription_id>
az configure --defaults location=$myloc
az configure --defaults group=$myrg
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
```
az webapp stop -n $myapp
```
4. Setting Port
```
az webapp config appsettings set \
-g $myrg  \
-n $myapp \
--settings PORT=3000
```
5. Create Storage Account and Storage Container(BLOB)
```
az storage account create -n $mysta -g $myrg
az storage container create -n $mystc --account-name $mysta
```
6. Check the key for storage account
```
az storage account keys list -n $mysta
```
7. Mount storage container(BLOB) to Grafana directory in App service
```
az webapp config storage-account add \
-g $myrg \
-n $myapp \
-i $mysti \
-a $mysta \
-k "<* key ********************************>" \
-sn $mystc \
-t AzureBlob \
-m /var/lib/grafana
```
8. Restart
```
az webapp restart -n $myapp
```

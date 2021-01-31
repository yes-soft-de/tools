# tools
this tools create new repo from template and deploy the new repo to google cloud
how this work flow is work :

tools/optin u can find this variable :
envpriv=true
if you want .env file and .htaccess file in privet repo but true and the tools will delete is in new repo and but is in directury with same name of new repo 
if you but fale the tools will add .env file and .htacces file in standerd position in new repo and the sensitive date will encripted with cloud secrets

OWENR=yes-cloud-env
name of owener of new repo for exa : yessoft/repo.git "yessoft" is owener
if the owner is not your account in github  u need write access to this account

REPO=tesd122
name of new repo u want to create 
if name is already exist in this account the error msg "already exist in this account " will appear in work flow steps 
the name shouldn't start with number "123" or char @!#$$% 

TEMPLATE="yes-soft-de/symfony-app-skeleton"
this is "TEMPLATE" repo and tools will be copy to new repo   
ACCESS=public
GKE_CLUSTER=yessoft-1    # TODO: update to cluster name
goolge cloud cluster to deploy 
GKE_ZONE=us-central1-c   # TODO: updte to cluster zone
goolge cloud zone




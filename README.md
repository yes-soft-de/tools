# tools
this tools create new repo from template with spicefic name and  with 2 brance and edit file of new repo and deploy the new repo to google cloud and but sensitive file in privet repo if u make ENVPRIV option on true in tools/optin and will create jwt (pub and priv) from keyparse key and but jwt it on privet repo in directory with same name of new repo to make it accessuble to devloper 


in tools/optin u will find this variable :
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

steps : 

name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
env:
  PROJECT_ID: ${{ secrets.GKE_PROJECT }}
  ORGANIZATION: yes-soft-de
  ENVREPO: environment-tools
  




# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  build:
    runs-on: ubuntu-latest
   
 

    steps:
      - uses: actions/checkout@v2
      at first i use actions/checkout@v2 to use tools repo
      - name: Run a multi-line script
        env: 
          REP: 123
        run: |
          source option 
          #here to read option file and make each line as variable
          #in next line  adding  normal variable to $GITHUB_ENV to make  variable scop covered  all steps in workflow 
          echo "RIPP=${REPO}" >> $GITHUB_ENV
          
          echo "envpriv=${envpriv}" >> $GITHUB_ENV
          echo "GKE_CLUSTER=${GKE_CLUSTER}" >> $GITHUB_ENV
          echo "GKE_ZONE=${GKE_ZONE}" >> $GITHUB_ENV
          echo "OWENR=${OWENR}" >> $GITHUB_ENV
          #echo "::set-env name=REP::$REPO"
          echo "path="${OWENR}/${REPO}.git"" >> $GITHUB_ENV
          echo "TEMPLATE=${TEMPLATE}" >> $GITHUB_ENV
          echo "${{ secrets.GHTOKEN }}" > token.txt
          
          echo txt is done 
          #gh cli login with token 
          gh auth login --with-token < token.txt
          echo login to gh is done
          #removing origin of tools repo 
          git remote rm origin 
          #change GHACCOUNT to yessoft when u finish 
          
          #in the newx line i add "Glory script" this Glory script create new repo from template but the code in this repo stay "Skeleton " and the need to edit 
          gh repo create $OWENR/$REPO  --template=$TEMPLATE --confirm --$ACCESS
          echo 0 
          #removing remote of new repo
          git remote rm origin 
          echo 0.1
          git config --global user.name ${{github.actor}}
          #add config to git to see how make new repo 
          echo 0.2
          git config --global user.email "yescloud@gmail.com"
          echo 0.3
          #magic touch and all work flow will not without this sleeeeeep 
          sleep 15
          echo 0.4
      - uses: actions/checkout@v2
        with:
         repository: ${{ env.path }}
         token: ${{ secrets.ENVGHTOKEN }}
         ref: symfony-app-skeleton
         #use new repo 


      - run: |
          echo 0.5
          ls -al
          #THE STATE OF ART START FROM HERE :
          
          cd k8s
          echo 0.4
          #I put some of the keywords that I used  to change these keywords BY USING SED 
          # and as a result of this, the entire project will be 
          #customized to fit the name of the new repo, including changing and adding PASSWORD 
          
          sed -i "s|FORREPLACE1235|$RIPP|g" server.yaml
          sed -i "s|PROJECT123|$PROJECT_ID|g" server.yaml
          sed -i "s|FORREPLACE1235|$RIPP|g" secret.yaml
          sed -i "s|FORREPLACE1235|$RIPP|g" mysql.yaml
          sed -i "s|FORREPLACE1235|$RIPP|g" namespace.yaml
          sed -i "s|FORREPLACE1235|$RIPP|g" configmap.yaml
          cd .. 
          sed -i "s|PROJECT_ID123|$PROJECT_ID|g" Dockerfile
          sed -i "s|SECRETNAME123|${RIPP}sec|g" Dockerfile
          if [[ $envpriv=='true' ]]
          #HERE IS THE ENV IS TRUE I DELETE TH COMMENT IN DOCKRE FILE TO MAKE IS AVIALABLE 
          then
          sed -i "s|#@1||g" Dockerfile
          fi
          echo 1
          #git remote rm origin 
          echo 2
          echo 3
          sed -i "s|FORREPLACE123|${RIPP}|g" .env
          cd public
          sed -i "s|FORREPLACE123|${RIPP}|g" .htaccess
          if [[ $envpriv=='true' ]]
          then
            rm -rf .htaccess
            cd ..
            rm -rf .env 
          fi 
          echo 4
          git add .
          echo 5
          git commit -a -m "iit"
          echo 6
          #git push --set-upstream origin symfony-app-skeleton
          #PUSH THE FILE TO NEW REPO 
          git push origin HEAD:symfony-app-skeleton
          echo 7
          git checkout -b dev symfony-app-skeleton
          #CREATE NEW BRANCH FROM symfony-app-skeleton
          echo 'this edited by tools repository for more informatin about this repository please visit https://yes-soft.de/category/blog/' >> README.md       
          git add .
          ls -al
          
          
          git commit -a -m "create dev b"
          git push origin HEAD:dev
          echo 8
      - uses: actions/checkout@v2
        with:
        #IF ENVPRIV IS TRUE  AND FILE  TO PRIVET REPO  
         repository: yes-soft-de/environment-tools
         token: ${{ secrets.ENVGHTOKEN }}
      - run: |
         echo 1
         mkdir ${{ env.RIPP  }}
         ls -la
         cd ${{ env.RIPP  }}
         echo 112
         ls -la
         echo $envpriv
         if [[ ${{ env.envpriv }}==true ]]
         then
           temp=https://github.com/${{ env.TEMPLATE }}.git
           echo $temp
           git clone $temp
           echo 2
           cd symfony-app-skeleton
           echo 3
           cp .env ../
           echo 4
           cd public
           echo 5
           cp .htaccess ../../
           echo 6
           cd ../../
           echo 7
           rm -rf symfony-app-skeleton
         
           echo 8
           
           echo create .env
           ls -al
           sed -i "s|FORREPLACE123|${{ env.RIPP  }}|g" .env
           sed -i "s|keyphr|${{ secrets.PASS }}|g" .env
          
           sed -i "s|FORREPLACE123|${{ env.RIPP  }}|g" .htaccess
           echo create .htaccess
           echo creating .env and .htaccess is done 
           echo 'FROM alpine:3.7' >> Dockerfile
           echo 'COPY . .' >> Dockerfile
           cat .env
           cat .htaccess
            
         fi
         mkdir jwt && cd jwt
         #HERE IS CREATING NEW openssl KEY TO ADD IS TO PRIVET REPO 
         openssl genpkey -out private.pem -aes256 -pass pass:${{ secrets.PASS }} -algorithm rsa -pkeyopt rsa_keygen_bits:4096 
         openssl pkey -passin pass:${{ secrets.PASS }} -in private.pem -out public.pem -pubout
         echo creating new jwt 
      
         cd ..
         git add .
         
         git commit -a -m "add .env and .htacces and jwt"
         git push 
      - uses: google-github-actions/setup-gcloud@v0.2.0
        with:
          service_account_key: ${{ secrets.GKE_SA_KEY }}
          project_id: ${{ secrets.GKE_PROJECT }}


      - run: |
      
         gcloud --quiet auth configure-docker
         cd ${{ env.RIPP  }}
         ls -al
         #CREATE NEW IMAGE FOR SECRET 
         gcloud builds submit --tag=gcr.io/${{ secrets.GKE_PROJECT }}/${RIPP}sec
         
      - uses: actions/checkout@v2
        with:
         repository: ${{ env.path }}
         token: ${{ secrets.ENVGHTOKEN }}
         ref: dev

      - uses: google-github-actions/setup-gcloud@master
        with:
          service_account_key: ${{ secrets.GCP_SA_KEY }}
          service_account_email: ${{ secrets.service_account_email }}
          project_id: ${{ secrets.GKE_PROJECT }}
          export_default_credentials: true
      - run: |
         gcloud container clusters get-credentials "$GKE_CLUSTER" --zone "$GKE_ZONE"
         #change $ghaccount to org
         #git clone  https://github.com/$GHACCOUNT/$RIPP.git
       
         gcloud builds submit --tag=gcr.io/${{ secrets.GKE_PROJECT }}/${RIPP}
         cd k8s
         kubectl apply -f namespace.yaml
         sed -i "s|123secret123|${{ secrets.ROOTPASSWORD }}|g" secret.yaml
         sed -i "s|123secretuser123|${{ secrets.USERPASSWORD }}|g" secret.yaml
         kubectl apply -f secret.yaml
         kubectl apply -f configmap.yaml
         sed -i "s|REPLACEUserPassword|${{ secrets.USERPASSWORD }}|g" mysql.yaml
         sed -i "s|REPLACERooTPassword|${{ secrets.ROOTPASSWORD }}|g" mysql.yaml
         kubectl apply -f mysql.yaml
         
         kubectl apply -f server.yaml




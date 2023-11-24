## Steps of Github action 
Steps 1 -> need to create a salesforce.crt and salesforce.key file for JWT authentication. 
        -> for getting these file use this command.
        -> openssl req -x509 -sha256 -nodes -days 36500 -newkey rsa:2048 -keyout salesforce.key -out salesforce.crt (Need some details like city,state,country name)
        -> now you have two file which name salesforce.crt and salesforce.key.
Steps 2 -> Create a connected app in your salesforce org.
          ![Screenshot from 2023-11-24 10-55-12](https://github.com/Himanshu987399/GitAction/assets/86918713/bf7c46ff-cfa6-48e7-b57a-712d3ccc76c8)
        -> Enabled **use digital signatures** option and upload salesforce.crt (Step 1)
        -> save it.
        -> Now you need to edit polices.
        ![Screenshot from 2023-11-24 11-08-43](https://github.com/Himanshu987399/GitAction/assets/86918713/21115001-9037-4c3a-a5be-8d583b5d6b68)
        -> now inside connected app you have a option manage profile give system administrator permission to connected app
        ![Screenshot from 2023-11-24 11-10-21](https://github.com/Himanshu987399/GitAction/assets/86918713/6a5ae140-154c-4b72-86a2-d151ea0253a7)
Steps 3 -> Now use this command in your termainal to identify everything is going good (checking JWT Authentication)
        -> for this command you need clientId(customer key),username(salesforce org) and salesforce.key file.
        -> sfdx auth:jwt:grant -u {username} -f {locationOfsalesforce.key} -i {clientId} -r {url maybe login.saleforce.com/test.salesforce.com}.
        -> url is depend on your org type (production,sandbox).
        -> if you recieved **successfully authorized** message means everything going far good (move step 4) otherwise you did something wrong and repeat same process again(move step 1).
Steps 4 -> now we are creating some variable which is used in our yaml
        -> for creating variable you need to go setting of git repository.
        ![Screenshot from 2023-11-24 12-11-36](https://github.com/Himanshu987399/GitAction/assets/86918713/285c94fd-ab9d-44a1-ac49-e2b38df286f8)
        -> now go to secrets and variable -> actions option.
        ![image](https://github.com/Himanshu987399/GitAction/assets/86918713/ceb2a772-71c8-488e-bdb3-b39befc57236)
        -> now go to variable option and create new Repository variable.
        ![Screenshot from 2023-11-24 12-14-31](https://github.com/Himanshu987399/GitAction/assets/86918713/0791595a-d4cd-487d-a66a-7e866d26874b)
        -> Add for variable which name is 
                -> HUB_LOGIN_URL (contains Login Url which may be login.salesforce.com/test.salesforce.com).
                -> SALESFORCE_JWT_SECRET_KEY (contains salesforce.key text include -----BEGIN PRIVATE KEY----- and -----END PRIVATE KEY-----).
                -> SF_CLIENT_ID (contains clientid(customer key) of connected app from salesforce).
                -> SF_USERNAME (contains username of your Org).
        -> **imporatnt Note :- if you are using my sample Yaml file(Steps 5) then you need to use same variable name otherwise you can use different variable name. because these variable is used in YAML file if anything is miss then you will get error on the time of deployemt or runing of git workflow.**        
Steps 5 -> In this step you need to create YAML file 
        -> for that go to Action button in github 
        ![Screenshot from 2023-11-24 11-54-57](https://github.com/Himanshu987399/GitAction/assets/86918713/76ffd1a3-590e-4d79-9b93-b2f3a7de679f)
        -> Now you need to select option (set up a workflow yourself). 
        ![Screenshot from 2023-11-24 11-55-49](https://github.com/Himanshu987399/GitAction/assets/86918713/3436f6b3-bb76-43ce-9cdd-1c2d334e8c7d)
        -> Here you need to write a YAML code (I provide a sample code below for deploy repository to salesforce)
        ![image](https://github.com/Himanshu987399/GitAction/assets/86918713/3a26fc71-d34d-4671-b51f-8fd78db99bec)
        -> sample code 
       {
        name: Salesforce Deployment
        on:
          push:
            branches:
              - main
        jobs:
          deploy: 
            runs-on: ubuntu-latest
            steps:
              - run: echo "🐧 GitHub Action running on ${{ runner.os }}"
              - run: echo "🔎 Retrieving ${{ github.ref }} from ${{ github.repository }}."
              - uses: actions/checkout@v3
              - run: npm install sfdx-cli -g
              - run: echo "${{ vars.SALESFORCE_JWT_SECRET_KEY }}" > server.key
              - run: sfdx auth:jwt:grant -u=${{ vars.SF_USERNAME }}  -f=server.key -i=${{ vars.SF_CLIENT_ID }} -r=${{vars.HUB_LOGIN_URL}}
              - run: sfdx force:source:deploy -p force-app/main/default -u=${{ vars.SF_USERNAME }}
        }
Steps 6 -> Now on the time of push files in main repository your deployment is done.
        ThankYou for reading

**Error May be Occur**
Assumption : I assume you are using my Sample YAML code otherWise these error resolution is not for you.
1) Authentication error with jwt
   -> need to check variable name and data is same as your salesforce.key,clientId,username and loginurl.
   -> Check your connected app callbackurl,profile Access, digital signature file.
2) not Deployed data in salesforce org     
   -> check you have main repoistory or not some time git create master repo instead of main rep.

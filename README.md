**Steps of GitHub Action**
Steps 1:Create a salesforce.crt and salesforce.key file for JWT authentication.
        Use the following command to obtain these files: openssl req -x509 -sha256 -nodes -days 36500 -newkey rsa:2048 -keyout salesforce.key -out salesforce.crt
        Provide details like city, state, and country name.
        You will now have two files named salesforce.crt and salesforce.key.
Steps 2:Create a connected app in your Salesforce org.
        ![Screenshot from 2023-11-24 10-55-12](https://github.com/Himanshu987399/GitAction/assets/86918713/bf7c46ff-cfa6-48e7-b57a-712d3ccc76c8)
        Enable the use digital signatures option and upload salesforce.crt (Step 1).
        Save it.
        Edit policies.
        ![Screenshot from 2023-11-24 11-08-43](https://github.com/Himanshu987399/GitAction/assets/86918713/21115001-9037-4c3a-a5be-8d583b5d6b68)
        Inside the connected app, grant system administrator permission to the connected app.
        ![Screenshot from 2023-11-24 11-10-21](https://github.com/Himanshu987399/GitAction/assets/86918713/6a5ae140-154c-4b72-86a2-d151ea0253a7)
Steps 3:Use this command in your terminal to check if JWT Authentication is working:
        sfdx auth:jwt:grant -u {username} -f {locationOfsalesforce.key} -i {clientId} -r {url maybe login.salesforce.com/test.salesforce.com}
        The URL depends on your org type (production, sandbox).
        If you receive the message successfully authorized, everything is good; move to step 4. Otherwise, review your steps in Step 1.
Steps 4:Create variables used in your YAML file:
        Go to the settings of the Git repository.
        ![Screenshot from 2023-11-24 12-11-36](https://github.com/Himanshu987399/GitAction/assets/86918713/285c94fd-ab9d-44a1-ac49-e2b38df286f8)
        Go to secrets and variables -> actions option.
        ![image](https://github.com/Himanshu987399/GitAction/assets/86918713/ceb2a772-71c8-488e-bdb3-b39befc57236)
        Go to the variable option and create a new Repository variable.
        ![Screenshot from 2023-11-24 12-14-31](https://github.com/Himanshu987399/GitAction/assets/86918713/0791595a-d4cd-487d-a66a-7e866d26874b)
        Add variables named:
                HUB_LOGIN_URL (contains Login URL which may be login.salesforce.com/test.salesforce.com).
                SALESFORCE_JWT_SECRET_KEY (contains salesforce.key text including -----BEGIN PRIVATE KEY----- and -----END PRIVATE KEY-----).
                SF_CLIENT_ID (contains client ID (customer key) of the connected app from Salesforce).
                SF_USERNAME (contains the username of your Org).
**Important Note: If you are using my sample YAML file (Steps 5), use the same variable names. Otherwise, you can use different variable names. These variables are used in the YAML file, and if anything is missing, you will encounter errors during deployment or when running the Git workflow.**
Steps 5:Create a YAML file:
        Go to the Actions button in GitHub.
        ![Screenshot from 2023-11-24 11-54-57](https://github.com/Himanshu987399/GitAction/assets/86918713/76ffd1a3-590e-4d79-9b93-b2f3a7de679f)
        Select the option (set up a workflow yourself).
        ![Screenshot from 2023-11-24 11-55-49](https://github.com/Himanshu987399/GitAction/assets/86918713/3436f6b3-bb76-43ce-9cdd-1c2d334e8c7d)
        Write a YAML code (I provide a sample code below for deploying a repository to Salesforce).
        ![image](https://github.com/Himanshu987399/GitAction/assets/86918713/3a26fc71-d34d-4671-b51f-8fd78db99bec)
        Sample code:
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
        
Steps 6:On pushing files to the main repository, your deployment is complete.

**Thank you for reading.**

**Possible Errors:**
**Assumption**: I assume you are using my Sample YAML code. Otherwise, these error resolutions may not be applicable.

1) Authentication error with JWT:
        Check that variable names and data match your salesforce.key, clientId, username, and loginurl.
        Verify your connected app callback URL, profile access, and digital signature file.
2) Data not deployed in Salesforce org:
        Check if you have the main repository; sometimes Git creates a master repository instead of the main repo.






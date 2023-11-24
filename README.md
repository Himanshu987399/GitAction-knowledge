![Screenshot from 2023-11-24 10-55-12](https://github.com/Himanshu987399/GitAction/assets/86918713/b10ce02c-6047-44a4-bb72-5edcc7bf78a3)# Salesforce DX Project: Next Steps

Now that you’ve created a Salesforce DX project, what’s next? Here are some documentation resources to get you started.

## How Do You Plan to Deploy Your Changes?

Do you want to deploy a set of changes, or create a self-contained application? Choose a [development model](https://developer.salesforce.com/tools/vscode/en/user-guide/development-models).

## Configure Your Salesforce DX Project

The `sfdx-project.json` file contains useful configuration information for your project. See [Salesforce DX Project Configuration](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_config.htm) in the _Salesforce DX Developer Guide_ for details about this file.

## Read All About It

- [Salesforce Extensions Documentation](https://developer.salesforce.com/tools/vscode/)
- [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
- [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
- [Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)


------------------------------------------------------------ Steps of Github action  -------------------------------------------------------------------------------------------------------------------------------------------------------------
Steps 1 -> need to create a salesforce.crt and salesforce.key file for JWT authentication 
        -> for getting these file use this command
            openssl req -x509 -sha256 -nodes -days 36500 -newkey rsa:2048 -keyout salesforce.key -out salesforce.crt
Steps 2 -> Create a connected app in your salesforce org 
          ![Screenshot from 2023-11-24 10-55-12](https://github.com/Himanshu987399/GitAction/assets/86918713/bf7c46ff-cfa6-48e7-b57a-712d3ccc76c8)
        

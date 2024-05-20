# Introduction
If you are a developer and in the tech world, you must have heard about databases. This is the heart of information that organizes your data well-mannered. Its properties like atomicity, consistency, isolation, and durability (ACID) ensure that your transaction is processed accurately.
There are so many database services available in today's world that we can choose any of them or use multiple of them together based on our use cases. 

The challenge is always having our database schema (structural or data changes) that needs to be promoted or synced to different working environments regularly because our needs change over time. This article will show you one of the popular methods to promote your database changes to different environments.

# What do we want to do?
I've made some changes in my database schema in the local environment and now this needs to deploy to other environments like we do with application code and this should be automated.

# How?
In this article, we are using the following tools to demonstrate database publishing:
1. SQL Server 2022
2. Azure DevOps Express
3. Azure Data Studio
   
## Begin the deployment now

### Create a database

### Create a database project
1. You must know the correct credential to connect with your database.
2. Open a Azure Data Studio client (download if you don't have one from here)
3. Add a new connection of your database.
  ![image](https://github.com/rajeesing/StraightToBusiness/assets/7796293/b609082f-86ff-4eac-b8f2-22f08421d1da)

### Azure DevOps Express Installation
Having Azure DevOps access may be costly or time bound which you can use only for some time without paying anything. But the Solution for that is Azure DevOps Express which you can download locally and install on your PC. Please follow the link to get instruction how to download [Azure DevOps Express](https://learn.microsoft.com/en-us/azure/devops/server/download/azuredevopsserver?view=azure-devops). Once you complete your setup successfully, you will get a valid URL to access DevOps platform locally.

#### Create a project
1. Login to your Azure Devops when which you get during installation of Devops Express
2. On home page, click on ![image](https://github.com/rajeesing/StraightToBusiness/assets/7796293/75c233ce-88de-4af1-9abe-b3b67a6ba994) button
3. Enter project name under DefaultCollection (or any of your choice) and hit to Create button.

### CI/CD for On-Prem Database
1. Select project and select "Project settings" which is generally placed bottom of the left navigations.
5. Under Pipelines section, click on Agent Pools.
6. In Agent Pools list page, click on "Add pool" button and create new pool. Leave the other option as default.
7. Create a New agent from selected pool.
   ![image](https://github.com/rajeesing/StraightToBusiness/assets/7796293/e638729e-6b2f-4baa-8a86-074905090d8d)
8. Follow the instructions from popup window.
![image](https://github.com/rajeesing/StraightToBusiness/assets/7796293/2ec1f13b-702f-432b-bf1f-0e0324a7a8bb)
9. If all good, you will notice the item created in Agents list with Online or offline status.
   ![image](https://github.com/rajeesing/StraightToBusiness/assets/7796293/478d3ff1-3d6a-40e9-b99e-c08756021521)




```
parameters:
- name: dbName
  displayName: Database Name
  type: string
  default: 

trigger:
- master

stages:
- stage: BuildProcess
  jobs:
  - job: BuildDACPAC
    displayName: BuildDACPACS
    pool:
     name: OnPremDBVM
     vmImage: windows-latest
    steps:
    - task: DotNetCoreCLI@2
      displayName: Build ADS SQL Database Project
      inputs:
        command: 'build'
        projects: '**/*.sqlproj'
    
- stage: dev
  displayName: Deploy to DEV environment
  dependsOn: BuildProcess
  pool:
    name: OnPremDBVM
  jobs:
   - deployment: DACPACDeployment
     displayName: DACPAC Deployment to DEV
     environment: Development
     strategy:
      runOnce:
       deploy:
         steps:
         - task: PublishBuildArtifacts@1
           inputs:
             PathtoPublish: '$(Build.SourcesDirectory)\<project folder>\bin\debug\<file name>.dacpac' # this path may change based on your location
             artifactName: 'Databases'
             targetPath: '$(Pipeline.Workspace)'
             publishLocation: 'Container'
         - task: SqlDacpacDeploymentOnMachineGroup@0
           inputs:
             TaskType: 'dacpac'
             DacpacFile: '$(Pipeline.Workspace)\Databases\<file name>.dacpac' # please change path based on dacpac location
             TargetMethod: 'connectionString'
             connectionString: 'Data Source=<sql server data source>;Initial Catalog=${{ parameters.dbName }};Encrypt=True;TrustServerCertificate=true;Integrated Security=True'
```



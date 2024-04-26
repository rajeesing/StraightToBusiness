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

### Azure Devops Express Configuration


### CI/CD for On-Prem Database

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



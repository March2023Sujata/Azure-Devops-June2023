## Agile way of Software Development
-------------------------------------
* Agile had added smaller and frequent releases, this needs more aggressive automations than CI.

**Expectation:**
  - Automated Pipeline which when developer pushes changes
  - Build/Package code
  - Code Quality and Security Issues
  - Automate test executions with System, Performance, Reliablity, Security
  - Report of the Quality of work done yesterday
  - Customer and Internal Releases every 2 weeks

<u>------------------------------------------------------------------------------------------</u>
### Quick Overview of Continuous Delivery Pipeline
* Overview
![Preview](images/1.jpeg)

### Building and Packaging the code
* Building the code and packaging the code to the suitable format for end deployment is very technology specific i.e. it is different depending on programming languages.
* Programming Languages can be categorized into 3 formats

    **Compiler based:**

    ![Preview](images/2.jpeg)

    **Interpreter based:**

    **Hybrid:**

    ![Preview](images/3.jpeg)
    ![Preview](images/4.jpeg)
<u>------------------------------------------------------------------------------------------</u>
### Dependecny Managment
* To develop any application , there will be lots of dependencies on other libraries/sdks
* before building/packaging we need to download these dependencies
    * nodejs npm
    * python pip
    * .net nuget
    * java mvn

### Test Executions :
* unit tests (test code by writing code) => developers
* integration tests
    * unit test
    * ui test
    * api test
* Functional tests
    * ui tests (simulate user) => selenium, cypress, qtp…
    * api tests (postman, rest assured)
* Performance tests:
    * load testing harness (jmeter, load runner)
* What we should know for ci/cd
    * command to invoke tests
    * where will be test results
    * converting test results to some common formats (junit xml)

### Java Based Applications:
* To build <u>**Java**</u> Based applications, we have many tools
    - ANT
    - Maven
    - Gradle
* <u>Build steps:</u>
    - mvn package
    - mvn clean install 

### Building and Packaging **Dotnet**
* <u>.net framework versions:</u>
    - .net 2,3,4 (Windows)
    - .net 5 +
        - .net core
        - aspnet core
* <u>Build steps:</u> (require either project or solution  files(.sln/.proj))
    - dotnet restore <sln/proj>
    - dotnet build <sln/prog>
    - dotnet publish <sln/prog>

<u>=====================================================</u>

## Azure DevOps
* Azure DevOps offers services to manage whole project
    * Project Management
        - Planning:
            * Agile Boards
            * Issue Tracker
        - Execution:
            * Wiki Pages
            * Test Management
    * DevOps:
        * VCS: 
            * Azure Source Repos
                - Git
                - TFVC
        *  Pipelines:
            - Build Pipelines
            - Release Pipelines
        *  Artifacts

### Azure DevOps can be used by two ways
* Self-Hosted [Refer here](https://learn.microsoft.com/en-us/azure/devops/server/download/azuredevopsserver?view=azure-devops)
* Cloud-Hosted [Refer here](https://azure.microsoft.com/en-in/products/devops)

### Azure DevOps Services: Cloud Hosted Version of Azure DevOps
   * Pricing: [Refer here](https://azure.microsoft.com/en-in/pricing/details/devops/azure-devops-services/)
   ![Preview](images/5.jpeg)

### <u>Initial setup:-</u>
* Create a git account
![Preview](images/6.jpg)
![Preview](images/7.jpg)
![Preview](images/8.jpg)
![Preview](images/9.jpg)
![Preview](images/10.jpg)
![Preview](images/11.jpg)

### Importing an Existing git repo into Azure DevOps
* Import Repository from github into your account

![Preview](images/12.jpg)
![Preview](images/13.jpg)
![Preview](images/14.jpg)
![Preview](images/15.jpg)
![Preview](images/16.jpg)

### Azure DevOps Pipeline
* Azure DevOps Pipelines are expressed in yaml formats in git repositories generally with name azure-pipelines.yaml

![Preview](images/17.jpg)
* YAML Schema for azure devops pipelines   - [Refer Here](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/?view=azure-pipelines)
 * [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/pipeline?view=azure-pipelines)

* Key Concepts of Azure DevOps [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops)
![Preview](images/18.jpg)

### Pipeline:
    - Where should it execute? => Agents
    - When should it run => Trigger
    - What should happend when pipeline executes
        * Stages
        * Jobs
        * Steps
* When pipeline is executed it is execute with code from version control already cloned and in the branch specified
```
---
trigger:
  - master

pool: ubuntu-latest

stages:
  - stage: stage1
    displayName: first stage

    jobs: 
      - job: build code
        displayName: Build Code
        steps:
          - task: Maven@4
            inputs:
              mavenPOMFile: 'pom.xml'
              goal: package
```
* If your pipeline has only one stage, consider pipeline is collection of jobs
* Lets try to write the same pipeline above as collection of jobs as we have only one stage
```
---
name: learning
trigger:
  - master
pool: ubuntu-latest
jobs:
  - job: buildjob
    displayName: Build JOB
    steps:
      - task: Maven@4
        inputs:
          mavenPOMFile: 'pom.xml'
          goal: 'package'
```
* Lets write the pipeline as collection of steps
```
---
name: learning
trigger:
  - master
pool: ubuntu-latest
steps:
  - task: Maven@4
    inputs:
      mavenPOMFile: 'pom.xml
```

### Agents in Azure DevOps Pipeline

* Azure DevOps Pipelines have two types of Agents:
  - **Microsoft hosted Agent** [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml)

![Preview](images/19.jpg)

  - Size is always fixed Standard_D2S i.e. 2 vcpu's 8 GB RAM

  - <u>**When to use:** </u>
      -  Build/Deploy uses standard tools/softwares and if the configuration required matches the above statement
      - No/Little configuration is what you like in CI/CD pipelines for executions

  - **Self Hosted Agents**[Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/linux-agent?view=azure-devops)
  ![Preview](images/20.jpg)
    - You need to configure agent to connect to azure devops(machine can be in any cloud azure,aws,gcp etc.)

* Azure DevOps Pipleines can be summarized as shown below
![Preview](images/21.jpg)
  - Till artifacts,it wiil be build pipeline.
  - After artifacts,release pipeline for deployment

  
### <u>**Configuring Self Hosted Agent**</u>
* Setting up agent to build jdk 17 and maven based softwares
  - Create a linux vm(Size = Standard_B2s)
![Preview](images/22.jpg)
  - install jdk 17 and maven
    ```
    sudo apt update 
    sudo apt install openjdk-17-jdk maven -y
    ```
### Now navigate to project settings and agent pools
![Preview](images/33.jpg)
![Preview](images/34.jpg)
![Preview](images/35.jpg)
![Preview](images/36.jpg)
![Preview](images/37.jpg)
![Preview](images/38.jpg)
* Create Personal Access Token
![Preview](images/39.jpg)
![Preview](images/40.jpg)
![Preview](images/41.jpg)
* Now come back to Add Agent
![Preview](images/42.jpg)
click on copy and go to the VM
```
wget <paste-it-here>
```
![Preview](images/43.jpg)
Now follow the stepes that are given 
![Preview](images/44.jpg)
```
mkdir myagent && cd myagent
tar zxvf ~/vsts-agent-linux-x64-3.220.5.tar.gz
./config.sh
./run.sh
```
![Preview](images/45.jpg)
![Preview](images/46.jpg)
![Preview](images/47.jpg)
![Preview](images/48.jpg)
![Preview](images/49.jpg)

### Import a repository in Azure-Devops using CLI based
  * Steps:
     - Create a folder in local machine
![Preview](images/23.jpg)
     - Clone the code
![Preview](images/24.jpg)
     - Now, in azure-devops
![Preview](images/25.jpg)  
![Preview](images/26.jpg)
![Preview](images/27.jpg)
![Preview](images/28.jpg)
![Preview](images/29.jpg)
Now copy id_rsa.pub key from local machine
![Preview](images/30.jpg)
Copy SSH url
![Preview](images/31.jpg)
![Preview](images/32.jpg)

* Now lets try to create a simple azure devops build pipeline
```
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
- main

pool:
  name: Default
steps:
  - bash: printenv
```
![Preview](images/50.jpg)
![Preview](images/51.jpg)
![Preview](images/52.jpg)
![Preview](images/53.jpg)
![Preview](images/54.jpg)
![Preview](images/55.jpg)
![Preview](images/56.jpg)

<u>------------------------------------------------------------------------------------------</u>

#### Building Project in Azure DevOps using Self Hosted Agent

* Azure Pipeline yaml schema [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/?view=azure-pipelines)

* **<u>Task</u>** in Azure DevOps Pipelines: Tasks internally get converted in low level os commands/api calls.
* Azure DevOps has lot of predefined tasks [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/?view=azure-pipelines)

![Preview](images/58.jpg)
![Preview](images/59.jpg)

* We can also get additional tasks from Market as a Extensions [Refer Here](https://marketplace.visualstudio.com/azuredevops)

![Preview](images/60.jpg)

* Lets achieve building the code without using any task.

* <u>**Java Project**</u>
  -  Manual command is 'mvn package'
    - install Java 8
    - install Maven (Game of life)
```
trigger:
- master

pool:
  name: Default

steps:
  - bash: mvn package
```
* Push the above changes to azure repo and then and build will start (trigger)

![Preview](images/61.jpg)
![Preview](images/62.jpg)

```
trigger:
- master

pool:
  name: Default

steps:
  - bash: mvn package
    displayName: "Build package using Maven"
```
![Preview](images/63.jpg)

### Lets try using tasks for maven [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/maven-v4?view=azure-pipelines)
![Preview](images/64.jpg)
```
trigger:
- master

pool:
  name: Default

steps:
  - task: Maven@3
    inputs:
      mavenPomFile: 'pom.xml'
      goals: 'package'
      publishJUnitResults: true
      testResultsFiles: '**/surefire-reports/TEST-*.xml'
      testRunTitle: 'RunUnitTests'
```
![Preview](images/65.jpg)
![Preview](images/66.jpg)
![Preview](images/67.jpg)
<u>-----------------------------------------------------------------------------------------------</u>
* <u>**Dotnet Project**</u>
* Manual steps:
    - Install .net 7
```
git clone https://github.com/nopSolutions/nopCommerce.git
cd nopCommerce
git checkout master
dotnet restore src/NopCommerce.sln
dotnet build src/NopCommerce.sln
```
* Find tasks to perform restore and build and fill with the values required to build this project google **'azure devops task list'**
```
trigger:
- master

pool:
  name: Default

steps:
  - task: DotNetCoreCLI@2
    inputs:
      command: 'restore'
      projects: 'src/NopCommerce.sln'
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: 'src/NopCommerce.sln'
      publishTestResults: true
```
![Preview](images/68.jpg)
![Preview](images/69.jpg)
![Preview](images/70.jpg)
![Preview](images/71.jpg)
![Preview](images/72.jpg)
![Preview](images/73.jpg)
![Preview](images/74.jpg)
![Preview](images/75.jpg)

### Microsoft Hosted Agent

* The list of microsoft hosted agents [Refer Here](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#software)

![Preview](images/19.jpg)
* **Setting up Microsoft hosted agent**
(watch class video)
![Preview](images/76.jpg)
![Preview](images/77.jpg)
![Preview](images/78.jpg)
![Preview](images/79.jpg)
![Preview](images/80.jpg)
![Preview](images/81.jpg)
![Preview](images/82.jpg)
![Preview](images/83.jpg)
![Preview](images/84.jpg)
![Preview](images/85.jpg)
![Preview](images/86.jpg)

### Azure DevOps pipeline Variables
--------------------------------------

* Define variables [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#user-defined-variables)
    - System variables
        - YAML or classic Build pipelines predefined variables [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml)
        - classic Release pipelines [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/variables?view=azure-devops&tabs=batch)

* Using **<u>CopyFiles@2</u>** - Copy files v2 task
![Preview](images/87.jpg)
![Preview](images/88.jpg)
* This will help in deploying 
![Preview](images/89.jpg)
![Preview](images/90.jpg)

* Using **<u>PublishBuildArtifacts@1</u>** - Publish build artifacts v1 task
![Preview](images/91.jpg)
![Preview](images/92.jpg)
![Preview](images/93.jpg)
![Preview](images/94.jpg)
* we can copy build artifacts to other repos also like JFrog etc
![Preview](images/95.jpg)

### Spring-Pet-clinic
![Preview](images/118.jpg)
![Preview](images/119.jpg)
![Preview](images/120.jpg)
![Preview](images/121.jpg)

### Dotnet project
* Self Hosted Agent
![Preview](images/96.jpg)

* Microsoft Hosted Agent
![Preview](images/97.jpg)
![Preview](images/98.jpg)
![Preview](images/99.jpg)
### Variables in Azure-Devops
-----------------------------

* Azure devops variables [Refer Here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch)
  - System variables(predefined variables) [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml)
  - User defined variables [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#user-defined-variables)

### <u>**Define variables:**</u>
----------------------------------
  -  Azure DevOps Pipeline has variables, we can assign values to be used within pipeline across stages, jobs or steps.
  - The most common use of variables is to define a value that you can then use in your pipeline. All variables are strings and are mutable. The value of a variable can change from run to run or job to job of your pipeline. 
  - When you define the same variable in multiple places with the same name, the most locally scoped variable wins. So, a variable defined at the job level can override a variable set at the stage level. A variable defined at the stage level overrides a variable set at the pipeline root level. 
* Variables values can be changed during the pipeline execution
* Pipeline execution has 3 phases
    - Queue time phase (till agent allocated)
    - Compile phase (before runtime chk synatx whether yaml is right or not)
    - Runtime phase (during execution)
* **<u>User-defined variables**</u>
    * Variables can be used by 3 different expressions [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#understand-variable-syntax)
      - Macro $(var)
          - Macro syntax variables ($(var)) get processed during runtime before a task runs. 
      - Template ${{ variables.var }}
          - In a pipeline, template expression variables (${{ variables.var }}) get processed at compile time, before runtime starts.
      - Runtime expression $[variables.var]
          - Runtime expressions ($[variables.var]) also get processed during runtime but are intended to be used with conditions and expressions. 

![Preview](images/100.jpg)

### Set variables in pipeline [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch#what-syntax-should-i-use)
### Set variables in scripts [Refer Here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/set-variables-scripts?view=azure-devops&tabs=bash)
  - eg: - bash: |
    echo "##vso[task.setvariable variable=myVar;]foo"
    * vso: visual studio object(from VSTS)
    * myvar is variable
    * foo is a value

* Variables can be created at
--------------------------------
    - pipeline level
    - stages level
    - jobs level 

* variable in Game of life
-----------------------------
![Preview](images/101.jpg)

* Variables at different pipeline's level
------------------------------------------
![Preview](images/102.jpg)
![Preview](images/103.jpg)
![Preview](images/104.jpg)
![Preview](images/105.jpg)

* Change the value of the variable
-------------------------------------
![Preview](images/106.jpg)
![Preview](images/107.jpg)

### **<u>Variable group</u>**
* Suppose we have **multiple variables** and to use them in multiple pipelines,these values can be secrets or text values, in these kind of cases we use variable groups. To create a variable group [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops&tabs=yaml)

* Lets create a variable group
![Preview](images/108.jpg)
![Preview](images/109.jpg)
![Preview](images/110.jpg)
![Preview](images/111.jpg)
![Preview](images/112.jpg)
![Preview](images/113.jpg)
![Preview](images/114.jpg)
![Preview](images/115.jpg)
![Preview](images/116.jpg)
![Preview](images/117.jpg)

### **<u>Making Pipelines resuable using templates</u>**
* Template usage reference [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops&pivots=templates-includes)
* Template parameters [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/template-parameters?view=azure-devops)

#### A reusable template with a name **"Java-Maven-Template.yaml"**
![Preview](images/122.jpg)

### **Spring-pet-clinic:-** 
![Preview](images/123.jpg)

### **Game-of-life:-** 
![Preview](images/124.jpg)

### Now store the template file into a separete repo
![Preview](images/125.jpg)
![Preview](images/126.jpg)
![Preview](images/127.jpg)
![Preview](images/128.jpg)

* Now lets try to reuse the template which is in different git repo with the help of Azure-devops pipeline resources [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops&pivots=templates-includes#use-other-repositories).
* Azure DevOps Pipline resources [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/resources?view=azure-devops&tabs=schema)
* Define a repositories resource [Refer here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/resources?view=azure-devops&tabs=schema#define-a-repositories-resource)


![Preview](images/133.jpg)
![Preview](images/130.jpg)
![Preview](images/131.jpg)
![Preview](images/132.jpg)
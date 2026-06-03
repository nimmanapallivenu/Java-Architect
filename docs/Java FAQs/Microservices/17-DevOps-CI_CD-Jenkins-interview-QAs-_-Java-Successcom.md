# 17+ DevOps CI CD Jenkins interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is Jenkins?](#q1)
- [Q2: What are the pre-requisites for getting started with Jenkins?](#q2)
- [Q3: Can you describe the Jenkins architecture?](#q3)
- [Q4: What is a Jenkins pipeline?](#q4)
- [Q5: What is a node block in Jenkins pipeline?](#q5)
- [Q6: What is a stage block in Jenkins pipeline?](#q6)
- [Q7: How do you change directory in Jenkins pipeline script say before executing the ](#q7)
- [Q8: How do you know what predefined actions like “sh”, “bat”, “mvn”, etc you can use](#q8)
- [Q9: How do you save a set of files for later use in the same build on another node?](#q9)
- [Q10: How does Jenkins limits the execution of any Groovy script?](#q10)
- [Q11: What is a post section in a Jenkinsfile?](#q11)
- [Q12: How will you print all the environment variables?](#q12)
- [Q13: How do you debug Jenkins?](#q13)
- [Q14: How do you capture user inputs in Jenkins?](#q14)
- [Q15: What does the following Jenkins pipeline code script do? catch (org.jenkinsci .p](#q15)
- [Q16: What does the following Jenkins pipeline code script do?](#q16)
- [Q17: How will Jenkins with cloud technologies like say A WS (i.e. Amazon W eb Service](#q17)
- [Q18: How does CI/CD help with Microservices architecture?](#q18)

---

## Q1: What is Jenkins?

**Answer:**

Jenkins is an open source CI/CD (i.e. Continuous Integration & Continuous Delivery/Deployment) server written in Java with over 2000 plugins (E.g.
Maven, Git, Amazon EC2, Docker , Ansible, etc) for various development, testing and deployment tasks. Refer to Jenkins Plugin Index for the plugins and the
syntaxes. Also, on your Jenkins W eb UI, within your pipeline script page, click on “ Pipeline Syntax ” link to generate pipeline sentences for predefined
actions.
CI is process where every code committed to the code repository like GIT is built and tested, but still is not in a condition to be released. If any unit test cases or
build are broken the team members will be notified to promptly rectify the problem.
CD in which Continuous Delivery is the process where the application is continuously deployed on the test servers for UA T. Continuous Deployment is the next
step past Continuous Delivery , where you are not only creating a deployable artefacts, but also deploying it in an automated fashion.

---

## Q2: What are the pre-requisites for getting started with Jenkins?

**Answer:**

To use Jenkins you require
#1. A source code repository . e.g. Git, BitBucket, Subversion, etc where you can check in your source code.
#2. A working build script (e.g. Maven pom.xml file, Makefile, Jenkinsfile, etc) checked into the source code repository .
Jenkins can trigger execution of the dif ferent stages like “ checkout ” the code from the code repository , “run unit tests ” using the build script like “mvn test”,
and “ build ” artefacts like jar , war or ear using a build script like “mvn package”.

---

## Q3: Can you describe the Jenkins architecture?

**Answer:**

Jenkins architecture is fundamentally “Master+Agent”. The master is designed to do co-ordination and provide the GUI and API endpoints, and the Agents
are designed to perform the work.
#1. Jenkins Server (i.e. a W eb UI, via a war file) – Master: schedules & dispatches build jobs to the agents for the actual execution, monitors the agents, records
& presents the build results. A master instance of Jenkins can also execute build jobs directly .
#2. Jenkins Node Agent: involves executing the build jobs dispatched by the master . It can be configured for a project to always run on a particular agent/node
machine or let Jenkins pick the next available agent/node.

---

## Q4: What is a Jenkins pipeline?

**Answer:**

The Jenkins Pipeline is a plugin for writing custom Domain Specific Language (DSL) in Groovy , which is incredibly powerful to develop complex, multi-
step DevOps pipelines.
Pipelines are implemented in code (E.g. Jenkinsfile ) and typically checked into the source code repository , giving teams the ability to edit and review .

Pipelines can not only optionally stop and wait for human input or approval before continuing, but also support complex real-world continuous delivery
requirements, including the ability to fork/join, loop, and perform work in parallel.
The Multibranch Pipeline project type extends Pipelines project for dif ferent branches (E.g. master , develop, feature/abc, pull request #75, etc) of the same
project.

---

## Q5: What is a node block in Jenkins pipeline?

**Answer:**

A “node” is part of the Jenkins distributed mode architecture, where the workload can be delegated to multiple “agent” nodes. The node block is not
mandatory , but it is a good practice as Jenkins will schedule and run all the steps once any node is available and creates a specific workspace directory .

---

## Q6: What is a stage block in Jenkins pipeline?

**Answer:**

Several steps in the pipline can be grouped into stages . For example stages to checkout code from GIT code repository , run unit tests via “mvn test”, build
project via “mvn install”, deploy your application to A WS EMR via Ansible plugin, run functional or performance tests, etc.
A scripted pipeline script
Scripted pipeline is a fully featured programming environment that of fers lots of flexibility and extensibility to Jenkins users.node {
}
#!/usr/bin/env groovy
/discard old builds
properties ([[$class : 'BuildDiscarderProperty' , strategy : [$class : 'LogRotator' , artifactDaysT oKeepStr : '', artifactNumT oKeepStr : '', daysT oKeepStr : '',
numT oKeepStr : '10']]]);
/run all the steps once any node is available 
node {

A declarative pipeline script
Declarative Pipeline was created to of fer a simpler and more opinionated syntax for writing Jenkins Pipelines. //global variables
 BUILD_VERSION = "0.0.1"
 ECR_URI = "99996765432.xyz.ecr .ap-southeast-2.amazonaws.com"
 DOCKER_REPO = "my-ecr -repo/my-java-maven-image"
 DOCKER_BASE_T AG = "simple-app"
 def APP_VERSION
 stage ('Clean workspace' ) {
 sh "echo ${BRANCH_NAME}" // a step
 cleanWs () // a step
 }
 stage ('Checkout' ) {
 checkout scm // a step
 sh "echo ${BUILD_VERSION}" -`git rev-parse --short HEAD ` > .version " // a step
 APP_VERSION = readFile('.version').trim() // a step
 }
 stage('Pull docker build container from A WS ECR') {
 sh " docker pull ${ECR_URI }/${DOCKER_REPO }:${DOCKER_BASE_T AG}" // a step
 }
 //Makefile in the project code repository executes " mvn tests" within a Docker container
 stage('Run tests in build container') {
 def ST AGE_V AR_PULL_REQUEST_ID = env .CHANGE_ID?: ''
 sh " DOCKER_IMAGE =${DOCKER_REPO }:${DOCKER_BASE_T AG} CHANGE_ID =${STAGE_V AR_PULL_REQUEST_ID } make run-unit-tests-on-
enkins "
 }
 //Makefile in the project code repository executes " mvn install " within a Docker container
 stage('Build artifact for deploy') {
 sh " DOCKER_IMAGE =${DOCKER_REPO }:${DOCKER_BASE_T AG} make build -app-on0jenkins "
 }

“Pipeline” defines the block that contains all the script contents. “Agent” defines where the pipeline will be run, and synonymous to the “node” for the scripted
one. “Stages” contains all of the stages.

---

## Q7: How do you change directory in Jenkins pipeline script say before executing the “automation tests”?

**Answer:**

Use the “ dir() ”pipeline {
agent any
stages {
 stage (‘Build ’) {
 steps {
 //…
 }
 }
 stage (‘Test’) {
 steps {
 //…
 }
 }
}
stage ('automation tests' ) {
 dir(AUT O_TESTS_P ATH) { //Changes the current directory to the value set on the AUT O_TESTS_P ATH variable.
 sh "mvn clean test -Dsuite=SMOKE_FUNCTIONAL_TEST -Denvironment=${ENV}"
 } 
}

---

## Q8: How do you know what predefined actions like “sh”, “bat”, “mvn”, etc you can use in your pipeline script?

**Answer:**

On your Jenkins W eb UI, within your pipeline script page, click on “ Pipeline Syntax ” interface to generate pipeline sentences for predefined actions.
These generated actions can be added to any of the script stages.

---

## Q9: How do you save a set of files for later use in the same build on another node?

**Answer:**

You can use stash & unstash . The stash and unstash steps are designed for use with small files. For lar ge data transfers, use the External W orkspace
Manager plugin, or use an external repository manager such as Nexus or Artefactory .
#!/usr/bin/env groovy
properties (
 [buildDiscarder (logRotator (artifactDaysT oKeepStr : '', artifactNumT oKeepStr : '', daysT oKeepStr : '1', numT oKeepStr : '50')), [$class : 'RebuildSettings' ,
autoRebuild : false , rebuildDisabled : false ], pipelineT riggers ([])]
node {
APP_TYPE = "my-app"
stage ('Clean workspace' ) {
 cleanWs ()
}
stage ('Checkout' ) {
 checkout scm
 // Get commit ID
 sh "git rev-parse HEAD | tail -c 8 > var_short_commit_id"
 // Stash vars
 stash includes : "var_*" , name : "var-stash"
}
unstash "var-stash"
def short_commit_id = readFile ('var_short_commit_id' ).trim()
stage ('Build Docker container' ) {
 def app = docker .build ("${APP_TYPE}:${short_commit_id}" )
}

---

## Q10: How does Jenkins limits the execution of any Groovy script?

**Answer:**

Jenkins limits the execution of any Groovy script by providing an option named “ Use Gr oovy Sandbox ” in the W eb UI. When this option is checked it
allows the scripts to be run by any user without requiring administrator privileges. When unchecked, if the script has operations that require approval, an
administrator will have to approve it. This method is known as “Script approval”. By default, all Jenkins pipelines run in a Groovy sandbox as it is impractical
to wait for an administrator to approve every change to a script, no matter how trivial the change is.

---

## Q11: What is a post section in a Jenkinsfile?

**Answer:**

The post section defines one or more additional steps that are run on completion of a Pipeline block or a stage block depending on the location of the post
section within the Pipeline.

---

## Q12: How will you print all the environment variables?

**Answer:**

For the declarative pipeline:
or
For the scripted pipeline:sh 'printenv'
node {
 sh 'env > env .txt'
 readFile ('env.txt').split("\r?\n" ).each {
 println it
 }
}

---

## Q13: How do you debug Jenkins?

**Answer:**

#1: Look at the Jenkins master UI console logs for errors.
#2: Adding a few print statements here and there, and rerun the job for more console logs.
#3: When you are running tasks within say Docker containers, add “input” prompt into your Jenkinsfile in the place you want to investigate. This way you can
pause the execution of the pipeline, and ssh into the container and investigate further .echo sh(script : 'env|sort' , returnStdout : true)
echo userInput
#!/usr/bin/env groovy
/discard old builds
properties ([[$class : 'BuildDiscarderProperty' , strategy : [$class : 'LogRotator' , artifactDaysT oKeepStr : '', artifactNumT oKeepStr : '', daysT oKeepStr : '',
numT oKeepStr : '10']]]);
/run all the steps once any node is available 
node {
 //global variables
 BUILD_VERSION = "0.0.1"

#4: Using the Jenkins unit testing framework – JenkinsPipelineUnit . This testing framework lets you write unit tests on the configuration and conditional logic
of the pipeline code, by providing a mock execution of the pipeline. Y ou can mock built-in Jenkins commands, job configurations, see the stacktrace of the
whole execution and even track regressions.

---

## Q14: How do you capture user inputs in Jenkins?

**Answer:**

Here is an example to prompt user for a file name. There could be other examples to get user inputs like “passwords”, etc.
“input” action to pause for user input def APP_VERSION
 stage ('Clean workspace' ) {
 sh "echo ${BRANCH_NAME}" // a step
 cleanWs () // a step
 }
 input 'stop'
 stage ('Checkout' ) {
 checkout scm // a step
 sh "echo ${BUILD_VERSION}" -`git rev-parse --short HEAD ` > .version " // a step
 APP_VERSION = readFile ('.version' ).trim() // a step
 }
def FILE_NAME = ''
ry {
 timeout (time: timeoutMillis , unit: 'MILLISECONDS' ) {
 FILE_NAME = input (id: 'Proceed1' , message : 'Please enter the FILE_NAME to run' , parameters : [
 [$class : 'TextParameterDefinition' , defaultV alue: 'UpgradeEligibility' , description : 'Select FILE_NAME for Running integration tests' , name :
FILE_NAME' ]
 ])
 }
 sh "echo '${FILE_NAME}' >> FILE_NAME.txt"
 stash includes : "FILE_NAME.txt" , name : "FILE_NAME"

“Build with parameters” to gather user inputs

---

## Q15: What does the following Jenkins pipeline code script do? catch (org.jenkinsci .plugins .workflow .steps .FlowInterruptedException e) { // timeout reached or input false
 cause = e.causes .get(0)
#!/usr/bin/env groovy
properties ([
 [$class : 'RebuildSettings' , autoRebuild : false , rebuildDisabled : false ],
 parameters ([
 choice (defaultV alue: 'dev', choices : '\ndev\nsit\nprd' , description : 'Which environment to run?' , name : 'ENVIRONMENT' ),
 string (defaultV alue: '1', description : 'Number of executors to run' , name : 'NO_OF_EXECUT ORS' ),
 string (defaultV alue: 'today' , description : 'Date to run for?' , name : 'RUN_DA TE')
 ]),
 pipelineT riggers ([])
)
node {
 //...........
APP_TYPE = "my_app"
ECR_ID = 99999876
tage('Build & Push to Registry' ) {

**Answer:**

It builds, tags and pushes a docker image to Amazon ECR, which is a managed A WS Docker registry service.

---

## Q16: What does the following Jenkins pipeline code script do?

**Answer:**

It executes an Ansible playbook “deploy-my-app.yml”.
A docker image containing layers of Debian Linux, Java, and Maven could be generating sample input files for functional & performance testing. In Q15
Jenkinsfile builds, tags, and publishes this Docker image to the Amazon ECR. The above code snippet in the Jenkinsfile invokes an Ansible script, which could
be
1) creating an Amazon ECS cluster . sh "git rev-parse HEAD | tail -c 8 > var_short_commit_id"
 def short_commit_id = readFile ('var_short_commit-id' ).trim()
 def app = docker .build ("${APP_TYPE}:${short_commit_id}" )
 sh "docker tag ${APP_TYPE}:${short_commit_id} ${ECR_ID}.dkr .ecr.ap-southeast-2.amazonaws.com/my-ecr -repo/${APP_TYPE}:${short_commit_id}"
 sh "docker push ${ECR_ID}.dkr .ecr.ap-southeast-2.amazonaws.com/my-ecr -repo/${APP_TYPE}:${short_commit_id}"
f ("${BRANCH_NAME}" == "develop" ) {
// Deploy to Dev environment
stage ('Release to DEV' ) {
 def env = "dev"
 def aws_account = "aws-account-dev"
 sshagent (credentials : ['ansible-jenkins-keypair' ]) {
 sh "ssh ansible-user@ansible-server .com" +
 "-o StrictHostKeyChecking=no " +
 "'cd /etc/ansible; ansible-playbook deploy-my-app.yml " +
 "-i inventories/aws " +
 "-e \"hosts=${aws_account} env=${env} \"' "
 }
}

2) creating an ECS task definition from the previously published image on ECR.
3) running the task definition on the ECS cluster to generate the sample input files.

---

## Q17: How will Jenkins with cloud technologies like say A WS (i.e. Amazon W eb Services)

**Answer:**

Using the relevant cloud technology Pipeline plugin. For example, for A WS use the Pipeline A WS Plugin as described in Jenkins Plugin Index .
For example: Download a file from A WS s3 bucket in Jenkins using the “Pipeline: A WS Steps”.
#!/usr/bin/env groovy
properties ([
 [$class : 'RebuildSettings' , autoRebuild : false , rebuildDisabled : false ],
 parameters ([
 choice (defaultV alue: 'dev', choices : '\ndev\nsit\nprd' , description : 'Which environment?' , name : 'ENV' ),
 ]),
 pipelineT riggers ([])
)
node {
 APP_TYPE = "my-app"
 def awsAccountIds = [dev: "9999991234" , sit: "88888881234" , prd: "7777771234" ]
 stage ('Checkout' ) {
 checkout scm
 if ("${BRANCH_NAME}" == "master" ) {
 withA WS(role: "role-assumed-jenkins" , roleAccount : accountIds .get(params .ENV ), region :"ap-southeast-2" , roleSessionName :
MY_MGMT_JENKINS_SESSION" ) {
 s3Download (file:'file_that_stores_the_latest_app_version.txt' , bucket :"releases-bucket-${params.Environment}" , path:"myapp/latest" )
 }
 def APP_VERSION_T O_DEPLOY = readFile ('file_that_stores_the_latest_app_version.txt' ).trim()
 }
 //....other stages that make use of

---

## Q18: How does CI/CD help with Microservices architecture?

**Answer:**

DevOps practices, such as CI/CD are used to drive microservice deployments . DevOps teams focusing on individual pieces of functionality in
microservices and build lar ger systems by composing the micro services. CI/CD pipelines will increase the team velocity .
Microservices play well with cloud-based application architectures by allowing software development teams to take advantage of several patterns such as event-
driven programming and autoscale scenarios. The microservice components expose APIs typically over REST pr otocols for communicating with other
services.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03

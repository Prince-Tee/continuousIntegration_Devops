# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP


IMPORTANT NOTICE – This project has some initial theoretical concepts that must be well understood before moving on to the practical part. Please read it carefully as many times as needed to completely digest the most important and fundamental DevOps concepts. To successfully implement this project, it is crucial to grasp the importance of the entire CI/CD process, roles of each tool and success metrics – so we encourage you to thoroughly study the following theory until you feel comfortable explaining all the concepts (e.g., to your new junior colleague or during a job interview).

In previous projects, you have been deploying a tooling website directly into the var/www/html directory on dev servers. Well, even though that worked fine, and we were able to access the website, it is not the best way to do it. Real world web application code written on Java, .NET or other compiled programming languages require a build stage to create an executable file. The executable file (e.g., jar file in case of Java) contains all the codes embedded, and the necessary library dependencies, which the application needs to run and work successfully.

Some other programs written languages such as PHP, JavaScript or Python work directly without being built into an executable file – these languages are called interpreted. That is why we could easily deploy the entire code from git into var/www/html and immediately the webserver was able to render the pages in a browser. However, it is not optimal to download code directly from Git onto our servers. There is a smarter way to package the entire application code, and track release versions. We can package the entire code and all its dependencies into an archive such as .tar.gz or .zip, so that it can be easily unpacked on a respective environment’s servers.

For a better understanding of the difference between compiled vs interpreted programming languages – read this short article. ( https://www.freecodecamp.org/news/compiled-versus-interpreted-languages/ )

In this project, you will understand and get hands on experience around the entire concept around CI/CD from applications perspective. To fully gain real expertise around this idea, it is best to see it in action across different programming languages and from the platform perspective too. From the application perspective, we will be focusing on PHP here; there are more projects ahead that are based on Java, Node.js, .Net and Python. By the time you start working on Terraform, Docker and Kubernetes projects, you will get to see the platform perspective of CI/CD in action.

What is Continuous Integration?
In software engineering, Continuous Integration (CI) is a practice of merging all developers’ working copies to a shared mainline (e.g., Git Repository or some other version control system) several times per day. Frequent merges reduce chances of any conflicts in code and allow to run tests more often to avoid massive rework if something goes wrong. This principle can be formulated as Commit early, push often.

The general idea behind multiple commits is to avoid what is generally considered as Merge Hell or Integration hell. When a new developer joins a new project, he or she must create a copy of the main codebase by starting a new feature branch from the mainline to develop his own features (in some organization or team, this could be called a develop, main or master branch). If there are tens of developers working on the same project, they will all have their own branches created from mainline at different points in time. Once they make a copy of the repository it starts drifting away from the mainline with every new merge of other developers’ codes. If this lingers on for a very long time without reconciling the code, then this will cause a lot of code conflict or Merge Hell, as rightly said. Imagine such a hell from tens of developers or worse, hundreds. So, the best thing to do, is to continuously commit & push your code to the mainline. As many times as tens times per day. With this practice, you can avoid Merge Hell or Integration hell.

CI concept is not only about committing your code There is a general workflow, let us start it …

Run tests locally: Before developers commit their code to a central repository, it is recommended to test the code locally. So, Test-Driven Development (TDD) approach is commonly used in combination with CI. Developers write tests for their code called unit-tests, and before they commit their work, they run their tests locally. This practice helps a team to avoid having one developer’s work-in-progress code from breaking other developers’ copy of the codebase.

Compile code in CI : After testing codes locally, developers commit and push their work to a central repository. Rather than building the code into an executable locally, a dedicated CI server picks up the code and runs the build there. In this project we will use, already familiar to you, Jenkins as our CI server. Build happens either periodically – by polling the repository at some configured schedule, or after every commit. Having a CI server where builds run is a good practice for a team, as everyone has visibility into each commit and its corresponding builds.

Run further tests in CI: Even though tests have been run locally by developers, it is important to run the unit-tests on the CI server as well. But, rather than focusing solely on unit-tests, there are other kinds of tests and code analysis that can be run using CI server. These are extremely critical to determining the overall quality of code being developed, how it interacts with other developers’ work, and how vulnerable it is to attacks. A CI server can use different tools for Static Code Analysis, Code Coverage Analysis, Code smells Analysis, and Compliance Analysis. In addition, it can run other types of tests such as Integration and Penetration tests. Other tasks performed by a CI server include production of code documentation from the source code and facilitate manual quality assurance (QA) testing processes.

Deploy an artifact from CI: At this stage, the difference between CI and CD is spelt out. As you now know, CI is Continuous Integration, which is everything we have been discussing so far. CD on the other hand is Continuous Delivery which ensures that software checked into the mainline is always ready to be deployed to users. The deployment here is manually triggered after certain QA tasks are passed successfully. There is another CD known as Continuous Deployment which is also about deploying the software to the users, but rather than manual, it makes the entire process fully automated. Thus, Continuous Deployment is just one step ahead in automation than Continuous Delivery.

Continuous Integration in The Real World
To emphasize a typical CI Pipeline further, let us explore the diagram below a little deeper.

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/first%20sceenshot.png)

Version Control: This is the stage where developers' code gets committed and pushed after they have tested their work locally.

Build: Depending on the type of language or technology used, we may need to build the codes into binary executable files (in case of compiled languages) or just package the codes together with all necessary dependencies into a deployable package (in case of interpreted languages).

Unit Test: Unit tests that have been developed by the developers are tested. Depending on how the CI job is configured, the entire pipeline may fail if part of the tests fails, and developers will have to fix this failure before starting the pipeline again. A Job by the way, is a phase in the pipeline. Unit Test is a phase, therefore it can be considered a job on its own.

Deploy: Once the tests are passed, the next phase is to deploy the compiled or packaged code into an artifact repository. This is where all the various versions of code including the latest will be stored. The CI tool will have to pick up the code from this location to proceed with the remaining parts of the pipeline.

Auto Test: Apart from Unit testing, there are many other kinds of tests that are required to analyse the quality of code and determine how vulnerable the software will be to external or internal attacks. These tests must be automated, and there can be multiple environments created to fulfil different test requirements. For example, a server dedicated for Integration Testing will have the code deployed there to conduct integration tests. Once that passes, there can be other sub-layers in the testing phase in which the code will be deployed to, so as to conduct further tests. Such as User Acceptance Testing (UAT), and another can be Penetration Testing. These servers will be named according to what they have been designed to do in those environments. A UAT server is generally be used for UAT, SIT server is for Systems Integration Testing, PEN Server is for Penetration Testing and they can be named whatever the naming style or convention in which the team is used. An environment does not necessarily have to reside on one single server. In most cases it might be a stack as you have defined in your Ansible Inventory. All the servers in the inventory/dev are considered as Dev Environment. The same goes for inventory/stage (Staging Environment) inventory/preprod (Pre-production environment), inventory/prod (Production environment), etc. So, it is all down to naming convention as agreed and used company or team wide.

Deploy to production: Once all the tests have been conducted and either the release manager or whoever has the authority to authorize the release to the production server is happy, he gives green light to hit the deploy button to ship the release to production environment. This is an Ideal Continuous Delivery Pipeline. If the entire pipeline was automated and no human is required to manually give the Go decision, then this would be considered as Continuous Deployment. Because the cycle will be repeated, and every time there is a code commit and push, it causes the pipeline to trigger, and the loop continues over and over again.

Measure and Validate: This is where live users are interacting with the application and feedback is being collected for further improvements and bug fixes. There are many metrics that must be determined and observed here. We will quickly go through 13 metrics that MUST be considered.

Common Best Practices of CI/CD
Before we move on to observability metrics – let us list down the principles that define a reliable and robust CI/CD pipeline:

Maintain a code repository
Automate build process
Make builds self-tested
Everyone commits to the baseline every day
Every commit to baseline should be built
Every bug-fix commit should come with a test case
Keep the build fast
Test in a clone of production environment
Make it easy to get the latest deliverables
Everyone can see the results of the latest build
Automate deployment (if you are confident enough in your CI/CD pipeline and willing to go for a fully automated Continuous Deployment)
Why are we doing everything we are doing? – 13 DEVOPS SUCCESS METRICS
After all, DevOps is all about continuous delivery or deployment, and being able to ship out quality code as fast as possible. This is a very ambitious thing to desire; therefore, we must be careful not to break things as we are moving very fast. By tracking these metrics, we can determine our delivery speed and bottlenecks before breaking things. Ultimately, the goals of DevOps are enhanced Velocity, Quality, and Performance. But how do we track these parameters? Let us have a look at the 13 metrics to watch out for.

Deployment frequency: Tracking how often you do deployments is a good DevOps metric. Ultimately, the goal is to do more smaller deployments as often as possible. Reducing the size of deployments makes it easier to test and release. I would suggest counting both production and non-production deployments separately. How often you deploy to QA or pre-production environments is also important. You need to deploy early and often in QA to ensure enough time for testing.

Lead time: If the goal is to ship code quickly, this is a key DevOps metric. I would define lead time as the amount of time that occurs between starting on a work item until it is deployed. This helps you know that if you started on a new work item today, how long would it take on average until it gets to production.

Customer tickets: The best and worst indicator of application problems is customer support tickets and feedback. The last thing you want is your users reporting bugs or having problems with your software. Because of this, customer tickets also serve as a good indicator of application quality and performance problems.

Percentage of passed automated tests: To increase velocity, it is highly recommended that the development team makes extensive usage of unit and functional testing. Since DevOps relies heavily on automation, tracking how well automated tests work is a good DevOps metrics. It is good to know how often code changes break tests.

Defect escape rate: Do you know how many software defects are being found in production versus QA? If you want to ship code fast, you need to have confidence that you can find software defects before they get to production. Defect escape rate is a great DevOps metric to track how often those defects make it to production.

Availability: The last thing we ever want is for our application to be down. Depending on the type of application and how we deploy it, we may have a little downtime as part of scheduled maintenance. It is highly recommended to track this metric and all unplanned outages. Most software companies build status pages to track this. Such as this Google Products Status Page

Service level agreements: Most companies have some service level agreement (SLA) that they promise to the customers. It is also important to track compliance with SLAs. Even if there are no formally stated SLAs, there probably are application non-functional requirements or expectations to be met.

Failed deployments: We all hope this never happens, but how often do our deployments cause an outage or major issues for the users? Reversing a failed deployment is something we never want to do, but it is something you should always plan for. If you have issues with failed deployments, be sure to track this metric over time. This could also be seen as tracking Mean Time To Failure (MTTF).

Error rates: Tracking error rates within the application is super important. Not only they serve as an indicator of quality problems, but also ongoing performance and uptime related issues. In software development, errors are also known as exceptions, and proper exception handling is critical. If they are not handled nicely, we can figure it out while monitoring the rate of errors.

Bugs – Identify new exceptions being thrown in the code after a deployment
Production issues – Capture issues with database connections, query timeouts, and other related issues
Presenting error rate metrics like this simply gives greater insights into where to focus attention.

Application usage & traffic: After a deployment, we want to see if the number of transactions or users accessing our system looks normal. If we suddenly have no traffic or a giant spike in traffic, something could be wrong. An attacker may be routing traffic elsewhere, or initiating a DDOS attack

Application performance: Before we even perform a deployment, we should configure monitoring tools like Retrace, DataDog,New Relic, or AppDynamics to look for performance problems, hidden errors, and other issues. During and after the deployment, we should also look for any changes in overall application performance and establish some benchmarks to know when things deviate from the norm.

It might be common after a deployment to see major changes in the usage of specific SQL queries, web service or HTTP calls, and other application dependencies. These monitoring tools can provide valuable visualizations like this one below that helps make it easy to spot problems.

Mean time to detection (MTTD): When problems happen, it is important that we identify them quickly. The last thing we want is to have a major partial or complete system outage and not know about it. Having robust application monitoring and good observability tools in place will help us detect issues quickly. Once they are detected, we also must fix them quickly!

Mean time to recovery (MTTR): This metric helps us track how long it takes to recover from failures. A key metric for the business is keeping failures to a minimum and being able to recover from them quickly. It is typically measured in hours and may refer to business hours, not calendar hours.

These are the major metrics that any DevOps team should track and monitor to understand how well CI/CD process is established and how it helps to deliver quality application to the users.

SIMULATING A TYPICAL CI/CD PIPELINE FOR A PHP BASED APPLICATION
As part of the ongoing infrastructure development with Ansible started from Project 11, you will be tasked to create a pipeline that simulates continuous integration and delivery. Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both Tooling and TODO Web Applications are based on an interpreted (scripting) language (PHP). It means, it can be deployed directly onto a server and will work without compiling the code to a machine language.

The problem with that approach is, it would be difficult to package and version the software for different releases. And so, in this project, we will be using a different approach for releases, rather than downloading directly from git, we will be using Ansible uri module.

(screenshot)

Set Up
This project is partly a continuation of your Ansible work, so simply add and subtract based on the new setup in this project. It will require a lot of servers to simulate all the different environments from dev/ci all the way to production. This will be quite a lot of servers altogether (But you don’t have to create them all at once. Only create servers required for an environment you are working with at the moment. For example, when doing deployments for development, do not create servers for integration, pentest, or production yet).

Try to utilize your AWS free tier as much as you can, you can also register a new account if you have exhausted the current one. Alternatively, you can use Google Cloud (GCP) to rent virtual machines from this cloud service provider – you can get $300 credit here or here (NOTE: Please read instructions carefully to get your credits)

NOTE: This is still NOT a cloud-focus project yet. AWS cloud end to end project begins from project-15. Therefore, do not worry about advanced AWS or GCP configuration. All we need here is virtual machines that can be accessed over SSH.

To minimize the cost of cloud servers, you don not have to create all the servers at once, simply spin up a minimal server set up as you progress through the project implementation and have reached a need for more.

To get started, we will focus on these environments initially.

Ci
Dev
Pentest
Both SIT – For System Integration Testing and UAT – User Acceptance Testing do not require a lot of extra installation or configuration. They are basically the webservers holding our applications. But Pentest – For Penetration testing is where we will conduct security related tests, so some other tools and specific configurations will be needed. In some cases, it will also be used for Performance and Load testing. Otherwise, that can also be a separate environment on its own. It all depends on decisions made by the company and the team running the show.

What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table and diagrams.

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/third%20screenhot.png)

CI-Environment
image

Other Environments from Lower To Higher
image

DNS requirements Make DNS entries to create a subdomain for each environment. Assuming your main domain is total.com

You should have a subdomains list like this:

Server	Domain
Jenkins	https://ci.infradev.totaldevops.
Sonarqube	https://sonar.infradev.totaldevops.com
Artifactory	https://artifacts.infradev.totaldevops.com
Production Tooling	https://tooling.totaldevops.com
Pre-Prod Tooling	https://tooling.preprod.totaldevops.com
Pentest Tooling	https://tooling.pentest.totaldevops.com
UAT Tooling	https://tooling.uat.totaldevops.com
SIT Tooling	https://tooling.sit.totaldevops.com
Dev Tooling	https://tooling.dev.totaldevops.com
Production TODO-WebApp	https://todo.totaldevops.com
Pre-Prod TODO-WebApp	https://todo.preprod.darey.io
Pentest TODO-WebApp	https://todo.pentest.totaldevops.com
UAT TODO-WebApp	https://todo.uat.totaldevops.com
SIT TODO-WebApp	https://todo.sit.totaldevops.com
Dev TODO-WebApp	https://todo.dev.totaldevops.com
Ansible Inventory should look like this
├── ci
├── dev
├── pentest
├── pre-prod
├── prod
├── sit
└── uat
ci inventory file

[jenkins]
<Jenkins-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[sonarqube]
<SonarQube-Private-IP-Address>

[artifact_repository]
<Artifact_repository-Private-IP-Address>
dev Inventory file

[tooling]
<Tooling-Web-Server-Private-IP-Address>

[todo]
<Todo-Web-Server-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<DB-Server-Private-IP-Address>
pentest inventory file

[pentest:children]
pentest-todo
pentest-tooling

[pentest-todo]
<Pentest-for-Todo-Private-IP-Address>

[pentest-tooling]
<Pentest-for-Tooling-Private-IP-Address>
Observations:
You will notice that in the pentest inventory file, we have introduced a new concept pentest:children This is because, we want to have a group called pentest which covers Ansible execution against both pentest-todo and pentest-tooling simultaneously. But at the same time, we want the flexibility to run specific Ansible tasks against an individual group.

The db group has a slightly different configuration. It uses a RedHat/Centos Linux distro. Others are based on Ubuntu (in this case user is ubuntu). Therefore, the user required for connectivity and path to python interpreter are different. If all your environment is based on Ubuntu, you may not need this kind of set up. Totally up to you how you want to do this. Whatever works for you is absolutely fine in this scenario.

This makes us to introduce another Ansible concept called group_vars. With group vars, we can declare and set variables for each group of servers created in the inventory file.

For example, If there are variables we need to be common between both pentest-todo and pentest-tooling, rather than setting these variables in many places, we can simply use the group_vars for pentest. Since in the inventory file it has been created as pentest:children Ansible recognizes this and simply applies that variable to both children.

ANSIBLE ROLES FOR CI ENVIRONMENT
Now go ahead and Add two more roles to ansible:

SonarQube (Scroll down to the Sonarqube section to see instructions on how to set up and configure SonarQube manually)

Artifactory

Why do we need SonarQube?
SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities. Watch a short description here. There is a lot more hands on work ahead with SonarQube and Jenkins. So, the purpose of SonarQube will be clearer to you very soon.

Why do we need Artifactory?
Artifactory is a product by JFrog that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be used for certain other automation, but we will it strictly to manage our build artifacts.

Watch a short description here Focus more on the first 10.08 mins

Configuring Ansible For Jenkins Deployment
In previous projects, you have been launching Ansible commands manually from a CLI. Now, with Jenkins, we will start running Ansible from Jenkins UI.

To do this 0. Set up SSH-agent

eval `ssh-agent -s`
ssh-add <path-to-private-key>
eval `ssh-agent -s`
ssh-add ade.pem
ssh-add -l 
ssh -A ubuntu@34.233.123.4
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/ssh%20into%20the%20jenkins%20server%20and%20create%20ssh%20agent.png)

Navigate to Jenkins URL
<Jenkins-server-public-IP>:8080
Install & Open Blue Ocean Jenkins Plugin In the Jenkins dashboard, click on Manage Jenkins -> Manage plugins and search for Blue Ocean plugin. Install and open Blue Ocean plugin

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/install%20blue%20ocean%20on%20jenkins.png)

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/downloading%20blue%20ocean.png)

Create a new pipeline

Select GitHub and click on create access token

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/click%20on%20create%20access%20token%20and%20it%20will%20take%20you%20to%20github.png)

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/click%20on%20create%20access%20token%20and%20it%20will%20take%20you%20to%20github.png)
Connect Jenkins with GitHub

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/creating%20access%20token%20on%20github.png)

Login to GitHub & Generate an Access Token then click on the github repository and create the pipeline

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/choose%20the%20git%20hub%20repository%20and%20create%20the%20pipeline.png)

At this point you may not have a [Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/) in the Ansible repository, so Blue Ocean will attempt to give you some guidance to create one. But we do not need that. We will rather create one ourselves. So, click on Administration to exit the Blue Ocean console.

Here is our newly created pipeline. It takes the name of your GitHub repository

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/our%20repository%20is%20now%20added%20in%20jenkins.png)

Let us create our Jenkinsfile
In Vscode, inside the Ansible project, create a new directory and name it deploy, create a new file Jenkinsfile inside the directory

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/creating%20jenkinsfile%20on%20vscode.png)

Add the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage

pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }
    }
}

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/content%20of%20the%20jenkins%20file.png)

Now go back into the Ansible pipeline in Jenkins, and select configure

![screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/go%20to%20jenkins%20and%20select%20configure.png)

Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/Build%20Configuration%20specify%20Jenkinsfile%20at%20deployJenkinsfile.png)

Back to the pipeline again, this time click Build now

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/result%20after%20clicking%20build%20now.png)

This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/the%20console%20output.png)

To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.

Click on Blue Ocean
Select your project
Click on the play button against the branch

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/the%20build%20from%20blue%20ocean%20interface.png)

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/the%20build%20from%20blue%20ocean%20interface2.png)

Notice that this pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

Let us see this in action

Create a new git branch and name it feature/jenkinspipeline-stages
git checkout -b feature/jenkinspipeline-stages

Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub.
pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    }
}
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/test%20stage%20jenkins%20file%20and%20pushing%20to%20github.png)
To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository.

Click on the "Administration" button

Navigate to the Ansible project and click on Scan repository now

Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too.
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/click%20on%20scan%20repository.png)

In Blue Ocean, you can now see how the Jenkinsfile has caused a new step in the pipeline launch build for the new branch.
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/test%20stage%20on%20blue%20ocean.png)

A QUICK TASK FOR YOU!
1. Create a pull request to merge the latest code into the main branch
2. After merging the PR, go back into your terminal and switch into the main branch.
3. Pull the latest change.
4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build
and test stages)
   1. Package 
   2. Deploy 
   3. Clean up
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/additional%20tasks%20for%20more%20stages.png)

  . Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch
6. Eventually, your main branch should have a successful pipeline like this in blue ocean
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/additional%20tasks%20for%20more%20stages%20on%20blue%20ocean%20pipeline.png)

 Running Ansible Playbook from Jenkins
Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

Installing Ansible on Jenkins
sudo apt update
sudo apt upgrade -y
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sudo%20apt%20update%20sudu%20apt%20upgrade.png)
Install Required Dependencies

sudo apt install software-properties-common -y
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sudo%20apt%20install%20software%20properties%20common.png)
Add the Ansible PPA (Personal Package Archive)

sudo add-apt-repository --yes --update ppa:ansible/ansible
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sudo%20add%20apt%20repository%20yess%20update%20ppa.png)
sudo apt update
sudo apt install ansible -y
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sudo%20apt%20update%20sudo%20apt%20install%20ansible%20y.png)
Verify the Installation

ansible --version
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/ansible%20version.png)

Installing Ansible plugin in Jenkins UI On the dashboard page, click on Manage Jenkins > Manage plugins > Available type in ansible and install without restart
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/downloading%20ansible%20from%20jenkins.png)

Click on Dashboard > Manage Jenkins > Global Tool Configuration > Add Ansible. Add a name and the path ansible is installed on the jenkins server

Get the path to ansible installed

 which ansible
 ![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/which%20ansible.png)
 Then enter the above path to Jenkins GUI as follows
 ![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/adding%20ansible%20path.png)

 Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)
You can watch a [10 minutes video here](https://youtu.be/PRpEbFZi7nI) to guide you through the entire setup

Let's delete the content of current Jenkinsfile nad create a new Jenkinsfile from scratch to run the ansible playbook against the dev environment.

To do this let's ensure git module is checking out SCM from main branch.

    pipeline {
    agent any

  stages {
     stage("Initial cleanup") {
      steps {
        dir("${WORKSPACE}") {
          deleteDir()
        }
      }
    }

    stage('Checkout SCM') {
      steps {
        git branch: 'main', credentialsId: 'Taiwo', url: 'https://github.com/Prince-Tee/ansible-config-mgt.git'
      }
    }
    
    stage('Prepare Ansible For Execution') {
      steps {
        sh 'echo ${WORKSPACE}'
        sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'
      }
    }

   stage('Test SSH Connection') {
    steps {
        sshagent(['Taiwo']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.62 exit'
        }
    }
}
    
   


    stage('Run Ansible playbook') {
      steps {
       ansiblePlaybook credentialsId: 'Taiwo', disableHostKeyChecking: true, installation: 'ansible-config-mgt', inventory: 'inventory/dev.ini', playbook: 'playbooks/site.yml', vaultTmpPath: ''
      }
    }
      stage('Clean Workspace after build') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
      }
    }


    }
}



![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/run%20anisble%20successful.PNG)

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/clear%20workspace%20after%20build%20working.PNG)

Summary
Initial cleanup: Ensures a fresh workspace.

Checkout SCM: Pulls the latest code.

Prepare Ansible: Configures Ansible for the pipeline.

Test SSH Connections: Verifies connectivity to all servers.

Run Ansible playbook: Deploys infrastructure or applications. Ansible playbook. : - Uses the sshagent step to ensure the SSH key is available for Ansible. - Runs the ansiblePlaybook step with the specified parameters . #### To ensure jenkins properly connects to all servers, you will need to install another plugin known as ssh agent , after that, go to manage jenkins > credentials > global > add credentials , usee ssh username and password , fill out the neccesary details and save.

Clean Workspace: Cleans up files post-build.


# Parameterizing Jenkinsfile For Ansible Deployment

Update SIT Inventory
Define servers in the sit inventory file:

[tooling]
SIT-Tooling-Web-Server-Private-IP-Address

[todo]
SIT-Todo-Web-Server-Private-IP-Address

[lb]
SIT-Nginx-Private-IP-Address

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
SIT-DB-Server-Private-IP-Address
Add Parameters to Jenkinsfile
Introduce parameters for environment and tags:

pipeline {
    agent any
    parameters {
       string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
       string(name: 'tags', defaultValue: '', description: 'Ansible tags to limit execution')
     }
}

![(Screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/adding%20the%20inventory%20and%20tag%20parameters%20in%20jenkins%20file.png)

Update the inventory path with this : ${inventory}

Default values ensure the dev environment is used if no other value is specified.

Update the jenkins file to included the ansible tags before it runs playbook

Execute Pipeline in Jenkins
Go to the pipeline job in Jenkins.
Select Build with Parameters.
Input values for the environment (e.g., sit) and tags (e.g., webserver).
Click Build Now to execute the deployment.

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/adding%20inventory%20and%20webserver%20as%20parameters.png)
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/after%20adding%20inventory%20and%20tags.png)

# CI/CD Pipeline for PHP TODO Application
We previously set up a tooling website deployment through Ansible. Now, we are adding another PHP application to our infrastructure management. This application includes unit tests, making it ideal for demonstrating an end-to-end CI/CD pipeline.

The deployment goal is to use Artifactory for storing and deploying code artifacts, bypassing direct Git integration, this will be done by installing artifactory on a new server.

Install Artifactory
Install artifactory by following this guide https://www.fosstechnix.com/install-jfrog-artifactory-on-ubuntu-24-04-lts/ 

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/artifactory%20on%20the%20browser%20after%20installation.png)


cd /opt/artifactory-oss-6.9.6

sudo ./bin/artifactory.sh start

http://<ip-address>:8081

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/logged%20into%20the%20jfrog%20artifactory.PNG)

Phase 1: Prepare Jenkins
1. Fork the Repository
Fork the following repository into your GitHub account:

https://github.com/StegTechHub/php-todo.git

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/create%20the%20fork%20of%20the%20todo%20repository.PNG)

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/fork%20created.PNG)

2. Install PHP and Dependencies
On you Jenkins server, install PHP, its dependencies and Composer tool (Feel free to do this manually at first, then update your Ansible accordingly later)
sudo apt update
Install dependancies

sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}
Install Composer Download the Installer:

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
Install Composer Globally

sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
Remove the Installer

php -r "unlink('composer-setup.php');"
Verify Installation

php -v
composer -v

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sudo%20app%20update.PNG)
![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/composer%20and%20others%20installed.PNG)

3. Install Required Jenkins Plugins
Plot Plugin: For displaying test reports and code coverage.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/download%20plot%20plugin.PNG)

Artifactory Plugin: For uploading code artifacts to Artifactory.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/install%20atifactory%20plugin.PNG)

4. Configure Artifactory in Jenkins
Go to the Jenkins UI, manage Jenkins> system> Jfrog Platform Instances.
Add Artifactory server details (ID, URL, and credentials).
Test the connection to ensure it’s working.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/configure%20jfrog%20artifactory%20in%20jenkins.PNG)

Create a local repository todo in your jrog atifactory and  set the repository type to generic

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/creating%20local%20repository%20on%20artifactory.PNG)

Integrate Artifatory repository with Jenkins

We will create a dummy Jenkinsfile in the php-todo-app repository.

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/creating%20jenkinsfile%20on%20vscode22.png)

Using blue ocean, we will create a multibranch jenkins pipeline connected to the php-todo-app repository.

![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/adding%20the%20php%20todo%20as%20a%20new%20pipeline%20in%20jenkins.png)
![(screenshot)](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/running%20our%20jenkins%20file%20dummy%20on%20jenkins%20server.PNG)

In jenkins server Install my sql client:

sudo apt install mysql-client -y

v3. Configure the Database
Update the env_vars/dev.yml file to include the todo app database named homestead.:

# MySQL configuration for the development environment
mysql_root_username: "root"
mysql_root_password: "YourNewPassword"

# Define databases and users to be created for the dev environment
mysql_databases:
  - name: homestead
    encoding: utf8
    collation: utf8_general_ci

mysql_users:
  - name: homestead
    host: "%"
    password: YourNewPassword
    priv: "*.*:ALL,GRANT"

Also update your security group for port 3306 to allow inbound traffic from your jenkins server

Login into the DB-server(mysql server) and set the the bind address to 0.0.0.0:

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
Restart the my sql- server:

sudo systemctl restart mysql

Update the database connectivity requirements in the file .env.sample

DB_HOST=172.31.34.228
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=YourNewPassword
DB_CONNECTION=mysql 
DB_PORT=3306

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/envsample%20file%20in%20the%20todo%20php%20app.PNG)

Save the actual details as environment variables in the Jenkins UI Navigate to Manage jenkins > System > Global properties > Environment variables

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/adding%20our%20db%20credentials%20to%20jenkins.PNG)

Update Jenkinsfile with proper pipeline configuration

pipeline {
    agent any
    stages {
        stage('Initial Cleanup') {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Prince-Tee/php-todo.git'
            }
        }
        stage('Prepare Dependencies') {
            steps {
                script {
                    // Move .env.sample to .env and set environment variables
                    sh '''
                        mv .env.sample .env
                        echo "DB_HOST=${DB_HOST}" >> .env
                        echo "DB_PORT=${DB_PORT}" >> .env
                        echo "DB_DATABASE=${DB_DATABASE}" >> .env
                        echo "DB_USERNAME=${DB_USERNAME}" >> .env
                        echo "DB_PASSWORD=${DB_PASSWORD}" >> .env
                    '''
                    
                    // Create bootstrap cache directory with appropriate permissions
                    sh '''
                        mkdir -p bootstrap/cache
                        chown -R jenkins:jenkins bootstrap/cache
                        chmod -R 775 bootstrap/cache
                    '''
                    
                    // Install Composer dependencies with error handling
                    sh '''
                        set -e
                        composer install --no-scripts
                    '''
                    
                    // Run Laravel artisan commands
                    sh '''
                        php artisan key:generate
                        php artisan clear-compiled
                        php artisan migrate --force
                        php artisan db:seed --force
                    '''
                }
            }
        }
    }
}

If the above fails it might be due to composer compatibility issues with your php version, for that try the step below to fix the potential issues:

STEP :
Downgrade your PHP version to 7.4 if possible:
Add the Required Repository Ubuntu does not include older PHP versions in its default repositories. Use the ondrej/php PPA (a popular source for PHP packages):
sudo apt update
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:ondrej/php
sudo apt update
Install PHP 7.4 Install PHP 7.4 and any necessary modules:
sudo apt install -y php7.4 php7.4-cli php7.4-fpm php7.4-mbstring php7.4-xml php7.4-curl php7.4-mysql
Update Alternatives Register PHP 7.4 with the update-alternatives system:
sudo update-alternatives --install /usr/bin/php php /usr/bin/php7.4 74
sudo update-alternatives --install /usr/bin/php php /usr/bin/php8.3 83
Switch to PHP 7.4 Use update-alternatives to select PHP 7.4:
sudo update-alternatives --config php
Verify the PHP Version Confirm the active PHP version:
php -v

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/jenkins%20running%20after%20chaning%20the%20content%20of%20the%20jenkins%20file.PNG)

– After successful run of this step, login to the database,password:root

mysql -u root -p
run show tables and you will see the tables being created for you

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/showing%20database.PNG)

Update the Jenkinsfile to include Unit tests step first install
composer require --dev phpunit/phpunit


stage ('Execute Unit Tests') {
            steps {
                sh './vendor/bin/phpunit'                
            }
        } 

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/execute%20units%20test%20running.PNG)

Phase 3 – Code Quality Analysis
This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc. Read the article here for more

The data produced by phploc can be ploted onto graphs in Jenkins.

Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/code%20analysis.PNG)

Configure the Plot Plugin Use the Plot plugin to graphically display phploc data in Jenkins. Add the following stage to the Jenkinsfile:
stage('Plot Code Coverage Report') {
      steps {

            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Lines of Code (LOC),Comment Lines of Code (CLOC),Non-Comment Lines of Code (NCLOC),Logical Lines of Code (LLOC)                          ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'A - Lines of code', yaxis: 'Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Directories,Files,Namespaces', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'B - Structures Containers', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Average Class Length (LLOC),Average Method Length (LLOC),Average Function Length (LLOC)', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'C - Average Length', yaxis: 'Average Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Cyclomatic Complexity / Lines of Code,Cyclomatic Complexity / Number of Methods ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'D - Relative Cyclomatic Complexity', yaxis: 'Cyclomatic Complexity by Structure'      
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Classes,Abstract Classes,Concrete Classes', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'E - Types of Classes', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Methods,Non-Static Methods,Static Methods,Public Methods,Non-Public Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'F - Types of Methods', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Constants,Global Constants,Class Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'G - Types of Constants', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Test Classes,Test Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'I - Testing', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Logical Lines of Code (LLOC),Classes Length (LLOC),Functions Length (LLOC),LLOC outside functions or classes ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'AB - Code Structure by Logical Lines of Code', yaxis: 'Logical Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Functions,Named Functions,Anonymous Functions', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'H - Types of Functions', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Interfaces,Traits,Classes,Methods,Functions,Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'BB - Structure Objects', yaxis: 'Count'


      }
    }

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/plotting%20code%20coverage%20report.PNG)

You should now see a Plot menu item on the left menu. Click on it to see the charts. (The analytics may not mean much to you as it is meant to be read by developers. So, you need not worry much about it – this is just to give you an idea of the real-world implementation).

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/linesof%20code%20for%20plot%20coverage1.PNG)

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/linesof%20code%20for%20plot%20coverage2.PNG)

Bundle the application code for into an artifact (archived package) upload to Artifactory
stage ('Package Artifact') {
    steps {
            sh 'zip -qr php-todo.zip ${WORKSPACE}/*'
     }
    }

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/package%20artifact.PNG)    

Publish the resulted artifact into Artifactory
stage ('Upload Artifact to Artifactory') {
          steps {
            script { 
                 def server = Artifactory.server 'artifactory'                 
                 def uploadSpec = """{
                    "files": [
                      {
                       "pattern": "php-todo.zip",
                       "target": "todo/php-todo",
                       "props": "type=zip;status=ready"

                       }
                    ]
                 }""" 

                 server.upload spec: uploadSpec
               }
            }
  
        }

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/upload%20artifact%20to%20artifactory.PNG)

Confirm upload on Artifactory:

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/confirm%20upload%20on%20jrog%20artifactory%20server.PNG)

Deploy the application to the dev environment by launching Ansible pipeline
stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-config-mgt/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
The build job used in this step tells Jenkins to start another job. In this case it is the ansible-project job, and we are targeting the main branch. Hence, we have ansible-project/main. Since the Ansible project requires parameters to be passed in, we have included this by specifying the parameters section. The name of the parameter is env and its value is dev. Meaning, deploy to the Development environment.

we need to get some things in place first.

first create an rhel ec2 instance for the development todo webserver named DEV-todo-webapp.

Include it's private IP in the dev environment inventory file.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/deploy%20to%20dev%20env.PNG)

But how are we certain that the code being deployed has the quality that meets corporate and customer requirements? Even though we have implemented Unit Tests and Code Coverage Analysis with phpunit and phploc, we still need to implement Quality Gate to ensure that ONLY code with the required code coverage, and other quality standards make it through to the environments.

To achieve this, we need to configure SonarQube - An open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities


SonarQube Installation
Understanding SonarQube and Software Quality
Before diving into the installation of SonarQube, it is crucial to understand the concepts it addresses:

Software Quality
Software quality refers to the extent to which a software component, system, or process meets specified requirements based on user needs and expectations.

Software Quality Gates
Quality gates are acceptance criteria presented as a predefined set of quality measures that a software project must satisfy to progress to the next stage of its lifecycle. SonarQube facilitates the creation of these quality gates, ensuring only high-quality software is shipped.

Importance of SonarQube in DevOps
In DevOps pipelines, speed is essential for software delivery. However, the quality of delivery must not be compromised. SonarQube addresses this by enabling the setup of quality gates, ensuring adherence to predefined quality standards. In this project, we will use the predefined "Sonar Way" quality gate. Custom quality gates can also be defined in collaboration with software testers, developers, and project leads.

SonarQube Installation on Ubuntu 20.04 with PostgreSQL Backend
This guide walks through the manual installation of SonarQube version 7.9.3 with PostgreSQL as its backend database. The installation process involves configuring the Linux kernel for optimal performance, installing dependencies, setting up PostgreSQL, and configuring SonarQube. Automation using Ansible is recommended for production environments.

First create the sonarqube EC2 instance preferably with t2.medium. Allow inbound traffic on sonarqube default port 9000

Step 1: Tune Linux Kernel
To optimize the performance of SonarQube, certain kernel parameters need to be adjusted. These changes can be made either for the current session or permanently.

Temporary Changes
Execute the following commands:

sudo sysctl -w vm.max_map_count=262144
sudo sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 4096

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/temporary%20change%20sonarqube.PNG)

Permanent Changes
To make these changes persistent:

Edit the file /etc/security/limits.conf.
Append the following lines:
sonarqube - nofile 65536
sonarqube - nproc 4096

Step 2: Update and Upgrade System Packages
sudo apt-get update
sudo apt-get upgrade -y
Step 3: Install Required Packages
Install wget and unzip:

sudo apt-get install wget unzip -y

![sh](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sudo%20apt%20get%20install%20wget%20unzip%20y.PNG)

Step 4: Install OpenJDK and Java Runtime Environment (JRE) 11
SonarQube is Java-based, so installing Java is a prerequisite.

sudo apt-get install openjdk-17-jdk -y
sudo apt-get install openjdk-17-jre -y

Set Default JDK
If multiple versions of Java are installed, set OpenJDK 11 as the default:

sudo update-alternatives --config java

From the list, select OpenJDK 11 by entering the corresponding number.

Verify Java Installation
java -version

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/openjk%20java%20version.PNG)

Step 5: Install and Configure PostgreSQL
Add PostgreSQL Repository

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

Add PostgreSQL Key

wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

Install PostgreSQL
sudo apt-get install postgresql postgresql-contrib -y

Start and Enable PostgreSQL
sudo systemctl start postgresql
sudo systemctl enable postgresql

Configure PostgreSQL
Change the password for the default postgres user:

sudo passwd postgres
Switch to the postgres user:

su - postgres

Create a new user and database for SonarQube:

Create a new user by typing
createuser sonar
Switch to the PostgreSQL shell
psql
Set a password for the newly created user for SonarQube database
ALTER USER sonar WITH ENCRYPTED password 'sonar';
Create a new database for PostgreSQL database by running:
CREATE DATABASE sonarqube OWNER sonar;
Grant all privileges to sonar user on sonarqube Database.
grant all privileges on DATABASE sonarqube to sonar;
Exit from the psql shell
\q

Exit the postgres user:
exit

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/postgre%20installation%20and%20database%20installation.PNG)


Step 6: Install SonarQube
Download and Extract SonarQube
Navigate to the /tmp directory:

cd /tmp
Download SonarQube:

 sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.8.100196.zip
Extract the downloaded archive to the /opt directory:

sudo unzip sonarqube-9.9.8.100196.zip -d /opt
sudo mv /opt/sonarqube-9.9.8.100196 /opt/sonarqube

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/installing%20sonarqube.PNG)

Configuring SonarQube for Continuous Integration
SonarQube is a powerful tool for continuous code quality inspection. This guide outlines the steps for setting up and configuring SonarQube, ensuring it runs optimally within a CI/CD pipeline.

Step 1: Create a User and Group for SonarQube
To prevent running SonarQube as the root user:

Create a group named sonar:
sudo groupadd sonar
Add a user named sonar to manage the SonarQube directory:
sudo useradd -c "User to run SonarQube" -d /opt/sonarqube -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube -R
Step 2: Configure SonarQube
Open the SonarQube configuration file:

sudo nano /opt/sonarqube/conf/sonar.properties
Update the database connection settings:

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
Save and close the file. 

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/updating%20sonar%20qube%20with%20username%20and%20password.PNG)

Edit the SonarQube script file to specify the sonar user:
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh

Update the RUN_AS_USER field:
RUN_AS_USER=sonar

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/run%20as%20user%20sonar.PNG)

Step 3: Start SonarQube
Switch to the sonar user:
sudo su sonar
Navigate to the startup script directory:
cd /opt/sonarqube/bin/linux-x86-64/
Start SonarQube:
./sonar.sh start
Expected output:
Starting SonarQube...
Started SonarQube
Check the status of SonarQube:
./sonar.sh status

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/running%20sonarqube.PNG)

To monitor logs:
tail /opt/sonarqube/logs/sonar.log

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/monitor%20sonarqube.PNG)

Step 4: Configure SonarQube as a Systemd Service
Stop the currently running SonarQube instance:
./sonar.sh stop
Create a systemd service file:
sudo nano /etc/systemd/system/sonar.service
Add the following configuration:
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonar
Group=sonar
Restart=always
LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
Save and close the file.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/content%20of%20systemd%20system%20sonar%20service.PNG)

Start the SonarQube service:

sudo systemctl start sonar
Enable SonarQube to start at boot:

sudo systemctl enable sonar
Verify the service status:

sudo systemctl status sonar

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/enabling%20sonar.PNG)

Step 5: Access SonarQube
Visit sonarqube config file and uncomment the line of sonar.web.port=9000
sudo nano /opt/sonarqube/conf/sonar.properties

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/uncomment%20the%20line%209000.PNG)

Open your browser and navigate to the SonarQube instance:
firstly add an inbound rule for port 9000 in your AWS security group

http://<server_IP>:9000

Replace <server_IP> with your server's IP address.

![sceenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sonar%20qube%209000.PNG)

Login with the default credentials:
Username: admin
Password: admin

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/login%20with%20admin%20admin%20on%209000.PNG)

Change the default password for better security.


Configuring SonarQube and Jenkins for Quality Gate
Steps to Configure SonarQube in Jenkins
1. Install SonarQube Scanner Plugin
Go to Manage Jenkins > Plugins > Available Plugins.
Search for SonarQube Scanner and install it.
2. Add SonarQube Server in Jenkins
Navigate to Manage Jenkins > Configure System.

Under "SonarQube Servers":

Add a new server configuration.
Provide a name (e.g., sonarqube).
Input the server URL (e.g., http://<SonarQube-Server-IP>:9000).
Add authentication token generated in the next step.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/configure%20sonarqube%20on%20jenkins.PNG)

3. Generate Authentication Token in SonarQube
Log into SonarQube and navigate to User > My Account > Security > Generate Tokens.
Create a token and copy it.

Paste the token in the Jenkins configuration under SonarQube server authentication

4. Configure Webhook for Quality Gate
In SonarQube, navigate to Administration > Configuration > Webhooks > Create.
Add the Jenkins server URL for the webhook: http://<JENKINS_HOST>/sonarqube-webhook/.
Save the configuration.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/adding%20jenins%20server%20to%20sonarqube.PNG)

Configure SonarQubeScanner tool:
Manage jenkins > Tools > Add SonarQube scanner


Update Jenkins Pipeline for SonarQube
1. Quality Gate Stage in Jenkinsfile
Add the following snippet to your Jenkinsfile:

stage('SonarQube Quality Gate') {
    environment {
        scannerHome = tool 'SonarQubeScanner'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }
}

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/ERROR%20ON%20SONAR%20QUBE.PNG)

note: The above step will fail because we have not updated `sonar-scanner.properties

Configure sonar-scanner.properties
Locate the scanner configuration directory on the Jenkins server:

cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/conf/

sudo vi sonar-scanner.properties
Update the file with your project configuration. Example for php-todo:

sonar.host.url=http://<SonarQube-Server-IP>:9000
sonar.projectKey=php-todo
#----- Default source code encoding
sonar.sourceEncoding=UTF-8
sonar.php.exclusions=**/vendor/**
sonar.php.coverage.reportPaths=build/logs/clover.xml
sonar.php.tests.reportPath=build/logs/junit.xml
hint: To know what exactly to put inside the sonar-scanner.properties file, SonarQube has a configurations page where you can get some directions.

3. Examine Scanner Tool
Verify the scanner setup:

cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/bin
ls -latr

You should see files like sonar-scanner and sonar-scanner-debug.

Run the build again

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sonarqube%20now%20running.PNG)

4. Generate Pipeline Syntax
To generate additional code snippets for SonarQube integration:

Navigate to Dashboard > pht-todo > Pipeline Syntax in Jenkins.
Click on steps then, Select withSonarQubeEnv.
Generate and add the required block to your pipeline.

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/generate%20pipeline%20script%20for%20sonarqube.PNG)

Run the build again

![screenshot](https://github.com/Prince-Tee/continuousIntegration_Devops/blob/main/sreenshot%20from%20my%20environment/sonarqube%20now%20running.PNG)

But we are not completely done yet!
The quality gate we just included has no effect. Why? Well, because if you go to the SonarQube UI, you will realise that we just pushed a poor-quality code onto the development environment.

Navigate to php-todo project in SonarQube

Implementing Quality Gate Conditions
1. Update Pipeline to Include Branch Filtering and Timeout
Modify the Jenkinsfile to check branch names and enforce quality gate conditions:

stage('SonarQube Quality Gate') {
    when {
        branch pattern: "^develop*|^hotfix*|^release*|^main*", comparator: "REGEXP"
    }
    environment {
        scannerHome = tool 'SonarQubeScanner'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
        }
        timeout(time: 1, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
2. Test Pipeline with Different Branches
Push code to branches like develop, hotfix, release, or main.
Observe that only code from these branches triggers the quality gate.
Deploy to Higher Environments
Branching Strategy (GitFlow)
Develop: Active development.
Release: Staging environment.
Hotfix: Emergency fixes.
Main: Stable production.
Ensure deployment happens only from allowed branches.

Additional Configuration Tasks
1. Jenkins Agents/Slaves
Jenkins architecture is fundamentally "Master+Agent". The master is designed to do co-ordination and provide the GUI and API endpoints, and the Agents are designed to perform the work. The reason being that workloads are often best "farmed out" to distributed servers.

Add two servers as Jenkins agents (Create two t2.medium) and install Java 17 on them.

Configure Jenkins to run jobs on any available slave node.
Go to dashboard > manage jenkins > security > Agents

Set the TCP port for inbound agents to fixed and set the port at 5000 ( or any one you choose )

Navigate to Dashboard > Manage Jenkins > Nodes
Create new node


Name: slave_1
Add /opt/build in the remote directory space (This can be any directory for the builds)
Labels: slave_1
Save




go back to slave terminal and run the following command
sudo mkdir -p /opt/build
sudo chown -R ubuntu:ubuntu /opt/build
sudo chmod -R 755 /opt/build
ls -ld /opt/build
curl -sO http://54.172.13.141:8080/jnlpJars/agent.jar


java -jar agent.jar -url http://54.172.13.141:8080/ -secret 654f1b09f00eb7476ab609cc5a304651e01dc69849e193abf048869c61850a1d -name "slave_1" -workDir "/opt/build"


Repeat for slave_2






2. Webhook Between Jenkins and GitHub
Set up a webhook in GitHub to trigger the Jenkins pipeline upon code push.


This project demonstrates the implementation of a comprehensive CI/CD pipeline for a PHP-based application using Jenkins, Ansible, Artifactory, SonarQube, and MySQL, ensuring automated builds, tests, deployments, and code quality analysis.
Key features include integrating Jenkins pipelines for multistage builds, Ansible for automated deployment, SonarQube for static code analysis, and Artifactory for artifact management.
The pipeline manages environment-specific configurations, runs unit tests, evaluates code quality against predefined gates, and deploys the application across multiple environments (e.g., Dev, SIT, UAT).
Challenges faced include setting up tools compatible with PHP 7.4, managing security credentials across tools, and ensuring seamless connectivity between Jenkins and remote servers.
This project showcases how companies can streamline development lifecycles, improve code quality, and enhance collaboration using DevOps principles.

conclusion

This project outlines the principles and practices of Continuous Integration (CI) and Continuous Delivery (CD) within software development, emphasizing the importance of understanding these concepts before practical implementation. It explains that CI involves merging developers' code into a shared repository multiple times a day to minimize integration issues, while CD ensures that code is always ready for deployment. The document details the roles of various tools like Jenkins, Ansible, and SonarQube in automating the build, test, and deployment processes, highlighting the difference between Continuous Delivery and Continuous Deployment. It also discusses best practices for establishing a robust CI/CD pipeline, such as maintaining a code repository and automating the build process. Additionally, it introduces key metrics for evaluating DevOps success, focusing on deployment frequency, lead time, and defect escape rate.
In conclusion, adopting CI/CD practices can significantly enhance software development efficiency and quality by promoting frequent integration and automated testing.
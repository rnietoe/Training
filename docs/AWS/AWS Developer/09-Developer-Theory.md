# Developer Theory

https://rnietoe.signin.aws.amazon.com/console

[Practicing Continuous Integration and Continuous Delivery on AWS](https://docs.aws.amazon.com/pdfs/whitepapers/latest/practicing-continuous-integration-continuous-delivery/practicing-continuous-integration-continuous-delivery.pdf)

* Continuos Integration -> shared code repository + automated build + automated tests
    * **AWS CodeCommit**: Source and version control of code repository. notifications can be set up for PR. HTTPS Git credentials are required for AWS CodeCommit

    ```shell
    sudo yum update -y
    sudo yum install git -y
    aws configure
    aws codecommit create-repository --repository-name RepoFromCLI --repository-description "My repository"
    git clone https://git-codecommit.eu-west-1.amazonaws/v1/repos/RepoFromCLI
    vim test.txt
    #Hit i to enter insert mode and type the text: This is just a test of working with CodeCommit from the CLI
    #Hit the Escape key, type :wq!, and press Enter.
    git add test.txt
    git commit -m "added test.txt"
    git log
    git push -u origin main
    ```
    * **AWS CodeArtifact**: (buildoutput.office.sbs) managed artifact repository service that makes it easy to securely store, publish, and share software packages used in their software development process.
    
    The lab creates three CodeArtifact resources:
    * The domain my-domain.
    * The repository my-repo that is contained in my-domain. This repository has an npm package available to it.
    * The repository npm-store that is contained in my-domain. This repository has an external connection to the public npm repository and is associated as an upstream repository with the my-repo repository.

    ```shell
    aws codeartifact create-domain --domain my-domain #1) Create a domain    
    aws codeartifact create-repository --domain my-domain --repository my-repo #2) Create a repository in your domain
    aws codeartifact create-repository --domain my-domain --repository npm-store #3) Create an upstream repository for your my-repo repository
    aws codeartifact associate-external-connection --domain my-domain --repository npm-store --external-connection "public:npmjs" #4) Add an external connection to the npm public repository to your npm-store repository
    aws codeartifact update-repository --repository my-repo --domain my-domain --upstreams repositoryName=npm-store #5) Associate the npm-store repository as an upstream repository to the my-repo repository
    aws codeartifact login --tool npm --repository my-repo --domain my-domain #6) Configure the npm package manager with your my-repo repository (fetches an authorization token from CodeArtifact using your AWS credentials)
    npm install express #7) Use the npm CLI to install an npm package. For example, to install the popular npm package express, use the following command, if we don’t specify a version, this command will install the latest version available in the external repo. express is a Node.js web application framework used to develop web and mobile applications
    aws codeartifact list-packages --domain my-domain --repository my-repo #8) View the package you just installed in your my-repo repository
    #9) To avoid further AWS charges, delete the resources you created:
    aws codeartifact delete-repository --domain my-domain --repository my-repo
    aws codeartifact delete-repository --domain my-domain --repository npm-store
    aws codeartifact delete-domain --domain my-domain
    ```

* Continuos Delivery: deploy the code manually
    * **AWS CodeBuild**: Compile code, run tests and prepare packages to deploy
    * **AWS CodeDeploy**: Automated deployment on premise, EC2, ECS and Lambda
        * in place. rollback redeploying previous version
        * blue/green. safest option! no capacity reduction, rollback faster. you pay for two environments
        * appspect.yml
            ```yml
            version: 0.0
            os: linux
            files:
                - source: /
                  destination: /var/www/html/WordPress
            hooks:
                BeforeInstall:
                    - location: scripts/install_dependencies.sh
                      timeout: 300
                      runas: root
                AfterInstall:
                    - location: scripts/change_permissions.sh
                      timeout: 300
                      runas: root
                ApplicationStart:
                    - location: scripts/start_server.sh
                    - location: scripts/create_test_db.sh
                      timeout: 300
                      runas: root
                ApplicationStop:
                    - location: scripts/stop_server.sh
                      timeout: 300
                      runas: root
            ```
        * folder setup in the root of the revision
            * appspect.yml
            * /script
            * /config
            * /source
        * phases:
            1. deregister instance from LB
            2. deploy the app
            3. register instance to LB
        * tasks:
            * BeforeBlockTraffic
            * BlockTraffic (phase 1)
            * AfterBlockTraffic
            * ApplicationStop
            * DownloadBundle
            * BeforeInstall
            * Install
            * AfterInstall
            * ApplicationStart
            * AfterApplicationStart
            * VAlidateService
            
        ```bash
        #Install CodeDeploy agent on your EC2 instance:
        sudo yum update
        sudo yum install ruby
        sudo yum install wget
        cd /home/ec2-user
        wget https://aws-codedeploy-eu-central-1.s3.amazonaws.com/latest/install
        chmod +x ./install
        sudo ./install auto
        sudo service codedeploy-agent status
        #Create your application.zip and load it into CodeDeploy:
        aws deploy create-application --application-name mywebapp
        aws deploy push --application-name mywebapp --s3-location s3://<MY_BUCKET_NAME>/webapp.zip --ignore-hidden-files
        ```

* Continuos Deployment; deploy the code fully automated 
    * **AWS CodePipeline**: E2E solution to build, test and deploy app (CI/CD)
    * **AWS CodeStar**: quickly develop, build and deploy projects on AWS
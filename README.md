# DevOps Project 

## Team 12
#### Members:
+ Ruturaj Vyawahare (rvyawah@ncsu.edu)
+ Avanti Santosh Lagavankar (alagava@ncsu.edu)
+ Sagar Bajaj (sbajaj@ncsu.edu)
+ Charan Ram Vellaiyur Chellaram (cvellai@ncsu.edu)

## Milestone 1
 This milestone demonstrates the use of the Continuos Intergration tool Jenkins and ansible to automate the setup of a simple pipeline for the following two applications:
- A nodejs web application checkbox.io
- An "enterprise" Java system iTrust
 
 The provisioning and configuring of the jenkins server is done on a remote VM using ansible. The build jobs for both applications are setup automatically on jenkins using the jenkins-job-builder and the build jobs are executed using git hooks that are triggered whenever a push is made to their respective repositories. A simple test script is also run in the end after each build to verify whether the proper functionality of the applications remian intact.
   
 ### Getting Started
 **pre-requisites**: AWS account

- Use `git clone https://github.ncsu.edu/alagava/Project-DevOps.git` to clone our project repository.
 
- Go to the proper directory using `cd Project-DevOps/servers/ansible-srv` and run `baker bake` to create the ansible controller VM (ansible-srv).
- SSH into ansible-srv:
 ```
 baker ssh
 ```
And do the following:



#### AWS credential configuration 
 You need to set up your AWS security credentials before you are able
 to connect to AWS. You can do this by creating a file named "credentials" at ~/.aws/ 
 and saving the following lines in the file:

    [default]
    aws_access_key_id = <your access key id>
    aws_secret_access_key = <your secret key>
    
 You can set the default region and output format by creating a file named config at ~/.aws/ and saving the following:

    [default]
    region=<your default region>
    output=<your desired output format>



#### Generate SSH key pair to enable GitHub passwordless authentication 
Generate a SSH key pair with the following command
```
ssh-keygen -t rsa -b 4096 -C "GitHub" -f ~/.ssh/git-key
```
Copy the contents of the public key file `~/.ssh/git-key.pub` and add it to your NCSU github account as done [here](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account).

### Instructions
#### Step 1: Provisioning a remote VM to run the Jenkins build server and to host checkbox and iTrust2 applications.

- Run the `provision_ec2.yml` to create an AWS instance to be run as the remote Jenkins build server. This script will also setup the private key file with restricted permissions and populate the inventory file. This inventory file contains the public IP address of the Jenkins build server and path to the private key file. 

```bash
ansible-playbook /ansible-srv/provision_ec2.yml -i 'localhost'
```
#### Step 2: Setup Jenkins on the remote VM(jenkins-srv)

- Run the `jenkins_init.yml` playbook to install and configure Jenkins on the remote VM.
```bash
ansible-playbook /ansible-srv/jenkins.yml -i /ansible-srv/inventory
```

#### Step 3: Create jobs for building checkbox and itrust2 using jenkins job builder

- Run the `create_jobs.yml` playbook to setup build jobs for checkbox.io and itrust2 on the jenkins server using jenkins-job-builder.

```
bash
ansible-playbook /ansible-srv/create_jobs.yml -i /ansible-srv/inventory
```
- Run the `build_jobs.yml` playbook to copy the `build_checkbox.yml` and `build_itrust.yml` playbooks that perform the actual build of the checkbox.io and itrust2 applications onto the jenkins server.

```
bash
ansible-playbook /ansible-srv/build_jobs.yml -i /ansible-srv/inventory
```

#### Step 4: Setup a git hook to trigger a jenkins build by setting a post receive hook

Create a bare repository production.git for both the applications and add a post-receive git hook. 

- On the Jenkins server, run the following commands to set up post-receive hooks on a localfor checkbox and itrust2

For Checkbox.io:
```bash
mkdir -p latest_checkbox/production.git/
git init --bare latest_checkbox/production.git/
echo "ansible-playbook ~/build_job_checkbox.yml" > latest_checkbox/production.git/hooks/post-receive
chmod 755 latest_checkbox/production.git/hooks/post-receive
```
For iTrust2:
```bash
mkdir -p latest_itrust/production.git/
git init --bare latest_itrust/production.git/
echo "ansible-playbook ~/build_job_itrust.yml" > latest_itrust/production.git/hooks/post-receive
chmod 755 latest_itrust/production.git/hooks/post-receive
```

Setup the local copy of the applications on the jenkins server and add the bare repositories as a remote.

For Checkbox.io:
```
git clone https://github.com/cvellai/checkbox.io.git
cd checkbox.io
git remote add prod <your path>/latest_checkbox/production.git
```
For iTrust2:
```
git clone https://github.ncsu.edu/sbajaj/iTrust2-v4
cd iTrust2-v4
git remote add prod <your path>/latest_itrust/production.git
```

Push some code changes to the bare repository to trigger the jenkin builds.

For both:
```
touch hello.txt
git add .
git status
git commit -m "new changes"
git push prod master
```
#### Step 5: Running test scripts after build  
 For Checkbox.io:
 
 Run npm test to execute the mocha test script.
 
 ```
 cd checkbox.io/server-side/site
 npm test
 ```
 
 For iTrust2:
 
 Run the unit and integrations tests using maven.
 
 ```
 cd iTrust2-v4/iTrust2
 mvn -f pom-data.xml process-test-classes
 mvn jetty:run
 mvn clean test verify checkstyle:checkstyle
 ```
 NOTE: The tests will be run as a part of the build job itself (ONLY for iTrust2).
 
### Contributions

  **Ruturaj** - Automating the creation and provisioning of AWS EC2 instances for the jenkins server and testing iTrust builds  
  **Avanti** - Installing and configuring Jenkins, Checkbox.io and all its dependencies using ansible                          
  **Charan** - Automating build job creation on jenkins, MongoDB configuration and testing of Checkbox.io using mocha test scripts       
  **Sagar** - Setting up iTrust application and its dependencies including MySQL database in an automated manner and helped with installing and configuring Jenkins
 
### Experience Gained and Challenges faced

1. Setting up Jenkins server automatically ran into some issues including password issues, after enabling the groovy script it was completed.
2. iTrust gave address already in use because the server it runs on also uses the port 8080, which is the same as Jenkins. Running jenkins on another port also took a lot of time because it would fail the groovy scripts and those had to be changed as well.
3. Git keys were set up the forked iTrust and checkbox repos along with their config files in order to clone them.
4. iTrust ran into a lot of memory issues and wont run completely on a 1GB aws instance and a larger memory system had to be provisioned.
5. Using older version of Ansible and NodeJs caused problems in running mocha test scripts and jenkins script modules in Ansible and hence,  had to be updated.



 ### Screencast link
 
 The video link for the screencast can be found [here](https://youtu.be/gundMHfOBXw)
 

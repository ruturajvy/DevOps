# Project DevOps 
## Milestone 1
 This milestone demonstrates the use of the Continuos Intergration tool Jenkins and ansible to automate the setup of a simple pipeline for the following two applications:
- A nodejs web application checkbox.io
- An "enterprise" Java system iTrust
 
 The provisioning and configuring of the jenkins server is done on a remote VM using ansible. The build jobs for both applications are setup automatically on jenkins using the jenkins-job-builder and the build jobs are executed using git hooks that are triggered whenever a push is made to their respective repositories. A simple test script is also run in the end after each build to verify whether the proper functionality of the applications remian intact.
 
 ### Getting Started
 prerequuisites
 AWS account
 
 Use `git clone https://github.ncsu.edu/rvyawah/DevOps-Project.git` to clone our project repository.
 
 Go to the proper directory using `cd DeveOps-Project/server/Ansible` and run `baker bake` to create the ansible controller VM (ansible-srv).
 SSH into ansible-srv:
 ```
 baker shh
 ```

 Make a AWS configuration file to store your AWS credentials
 #### AWS credential configuration 
 You need to set up your AWS security credentials before you are able
 to connect to AWS. You can do this by creating a file named "credentials" at ~/.aws/ 
 (`C:\Users\USER_NAME\.aws\` for Windows users) and saving the following lines in the file:

    [default]
    aws_access_key_id = <your access key id>
    aws_secret_access_key = <your secret key>
    
 You can set the default region and output format by creating a file named config at ~/.aws/ and saving the following:

    [default]
    region=<your d# DevOps Project
    ## Milestone 1
 This milestone demonstrates the use of the Continuos Intergration tool Jenkins and ansible to automate the setup of a simple pipeline for the following two applications:
- A nodejs web application checkbox.io
- An "enterprise" Java system iTrust
 
 The provisioning and configuring of the jenkins server is done on a remote VM using ansible. The build jobs for both applications are setup automatically on jenkins using the jenkins-job-builder and the build jobs are executed using git hooks that are triggered whenever a push is made to their respective repositories. A simple test script is also run in the end after each build to verify whether the proper functionality of the applications remian intact.
 
 ### Getting Started
 prerequuisites
 AWS account
 
 Use `git clone https://github.ncsu.edu/rvyawah/DevOps-Project.git` to clone our project repository.
 
 Go to the proper directory using `cd DeveOps-Project/server/Ansible` and run `baker bake` to create the ansible controller VM (ansible-srv).
 SSH into ansible-srv:
 ```
 baker shh
 ```

 Make a AWS configuration file to store your AWS credentials
 #### AWS credential configuration 
 You need to set up your AWS security credentials before you are able
 to connect to AWS. You can do this by creating a file named "credentials" at ~/.aws/ 
 (`C:\Users\USER_NAME\.aws\` for Windows users) and saving the following lines in the file:

    [default]
    aws_access_key_id = <your access key id>
    aws_secret_access_key = <your secret key>
    
 You can set the default region and output format by creating a file named config at ~/.aws/ and saving the following:

    [default]efault region>
    output=<your desired output format>

 Run the `provision script` to create the ansible-srv( which is the ansible controller machine) and the jenkins-srv(on which the apllications and jenkins sever runs).
 
 
 
 
 
 
 
 ### Checkbox.io
 
 Obtain a local clone of the application using `git clone https://github.com/cvellai/checkbox.io.git`

 Make a simple change like `touch demo.txt` inside the repo and push it upstream.
 
 You should be able to observe the build getting executed on the Jenkins UI.
 
 After the build is done, go to `server-side/site/` inside the local checkbox repo.
 
 Run `npm test`. 
 
 ### iTrust

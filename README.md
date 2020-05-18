# CI/CD 
## 1. What is CI/CD? 
CI/CD is famous in software engineering. One may know that CI stands for Common Interface and CD is the good old Compact Disk. But this is not our case. 
> **CI/CD is the acronym for Continuous Integration/Continuous Delivery or, in some document, Continuous Integration/Continuous Development.** 

### 1.1 The first one - CI

>**_CI is the practice of merging all developers' working copy of code into a shared mainline several times a day_**. 

CI does not mean that we do not use *branching feature* of VCS tools. In fact, we can use it, but every branch would have short life - because once a branch is merged to the mainline, it gets removed. 

To ensure code quality and minimize chance of failure, **automation testing** is employed during the process, and there may also be **code review** to improve code quality. CI requires a **culture** of the code team, especially a **coding standard** to reduce conflicting coding style. 

**Automated tests** play an essential role in CI, and they should be carefully designed, frequently update and reviewed. Whenever there's a failure occuring in production, we should add a test case for it to our test suite, and bug fixing should be prioritized over new builds. These tests include uniting tests, integration testing and system testing. 

_Unit testing_ is the most important and should be focused - it that make sure one's changes to a current working function do not affect others. **Unit tests should be frequently updated whenever new changes are made.**. _Integration test_ and _system testing_ are important as well. However, we should keep in mind the difference between testing environment and production environment - these two environments should be as close to each other as possible. Also, **_since we merge code serveral times a day, testing should not take too long_.**

### 1.2 The other one - CD 
> **Continuous delivery (CD)** is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time and, when releasing the software, doing so manually. It aims at building, testing, and releasing software with greater speed and frequency. 

> CD contrasts with **continuous deployment**, a similar approach in which software is also produced in short cycles but through automated deployments rather than manual ones.

Those text from Wikipedia nicely wrap up the definition of Continuous Delivery and Continuous Development. In fact, CI and CD are like brothers that usually go together, creating a DevOps pipeline. CI is in the development/coding phase, and CD is in the production phase. Keep in mind that these phases interleave with each other. 

> ![Image of CI/CD relationship](https://stackify.com/wp-content/uploads/2019/04/big-Feature-Image-on-What-Is-CI_CD.jpg)

For example, a bug is found in production, then we need to work out a plan to fix it, where our CI starts. The task is carried out by a certain developer, on a branch in our repository. After he thinks it's good enough, he executes the pipeline to test, and if all tests are passed, the new changes are automatically accepted to be merged in the mainline branch, namely _'master'_. The newly merged code may be tested and reviewed again. After that, CD comes into play. The new code is built automatically and ready for release. The sysadmin can choose to **automatically deploy** whenever new changes are made so that bugs can be fixed on the fly, that is, he implements **Continuous Development**; otherwise, he can **manually chooses a time to deploy** these changes to production environment, which is **Continuous Delivery**. 

> ![CD pipeline](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Continuous_Delivery_process_diagram.svg/1920px-Continuous_Delivery_process_diagram.svg.png)

Remember that the deployment can be *automated* - the sysadmin chooses a good nice day and clicks one single button, letting automated tasks do their work, saving him from all complicated things other than choosing the right time, which he does *manually*. 

## 2. Why CI/CD? 
- Reduce cost and labor (less sweat and curses) - less time spent on deploying, more time on developing. 
- Reduce risk (with careful designed tests and frequent review), early problem detection. 
- Scalability
- Early and fast hotfixes and updates to users. 
- Easy code update notification to team member --> better team working

CI is best fit for Agile methods. It is the philosophy of 'divide-and-conquer'. Frequent integration and testing make it easier to early point out the bugs lurking in our code. Debugging a small snippet of code is always easier than fixing thousands of lines of code. Also, with VCS (version controls), we can revert buggy changes, as well as finding out what changes are causing problem. 

CD becomes essential as the product scales out. It allows more time for developing by cutting down on the workload of delopying. With microservice architecture - many database servers scatter here and there. and a bunch of microservices that need to be launched/restarted/execute commands in a particular order when deployed, it is almost impossible to manually deploy the product, and even if it is possible, the deployment is super time-consuming and error-proned. 

User experience (UX) is also enhanced, as hotfixes and patches are quickly delivered to users. 

## 3. CI/CD tools 
### 1. VCS:  
A version control tool is a must for CI. Here are some common choices: 
- Github
- Gitlab
- SVN 

A popular pipeline to work with VCS is that we maintain *master* as the branch holding current working version, and *dev* as the branch containing to-be-released code. When we need to develop a new feature, just create a new branch from *dev*, e.g *feature-1*. When the feature is done, merge it back to *dev* and remove it. When we want to release a new version, merge the code in *dev* to *master*. 
### 2. Pipeline tools: 
- Jenkins
- TeamCity
- Travis CI
- Circle CI
- Github Actions
- Gitlab CI
- Bitbucket Pipelines
- Codeship
These tools automates integration and deployment. For example, with Github action, we can run automation test right after *git push*, if no error is found, Github Action can automatically merge to mainline branch, build and even perform automatic deployment. 

### 3. Docker and Virtual Machines: 
It should be noticed that there can be huge different in development environment and production environment. For example, one team member code on MacOS and the Java backend runs just fine, another has the same backend on Windows PC operate without any problem, yet when we deploy the very same backend to a server running CentOS for production, problems constantly show up. Another example is that a an Laptop using Intel CPU runs the Graphic Libaries (GL) just fine, yet another one with AMD architecture encounters crashes. 

Containers and virtual machines come to our rescue. They help reduce the gap between the two environments, and unify development environment among team members.

### 4. Circle CI demo 

We implement a demo on our Capstone project, using Circle CI. 

Circle CI uses the principle of *configuration as code*. We first need to get to know its concepts, and then go on to write a configuration file. 

- **Orb**: CircleCI orbs are shareable packages of configuration elements, including jobs, commands, and executors. We can image that it is like a code libary which functions that can be shared and reused. There are many certified orbs in the orbs registry (https://circleci.com/orbs/registry/) and it is always a good practice trying to use them instead of manually re-create one. 
- **Job**: as the name suggest, a job contains many **steps** that are executed one by one. A job must have an **executor**, independent from other jobs. The **executor** can be *docker*, *macos*, *window* or *machine*. 
- A **step** is a collection of command - shell script to be run by the executor. Since **every run step is a new shell**, environment variables are not shared across steps. If you need an environment variable to be accessible in more than one step, export the value using BASH_ENV. 
- A **workflow** is a set of rules for defining a collection of jobs and their run order. We can configure many workflows to run concurrently to save time, or enforce constraints so that one job must only run once another job is done. 

For example, a simple job of *build-on-docker* may include these step:
- checkout code from Github 
- install dependencies and call docker build by using shell script
- push the docker image to DockerHub

A simple job of *deloy-to-server* may include:  
- Copy the deployment script to server
- Execute the deployment script. 

It can be configured so that *CircleCI* run after code commit or pull request, and hence Continuous Integration/Continuous Development. For more infomation, see the slide https://github.com/nmnhut1998/CI-CD-project/blob/master/Presentation.pptx

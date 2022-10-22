# Task3 solution
#### This file contains all the necessary information’s about my decisions regarding to task3 that is in [instructions.md](instructions.md) file.
#### Answers by ["Mohammad Taghadosi"](https://linkedin.com/in/mtaghadosi)

# Provided solution for task 3:
 We need to have a [Jenkinsfile](/Jenkinsfile) for CI/CD procedures also a Jenkins server, I wrote a pipeline [here](/Jenkinsfile) and included it into the project (I used so many comments as you mentioned that I should). As long as our program need python and fast API some precautions must be considered before we even begin to test and integrate and deploy our code. The best method will be that when a new commit triggered our pipeline, we test the environment first, then test the code and if the code test was successful we can build it and push it to the deployment environment also we can have a staging step to manually verify the deployment result too, or even we can use selenium tools for end-to-end testing after the CI/CD processes. Also another thing to mention is: I am using Jenkins on my windows so the files shoult run on windows based jenkins server or change the codes to adapt it to a linux based Jenkins server.

# As our developer suggest!

 - As the developer team said in the [README.md](/python_service/README.md) we have to ensure that python3, pip and venv installed on our server. so first thing first! I checked for installation of those in fisrt stage called "pre-compile-stage" in my [Jenkinsfile](/Jenkinsfile).
 - After doing that I need to implement and install a database (postgress). [Here I explained the deployment of PorsgresSQL.](/solutions/task2.md) So now we have an up and running DB on port 30002 and also we have our connection-string in the [.env](/python_service/.env) file that is not included in this repository. One thing to mention is that the connection string in the .env file had some misstyping error so I fixed it
# DevOps Exercise

## Background

There are two Git repositoris, one for various components of a browser-based JavaScript UI and one for a REST API written in Python. Both are set up with a GitLab CI/CD pipeline that ultimately leads to a few cloud servers for three environments: development, staging and production. Some of the code is deployed in Docker containers, but not everything (the JavaScript builds f.e. are static files). All of it is served behind nginx.

In terms of processes and organization, we use a Kanban system for development (using the features provided by GitLab). Beyond relatively typical stations for the issues and regular refill and coordination meetings, this includes aspects like code reviews and mandatory checks for the state of the CI before changes are accepted.

## Structure

You can find a sample python service in the `python-service` folder. This is additional material for the tasks below. You can copy the entire folder and insert extra files to clarify your solutions for task 2 and 3 each, but please don't edit any of the original files. The rest of the solutions should remain in the root folder and use the markdown format where possible.

## Tasks

1. The information provided above is neither complete, nor is it meant to be. Use your previous experience and imagination to make an educated guess what the details of the setup might look like and note down your assumptions. If you consider multiple variants to be plausible, you're welcome to mention a few of them, but please decide on a specific scenario in the end. This will be the basis for the next steps.
2. Take a look at the code in the `python-service` folder. Let's say we want to launch another project using this service to integrate our solution with the software of a customer. It's not complete, but the devs will add the rest later ;)
3. Describe how you would deploy the code based on your previous assumptions about our setup. Include relevant details like a Dockerfile or definitions for the CI, relevant tools and commands, as well as explanations for the reasoning behind your choices. Your solution should respect the three environments mentioned above and be able to stand on its own, without relying on the rest of our assumed setup too much.
4. At this point, you'll likely have questions and recommendations regarding the other repositories and deployments to ensure everything works together without a problem. Assume we'll be open to your suggestions and propose an overarching solution if you have an idea. Please separate that clearly from your materials regarding Task 3 as a second step.

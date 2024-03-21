# Continuous Integration and Continuous Deployment

## Introduction
Projects should have built in to them a form of continuous integration to automate testing and deployment. This is a key part of the software development process and is essential for ensuring that code is of high quality and is deployed quickly and efficiently. It is of particular importance in the scientific community where reproducibility is key.

## What is CI/CD?
Continuous Integration (CI) is the practice of automating the integration of code changes from contributors into a single software project. It includes ensuring that people don;t break each other's changes, and that the key requirements are not broken by changes. This is done by automatically building and testing the code whenever a change is made, and providing feedback to the developers.

Continuous Deployment (CD) is the practice of automatically deploying code changes to a production environment whenever they pass the automated tests. This is done to ensure that code changes are deployed quickly and efficiently, and to reduce the risk of errors in the deployment process. This can be automated to a greater or lesser extent depending on the requirements of the project - some projects may require manual approval before deployment, while others may deploy automatically.

## Why use CI/CD?
There are several benefits to using CI/CD in software development:

- Faster feedback: CI/CD provides immediate feedback to developers on the quality of their code, allowing them to fix issues quickly and efficiently.
- Reduced risk: CI/CD automates the testing and deployment process, reducing the risk of human error and ensuring that code changes are deployed consistently.
- Faster deployment: CI/CD automates the deployment process, allowing code changes to be deployed quickly and efficiently.
- Improved quality: CI/CD enforces best practices in software development, ensuring that code changes are of high quality and meet the project's standards.
- Definition of "right" - by using tests to define the requirements of the code you have a definition of the project's requirements that can be tested against.
- Reproducibility - by having a defined set of tests that are run on every change, you can ensure that the code is reproducible and that the tests are run on every change.

## How to use CI/CD at the ICR
At the ICR we use github actions to automate the CI/CD process. This allows us to define workflows that automate the building, testing, and deployment of code changes. For the test framework for a given language click on the langaue Good Practices link in the left hand menu. 

## Github Actions
Github Actions is a feature of Github that allows you to automate the building, testing, and deployment of code changes. It is a powerful tool that can be used to define workflows that automate the entire software development process.

For docs on github actions see the [github actions documentation](https://docs.github.com/en/actions).

For ICR help please contact the schelpdesk@icr.ac.uk.
An example of a project with CICD is SPORANO. 
This has a public website [SOPRANO website](https://software.icr.ac.uk/app/soprano) and a [public github repo](https://github.com/instituteofcancerresearch/SOPRANO/tree/python/.github/workflows) where you can see in the workflows directory there tests, linting, documentation and deployment (to docker) actions.


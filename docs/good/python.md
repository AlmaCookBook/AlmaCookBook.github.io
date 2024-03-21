# Python Good Practises

## Introduction
Python is a powerful and flexible programming language that is widely used in scientific software development. It is known for its simplicity and readability, which makes it an excellent choice for beginners and experienced programmers alike. This document outlines some good practices for developing scientific software in Python.

## Style
Python has a well-defined coding style that is outlined in [PEP 8](https://www.python.org/dev/peps/pep-0008/). Following this style guide will make your code more readable and maintainable, and will help you avoid common pitfalls in Python programming.

Some key points from PEP 8 include:
- Use 4 spaces for indentation
- Limit lines to 79 characters
- Use blank lines to separate functions and classes
- Use descriptive variable and function names
- Use comments to explain complex code

## Documentation
Documentation is an essential part of software development, as it helps other developers understand your code and how to use it. Python has a built-in documentation system called [docstrings](https://www.python.org/dev/peps/pep-0257/), which allows you to write documentation directly in your code.

Some key points for writing good documentation include:
- Use docstrings to document functions, classes, and modules
- Use descriptive names for functions and variables
- Write clear and concise comments

## Testing
Testing is an important part of software development, as it helps you ensure that your code is working correctly and that it continues to work as you make changes. For python we use [pytest](https://docs.pytest.org/en/8.0.x/) and have it built into the continuous integration pipelines - see [CI/CD](cicd)

An important concept in testing is that of the tests for scientific requirements. These are tests that define the project's scientific requirements and key algorithms and form the basis of proof of the scientific software.

Some key points for writing good tests include:
- Write tests for scientific proof of the code
- Write tests for all functions and classes
- Test edge cases and corner cases
- Use descriptive test names
- Build tests into the continuous integration pipeline

## Packaging
Packaging is the process of bundling your code into a distributable format that can be installed and used by others See [packaging and deploying](packaging).

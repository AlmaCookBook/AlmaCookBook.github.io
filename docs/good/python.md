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

Some key points for writing good tests include:
- Write tests for all functions and classes
- Test edge cases and corner cases
- Use descriptive test names
- Build tests into the continuous integration pipeline

## Packaging
Packaging is the process of bundling your code into a distributable format that can be installed and used by others. Python has a built-in packaging system called [setuptools](https://setuptools.pypa.io/en/latest/), which allows you to create packages that can be installed using `pip`.

Some key points for packaging your code include:
- Create a `setup.py` file to define your package
- Use a `requirements.txt` file to list dependencies
- Use a `MANIFEST.in` file to include additional files in your package
- Publish your package to [PyPI](https://pypi.org/) for others to use


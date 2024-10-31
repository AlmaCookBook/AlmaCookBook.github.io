# Conda environment management

A central theme in conda is that of environments. An environment is an object that we create, which enables us to 
"contain" programming languages and any packages that we want to install to use alongside it, in a single place.

Environments can be *created*, *updated* and *removed*. 
We can *activate* and *deactivate* different environments to use with different projects.

***

## 1. Creating a new environment

It is best practice, when we create a new environment, to define the version of our programming language we want 
to use. This is much easier to setup now than change later. 

To create a new environment, run the command

- **Python**
    
  ```shell
  mamba create --name <env name> python=<version>
  ```

- **R**

  ```shell
  mamba create --name <env name> r-base=<version>
  ```

This will create a new environment that has an associated name that you have provided, here represented by `<env name>`, and a
corresponding version, here represented by `<version>`. As a concrete example, we can create an environment called "tmp_python"
using Python 3.11:

```console
mamba create --name tmp_python python=3.11
```

When prompted
```
Confirm changes: [Y/n]
```
write Y and press enter. 

The process will probably take a minute or longer to complete.

## 2. Installing packages into an active environment

After creating an environment, it can be activated via

```console
mamba activate <env name>
```

The lower-left hand corner of your terminal will probably then look like: `(<env name>) [<username>@login(alma) ~]$`.

To install a package we should run the command

```console
mamba install <package name>
```

where `<package name>` is the name of the package we want to install, for example, `numpy`.

Note to R users: All R package names should be preceded by `r-`. For example, `dplyr` should be `r-dplyr`. 

If you require a specific package version, you can instead use the syntax `<package name>=<version>`, for example, `numpy=1.23.5`.

You can now start a console session (by writing `python` or `R` and hitting enter), and check that your package is indeed available.

## 3. Deactivating and removing environments

We can deactivate an active environment by pressing ctrl+d. 

If you wish to remove an environment that you no longer need, first ensure that it has been deactivated, then run the command

```console
mamba env remove <env name>
```
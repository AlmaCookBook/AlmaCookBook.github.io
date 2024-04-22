# Using python scripts on Alma - Environments

Creating an environment with your installations will enable you to use python scripts on Alma in a reproduceable manner. The environment can be shared with colleagues, help desk, and collaborators to ensure that the same environment is used for the same results. It also means that in the future you can recreate the environment to run the same scripts.

1. First get an interactive session:
```
ssh username@alma.icr.ac.uk
srun --pty -t 12:00:00 -p interactive bash
```

2. Check that mamba is working
```shell
mamba --version
# which should return:
mamba 1.4.0
conda 23.1.0
```
If not make sure you have followed the [setup mamba instructions](mamba-first.md)

3. Create a new environment
```shell
mamba create --name my-env-name python=3.10
mamba activate my-env-name
# locally as an alternative
mamba create --prefix ./my-env-name python=3.10
mamba activate ./my-env-name
```

4. Check you have the environment you expect
```shell
python --version
```

5. Installing packages
It is preferable to use mamba to install python packages like matplotlib etc
```
mamba install matplotlib
```

6. If you need to use a python script, you can use the python command
```shell
pip install matplotlib
```
But it is better to use mamba to install packages where possible.

7. Export and import the environment
```shell
# exporting the environment
mamba env export --name my-env-name > my-env-name.yml
pip freeze > my-env-name.txt

# Loading the environment
mamba env create --file my-env-name.yml
pip install -r my-env-name.txt
```

8. Deactivate the environment
```shell
mamba deactivate
```

9. Remove the environment
Warning this means to remove all the packages and the environment, only do this if you really want to.
```shell
mamba remove --name my-env-name --all
```

10. Check what environments I have available
```shell
mamba env list
```



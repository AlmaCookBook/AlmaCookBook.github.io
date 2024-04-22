# Using python scripts on Alma - python venv

Creating an environment with your installations will enable you to use python scripts on Alma in a reproduceable manner. The environment can be shared with colleagues, help desk, and collaborators to ensure that the same environment is used for the same results. It also means that in the future you can recreate the environment to run the same scripts.

An alternative to conda/mamba is the python virtual environment. This is lighter and simpler than mamba, but not as powerful - it does not have the ability to also install some of the bioinformtics tools available on conda-forge but is limitted to python librraries on pypi. Nevertheless, it is a good option for simple python scripts.

1. First get an interactive session:
```
ssh username@alma.icr.ac.uk
srun --pty -t 12:00:00 -p interactive bash
```

2. Check the version of python
```shell
python --version
```

3. Create a new environment
```shell
python -m venv my-env-name
source my-env-name/bin/activate
```

4. Installing packages
You can pip install the packages, optionally with version (=1.3.2).
```shell
pip install matplotlib
```

5. Installing packages from a requirements file
The pacakges can be listed in a simple text file - a requirements file. This can be installed with pip. This is an approximation of a reproduceable envirnment but may not cover versions and dependencies.
```shell
pip install -r requirements.txt
```

6. Deactivate the environment
```shell
deactivate
```

7. Remove the environment
This is installed in the directory, so can be removed with `rm -rf my-env-name`. 


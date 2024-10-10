# Using Jupyter OnDemand on Alma - Environments

Creating an environment with your installations will enable you to use Jupyter notebooks on Alma in a reproduceable manner. The environment can be shared with colleagues, help desk, and collaborators to ensure that the same environment is used for the same results. It also means that in the future you can recreate the environment to run the same notebooks.

You will first need to create the environment on the commandline and activate it as a kernel for Jupyter.

1. First get an interactive session:
```
ssh username@alma.icr.ac.uk
srun --pty -t 12:00:00 -p interactive bash
mamba --version
```

2. Follow the steps for creating a new environment in the [mamba documentation](python-scripts.md) and activate it.

3. Install the ipykernel package
```shell
mamba install -c conda-forge ipykernel
```

4. Activate the kernel with python
```shell
python -m ipykernel install --user --name=my-env-name
```

5. Start Jupyter OnDemand

The dropdown for the kernels will now include this environment:
![alt text](../assets/image.png)



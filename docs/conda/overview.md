# Conda

[Conda](https://conda.org/) is an open-source package and environment management tool that is available to use on Alma. 
It is language agnostic, meaning that it is a tool that can be used to manage a variety of languages; including Python, R and Perl, for example.

There are multiple implementations of conda with slight differences. The base implementation of the software is known as **Anaconda**, 
though there is a highly efficient alternative known as **Mamba** which can offer orders of magnitude speed up when resolving complex environments.

*Alma users are encouraged to use Mamba, rather than Anaconda when they create environments and install packages for this reason.*

***

# How to setup conda

## 1. Load the `Mamba` module

In order to use `conda`, users should add the EasyBuild modules to their user path and load the `Mamba` module:

```console
module use /opt/software/easybuild/modules/all
module load Mamba
```

You can check that the module has been loaded successfully by running `module list`, which should return the output:

```
Currently Loaded Modules:
  1) slurm-21.08.5   2) Mamba/23.1.0-0
```

in addition to any other modules you may have already loaded. You can further check that the `mamba` and `conda` commands are now
available for you to use, via `mamba --version`, which should return the output:

```
mamba 1.4.0
conda 23.1.0
```

## 2. Initialize your shell

After the module has been loaded, you should now initialize your shell. The easiest way to achieve this, is to run

```console
mamba init
```

This should generate the output:

```
                  __    __    __    __
                 /  \  /  \  /  \  /  \
                /    \/    \/    \/    \
███████████████/  /██/  /██/  /██/  /████████████████████████
              /  / \   / \   / \   / \  \____
             /  /   \_/   \_/   \_/   \    o \__,
            / _/                       \_____/  `
            |/
        ███╗   ███╗ █████╗ ███╗   ███╗██████╗  █████╗
        ████╗ ████║██╔══██╗████╗ ████║██╔══██╗██╔══██╗
        ██╔████╔██║███████║██╔████╔██║██████╔╝███████║
        ██║╚██╔╝██║██╔══██║██║╚██╔╝██║██╔══██╗██╔══██║
        ██║ ╚═╝ ██║██║  ██║██║ ╚═╝ ██║██████╔╝██║  ██║
        ╚═╝     ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═════╝ ╚═╝  ╚═╝

        mamba (1.4.0) supported by @QuantStack

        GitHub:  https://github.com/mamba-org/mamba
        Twitter: https://twitter.com/QuantStack

█████████████████████████████████████████████████████████████

no change     /opt/software/easybuild/software/Mamba/23.1.0-0/condabin/conda
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/bin/conda
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/bin/conda-env
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/bin/activate
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/bin/deactivate
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/etc/profile.d/conda.sh
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/etc/fish/conf.d/conda.fish
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/shell/condabin/Conda.psm1
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/shell/condabin/conda-hook.ps1
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/lib/python3.10/site-packages/xontrib/conda.xsh
no change     /opt/software/easybuild/software/Mamba/23.1.0-0/etc/profile.d/conda.csh
modified      /home/<username>/.bashrc

==> For changes to take effect, close and re-open your current shell. <==

Added mamba to /home/<username>/.bashrc

==> For changes to take effect, close and re-open your current shell. <==
```

It is important that we pay attention to the last part of this output: **Close your Alma terminal and open it up again.**

## 3. Using conda post-initialization

After you have started a fresh terminal session, **you do not need to follow the previous steps again**. Check that you can run

```console
mamba --version
```

which as before, should now return:

```
mamba 1.4.0
conda 23.1.0
```

You should now be ready to use conda.

***

# How to use conda

- Creating a new environment
- ...
# Running Matlab code from Python on Alma
`Maintained by: Rachel Alcraft. Last updated: 31st July 2024`

Running MatLab code from python requires the installation of an additional package called `matlab.engine`. This package is installed using pip system into a mamba environment.

We need to control the version to be compatible with Alma - pythons 3.9, 3.10 and 3.11 are compatible with the latest Alma installation of MatLab R2023b, and compatible with matlabengine==23.2.1

If you want to use an earlier python version, you will need to install an earlier version of the MatLab engine.
For compatibility with python 3.8 (eg ig=f you are using a cuda gpu) you will need to install matlabengine==9.14.7


## Get setup on Alma
### Log onto an interactive session on Alma
```shell
ssh username@alma.icr.ac.uk
srun --pty --mem=10GB -c 1 -t 30:00:00 -p interactive bash
```

### Create a mamba environment to work in
```shell
mamba create -n my-matlab python=3.11
mamba activate my-matlab
```

### add the library path
```shell
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/software/applications/MATLAB/R2023b/bin/glnxa64
```

### Install the MatLab engine
```shell
python -m pip install --upgrade pip
python -m pip install matlabengine==23.2.1
```

### Check that the required python and MatLab versions were correctly installed
```shell
which python
python --version
python -m pip show matlabengine
python -c "import sys; print('\n'.join(sys.path))"
```
## Start the MatLab engine and test it
### Open the python shell
```shell
python
```

### Run the following: paste it in line by line
```python
print("Starting MatLab Test")
import matlab.engine
eng = matlab.engine.start_matlab()
x = 4.0
eng.workspace['y'] = x
a = eng.eval('sqrt(y)')
print("If a == 2.0 then it works!")
print("a =",a)
```
You can now create scripts that use MatLab and run on Alma when using this mamba environment.
Further resources for the installation and use of the MatLab engine can be found below.

## Resources
### Installation
- [https://pypi.org/project/matlabengine/23.2.1/](https://pypi.org/project/matlabengine/23.2.1/)  
- [https://uk.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html](https://uk.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html)  

### Use
- [https://uk.mathworks.com/help/matlab/matlab_external/get-started-with-matlab-engine-for-python.html](https://uk.mathworks.com/help/matlab/matlab_external/get-started-with-matlab-engine-for-python.html)
- [https://uk.mathworks.com/help/matlab/matlab_external/start-the-matlab-engine-for-python.html](https://uk.mathworks.com/help/matlab/matlab_external/start-the-matlab-engine-for-python.html)
- [https://uk.mathworks.com/help/matlab/matlab_external/use-the-matlab-engine-workspace-in-python.html](https://uk.mathworks.com/help/matlab/matlab_external/use-the-matlab-engine-workspace-in-python.html)

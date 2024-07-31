# Running Matlab code from Python on Alma

Running MatLab code from python requires the installation of an additional package called `matlab.engine`. This package is not available in the default conda channels, so you will need to install it from the MathWorks channel.

Older versions of matlab required an installation from within a matlab installation directory on Alma, but newer versions do not require this. We need to control the version to be compaptible with Alma - pythons 3.9, 3.10 and 3.11 are compatible with R2023b for example and compatible with maatlabengine==23.2.1

## Log onto an interactive sessiopn on Alma
```shell
ssh username@alma.icr.ac.uk
srun --pty --mem=10GB -c 1 -t 30:00:00 -p interactive bash
```

## Create a mamba environment to work in
```shell
mamba create -n my-matlab python=3.11
mamba activate my-matlab
```

## add the library path
```shell
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/software/applications/MATLAB/R2023b/bin/glnxa64
```

## Install the matlab engine
```shell
python -m pip install --upgrade pip
python -m pip install .
python -m pip install matlabengine==23.2.1
```

## Check you have the versions of python and matlab you think you have
```shell
which python
python --version
python -m pip show matlabengine
python -c "import sys; print('\n'.join(sys.path))"
```
## Start the matlab engine and test it
### Open the python shell
```shell
python
```

### Run the following paste it in line by line
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
You can now create scripts that use matlab and run on alma when using this mamba environment.
Further resources for the installation and use of the matlab engine can be found below.

## Resources
### Installation
https://pypi.org/project/matlabengine/23.2.1/
https://uk.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html
### Use
https://uk.mathworks.com/help/matlab/matlab_external/get-started-with-matlab-engine-for-python.html
https://uk.mathworks.com/help/matlab/matlab_external/start-the-matlab-engine-for-python.html
https://uk.mathworks.com/help/matlab/matlab_external/use-the-matlab-engine-workspace-in-python.html

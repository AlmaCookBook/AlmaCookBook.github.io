# A Basic Loading of a MuSpAn Dataset
In order to use MuSpAn you need to secure a license.  
Request the code from: https://www.muspan.co.uk/get-the-code and you will receieve an 
email with the username and password needed for the install.


We recommend using conda or mamba to install the muspan package. 
The following will create a new conda environment called `muspan-env` and install 
the package in an environment you can use in a jupyter notebook:

```bash
conda create -y -n muspan-env -c conda-forge python=3.12 
conda activate muspan-env
python -m pip install --upgrade pip
python -m pip install https://docs.muspan.co.uk/code/latest.zip
# follow the username and password prompts
conda install jupyterlab ipykernel ipywidgets
python -m ipykernel install --user --name python_muspan --display-name "Python Muspan 2025"
```

In python verify you can access the MuSpAn package and a sample dataset:

```python
import pandas as pd
import os
import muspan as ms
file_path = os.path.dirname(ms.datasets.__file__) + '/data/Bull_2024_mouse_colon.csv'
print(file_path)
df = pd.read_csv(file_path)
print(df.head())
```
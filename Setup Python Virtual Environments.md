This guide explains how to set up a Conda environment named `VE`, including optional installations for PyTorch and BACCO. Each step includes explanations of the purpose of the packages.

---
## Step 1: Create the Conda Environment
```bash
# Update Conda to the latest version for stability and compatibility
conda update -n base -c defaults conda

# Create a new Conda environment named 'VE' with Python 3.9 for compatibility
conda create --name VE python=3.9 -y

# Activate the environment
conda activate VE
```
---
## Step 2: Install Core Packages
Install the required packages for scientific computing, data analysis, and debugging.
```bash
# Add the conda-forge channel for access to updated, compatible packages
conda config --add channels conda-forge
conda config --set channel_priority strict

# Install scientific libraries and tools
conda install numpy scipy astropy cython h5py matplotlib pandas pytest -y

# Install iminuit for numerical minimization and ipdb for debugging
conda install iminuit ipdb -y

# install jupyter notebook packages
pip install notebook jupyterlab ipykernel

# Register your virtual environment (in this case, VE) as a Jupyter kernel. This allows tools like Jupyter Notebook, JupyterLab, and VS Code to recognize and use that environment to execute Python code.
python -m ipykernel install --user --name=VE --display-name "Python (VE)"```
### Explanation of Installed Packages
- **numpy**: For numerical computations and array operations.
- **scipy**: Provides advanced scientific and engineering tools.
- **astropy**: A library for astronomy and astrophysics.
- **cython**: Optimizes Python code by compiling it into C for improved performance.
- **h5py**: Handles HDF5 file formats for large datasets.
- **matplotlib**: For creating visualizations and plots.
- **pandas**: Used for data manipulation and analysis with dataframes.
- **pytest**: A framework for writing and running tests in Python.
- **iminuit**: A library for minimization problems, often used in statistical modeling.
- **ipdb**: An interactive debugger for Python scripts.

| **Package**  | **Purpose**                                                        | **Interface**            | **Notes**                                                                                    |
| ------------ | ------------------------------------------------------------------ | ------------------------ | -------------------------------------------------------------------------------------------- |
| `notebook`   | Provides the server and the classic Jupyter Notebook interface.    | Classic (browser-based). | Lightweight, a good option if you only need simple notebooks.                                |
| `jupyterlab` | Provides a modern and advanced interface with additional features. | Modern (browser-based).  | Ideal for complex projects, a replacement for the classic Jupyter Notebook.                  |
| `ipykernel`  | Allows Python to be used as a kernel to execute code in notebooks. | N/A (it's a backend).    | Necessary if using Python with Jupyter; enables registering virtual environments as kernels. |

---
## Step 3: Install Optional Packages

### Option 1: Install PyTorch
PyTorch is a popular deep learning framework, but it is optional if you are not working on machine learning projects.
```bash
# Install PyTorch with CPU support (default option for compatibility)
conda install pytorch torchvision torchaudio cpuonly -c pytorch -y
```
If you have a GPU and want to enable GPU acceleration, replace `cpuonly` with your CUDA version (e.g., 11.8):
```bash
# Install PyTorch with GPU support
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```
---
### Option 2: Install BACCO
**BACCO** is a library for cosmological emulators, useful in research for fast interpolation of theoretical predictions.
```bash
# Clone the BACCO repository and install it in editable mode
git clone https://DanielLopezCano@bitbucket.org/rangulo/baccogit.git
cd ./baccogit
pip install -e .
```
**Optional: Install CLASS**
Some BACCO functionalities require the **CLASS** cosmology code for Boltzmann equation solutions.
```bash
# Navigate to the directory where you want to install CLASS
cd /lscratch/dlopez/programs

# Clone the CLASS repository
git clone https://github.com/lesgourg/class_public.git class

# Compile CLASS
cd class
make
```
### Explanation of BACCO and CLASS
- **BACCO**: Provides tools for emulating cosmological data, making it faster to obtain predictions compared to running full simulations.
- **CLASS**: A Boltzmann solver used in cosmology to compute power spectra and other theoretical predictions.
---
## Step 4: Verify the Environment
Run the following script to check different libs and packages: [[validate_environment.py]]

---
## Step 5: Save and Share the Environment
```bash
# Export the environment to a YAML file
conda env export > VE.yml

# Recreate the environment later or on another machine
conda env create -f VE.yml
```
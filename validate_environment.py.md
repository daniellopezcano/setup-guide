
The `validate_environment.py` script outputs `[PASS]` or `[FAIL]` messages for each test, helping you quickly identify any missing or misconfigured components.

```python
import sys
import subprocess

def validate_import(module_name):
    """Check if a module can be imported."""
    try:
        __import__(module_name)
        print(f"[PASS] {module_name} imported successfully.")
    except ImportError:
        print(f"[FAIL] {module_name} failed to import.")

def test_matplotlib_plot():
    """Test Matplotlib by creating a simple plot."""
    try:
        import matplotlib.pyplot as plt
        plt.plot([0, 1], [0, 1])
        plt.title("Test Plot")
        plt.show(block=False)
        plt.close()
        print("[PASS] Matplotlib plot test succeeded.")
    except Exception as e:
        print(f"[FAIL] Matplotlib plot test failed: {e}")

def test_jupyter_notebook():
    """Test if Jupyter Notebook is installed and functional."""
    try:
        subprocess.run(["jupyter", "notebook", "--version"], check=True, stdout=subprocess.PIPE)
        print("[PASS] Jupyter Notebook command works.")
    except FileNotFoundError:
        print("[FAIL] Jupyter is not installed or not in PATH.")
    except subprocess.CalledProcessError:
        print("[FAIL] Jupyter Notebook command failed.")

def test_pytorch():
    """Test PyTorch installation and CUDA support."""
    try:
        import torch
        print(f"[PASS] PyTorch version: {torch.__version__}")
        print(f"[PASS] CUDA available: {torch.cuda.is_available()}")
        if torch.cuda.is_available():
            print(f"[PASS] CUDA device: {torch.cuda.get_device_name(0)}")
    except ImportError:
        print("[FAIL] PyTorch failed to import.")
    except Exception as e:
        print(f"[FAIL] PyTorch test failed: {e}")

def test_bacco():
    """Test BACCO installation."""
    try:
        import bacco
        print("[PASS] BACCO imported successfully.")
    except ImportError:
        print("[FAIL] BACCO failed to import.")

def main():
    print("Starting environment validation...\n")

    # Validate imports for all core packages
    core_packages = [
        "numpy", "scipy", "astropy", "cython", "h5py",
        "matplotlib", "pandas", "pytest", "iminuit", "ipdb",
        "notebook", "jupyterlab", "ipykernel"
    ]
    for module in core_packages:
        validate_import(module)

    # Test Matplotlib plotting
    test_matplotlib_plot()

    # Test Jupyter Notebook command
    test_jupyter_notebook()

    # Test PyTorch installation and functionality
    test_pytorch()

    # Test BACCO installation
    test_bacco()

    print("\nEnvironment validation complete.")

if __name__ == "__main__":
    main()
```

**Run the Validation Script with the virtual environment activated**:
```bash
(VE) -bash-4.2$ python validate_environment.py
```

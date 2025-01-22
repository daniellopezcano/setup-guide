(https://docs.conda.io/projects/miniconda/en/latest/)
Miniconda is a lightweight Python distribution that includes `conda`, a powerful environment and package manager. Follow these steps to install it:
```bash
# Navigate to the desired directory
cd ~

# Create a directory for Miniconda
mkdir -p ~/miniconda3

# Download the latest installer
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh

# Run the installer
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3

# Remove the installer to save space
rm -rf ~/miniconda3/miniconda.sh

# After installing, close and reopen your terminal application or refresh it by running the following command:
source ~/miniconda3/bin/activate

# To initialize conda on all available shells, run the following command:
conda init --all
```
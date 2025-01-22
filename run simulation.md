```
# ----------------------------------------------------------- #
# ---------------- Modules to compile gadget ---------------- #
# ----------------------------------------------------------- #

Create a source file containing the following lines:

'''
module purge
export SYSTYPE="atlas_248"
export OMP_NUM_THREADS=1

module load OpenMPI/4.0.3-GCC-9.3.0
module load FFTW/3.3.8-gompi-2020a
module load GSL/2.6-GCC-9.3.0
module load HDF5/1.10.6-gompi-2020a
module load CMake/.3.16.4-GCCcore-9.3.0
module load OpenBLAS/0.3.9-GCC-9.3.0

#GSL_HOME  = /scicomp/EasyBuild/CentOS/7.5.1804/Skylake/software/GSL/2.5-GCC-7.3.0-2.30
#FFTW_HOME = /scicomp/EasyBuild/CentOS/7.5.1804/Skylake/software/FFTW/3.3.8-intel-2018b/
export MPI_HOME=/scicomp/EasyBuild/CentOS/7.5.1804/Skylake/software/OpenMPI/3.1.1-GCC-7.3.0-2.30
export HDF5_HOME=/scicomp/EasyBuild/CentOS/7.5.1804/Skylake/software/HDF5/1.10.2-foss-2018b
'''

# ----------------------------------------------------------- #
# ----------- Create a makefile.path for atlas_248 ---------- #
# ----------------------------------------------------------- #

Create ./l-gadget3/sysfiles/Makefile.path.atlas_248 including:

'''
GSL_INCL   =  -I$(GSL_HOME)/include
GSL_LIBS   =  -L$(GSL_HOME)/lib  -Xlinker -R -Xlinker $(GSL_HOME)/lib
FFTW_INCL  =  -I$(FFTW_HOME)/include
FFTW_LIBS  =  -L$(FFTW_HOME)/lib  -Xlinker -R -Xlinker $(FFTW_HOME)/lib
HDF5_INCL  =  -I$(HDF5_HOME)/include
HDF5_LIBS  =  -L$(HDF5_HOME)/lib -Xlinker -R -Xlinker $(HDF5_HOME)/lib
HWLOC_INCL =
HWLOC_LIBS =
MAPS_INCL  =  -I/u/vrs/Libs/include
MAPS_LIBS  =  -L/u/vrs/Libs/lib  -Xlinker -R -Xlinker /u/vrs/Libs/lib
'''

# ----------------------------------------------------------- #
# ---------------------- Modify Makefile -------------------- #
# ----------------------------------------------------------- #

Modify ./l-gadget3/Makefile and include:

'''
ifeq ($(SYSTYPE),"atlas_248")
include systems/Makefile.comp.dipc
include systems/Makefile.path.atlas_248
endif
'''

# ----------------------------------------------------------- #
# --------------- Create a Makefile.systype ----------------- #
# ----------------------------------------------------------- #

Create ./l-gadget3/Makefile.systype and include:

'''
SYSTYPE="atlas_248"
'''
```


```shell
#!/bin/bash
### sed -i -e 's/\r$//' ./test_simulations/Planck13_N256_L50.0_output/job.slurm

###  ----------------- Run in the atlas-248 ----------------- ###
### sed -i -e 's/\r$//' /scratch/dlopez/Projects/ML_simulations/source.src
### source /scratch/dlopez/Projects/ML_simulations/source.src
### nice -n 5 mpirun -np 10 /scratch/dlopez/Projects/ML_simulations/l-gadget3_node/L-Gadget3 ./LG3_param.txt  2>&1 | tee ./256A.txt

###  ----------------- slurm utils ----------------- ###
### sbatch ./test_simulations/Planck13_N256_L50.0_output/job.slurm
### squeue = what's the queue
### squeue -u dlopez = show only your jobs
### scancel jobID = stop one job and remove it from the queue
### showq = another way of seeing the jobs in the queue

#SBATCH --partition=regular
#SBATCH --job-name=LG3
#SBATCH --cpus-per-task=1
#SBATCH --mem=370gb
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=48

module purge
module load iimpi/2020a
module load FFTW/3.3.8-intel-2020a
module load GSL/2.6-GCC-8.3.0
module load HDF5/1.10.6-iimpi-2020a
mpirun -np $SLURM_NTASKS /scratch/dlopez/Projects/ML_simulations/l-gadget3/L-Gadget3 /scratch/dlopez/Projects/ML_simulations/test_simulations/Planck13_N256_L50.0_output/LG3_param.txt
```
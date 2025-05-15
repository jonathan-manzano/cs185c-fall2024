# Issues

## `CondaError: Use conda init before conda activate`

### MITgcm Message

Not available.

### Issue Remedy

Adding the following line will help `slurm` initialize `conda` for the session and resolves the `CondaError`.

`source $(conda info --base)/etc/profile.d/conda.sh`

Add this line right after the `slurm` parameters but the first line of the main script.

```shell
#!/bin/bash
#SBATCH --partition=nodes
#SBATCH --nodes=1
#SBATCH --ntasks=2
#SBATCH --time=10:00:00

# Prepend this before the other commands
source $(conda info --base)/etc/profile.d/conda.sh

conda activate base
conda activate --stack cs185c
export MPI_HOME=/home/[username]/.conda/envs/cs185c
mpirun -np 24 ./mitgcmuv
```
---

## `PARAM0# not in namelist`

- This error can be a result of many moving parts if tracking becomes cumbersome.
In my situation the `aqhtemp` file `AQH_2009` was misspelled as `AQG_2009`
- This happened again but `VVEL_IC.bin` symlink was missing from the run directory.

### MITgcm Message

STDERR.00**:

````shell
*something along the lines of a certain file, parameter, etc is missing in the namelist and highly likely the `data` file is ill formated*
````

### Issue Remedy

- Correct the spelling: `AQG_2009` --> `AQH_2009`
- You can either create a new symlink: `ln -s ../input/VVEL_IC.bin`, or

```shell
$ cd configurations
$ rm -rf ./run/*
$ cd ./run
$ ln -s ../namelist/* .
$ ln -s ../input/* .
$ ln -s ../build/mitgcm .
```

## `STOP ABNORMAL END: S/R INI_THETA`x3

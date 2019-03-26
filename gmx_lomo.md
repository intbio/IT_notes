# Quick instructions on running Gromacs on Lomonosov1/2 supercomputers

## Running gromacs on lomonosov
* [suggested reading](http://manual.gromacs.org/documentation/2018.3/user-guide/mdrun-performance.html)
* Load modules
```
module load slurm gromacs/2018-gcc
```
* Prepare script
```
#!/bin/bash -l
#SBATCH -t 7-00:00:00
#SBATCH -p compute
#SBATCH -J jobname
#SBATCH -o ogmx.%j
#SBATCH -e egmx.%j
#SBATCH -N 10
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=6

# N - number of nodes, 
# --ntasks-per-node - amount of MPI tasks to run on one node
# --cpus-per-task - amount of MP threads per one MPI task
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
mpitasks=$(($SLURM_JOB_NUM_NODES*$SLURM_NTASKS_PER_NODE))


mpirun -np $mpitasks gmx_mpi mdrun -ntomp $OMP_NUM_THREADS -gputasks 00 -pme cpu -nb gpu -deffnm $1

#Script should be run with 
#module load slurm gromacs/2018-gcc
#To override
#sbatch -p partition_name -t hours:minutes:seconds -N num_nodes runscript.sh your_tpr_file_name
```
* Run it
```
sbatch runscript.sh sonename
```

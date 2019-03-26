### To run gromacs with plumed on lomonosov
add environment variable
`export MODULEPATH=$MODULEPATH:/home/kniazevaanastasiia2015_1609/_scratch/_software/modules`
Then load module 
`module load gromacs_2018.4_plumed`
and run gromacs with script 
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


mpirun -np $mpitasks gmx_plumed mdrun -ntomp $OMP_NUM_THREADS -plumed plumed.dat -deffnm $1

#Script should be run with 
#module load slurm gromacs/2018-gcc
#To override
#sbatch -p partition_name -t hours:minutes:seconds -N num_nodes runscript.sh your_tpr_file_name
```

Run it
sbatch runscript.sh sonename

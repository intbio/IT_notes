## Cluster config
### There are 3 kinds of nodes
1. **New nodes with GPU.** 40 CPU cores, 128 GB of RAM, 2x nvidia 2080Ti GPUS (node_11, node_12)
2. **Old nodes with GPU.** 16 CPU cores, 12 GB of RAM, 2x nvidia GTX970 GPUS (node_07,node_08,node_09, node_10)
2. **Old nodes without GPU.** 16 CPU cores, 12 GB of RAM (node_01- node_06)
### All nodes are in two *PARTITIONS*
1. GPU - contains New nodes
2. CPU - contains Old nodex

## Commands
`squeue` - show job status where you can monitor the state (ST) of your job.

Sample output, more info `man squeue`
```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
              1052       cpu Nucl+Snf g_vykhod  R 4-21:51:26      1 node_09
              1068       gpu tandem_c   armeev  R   21:31:39      1 node_11
```
`sinfo` - cluster state.
* Down - node is down due to reasons (mainly software failures and maintenence)
* idle - nodes is free to use
* mix - node is used, but there are resources left

Sample output, more info `man sinfo`
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
cpu*         up   infinite      1  down* node_10
cpu*         up   infinite      1    mix node_09
cpu*         up   infinite      8   idle node_[01-08]
gpu          up   infinite      1    mix node_11
gpu          up   infinite      1   idle node_12
```

## RUN NEW JOB
`sbatch` is the command to use with special run script

Sample run script (**should be executable!** run chmod u+x scriptname.sh)

samplescript.sh
```
#!/bin/bash -l
#SBATCH -t 7-00:00:00
#SBATCH -p cpu
#SBATCH -J jobname
#SBATCH -o slurmout.%j
#SBATCH -e slurmerr.%j
#SBATCH -N 1
#SBATCH --gres=gpu:1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=10

# N - number of nodes, 
# --ntasks-per-node - amount of MPI tasks to run on one node
# --cpus-per-task - amount of MP threads per one MPI task
# -gres=gpu:1 - amount of GPUs to use
# -t time you anticipate to run the job

#if you use mpi:
#mpirun YOUR_SCRIPT_OR_PROGRAMM_HERE
#otherwise just
#YOUR_SCRIPT_OR_PROGRAMM_HERE

#example run foldx
# !!! BE AWARE that you might have to setup your environment
module load conda3
source activate moldyn

foldx --command=Pssm --analyseComplexChains=V --pdb=4X23_Repair.pdb --positions=RV719a,LV720a,KV721a,PV722a,LV723a,EV724a,YV725a,WV726a,RV727a,GV728a,EV729a
```

* to run execute `sbatch samplescript.sh`
* monitor your job with `squeue`
* errors and output will be stored in files slurmout.*JOBNUMBER*


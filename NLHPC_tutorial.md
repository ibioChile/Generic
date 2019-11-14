** Access server:

Access the server using your credentials.

```ssh pcamejo@leftraru.nlhpc.cl```

Enter initial password (found on email sent by NLHPC support). 

Change password if it is the first time you access the server.

```pcamejo@leftraru2:~$ passwd```

If you need to access a specific node, specify the node name when accessing the server.

```ssh pcamejo@leftraru2.nlhpc.cl```

More information [here](http://usuarios.nlhpc.cl/index.php/Tutorial_de_acceso_a_Leftraru_via_SSH)

** Example 1: Create conda environment and run job.

Load Anaconda module (this module is already found on SLURM).

```pcamejo@leftraru2:~$ ml load Anaconda3/5.3.0```
```pcamejo@leftraru2:~$ conda create --name example_bot```
```pcamejo@leftraru2:~$ conda init``` # (you may need to exit the server and login again)
```pcamejo@leftraru2:~$ conda activate example_bot```
```pcamejo@leftraru2:~$ conda install -c bioconda bowtie```

Now create job file

```pcamejo@leftraru2:~$ mkdir botrytis_example```
```pcamejo@leftraru2:~$ cd botrytis_example```
```pcamejo@leftraru2:~/botrytis_example$ nano bowtie.sh```

```
#!/bin/bash
#SBATCH --job-name=example
#SBATCH --partition=slims
#SBATCH -n 10
#SBATCH --output=example_bowtie.out
#SBATCH --error=example_bowtie.err
#SBATCH --mail-user=user@gmail.com
#SBATCH --mail-type=ALL

ml load Anaconda3/5.3.0

#conda init bash (optional)
conda activate example_bot

bowtie-build GCF_000143535.2_ASM14353v4_genomic.fna GCF_000143535.2_ASM14353v4_genomic
bowtie -S GCF_000143535.2_ASM14353v4_genomic -1 b23_1.short.fastq -2 b23_2.short.fastq > b23.short.sam
```
Run job:

```pcamejo@leftraru2:~/botrytis_example$ sbatch bowtie.sh```


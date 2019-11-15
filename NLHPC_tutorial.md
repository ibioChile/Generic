# Access server:

Access the server using your credentials.

```ssh pcamejo@leftraru.nlhpc.cl```

Enter initial password (found on email sent by NLHPC support). 

Change password if it is the first time you access the server.

```pcamejo@leftraru2:~$ passwd```

If you need to access a specific node, specify the node name when accessing the server.

```ssh pcamejo@leftraru2.nlhpc.cl```

More information [here](http://usuarios.nlhpc.cl/index.php/Tutorial_de_acceso_a_Leftraru_via_SSH)

## Example 1: Create conda environment and run job.

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

To check the job status:

```pcamejo@leftraru2:~/botrytis_example$ squeue```


## Example 2: Install program locally

I recommend to create a folder where all local programs will be stored:

```pcamejo@leftraru2:~$ mkdir bin```

```pcamejo@leftraru2:~$ cd bin```

```pcamejo@leftraru3:~/bin$ wget https://sourceforge.net/projects/bowtie-bio/files/bowtie/1.2.3/bowtie-1.2.3-linux-x86_64.zip/download```

```pcamejo@leftraru3:~/bin$ mv download bowtie-1.2.3-linux-x86_64.zip```

```pcamejo@leftraru3:~/bin$ unzip bowtie-1.2.3-linux-x86_64.zip```

```pcamejo@leftraru3:~/bin$ cd bowtie-1.2.3-linux-x86_64```

The binaries of this program are ready to run, but consider that many times, you will have to configure and make some programs. In each case look for the information about how to install a program locally (without sudo permissions).

You can store the path of this binaries on your .bash_profile file:

```pcamejo@leftraru3:~/bin/bowtie-1.2.3-linux-x86_64$ cd ~```

```pcamejo@leftraru3:~$ nano .bash_profile```

Add this line with bowtie path to file:

```export PATH="/home/pcamejo/bin/bowtie-1.2.3-linux-x86_64:$PATH"```

Then source .bash_profile:

```pcamejo@leftraru3:~$ source .bash_profile```

Now, we can create a job file with the commands to run:

```pcamejo@leftraru3:~$ cd botrytis_example/```

```pcamejo@leftraru3:~/botrytis_example$ nano bowtie2.sh```


```
#!/bin/bash
#SBATCH --job-name=example
#SBATCH --partition=slims
#SBATCH -n 10
#SBATCH --output=example_bowtie2.out
#SBATCH --error=example_bowtie2.err
#SBATCH --mail-user=user@gmail.com
#SBATCH --mail-type=ALL

bowtie-build GCF_000143535.2_ASM14353v4_genomic.fna GCF_000143535.2_ASM14353v4_genomic
bowtie -S GCF_000143535.2_ASM14353v4_genomic -1 b23_1.short.fastq -2 b23_2.short.fastq > b23.short.sam
```

Run job:
``` pcamejo@leftraru2:~/botrytis_example$ sbatch bowtie2.sh ```

Check example_bowtie2.err and example_bowtie2.out for more information about program errors and output. 

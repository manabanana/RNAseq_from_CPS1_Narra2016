# Download sequences from NCBI using SRA toolkit
- SRR4015208 Silveira 48 hr spherule 1
- SRR4015209 Silveira 48 hr spherule 2
- SRR4015210 Delta CPS1 48 hr spherule 1
- SRR4015211 Delta CPS1 48 hr spherule 2

```
module load nirav/sratoolkit/2.8.0
fastq-dump -I --split-files SRR4015208
fastq-dump -I --split-files SRR4015209
fastq-dump -I --split-files SRR4015210
fastq-dump -I --split-files SRR4015211
```

# Trim sequences with Trimmomatic

Load program and check what the adapter file contains
```
module load nirav/trimmomatic/0.38

cat /unsupported/nirav/trimmomatic/0.38/adapters/TruSeq3-PE-2.fa 
>PrefixPE/1
TACACTCTTTCCCTACACGACGCTCTTCCGATCT
>PrefixPE/2
GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT
>PE1
TACACTCTTTCCCTACACGACGCTCTTCCGATCT
>PE1_rc
AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTA
>PE2
GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT
>PE2_rc
AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC
```
PBS script to run Trimmomatic
```
#!/bin/bash
#PBS -N trimmomatic
#PBS -M manabanana@email.arizona.edu
#PBS -m bea
#PBS -W group_list=orbachmj
#PBS -q standard
#PBS -l select=1:ncpus=4:mem=24gb:pcmem=6gb
#PBS -l place=pack:shared
#PBS -l walltime=02:00:00
#PBS -l cput=08:00:00


###on hpc/standard, # nodes is 348, max cpu is 2016 per group, max memory 8064 gb, max walltime is 240 hrs per job.
###max 168 gb per node, 28 cores or cpus per node, 6gb per cpu --- select=1:ncpus=28:mem=168gb:pcmem=6gb
###if using less than 28 cores, use a number that divides into 28 like 7,14 or 21. 
###select=X:ncpus=Y:mem=Z
###X = the number of nodes of the resources required
###Y = the number of cores/cpu's required in each chunk
###Z = the amount of memory (in mb or gb) required per node
###pcmem=6gb is the max memory per core - optional


module load nirav/trimmomatic/0.38

cd /extra/manabanana/oldRNAseq_Narra2016

#Get the unique part of each fastq file to use as the base name for input and output files
#Need to add _1 to get the unique file names, otherwise trimmomatic will run twice for the _1 (forward) and _2 (reverse) files
for i in fastq/*_1.fastq 
do
NAME=$(echo $i | cut -f 1 -d "_") 

#Run trimmomatic on each of the unique file names
#Used parameters that are more stringent than what is listed on the Trimmomatic website
java -jar /unsupported/nirav/trimmomatic/0.38/trimmomatic-0.38.jar PE -threads 4 -phred33 ${NAME}_1.fastq ${NAME}_2.fastq ${NAME}_F_paired.fq.gz ${NAME}_F_unpaired.fq.gz ${NAME}_R_paired.fq.gz ${NAME}_R_unpaired.fq.gz ILLUMINACLIP:/unsupported/nirav/trimmomatic/0.38/adapters/TruSeq3-PE-2.fa:2:30:10 LEADING:15 TRAILING:15 SLIDINGWINDOW:4:18 AVGQUAL:20 MINLEN:40


done

```





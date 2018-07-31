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
Used slightly more stringent parameters than what is listed on the Trimmomatic website
```
java -jar /unsupported/nirav/trimmomatic/0.38/trimmomatic-0.38.jar PE -phred33 SRR4015208_1.fastq SRR4015208_2.fastq wt_rep1_for_paired.fq.gz wt_rep1_for_unpaired.fq.gz wt_rep1_rev_paired.fq.gz wt_rep1_rev_unpaired.fq.gz ILLUMINACLIP:/unsupported/nirav/trimmomatic/0.38/adapters/TruSeq3-PE-2.fa:2:30:10 LEADING:18 TRAILING:18 SLIDINGWINDOW:4:18 AVGQUAL:20 MINLEN:40


```





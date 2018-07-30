# Download sequences from NCBI using SRA toolkit
SRR4015208-Silveira 48 hr spherule 1
SRR4015209-Silveira 48 hr spherule 2
SRR4015210 - Delta CPS1 48 hr spherule 1
SRR4015211 - Delta CPS1 48 hr spherule 2

```
module load nirav/sratoolkit/2.8.0
fastq-dump -I --split-files SRR4015208
fastq-dump -I --split-files SRR4015209
fastq-dump -I --split-files SRR4015210
fastq-dump -I --split-files SRR4015211
```




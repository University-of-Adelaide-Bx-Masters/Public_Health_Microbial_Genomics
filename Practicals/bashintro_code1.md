
Example code for script.sh
- processes SampleA

```bash
#!/bin/bash


# Load software
source activate bioinf

# Create directories
mkdir 0_data
mkdir -p 1_trimmed/fastp
mkdir 2_aligned
mkdir 3_stats

# Get data
# symlink to illumina reads
ln -s /shared/data/bash_crash_prac/*.fq.gz 0_data/

# copy reference to current directory
cp /shared/data/bash_crash_prac/reference.fa .

# Run Quality Control
fastp --thread 2 \
-i 0_data/sampleA_R1.fq.gz \
-I 0_data/sampleA_R2.fq.gz \
-o 1_trimmed/sampleA_R1.fq.gz \
-O 1_trimmed/sampleA_R2.fq.gz \
--cut_right \
--cut_window_size 4 \
--cut_mean_quality 25 \
--length_required 90 \
--html 1_trimmed/fastp/sampleA.html


# Align reads to reference genome
# index reference genome
bwa index reference.fa

# align reads to reference
bwa mem -t 2 reference.fa \
1_trimmed/sampleA_R1.fq.gz \
1_trimmed/sampleA_R2.fq.gz \
| samtools view -bh - > 2_aligned/sampleA.bam

# Summarise alignment statistics
samtools stats 2_aligned/sampleA.bam > 3_stats/sampleA.txt
```

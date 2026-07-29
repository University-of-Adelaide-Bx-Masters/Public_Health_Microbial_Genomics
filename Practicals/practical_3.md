# Variant calling for phylogenomic trees & surveillance
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**
Whole-genome sequencing (WGS) of bacterial pathogens has transformed public health microbiology. By comparing the genomes of bacterial isolates, we can determine how closely related they are, identify outbreaks, and track the spread of antimicrobial resistance. 

The figure below is an example of a microbial genomics workflow that may be undertaken in a public health laboratory. In Practicals 1 and 2 you learned about species classification and microbial genotyping, this practical will focus on the steps highlighted in blue in the below figure. Which includes sequence read mapping and variant calling for constructing phylogenomic trees and visualise the tree in microreact with epidemiological data.  

<img width="806" height="591" alt="image" src="https://github.com/user-attachments/assets/8ab7ea23-a1a3-416e-9a79-88757197144c" />


## 1.1 Practical Overview


## 1.2 Learning Outcomes
1.	Gain more in-depth knowledge and further practice on identifying variants 
2.	Learn how a variant calling tool works 
3.	Learn how to build a phylogenomic trees from variants 
4.	Learn how to visualise and interpret a phylogenenomic trees
5.	Learn how to overlay metadata onto a phylogenomic tree and application to pathogen surveillance

# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create subdirectories that reflect the main steps in our analysis. This will help us stay organised.

```bash
mkdir --parents ~/Practical_variants_trees/{assembly,metadata,reads,snippy,tree}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
# navigate to working directory
cd ~/Practical_variants_trees
# create symlinks for all fastq files
ln -s ~/data/public_health_genomics/microbial_genomics/*.fastq.gz reads/
# create symlink for reference genome
ln -s ~/data/public_health_genomics/microbial_genomics/GCA_000009505.1_ASM950v1_genomic.fasta assembly/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
.
├── assembly
│   └── GCA_000009505.1_ASM950v1_genomic.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/GCA_000009505.1_ASM950v1_genomic.fasta
├── metadata
├── reads
│   ├── ERR10479021_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479021_1.fastq.gz
│   ├── ERR10479021_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479021_2.fastq.gz
│   ├── ERR10479025_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479025_1.fastq.gz
│   ├── ERR10479025_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479025_2.fastq.gz
│   ├── ERR10479028_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479028_1.fastq.gz
│   ├── ERR10479028_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479028_2.fastq.gz
│   ├── ERR10479029_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479029_1.fastq.gz
│   ├── ERR10479029_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479029_2.fastq.gz
│   ├── ERR10479032_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479032_1.fastq.gz
│   ├── ERR10479032_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479032_2.fastq.gz
│   ├── ERR10479034_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479034_1.fastq.gz
│   ├── ERR10479034_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479034_2.fastq.gz
│   ├── ERR10479035_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479035_1.fastq.gz
│   ├── ERR10479035_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479035_2.fastq.gz
│   ├── ERR10479037_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479037_1.fastq.gz
│   ├── ERR10479037_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479037_2.fastq.gz
│   ├── ERR10479039_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479039_1.fastq.gz
│   └── ERR10479039_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479039_2.fastq.gz
├── snippy
└── tree
```

# **3. Mapping sequencing reads to a reference genome using snippy**

A very talented bioinformatician who is a friend of mine (Torsten Seemann) wrote Snippy (https://github.com/tseemann/snippy). Snippy is a widely used pipeline that maps reads to a reference genome, to identify variants and produces a range of outputs. snippy uses BWA-MEM, SAMtools and FreeBayes, which we will learn more about in this practical. If you have done one of our bioinformatics courses you may find snippy familiar, however as a bioinformatician snippy will likely become one of the tools that you will most frequently use and so in this practical we will be looking at snippy in more detail. 

## 3.1 Run Snippy for variant calling 

Snippy requires: 

- A reference genome (FASTA or GENBANK format)
- Single- or paired-end sequencing reads from one or more isolates
- An output folder to put results in

We can explore the snippy help page to understand the available options:

```bash
snippy --help
```

The key parameters of Snippy are:

Parameter	Description
- outdir:	Directory where Snippy will write its output files
- R1:	Path to the forward (R1) FASTQ file
- R2: Path to the reverse (R2) FASTQ file
- ref:	Path to the reference genome (FASTA format)
- cpus:	Number of CPU threads to use (default: 8)
- ram:	Maximum RAM in GB (default: 8)
- minqual:	Minimum variant quality score (default: 100)
- minfra: Minimum allele frequency to call a variant (default: 0.9)
- prefix: Prefix for output files (default: snps)

Now the fun part, run snippy for each sample. Example command for one sample:

```bash
snippy --outdir snippy/ERR10479021 --ref assembly/GCA_000009505.1_ASM950v1_genomic.fasta --R1 reads/ERR10479021_1.fastq.gz --R2 reads/ERR10479021_2.fastq.gz 
```
Now we wait for snippy to finish this should take ~2 minutes for one sample 

## 3.2 Examine snippy logs 
To better understand how Snippy processes sequencing reads and generates variant calls, we will inspect the snp.log output file. The log records the commands executed during the analysis, allowing you to trace each stage of the snippy pipeline, including read alignment, BAM processing, variant calling and variant filtering. If you run a tool and it fails – information on why the tool failed to run will often be in the .log file – so this is an important file. 

We will use `grep` to look at what is happening in the snippy log files and to understand the order in which the core tools used by snippy are executed. Note that for this section you do not need to understand all of the commands that snippy uses at each step (that would be alot of information), this is more so to understand the overall steps. 

First view the snippy command and parameters used when we you ran snippy: 

```bash
grep "outdir" snippy/ERR10479021/snps.log
```
You should see something like this on the terminal:

```bash
/apps/conda3/singularity/envs/bioinf/bin/snippy --outdir snippy/ERR10479021 --ref assembly/GCA_000009505.1_ASM950v1_genomic.fasta --R1 reads/ERR10479021_1.fastq.gz --R2 reads/ERR10479021_2.fastq.gz
```
This shows you the exact `snippy` command that you ran above, including all parameters. This is useful for reproducibility — you can see precisely how the analysis was performed.

`snippy` then maps the sequencing reads to the reference genome using `bwa mem`, run the below to see the `bwa mem`command: 

```bash
grep "bwa mem" snippy/ERR10479021/snps.log
```

You should see the `bwa mem` command on the terminal - looks something like this: 

```bash
bwa mem  -Y -M -R '@RG\tID:ERR10479021\tSM:ERR10479021' -t 8 reference/ref.fa /shared/data/public_health_genomics/microbial_genomics/ERR10479021_1.fastq.gz /shared/data/public_health_genomics/microbial_genomics/ERR10479021_2.fastq.gz | samclip --max 10 --ref reference/ref.fa.fai | samtools sort -n -l 0 -T /tmp --threads 3 -m 2000M | samtools fixmate -m --threads 3 - - | samtools sort -l 0 -T /tmp --threads 3 -m 2000M | samtools markdup -T /tmp --threads 3 -r -s - - > snps.bam
```

`snippy` then  run the below to see the `samtools`command: 

```bash
grep "COMMAND: samtools" snippy/ERR10479021/snps.log
```

You should see the `samtools` command on the terminal - looks something like this: 

```bash
 samtools markdup -T /tmp --threads 3 -r -s - -
```

`snippy` then uses `freebays` to call variants in your sample against the reference genome, producing a variant call file (snps.raw.vcf), run:  

```bash
grep "freebayes" snippy/ERR10479021/snps.log
```

You should see something like this on the terminal:

```bash
freebayes-parallel reference/ref.txt 8 -p 2 -P 0 -C 2 -F 0.05 --min-coverage 10 --min-repeat-entropy 1.0 -q 13 -m 60 --strict-vcf   -f reference/ref.fa snps.bam > snps.raw.vcf
```

`Snippy` then applies some filters to assess the quality of those variants - retaining only high confidence variants. It then applies the high quality variants to the reference genome sequence to create a ‘pseudosequence consensus’ -  a version of the reference genome with the samples variants substituted in, run: 

```bash
grep "bcftools consensus" snippy/ERR10479021/snps.log
```

When you ran `snippy` above it created two versions of the pseudosequence consensus:
- snps.consensus.fa, contains all high-quality variants (SNPs and INDELs)
- snps.consensus.subs.fa, contains only high-quality SNPs (no INDELs)


## 3.3 Now let’s look at some of the snippy output files 

Let's now look at the key output files produced by `snippy`

bam file 

vcf file 

The tab file - human-readable variant summary 

The consensus FASTA file 



## 3.4 Run snippy over all samples 

You now need to run `snippy` over the remaining samples using the below. You need to create a script called snippy.sh and copy the contents below into it and complete the # Run snippy over the samples section of the script. Then save and run the script. You learnt how to do this in previous practicals. Please let us know if you need help :)

This will take ~30 minutes to finish 

```bash
#!/bin/bash

# Load software
source activate bioinf

# List of samples with Illumina data
SAMPLES=(ERR10479025 ERR10479028 ERR10479029 ERR10479032 ERR10479034 ERR10479035 ERR10479037 ERR10479039)

# Loop over each sample
for SAMPLE in "${SAMPLES[@]}";
do

# Run snippy over the remaining samples



done
```


**WE WILL END THE FIRST PART OF THE PRACTICAL HERE AND THE REMAINING PART OF THE PRACTICAL WE WILL DO IN THE NEXT PRACTICAL SESSION**



# **4. Run snippy core to create input for building a tree** 
 
We now need to created a core genome SNP alignment to input into our tree building tool, to do this we use snippy-core. 

Now run snippy-core to generate the core genome SNP alignment files across all samples: 

```bash
#Note: This is one long command
snippy-core --prefix snippy/core_genome --ref assembly/GCA_000009505.1_ASM950v1_genomic.fasta snippy/ERR10479021 snippy/ERR10479025 snippy/ERR10479028 snippy/ERR10479029 snippy/ERR10479032 snippy/ERR10479034 snippy/ERR10479035 snippy/ERR10479037 snippy/ERR10479039
```
the above snippy-core command will generate files with core.* prefix. To view a list of the ouput files we can use `ls`

```bash
ls snippy/core*
```
You should see the path to 6 files, and the files that are the most important include:
- core_genome.aln: Core genome SNP alignment 
- core_genome.tab: Tab-seperated table of core SNPs with alleles
- core_genome.full.aln: Whole genome SNP alignment, including invariant sites 

# **5. Build a phylogenomic tree from variant sites**

Run `iqTree` to create a tree from the core SNP alignment:

```bash
# first lets move the core.genome.full.aln into the tree directory using the 'mv' command
mv snippy/core_genome.full.aln tree
# now run iqtree 
iqtree -T 2 --mem 16G -s tree/core_genome.full.aln
```
This creates a phylogenomic tree in newick format - its the 'core_genome.full.aln.treefile' which we will visualise in a tree visualisation tool called Microreact. 

# **6. Visualise the phylogenomic tree using Microreact**

For this practical we will use Microreact to visulise the phylogenomic tree. It is important to understand that many other tools exist to visualise trees such as FigTree and iTOL. 

Download tree file for Microreact: 
- A phylogenomic tree in Newick format (`.nwk` or `.tre`): Download the `todo.nwk` file from the VM. This can be found in your `Practical_phylogenetic_trees_surveillance/results` folder.



<img width="1821" height="658" alt="image" src="https://github.com/user-attachments/assets/2d7e8ec5-947a-40d5-be09-171c0ef42b56" />

now lets midpoint root the tree 

<img width="1873" height="733" alt="image" src="https://github.com/user-attachments/assets/40a7cb6f-66db-45ef-a69d-c7b244f0bb18" />



# **7. Overlay genotyping data onto the tree in Microreact**

# **8. Overlay epidemiological metadata onto the tree in Microreact**

One of the most powerful aspects of microbial genomics is the ability to integrate genomic data with epidemiological metadata to investigate outbreak dynamics. 

Download metadata file for Microreact: 
- A metdata `.CSV` file with a column named `id` matching the sample names in the tree: Download the metadata file from the VM. Note in a real surveillance scenario, your metadata CSV could include columns such as: `collection_date`, `country`, `hospital`, `patient_id`, `MLST_ST`, `resistance_profile`, etc. For this practical this file has been generated for you and can be found in your`Practical_phylogenetic_trees_surveillance/results` folder. 



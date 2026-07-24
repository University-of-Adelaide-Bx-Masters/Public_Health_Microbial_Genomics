# Antimicrobial resistance variant detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="857" height="586" alt="image" src="https://github.com/user-attachments/assets/fbac366b-6c0d-4e17-b43e-76bb52025f4c" />


## 1.1 Practical Overview 

Consider the following scenario: Three patients (Table 1) have developed serious bloodstream infections caused by the bacterium *Burkholderia pseudomallei*. Despite receiving first-line antibiotic therapy with meropenem, all patients have failed to respond to treatment.

Whole-genome sequencing (WGS) was performed on the initial bacterial isolates collected from each patient prior to antibiotic exposure, as well as on subsequent isolates obtained after treatment had commenced. These secondary isolates were found to be resistant to meropenem, indicating the emergence of antibiotic resistance during therapy.

Previous genomic analyses were unable to identify the genetic mechanism responsible for resistance in the secondary isolates, suggesting that a previously uncharacterised mutation or novel genetic variant may be contributing to treatment failure. Your task is to identify the genetic variant(s) present in the secondary isolates that could explain the observed meropenem resistance.

In this practical, you will:

* Annotate a reference bacterial genome to identify genes and their predicted functions.
* Align sequencing reads from a resistant isolate to the annotated reference genome.
* Identify genetic variants, including single nucleotide polymorphisms (SNPs) and insertions/deletions (indels), that differ between the isolate and the reference genome.
* Annotate detected variants to predict their potential functional effects, including synonymous, missense, nonsense, and frameshift mutations.
* Visualize variants within their genomic context to evaluate data quality and assess their potential biological relevance.


## 1.2 Learning Outcomes


# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create subdirectories that reflect the main steps in our analysis. This will help us stay organised.

```bash
mkdir --parents ~/Practical_amr_variants/{assembly,reads,prokka,snippy}
```

## 2.3 Get data
The data for today's practical is located in `~/data/amr_variants`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
# navigate to working directory
cd ~/Practical_amr_variants
# create symlinks for all fastq files
ln -s ~/data/public_health_genomics/amr_variants/burk_reads/*.fastq.gz reads/
# create symlink for reference genome
ln -s ~/data/public_health_genomics/amr_variants/burk_references/*.fasta assembly/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```bash.
├── assembly
│   ├── MSHR3763_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_references/MSHR3763_genomic.fasta
│   ├── MSHR5864_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_references/MSHR5864_genomic.fasta
│   └── MSHR6522_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_references/MSHR6522_genomic.fasta
├── bakta
├── reads
│   ├── MSHR4083_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_reads/MSHR4083_1.fastq.gz
│   ├── MSHR4083_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_reads/MSHR4083_2.fastq.gz
│   ├── MSHR6755_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_reads/MSHR6755_1.fastq.gz
│   ├── MSHR6755_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_reads/MSHR6755_2.fastq.gz
│   ├── MSHR7929_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_reads/MSHR7929_1.fastq.gz
│   └── MSHR7929_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/amr_variants/burk_reads/MSHR7929_2.fastq.gz
└── snippy
```

# **3. AMR variant detection**

## 3.1 Reference genome annotation 

You may be thinking - what does it mean to annotate a genome? 

Run `prokka` over one sample: 

```bash
prokka \
  --outdir prokka/MSHR3763_annotation \
  --prefix MSHR3763_annotated \
  --genus burkholderia \
  --species pseudomallei \
  --strain MSHR3763 \
  --kingdom Bacteria \
  --force \
  --compliant \
  --locustag BPS \
  assembly/MSHR3763_genomic.fasta
```
start 13:59

``` bash
look at .gbk file
```

Lucky for you we already ahve the genome annotations, annoatted the first isolate from the patient becuase we will use this as our reference genome to call variants in the next step. 

## 3.2 Identify variants 

Now that we have an genome annotation file we can use this do with snippy using .gbk annotation as reference 

we will run snippy for one patient pair (primary isolate as the reference genome, and map the reads back from the follow up isolate back to the primary) at a time, 

``` bash
snippy --outdir snippy/MSHR4083 --ref assembly/MSHR3763_1.gb assembly/MSHR3763_2.gb --R1 reads/MSHR4083_1.fastq.gz --R2 reads/MSHR4083_2.fastq.gz  
```

## 3.3 AMR variants of interest 
name of the gene - .tsv from prokka

## 3.4 Visualise AMR variants 

--> use IGV




# Antimicrobial resistance variant detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="857" height="586" alt="image" src="https://github.com/user-attachments/assets/fbac366b-6c0d-4e17-b43e-76bb52025f4c" />


## 1.1 Practical Overview 

Consider the following scenario: we have 3 patients (Table 1, below) who have serious blood stream infections caused by the bacterium Burkholderia pseudomallei with all patients failing to respond to first line antibiotic (merepenem) therapy. Whole genome sequencing was performed on their first bacterial isolates (taken from the patients prior to antibiotic therpay) and on a second isolate that is now resistant to the treatment drug meropenem (taken from the patients after they had started antibiotic treatment). Genomic tools were unable to idenify the genetic mechanism that is leading to the resistance in the secondary isolates, indicating that a novel mutation that hasent been seen before may be present. Your task is to determine what genetic variant is present in the secondary isolates that could explain the drug resistance and treatment failure. 

**In this practical, you will:**
- Annotate a reference bacterial genome to identify genes and their functions
- Align sequencing reads from a resistant isolate to the annotated reference genome
- Call variants (SNPs and indels) that differ between the isolate and reference
- Annotate variants to predict their functional impact (synonymous, missense, nonsense, frameshift)
- Visualize variants in genomic context to assess quality and biological relevance


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
mkdir --parents ~/Practical_amr_variants/{assembly,reads,bakta,snippy}
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

## 3.1 Annotate reference genome 

Firstaly we need to annotate the first isolate from the patient. 


``` bash
bakta \
  --db shared/a1237649/bakta_db \
  --output bakta/MSHR3763_annotation \
  --prefix reference_annotated \
  --genus Burkholderia \
  --species pseudomallei \
   --gram - \
   --keep-contig-headers \
  --complete \
  --locus-tag burk \
  assembly/MSHR3763_genomic.fasta
```

## 3.2 Identify variants 

Now that we have an genome annotation file we can use this do with snippy using .gbk annotation as reference 

we will run snippy for one patient pair (primary isolate as the reference genome, and map the reads back from the follow up isolate back to the primary) at a time, 

``` bash
snippy --outdir snippy/MSHR4083 --ref assembly/MSHR3763.gff --R1 reads/MSHR4083_1.fastq.gz --R2 reads/MSHR4083_2.fastq.gz  
```

## 3.3 AMR variants of interest 
name of the gene - .tsv from prokka

## 3.4 Visualise AMR variants 

--> use IGV




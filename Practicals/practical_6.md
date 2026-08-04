# Antimicrobial resistance variant detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="857" height="586" alt="image" src="https://github.com/user-attachments/assets/fbac366b-6c0d-4e17-b43e-76bb52025f4c" />


## 1.1 Practical Overview 

Consider the following scenario: Three patients (Table 1) have developed serious bloodstream infections caused by the bacterium *Burkholderia pseudomallei* (a soil bacterium that is found in the north of Australia that causes the serious infectious disease melioidosis). Despite receiving first-line antibiotic therapy with meropenem, all patients have failed to respond to treatment.

Whole-genome sequencing (WGS) was performed on the initial bacterial isolates (we often call this the primary isolate) collected from each patient prior to antibiotic exposure, as well as on subsequent isolates (we call this a secondary isolate) obtained after treatment had commenced. These secondary isolates were found to be resistant to meropenem, indicating the emergence of antibiotic resistance during therapy.

Previous genomic analyses were unable to identify the genetic mechanism responsible for resistance in the secondary isolates, suggesting that a previously uncharacterised mutation or novel genetic variant may be contributing to treatment failure. Your task is to identify the genetic variant(s) present in the secondary isolates that could explain the observed meropenem resistance.

In this practical, you will:

* Annotate the reference bacterial genome to identify genes and their predicted functions
* Align sequencing reads from a resistant isolate to the annotated reference genome and identify genetic variants that differ between the isolate and the reference genome
* Annotate detected variants to predict their potential functional effects, including synonymous, missense, nonsense, and frameshift mutations.
* Visualise variants within their genomic context 

**The data that we will be working with today includes a primary isolate and secondary isolate from three patients:**

Patient 1:
- MSHR3763 (primary isolate)
- MSHR4083 (secondary isolate)

Patient 2:
- MSHR5864 (primary isolate)
- MSHR6755 (secondary isolate)

Patient 3: 
- MSHR6522 (primary isolate)
- MSHR7929 (secondary isolate)

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
# create symlinks for the reference genomes
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

`prokka` is bla bla bla 

Run `prokka` over one reference genome: 

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

**Parameter explanations:**
  - outdir prokka/MSHR3763_annotation: Specifies the output directory where all Prokka results will be saved. If the directory exists, use --force to overwrite
  - prefix  MSHR3763_annotated: Sets the prefix for all output files (e.g., reference_annotated.gff, reference_annotated.gbk). This helps organise results when annotating multiple genomes
  - genus burkholderia: Specifies the genus of the organism. Prokka uses this information to search genus-specific databases for more accurate functional annotations
  - species pseudomallei: Specifies the species. Combined with --genus, this improves annotation accuracy by prioritising species-specific gene names and functions
  - strain MSHR3763: Optional strain identifier included in the output metadata. Useful for record-keeping and reproducibility
  - kingdom Bacteria: Indicates the organism is bacterial (as opposed to Archaea or Viruses). This determines which gene prediction models and databases Prokka will use
  - force: Overwrites the output directory if it already exists. Without this flag, Prokka will exit with an error if the directory is present
  - compliant: Ensures output files comply with NCBI submission standards (e.g., GenBank format). Useful if you plan to submit annotations to public databases
  - locustag BPS: Locus tag prefix for gene identifiers (e.g., BPS_00001, BPS_00002). This creates systematic, unique gene IDs
  - assembly/MSHR3763_genomic.fasta: Path to the input reference genome in FASTA format

**Expected Runtime: 15 minutes (~8 Mb bacterial genome)**

Whilst we wait for `prokka` to complete, here is some information on genome annotation. 



`prokka` generates a number of files but the one we care most about for this practical is the `MSHR3763.annotated.gbk` file. This file has the extension `.gbk` ...... 


To look at the files on the terminal:   
``` bash
look at .gbk file
```

We will use this as our reference genome to call and annotate variants in the next step. 

## 3.2 Identify variants 

Now that we have an genome annotation file we can use this as the refrence gene for input into snippy using .gbk annotation as reference 

we will run snippy for one patient pair (primary isolate as the reference genome, and map the reads back from the secondary isolate back to the primary) at a time.

**Expected run time: 6 minutes**

``` bash
snippy --outdir snippy/MSHR4083 --ref prokka/MSHR3763_annotation/MSHR3763_annotated.gbk --R1 reads/MSHR4083_1.fastq.gz --R2 reads/MSHR4083_2.fastq.gz --report 
```

## 3.3 AMR variants of interest 

Gene of interest is ttrR-2 (mutations within these gene lead to meropenem resistance in the bacteria burkholderia) 

name of the gene - .tsv from prokka

**You can visualise the AMR variant**

 snps.report.txt 


# **4. Repeat the above steps for the remaining patient pairs**

1. Run `prokka` to annotate the genome assembly 
2. Run 'snippy' to identify variants
3. View `snippy` results to identify AMR mutations 


**Questions:**
- xxx
- ccccc
- xxxx
- 


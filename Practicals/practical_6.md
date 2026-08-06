# Antimicrobial resistance variant detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="857" height="586" alt="image" src="https://github.com/user-attachments/assets/fbac366b-6c0d-4e17-b43e-76bb52025f4c" />


## 1.1 Practical Overview 

Consider the following scenario: Three patients (listed below) admitted to hospitals in northern Australia developed serious bloodstream infections caused by the bacterium *Burkholderia pseudomallei* (a soil bacterium that is found in the north of Australia that causes the serious infectious disease melioidosis). Each patient initially responded poorly to treatment with the antibiotic meropenem, before relapsing with an antibiotic resistant infection. 

Whole-genome sequencing (WGS) was performed on isolates collected before treatment (primary isolates, susceptible to meropenem antibiotic) and after treatment failure (secondary isolates, resistant to meropenem antibiotic). 

Previous genomic analyses were unable to identify the genetic mechanism responsible for resistance in the secondary isolates, suggesting that a previously uncharacterised mutation or novel genetic variant may be contributing to treatment failure. 

You have been asked by the hospital microbiology laboratory to determine the genetic variant(s) present in the secondary isolates that could explain the observed drug resistance. Hint, we suspect that the resistance might be due to mutations in a **gene called ttR-2.** 

**In this practical, you will:**
- Annotate the primary isolate genome and use it as the reference genome for downstream analyses
- Align sequencing reads from the secondary resistant isolate to the corresponding annotated reference genome
- Identify genetic variants that distinguish the secondary resistant isolate from the reference genome
- Annotate identified variants to predict their potential functional impacts, including synonymous, missense, nonsense, and frameshift mutations
- Visualise genomic variants 

**The data that we will be working with today includes a primary isolate and secondary isolate from three patients:**

**Patient 1:**
- MSHR3763 (primary isolate)
- MSHR4083 (secondary isolate)

**Patient 2:**
- MSHR5864 (primary isolate)
- MSHR6755 (secondary isolate)

**Patient 3:**
- MSHR6522 (primary isolate)
- MSHR7929 (secondary isolate)

## 1.2 Learning Outcomes
1. Learn how to annotate a genome
2. Learn how to identify genetic variants that confer drug resistance 

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
Whole genome annotation is the process of identifying features of interest in genomic DNA sequences and labeling them with useful information. `prokka` (https://github.com/tseemann/prokka) is a software tool to rapidly annotate bacterial, archaeal and viral genomes, and was written by a world leading bioinformatician, Torsten Seemann. 

You will first work with the isolates associated with patient 1. 

Run `prokka` to annotate the primary isolate (MSHR3763) from patient 1: 

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

**Expected Runtime: 15 minutes**

Whilst we wait for `prokka` to complete, here is some information on genome annotation. 

**What is a genome annotation?**
- bla bla 




`prokka` generates multiple output files - the file we care most about for this practical is the `MSHR3763.annotated.gbk` file, you will use this file as the reference genome in section 3.2.

The `.gbk` file is the complete annotated genome, it contains DNA or protein sequences alongside biological annotations. Look at the .gbk file to understand its contents: 

``` bash
less prokka/MSHR3763_annotation/MSHR3763.annotated.gbk
```

You should see something like this on your terminal, and I have added comments to explain what each section means. 

```bash
LOCUS       BPS_1                4056707 bp    DNA     linear       24-JUL-2026
DEFINITION  Burkholderia pseudomallei strain MSHR3763.
ACCESSION   
VERSION
KEYWORDS    .
SOURCE      Burkholderia pseudomallei
  ORGANISM  Burkholderia pseudomallei
            Unclassified.
COMMENT     Annotated using prokka 1.15.6 from
            https://github.com/tseemann/prokka.
FEATURES             Location/Qualifiers
     source          1..4056707                                                            # Means the source sequence spans 1 - 4,056,707 bases 
                     /organism="Burkholderia pseudomallei"                                 # The organism identify 
                     /mol_type="genomic DNA"                                               # The sequence is genomic DNA
                     /strain="MSHR3763"                                                    # The isolate strain name 
     gene            633..1574                                                             # The location of a predicted gene, starts position 633 bp and ends 1574 bp (=942bp long)
                     /locus_tag="BPS_00001"                                                # the unique identifier assigned by prokka 
     CDS             633..1574                                                             # Coding Sequence, This indicates that prokka predicts this regions encodes a protein 
                     /locus_tag="BPS_00001"
                     /inference="ab initio prediction:Prodigal:v2.6"
                     /codon_start=1
                     /transl_table=11
                     /product="hypothetical protein"                                       # prokka predicted the protein to be a hypothetical protein 
                     /protein_id="Prokka:BPS_00001"                                        # Unique identifier generated by prokka
                     /translation="MRVHREHRAHAVDELAHVDTRLLDRELPRLDARVIENVVQHPRE            # The predicted protein sequence translated from the CDS (942bp / 3 = 314 amino acids) 
                     RLPRVLHEPEQPLLLRREQPLPHQLDEREHAVHRRAQFVARGRERAPARSERRLQPLQ
                     VAVRPSVAQQRDEHVAAPQHRVHMRPLAGLSRDQHALLHPVGQLRQRQERERRCAETY
                     EYASTEFASAKAVAFHTFIPYGTSPEYASLPFPEKTRTSFFCGVPLMGTVVYPIARAL
                     SPIPRFDGAQSRAAASALEPPRVRAGRPASESRRSTHAHRDILAPDLAISGFAPRQIY
                     ASSENEKTDSAFPFTSVSAVRFGSHARNDCRIAPIFG"

````


## 3.2 Identify variants 

Now that we have a genome annotation file we can use this as the reference genome for input into `snippy`

Here you will run snippy for patient 1  (annotated primary isolate as the reference genome, and map the reads back from the secondary isolate back to the primary): 

**Expected run time: 6 minutes**

``` bash
snippy --outdir snippy/MSHR4083 --ref prokka/MSHR3763_annotation/MSHR3763_annotated.gbk --R1 reads/MSHR4083_1.fastq.gz --R2 reads/MSHR4083_2.fastq.gz --report 
```

## 3.3 AMR variants of interest 

Gene of interest is ttrR-2 (mutations within these gene lead to meropenem resistance in the bacteria burkholderia) 

name of the gene - .tsv from prokka

**You can visualise the AMR variant**

 snps.report.txt 

 BAM visualisation??


# **4. Repeat the above steps for the remaining 2 patients**

1. Run `prokka` to annotate the genome assembly 
2. Run `snippy` to identify variants
3. View `snippy` results to identify variants in the gene ttR-2


**Once ran above for all patients, complete the below table:**

| **Patient**            | **Gene, ttR-2**  | **Variant** | **Effect** | 
|:----------             |:---------------- |:----------- |:-----------|
| Patient 1              |                  |             |            |
| Patient 2              |                  |             |            |
| Patient 3              |                  |             |            |

**Answer the following questions:**
- Did all patients acquire the same mutation associated with meropenem resistance? 
- Does the genomic evidence support independent evolution of meropenem resistance in each patient?
- Would experimental validation be required to confirm that these variants contribute to meropenem resistance? yes or no? 



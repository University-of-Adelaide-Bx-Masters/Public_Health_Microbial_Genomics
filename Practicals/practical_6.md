# Antimicrobial resistance variant detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

Antimicrobial resistance (AMR) occurs when microorganisms acquire genetic changes that allow them to survive treatment with antimicrobial drugs. Identifying the genetic basis of AMR is an important application of bacterial whole-genome sequencing (WGS).

For this practical you will be working on the workflow step indicated with a star as below. 

<img width="857" height="586" alt="image" src="https://github.com/user-attachments/assets/fbac366b-6c0d-4e17-b43e-76bb52025f4c" />


## 1.1 Practical Overview 

In this practical, you will investigate a real-world  scenario involving _Burkholderia pseudomallei,_ the bacterium that causes melioidosis.

Three patients in the Northern Territory, Australia developed serious bloodstream infections caused by _B. pseudomallei_. Each patient initially received the antibiotic meropenem but responded poorly to treatment. Following treatment, a second isolate was recovered from each patient that showed resistance to meropenem.

Whole-genome sequencing (WGS) was performed on isolates collected:
- before treatment: the primary isolate, which was susceptible to meropenem
-  and after treatment failure: the secondary isolate, whihc was resistant to meropenem 

Previous genomic analyses did not identify an obvious genetic explanation for the resistance phenotype. This suggests that the resistance may be associated with a previously uncharacterised genetic variant.

**Your task**

You have been asked by the hospital microbiology laboratory to investigate the genetic differences between the susceptible and resistant isolates. Previous evidence suggests that mutations in a gene called **ttgR_2** may be involved in meropenem resistance. Your aim is to determine whether mutations in ttgR_2 are present in the resistant isolates and therefore explain the resistance.
 

**In this practical, you will:**
- Annotate the genome of a susceptible primary isolate
- Use the annotated genome as a reference for variant detection
- Align sequencing reads from the resistant isolate to the susceptible reference
- Identify genetic variants that distinguish the resistant isolate from the susceptible isolate
- Determine whether ttgR_2 contains a mutation
- Interpret the predicted effect of the mutation

**The data that we will be working with today:**

Each patient has a susceptible primary isolate and a resistant secondary isolate. The primary isolate from each patient provides the most appropriate reference for analysing the corresponding secondary isolate.

**Patient 1:**
- MSHR3763 (primary susceptible isolate)
- MSHR4083 (secondary resistant isolate)

**Patient 2:**
- MSHR5864 (primary susceptible isolate)
- MSHR6755 (secondary resistant isolate)

**Patient 3:**
- MSHR6522 (primary susceptible isolate)
- MSHR7929 (secondary resistant isolate)

## 1.2 Learning Outcomes
1. Learn how to annotate a genome
2. Learn how to identify genetic variants 

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
├── prokka
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

## 3.1 Annotate the reference genome

You will first work with Patient 1 - you will use the susceptible isolate MSHR3763 as the reference genome

You first need to annotate MSHR3763 so that, when you identify variants, you can determine whether they occur within genes and predict their possible consequences.

 You will use `prokka` (https://github.com/tseemann/prokka), a software tool to rapidly annotate bacterial, archaeal and viral genomes, and was written by a world leading bioinformatician, Torsten Seemann. 


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

**What do these parameters mean?**
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

**Whilst we wait for `prokka` to complete, here is some information on genome annotation.**

**What is a genome annotation?**
A genome sequence tells us the order of DNA bases, but the sequence alone does not tell us where genes are located or what those genes may encode. Genome annotation is the process of identifying biological features within a genome and assigning information to those features.

For example, annotation can identify:
- protein-coding genes
- ribosomal RNA genes
- transfer RNA genes
- predicted protein products
- gene locations
- gene identifiers
- predicted protein sequences

This information becomes particularly useful when analysing mutations.

For example, suppose we identify the following mutation:

```bash
Chromosome 1, position 1,234,567:
A → G
```
By itself, this tells us that the DNA sequence differs at one position.

If the genome is annotated, we can instead determine that the mutation occurs:

```bash
gene:       ttgR_2
feature:    CDS
nucleotide: 245/900
amino acid: 82/300
effect:     missense_variant
```
We can therefore begin to ask whether the mutation could alter the function of the encoded protein.

**Key concept:** Variant calling tells us where the DNA differs. Genome annotation helps us understand what biological feature the difference affects

**Hopefully `prokka` has now finished**

`prokka` generates several output files, including an annotated GenBank file (`MSHR3763.annotated.gbk`). For this practical, this is the most important output because it contains both the genome sequence and its annotation, and you will use this file as the reference genome to input into `snippy` in section 3.2.

The `.gbk` file contains both sequence information and biological annotations. Look at the .gbk file to understand its contents: 

``` bash
less /prokka/MSHR3763_annotation/MSHR3763.annotated.gbk
```

You should see something like this on your terminal, and I have added comments to explain what the relevant sections mean.  

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

**Questions:**
- Approximately how many genes were predicted?
- What information is stored for each coding sequence?
- Why is genome annotation necessary before variant effect prediction?
- What does the locus tag represent? 

## 3.2 Identify variants in the resistant isolate  
You now want to identify differences between MSHR3763 (susceptible primary isolate) and MSHR4083 (resistant secondary isolate), for patient 1. You will use sequencing reads from MSHR4083 and align them to the annotated MSHR3763 reference (`MSHR3763_annotated.gbk`) using `snippy`. 

You are already familiar with the fact that `snippy` identifies variants between a reference genome and sequencing reads, including SNPs and insertions/deletions. When a GenBank annotation file (`MSHR3763_annotated.gbk`) is supplied to `snippy` as the reference genome, `snippy` uses the annotation file to identify the affected feature and predict the consequence of the variant.


Run `snippy` for Patient 1: 

``` bash
snippy --outdir snippy/MSHR4083 --ref prokka/MSHR3763_annotation/MSHR3763_annotated.gbk --R1 reads/MSHR4083_1.fastq.gz --R2 reads/MSHR4083_2.fastq.gz --report 
```

The `--report` option generates an additional report containing detailed information about the called variants

**Expected run time: 6 minutes**

`snippy` generates multiple output files - the relevant file for today's practical is the `snps.html` file. This file contains the differences between the secondary isolate and the reference genome. Each row is one detected variant.

click on the file and open it, you should see something like this: 

<img alt="image" src="https://github.com/user-attachments/assets/1c3f2d4e-ba43-4663-be04-689d62a097f5" />

**Here's what each column means:**
- `CHROM`	Chromosome or contig where the variant occurs. (BPS_1, BPS_2)
- `POS`	Position of the variant on the reference genome
- `TYPE`	Type of variant, such as SNP, insertion or deletion
- `REF`	Reference nucleotide(s)
- `ALT`	Alternative nucleotide(s) observed in the resistant isolate
- `EVIDENCE`	Number of sequencing reads supporting each allele
- `FTYPE`	Feature type where the mutation occurs (e.g. CDS = coding sequence). Blank usually means the variant is outside annotated genes
- `STRAND`	Gene orientation (+ or -)
- `NT_POS`	Nucleotide position within the coding sequence
- `AA_POS`	Amino acid position within the protein
- `EFFECT`	Predicted effect on the protein
- `LOCUS_TAG`	Gene identifier in the annotation
- `GENE`	Gene name
- `PRODUCT`	Protein encoded by the gene

These annotation fields are generated because we supplied `snippy` with an annotated GenBank reference.

**Questions:**

Previous work suggests that mutations in **ttgR_2** may be associated with meropenem resistance. Use the `snippy` report to answer the following:
- Is **ttgR_2** mutated?
- What type of mutation has occurred (e.g. SNP/insertion/deletion)?
- Is the mutation predicted to alter the protein sequence (e.g. synonymous, missense, frameshit, stop-gained)?
- Would you predict this mutation increases, decreases or abolishes protein function? 
- What chromosome contains the mutation?
- Is this a strong candidate resistance mutation?

**Explore variant evidence**

The variant table tells us which variants were identified, but it is also important to consider the sequencing evidence supporting each variant call.

Open: 

``` bash
less snippy/MSHR4083/snps.report.txt 
```

The `snps.report.txt` provides detailed information about individual variants and the reads supporting them. `snippy's` `--report` functionality generates this type of detailed report from the variant calls.

See if you can find the **ttgR_2** variant in the `snps.report.txt`. Hint: To identify the **ttgR_2** mutation in the `snps.report.txt`, you can use the coordinates of the gene from `snps.html`, then locate variants that fall within those coordinates in `snps.report.txt`. **This might be tricky so please ask us for help if you need it.**

**Question:**
- Do you think a good number of reads cover the mutation?
  

# **4. Repeat the above steps for the remaining 2 patients**

1. Run `prokka` to annotate the genome assembly 
2. Run `snippy` to identify variants
3. View `snippy` results to identify variants in the gene ttR_2


**Once ran above for all patients, complete the below table:**

| **Patient**            | **Gene**  | **Variant** | **Predicted effect** | 
|:----------             |:----------|:----------- |:-----------|
| Patient 1              |    ttR_2  |             |            |
| Patient 2              |    ttR_2  |             |            |
| Patient 3              |    ttR_2  |             |            |

**Answer the following questions:**
- Were the ttgR-2 mutations the same or different among the patients?
- Why might different mutations in the same gene produce a similar resistance phenotype?
- Does the genomic evidence suggest transmission of a resistant strain between patients, or does it suggest that resistance evolved independently during treatment?
- Do the results prove that ttgR_2 mutations cause meropenem resistance, or would experimental validation be required to confirm that these variants contribute to meropenem resistance? Yes or no?
- Why do we compare the resistant isolate with the susceptible isolate from the same patient, rather than with a public reference genome?




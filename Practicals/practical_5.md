# Antimicrobial resistance gene detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="826" height="592" alt="image" src="https://github.com/user-attachments/assets/9ec3dbde-b6cf-4e9d-8b51-780ab1e99480" />


## 1.1 Practical Overview

In this practical, you will analyse genome assemblies (FASTA files) from a range of bacterial isolates:
| **Species**            | **Assembly file name**  | 
|:----------             |:----------|
| _Staphylococcus aureus_              |    07-02477_Staphylococcus_aureus_genomic |   
| _Salmonella enterica_              | 1048349_Salmonella_enterica_genomic|  
| _Klebsiella pneumoniae_              | cpe004_Klebsiella_pneumoniae_genomic |  
| _Acinetobacter baumannii_              | cpe017_Acinetobacter_baumannii_genomic |  
| _Pseudomonas aeruginosa_              | cpe019_Pseudomonas_aeruginosa_genomic |  
| _Escherichia coli_              | cpe069_Escherichia_coli_genomic |  
| _Staphylococcus aureus_              | HE681097_Staphylococcus_aureus_genomic |


For each of the genomes, you will investigate if antimicrobial resistance determinants are present

## 1.2 Learning Outcomes
1. Use command-line tools to screen bacterial genome assemblies for known AMR determinants
2. Learn how to interperate the main fields in an `AMRFinderPlus` and `abritamr` output file


# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create subdirectories that reflect the main steps in our analysis. This will help us stay organised.

```bash
mkdir --parents ~/Practical_amr_genes/{assemblies,db,abritamr,amrfinder}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_amr_gene
# create symlinks for genome assembly (FASTA format) files
ln -s ~/data/public_health_genomics/amr_genes/assemblies/*.fasta assemblies/
# create symlink for amrfinder database 
ln -s ~/data/public_health_genomics/microbial_genomics/amrfinder_db/2024-07-22.1 db/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
.
├── abritamr
├── amrfinder
├── assemblies
│   ├── 07-02477_Staphylococcus_aureus_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/07-02477_Staphylococcus_aureus_genomic.fasta
│   ├── 1048349_Salmonella_enterica_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/1048349_Salmonella_enterica_genomic.fasta
│   ├── cpe004_Klebsiella_pneumoniae_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/cpe004_Klebsiella_pneumoniae_genomic.fasta
│   ├── cpe017_Acinetobacter_baumannii_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/cpe017_Acinetobacter_baumannii_genomic.fasta
│   ├── cpe019_Pseudomonas_aeruginosa_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/cpe019_Pseudomonas_aeruginosa_genomic.fasta
│   ├── cpe069_Escherichia_coli_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/cpe069_Escherichia_coli_genomic.fasta
│   └── HE681097_Staphylococcus_aureus_genomic.fasta -> /shared//a1237649/data/public_health_genomics/amr_genes/assemblies/HE681097_Staphylococcus_aureus_genomic.fasta
└── db
    └── 2024-07-22.1 -> /shared//a1237649/data/public_health_genomics/microbial_genomics/amrfinder_db/2024-07-22.1
```

# **3. Genomic detection of AMR using AMRFinderplus**

To accurately detect antimicrobial resistant genes or mutations, the US National Center for Biotechnology Information (NCBI) developed the Bacterial Antimicrobial Resistance Reference Gene Database and AMRFinder, a tool for detecting AMR genes. More recently, NCBI released AMRFinderPlus, which includes additional functionality such as the detection of point mutations in protein and nucleotide sequences, as well as taxon-specific analyses that can include or exclude specific genes and mutations. AMRFinderPlus is available as a command-line tool. In this section, you will use AMRFinderPlus to analyse antimicrobial resistant bacterial strains. 

**Now run `amrfinder` on one of the bacterial genomes**

Before you run `amrfinder` you would check to see if the most up to date AMR database is downloaded. I have already done this step for you so you don't need to worry about that.

`amrfinder` is a relatively simple tool to run, just give it a genome assembly as the input file. 

Run `amrfinder` on one sample:

```bash
amrfinder -n assemblies/1048349_Salmonella_enterica_genomic.fasta -O Salmonella -o amrfinder/ERR2093245_Salmonella_typhi.txt -d db/2024-07-22.1
```

**The command explained:** 
- `-n` Specifies a nucleotide FASTA input
- `-O` `Salmonella`	Specifies the AMRFinderPlus organism/taxonomic group
- `-o`	Specifies the output file
- `-d`	Specifies the AMRFinderPlus database

**Important to remember**
The `-O` option is particularly important when analysing organisms for which AMRFinderPlus contains curated information on resistance-associated mutations. When running AMRFinderPlus, you will need to specify the appropriate organism for each bacterial genome.

The correct organism name must match one of the taxonomic groups recognised by AMRFinderPlus. **Before running your analysis, consider how you could identify the organism names available to AMRFinderPlus and use this information to ensure you provide the exact name required by the `-O` option when running `abritamr`.**

Examine the `amrfinder` output:

The table below includes the `amrfinder` output of ERR2093245_Salmonella_typhi (file `ERR2093245_Salmonella_typhi.txt`)

<img alt="image" src="https://github.com/user-attachments/assets/7132f62b-a7a3-493f-ad61-6908a0feb894" />


**If you need any help understanding what each of the columns in the table means, please feel free to ask :) We’d be happy to talk you through it!**

**Questions:**
- Which antimicrobial resistance (AMR) genes or mutations are present in the genome?
- Which antibiotics or antimicrobial classes are these genes/mutations associated with?
- How strong is the sequence match for each AMR determinant (e.g., % coverage and % identity)?
- Are there multiple AMR determinants associated with the same antimicrobial class?
- What does the overall AMR profile tell you about the potential resistance of this bacterium? (Hint: In bacterial genomics, a bacterium is considered **multidrug-resistant (MDR)** if it has resistance to more **than 3 drug classes**.)? 

# **4. Genomic detection of AMR using abritamr**

`abriTamr` is a pipeline designed to simplify the interpretation of antimicrobial resistant results. It runs AMRFinderPlus and then organises detected resistance determinants into functional antimicrobial classes. It also produces summary files that make it easier to compare resistance determinants across isolates.

`amrfinder` is useful when we want to inspect the detailed evidence for an individual match, whereas `abritamr` provides a more streamlined summary of the resistance determinants detected and their associated drug classes.

Some resistance-associated point mutations are species-specific. Therefore, when using the species option, we need to specify a supported species.

Check the available options:

```bash
abritamr run -h
```

You can search the help output for _Salmonella_

```bash
abritamr run -h | grep Salmonella
```
You should see Salmonella in the terminal output highlighted in red, meaning it can be supplied to the `--species` option.

**Note:** The species option is used when you want abriTAMR/AMRFinderPlus to include species-specific point-mutation analysis. It should not be interpreted as a species-identification method.

Now run `abritamr` over one sample and include `Salmonella` as the designated species: 

```bash
abritamr run --contigs assemblies/1048349_Salmonella_enterica_genomic.fasta  --prefix abritamr/1048349_Salmonella_enterica_genomic  --species Salmonella 
```

The command:
- `--contigs`	Specify the genome assembly
- `--prefix`	Specify the output location/prefix
- `--species` `Salmonella`	Use Salmonella-specific analysis 

`anritamr` `run` generates five output files per sample:
- `amrfinder.out`
- `summary_matches.txt`
- `summary_partials.txt`
- `sumamry_virulance.txt`

Now compare these results with the `amrfinder` output from Section 3

Questions:
- Which AMR genes detected by AMRFinderPlus are present in the abriTAMR summary?
- Which antimicrobial class(es) are associated with these genes?
- Did `abritamr` identify any partial matches? If so, what might explain a partial match?
- Why might the `abritamr` summary be more useful than the raw `amrfinder` output when comparing many bacterial isolates?
- What information would you lose if you only looked at the summary file rather than the raw `amrfinder` output?


# **5. Run `amrfinder` and `abritamr` for the remaining bacterial assemblies**

Now run `amrfinder` and `abritamr` on the remaining bacterial assemblies - make sure you designate the correct `-- species` flags when running each of the tools. 

**Questions:**
- Which genome contained the greatest number of detected AMR determinants?
- Which antimicrobial classes were most frequently represented?
- Were any isolates free of known AMR genes?


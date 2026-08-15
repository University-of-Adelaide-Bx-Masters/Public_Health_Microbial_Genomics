# Antimicrobial resistance gene detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="826" height="592" alt="image" src="https://github.com/user-attachments/assets/9ec3dbde-b6cf-4e9d-8b51-780ab1e99480" />


## 1.1 Practical Overview

In this practical, you will analyse genome assemblies (FASTA files) from antimicrobial resistant bacterial isolates:
| **Species**            | **Assembly file name**  | 
|:----------             |:----------|
| _Staphylococcus aureus_              |    ERR017261_Staphylococcus_aureus |   
| _Salmonella typhi_              | ERR2093245_Salmonella_typhi |  
| _Salmonella typhi_               |  ERR2093329_salmonella_typhi   |  
| _Klebsiella pneumoniae_              | ERR4095909_Klebsiella_pneumoniae |  
| _Burkhodleria pseudomallei_              | MSHRxxxxx_ |  

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
cd 
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_amr_gene
# create symlinks for genome assembly (FASTA format) files
ln -s ~/data/public_health_genomics/amr_genes/fasta_files/*.fasta assemblies/
# create symlink for amrfinder database 
ln -s ~/data/public_health_genomics/microbial_genomics/amrfinder_db/latest db/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Genomic detection of AMR using AMRFinderplus**

To accurately detect antimicrobial resistant genes or mutations, the US National Center for Biotechnology Information (NCBI) developed the Bacterial Antimicrobial Resistance Reference Gene Database and AMRFinder, a tool for detecting AMR genes. More recently, NCBI released AMRFinderPlus, which includes additional functionality such as the detection of point mutations in protein and nucleotide sequences, as well as taxon-specific analyses that can include or exclude specific genes and mutations. AMRFinderPlus is available as a command-line tool. In this section, you will use AMRFinderPlus to analyse antimicrobial resistant bacterial strains. 

**Now run `amrfinder` on one of the bacterial genomes**

Before you run `amrfinder` you would usually check to see if you have the most upto date AMR database downloaded. I have already done this step for you so you dont need to worry about that.

`amrfinder` is a relatively simple tool to run - use a genome assembly as the input file. 

Run `amrfinder` on one sample:

```bash
amrfinder -n assemblies/ERR2093245_Salmonella_typhi -O Salmonella -o amrfinder/ERR2093245_Salmonella_typhi.txt -d db/latest
```

The command explained: 
- `-n` Specifies a nucleotide FASTA input
- `-O` `Salmonella`	Specifies the AMRFinderPlus organism/taxonomic group
- `-o`	Specifies the output file
amrfinder/ERR10479021_amrfinder.txt	Location and name of the output file
- `-d`	Specifies the AMRFinderPlus database

**NOTE**
The `-O` option is particularly important when analysing organisms for which AMRFinderPlus contains curated information on resistance-associated mutations. When running AMRFinderPlus, you will need to specify the appropriate organism for each bacterial genome.

The correct organism name must match one of the taxonomic groups recognised by AMRFinderPlus. **Before running your analysis, consider how you could identify the organism names available to AMRFinderPlus and use this information to ensure you provide the exact name required by the `-O` option when running `abritamr`.**

Interpreting AMRFinderPlus results:

Questions:
- Did you find any AMR genes in the results file?
- What does this result mean?


```````````````````````````````````````````````````````````````````````````````````

``THIS ONE WORKED over Bp in a previous run 

amrfinder -n /shared/a1237649/Practical_amr_variants/assembly/MSHR3763_genomic.fasta -O Burkholderia_pseudomallei -o amrfinder/MSHR3763_amrfinder.txt -d db/latest

``````````````````````````````````````````````````````````````````````````````````````````

# **4. Genomic detection of AMR using abritamr**
abritamr - assemblies - antibiotic classes and custom reports 

We can run `abritamr` based on the species we have in the samples. Before we run lets check if Salmonella is available:

```bash
abritamr run -h | grep Salmonella
```
You should see Salmonella in the terminal output highlighted in red, confirming the species is available.

Now run over one sample and include salmonella as the designated species: 

```bash
abritamr run --contigs assemblies/ERR10479021.fasta --prefix abritamr/ERR10479021 --species Salmonella 
```

Running `anritamr` `run` generates five outpur files per sample:
- amrfinder.out
- summary_matches.txt
- summary_partials.txt
- sumamry_virulance.txt

.....

Now write a script to repeat `abritamr` on the remaning samples 

# **5. Run amrfinder and abritamr over remaining assemblies**





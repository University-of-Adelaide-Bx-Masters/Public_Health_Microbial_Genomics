# Antimicrobial resistance gene detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="826" height="592" alt="image" src="https://github.com/user-attachments/assets/9ec3dbde-b6cf-4e9d-8b51-780ab1e99480" />


## 1.1 Practical Overview

Bacterial strains to be analysed in this practical: 
- s.aureus
- s. typhi
- s.thypi
- 
- sss
- ss 

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



AMRFinderPlus
- nnnnnn
amrfinder - (what lacking that abritamr does?)


**Now run amrfinder**

Before you run `amrfinder` you would usually check to see if you have the most upto date AMR database downloaded. I have already done this step for you so you dont need to worry about that. `amrfinder` is a relatively simple tool to run - all you really need is a genome assembly as the input file. 

Run `amrfinder` on one sample:

```bash
amrfinder -n assemblies/ERR10479021.fasta -O Salmonella -o amrfinder/ERR10479021_amrfinder.txt -d db/latest
```

THIS ONE WORKED over B.p so maybe no AMR in any of the salmonella

amrfinder -n /shared/a1237649/Practical_amr_variants/assembly/MSHR3763_genomic.fasta -O Burkholderia_pseudomallei -o amrfinder/MSHR3763_amrfinder.txt -d db/latest


This should run quickly

The above command explained:
- lkkk
- kkkk
- kkkkk

Interpreting AMRFinderPlus results:

Questions:
- Did you find any AMR genes in the results file?
- What does this result mean? 

You can see that for the Salmonella sample - that no AMR genes were detected. 


# **4. Genomic detection of AMR using abritamr**

Create a script called amrfinder.sh to run `amrfinder` over all Salmonella samples (Remember the genome assemblies are in the assemblies/ folder). 

Questions:
- Did you find any AMR genes in the results files?
- What do these results mean?


# **4. AMR detection using abritamr**
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





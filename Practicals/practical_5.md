# Antimicrobial resistance gene detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="826" height="592" alt="image" src="https://github.com/user-attachments/assets/9ec3dbde-b6cf-4e9d-8b51-780ab1e99480" />


## 1.1 Practical Overview
 - working with the same set of salmonella isolates - now know that we are deadling with a clonal salmonella outbreak, and we want to determine wheather it is also an antimicrobial resistant outbreak. 

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
mkdir --parents ~/Practical_amr_gene/{assemblies, amrfinder, abritamr, salmonella_tree}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_amr_gene
# create symlinks for all genome assembly (FASTA format) files
ln -s ~/data/public_health_genomics/microbial_genomics/assemblies/*.fasta assemblies/
# create symlink for amrfinder database 
ln -s ~/data/public_health_genomics/microbial_genomics/amrfinder_db
# create symlink for the tree you genertated in the variants and phylo practical
ln -s ~/data/public_health_genomics/microbial_genomics/TODOOOO
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. AMR detection using AMRFinder plus**

AMRFinderPlus
- nnnnnn


**Now run amrfinder**

'amrfinder' is a relatively simple tool to run 
amrfinder - (what lacking that abritamr does?) 




# **4. AMR detection using abritamr**
abritamr - assemblies - antibiotic classes and custom reports 

We can run `abritamr` based on the species we have in the samples. Before we run lets check if Salmonella is available:

```bash
abritamr run -h | grep Salmonella
```
You should see Salmonella in the terminal output highlighted in red, confirming the species is available.

Now run over one sample and include salmonella as the designates species: 

```bash
abritamr run --contigs assemblies/ERR10479021.fasta --prefix abritamr/ERR10479021 --species Salmonella 
```

running `anritamr` `run` generates five outpur files per sample:
- amrfinder.out
- summary_matches.txt
- summary_partials.txt
- sumamry_virulance.txt

.....

Now repeat `abritamr` on the 8 remaning samples 

# **5. Overlay AMR data onto the Salmonella tree**
Brining the data together to aid in intepretation 

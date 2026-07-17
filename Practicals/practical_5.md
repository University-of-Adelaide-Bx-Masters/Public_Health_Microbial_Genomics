# Antimicrobial resistance gene detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="826" height="592" alt="image" src="https://github.com/user-attachments/assets/9ec3dbde-b6cf-4e9d-8b51-780ab1e99480" />


## 1.1 Practical Overview
 - working with the same set of salmonella isolates - now know that we are deadling with a clonal salmonella outbreak, and we want to know if the outbreak is a clonal Antimcirobial resistant outbreak. 

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
mkdir --parents ~/Practical_amr_gene/{assemblies, amrfinder, abritamr}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_amr_gene
# create symlinks for all genome assembly (FASTA format) files
# create symlink for amrfinder and abritamr
ln -s ~/data/public_health_genomics/microbial_genomics/TODOOOOOOO
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. AMR gene detection**

amrfinder - (what lacking that abritamr does?) 
abritamr - assemblies - antibiotic classes and custom reports 


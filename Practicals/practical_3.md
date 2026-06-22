# Variant calling for phylogenetic trees & application to public health 
{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

Insert diagram -  public health microbial genomics workflow &  indicate which step we at 

## 1.1 Practical Overview


## 1.2 Learning Outcomes
1.	Gain more in-depth knowledge and further practice on calling variants 
2.	Learn how a variant calling tool works 
3.	Learn how to build phylogenetic trees from variants 
4.	Learn how to interpret phylogenetic trees and application to pathogen surveillance

# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create subdirectories that reflect the main steps in our analysis. This will help us stay organised.
```bash
mkdir --parents ~/Practical_variant_calling_phylogenetic_trees_surveillance/ xxx
mkdir -p ~/Practical_variant_calling_phylogenetic_trees_surveillance/ xx
mkdir -p ~/Practical_variant_calling_phylogenetic_trees_surveillance/ xx

## 2.3 Get data
The data for today's practical is located in `~/data/intro_pathogens`. As in previous practicals, we will use symlinks instead of copying large data files.
```bash
cd ~/Practical_phylogenetic_trees_surveillance
# create symlinks for all fastq files
ln -s ~/data/Practical_phylogenetic_trees_surveillance/*.fastq.gz xx/
# create symlink for reference genome
ln -s ~/data/Practical_phylogenetic_trees_surveillance/xyz.fasta xx/
# create symlink for kraken database
ln -s ~/data/dbs/kraken/std_8g db/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```


# **3. Mapping of reads to a reference genome using snippy**

# **4. Build a phylogenetic tree from variant sites**

# **5. Visualise a phylogenetic tree using Microreact**

# **6. Overlay genotyping data and epidemiological data for interpretation – “genomic epidemiology”**







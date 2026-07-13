# Antimicrobial resistance variant detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

Insert diagram -  public health microbial genomics workflow &  indicate which step we at 

## 1.1 Practical Overview


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
mkdir --parents ~/Practical_amr_variant/ xxx
mkdir -p ~/Practical_amr_variant/ xx
mkdir -p ~/Practical_amr_variant/ xx
```

## 2.3 Get data
The data for today's practical is located in `~/data/amr_variant`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_amr_variant
# create symlinks for all fastq files
ln -s ~/data/Practical_amr_variant/*.fastq.gz xx/
# create symlink for reference genome
ln -s ~/data/Practical_amr_variant/ xyz.fasta xx/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. AMR variant detection**

## 3.1 Annotate reference genome 

``` bash
`prokka` `bacta`
```

## 3.2 Identify variants 

do with snippy using .gff annotation as reference 

``` bash
`snippy` 
```

## 3.3 AMR variants of interest 
name of the gene - .tsv from prokka

## 3.4 Visualise AMR variants 

--> use IGV




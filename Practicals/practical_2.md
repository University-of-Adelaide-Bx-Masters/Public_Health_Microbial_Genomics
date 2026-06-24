# Bacterial genotyping
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
mkdir --parents ~/Practical_bacterial_genotyping/ xxx
mkdir -p ~/Practical_bacterial_genotyping/ xx
mkdir -p ~/Practical_bacterial_genotyping/ xx
```

## 2.3 Get data
The data for today's practical is located in `~/data/bacterial_genotyping`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_bacterial_genotyping
# create symlinks for all fastq files
ln -s ~/data/Practical_bacterial_genotyping/*.fastq.gz xx/
# create symlink for reference genome
ln -s ~/data/Practical_bacterial_genotyping/xyz.fasta xx/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Assigning strains an MLST using the `mlst` tool**

The `mlst` tool by Torsten Seemann scans bacterial genome assemblies (in FASTA format) against PubMLST typing schemes and reports the sequence type.

### 3.1 Before running `mlst` we can check what schemes are available and confirm the M. tuberculosis scheme is listed

```bash
# List all available schemes
mlst --list

# Check if M. tuberculosis scheme is listed - we can use grep
mlst --list | grep tuberculosis
```
you should see `mtuberculosis` in the output, confirming the scheme is available. 

### 3.2 Run `mlst` 

`mlst` requires a genome assembly (FASTA format) file as input. 

Example command for one sample:

```bash
mlst todo.fasta todo.fasta
```

`mlst` returns a tab-seperated line containing:
- the filename
- the macthing PubMLST schema name
- the ST (sequence type)
- the allel IDs

Task:
- Repeat `mlst` for the remaining samples and direct the results to an `output` file 

```bash 
mlst --quiet *.fasta > tb_mlst_results.txt
```
Task: 
- View the results on the terminal
- Count how many isolates belong to each ST 
- Extract isolates belonging to STx

## Questions
1. to test conceptial undertanding
2. allele intepretation 

# **4. Perform Core-genome MLST using `chewBBACA`**

`chewBBACA` is a commonly used tool to xyz 


### 4.2 Download the Salmonella enterica core genome MLST schema

`chewbbaca` requires a scheme for xyz 

The Salmonella enterica core genome MLST schema has already been downloaded for you and is located here `~/data/bacterial_genotyping`


### 4.3 Adapt the Salmonella enterica core genome MLST schema so that it can be used with `chewBBACA`

bla bla bla

```bash
chewBBACA.py PrepExternalSchema -i downloaded_schema_to_do/ -o salmonella_schema_adapted --cpu 4
```

### 4.4 Performe allele calling on the Salmonella enterica genome allemblies

`chewBACCA` requires genome assemblies (FASTA) as input files

run `chewBACCA` to determine the allelic profiles of the Salmonella enterica genomes 

```bash
chewBBACA.py AlleleCall -i genomes_to_analyze -g salmonella_schema_adapted -o allele_calling_results --cpu 4 --bsr 0.6(WHICH VALUE TO USE HERE!!!!)
```
The main output fils is `allele_calling_results.tsv`, which is a tab delimited file with"
- Rows: genome assemblies
- Columns: schema loci
- Values: allele identiffiers or classification code 

# **5. Calculate distance matrix from cgMLST allele call tables of ChewBBACA**

use `cgmlst-dists`

# **6. Comparison of MLST and core-genome MLST results**

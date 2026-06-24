# Bacterial genotyping
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

Insert diagram -  public health microbial genomics workflow &  indicate which step we at 

## 1.1 Practical Overview

In practice, MLST and cgMLST are often used together:
   -  MLST first: Quick screening to identify strain types
   - cgMLST second: High-resolution analysis of isolates of interest
   - Combined interpretation: MLST for context, cgMLST for detail


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

### 4.5 schema evaluator

### 4.6 Allele call evaluator

### 4.7 Determine the set of loci that make up the core genome 

Not all loci are present in all genomes.

To focus on the core genome (loci present in ≥95% or 100% of isolates) we will run `chewBBACA` as here:

```bash
# Extract core genome loci
chewBBACA.py ExtractCgMLST -i allele_calling_results.tsv \
                            -o cgmlst_results \
                            --t 0.95 \
                           #(OPTIONAL) --r allele_calling_results/RepeatedLoci.txt
```

Parameters explained:
-i: Input allele calling results file
-o: Output directory
--t: Threshold (0.95 = loci present in ≥95% of genomes)
--r: File with repeated loci to exclude (optional)

Outputs:
- cgMLST.tsv: Allele profiles for core genome loci only
- cgMLSTschema.txt: List of core genome loci
-  mdata_stats.tsv: Statistics on missing data per genome

### 4.8. Calculate distance matrix from cgMLST allele call tables of ChewBBACA

use `cgmlst-dists`

### 4.9. Cluster at various thresholds (5, 10, 20 - MIGHT HAVE TO WRITE CUSTOM SCRIPT)


# **5. Visual comparison of MLST and core-genome MLST results**

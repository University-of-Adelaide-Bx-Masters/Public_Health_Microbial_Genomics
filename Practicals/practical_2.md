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
mkdir --parents ~/Practical_bacterial_genotyping/{assembly,cgmlst,db,mlst}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_bacterial_genotyping
# create symlinks for all genome assembly (FASTA format) files
ln -s ~/data/public_health_genomics/microbial_genomics/assemblies/*.fasta assembly/
# create symlink for cgmlst schema 
ln -s ~/data/public_health_genomics/microbial_genomics/TO DOOO
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Assigning strains an MLST using the `mlst` tool**

The `mlst` tool by Torsten Seemann scans bacterial genome assemblies (in FASTA format) against PubMLST typing schemes and reports the sequence type.

Run this to get the help information for mlst

```bash
mlst -h
```

### 3.1 Before running `mlst` we can check what schemes are available

```bash
# List all available schemes
mlst --list

# Check if salmonella scheme is listed - we can use grep
mlst --list | grep salmonella
```
you should see `salmonella` in the terminal output highlighted in red, confirming the scheme is available. 

### 3.2 Run `mlst` over one sample

`mlst` requires a genome assembly (FASTA format) file as input. 

Example command for one sample:

```bash
mlst assembly/ERR10479021.fasta
```
You should see something like this on the terminal - you can see that running the above command has returned alot of information, but we are most interested in one of the lines. This line contains the mlst information that we are interested in. 

```bash
This is mlst 2.35.0 running on linux with Perl 5.032001
Checking mlst dependencies:
Found 'blastn' => /apps/conda3/singularity/envs/bioinf/bin/blastn
Found 'any2fasta' => /apps/conda3/singularity/envs/bioinf/bin/any2fasta
Excluding 4 schemes: abaumannii senterica_achtman_2 vcholerae_2 ecoli
Running: any2fasta -q assembly\/ERR10479021\.fasta > /tmp/9CN_VXuNnA/mlst.fna
Running:  blastn -query /tmp/9CN_VXuNnA/mlst.fna -out /tmp/9CN_VXuNnA/mlst.bls -db \/apps\/conda3\/singularity\/envs\/bioinf\/db\/blast\/mlst\.fa -num_threads 1 -ungapped -dust no -word_size 32 -max_target_seqs 100000 -perc_identity 95 -evalue 1E-20 -outfmt '6 sseqid slen length nident qseqid qstart qend qseq sstrand'
Found exact allele match salmonella.hisD-1135
Found exact allele match ecoli_achtman_4.fumC-532
Found exact allele match salmonella.purE-5
Found exact allele match salmonella.sucA-192
Found exact allele match salmonella.aroC-5
Found exact allele match salmonella.thrA-1
Found exact allele match salmonella.hemD-3
Found exact allele match salmonella.dnaN-67
assembly/ERR10479021.fasta      salmonella      5438    aroC(5) dnaN(67)        hemD(3) hisD(1135)      purE(5) sucA(192)       thrA(1)
If you like MLST, you're absolutely going to love wgMLST!
```

`mlst` has a special flag that we can use to get quiet output by using the `--quiet` option. 

Now try this and take note of what is printed on the terminal 

```bash
mlst --quiet assembly/ERR10479021.fasta
```

On the terminal you should see the `mlst` command returned a tab-seperated line (this is the line that we are most interested in) containing:
- The filename
- The macthing PubMLST schema name
- The Sequence Type (ST)
- Genes and their allele IDs

<img width="1265" height="342" alt="image" src="https://github.com/user-attachments/assets/663749fc-2dad-4df5-80a6-0cfb8ecc15a0" />


The command we ran above generally autodetects an appropriate bacterial schemea to use. However their may be times where an incorrect schema is selected by the `mlst` tool. This can happen for example between closeley related bacterial species (e.g. shigella and e.coli), where the house keeping genes are genetically similar. To overcome this, you can force `mlst` to use a specific scheme by adding the option `--schema` followed by the name of the schema - in our case we are working with salmonella samples so we will use the `salmonella` schema: 

```bash
mlst --scheme salmonella --quiet assembly/ERR10479021.fasta
```
Looking at the results you can see that we get the same ST, genes and allele IDs when we forced the schema campared to when we didnt force the schema - lucky! 

### 3.2 Run `mlst` over all samples

Task:
- Repeat `mlst` for the remaining samples and direct the results to an `output` file named `salmonella_mlst_results.txt`

```bash 
mlst --quiet *.fasta > salmonella_mlst_results.txt
```

Task: 
- View the results on the terminal
- How many STs are present in the salmonella samples?
- Count how many isolates belong to each ST 
- Extract isolates belonging to STx

## Questions
1. to test conceptial understanding
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

# **5. Calculate distance matrix from cgMLST allele call tables of `ChewBBACA`**

Calculate pairwise allelic differences

Run `cgmlst-dists` to calculate distance matrix from cgMLST allele call table that was generated by `chewbBACA`

``` bash
cgmlst-dists pathto/allele_call.tab > distance.tab
```

# **6. Cluster isolates using various (5, 10, 20) allelic differences thresholds**

single linkage clustering 

``` bash
custom python script???
```

# **7. Visual comparison of MLST and core-genome MLST results**

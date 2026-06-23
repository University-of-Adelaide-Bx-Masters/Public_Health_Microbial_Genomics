# Variant calling for phylogenetic trees & application to public health 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**
Whole-genome sequencing (WGS) of bacterial pathogens has transformed public health microbiology. By comparing the genomes of bacterial isolates, we can determine how closely related they are, identify outbreaks, and track the spread of antimicrobial resistance. 

The figure below is an example of a microbial genomics workflow that may be undertaken in a public health laboratory. In Practicals 1 and 2 you learned about species classification and microbial genotyping, this pratcial will focus on the steps highlighted in blue in the below figure. Which includes sequence read mapping and variant calling for constructing phylogenomic trees and visualise the tree in microreact with epidemiological data.  

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
```

## 2.3 Get data
The data for today's practical is located in `~/data/variants_trees`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_phylogenetic_trees_surveillance
# create symlinks for all fastq files
ln -s ~/data/Practical_phylogenetic_trees_surveillance/*.fastq.gz xx/
# create symlink for reference genome
ln -s ~/data/Practical_phylogenetic_trees_surveillance/xyz.fasta xx/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Mapping sequencing reads to a reference genome using snippy**

A very talented bioinformatician who is a friend of mine (Torsten Seemann) wrote Snippy (https://github.com/tseemann/snippy). Snippy is a widely used pipeline that maps reads to a reference genome, to identify variants and produces a range of outputs. snippy uses BWA-MEM, SAMtools and FreeBayes, which we will learn more about in this practical. If you have done one of our bioinformatics courses you may find snippy familiar, however as a bioinformatician snippy will likely become one of the tools that you will most frequently use and so in this practical we will be looking at snippy in more detail. 

## 3.1 Run Snippy for variant calling 

Snippy requires: 

- A reference genome (FASTA or GENBANK format)
- Single- or paired-end sequencing reads from one or more isolates
- An output folder to put results in

We can explore the snippy help page to understand the available options:

```bash
snippy --help
```

The key parameters of Snippy are:

Parameter	Description
- outdir:	Directory where Snippy will write its output files
- R1:	Path to the forward (R1) FASTQ file
- R2: Path to the reverse (R2) FASTQ file
- ref:	Path to the reference genome (FASTA format)
- cpus:	Number of CPU threads to use (default: 8)
- ram:	Maximum RAM in GB (default: 8)
- minqual:	Minimum variant quality score (default: 100)
- minfra: Minimum allele frequency to call a variant (default: 0.9)
- prefix: Prefix for output files (default: snps)

Now the fun part, run snippy for each sample. Example command for one sample:

```bash
#Note: This is one long command
snippy --outdir /todo/ --ref
references/pathtorftodo.fa --R1
/todo/ 1.fastq.gz --R2
/todo/ _2.fastq.gz 
```
Now we wait for snippy to finish this should take ~xx for one sample 

#### Task: Repeat snippy for the remaining xx samples

## 3.3 Examine snippy logs 
To better understand how Snippy processes sequencing reads and generates variant calls, we will inspect the snp.log output file. The log records the commands executed during the analysis, allowing you to trace each stage of the snippy pipeline, including read alignment, BAM processing, variant calling and variant filtering. If you run a tool and it fails – information on why the tool failed to run will often be in the .log file – so this is an important file. 

We will use `grep` to look at what is happening in the snippy log files and to understand the order in which the core tools used by snippy are executed. 

## 3.4 Now let’s look at some of the snippy output files 
Snippy 

# **4. Run snippy core to create input for building a tree** 
 
We now need to created a core genome SNP alignment to input onto our tree building tool, to do this we use snippy-core. 

Now run snippy-core to generate the core gneome SNP alignment files across all samples: 




# **5. Build a phylogenetic tree from variant sites**

# **6. Visualise a phylogenetic tree using Microreact**

# **7. Overlay genotyping data and epidemiological data for interpretation – “genomic epidemiology”**







# Species classification
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="804" height="583" alt="image" src="https://github.com/user-attachments/assets/9adc05b6-a0ff-4e59-9a03-e8d57d3fc899" />





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
mkdir --parents ~/Practical_species_classification/{kraken,fastani,reads,reference,db,assembly}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_species_classification
# create symlinks for all fastq files
ln -s ~/data/public_health_genomics/microbial_genomics/*.fastq.gz reads/
# create symlinks for all genome assembly (FASTA format) files
ln -s ~/data/public_health_genomics/microbial_genomics/assemblies/*.fasta assembly/
# create symlink for reference genome
ln -s ~/data/public_health_genomics/microbial_genomics/GCA_000009505.1_ASM950v1_genomic.fasta reference/
# create symlink for kraken database
ln -s ~/data/dbs/kraken/std_8g db/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:

```bash
.
├── assembly
│   ├── ERR10479021.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479021.fasta
│   ├── ERR10479025.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479025.fasta
│   ├── ERR10479028.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479028.fasta
│   ├── ERR10479029.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479029.fasta
│   ├── ERR10479032.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479032.fasta
│   ├── ERR10479034.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479034.fasta
│   ├── ERR10479035.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479035.fasta
│   ├── ERR10479037.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479037.fasta
│   └── ERR10479039.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/assemblies/ERR10479039.fasta
├── db
│   └── std_8g -> /shared//a1237649/data/dbs/kraken/std_8g
├── fastani
├── kraken
├── reads
│   ├── ERR10479021_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479021_1.fastq.gz
│   ├── ERR10479021_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479021_2.fastq.gz
│   ├── ERR10479025_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479025_1.fastq.gz
│   ├── ERR10479025_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479025_2.fastq.gz
│   ├── ERR10479028_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479028_1.fastq.gz
│   ├── ERR10479028_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479028_2.fastq.gz
│   ├── ERR10479029_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479029_1.fastq.gz
│   ├── ERR10479029_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479029_2.fastq.gz
│   ├── ERR10479032_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479032_1.fastq.gz
│   ├── ERR10479032_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479032_2.fastq.gz
│   ├── ERR10479034_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479034_1.fastq.gz
│   ├── ERR10479034_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479034_2.fastq.gz
│   ├── ERR10479035_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479035_1.fastq.gz
│   ├── ERR10479035_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479035_2.fastq.gz
│   ├── ERR10479037_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479037_1.fastq.gz
│   ├── ERR10479037_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479037_2.fastq.gz
│   ├── ERR10479039_1.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479039_1.fastq.gz
│   └── ERR10479039_2.fastq.gz -> /shared//a1237649/data/public_health_genomics/microbial_genomics/ERR10479039_2.fastq.gz
└── reference
    └── GCA_000009505.1_ASM950v1_genomic.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/GCA_000009505.1_ASM950v1_genomic.fasta
```

# **3. Species classification using Kraken2**

We need to determine the species present in a sample, and `Kraken2` is a very useful tool for this purpose. It is extremely fast, making it well suited for public health settings where rapid results are essential. As a result, `Kraken2` is often used as an initial screening tool to identify the species present in a sample before more detailed analyses are performed.

### 3.1 Run Kraken2 

The input files for `kraken2` are assumed to be genome assemblies (FASTA) by default, but `kraken2` can also take other files as input including sequencing reads (FASTQ). For today we will run `kraken2` over sequencing reads.  

To investigate the general command structure of `Kraken2` type:

``` bash
kraken2
```
On your terminal you should see the options for running `kraken2` 

We will run `kraken2` using the following options:
```bash
Options:
  --threads NUM           Number of threads (default: 1)
  --db NAME               Name for Kraken 2 DB
                          (default: none)
  --output FILENAME       Print output to filename (default: stdout); "-" will
                          suppress normal output
  --report FILENAME       Print a report with aggregrate counts/clade to file
  --paired              The filenames provided have paired-end reads (
 ```

Now run `kraken2` on one isolate using the following command:

``` bash
kraken2 --threads 2 --db db/std_8g --output -  --report kraken/ERR10479037.report --paired reads/ERR10479037_1.fastq.gz reads/ERR10479037_2.fastq.gz
```

`kraken2` generates a report of all taxa found in the sample, we wont be looking at this report in detail but we will use it as input for `bracken` in step 3.2.  


### 3.2 Run Bracken

`bracken` (Bayesian Reestimation of Abundance with Classification KrakEN) is a companion tool to Kraken2 that improves species- or genus-level abundance estimates.

Why do we need to use `bracken`?
- `kraken2` classifies each read to the lowest taxonomic level it can confidently assign. Because many species share identical genomic regions, some reads are assigned only to a higher taxonomic rank (e.g., genus instead of species). This means simply counting `kraken2` species assignments can underestimate the abundance of some species in a sample.

`bracken` requires: 
- The `kraken2` report (This can be found here, kraken/ERR10479037.report)
- the `kraken2` database (This can be found here, db/std_8g) 

View the options for running `bracken`

```bash
bracken -h
```

We will run `bracken` with the following command: 

```bash
bracken -d db/std_8g -r 150 -i kraken/ERR10479037.report -o kraken/ERR10479037.bracken
```

Where:
- -d = Kraken2 database
- -r = read length (150 bp)
- -i = Kraken2 report
- -o = Bracken output

Important to note that we are running with the -r option, as the read length of our sequencing reads are 150bp and not the default which is 100bp. 

The important result from `bracken` is the species focused results table (found here: kraken/ERR10479037_bracken) 

Have a look at the file on the terminal

- xx
- xx
  
### QUESTIONS
- What bacterial species is present in the sample?  
-

### 3.1 Run Kraken2 and Bracken over all samples 


# **4. Species classification using `fastANI`**



### 4.1 Run `fastANI` 


### QUESTIONS

# **5. Compare `kraken2` and `fastANI` results**


### QUESTIONS







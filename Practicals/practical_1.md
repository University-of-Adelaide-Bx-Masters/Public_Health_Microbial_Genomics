# Species classification
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
mkdir --parents ~/Practical_species_classification/{kraken,fastani,reads,reference,db}
```

## 2.3 Get data
The data for today's practical is located in `~/data/species_classification`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_species_classification
# create symlinks for all fastq files
ln -s ~/data/public_health_genomics/species_classification/*.fastq.gz reads/
# create symlink for reference genome
ln -s ~/data/public_health_genomics/species_classification/xyz.fasta reference/
# create symlink for kraken database
ln -s ~/data/dbs/kraken/std_8g db/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Species classification using Kraken2**

We need to determine the species present in a sample, and Kraken is a very useful tool for this purpose. It is extremely fast, making it well suited for public health settings where rapid results are essential. As a result, Kraken is often used as an initial screening tool to identify the species present in a sample before more detailed analyses are performed.

### 3.1 Run Kraken2 

The input files for `kraken2` are assumed to be genome assemblies (FASTA) by default, but `kraken2` can also take other files as input including sequencing reads (FASTQ). For today we will run `kraken2` over sequencing reads.  

View the option for running `Kraken2` 

``` bash
kraken2 --help
```
Run `Kraken2` on one isolate using the following command:

``` bash
kraken2 --threads 2 --db db/std_8g --output --report addpath/nameofreads.report --paired addpath/toadd_1.fastq.gz addpath/toadd_2.fastq.gz
```

The command includes
- todo
  
`kraken2` generates the following file:
- x

### 3.2 Run Bracken

The `kraken2` tool does not xyz and thus we next need to run a tool called `bracken`

View the options for running `bracken`

```bash
bracken -h
```

Run `bracken` with the following command 

```bash
bracken -d db/std_8g -r 150 -i addpath/nameofreads.report -o addpath/todo.bracken
```

`bracken` generates two files:
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







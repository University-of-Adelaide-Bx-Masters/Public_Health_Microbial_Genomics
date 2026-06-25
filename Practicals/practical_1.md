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
mkdir --parents ~/Practical_species_classification/ xxx
mkdir -p ~/Practical_species_classification/ xx
mkdir -p ~/Practical_species_classification/ xx
```

## 2.3 Get data
The data for today's practical is located in `~/data/species_classification`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_species_classification
# create symlinks for all fastq files
ln -s ~/data/Practical_species_classification/*.fastq.gz xx/
# create symlink for reference genome
ln -s ~/data/Practical_species_classification/xyz.fasta xx/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Species classification using Kraken 2**

We need to confirm which species is present, and Kraken is a very useful tool for this purpose. It is extremely fast, making it well suited for public health settings where rapid results are essential. As a result, Kraken is often used as an initial screening tool to identify the species present in a sample before more detailed analyses are performed.

### 3.1 Run `kraken 2` 

The input files for `Kraken 2` are assumed to be genome assemblies (FASTA) by default, but `Kraken 2` can also take other files as input including sequencing reads (FASTQ). For today we will run `Kraken 2` over sequencing reads.  

Run `Kraken 2` using the following command:

``` bash
kraken2 --threads 2 --db db/std_8g --output --report addpath/nameofreads.report --paired addpath/toadd_1.fastq.gz addpath/toadd_2.fastq.gz
```

# **4. Species classification using `fastANI`**

### 4.1 Run `fastANI` 


# **5. Compare `kraken` and `fastANI` results**






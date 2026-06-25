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

# **3. Species classification using Kraken**

we need to confirm what species is present and `kraken` is a very useful tool for this. The tool is very fast and so in public health settings we want a quick result and so this tool is often used for an initual screen of isolates to identy the species prsent in a sample.  

### 3.1 run `kraken 2` 

Input files to `Kraken 2` can be genome assemblies (FASTA)  or sequencing reads (FASTQ)

Run `Kraken 2` using the following command:

``` bash
bla bla bla
```

# **4. Species classification using `fastANI`**


# **5. Compare `kraken` and `fastANI` results**






# Species classification
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

### Pathogen genomics landscape in Australia

Australia is divided into states and territories, known collectively as jurisdictions. Each jurisdiction is responsible for delivering its own public health services, including the surveillance, diagnosis, and management of infectious diseases. To support these activities, each jurisdiction has a central public health laboratory that provides specialised diagnostic testing, disease surveillance, outbreak investigation, and pathogen genomics services for its population.

Many infectious diseases in Australia are classified as notifiable diseases. This means that when a laboratory or healthcare provider identifies a case of one of these diseases, they are legally required to notify the relevant state or territory public health authority. These notifications enable public health teams to monitor disease trends, rapidly detect outbreaks, implement control measures, and coordinate responses to protect the community.

As part of the public health response, selected pathogens are referred to the jurisdictional public health laboratory for whole-genome sequencing. Every year a public health laboratory may recieve 100s of thousands of pathogens for sequencing and so a streamlined genomic workflow is required to promtly identy and characterise isolates. The resulting genomic data are analysed using pathogen genomics workflows to determine characteristics such as species identification, genotyping, genetic relatedness, phylogenomics and antimicrobial resistance gene and variants. Genomic information can then be combined with epidemiological data, we can then identify transmission links, detect outbreaks, monitor the emergence of new variants or resistant strains, and inform public health interventions.

The public helath microbial genomcis workflow can look a bit like the below workflow figure, and for the Public Health Genomics module we will work through a number of the workflow steps during the practical sessions. One of the very first steps is to identfy what bacterial species the corresponding sample is, this confirms what pathogen is present and once this is known we can then perform downstream analysis (e.g. genotyping, phylogenomics, antimicrobial resistance investigations). 

<img width="804" height="583" alt="image" src="https://github.com/user-attachments/assets/9adc05b6-a0ff-4e59-9a03-e8d57d3fc899" />


## 1.1 Practical Overview
For this practcial you will be working with paired-end illumina sequencing data (FASTQ) and their corrsponding genome assemblies (FASTA) from 9 bacterial isolates. You will work thorugh the practical to determine the bacterial species that is present in the 9 isolates.

In this practical you will be testing different tools for bacterial species classification including `kraken2`, `bracken` and `FastANI`. These three tools complement one another and are often used together. 

`kraken2` classifies sequencing reads by comparing short k-mers to a reference database. It tells you which organisms/organism are present in a sample.
`bracken` builds on Kraken's results to provide more accurate estimates of species abundance, correcting for reads that are shared between closely related organisms.
`FastANI` compares whole genome sequences (in FASTA format) and calculates the Average Nucleotide Identity (ANI) between them.


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
  --paired                The filenames provided have paired-end reads
 ```

Now run `kraken2` on one isolate using the following command:

``` bash
kraken2 --threads 2 --db db/std_8g --output -  --report kraken/ERR10479037.report --paired reads/ERR10479037_1.fastq.gz reads/ERR10479037_2.fastq.gz
```

`kraken2` generates a report of all taxa found in the sample, we wont be looking at this report in detail but we will use it as input for `bracken` in step 3.2.  


# **4. Species classification using Bracken**

`bracken` (Bayesian Reestimation of Abundance with Classification KrakEN) is a companion tool to `kraken2` that improves species or genus-level abundance estimates.

Why do we need to use `bracken`?
- `kraken2` classifies each read to the lowest taxonomic level it can confidently assign. Because many species share identical genomic regions, some reads are assigned only to a higher taxonomic rank (e.g., genus instead of species). This means simply counting `kraken2` species assignments can underestimate the abundance of some species in a sample.

`bracken` requires: 
- The `kraken2` report (This can be found here, kraken/ERR10479037.report)
- the `kraken2` database (This can be found here, db/std_8g) 

### 4.1 Run Bracken
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

The important result from `bracken` is the species focused results table (found here: kraken/ERR10479037.bracken). Important columns are the `name`, `new_est_reads` and `fraction_total_reads`. 

### QUESTIONS
Have a look at the `bracken` output file (ERR10479037_bracken) on the terminal (you can use the `head` command for this)
- Can you confirm what species of bacteria is present in our sample? 
- What is the percentage of reads in our sample that belong to this species?  
  
# **5 Run Kraken2 and Bracken over all samples** 

You have run `kraken2` and `bracken` over one sample, we now need to run the tools over the remaining samples so that we can confirm what bacteiral species is present. 

Since we have 8 samples to process, it will make our code much simpler if we use a for loop. The script below uses steps that you may have learned about if you have completed a bioinformatics course at Adelaide Uni. Make sure you understand what each step is doing and ask for help if you dont understand. It is important that you understand both the setup and the contents of this script, this is because you will need to submit scripts as appart of the assignments for this course. 

In order to run the script:
- Open a file called species.sh by typing nano species.sh
- Copy the text from the code block below into the nano editor
- Save the file and close nano by holding down Ctrl and typing x, then type y, then press Enter
- To run the script, type bash species.sh. It should take around 5 minutes to run.

```bash
#!/bin/bash

# Load software
source activate bioinf

# List of samples with Illumina data
SAMPLES=(ERR10479025 ERR10479028 ERR10479029 ERR10479032 ERR10479034 ERR10479035 ERR10479039)

# Loop over each sample
for SAMPLE in "${SAMPLES[@]}";
do

# Run kraken2 over the samples
kraken2 \
        --threads 2 \
        --db db/std_8g \
        --output - \
        --report kraken/${SAMPLE}.report \
        --paired reads/${SAMPLE}_1.fastq.gz reads/${SAMPLE}_1.fastq.gz

# Run bracken over the samples
bracken \
        -d db/std_8g \
        -r 150 \
        -i kraken/${SAMPLE}.report \
        -o kraken/${SAMPLE}.bracken
done
```

### QUESTIONS
Have a look at the `bracken` output files on the terminal
- Can you confirm what species of bacteria is present in the samples? 
- What is the percentage of reads in the samples that belong to this species?
- Do all samples belong to the same species?

# **6. Species classification using `fastANI`**

You have now had a play with identifying species in samples using `kraken2` and `bracken`. In this section of the practical we will use and become familiar with `fastANI` for species classifiction.  

Above `kraken2` and `bracken` gave us an indication of "What species is there, and how much?", whilst `fastANI` answers "How genetically similar is this genome to another genome?".

ANI thresholds and their meanings:
- ≥ 95%	ANI, same species
- <83% ANI, different species 

### 6.1 Run `fastANI` 

`fastANI` supports three comparison modes: 
- one query genome to one reference genome
- one query genome to many reference genomes
- many query genomes to many reference genomes 

You can look at the options to run `fastANI`

```bash
fastANI -h
```
First lets do a one to one comparison. In this instance we will compare one of our Salmonella enterica samples against a reference genome (we already know that this genome is S. enterica): 

```bash
fastANI -q assembly/ERR10479037.fasta -r reference/GCA_000009505.1_ASM950v1_genomic.fasta -o fastani/ERR10479037
```
and use the `cat` command to view the results 

```bash
cat fastani/ERR10479037
```
You should get something that looks like this - with five columns: 

```bash
assembly/ERR10479037.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0338 1483    1529
``` 
Column descriptions:
- Query genome: Path to query genome file
- Reference genome: Path to reference genome file
- ANI value: Average Nucleotide Identity (percentage)
- Mapped fragments: Number of query fragments that aligned to reference
- Total fragments: Total number of query fragments

### QUESTIONS
- What does an ANI % of 99.0338% indicate? does this indicate that the sample and reference are the same species? 
- 1483 out of 1529 fragments mapped. Is this a good number?

### 6.2 Run `fastANI` over the remaning samples 
Now run `fastANI` over the remaining samples using the below. You need to create a script and copy the contents below into it and then save and run. You learnt how to do this earlier in the practical.  

```bash
#!/bin/bash

# Load software
source activate bioinf

# List of samples with Illumina data
SAMPLES=(ERR10479025 ERR10479028 ERR10479029 ERR10479032 ERR10479034 ERR10479035 ERR10479039)

# Loop over each sample
for SAMPLE in "${SAMPLES[@]}";
do

        # Run fastANI over the samples
        fastANI \
        -q assembly/${SAMPLE}.fasta \
        -r reference/GCA_000009505.1_ASM950v1_genomic.fasta \
        -o fastani/${SAMPLE}
done
```
Then use `cat` command and wildcard `*` to print the results for each sample to the terminal: 

```bash
cat fastani/E*
```

You should see the following:

```bash
assembly/ERR10479025.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0572 1476    1542
assembly/ERR10479028.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0485 1473    1522
assembly/ERR10479029.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.02   1478    1526
assembly/ERR10479032.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0909 1412    1455
assembly/ERR10479034.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.081  1470    1519
assembly/ERR10479035.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0612 1474    1521
assembly/ERR10479037.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0338 1483    1529
assembly/ERR10479039.fasta      reference/GCA_000009505.1_ASM950v1_genomic.fasta        99.0423 1479    1527
```
### Question 
- What do the ANI % indicate? are the sample and reference the same species? 
- are all of the samples the same species? 

# **7.`kraken2`, `bracken` and `fastANI` results**

In our samples `kraken2` identified reads as Salmonella enterica and `bracken` estimated that Salmonella enterica makes up ~99% of reads in all our samples. Those tools tell you what is present and in what abundance.

We then took the Salmonella enterica genome assemblies and compared them with a reference genome (that we already know is Salmonella enterica) using `fastANI`, we found that:

Across all our samples when compared to the reference, ANI = ~99% → all genomes are nearly identical to the reference genome. 

This is a further confirmation that our samples belong to Salmonella enterica - well done. We can also have instances whereby `kraken2`, `bracken` are not able to deliniate the species present in a sample and in that case `fastANI` can be used to determine the species. 





# (Re)introducing BASH - A Crash Course
By Chelsea Matthews

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

## 1.1 Practical Overview

This practical serves to re-familiarise students in their second semester of Bioinformatics with BASH. 
It assumes students have completed one semester of Bioinformatics and is designed to remind and reintroduce, and not to teach from scratch. 

It runs for a single 2 hour practical and has three parts:
- Commonly used commands
- Data Wrangling
- BASH scripts

## 1.2 Learning Outcomes

1. Re-familiarise yourself with navigating the terminal and using common BASH commands
2. Practice data wrangling in BASH using common commands and regular expressions
3. Understand the purpose of BASH scripting and how to write and comment simple scripts

## 1.3 Why is BASH and BASH scripting essential for Bioinformatics? 

**Linux is the standard platform for Bioinformatics**

Most bioinformatics software is designed to run on Linux. 
Large analyses, such as genome assembly and sequence alignment, often require more computing power than a personal computer can provide and are instead run on Linux-based high-performance computing (HPC) clusters or cloud platforms. 
These systems typically have no graphical interface, so Bash is used to navigate files, run software, and manage analyses.

**Automation and Scalability**

Many bioinformatics analyses are repetitive and can take hours or days to complete. Bash scripts automate these tasks, allowing the same workflow to be applied consistently to hundreds or thousands of samples. This saves time, reduces manual effort, and makes analyses easy to scale.

**Reproducibility and reducing human error**

Bash scripts provide a complete record of the commands used to analyse data, making analyses easy to reproduce and share. 
This allows other researchers to verify results and to run the same process on a different dataset.
They also reduce human error by ensuring every sample is processed using the same commands, avoiding mistakes such as incorrect file names, missing options, or overwritten outputs.

# **2. Login and Setup**

Log into your Virtual Machine. [Click here](https://university-of-adelaide-bx-masters.github.io/Fundamentals_of_Bioinformatics/Course_materials/vm_login_instructions.html) for instructions if you can't remember how. 

We will also be using some of the software provided by the `bioinf` conda environment. 
Activate it as below. 

```bash
source activate bioinf
```

# **3. Commonly used commands**

In this section, we'll review a few simple commands/skills which we will then use to work out what a number of other commands are doing.

## 3.1 Where am I?
**Check your command prompt or use `pwd` - Print Working Directory**

The terminal command prompt on your VM includes the path to your current location - your working directory.
In the example below, the user is in their home directory `/shared/a1761942`.

<img src="images/command_prompt_annotated.png" alt="Terminal command prompt" width="500">


Alternatively, you can use the `pwd` command to find your current working directory. 
While this isn't super useful when we're working in our VMs, other terminal command prompts may not include the current working directory so it's worth knowing. 

Run the following `cd` commands one by one and watch how your command prompt path changes. Type the commands by hand and don't forget you can use TAB autocomplete.

```bash
# change to the root directory
cd /
# change to the data directory using its absolute path
cd /shared/data
# change to the public_health_genomics directory inside of the data directory
cd public_health_genomics
# change to your home directory
cd ~
```

**Questions:**
- What does the `~` mean?
- An absolute path always points to the same location, regardless of your current directory, while a relative path specifies a location __relative__ to your current working directory. Which of the commands above are absolute and which are relative? 

## 3.2 What else is here?
**`ls` - list**

The `ls` command is an abbreviation of list. It lists the contents of a directory. 

Try the commands below to see the difference in output. 

```bash
cd ~
# list contents of current directory
ls
# long, human readable list of current directory
ls -lh 
# long list contents of the intro_bash directory
ls -lh /shared/data/intro_bash
```

**Questions:**
- How many files are in the `intro_bash` directory? 
- Which file is biggest and how big is it?
- When were these files last modified? 

## 3.3 Look at contents of a file
**`less filename`**

Use the `less` command to view the contents of a file. 

```bash
# view contents of the words file
less /shared/data/intro_bash/words
```

Move around the file by scrolling with your mouse or using the spacebar to page down and `b` to page back up. 
Some terminals don't allow mouse scrolling so the keyboard commands are good to know. 

When you're done looking, press `q` to exit. 

You can view any plain-text file using `less`.

## 3.4 How to get help 
For inbuilt BASH commands: **`man commandname`**

For other tools/software: usually **`commandname --help`** OR  **`commandname -h`**

Here we're using inbuilt BASH commands so the `man` command will be most useful.
Open the manual for the `ls` command as below and answer the following questions (you will need to scroll):

```bash
man ls
```

**Questions:**
- What does it say the `ls` command does? 
- What do the `-l` and `-h` options do?
- What would the command `ls -lt` do?

To exit a manual page, press `q`.   


## 3.5 Investigating more common commands

Now you know how to:
- see where you are by checking the command prompt
- list the contents of a directory with `ls`
- see the contents of a file with `less filename`
- open the manual for a command with `man commandname`

Run each line of code below one at a time and in between, use the commands and techniques we just covered to find out what each one does. 
Write down your findigs to go through as a class. Make sure you know what a command does before moving on to the next one. 


```bash
cd ~ 

mkdir bash_crash_course

cd ~/bash_crash_course

touch notes.txt

echo "This is some very boring text" > notes.txt

echo "And this is some more very boring text" >> notes.txt

wget http://ftp.ensembl.org/pub/release-107/fasta/caenorhabditis_elegans/dna/Caenorhabditis_elegans.WBcel235.dna.toplevel.fa.gz

gunzip Caenorhabditis_elegans.WBcel235.dna.toplevel.fa.gz

mv Caenorhabditis_elegans.WBcel235.dna.toplevel.fa c_elegans.fa

head -n5 c_elegans.fa

tail c_elegans.fa

head c_elegans.fa >> notes.txt

wc -l notes.txt

cp notes.txt otherfile.txt 

rm c_elegans.fa

rm notes.txt otherfile.txt

ln -s /shared/data/intro_bash/words words.txt

rm words.txt

cd ..

du -sh ~
```

After running all of the commands above in order, there should be an empty directory `bash_crash_course` in your home directory and your working directory should be your home directory.

Using commands and techniques from above, try to do the tasks below.
- Change into the `~/bash_crash_course` directory
- Download GFF file at ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/182/855/GCF_000182855.2_ASM18285v1/GCF_000182855.2_ASM18285v1_genomic.gff.gz to this directory
- Unzip the GFF file
- Rename the GFF file to `lactobacillus.gff`
- Send first 25 lines of `lactobacillus.gff` to a file called `example.gff`


At the end of this section, you should have a directory called `bash_crash_course` in your home directory and it should contain two files, one called `lactobacillus.gff` and the other called `example.gff`.  
These files will be used in the next section. 



# **BASH scripts**

In this section we'll write a BASH script that runs a simple workflow on 3 samples. This script will:
- Create necessary directories
- Obtain reference genome and Illumina reads
- Run quality control with `fastp`
- Align reads to a reference genome with `bwa mem`
- Summarise alignment statistics with `samtools`


We will work through the process of testing code and building the script step by step. 

## Create script skeleton

You can use the code below as a skeleton for your script.
You can either create the script now using `nano` or you can use a text file (use notepad on Windows) and transfer the contents of the text file to a script once we're ready to start testing it. 
Call the script whatever you want (it's called `script.sh` in the examples) but make sure it ends with `.sh` and is located in your `~/bash_crash_course` directory. 


```bash
#!/bin/bash

# Load software
source activate bioinf

# Create directories

# Get data

# Run Quality Control

# Align reads to reference genome

# Summarise alignment statistics

```

The data we'll be using is located in the `/shared/data/bash_crash_prac/` directory.
Use `ls` and whatever options you like to answer the following questions about the contents of this directory: 
- Is the `reference.fa` file compressed?
- How big is the `reference.fa` file?
- When was the 'reference.fa' file last modified?
- How many samples are there?
- Are the files containing sample reads compressed? 

## Create directories

Some tools will fail if the specified output directory doesn't already exist. Therefore, it's good practice to create necessary directories up front.
This also helps us to stay organised. 

Make sure you're in the `bash_crash_course` directory and then write and run code in the terminal to create the directory structure shown below. 
Use `ls` or `tree` to check your progress. 

```txt
.
├── 0_data
├── 1_trimmed
│   ├── fastp
├── 2_aligned
├── 3_stats
└── script.sh
```

Add the code you used to your `script.sh` in the appropriate location. 

<details>
<summary>Code</summary>

```bash
mkdir 0_data
mkdir -p 1_trimmed/fastp
mkdir 2_aligned
mkdir 3_stats
```

</details>

## Get data

Write code to copy the reference genome `/shared/data/bash_crash_prac/reference.fa` to your `bash_crash_prac` directory. 

Also, create symlinks in the `0_data` directory to the fastq files in `/shared/data/bash_crash_prac/`. 

Once you've done this, running `tree` should show something like below:

```txt
.
├── 0_data
│   ├── sampleA_R1.fq.gz -> /shared/data/bash_crash_prac/sampleA_R1.fq.gz
│   ├── sampleA_R2.fq.gz -> /shared/data/bash_crash_prac/sampleA_R2.fq.gz
│   ├── sampleB_R1.fq.gz -> /shared/data/bash_crash_prac/sampleB_R1.fq.gz
│   ├── sampleB_R2.fq.gz -> /shared/data/bash_crash_prac/sampleB_R2.fq.gz
│   ├── sampleC_R1.fq.gz -> /shared/data/bash_crash_prac/sampleC_R1.fq.gz
│   └── sampleC_R2.fq.gz -> /shared/data/bash_crash_prac/sampleC_R2.fq.gz
├── 1_trimmed
│   ├── fastp
├── 2_aligned
├── 3_stats
├── reference.fa
└── script.sh

```

Add the code you used to your `script.sh`.

<details>
<summary>Code</summary>

```bash
# get reference
cp /shared/data/bash_crash_prac/reference.fa .

# symlinks to reads
ln -s /shared/data/bash_crash_prac/*.fq.gz 0_data/
```

</details>


## Run quality control

We will now use `fastp` to trim our reads. 
A skeleton `fastp` command is shown below.
Replace `<value>`s so that sampleA is trimmed making sure that:
- Trimmed reads are output to the `1_trimmed` directory and named `sampleA_R1.fq.gz` and `sampleA_R2.fq.gz`
- Reads are a minimum of 90bp long
- The mean quality of bases within a 4bp window is 25
- The html report is placed in `1_trimmed/fastp` 

```bash
fastp --thread 2 \
-i <value> \
-I <value> \
-o <value> \
-O <value> \
--cut_right \
--cut_window_size 4 \
--cut_mean_quality <value> \
--length_required <value> \
--html <value>

```


Add the working code to your `script.sh`.

<details>
<summary>Code</summary>

```bash
fastp --thread 2 \
-i 0_data/sampleA_R1.fq.gz \
-I 0_data/sampleA_R2.fq.gz \
-o 1_trimmed/sampleA_R1.fq.gz \
-O 1_trimmed/sampleA_R2.fq.gz \
--cut_right \
--cut_window_size 4 \
--cut_mean_quality 25 \
--length_required 90 \
--html 1_trimmed/fastp/sampleA.html
```

</details>



## Align reads to reference genome

To align reads to a reference genome, we first need to index the reference genome. 
Use the code below to do this: 

```bash
bwa index reference.fa
```

Now we can align reads. Use the code skeleton below for this command, replacing `<value>`s as appropriate.
Use the `samtools view --help` documentation to find out what type of file will be output so that your output file in the `2_aligned` directory has the correct extension. 


```bash
bwa mem -t 2 reference.fa <value> <value> | samtools view -bh - > <value>
```

Add the code you used to your `script.sh`.

<details>
<summary>Code</summary>

```bash
# index the reference genome first
bwa index reference.fa

# align reads to reference
bwa mem -t 2 reference.fa \
1_trimmed/sampleA_R1.fq.gz \
1_trimmed/sampleA_R2.fq.gz \
| samtools view -bh - > 2_aligned/sampleA.bam
```

</details>


## Summarise alignment statistics

Now we'll generate some alignment statistics. 
Replace `<value>` and modify the command below so that the output is sent to `3_stats/sampleA.txt`. 


```bash
samtools stats <value>
```

Check that this command worked with `less 3_stats/sampleA.txt`.
The file that displays should start with the line `# This file was produced by samtools stats (1.20+htslib-1.21) and can be plotted using plot-bamstats`. 
We'll take a closer look at these results later.

Add the code you used to your `script.sh` but don't include the `less` command.

<details>
<summary>Code</summary>

```bash
samtools stats 2_aligned/sampleA.bam > 3_stats/sampleA.txt
```

</details>


## Test your script

Because you have already run all of the code that is currently in your script, it will be hard to tell if the script is working properly if you simply run it again. 
Therefore, to test it more thoroughly, we'll delete the directories and files that it's supposed to generate before running it. 

```bash
# delete the reference genome and index files
rm reference.fa reference.fa.*

# delete directories
rm -r 0_data/ 1_trimmed/ 2_aligned/ 3_stats/ 
```

Make sure you don't delete the `script.sh` that you've been working on! 


Now, run your script with `bash script.sh`. 
It will probably take about a minute to complete and you should see some output to the terminal while its running. 


If your script works as expected, running `tree` should show something like below:

```txt
.
├── 0_data
│   ├── sampleA_R1.fq.gz -> /shared/data/bash_crash_prac/sampleA_R1.fq.gz
│   ├── sampleA_R2.fq.gz -> /shared/data/bash_crash_prac/sampleA_R2.fq.gz
│   ├── sampleB_R1.fq.gz -> /shared/data/bash_crash_prac/sampleB_R1.fq.gz
│   ├── sampleB_R2.fq.gz -> /shared/data/bash_crash_prac/sampleB_R2.fq.gz
│   ├── sampleC_R1.fq.gz -> /shared/data/bash_crash_prac/sampleC_R1.fq.gz
│   └── sampleC_R2.fq.gz -> /shared/data/bash_crash_prac/sampleC_R2.fq.gz
├── 1_trimmed
│   ├── fastp
│   │   └── sampleA.html
│   ├── sampleA_R1.fq.gz
│   └── sampleA_R2.fq.gz
├── 2_aligned
│   └── sampleA.bam
├── 3_stats
│   └── sampleA.txt
├── reference.fa
├── reference.fa.amb
├── reference.fa.ann
├── reference.fa.bwt
├── reference.fa.pac
├── reference.fa.sa
├── script.sh
```

If it doesn't or if your script stopped with an error message, try to work out where your script went wrong and fix it. 

## Generalise to multiple samples

Now that your script runs on a single sample, let's set it up to process multiple samples using a loop. 
We'll use a `for` loop like in the example below to do this. 


```bash
for SAMPLE in sampleA sampleB sampleC;
do

	# Place all code that must be run for each sample individual in here
	# Replace all instances of sampleA in this code with ${SAMPLE} <- See below for a shortcut on how to do this.
	# Then when the loop runs, it will run three times, once for each of the samples listed, replacing ${SAMPLE} inside of the loop with the sample name on each iteration
	
done
```

Questions to consider:
- Which parts of our workflow only need to be run once, regardless of how many samples we're processing? 
- Does it matter whether these steps are placed before or after the for loop?


# **Filtering, Parsing, and Wrangling text**

This section covers some useful commands for wrangling data in BASH and uses the `lactobacillus.gff` and `example.gff` files from the previous sections.  

Overview of useful commands and applications:
- `grep` - search a file for a word or pattern
- `cut`	 - remove or select sections from each line of a file
- `sort` - sort lines
- `uniq` - Used after sorting to retain only one of each unique item/line
- `awk`	 - Good for filtering based on criteria and processing text
- `sed`	 - Stream EDitor - filtering and transforming text



## Inspect files

`lactobacillus.gff` is a GFF file (General Feature Format) and GFF files store genome annotations in a tab-delimited plain-text format.
View the file using `less` as below:

```bash
less --chop-long-lines lactobacillus.gff
```

The file begins with a number of header lines which all begin with `#`. The header includes the version of the gff format, details of the genome assembly build and annotation source, and the species. 
After the header are the annotations themselves. Each line annotates a different feature and the tab-separated columns are listed below. 
The aim of this exercise is to practice your BASH skills so you can focus on the items/columns in bold for this exercise.

1. **Sequence ID**
2. Source
3. **Feature type**
4. **Start**
5. **Stop**
6. Score
7. Strand
8. Phase
9. **Attributes**


Press `q` to exit `less`. 

Now view `example.gff`.

```bash
less -S example.gff
```

The file `example.gff` is just the first 25 lines of the lactobacillus.gff file and we'll use it to test the commands below so that we can clearly see what is going on. 
The following is a set of questions about the contents of `example.gff` and some code that steps through the development of a command that answers that question. 
Read each question, view `example.gff` using less to work out what the question is asking, and then run each line of code individually so that you can see how the command develops.


**How many features are annotated (ie. how many non-header lines are there)?**

```bash
grep "^#" example.gff

grep -c "^#" example.gff 
```


**How many different sources of annotations are there?**

```bash
cut -s -f2 example.gff

cut -s -f2 example.gff | sort

cut -s -f2 example.gff | sort | uniq

cut -s -f2 example.gff | sort | uniq -c

cut -s -f2 example.gff | sort | uniq -c | wc -l
```

**What are the coordinates of the longest gene and how long is it?**

```bash
awk '$3=="gene"{print $0}' example.gff

awk '$3=="gene"{print $1, $4, $5}' example.gff

awk '$3=="gene"{print $5-$4+1, $1, $4, $5}' example.gff

awk '$3=="gene"{print $5-$4+1, $1, $4, $5}' example.gff | sort -nr 

awk '$3=="gene"{print $5-$4+1, $1, $4, $5}' example.gff | sort -nr | head -n1

``` 


Now, using what you learnt from the questions above, write commands to answer the questions below:


- How many different sequences are annotated? ie. How many unique values in Column 1.
- How many features are of the type "CDS". ie. How many times is CDS listed in Column 3.
- How many genes are annotated on each sequence/chromosome?
`awk '$3=="gene"{print $0}' lactobacillus.gff | cut -f1 | sort | uniq -c` 
- Which CDS region is the shortest?
- How many genes are annotated?

```bash
grep "\sgene\t" lactobacillus.gff
```






# **BASH scripts**

BASH scripts 

Write a BASH script to do the following:

- Create a symlink to the file 
- Run FastQC on the file 
- Run Fastp on the file
- Index the reference genome with `bwa mem`
- Align reads to the reference genome with `bwa mem`
- Summarise the alignments using `samtools stats`


```bash
#!/bin/bash

## Create necessary directories

## 
```


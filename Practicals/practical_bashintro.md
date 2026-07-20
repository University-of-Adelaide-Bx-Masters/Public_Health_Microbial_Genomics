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

# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

# **Commonly used commands**

In this section, we'll review a few simple commands/skills which we will then use to work out what a number of other commands are doing.

### Where am I?
**Check your command prompt or use `pwd` - Print Working Directory**

The terminal command prompt on your VM includes the path to your current location - your working directory.
In the example below, the user is in their home directory `/shared/a1761942`.

![Terminal Command Prompt](images/command_prompt_annotated.png)

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

### What else is here?
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

### Look at contents of a file
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

### How to get help 
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


### Investigating common commands

Now you know how to:
- see where you are by checking the command prompt
- list the contents of a directory with `ls`
- see the contents of a file with `less filename`
- open the manual for a command with `man commandname`

Run each line of code below one at a time and in between, use the commands and techniques we just covered to find out what each one does. 
Write down what you find as a helpful reference for yourself and do we can go through them together. 


```bash
cd ~ 

mkdir bash_crash_course

cd ~/bash_crash_course

touch notes.txt

echo "Lactobacillus amylovorus GRL 1112" > notes.txt

echo "Obtained with wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/182/855/GCF_000182855.2_ASM18285v1/GCF_000182855.2_ASM18285v1_genomic.gff.gz" >> notes.txt

wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/182/855/GCF_000182855.2_ASM18285v1/GCF_000182855.2_ASM18285v1_genomic.gff.gz

gunzip GCF_000182855.2_ASM18285v1_genomic.gff.gz

mv GCF_000182855.2_ASM18285v1_genomic.gff lactobacillus.gff

head lactobacillus.gff

tail lactobacillus.gff

cat notes.txt

cp /shared/data/intro_bash/words words.txt

head -n5 words.txt

tail -n5 words.txt

wc -l words.txt

rm words.txt

du -sh ~

```

At the end of this section, you should have 


# **Filtering, Parsing, and Wrangling text**

This section covers some useful commands for wrangling data in BASH. These include:
- `grep`
- `cut`
- `sort`
- `uniq`
- `awk`
- `sed` and regular expressions


### Download and inspect the file


### How many genes are annotated?

### How many CDS features are there?

```bash

```



grep, cut, sort, count etc. 



### The power of patterns 
- Regex (sed)


### sed and regular expressions


- grep and sed
- awk
- cut
- sort 


```bash

```

- head/tail,  grep, cut

###  


- Regular expressions with grep
- inspect fasta and gff and get info from them
- sed


# **BASH scripts**

- bash scriptsi
- A basic workflow that downloads a dataset and does something basic? run fastqc? get data from the file? 
- loops
- read qc, read alignment and variant calling



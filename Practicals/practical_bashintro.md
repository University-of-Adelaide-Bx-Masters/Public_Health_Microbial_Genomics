# (Re)introducing BASH - A Crash Course
{:.no_toc}
By Chelsea Matthews

* TOC
{:toc}

# **1. Introduction**

## 1.1 Practical Overview

This practical serves to re-familiarise students in their second semester of Bioinformatics with BASH. 
It assumes students have completed one semester of Bioinformatics and is designed to remind and reintroduce, and not to teach from scratch. 

It has three parts:
- Commonly used commands
- BASH scripts
- Filtering, parsing, and wrangling

While we some parts will use common Bioinformatics files and the BASH scripting section builds a very simple bioinformatics workflow, the emphasis is on BASH scripting and not on interpretation of results. 

## 1.2 Learning Outcomes

1. Re-familiarise yourself with navigating the terminal and using common BASH commands
2. Understand the purpose of BASH scripting and how to write and comment simple scripts
3. Practice data wrangling in BASH using common commands and regular expressions

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

<img src="images/command_prompt_annotated.png" alt="Terminal command prompt" width="600">


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
- An absolute path always points to the same location, regardless of your current directory, while a relative path specifies a location relative to your current working directory. Which of the paths above are absolute and which are relative? 

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

**In Class Exercise**

Run each line of code below one at a time and in between each line, use the commands and techniques we just covered (`man`, `ls`, `less`, and `pwd`) to find out what each one does. 
Write down your findings and make sure you know what a command does before moving on to the next one. 
These commands don't perform any analysis.
They are simply examples that demonstrate a range of very commonly used BASH commands. 
If they are run in the order presented, you will end up with an empty directory called `bash_crash_course` in which you will work for the remainder of the practical. 

Don't worry if you can't remember the exact syntax for these commands. 
It's expected that you will need to use the help documentation or consult the command manual because even experienced bioinformaticians still need to do this frequently. 


```bash
cd ~ 

mkdir bash_crash_course

cd ~/bash_crash_course

echo "This is some very boring text" > notes.txt

echo "And this is some more very boring text" >> notes.txt

wget http://ftp.ensembl.org/pub/release-107/fasta/caenorhabditis_elegans/dna/Caenorhabditis_elegans.WBcel235.dna.toplevel.fa.gz

gunzip Caenorhabditis_elegans.WBcel235.dna.toplevel.fa.gz

mv Caenorhabditis_elegans.WBcel235.dna.toplevel.fa c_elegans.fa

head -n5 c_elegans.fa

tail c_elegans.fa

head c_elegans.fa > notes.txt

wc -l notes.txt

cp notes.txt otherfile.txt 

rm c_elegans.fa

rm notes.txt otherfile.txt

ln -s /shared/data/intro_bash/words words.txt

rm words.txt

cd ..

du -sh ~
```

Go through your findings together as a class.

After running all of the commands above in order, there should be an empty directory `bash_crash_course` in your home directory and your working directory should be your home directory.


# **4. BASH scripts**

In this section we'll write a BASH script that processes 3 samples in a very simple bioinformatics workflow. This script will:
- Create necessary directories
- Obtain reference genome and Illumina reads
- Run quality control with `fastp`
- Align reads to a reference genome with `bwa`
- Summarise alignment statistics with `samtools`


We will work through the process of testing code and building the script step by step.
Code is provided for each of these steps but it is hidden in a dropdown box. 
Try to work out the code for yourself using the information and hints provided.


The data we'll be using is located in the `/shared/data/bash_crash_prac/` directory.
Use `ls` with whatever options are appropriate to answer the following questions about the contents of this directory:
- Is the `reference.fa` file compressed?
- How big is the `reference.fa` file?
- When was the 'reference.fa' file last modified?
- How many samples are there?
- Are the files containing sample reads compressed?


<details>
<summary>Code</summary>

```bash
<pre>$ ls -lh /shared/data/bash_crash_prac/</pre>
```

</details>


## 4.1 Create script skeleton

You can use the code below as a skeleton for your script. 
You can either create the script now using `nano` or you can use a text file (use notepad on Windows) and transfer the contents of the text file to a script once we're ready to start testing it. 
The script should be called `script.sh` and be located in your `~/bash_crash_course` directory.

To do this:
- Run `nano script.sh` 
- Paste the code skeleton below into the empty text file that appears
- Once you're happy with the file, press `Ctrl + x` to exit the editor
- Press `y` when asked `Save modified buffer?`
- Press `enter` when asked `File Name to Write: script.sh` to save the file


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

## 4.2 Create directories

Many tools will fail if the specified output directory doesn't exist before the tool is run. 
Therefore, it's good practice to create necessary directories up front.

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

Add the code you used to create your directories to `script.sh` in the appropriate location. 

<details>
<summary>Code</summary>

<pre>mkdir 0_data<br>mkdir -p 1_trimmed/fastp<br>mkdir 2_aligned<br>mkdir 3_stats</pre>

</details>

## 4.3 Get data

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


## 4.4 Run quality control

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



## 4.5 Align reads to reference genome

To align reads to a reference genome, we first need to index the reference genome. 
Use the code below to do this: 

```bash
bwa index reference.fa
```

Now we can align reads. Use the code skeleton below for this command, replacing `<value>`s as appropriate.
Use the `samtools view --help` documentation to find out what type of file will be output and replace `<EXT>` with the correct extension. 

```bash
bwa mem -t 2 reference.fa <value> <value> | samtools view -bh - > 2_aligned/sampleA.<EXT>
```

Add both the indexing and alignment code to your `script.sh`.

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


## 4.6 Summarise alignment statistics

Now we'll generate some alignment statistics. 
Replace `<value>` and modify the command below so that the output is sent to `3_stats/sampleA.txt`. 

You can check if this command worked using `less 3_stats/sampleA.txt`.

The file that displays should start with the line `# This file was produced by samtools stats (1.20+htslib-1.21) and can be plotted using plot-bamstats`.
Because the focus of this practical is on BASH, we won't worry about inspecting these results closely today. 

```bash
samtools stats <value>
```

Add the code to your `script.sh` but don't include the `less` command.

<details>
<summary>Code</summary>

```bash
samtools stats 2_aligned/sampleA.bam > 3_stats/sampleA.txt
```

</details>


## 4.7 Test your script


Because you have already run all of the code that is currently in your script, it will be hard to tell if the script is working properly if you simply run it again. 
To test it more thoroughly, we'll delete the directories and files that the script is supposed to generate before running it. 


```bash
# delete the reference genome and index files
rm reference.fa reference.fa.*

# delete directories
rm -r 0_data/ 1_trimmed/ 2_aligned/ 3_stats/ 

# check the contents of directory
ls 
```

Make sure you don't delete the `script.sh` that you've been working on!
If you have been working in a text file, you'll need to create your `script.sh` using `nano`. 


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

## 4.8 Generalise to multiple samples

Now that your script runs on a single sample, let's set it up to process multiple samples using a loop. 

So that you don't accidentally break your script, make a copy of it called `script_loop.sh` and make your changes there. 

```bash
cp script.sh script_loop.sh
```

We'll use a `for` loop like in the example below.
The loop iterates over the provided sample names and will run whatever code is inside of the loop for each sample, replaing `${SAMPLE}` with the sample name each time.  

```bash
for SAMPLE in sampleA sampleB sampleC;
do
	echo ${SAMPLE}
	
	# Place all code that runs once for each sample inside the loop
	
done
```

Add the loop to your script and rearrange your code so that the parts of the workflow that only need to be run once, regardless of how many samples are being processed, are outside of the loop. 
Code that runs for each sample should be inside of the loop. 

Now replace all of the instances of `sampleA` inside of the loop with `${SAMPLE}` so that the loop can sub in each sample name. 


## 4.9 Test again!

Now test your script again! It should process all 3 samples, one after the other. 

```bash
bash script_loop.sh
```


<details>
<summary>Code</summary>

```bash
#!/bin/bash

# Load software
source activate bioinf

# Create directories
mkdir 0_data
mkdir -p 1_trimmed/fastp
mkdir 2_aligned
mkdir 3_stats

# Get data
# symlink to illumina reads
ln -s /shared/data/bash_crash_prac/*.fq.gz 0_data/

# copy reference to current directory
cp /shared/data/bash_crash_prac/reference.fa .

# index reference genome
bwa index referencefa

for SAMPLE in sampleA sampleB sampleC;
do

	# Run Quality Control
	fastp --thread 2 \
	-i 0_data/${SAMPLE}_R1.fq.gz \
	-I 0_data/${SAMPLE}_R2.fq.gz \
	-o 1_trimmed/${SAMPLE}_R1.fq.gz \
	-O 1_trimmed/${SAMPLE}_R2.fq.gz \
	--cut_right \
	--cut_window_size 4 \
	--cut_mean_quality 25 \
	--length_required 90 \
	--html 1_trimmed/fastp/${SAMPLE}.html


	# Align reads to reference genome
	bwa mem -t 2 reference.fa \
	1_trimmed/${SAMPLE}_R1.fq.gz \
	1_trimmed/${SAMPLE}_R2.fq.gz \
	| samtools view -bh - > 2_aligned/${SAMPLE}.bam

	# Summarise alignment statistics
	samtools stats 2_aligned/${SAMPLE}.bam > 3_stats/${SAMPLE}.txt

done

```

</details>


# **5. Filtering, Parsing, and Wrangling text**

In this final section we'll use some more BASH commands to extract information from some of the files we've used or produced with our workflow.
These commands, or variations of them, may be very useful in the rest of the course. 

Overview of useful commands and applications:
- `grep` - search a file for a word or pattern
- `cut`  - remove or select sections from each line of a file
- `sort` - sort lines
- `uniq` - Used after sorting to retain only one of each unique item/line
- `awk`  - Good for filtering based on criteria and processing text
- `sed`  - Stream EDitor - filtering and transforming text

For each of the following sections, try to work out which parts of the command are doing what. 


## 5.1 Get all sequence identifiers in a FASTA file

  
```bash
grep "^>" reference.fa
```

Modify the above command to count the number of sequence identifiers in a FASTA file.

## 5.2 Print just the file header (lines beginning with #)

```bash
grep "^#" 3_stats/sampleA.txt
```

Modify the command to print all of the lines that **don't** begin with a `#`.


## 5.3 Count the number of bp in a FASTA file

```bash
grep -v "^>" reference.fa | wc | awk '{print $3-$1}'

```

## 5.4 Extract a single column of data and count of each value

```bash
cut -s -f2 example.gff | sort | uniq -c
```

Modify the command to count the number of unique values, not how many there are of each value.

## 5.5 Find and replace `sampleA` with `${SAMPLE}`

```bash
sed 's/sampleA/${SAMPLE}/g' script.sh
```

- When might this command have been useful?
- Work out how to save the output of this command to a new file




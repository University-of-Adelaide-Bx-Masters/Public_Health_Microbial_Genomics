# Bacterial genotyping
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

Firstly, congratulations, in the previous practical (species classification) you idenitifed that the 9 isolates belong to Salmonella enterica. Before we move on, I wanted to say that in Australia Salmonella makes up a large portion of disease cases that are notified to the Australian government. We get ~20,000 cases of Salmonella in Australia each year. That is alot of cases and you can imagine how busy the Public health labs are with sequencing and analysing all of the Salmonella cases. 

Now that we know that we are working with Salmonella enterica, we can now move on to the next step of the genomics workflow, which is to perform bacterial genotyping. 


<img width="808" height="588" alt="image" src="https://github.com/user-attachments/assets/a0719c5b-af60-4605-ad0c-a00da9f892e2" />


## 1.1 Practical Overview


In practice, MLST and cgMLST are often used together:
   -  MLST first: Quick screening to identify strain types and potential outbreaks
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
# create symlink for salmonella cgmlst schema 
ln -s ~/data/public_health_genomics/microbial_genomics/salmonella_schema db/
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


The command we ran above generally autodetects an appropriate bacterial scheme to use. However their may be times where an incorrect scheme is selected by the `mlst` tool. This can happen for example between closeley related bacterial species (e.g. shigella and e.coli), where the house keeping genes are genetically similar. To overcome this, you can force `mlst` to use a specific scheme by adding the option `--schema` followed by the name of the scheme - in our case we are working with salmonella samples so we will use the `salmonella` scheme: 

```bash
mlst --scheme salmonella --quiet assembly/ERR10479021.fasta
```
Looking at the results you can see that we get the same ST, genes and allele IDs when we forced the scheme campared to when we didnt force the scheme - lucky! 

However, because we used the `--quiet` setting we did not get all of the information printed on the terminal - if we run without `--quiet` 

```bash
mlst --scheme salmonella assembly/ERR10479021.fasta
```
You can actually see that the results are different to the results from when we didnt specify a scheme. 

**Question:** 
- can you see which result is different? hint its not to do with the ST or allele IDs. 

Now lets look at if you were to specificy the wrong scheme for the organism in your sample. The command will run but you will not get any results. For example lets run the Mycobacterium tuberculosis scheme over a salmonella sample:

```bash
mlst --scheme mycobacteria_2 --quiet assembly/ERR10479021.fasta
```

On the terminal you can see that a `-` is present for the ST and allele IDs. This is because the genes used for the `mlst` scheme that we specified have not been detected in out salmonella sample. What this shows you is that it is so important to first determine what species your samples belongs to as this helps with ensuring thet you are running the correct downstream analysis - and as we have just seen this is important to get a sequence type. 

### 3.2 Run `mlst` over all samples

Now you have genotyped one sample! well done - you can see how simple `mlst` is to run for genotyping samples. We can now go ahead and genotype the remaining salmonella samples. All of our salmonella assemblies begin with the letter "E". So the easiest way to run `mlst` over all our samples is to use a wild card to call all the .fasta samples begining with E and run the `mlst` tool on each of the assembly files. 

You can do this using the following command, at the same time we will direct the results to an output file named (`salmonella_mlstresults.tsv`):

```bash
mlst --scheme salmonella --quiet assembly/E*.fasta > mlst/salmonella_mlst_results.tsv
```

**Questions:** 
Remember that if multiple samples belong to the same ST they likely represent an outbreak. View the output results(`salmonella_mlst_results.tsv`) on the terminal and answer the following questions: 
- How many STs are present in the salmonella samples?
- Do you observe any outbreaks?
- How many outbreaks could be present?
  

# **4. Perform Core-genome MLST using `chewBBACA`**
We have used `mlst` to assign a sequence type to our salmonella samples, which is an important tool for the initual quick screaning of samples to identify potential bacterial outbreaks. The next step would be to then undertake core genome MLST (cgMLST) analysis on the samples. cgMLST is based on thousands of core genes (genes present in >95% of strains) and thus compared to mlst it provides much higher genetic resolution needed for confirming outbreaks. For this part of the practical we will undertake cgMLST on the salmonella samples. 

`chewBBACA` is a commonly used tool for undertaking cgMLST on bacterial genomes, particulalry in public health settings for outbreak investigations. `chewBBACA` uses a BLAST score ratio (BCR)- based allele calling alogarithm to identify allels across genome assemblies, producing allelic profiles that can be compared between bacterial samples. Generally, samples with fewer allele differences are considered to be more closely related. 

### 4.1 Salmonella enterica core genome MLST schema

cgMLST schemes are specific to each bacteria, and for Salmonella enterica a cgMLST schema has already been developed and contains 3,002 genes (you can see that this is quiet alot more genes compared to the seven genes included in the mlst) 

Before `chewbbaca` can be run on the samples the salmonella enterica cgMLST schema (file containing all schema genes in FASTA format) needs to be downloaded from ridom seqsphere and adapted so that it is in the correct format for input into `chewbacca`. Lucky for you - I have already done this step.   

YOU DO NOT NEED TO RUN THIS COMMAND I HAVE ALREADY DONE THIS FOR YOU - THIS IS TO SHOW YOU HOW THE COMMAND LOOKS FOR CONVERTING cgMLST SCHEMA FOR INPUT INTO `chewbacca`. 

```bash
chewBBACA.py PrepExternalSchema -g /shared/data/public_health_genomics/microbial_genomics/salmonella_cgMLST/ -o salmonella_schema
```

### 4.2 Perform allele calling on the Salmonella enterica genome assemblies

To run `chewBACCA` `AlleleCall` you need:
- Genome assemblies (FASTA) as input files
- A cgMLST schema (includes gene loci and alleles)

Now the fun part - lets go ahead and run `chewBACCA` to determine the allelic profiles of the Salmonella enterica genomes: 

This will take ~15 minutes to run - you will see stuff happening in the terminal and it will stop once finihsed. 

```bash
chewBBACA.py AlleleCall -i assembly/ -g db/salmonella_schema -o cgmlst/allele_calling_results 
```

The main output file is the `results_alleles.tsv`, which is a tab delimited file with:
- Rows, genome assemblies
- Columns, schema loci
- Values, allele identifiers or classification code

### 4.3 schema evaluator 

build an interactive report for schema evaluation: 

```bash
chewBBACA.py SchemaEvaluator -g db/salmonella_schema -o cgmlst/schema_evaluator
```
This takes ~10 minutes to complete 

Main output is the `schema_report.html` file 

### 4.4 Allele call evaluator

build an interactive report for allele calling schema evaluation 

```bash
chewBBACA.py AlleleCallEvaluator -i cgmlst/allele_calling_results/results_20260713T071622/ -g db/salmonella_schema -o cgmlst/AlleleCallEvaluator
```

This takes ~12 minutes to complete 

This produces the following outputs:


### 4.5 Determine the set of loci that make up the core genome 

Not all loci are present in all genomes.

To focus on the core genome (loci present in ≥95% or 100% of isolates) we will run `chewBBACA` as here:

```bash
# Extract core genome loci
chewBBACA.py ExtractCgMLST -i cgmlst/allele_calling_results/results_20260713T071622/results_alleles.tsv -o cgmlst/cgmlst_matrix 
                            #--t 0.95 \
                           #(OPTIONAL) --r allele_calling_results/RepeatedLoci.txt
```

Parameters explained:
-i: Input allele calling results file
-o: Output directory
--t: Threshold (0.95 = loci present in ≥95% of genomes)
--r: File with repeated loci to exclude (optional)

The main output of `chewBBACA.py` `ExtractCgMLST` is a cgMLST schema: a list of loci selected as the core genome MLST targets based on their presence across the input genome set (in our case the salmonella samples).

Outputs:
- cgMLST.tsv: Allele profiles for core genome loci only
- cgMLSTschema.txt: List of core genome loci
-  mdata_stats.tsv: Statistics on missing data per genome

# **5. Calculate distance matrix from cgMLST allele call tables of `ChewBBACA`**

Calculate pairwise allelic differences

Run `cgmlst-dists` to calculate distance matrix from cgMLST allele call table  that was generated by `chewbBACA` (step 4.2)

``` bash
cgmlst-dists cgmlst/allele_calling_results/results_20260713T071622/results_alleles.tsv > cgmlst/distance.tab
```
This command calculates the number of allele differences between every pair of isolates and writes the results to `distance.tab` 

look at the output 

```bash
cat cgmlst/distance.tab
```

```bash
cgmlst-dists    ERR10479021     ERR10479025     ERR10479028     ERR10479029     ERR10479032     ERR10479034     ERR10479035     ERR10479037 ERR10479039
ERR10479021     0       77      77      79      57      0       4       2       3
ERR10479025     77      0       17      19      77      77      78      77      78
ERR10479028     77      17      0       4       72      77      76      75      76
ERR10479029     79      19      4       0       76      79      78      77      76
ERR10479032     57      77      72      76      0       57      57      57      58
ERR10479034     0       77      77      79      57      0       4       2       3
ERR10479035     4       78      76      78      57      4       0       2       3
ERR10479037     2       77      75      77      57      2       2       0       1
ERR10479039     3       78      76      76      58      3       3       1       0
```

Here you can see (for example):
- 0 = identical allelic profiles
- 4 = four allele differences
- 77 = twenty-five allele differences
  and so on 

The fewer allele differences between two isolates, the more closely related they are.

# **6. Cluster isolates using various (5, 10, 20) allelic differences thresholds**

single linkage clustering 

``` bash
to do custom script 
```

# **7. Visual comparison of MLST and core-genome MLST results**

- combine results into a table - based on the two methods do the isolates belong to the same genotype/group? or has cgMLST split the samples into more groups? 

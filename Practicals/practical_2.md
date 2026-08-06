# Bacterial genotyping
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

Firstly, congratulations, in the previous practical (species classification) you identified that the 9 isolates belong to Salmonella enterica. Before we move on, I wanted to say that in Australia Salmonella makes up a large proportion of infectious disease cases that are notified to the Australian government. We get ~20,000 cases of Salmonella in Australia each year. That is alot of cases and you can imagine how busy the public health laboratories are with sequencing and analysing samples from Salmonella cases. 

Now that we know that we are working with Salmonella enterica, we can now move on to the next step of the genomics workflow (indicated by a yellow start in the workflow figure below), which is to genotype the 9 Salmonella enterica sequences. 

<img width="808" height="588" alt="image" src="https://github.com/user-attachments/assets/a0719c5b-af60-4605-ad0c-a00da9f892e2" />

## 1.1 Practical Overview
For this practical you will be working with the same 9 Salmonella enterica sequences as you did in the species classification practical, for this practical we will be working with their genome assemblies (FASTA). Remember that in the species classification practical you identified the 9 isolates as salmonella enterica and the next step in the microbial genomics workflow is to genotype the 9 samples. You will be testing different genotyping approaches that are commonly used for bacterial genotyping, they include MLST (genotyping of bacteria based on seven house keeping genes) and cgMLST (genotyping of bacteria based on thousands of core genome genes). Remember that bacterial samples belonging to the same MLST sequence type are likely to be associated with the same outbreak.

In practice, MLST and cgMLST are often used together:
   -  MLST is used first to quickly screen bacterial isolates to identify any potential outbreaks
   - cgMLST is then used as a higher genetic resolution approach to confirm outbreaks 
 
## 1.2 Learning Outcomes
1. Gain practice in performing bacterial genotyping 
2. Learn how to interoperate results and relevance to public health surveillance of bacteria 

# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create sub directories that reflect the main steps in our analysis. This will help us stay organised.

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
├── cgmlst
├── db
│   └── salmonella_schema -> /shared//a1237649/data/public_health_genomics/microbial_genomics/salmonella_schema
└── mlst
```

# **3. Assigning strains an MLST using the `mlst` tool**

The `mlst` tool written by Torsten Seemann scans bacterial genome assemblies (in FASTA format) against an online database (known as PubMLST, we won't be looking at this database in detail today) that houses the MLST typing schemes and reports the sequence type.

Lets get started, 

First run this to get the help information for `mlst`

```bash
mlst -h
```

You can see a whole bunch of stuff on the terminal - feel free to read over this if you like. 

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

On the terminal you should see the `mlst` command returned a tab-separated line (this is the line that we are most interested in) containing:
- The filename
- The macthing PubMLST schema name
- The Sequence Type (ST)
- Genes and their allele IDs

<img width="1265" height="342" alt="image" src="https://github.com/user-attachments/assets/663749fc-2dad-4df5-80a6-0cfb8ecc15a0" />


The command we ran above generally auto detects an appropriate bacterial scheme to use. However their may be times where an incorrect scheme is selected by the `mlst` tool. This can happen for example between closely related bacterial species (e.g. shigella and e.coli), where the house keeping genes are genetically similar. To overcome this, you can force `mlst` to use a specific scheme by adding the option `--schema` followed by the name of the scheme - in our case we are working with salmonella samples so we will use the `salmonella` scheme: 

```bash
mlst --scheme salmonella --quiet assembly/ERR10479021.fasta
```
Looking at the results you can see that we get the same ST, genes and allele IDs when we forced the scheme compared to when we didn't force the scheme - lucky! 

However, because we used the `--quiet` setting we did not get all of the information printed on the terminal - if we run without `--quiet` 

```bash
mlst --scheme salmonella assembly/ERR10479021.fasta
```
You can see that the results are slightly different to the results from when we didn't specify a scheme. 

**Question:** 
- can you see which result is different? hint its not to do with the ST or allele IDs. 

Now lets look at if you were to specify the wrong scheme for the organism in your sample. The command will run but you will not get any results. For example lets run the Mycobacterium tuberculosis scheme over a salmonella sample:

```bash
mlst --scheme mycobacteria_2 --quiet assembly/ERR10479021.fasta
```

On the terminal you can see that a `-` is present for the ST and allele IDs. This is because the genes used for the `mlst` scheme that we specified have not been detected in the Salmonella samples. What this shows is that it is important to first determine what species your samples belong to, as this helps with ensuring that you are running the correct downstream analysis, and as we have just seen this is important to get a sequence type. 

### 3.2 Run `mlst` over all samples

Now you have genotyped one sample! well done - you can see how simple `mlst` is to run for genotyping bacterial samples. You can now go ahead and genotype the remaining salmonella samples. Create a file called `run_mlst.sh` by typing `nano run_snippy.sh` and pasting in the template below. Then fill in the missing gaps, save the file and close nano. Then run the script. 

```bash
#!/bin/bash

# Load software

# List of samples with assemblies 

# Loop over each sample

        # Run mlst
```

**Questions:** 
Remember that if multiple samples belong to the same ST they could represent an outbreak. View the output results for each sample on the terminal and answer the following questions: 
- How many STs are present?
- What are the STs present? 
- How many samples belong to each ST?
- Do you think any outbreaks are present? 
- Why are housekeeping genes suitable for MLST?

# **4. Perform Core-genome MLST using `chewBBACA`**
We have used `mlst` to assign a sequence type to our Salmonella enterica samples, which is an important tool for the initial quick screening of samples to identify potential bacterial outbreaks. The next step would be to then undertake core genome MLST (cgMLST) analysis on the samples. cgMLST is based on thousands of core genes (genes present in >95% of strains) and thus compared to mlst it provides much higher genetic resolution needed for confirming outbreaks. For this part of the practical we will undertake cgMLST on the salmonella samples. 

`chewBBACA` is a commonly used tool for undertaking cgMLST on bacterial genomes, particularly in public health settings for outbreak investigations. `chewBBACA` uses a BLAST score ratio (BCR) based allele calling algorithm to identify alleles across genome assemblies, producing allelic profiles that can be compared between bacterial samples. Generally, samples with fewer allele differences are more closely related. 

### 4.1 Salmonella enterica core genome MLST schema

cgMLST schemes are specific to each bacteria, and for Salmonella enterica a cgMLST schema has already been developed and contains 3,002 genes (you can see that this is many more genes compared to the seven genes included in mlst) 

Before `chewbbaca` can be run on the samples the Salmonella enterica cgMLST schema (file containing all schema genes in FASTA format) needs to be downloaded from ridom seqsphere and adapted so that it is in the correct format for input into `chewbacca`. I have already done this step for you, the Salmonella enterica schema is large so we don't want everyone downloading it.  


### 4.2 Perform allele calling on the Salmonella enterica genome assemblies

Now that the Salmonella cgMLST schema has been adapted we can perform allele calling on the 9 Salmonella enterica samples (we will use the genome assemblies as input, FASTA). You will do this using the `AlleleCall` module in `chewBACCA`. The AlleleCall module analyses each genome assembly, identifies loci from the Salmonella cgMLST schema, and assigns an allele identifier for each locus. Novel alleles identified in the analysed genomes are added to the schema. 

To run `chewBACCA` `AlleleCall` you need:
- Genome assemblies (FASTA) as input files
- A cgMLST schema (includes gene loci and alleles)

Now the fun part - lets go ahead and run `chewBACCA` to determine the allelic profiles of the Salmonella enterica genomes: 

This will take ~5 minutes to run - you will see stuff happening in the terminal and it will stop once finished. 

```bash
chewBBACA.py AlleleCall -i assembly/ -g db/salmonella_schema -o cgmlst/allele_calling_results 
```

The main output is `results_alleles.tsv`, which is a tab delimited file with:
- Rows: genome assemblies
- Columns:  schema loci
- Values: allele identifiers or classification code


### 4.5 Determine the set of loci that make up the core genome in our Salmonella enterica dataset

In this section, you will use the `results_alleles.tsv` file generated in Step 4.4 as input to the `ExtractCgMLST` module in chewBBACA. The `ExtractCgMLST` module identifies the set of core genome loci, those present in all or most of the analysed genomes (in this case, the 9 Salmonella genomes). By default, ExtractCgMLST generates core genome schemes using locus presence thresholds of 95%, 99%, and 100% (for example present in 95% of samples), these thresholds are standard for defining the core genome. The resulting core genome locus list can then be used to perform allele calling at the cgMLST level, providing higher-resolution typing for strain discrimination. 

To determine the cgMLST loci from the allele calling results, run the following command:


```bash
# Determine the core genome loci
chewBBACA.py ExtractCgMLST -i cgmlst/allele_calling_results/results_alleles.tsv -o cgmlst/cgmlst_matrix 
```

Parameters explained:
- `- i`: Input allele calling results file
- `- o`: Output directory

 The `ExtractCgMLST` module creates a file with the list of core loci and the cgMLST allelic profiles for each threshold (95%, 99% and 100%)

We will use the cgMLST determined at the 95% threshold for further analyses. The 95% allows a balance between including a sufficient number of loci for high-resolution typing and accounting for potential missing data due to sequencing or assembly issues. The file `cgMLSTschema95.txt` contains a list of the core loci identified at the 95% threshold. In step 4.6 you will pass this file to the --gl parameter of the AlleleCall module to perform allele calling at the core genome MLST level.


### 4.6 AlleleCall module to perform allele calling at the cgMLST level

In step 4.2 we ran AlleleCall using the complete Salmonella schema to identify **all** loci in our 9 Salmonella isolates. After defining the core genome above, a second AlleleCall is performed using only the **core genome loci**, producing allele profiles that are directly specific to our analysis. 

Run `Allelecall` as follows: 

```bash
chewBBACA.py AlleleCall -i assembly/ -g  db/salmonella_schema --gl cgmlst/cgmlst_matrix/cgMLSTschema95.txt -o cgmlst/allele_calling_results_95_cgMLST
```

The structure of the command is similar to the one used to perform allele calling in step 4.2, with the addition of the --gl parameter to specify the list of core loci `cgmlstschema95.txt`. The output folder will also have the same structure, but the files include results at the core genome MLST level. The file that we care most about is the `results_alleles.tsv` which can be found here cgmlst/allele_calling_results_95_cgMLST. 

### 5. View the chewBBACA output

The `chewBBACA` `AlleleCall` (the one you ran in step 4.6) output file `results_alleles.tsv` can be input in to the online tool known as PHYLOViZ (https://online2.phyloviz.net), whereby PHYLOViZ generates a Minimum Spanning Tree from the allelic profiles. 

The tree shows genetic relatedness based on allele differences, remember the fewer allele differences between samples the more closely related they are. 

To upload to PHYLOViZ follow this: 
- Download `results_alleles.tsv` file from the VM
- Open a web browser and go to the PHYLOViZ website (https://online2.phyloviz.net)
  Sscroll down to Test PHYLOViZ online and click login-free
- Then click profile data and then changes analysis method to pairwise comparison
- Click browse and select the `results_alleles.tsv`
- Give the dataset a name (Salmonella) and a description (outbreak)
- Click Launch Tree 
- Then click graphic properties and click nodes and select node labels and click yes 
- Then click Links and select add link labels and click yes

You should see something like this:

<img width="1474" height="802" alt="image" src="https://github.com/user-attachments/assets/721b210f-71fc-48e1-87e8-4daf13bee4ab" />

How to interpret this image:
- Each node represents a Salmonella enterica sample 
- Each edge (line) connects isolates that are genetically most similar
- Clusters of nodes represent closely related isolates that may belong to the same outbreak or lineage
- The number of allele differences between connected isolates is displayed on the branches

It is important to remember that a Minimum Spanning Tree generated in PHYLOViZ is not a true phylogenetic tree. It does not infer a common ancestor or evolutionary direction. It is a network that connects isolates based on allele differences, making it particularly useful for outbreak investigations and comparing MLST to cgMLST. 

In PHYLOViZ, you can explore how changing the allelic difference threshold affects the clustering of isolates: 
- Open the Tree Modifiers tab in PHYLOViZ
- Adjust the Tree cut-off value, which represents the maximum number of allelic differences allowed for isolates to remain connected
- In outbreak investigations, an allelic difference threshold of 10 is commonly used to define isolates as belonging to the same outbreak cluster
- Set the tree cut-off to 10 and observe how the relationships between the isolates change

You should see a result similar to the figure below:

<img width="1072" height="820" alt="image" src="https://github.com/user-attachments/assets/e27a9a4b-5f23-438d-8dcf-42c2857929b3" />

**Questions:**
- At a threshold of 10 allelic differences, how many distinct clusters are present?
- Which isolates belong to each cluster?

**cgMLST reflection question:**

Suppose two isolates differ by: 
- 12 cgMLST alleles
- 150 cgMLST alleles
- 0 cgMLST alleles
- 3 cgMLST alleles


Rank these pairs from most likely to least likely to belong to the same outbreak.

 
# **5. Compare the MLST and cgMLST results** 

Complete the below table:

| **Sample**            | **MLST ST**  | **cgMLST cluster** | **Interpretation** |
|:----------            |:------------ |:------------------ |:-------------------|
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |
| ERR                   |              |                    |                    |

**Answer the following questions:**
- Based on the MLST and cgMLST results how many outbreaks are present?
- Which samples belong to an outbreak?
- Which isolates would have been incorrectly grouped together if only MLST had been used?
- Why was cgMLST able to break down the samples into further groups?
- Why is MLST still used despite cgMLST providing higher genetic resolution?
- Does sharing a MLST ST alone provide sufficient evidence that isolates belong to the same outbreak? Why or why not?
- Imagine these nine isolates were received by a public health laboratory investigating a suspected outbreak. Based on your analyses, Which isolates would you prioritise for epidemiological follow-up?








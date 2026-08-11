# Variant calling for phylogenomic trees & surveillance
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

**This practical will be run over two practical sessions - please take your time to understand each section**

In the previous practical on bacterial genotyping, you used MLST to determine that the Salmonella enterica isolates belonged to two different sequence types (STs). You then used cgMLST to examine the isolates at higher genetic resolution and identified a cluster of five closely related isolates that were associated with the outbreak. The remaining isolates were sufficiently genetically distinct to indicate that they were not part of the outbreak.

So, if cgMLST has already identified the outbreak-associated isolates, **why do we need to perform Single Nucleotide Polymorphism (SNP) phylogenomics?**

In this practical, you will use core-genome SNP phylogenomics to examine the genetic relationships between the isolates in greater detail. Unlike cgMLST, which describes isolates according to differences in the alleles assigned to a defined set of core genes, SNP analysis compares the actual nucleotide differences across the core genome. This allows the genetic distance between isolates to be examined at the individual nucleotide level and represented in a phylogenomic tree.

**SNP analysis therefore provides a complementary approach to cgMLST rather than simply repeating the same analysis**. In this practical, the purpose is not to re-establish which isolates belong to the outbreak - the cgMLST analysis has already provided this evidence— but to investigate the genetic relationships within the outbreak cluster and to visualise how the outbreak-associated isolates relate to the other isolates.

Another important advantage of SNP-based analysis is that the results are based on **individual nucleotide changes rather than a scheme-specific allele nomenclature.** cgMLST relies on defined gene sets, allele databases and nomenclature schemes. Different cgMLST schemes or databases can therefore produce different allele assignments, which can make results more difficult to compare between laboratories if different schemes are used. SNP differences, by contrast, are based on the underlying genome sequence and can provide a more directly comparable measure of genetic difference when laboratories use the same reference genome and SNP-calling criteria.

This makes SNP phylogenomics particularly useful for high-resolution outbreak investigation and inter-laboratory comparison, where it may be important to determine if isolates differ by only a small number of nucleotide changes and to place those isolates within a phylogenomic context.

**The key distinction is therefore:**
- **MLST** provides broad classification into sequence types
- **cgMLST** provides higher-resolution clustering using allele differences and can be used to identify potential outbreak clusters
- **Core-genome SNP phylogenomics** provides nucleotide-level resolution and shows the evolutionary relationships between isolates, allowing the genetic structure of an outbreak cluster to be examined in greater detail

This practical covers the steps highlighted with a star in the workflow figure below.

<img width="806" height="591" alt="image" src="https://github.com/user-attachments/assets/8ab7ea23-a1a3-416e-9a79-88757197144c" />


## 1.1 Practical Overview
For this practical you will be working with the same 9 Salmonella enterica sequences as you did in the previous practicals. You will be undertaking sequence read mapping and variant calling for constructing a phylogenomic tree and then visualise the tree alongside epidemiological data in microreact. This will provide you with an understanding of how to apply phylogenomics to public health settings. 

## 1.2 Learning Outcomes
1.	Gain more in-depth knowledge and further practice on identifying variants 
2.	Learn how a variant calling tool works 
3.	Learn how to build a phylogenomic trees from variants 
4.	Learn how to visualise and interpret a phylogenenomic trees
5.	Learn how to overlay metadata onto a phylogenomic tree and application to pathogen surveillance

# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create subdirectories that reflect the main steps in our analysis. This will help us stay organised.

```bash
mkdir --parents ~/Practical_variants_trees/{assembly,metadata,reads,snippy,tree}
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
# navigate to working directory
cd ~/Practical_variants_trees
# create symlinks for all fastq files
ln -s ~/data/public_health_genomics/microbial_genomics/*.fastq.gz reads/
# create symlink for reference genome
ln -s ~/data/public_health_genomics/microbial_genomics/GCA_000009505.1_ASM950v1_genomic.fasta assembly/
# create symlink for metadata
ln -s ~/data/public_health_genomics/microbial_genomics/metadata.csv metadata/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
.
├── assembly
│   └── GCA_000009505.1_ASM950v1_genomic.fasta -> /shared//a1237649/data/public_health_genomics/microbial_genomics/GCA_000009505.1_ASM950v1_genomic.fasta
├── metadata
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
├── snippy
└── tree
```

# **3. Mapping sequencing reads to a reference genome using snippy**

A very talented bioinformatician who is a friend of mine (Torsten Seemann) wrote `snippy` (https://github.com/tseemann/snippy). `snippy` is a widely used pipeline that maps reads to a reference genome, to identify variants and produces a range of outputs. `snippy` uses BWA-MEM, SAMtools and FreeBayes, which we will learn more about in this practical. If you have done one of our bioinformatics courses you may find `snippy` familiar, however as a bioinformatician `snippy` will likely become one of the tools that you will most frequently use and so in this practical we will be looking at snippy in more detail. 

## 3.1 Run Snippy for variant calling 

`snippy` requires: 

- A reference genome (FASTA or GENBANK format)
- Single- or paired-end sequencing reads from one or more isolates
- An output folder to put results in

We can explore the snippy help page to understand the available options:

```bash
snippy --help
```

The key parameters of Snippy are:

Parameter	Description
- outdir:	Directory where Snippy will write its output files
- R1:	Path to the forward (R1) FASTQ file
- R2: Path to the reverse (R2) FASTQ file
- ref:	Path to the reference genome (FASTA format)
- cpus:	Number of CPU threads to use (default: 8)
- ram:	Maximum RAM in GB (default: 8)
- minqual:	Minimum variant quality score (default: 100)
- minfra: Minimum allele frequency to call a variant (default: 0.9)
- prefix: Prefix for output files (default: snps)

Now the fun part, run snippy for each sample. Example command for one sample:

```bash
snippy --outdir snippy/ERR10479021 --ref assembly/GCA_000009505.1_ASM950v1_genomic.fasta --R1 reads/ERR10479021_1.fastq.gz --R2 reads/ERR10479021_2.fastq.gz 
```
Now we wait for snippy to finish this should take ~2 minutes for one sample 

## 3.2 Examine `snippy` logs 

To better understand how `snippy` processes sequencing reads and identifies genetic variants, we will inspect the `snps.log` output file.

The log records the commands and outputs generated during the analysis. This allows you to trace the different stages of the `snippy` workflow, including read alignment, alignment processing, variant calling, variant filtering and consensus sequence generation.

Log files are also useful when troubleshooting. If a tool fails during an analysis, the log file will often contain information that can help you identify what went wrong. For this reason, it is good practice to know where to find and how to read log files.

We will use `grep` to search the `snippy` log and identify the main tools used during the analysis. **You do not need to understand every command or parameter shown in the log**. The aim of this section is to understand the overall workflow and the role of each major tool that `snippy` uses. 

**Step 1: Examine the `snippy` command:**

First, view the `snippy` command and parameters used when you ran snippy:

```bash
grep "outdir" snippy/ERR10479021/snps.log
```

You should see something similar to this on the terminal:

```bash
/apps/conda3/singularity/envs/bioinf/bin/snippy --outdir snippy/ERR10479021 --ref assembly/GCA_000009505.1_ASM950v1_genomic.fasta --R1 reads/ERR10479021_1.fastq.gz --R2 reads/ERR10479021_2.fastq.gz
```

This shows the exact `snippy` command that was run, including the reference genome and sequencing read files. Recording the command and parameters is important for reproducibility, because it allows you to determine exactly how an analysis was performed.

**Step 2: Examine read alignment with `BWA-MEM`**

The first major step is to map the sequencing reads to the reference genome. `snippy` uses `BWA-MEM` to perform this alignment.

 Run the below to see the `bwa mem`command:**

```bash
grep "bwa mem" snippy/ERR10479021/snps.log
```
You should see the `bwa mem` command on the terminal: 

```bash
bwa mem  -Y -M -R '@RG\tID:ERR10479021\tSM:ERR10479021' -t 8 reference/ref.fa /shared/data/public_health_genomics/microbial_genomics/ERR10479021_1.fastq.gz /shared/data/public_health_genomics/microbial_genomics/ERR10479021_2.fastq.gz | samclip --max 10 --ref reference/ref.fa.fai | samtools sort -n -l 0 -T /tmp --threads 3 -m 2000M | samtools fixmate -m --threads 3 - - | samtools sort -l 0 -T /tmp --threads 3 -m 2000M | samtools markdup -T /tmp --threads 3 -r -s - - > snps.bam
```
You do not need to understand every parameter in this command. The important point is:

`BWA-MEM` determines where each sequencing read aligns to the reference genome. This alignment provides the basis for identifying differences between the isolate and the reference genome.

**Step 3: Examine `SAMtools`**

The `BWA-MEM` output is processed using a series of tools, including `SAMtools`, to produce a sorted and processed BAM alignment file.

Run:

```bash
grep "COMMAND: samtools" snippy/ERR10479021/snps.log
```

You should see commands such as:

```bash
 samtools markdup -T /tmp --threads 3 -r -s - -
```

For this practical, the important point is:

`SAMtools` processes the read alignments and produces the BAM file used for downstream variant calling.

`SAMtools` performs several alignment-processing tasks within the `snippy` workflow, including sorting and marking/removing duplicate reads. The resulting BAM file contains the processed alignment information needed for variant calling.


**Step 4: Examine variant calling with `FreeBayes`**

Once the reads have been aligned to the reference genome, `snippy` uses `FreeBayes` to identify positions where the isolate differs from the reference.

Run:

```bash
grep "freebayes" snippy/ERR10479021/snps.log
```

You should see something similar to this on your terminal:

```bash
freebayes-parallel reference/ref.txt 8 -p 2 -P 0 -C 2 -F 0.05 --min-coverage 10 --min-repeat-entropy 1.0 -q 13 -m 60 --strict-vcf   -f reference/ref.fa snps.bam > snps.raw.vcf
```

FreeBayes examines the aligned reads and identifies candidate SNPs, insertions and deletions (indels) relative to the reference genome. These initial variant calls are written to:

`snps.raw.vcf`

The .raw.vcf file therefore contains the variants identified before `snippy` applies its additional filtering criteria.


**Step 5: Variant filtering**

Not every difference identified during variant calling represents a reliable biological variant. Low sequencing coverage, poor-quality bases or poorly mapped reads can produce unreliable variant calls.

`snippy` therefore applies quality filters to the variants identified by `FreeBayes` and produces a set of higher-confidence variants.

The filtered variants are used to generate the final Snippy variant outputs, including:

`snps.vcf`
`snps.tab`
`snps.csv`

For this practical, the important concept is:

`FreeBayes` identifies candidate variants; `snippy` then applies filtering criteria to retain higher-confidence variants.


**Step 6: Generate the consensus sequence with `bcftools`**

Finally, `snippy` uses `bcftools consensus` to apply the identified high quality variants to the reference genome.

Run:

```bash
grep "bcftools consensus" snippy/ERR10479021/snps.log
```

The resulting sequence is a consensus sequence, which represents the reference genome with the variants identified in the isolate incorporated into it.

When you ran `snippy` above, it produced two consensus sequences:
- `snps.consensus.fa` — the reference sequence with high-confidence SNPs and indels incorporated
- `snps.consensus.subs.fa` — the reference sequence with only SNPs incorporated

The consensus sequence is useful because it provides a reconstructed genome sequence representing the isolate relative to the reference.


**Complete the below table:**

What happens at each `snippy` step? 

| **`snippy` step**     | **What is the tool doing?**  |
|:----------------------|:---------------------------- |
| `BWA-MEM`              |                              |                  
| `SAMtools`              |                              |               
| `FreeBayes`             |                              |                  
| `snippy` filtering    |                              |
| `bcftools` consensus    |                              |      


**As a summary you can think of each `snippy` step in this way**: 
- `BWA` asks: Where do the reads belong?
- `SAMtools` asks/handles: How do we process those alignments?
- `FreeBayes` asks: Do the reads provide evidence for a variant?
- `snippy` asks: Which variants meet our quality criteria?
- `bcftools` asks: What does the isolate's genome look like when those variants are applied to the reference?

## 3.3 Now let’s look at some of the snippy output files 

Some of the important output files are:
- The alignments in BAM format. Includes unmapped, multimapping reads (`snps.bam`)
- The final annotated variants in VCF format (`snps.vcf`)
- A simple tab-separated summary of all variants (`.tab`) 
- A version of the reference genome with all variants instantiated (`consensus.fa`)  

To look at the alignment in BAM format:

```bash
 samtools view -H snippy/ERR10479021/snps.bam | head -30
```

To look at final annotated variants: 

```bash 
 head -35  snippy/ERR10479021/snps.vcf 
`````

 To look at the table of variants: 

  ```bash
 head -5  snippy/ERR10479021/snps.tab
 `````

To look at the consensus genome sequence:

```bash
head snippy/ERR10479021/snps.consensus.fa
`````

   
## 3.4 Run snippy over all samples 

You now need to run `snippy` over the remaining samples using the below. You need to create a script called `run_snippy.sh` by typing `nano run_snippy.sh` and pasting in the template below. Then fill in the missing gaps, save the file and close nano. Then run the script.

**This script will take ~30 minutes to finish**

```bash
#!/bin/bash

# Load software


# List of samples with Illumina data

# Loop over each sample


# Run snippy over the remaining samples


done
```

# **4. Run snippy core to create input for building a tree** 
 
We now need to create a core genome SNP alignment to input into our tree building tool, to do this we use snippy-core. 

Now run snippy-core to generate the core genome SNP alignment files across all samples: 

```bash
#Note: This is one long command
snippy-core --prefix snippy/core_genome --ref assembly/GCA_000009505.1_ASM950v1_genomic.fasta snippy/ERR10479021 snippy/ERR10479025 snippy/ERR10479028 snippy/ERR10479029 snippy/ERR10479032 snippy/ERR10479034 snippy/ERR10479035 snippy/ERR10479037 snippy/ERR10479039
```
the above snippy-core command will generate files with core.* prefix. To view a list of the ouput files we can use `ls`

```bash
ls snippy/core*
```
You should see the path to 6 files, and the files that are the most important include:
- core_genome.aln: Core genome SNP alignment 
- core_genome.tab: Tab-seperated table of core SNPs with alleles
- core_genome.full.aln: Whole genome SNP alignment, including invariant sites 

# **5. Build a phylogenomic tree from variant sites**

Run `iqTree` to create a tree from the core SNP alignment:

```bash
# first lets move the core.genome.full.aln into the tree directory using the 'mv' command
mv snippy/core_genome.full.aln tree
# now run iqtree 
iqtree -T 2 --mem 16G -s tree/core_genome.full.aln
```
This creates a phylogenomic tree in newick format - its the 'core_genome.full.aln.treefile' which we will visualise in a tree visualisation tool called Microreact. 

# **6. Visualise the phylogenomic tree using Microreact**

For this practical we will use Microreact to visulise the phylogenomic tree (`core_genome.full.aln.treefile`). Microreact (https://microreact.org) is a free, web-based tool developed by the Centre for Genomic Pathogen Surveillance (CGPS) that allows you to interactively visualise phylogenomic trees alongside epidemiological metadata. It is widely used in public health genomics for outbreak investigation and surveillance.

It is important to understand that many other tools exist to visualise trees such as FigTree and iTOL. 

Download tree file for Microreact: 
- A phylogenomic tree in Newick format (`.nwk` or `.tre`): Download the `core_genome.full.aln.treefile`file from the VM. This can be found in your `Practical_variants_trees/tree` folder
- Open a web browser and go to the Microreact website (https://microreact.org)
- Click on "UPLOAD" to create a new project
- Then drag and drop the `core_genome.full.aln.treefile` file in to the browser
- Click `continue` to visualise and explore the tree.

We want to first add labels to the tree do this: 
Click on the ‘Show controls’ button <img alt="image" src="https://github.com/user-attachments/assets/3da64599-ad7d-4137-839e-8c60baeff42e" /> and then click nodes & Labels and select leaf labels. 

You should now see a tree in Microreact that looks something like this: 

<img alt="image" src="https://github.com/user-attachments/assets/e19ebeab-2639-432a-a1df-139f70edc27b" />


Note this tree is unrooted, which means that it does not have a defined direction in time.

Now midpoint root the tree: 
- In the Microreact tree panel, right click and select midpoint root
- The tree will be re-drawn with the reference genome at the base, giving the tree a temporal direction.

The rooted tree should look something like this: 

<img alt="image" src="https://github.com/user-attachments/assets/28a0b067-3555-4ded-9847-fa12ec4a1a53" />



# **7. Overlay genotyping and epidemiological data onto the tree in Microreact**

One of the most powerful aspects of microbial genomics is the ability to integrate genomic data with epidemiological metadata to investigate outbreak dynamics. 

Download metadata file for Microreact: 
- A metdata `.CSV` file with a column named `id` matching the sample names in the tree is needed: Download the metadata file from the VM. (Note in a real surveillance scenario, your metadata CSV could include columns such as: `collection_date`, `country`, `hospital`, `patient_id`, `MLST_ST`, `resistance_profile`, etc).
- For this practical `metadata.csv` file has been generated for you and can be found in your `Practical_variants_trees/metadata` folder.
- In this file you can see 6 rows: `id` (matching the sample names in the tree), `source` (source of samples linked to the outbreak), `species` (bacterial species as designated in the species classification practical), `mlst_ST` (mlst ST from genotyping practical) and `cgMLST_mlst_group` (from the genotyping practical) and `geography`.  

Now upload the metadata file to Microreact and explore the tree:
- Drag and drop the `metadata.csv` into the Microreact browser containing tree
- Click continue
- Click on column in tree-labels and select "id"
- Click on column in metdata.csv and select "id"
- Click continue
- Then click the Tree tab - top left hand side (ask me if you cant find this)
- Then click Metadata blocks top right and under the "select all" select "source", "species", "mlst_ST", "cg_mlst_group" and "geography"
- Then click legend on the right hand side

  You should see something like this, now with the metadata overlaid next to the phylogenomic tree. 

  <img alt="image" src="https://github.com/user-attachments/assets/d7dc0ca7-b107-4686-9aab-f87832fa82f4" />




# **8. Now compare the MLST, cgMLST and phylogenomics results** 

Complete the below table:

| **Sample**            | **MLST ST**  | **cgMLST cluster** | **Phylogenomic cluster** | **Geography** | **Interpretation** |
|:----------            |:------------ |:------------------ |:-------------------------|:--------------|:-------------------|
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |
| ERR                   |              |                    |                          |               |                    |


**Use the Phylogenomic tree image and the table to answer the following questions:**
- Which isolates appear to represent an outbreak? and do they belong to the same ST, cgMLST group and phylogenomic group?
- What does the close clustering of the ST5438 water and human isolates on the phylogenomic tree indicate about the likely source of the Salmonella outbreak?
- Which isolate is the closest to the water isolate?
- What does the geographical information tell us about the outbreak? and does this further support the genomics results? 
- What do the longer branches of the ST1972 isolates indicate?
- Can isolates with the same ST differ genetically? 

# **9. Re-run analysis using a more closely related genome as the reference genome** 

In the previous sections, you used a relatively distantly related reference genome to map the sequencing reads and identify genetic variants. The choice of reference genome is an important consideration in bacterial variant calling because the reference genome provides the coordinate system against which sequencing reads are mapped and variants are identified.

The reference genome should ideally be closely related to the isolates being analysed. In the context of an outbreak investigation, a closely related reference would typically be a genome from the same sequence type (ST) as the outbreak isolates, or, preferably, a genome from the same outbreak, that is genetically very similar to the isolates under investigation. The closer the reference is to the outbreak isolates, the more likely it is to represent the genomic regions and sequence structure present in those isolates.

Using a distantly related reference can introduce problems during read mapping. Sequence differences between the reference and the isolates can result in some regions mapping poorly or not mapping at all. Highly divergent or absent regions may therefore be excluded from the analysis, while differences between the reference and the isolates may be incorrectly identified as variants. Consequently, the choice of reference can influence both the number and distribution of SNPs identified and the resulting phylogenomic relationships.

For this reason, reference genome selection is particularly important in outbreak investigations, where the aim is often to distinguish between very closely related isolates. A reference genome that is closely related to the outbreak strain can provide better mapping and variant detection across the regions shared by the isolates, potentially producing a more informative and accurate representation of their genetic relationships.

In this section, you will repeat the snippy and snippy-core analyses using a more closely related reference genome and regenerate the phylogenomic tree. You will then compare the new tree with the one generated above to investigate how the choice of reference genome affects the resulting phylogenomic relationships.

## 9.1 Additional directories and data

Add additional directories: 

```bash
# navigate to working directory
cd ~/Practical_variants_trees
# create additional directories needed for this section of the practical
mkdir tree_new_ref close_ref snippy_new_ref
```

A more closely related reference genome has been provided, get the new reference genome:

```bash
# create symlink to the closely related reference genome
ln -s ~/data/public_health_genomics/close_ref/close_ref.fasta close_ref/
```

## 9.2 Repeat snippy using the closely related reference genome

You will now repeat the `snippy` analysis using the closely related genome as the reference genome.

Create a new script called `run_new_ref_snippy.sh` by typing `nano run_new_ref_snippy.sh` and pasting in the template below. Then fill in the missing gaps, save the file and close nano. Then run the script. 

**Important: For this analysis, make sure that the output is directed to the `snippy_new_ref` directory**. Do not use the original `snippy` output directory, as this would overwrite the results from the previous analysis.


**This script will take ~30 minutes to finish**

```bash
#!/bin/bash

# Load software


# List of samples with Illumina data

# Loop over each sample


# Run snippy over the remaining samples, Use close_ref/close_ref.fasta as the reference genome, and direct the results to snippy_new_ref/


done
```

## 9.3 Re-run snippy-core to generate input for a phylogenomic tree

Next, use `snippy-core` to generate a core-genome alignment based on the results obtained using the closely related reference genome:

```bash
snippy-core --prefix snippy_new_ref/core_genome --ref  close_ref/close_ref.fasta snippy_new_ref/ERR10479021 snippy_new_ref/ERR10479025 snippy_new_ref/ERR10479028 snippy_new_ref/ERR10479029 snippy_new_ref/ERR10479032 snippy_new_ref/ERR10479034 snippy_new_ref/ERR10479035 snippy_new_ref/ERR10479037 snippy_new_ref/ERR10479039
```
This will generate the alignment that will be used to reconstruct the phylogenomic tree.

## 9.4 Reconstruct the phylogenomic tree

Run `iqTree` to create a tree from the core genome alignment:

```bash
iqtree -T 2 --mem 16G -s snippy_new_ref/core_genome.full.aln
```

## 9.5 Compare the two phylogenomic trees

You will now compare the phylogenomic tree generated using the original reference genome with the tree generated using the closely related reference genome.

The first tree you created is located in:

`Practical_variants_trees/tree`

The new tree is located in:

`Practical_variants_trees/snippy_new_ref`

For both analyses, the tree file you need is:

`core_genome.full.aln.treefile`

You will first need to download the two tree files, don't forget to also download the metadata file from step 6. 

Then, view each tree in Microreact. The easiest approach is to open two separate browser windows or tabs. In each browser, go to the Microreact website ((https://microreact.org)) and open one of the two trees. Follow the instructions in Step 6 to annotate each tree.

**Using both trees, answer the following questions:**
- Did the overall structure of the tree change?
- Does the outbreak cluster remain together?
- Does the relationship between the outbreak isolates remain the same?
- Did the branch lengths change?
- What does this comparison tell you about the importance of choosing an appropriate reference genome for bacterial outbreak analysis?
























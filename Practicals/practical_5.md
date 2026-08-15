# Antimicrobial resistance gene detection 
By Dr Jessica Webb

{:.no_toc}

* TOC
{:toc}

# **1. Introduction**

<img width="826" height="592" alt="image" src="https://github.com/user-attachments/assets/9ec3dbde-b6cf-4e9d-8b51-780ab1e99480" />


## 1.1 Practical Overview

In this practical, you will analyse genome assemblies (FASTA files) from antimicrobial resistant bacterial isolates:
| **Species**            | **Assembly file name**  | 
|:----------             |:----------|
| _Staphylococcus aureus_              |    ERR017261_Staphylococcus_aureus |   
| _Salmonella typhi_              | ERR2093245_Salmonella_typhi |  
| _Salmonella typhi_               |  ERR2093329_salmonella_typhi   |  
| _Klebsiella pneumoniae_              | ERR4095909_Klebsiella_pneumoniae |  
| _Burkhodleria pseudomallei_              | MSHRxxxxx_ |  

For each of the genomes, you will investigate if antimicrobial resistance determinants are present

## 1.2 Learning Outcomes
1. Use command-line tools to screen bacterial genome assemblies for known AMR determinants
2. Learn how to interperate the main fields in an `AMRFinderPlus` and `abritamr` output file


# **2. Setup**

## 2.1 Activate software
For today's practical, you will need to activate the `bioinf` conda environment:

```bash
source activate bioinf
```

## 2.2 Create directory structure
Let's create a new directory for today's practical and create subdirectories that reflect the main steps in our analysis. This will help us stay organised.

```bash
cd 
```

## 2.3 Get data
The data for today's practical is located in `~/data/microbial_genomics`. As in previous practicals, we will use symlinks instead of copying large data files.

```bash
cd ~/Practical_amr_gene
# create symlinks for genome assembly (FASTA format) files
ln -s ~/data/public_health_genomics/amr_genes/fasta_files/*.fasta assemblies/
# create symlink for amrfinder database 
ln -s ~/data/public_health_genomics/microbial_genomics/amrfinder_db/latest db/
# we can confirm where we are 
pwd
```

If you run the `tree` command, you can see the structure of all the directories and symlinks you've created. It should look something like this:
```
TO ADD IN
```

# **3. Genomic detection of AMR using AMRFinderplus**

To accurately detect antimicrobial resistant genes or mutations, the US National Center for Biotechnology Information (NCBI) developed the Bacterial Antimicrobial Resistance Reference Gene Database and AMRFinder, a tool for detecting AMR genes. More recently, NCBI released AMRFinderPlus, which includes additional functionality such as the detection of point mutations in protein and nucleotide sequences, as well as taxon-specific analyses that can include or exclude specific genes and mutations. AMRFinderPlus is available as a command-line tool. In this section, you will use AMRFinderPlus to analyse antimicrobial resistant bacterial strains. 

**Now run `amrfinder` on one of the bacterial genomes**

Before you run `amrfinder` you would check to see if the most up to date AMR database is downloaded. I have already done this step for you so you don't need to worry about that.

`amrfinder` is a relatively simple tool to run, just give it a genome assembly as the input file. 

Run `amrfinder` on one sample:

```bash
amrfinder -n assemblies/ERR2093245_Salmonella_typhi.fasta -O Salmonella -o amrfinder/ERR2093245_Salmonella_typhi.txt -d db/latest
```

**The command explained:** 
- `-n` Specifies a nucleotide FASTA input
- `-O` `Salmonella`	Specifies the AMRFinderPlus organism/taxonomic group
- `-o`	Specifies the output file
- `-d`	Specifies the AMRFinderPlus database

**Important to remember**
The `-O` option is particularly important when analysing organisms for which AMRFinderPlus contains curated information on resistance-associated mutations. When running AMRFinderPlus, you will need to specify the appropriate organism for each bacterial genome.

The correct organism name must match one of the taxonomic groups recognised by AMRFinderPlus. **Before running your analysis, consider how you could identify the organism names available to AMRFinderPlus and use this information to ensure you provide the exact name required by the `-O` option when running `abritamr`.**

Examine the `amrfinder` outputs:

**Questions:**
- What gene or resistance determinant was detected?
- What antimicrobial or antimicrobial class is it associated with?
- Is the match complete or partial?
- How similar is the detected sequence to the reference?

```````````````````````````````````````````````````````````````````````````````````
TO DELETEEEEEEEEEEEE

``THIS ONE WORKED over Bp in a previous run 

amrfinder -n /shared/a1237649/Practical_amr_variants/assembly/MSHR3763_genomic.fasta -O Burkholderia_pseudomallei -o amrfinder/MSHR3763_amrfinder.txt -d db/latest

``````````````````````````````````````````````````````````````````````````````````````````

# **4. Genomic detection of AMR using abritamr**

`abriTamr` is a pipeline designed to simplify the interpretation of antimicrobial resistant results. It runs AMRFinderPlus and then organises detected resistance determinants into functional antimicrobial classes. It also produces summary files that make it easier to compare resistance determinants across isolates.

`amrfinder` is useful when we want to inspect the detailed evidence for an individual match, whereas `abritamr` provides a more streamlined summary of the resistance determinants detected and their associated drug classes.

Some resistance-associated point mutations are species-specific. Therefore, when using the species option, we need to specify a supported species.

Check the available options:

```bash
abritamr run -h
```

You can search the help output for _Salmonella_

```bash
abritamr run -h | grep Salmonella
```
You should see Salmonella in the terminal output highlighted in red, meaning it can be supplied to the `--species` option.

**Note:** The species option is used when you want abriTAMR/AMRFinderPlus to include species-specific point-mutation analysis. It should not be interpreted as a species-identification method.

Now run `abritamr` over one sample and include `Salmonella` as the designated species: 

```bash
abritamr run --contigs assemblies/ERR2093245_Salmonella_typhi.fasta  --prefix abritamr/ERR2093245_Salmonella_typhi  --species Salmonella 
```

The command:

abritamr run	Run the abriTAMR analysis
--contigs	Specify the genome assembly
assemblies/ERR10479021.fasta	Input FASTA file
--prefix	Specify the output location/prefix
abritamr/ERR10479021	Output prefix
--species Salmonella	Use Salmonella-specific analysis where available

The current abriTAMR documentation describes the --contigs option as accepting either a single FASTA file or a tab-delimited file containing multiple samples and assembly paths.






Running `anritamr` `run` generates five output files per sample:
- amrfinder.out
- summary_matches.txt
- summary_partials.txt
- sumamry_virulance.txt

.....

# **5. Run amrfinder and abritamr over remaining assemblies**





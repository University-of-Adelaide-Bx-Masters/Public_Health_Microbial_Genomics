# **Practical Assignment 1b - Public Health Microbial Genomics**

The data you need for this assignment can be found in `~/data/Assignment1b`. This includes:

- Illumina data for 10 bacterial isolates (Part A and Part B)
- A reference _B. pseudomallei_ genome, `todo.fasta` (Part B)
- Metadata file for interpreting the phylogenomics tree, `Metadata.csv` (Part B)

### **You must use tools and approaches covered during the practicals and scripts are required to be formatted as done in the practicals. All analysis must be performed on your allocated VM in a directory named `~/Assignment1b`.**

Your submission should include the following files:
- Bash scripts named `axxxxxxx_species_genotype.sh` and `axxxxxxx_variants_phylo.sh` for Parts A and B, respectively (replacing axxxxxxx with your student number)
- A single pdf file containing answers to all other questions and screenshots as specified below, named `axxxxxxx_Assignment1b.pdf` (replacing axxxxxxx with your student number). **All screenshots must be clearly legible.** Please ensure that this pdf file also includes your name and student number within the file.

## Part A (species classification and microbial genotyping) - 28 marks
You have been provided with paired-end Illumina whole-genome sequencing (WGS) data for 10 bacterial isolates. Your task for part A is to perform species classification for each of the 10 isolates to determine the bacterial species to which they belong. Based on the identified species, perform multilocus sequence typing (MLST) using the appropriate species-specific MLST scheme. 

 1. Write a bash script called `axxxxxxx_species_genotype.sh` (replacing axxxxxxx with your student number) **[15 marks]** that: 
    - a. Creates a suitable directory structure and creates symlinks for required data files
    - b. Performs species classification for each sample
    - c. Summarises species classification results 
    - d. Performs mlst to get a Sequence Type for each sample
    - d. Summarises mlst results into a single output
    - e. Includes informative comments
    - f. Is clear, concise and easy to understand

2. Based on the results from your species classification analysis, what bacterial species do the 10 isolates belong to? **[5 marks]**. 
 
3. Summarise the main findings from your mlst analysis **[8 marks]**. Make sure you include the following information:
    - a. How many MLST STs are present? 
    - b. What are the MLST STs present? 
    - c. How many samples belong to each MLST ST? 
    - d. Do you think that an outbreak is present? what analysis might you do next to confirm that an outbreak is present? explain your answer. 
    
## Part B (Variant calling & phylogenomic tree) - 17 marks

1. Write a bash script called `axxxxxxx_variants_phylo.sh` (replacing axxxxxxx with your student number) to generate phylogenomic tree, using the xxx.fasta as the reference genome. **[10 marks]**.
   The script should contain the following components:
    - a. Performs variant calling (`snippy`)
    - b. Creates a core genome alignment (`snippy-core`)
    - c. Generates a phylogenomic tree (`igtree`)
   
2. microreact and provided metadata **[7 marks]**.
    - a. dddd
    - b. dddd
    - c. ddddd

## Part C (antimicrobial resistance) - 5 marks
Please answer the following questions about the two AMR practicals:
1. something on genome annotation 

## Part D - 10 marks 
Please answer the following questions about the microbial genotyping practical:
1. For the cgMLST results, at 10 allelic differences how many isolates remained clustered together and which isolates were they? Please include a screenshot demonstrating this **[5 marks]**
2. Based on the MLST and cgMLST results how many outbreaks were present? explain your answer. **[5 marks]**


   

# **Practical Assignment 1b - Public Health Microbial Genomics**

The data you need for this assignment can be found in `~/data/Assignment1b`. This includes:

- Illumina and corresponding assembly data for 7 bacterial isolates (Part A and Part B)
- A reference genome, `ATS2021_genome_reference.fasta` (Part B)
- Metadata file for interpreting the phylogenomics tree, `Metadata.csv` (Part B)

### **You must use tools and approaches covered during the practicals and scripts are required to be formatted as done in the practicals. All analysis must be performed on your allocated VM in a directory named `~/Assignment1b`.**

Your submission should include the following files:
- Bash scripts named `axxxxxxx_species_genotype.sh` and `axxxxxxx_variants_phylo.sh` for Parts A and B, respectively (replacing axxxxxxx with your student number)
- A single pdf file containing answers to all other questions and screenshots as specified below, named `axxxxxxx_Assignment1b.pdf` (replacing axxxxxxx with your student number). **All screenshots must be clearly legible.** Please ensure that this pdf file also includes your name and student number within the file.

## Part A (species classification and microbial genotyping) - 28 marks
You have been provided with paired-end Illumina whole-genome sequencing (WGS) data and corresponding genome assemblies for 7 bacterial isolates. Your task for part A is to perform species classification for each of the 7 isolates to determine the bacterial species to which they belong. Then based on the identified species, perform multilocus sequence typing (MLST) using the appropriate species specific MLST scheme. 

 1. Write a bash script called `axxxxxxx_species_genotype.sh` (replacing axxxxxxx with your student number) **[15 marks]** that: 
    - a. Creates a suitable directory structure and creates symlinks for required data files
    - b. Performs species classification for each sample
    - c. Summarises species classification results 
    - d. Performs mlst to get a Sequence Type for each sample
    - d. Summarises mlst results into a single output
    - e. Includes informative comments
    - f. Is clear, concise and easy to understand

2. Based on the results from your species classification analysis, what bacterial species do the 7 isolates belong to? **[3 marks]**. 
 
3. Summarise the main findings from your MLST analysis **[8 marks]**. Make sure you include the following information:
    - a. How many MLST STs are present? 
    - b. What are the MLST STs present? 
    - c. How many samples belong to each MLST ST? 
    - d. Do you think that an outbreak is present? what analysis might you do next to confirm that an outbreak is present? explain your answer.
    - e. What was the species specific flag that you provided to the MLST tool? 
  
 4. Based on the results from your species classification and mlst analysis, complete the `species` and `mlst_ST` columns in the provided `Metadata.csv` file. **[4 marks]**. 
    
## Part B (Variant calling & phylogenomic tree) -**[17 marks]**

1. Write a bash script called `axxxxxxx_variants_phylo.sh` (replacing axxxxxxx with your student number) to generate phylogenomic tree using the `ATS2021_genome_reference.fasta` as the reference genome. **[10 marks]**.
   The script should contain the following components:
    - a. Performs variant calling 
    - b. Creates a core genome alignment 
    - c. Generates a phylogenomic tree 
   
2. Input the final tree file into Microreact, overlay the provided metadata file (`Metadata.csv`) onto the tree and annotate the tree as you did during the corresponding practical session. **[7 marks]**.
    - a. Include a screenshot of your final phylogenomic tree
    - b. dddd
    - c. ddddd

## Part C (antimicrobial resistance) - **[5 marks]**
Please answer the following questions about the two AMR practicals:
1. something on genome annotation 

## Part D - **[10 marks]**
Please answer the following questions about the microbial genotyping practical:

1. For the cgMLST results, at 10 allelic differences how many isolates remained clustered together and which isolates were they? Please include a screenshot demonstrating this **[5 marks]**
      
2. Based on the MLST and cgMLST results how many outbreaks were present and what was the source of the outbreak? explain your answer. **[5 marks]**


   

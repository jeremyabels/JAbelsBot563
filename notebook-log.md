20250211

Iowa bluegill dataset description: 

Bluegill are among the most common species in the family Centrarchidae, which contains the sunfishes, crappies, and large- and smallmouth bass. Bluegill are native to North America, though they have been naturalized in temperate climates worldwide.
This dataset is a single-digest RAD-seq dataset that was the primary focus of my MS. Unlike my work for my PhD, it is ‘complete’ with more than 400 individuals across half a dozen populations. These populations have not yet been examined phylogenetically, as the original project focused on phenotypic associations and measures of genetic diversity. I hope to utilize the results from these phylogenetic analyses in the final manuscript.

Note: As of 11 Feb 2025, I am working with my MS advisor to obtain the entire dataset. Many more individuals have been sequenced since I graduated in 2022. The final project will contain these data, and will be based on data that has already been reference aligned. For the git commit assignment due on 11 Feb 2025, I will be using a small subset of the raw dataset to demonstrate my ability to run FastQC.

Steps

1: Transfered raw data from a single NGS library 'I11' to Weber lab server
	-Located under /home/jabels/data/Bluegill2021

2: Created directory for FastQC Results
	- /home/jabels/JAbelsBot563/FastQC

3: Navigate to directory with data
	~ % cd /home/jabels/data/Bluegill2021

4: FastQC already installed & updated under 'Jabels" conda environment
	~ % conda activate Jabels

5: Call FastQC for all .fastq.gz files in directory
	~ % fastqc --noextract -o /home/jabels/JAbelsBot563/FastQC *fastq.gz

6: evaluate results
	-Reads need to be trimmed to remove adapter sequences and pcr duplicates need to be removed before alignment to the reference genome. These steps can be done in Stacks: https://catchenlab.life.illinois.edu/stacks/ 
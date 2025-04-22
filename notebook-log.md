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

20250304

Bluegill data were too large to work with effectively for the purposes of this course.

New Dataset description: Shifted to using data from the parastic copepod genus Salmincola, my current study system. These are ectoparasites that infect members of the family Salmonidae in freshwater.

Few genetic resources, built a small dataset out of COI gene sequences for three species found in NA: S. siscowet, S. edwardsii and S. californiensis. These species have ecological variatiion that makes them an interesting group to study. Additionally added three sequences from an unknown Salmincola species from Nunavut, Canada and an outgroup species (Achtheres pimelodi). The outgroup species is from the same family (Lernaeopodidae) and also infects freshwater salmonids. 

Reorganized Folders:
Deleted all old bluegill files
Created "Final_Project" subdirectory in "JabelsBot563" directory
Created subdirectories "Data", "Metadata" and "Output" within "Final_Project"

Sequence alignment:

Downloaded FASTA files for COI gene sequences.
NCBI accession numbers are in text file in Metadata subdirectory.
Trimed IDs of sequences in the FASTA file. Trimmed IDs can be associated with accession numbers in SalmincolaCOI.annot.csv file

Ran multiple sequence alignment with muscle

1: ~ % muscle -in /home/jabels/JAbelsBot563/Final_Project/Data/SalmincolaCOIAnnotated.fasta -out SalmincolaCOIAligned.fasta

Created neighbor joining and max. parsimony trees using packages ape and phangorn in R

Reproducible script is in "Scripts" subdirectory.


20250325

Creating Maximum Likelihood tree with IQTree.
Renamed .fasta files to use .fa suffix to reduce filename length.

Install IQTree2 after activating conda environment

1: ~ % conda install bioconda::iqtree


Navagate to output subdirectory containing muscle-aligned .fasta file

1: ~ % cd /home/jabels/JAbelsBot563/Final_Project/Output

Run IQTree on data

1: ~ % iqtree -s SalmincolaCOIAligned.fa -m MFP

This produces a number of files, including a .iqtree file which contains the report of the best-fitting models. In the case of these data, HKY+F+G4 is the best-fitting model.

2: ~ % iqtree -s SalmincolaCOIAligned.fa -m HKY+F+G4 -redo

This redoes the analyses with the most appropriate model. 

From this point, the provided treefile can be used for vizualization and figure creation.

IQTree Cheat Sheet


Description: Creates trees using maximum likelihood (ML). Evaluates substitution models to determine best fit. Builds initial maximum parsimony tree and then uses bootstapping to randomly assess sites and build most likely tree. 

Strengths: Accounts for missing data by reproting trees with identical likelihoods allowing users to know when more data must be gathered. Uses random pertubations to escape local optima. Scales easilyt with large datasets. 

Weaknesses: Main weakness is in random pertubations which reduces reproducibility. Memory and computationally intensive.

Assumptions: Assumes users will run software multiple times to limit the effect of randomness.

Options: Can use NNI for improved treespace search. Can change number of initial trees. Can specificy evolutionary model based on either nucleic acid or protein samples.

20250410

Ran MrBayes on data on Windows

Downloaded MrBayes .exe from GitHub

Converted aligned .fasta files to .nexus using online tool: http://phylogeny.lirmm.fr/phylo_cgi/data_converter.cgi

Running MrBayes

1: MrBayes > exe SalmincolaCOIAligned.nexus

Puts data into MrBayes.

2: MrBayes > lset nst=2 rates=gamma

Sets substituion mode, choices informed by IQTree results above.

Kept priors at default.

3: MrBayes > mcmcp ngen=20000

Set initial number of generations to 20000.

4: MrBayes > mcmcp diagnfreq=2000

Lowered freqeuncy of diagnostics to 2000 to account for smaller dataset.

5: MrBayes > mcmc

Ran the analyses. Total generations was 400000 and average standard deviation was lowered to ~0.14.

6: MrBayes > sumt

Visualized summary statistics and trees.


MrBayes Cheatsheet

Description: MrBayes is a program for performing Bayeesian inference of phylogenies. MrBayes uses Markov Chain Monte Carlo methods to estimate the posterior distribution of model parameters. 

Strengths: Can utlizize and combine multiple types of data including non-nucleotide data. Models increase the chance of escaping local optima in large datasets. 

Assumptions: Assumes equal rates of evolution across sites without user input specifying this is not the case. Much depends on the chosen substitution model. Chosen priors can change outcomes significantly. 

Weaknesses: Bootstrap support values for ML or Parsimony tend to be lower than posterior probabilities for Bayesian approaches using the same data.

Options: Priors: topology, branch length, nucloetide substituion rates, stationary frequencies, proportion of invariable sites. Partitioning by data type. Set model tructure to link/unlink models across data subset.



20250422

Running Astral on example dataset, as my data is only a single gene. 

Downloaded Astral for Windows from: https://github.com/smirarab/ASTRAL/tree/master

Following Tutorial: https://github.com/smirarab/ASTRAL/blob/master/astral-tutorial.md#installation

Running astral:

1: Naviagate to directory with Astral .jar file

cd  C:\Users\jerem\OneDrive\Desktop\Astral

2: run Astral on mammals dataset

java -jar astral.5.7.8.jar -i test_data/song_mammals.424.gene.tre -o test_data/song_mammals.tre

3: Testing astral on a larger dataset

java -jar astral.5.7.8.jar -i test_data/1KP-genetrees.tre -o test_data/1kp.tre 2> test_data/1kp.log

Utlized FigTree to view the outputs

Astral Cheatsheet

Description: A method used to estimate species trees after inferring a set of gene trees.

Strengths: Statiscally consistent, scaleable, high accuracy in simulated studies. 

Assumptions: Presence of a gene for one species should be independent of the gene tree topology and presence of other genes. Assumption of locality: when calculating branch support, astral assumes that all four branches around it are correct. 

Weaknesses: Cannot work with a single gene. Requires java.

Options: Can run on either large or small datasets. Can be used with unresolved gene trees.









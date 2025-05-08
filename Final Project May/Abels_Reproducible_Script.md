##Dataset description: Data from the parastic copepod genus Salmincola, my current study system. These are ectoparasites that infect members of the family Salmonidae in freshwater. Cytochrom oxidase subunit 1 (COI) gene, a common mitochondrial marker used in phylogenetics.

##Sequence alignment:

##Download these accesion numbers as a single FASTA.
OQ844022.1
OQ844023.1
OQ844024.1
OQ844025.1
OQ844026.1
OQ844027.1
OQ844028.1
OQ844029.1
OQ844030.1
OQ844031.1
OQ844032.1
OQ844033.1
OQ844034.1
OQ844035.1
OQ844036.1
OQ844037.1
OQ844038.1
OQ844039.1
OQ844040.1
OQ844041.1
OQ872370.1
OP829963.1
OP830199.1
OP830294.1
OP830355.1
OP830356.1
OP830357.1
OP830358.1
OQ843996.1
OQ843997.1
OQ843998.1
OQ843999.1
OQ844000.1
OQ844001.1
OQ844002.1
OQ844003.1
OQ844004.1
OQ844005.1
OQ844006.1
OQ844007.1
OQ844008.1
OQ844009.1
OQ844010.1
OQ844011.1
OQ844012.1
OQ844013.1
OQ844014.1
OQ844015.1
OQ844016.1
OQ844017.1
OQ844018.1
OQ844019.1
OQ844020.1
OQ844021.1
LC713314.1
LC713315.1
LC713316.1
LC713317.1
LC713318.1
LC713319.1
LC713320.1
LC713321.1
LC713322.1
LC713323.1
LC713324.1
LC713325.1
OQ355023.1
OQ355024.1
OQ355025.1
OQ355026.1
OQ355027.1
OQ355028.1
OQ355029.1
OQ844042.1
OQ844043.1
OQ844044.1
OQ844045.1
OQ844046.1
OQ844047.1
OQ844048.1
OQ844049.1
OQ844050.1
OP829914.1
OP830136.1
OP830171.1
OP830173.1
OP830337.1
OQ843970.1
OQ843971.1
OQ843972.1
OQ843973.1
OQ843974.1
OQ843975.1
OQ843976.1
OQ843977.1
OQ843978.1
OQ843979.1
OQ843980.1
OQ843981.1
OQ843982.1
OQ843983.1
OQ843984.1
OQ843985.1
OQ843986.1
OQ843987.1
OQ843988.1
OQ843989.1
OQ843990.1
OQ843991.1
OQ843992.1
OQ843993.1
OQ843994.1
OQ843995.1
OP830005.1
OP830067.1
OP830211.1
OP830325.1
MG311685.1
MG313616.1
MG313633.1
OQ844051.1
OQ844052.1
OQ844053.1
OQ844054.1
OQ844055.1
OQ844056.1
OQ844057.1
OQ844058.1
OQ844059.1
OQ844060.1
OP830208.1

------------------------------This Portion Done in Linux---------------------------------------------------------------------
#Trim the IDs of sequences in the FASTA file. Store the trimmed IDs and accession numbers in a spreadsheet for future reference.

#Create a directory to contain the Salmincola Project

#Example /home/usr/Desktop/Salmincola_Project

#Navigate to this directory

1: ~ % cd /home/usr/Desktop/Salmincola_Project

#Create a conda environment for portions of project done in linux

1: ~ % Conda create Salmincola_Project
2: ~ % Conda activate Salmincola_Project
3: ~ % Conda install -c bioconda muscle
4: ~ % Conda install -c bioconda clustalw
5: ~ % Conda install -c bioconda iqtree

#Run multiple sequence alignment with muscle

1: ~ % muscle -in /home/jabels/JAbelsBot563/Final_Project/Data/Salmincola_COI_CleanIDs.fasta -out Salmincola_COI_CleanIDs_AlignedMuscle.fasta

#Run Multiple sequence alignment with ClustalW

1: ~ % clustalw2 Salmincola_COI_CleanIDs.fasta -output fasta


#Creating Maximum Likelihood tree with IQTree.


#Navagate to output subdirectory containing muscle-aligned .fasta file

1: ~ % cd /home/usr/Desktop/Salmincola_Project

#Run IQTree on data

1: ~ % iqtree -s Salmincola_COI_CleanIDs_AlignedMuscle.fasta -m MFP

#This produces a number of files, including a .iqtree file which contains the report of the best-fitting models. In the case of these data, HKY+F+G4 is the best-fitting model.

2: ~ % iqtree -s Salmincola_COI_CleanIDs_AlignedMuscle.fasta -m HKY+F+G4 -redo

#This redoes the analyses with the most appropriate model. 

#From this point, the provided treefile can be used for vizualization and figure creation. I recommend FigTree and its simple point-and-click interface for basic visualizations.


--------------------------------------------------------------------------------------------------------------------------------------


------------------------------------------The following Portion was done in R---------------------------------------------------------

##Creating a Simple Neighbor-Joining tree with ape

##Install Packages, if necessary

install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)

##Load Packages

library(ape)
library(adegenet)
library(phangorn)

##Set working Directory

setwd("C:/Users/jerem/OneDrive/Desktop/Phylo-Local/Final Project May//")

##Import Data

dna <- fasta2DNAbin(file="Salmincola_COI_CleanIDs_AlignedMuscle.fasta")

##Compute Genetic Distances

D <- dist.dna(dna, model="TN93")

##Check Class and Length

class(D)
length(D)


##Get Neighbor-Joining Tree

tre <- nj(D)

##Ladderize Tree

tre <- ladderize(tre)

##Plot Simple Neighbor-Joining Tree

plot(tre, cex=.6)
title("A simple NJ tree")


plot(tre, show.tip=TRUE)
title("Unrooted NJ tree")


##Root the tree using the Achtheres species
##Root the tree with Achtheres as outgroup and Ladderize the Tree

tre2 <- root(tre, out="Achtheres.pimelodi.NY.1")
tre2 <- ladderize(tre2)

##Plot the Rooted Tree

plot(tre2, show.tip=TRUE, edge.width=1)
title("Rooted NJ tree")


##Creating a Maximum Parsimony Tree with Phygorn

##Convert Data to PhyDat format

dna2 <- as.phyDat(dna)
class(dna2)

##Check Object

dna2

##Create basic NJ tree
tre.ini <- nj(dist.dna(dna,model="raw"))
tre.ini

##Check Parsimony, Optimize Tree, Check optimized tree

parsimony(tre.ini, dna2)
tre.pars <- optim.parsimony(tre.ini, dna2, )
tre.pars

tre.pars.root <- root(phy = tre.pars, outgroup = "Achtheres.pimelodi.NY.1",
     resolve.root=TRUE, edgelabel=TRUE)

##Plot Tree

plot(tre.pars.root, type="phylogram", show.tip=TRUE, edge.width=2)
title("Maximum-parsimony tree")

--------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------The following portion was completed using MrBayes Command Line Interface on Windows---------------------------


#Running MrBayes on Salmincola data on Windows

#Download MrBayes .exe from GitHub: https://nbisweden.github.io/MrBayes/download.html

#Converted aligned .fasta files to .nexus using online tool: http://phylogeny.lirmm.fr/phylo_cgi/data_converter.cgi

Running MrBayes

1: MrBayes > exe SalmincolaCOIAligned.nexus

#Puts data into MrBayes.

2: MrBayes > lset nst=2 rates=gamma

#Sets substituion mode, choices informed by IQTree results above. nst=2 uses HKY

3: MrBayes > mcmcp ngen=20000

#Set initial number of generations to 20000.

4: MrBayes > mcmcp diagnfreq=2000

Lowered freqeuncy of diagnostics to 2000 to account for smaller dataset.

5: MrBayes > mcmc

#Ran the analyses. Total generations was 2000 and average standard deviation was lowered to ~0.08.

6: MrBayes > sumt

#Visualize summary statistics and trees using figtree.





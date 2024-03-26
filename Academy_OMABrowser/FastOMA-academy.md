# Module 3: FastOMA

////

## 3.1 Obtaining data and getting setup



///I
FastOMA is a software for inferring homology information on your custom genomes,  including generating Hierarchical Orthologous Groups. It takes as input the protein sequence in FASTA format in addition to the species tree. 


///T

---

For Modules 2-4, you will need to use our [GitPod instance](https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy/)

---

In this exercise, we will run FastOMA standalone to infer the orthology information for five yeast species.  We already provided the proteomes of five species in the GitPod environment, located at ```/workspace/FastOMA-academy/Module_FastOMA/working_dir/in_folder/proteome```. 

Another input needed by FastOMA is the species tree. For our case, the species tree in [newick format](https://en.wikipedia.org/wiki/Newick_format) is provided in the [GitPod workspace](https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy/): ```Module_FastOMA/working_dir/in_folder/species_tree.nwk```.  It is as follows:

```
(((Yarrowia_lipolytica:1,Saccharomyces_cerevisiae:1)Saccharomycetales:1,(Neosartorya_fumigata:1,Sclerotinia_sclerotiorum:1)leotiomyceta:1)Saccharomyceta:1,Schizosaccharomyces_pombe:1)Ascomycota;
```

The FastOMA software is already installed, and you should be able to use it  after logging into your GitPod workspace. 

---

***Optional** (If you are not using GitPod)*

If you want to install FastOMA on your system, you can follow the installation instructions on the [FastOMA GitHub page](https://github.com/DessimozLab/fastoma)[https://github.com/DessimozLab/fastoma].

If you want to download the proteomes on your own system, check out the following hint:

///H
**Instruction for downloading proteomes**
The UniProt database includes the proteome of species. You can download the reference proteomes of the following species from UniProt by clicking on “Download one protein sequence per gene (FASTA)”: 
<ul>
<li>Schizosaccharomyces pombe (Fission yeast) <a href='https://www.uniprot.org/proteomes/UP000002485'>https://www.uniprot.org/proteomes/UP000002485</a></li>
<li>Sclerotinia sclerotiorum (White mold) <a href='https://www.uniprot.org/proteomes/UP000001312/'>https://www.uniprot.org/proteomes/UP000001312/</a></li>
<li>Yarrowia lipolytica  <a href='https://www.uniprot.org/proteomes/UP000001300'>https://www.uniprot.org/proteomes/UP000001300)</a></li>
<li>Saccharomyces cerevisiae  <a href='https://www.uniprot.org/proteomes/UP000002311/'>https://www.uniprot.org/proteomes/UP000002311/</a></li>
<li>Neosartorya fumigata (Aspergillus fumigatus) <a href='https://www.uniprot.org/proteomes/UP000002530/'>https://www.uniprot.org/proteomes/UP000002530/</a><</li>
</ul>

Right click on “Download one protein sequence per gene (FASTA)" and copy the link. Then, use wget to download the file and unzip the file using gunzip software. For example for Schizosaccharomyces pombe:

```wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000002485/UP000002485_284812.fasta.gz 
gunzip -k UP000002485_284812.fasta.gz
```

///T

---

///Q
1.  In what format are the proteome files?

///R
FASTA format

///Q
2.  How many proteins are there in the Schizosaccharomyces pombe proteome?

///H
Each record in a FASTA file always starts with ">".  You can use the following command line to calculate number of records in the FASTA file:  ```grep ">" in_folder/proteome/Schizosaccharomyces_pombe.fa  | wc -l```

///R
 There are 5122 proteins in this FASTA file. 

///Q
3.  How many leaves are in the species tree? For how many species does the species tree provide evolutionary information? 

///R
The answer to both questions is 5. You can visualize the species tree using the [phylo.io](https://beta.phylo.io) website. 

////

## 3.2 Running FastOMA

///T
The FastOMA algorithm runs in three main steps: 

<ol>
<li> Mapping input proteins to HOGs in the OMA  database using OMAmer (see Module 2)
</li>
<li>Inferring gene families based on the OMAmer results 
</li>
<li>Orthology inference in the form of Hierarchical Orthologous Groups (HOGs)
</li>
</ol>

Note that these steps are executed thanks to our highly-parallelized pipeline implemented in [Nextflow](https://www.nextflow.io/). The output of FastOMA is reported in [OrthoXML](https://orthoxml.org/0.4/orthoxml_doc_v0.4.html), which is the standard format of HOG. For more information on HOGs, see Module 1 and also Page 4 of [Zahn-Zabal et al. F1000, 2020](https://f1000research.com/articles/9-27/v1).

First change directory to the Module3_FastOMA/working_dir/ where the folder in_folder exists. 

```cd /workspace/FastOMA-academy/Module_FastOMA/working_dir``` 

Then, check whether Nextflow is installed your system by running ```nextflow -h ```. Now we can use the command line to run FastOMA on the five proteomes in ```in_folder/species_tree.nwk```, also using the species tree from ```in_folder/species_tree.nwk.```

///Q
1. What is the command line to run FastOMA?

///H
Check the [FastOMA GitHub page](https://github.com/DessimozLab/fastoma). Important note: without specifiying ```--omamer_db``` FastOMA will download the big database, which could be problematic for gitpod. 

///R
```nextflow FastOMAnf  --input_folder in_folder   --output_folder out_folder --omamer_db in_folder/omamerdb.h5```

///T
Execute the above command to run FastOMA.  

///Q 
2. Where is the output orthoXML file?

///H
Check the directory where in_folder is present.

///R
Output_folder

////

## 3.3 Intepreting the results

///I
Recall that Orthologous Groups are groups of strict orthologs, with at most 1 representative per species. Hierarchical Orthologous Groups are groups of orthologs and paralogs, defined at each taxonomic level.

///T
The output of  FastOMA includes three folders (hogmap, OrthologousGroupsFasta, RootHOGsFasta) and seven files (FastOMA_HOGs.orthoxml, orthologs.tsv.gz,  OrthologousGroups.tsv   phylostratigraphy.html,  report.ipynb,  RootHOGs.tsv, report.html,  species_tree_checked.nwk).  

The hogmap folder includes the output of OMAmer (Described in [OMAmer Module](https://omabrowser.org/oma/academy/module/OMAmer_2023)); each file corresponds to an input proteome. The folder OrthologousGroupsFasta includes FASTA files, and all proteins inside each FASTA file are orthologous to each other. These could be used as gene markers for species tree inference (Module 3).

///Q
1.  How many Orthologous Groups are there? 

///H
You can count the number of FASTA files in the folder OrthologousGroupsFasta.

///R
6773, Note: The numbers could slightly change in different runs. 

///Q
2.  How many genes in total are present in all Orthologous Groups? 

///H
Genes are coded in the orthoxml file as <geneRef id="1002001760"/\>. We need to count the number of lines including  "geneRef": 
``` grep geneRef output_hog.orthoxml  | wc -l```

///R
There are 22606 genes in the groups.  

///T
Orthologous Groups which have a representative gene in every species could be considered as the core genome. 

///Q
3. How many Orthologous Groups include one representative gene for each species? 

///H
Count how many groups in OrthologousGroups.tsv have five genes. You can count how many times each group appears using this command: ```cat OrthologousGroups.tsv | cut -f 1 |  sort | uniq -c |  awk '{print $1}' | grep 5 | wc -l ```.  

///R
 There are 1618 Orthologous Groups having five genes. 

///Q
4. How many Root HOGs are in the HOG file?

///H
Each line in the output file RootHOGs.tsv denotes a gene. You can count how many times each RootHOG appears using this command ``` cat RootHOGs.tsv | cut -f 1 |  sort | uniq -c  | wc -l```
 Note that the first line is the header. So the output value - 1 will be the answer.

///R
There are 6793 rootHOG (gene families) in this file.

///Q
5. Consider the gene “60S ribosomal protein L15-A” in *Schizosaccharomyces pombe* with protein ID: RL15A_SCHPO. How many proteins are in the gene family (for these 5 species of interest)?

///H
Find the corresponding line in the RootHOGs.tsv using ```$ grep RL15A_SCHPO RootHOGs.tsv ```. Then, use grep on the first column ```$ grep "HOG:0006324" RootHOGs.tsv ```

///R
There are 7 proteins in this family.

///Q
6. Which genes are orthologous to the gene A7EQW0_SCLS1? 

///H
Find the corresponding line in the OrthologousGroups.tsv using ```$ grep A7EQW0_SCLS1 OrthologousGroups.tsv ```. Then, use grep on the first column ```$ grep "OG_0003358" OrthologousGroups.tsv ```
 .

///R
 'tr|Q4WEI0|Q4WEI0_ASPFU', 'tr|A7EQW0|A7EQW0_SCLS1', 'sp|P32468|CDC12_YEAST', 'tr|Q6C7L3|Q6C7L3_YARLI', 'sp|P48009|SPN4_SCHPO

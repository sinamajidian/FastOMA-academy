# Module 3: FastOMA

////

## 3.1 Obtaining data and getting setup



///I
FastOMA is a software package for inferring homologous relationships between coding genes of multiple species, including generating Hierarchical Orthologous Groups. It takes as input  several files in the FASTA format; each containing the sequences of proteins coded by all genes in a species genome - the proteome. It also requires a species tree, resolving the relationship between species


///T

---

For this Module, you will need to use our [GitPod instance](https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy/)

---

In this exercise, we will run FastOMA standalone to infer the orthology information for five yeast species.  We already provided the proteomes of five species in the GitPod environment, located at ```/workspace/FastOMA-academy/Module_FastOMA/working_dir/in_folder/proteome```. 

Another input needed by FastOMA is the species tree. For our case, the species tree in [newick format](https://en.wikipedia.org/wiki/Newick_format) is provided in the [GitPod workspace](https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy/): ```Module_FastOMA/working_dir/in_folder/species_tree.nwk```.  It is as follows:

```
(((Yarrowia_lipolytica:1,Saccharomyces_cerevisiae:1)Saccharomycetales:1,(Neosartorya_fumigata:1,Sclerotinia_sclerotiorum:1)leotiomyceta:1)Saccharomyceta:1,Schizosaccharomyces_pombe:1)Ascomycota;
```
You can visualize the species tree using the [phylo.io](https://beta.phylo.io/viewer/?session=8035308639183da6bdbcf883de3f30750e13ffa7) website. 

In the GitPod, FastOMA is already installed – you should be able to use it after logging into your GitPod workspace. 

---

***Optional** (If you are not using GitPod)*

If you want to install FastOMA on your system, you can follow the installation instructions on the [FastOMA GitHub page](https://github.com/DessimozLab/fastoma)[https://github.com/DessimozLab/fastoma].

If you want to download the proteomes on your own system, check out the following hint:

///H
**Instruction for downloading proteomes**
The UniProt database includes the proteomes of many species. You can download the reference proteomes of the following species from UniProt by clicking on “Download one protein sequence per gene (FASTA)”: 
<ul>
<li>Schizosaccharomyces pombe (Fission yeast) <a href='https://www.uniprot.org/proteomes/UP000002485'>https://www.uniprot.org/proteomes/UP000002485</a></li>
<li>Sclerotinia sclerotiorum (White mold) <a href='https://www.uniprot.org/proteomes/UP000001312/'>https://www.uniprot.org/proteomes/UP000001312/</a></li>
<li>Yarrowia lipolytica  <a href='https://www.uniprot.org/proteomes/UP000001300'>https://www.uniprot.org/proteomes/UP000001300)</a></li>
<li>Saccharomyces cerevisiae  <a href='https://www.uniprot.org/proteomes/UP000002311/'>https://www.uniprot.org/proteomes/UP000002311/</a></li>
<li>Neosartorya fumigata (Aspergillus fumigatus) <a href='https://www.uniprot.org/proteomes/UP000002530/'>https://www.uniprot.org/proteomes/UP000002530/</a></li>
</ul>

Right click on “Download one protein sequence per gene (FASTA)" and copy the link. Then, use ```wget``` to download the file and unzip the file using gunzip software. For example for Schizosaccharomyces pombe:

```wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000002485/UP000002485_284812.fasta.gz 
gunzip -k UP000002485_284812.fasta.gz
```

///T

---

///Q
1.  In which format are the proteome files?

///R
FASTA format

///Q
2.  How many proteins are there in the <i>Schizosaccharomyces pombe</i> proteome?

///H
Each record in a FASTA file always starts with ">".  You can use the following command to calculate the number of records in the FASTA file:  ```grep ">" in_folder/proteome/Schizosaccharomyces_pombe.fa  | wc -l```

///R
 There are 5,122 proteins in this FASTA file. 

///Q
3.  How many leaves are in the species tree? For how many species does the species tree provide evolutionary information? 

///R
The answer to both questions is 5. 

////

## 3.2 Running FastOMA

///T
The FastOMA algorithm runs in three main steps: 

<ol>
<li> Mapping input proteins to existing Hierarchical Orthologous Groups (HOGs) in the OMA database using OMAmer (see Module 2)
</li>
<li>Inferring gene families based on the OMAmer results 
</li>
<li>Orthology inference in the form of new HOGs
</li>
</ol>

Note that these steps are efficiently executed, thanks to our highly-parallelized pipeline implemented in [Nextflow](https://www.nextflow.io/). The output of FastOMA is reported in [OrthoXML](https://orthoxml.org/0.4/orthoxml_doc_v0.4.html), which is the standard format for HOGs. For more information on HOGs, see Module 1 and also Page 4 of [Zahn-Zabal et al. F1000, 2020](https://f1000research.com/articles/9-27/v1).

First change directory to the ```Module3_FastOMA/working_dir/``` where the folder ```in_folder``` exists. 

```cd /workspace/FastOMA-academy/Module_FastOMA/working_dir``` 

Then, check whether Nextflow is installed on your system by running ```nextflow -h ```. Now we can use the command line to run FastOMA on the five proteomes in ```in_folder```, also using the species tree from ```in_folder/species_tree.nwk.```

///Q
1. What is the command line to run FastOMA using a local OMAmer database?

///H
Check the [FastOMA GitHub page](https://github.com/DessimozLab/fastoma). Important note: without specifying ```--omamer_db``` FastOMA will download a large database, covering the entire OMA database. This is not a problem for most machines, but could be problematic on GitPod.

///R
```nextflow /workspace/FastOMA/FastOMA.nf  --input_folder in_folder   --output_folder out_folder --omamer_db in_folder/omamerdb.h5```

///T
Execute the above command to run FastOMA.  

///Q 
2. Where is the output orthoXML file?

///H
Check the parent directory of```in_folder```. What does it contain?

///R
Output_folder. The output of FastOMA includes three folders (hogmap, OrthologousGroupsFasta, RootHOGsFasta) and seven files (FastOMA_HOGs.orthoxml, orthologs.tsv.gz,  OrthologousGroups.tsv   phylostratigraphy.html,  report.ipynb,  RootHOGs.tsv, report.html,  species_tree_checked.nwk).  

////

## 3.3 Interpreting the results

///I
Recall that Orthologous Groups are groups of strict orthologs, with at most 1 representative per species. Hierarchical Orthologous Groups are groups of orthologs and paralogs, defined at each taxonomic level.

///T
The output of FastOMA includes three folders 
*hogmap : contains the OMAmer results used by FastOMA
*OrthologousGroupsFasta: contains FASTA files of marker orthologous groups *RootHOGsFasta: contains FASTA file of sequences in each HOGs) and seven files 
*The main orthology results: FastOMA_HOGs.orthoxml, orthologs.tsv.gz, RootHOGs.tsv, OrthologousGroups.tsv   
*Synthetic reports : phylostratigraphy.html,  report.ipynb, report.html,  *The transformed input species tree: species_tree_checked.nwk).  

The ```hogmap``` folder includes the output of OMAmer (Described in the [OMAmer Module](https://omabrowser.org/oma/academy/module/OMAmer_2023)); each file corresponds to an input proteome.

**Orthologous groups**
The folder OrthologousGroupsFasta includes FASTA files, in which all proteins inside each FASTA file are orthologous to each other. These could be used as gene markers for species tree inference (Module 3).

///Q
1.  How many Orthologous Groups are there? 

///H
You can count the number of FASTA files in the folder OrthologousGroupsFasta.

///R
6,773. Note: The number could slightly change in different runs. 

///Q
2.  How many genes in total are present in all Orthologous Groups? 

///H
Genes are coded in the OrthoXML file as <geneRef id="1002001760"/\>. We need to count the number of lines including  "geneRef": 
``` grep geneRef output_hog.orthoxml  | wc -l```

///R
There are 22,606 genes in the groups.  

///T
Orthologous Groups which have a representative gene in every species could be considered as the “core genome” of the species of interest - genes that are conserved in all of the species. 

///Q
3. How many Orthologous Groups include one representative gene for each species? 

///H
Count how many groups in ```OrthologousGroups.tsv``` have five genes. You can count how many times each group appears using this command: ```cat OrthologousGroups.tsv | cut -f 1 |  sort | uniq -c |  awk '{print $1}' | grep 5 | wc -l ```.  

///R
 There are 1,618 Orthologous Groups having five genes. 

///T
Genes in orthologous groups could be used for tasks such as resolving a species tree. One possible use of FastOMA would be to generate an orthologous group with a mostly unresolved species then use those groups to make a better resolved one.

///Q
4. Which genes are orthologous to the gene A7EQW0_SCLS1? 

///H
Find the corresponding line in the ```OrthologousGroups.tsv``` using ```$ grep A7EQW0_SCLS1 OrthologousGroups.tsv ```. Then, use the ```grep``` command on the first column ```$ grep "OG_0003358" OrthologousGroups.tsv ```.

///R
 'tr|Q4WEI0|Q4WEI0_ASPFU', 'tr|A7EQW0|A7EQW0_SCLS1', 'sp|P32468|CDC12_YEAST', 'tr|Q6C7L3|Q6C7L3_YARLI', 'sp|P48009|SPN4_SCHPO


**Root HOGs**

The file ```RootHOG.tsv``` and the ```RootHOG``` folder contain information about the highest level of HOGs included in the OrthoXML file. Contrary to Orthologous Groups, Root HOGs represent families of genes that all descend from one common ancestor, in the ancestor of the species represented in the dataset. As such genes may have undergone duplication during their evolutionary histories, they may contain more than one gene per species, which differentiates them with Orthologous Groups.
RootHOGs may be used in order to resolve the evolutionary history of a certain gene family.

///Q
5. How many root HOGs are in the HOG output file?

///H
Each line in the output file ```RootHOGs.tsv``` denotes a gene. You can count how many times each root HOG appears using this command ``` cat RootHOGs.tsv | cut -f 1 |  sort | uniq -c  | wc -l```
 Note that the first line is the header. So the output value - 1 will be the answer.

///R
There are 6,793 root HOGs (gene families) in this file.

///Q
5. Consider the gene “60S ribosomal protein L15-A” in *Schizosaccharomyces pombe* with protein ID: RL15A_SCHPO. How many proteins are in the gene family (for these 5 species of interest)? What are their identifiers?

///H
Find the corresponding line in ```RootHOGs.tsv``` using ```$ grep RL15A_SCHPO RootHOGs.tsv ```. Then, use the ```grep``` command on the first column ```$ grep "HOG:0006324" RootHOGs.tsv ```

///R
There are 7 proteins in this family. RL15A_YEAST, Q4WJV5_ASPFU, sp|P54780|RL15B_YEAS, RL15B_SCHPO, Q6C7Y3_YARLI, A7EYU6_SCLS1, sp|O74895|RL15A_SCHPO

///Q 
6.In which species are there more than one protein? What does it mean?

///R
YEAST (<i>Saccharomyces cerevisiae</i>) and SCHPO (<i>Schizosaccharomyces pombe</i>) have more than one protein in the root HOG, which indicate there have been one or more duplications in this gene family.


///Q Find the FASTA file for this root HOG. It could be used as an input for more analysis, such as gene tree analysis.

///R 
FASTA files are in the folder ``` RootHOGsFasta```, which could be used to align them using [MAFFT](https://mafft.cbrc.jp/alignment/software/) and infer the gene trees ([FastTree](https://microbesonline.org/fasttree/) both are already installed in the environment). 


**Phylostratigraphy**	

By reconstructing HOGs, FastOMA also models the evolutionary histories of gene families: at which taxonomic level they are gained, lost and duplicated. The results of this are contained in the phylostratigraphy files. 

///Q
7. How many genes are duplicated at the level of <i>Saccharomyceta</i>?

///H
Download the ```phylostratigraphy.html``` file in the ```out_folder``` to your computer and open it in a browser.

///R
265

///Q
Which species appear to have the most unique proteins?

///H
Proteins unique to a species are gained on the terminal branch and reported at the leaf level.

///R
<i>Sclerotinia sclerotiorum</i>, with 5,363 genes gained.

**Report from FastOMA**

FastOMA produces a report in HTML format (```report.html```) indicating information about the input proteomes and about specificity from the output. This report can be also explored using the Jupyter Notebook  (```report.ipynb```).


///Q
8. Which species has the most proteins in its proteome? 

///H
Check the section "Stats on input dataset" in the ```report.html``` file. 

///R
<i>Sclerotinia sclerotiorum</i>

///T 
The report also contains basic statistics about HOGs in the dataset. 

///Q
What is the size of the HOG with the most members? What does it mean to have so many members?

///R One HOG has 30 members. This indicates it likely underwent many duplications.
This HOG contains proteins from the Hexose transporter family, which is highly duplicated in Yeast (https://pubmed.ncbi.nlm.nih.gov/20660490/).

///T
FastOMA computes a so-called completeness score, which indicates how many species below the defined taxonomic level are found in a givenHOG. A high completeness score indicates genes have been found in all species in the clade, which typically gives high confidence in the HOG.

///Q
9. What is the maximum value of the CompletenessScore? 

///H
Check the section "Roothogs (deepest levels for every HOG)" in the ```report.html``` file. 

///R
1



///Q
10. Are there many HOGs with a high completeness score?

///R
Yes, most HOGs have a completeness score of 1.

///T
Given FastOMA is installed in your local computer and you have a Jupyter Notebook installation. You can dynamically explore the contents of the HOGs in the FastOMA results using the [PyHAM library](https://github.com/DessimozLab/pyham) which is already installed in the FastOMA environment .




# FastOMA-academy




## Summary

* Create an account on GitPod https://gitpod.io
* Access the FastOMA's GitPod [https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy](https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy).
* Use the standard GitPod session and click on Continue
* Wait a few minutes (GitPod will install FastOMA automatically).
* Run the following command lines
```
cd /workspace/FastOMA-academy/Module_FastOMA/working_dir

nextflow -h


nextflow /workspace/FastOMA/FastOMA.nf --input_folder in_folder --output_folder out_folder \
 --omamer_db in_folder/omamerdb.h5
```



## Teachers

* [Sina Majidian](https://sinamajidian.github.io/)
* [Yannis Nevers](https://lab.dessimoz.org/people/yannis_nevers)
* [Stefano Pascarelli](https://scholar.google.com/citations?user=UU4EztcAAAAJ)



## Learning outcomes

This course is centered around comparative genomics. After the course, you will be able to:

* Define orthology and paralogy
* Map sequences quickly to their Hierarchical Orthologous Groups
* Infer orthologs on custom genomes using the FastOMA pipeline

## Course lectures

The pdf for the slides of the course will be available soon. 
(The material in this page is partly from the [SIB Biodiversity Bioinformatics 2023](https://github.com/DessimozLab/SIBBiodiversityBioinformatics2023/tree/main) by Natasha Glover and Christophe Dessimoz.

## The OMA Academy

To complete the course, you will follow the exercises of the OMA Academy on https://omabrowser.org/oma/academy/module/fastOMA_2023 for this course. OMA Academy is an e-learning website with a suite of self-learning modules centered around comparative genomics and phylogenetics. The modules are specifically designed to help computational biologists use OMA, a method and database for inferring orthologs. Each module is stand-alone, with a basic introduction and a series of exercises with hints and answers.
This repository focuses on FastOMA module.



## Notes


### _UNIX_

Participants should have a fundamental knowledge of utilizing the command line on UNIX-based systems. To assess your UNIX skills, you can take a quiz available [here](https://docs.google.com/forms/d/e/1FAIpQLSd2BEWeOKLbIRGBT_aDEGPce1FOaVYBbhBiaqcaHoBKNB27MQ/viewform?usp=sf_link). If you lack experience with the UNIX command line or are uncertain about meeting the prerequisites, we recommend completing the SIB [online UNIX tutorial](https://edu.sib.swiss/pluginfile.php/2878/mod_resource/content/4/couselab-html/content.html). 


### _Software_

We will be mainly working on an [GitPod](https://gitpod.io/), an online integrated development environment (IDE) that allows users to write, edit, and run code directly in a web browser. GitPod is cloud-based, meaning that all software, code, and files needed for the course are stored and processed on remote servers; you will not need to install or configure anything locally.
 
You can access the FastOMA's GitPod [here](https://gitpod.io/#https://github.com/sinamajidian/FastOMA-academy).


Participants need to sign up for a GitPod account via Github and/or LinkedIn to access 50 hours per month for free, which is ample time to complete the exercises. After logging in, create a new workspace by choosing FastOMA-academy, Browser Editor, and Large configuration (8 cores, 16 GB RAM, 50 GB storage). 

Notes: 
GitPod might ask you for permission when it comes to pasting in GitPod terminal, You can click on Allow on the top left corner in Google Chrome. The Safari browser is not recommended. 
You can click on OK if you see a box about changes in the git repository.

If you mistakenly close the browser window, you can go to the GitPod Dashboard and enter your workspace again.
As each user has limited CPU hours, please make sure that you stop the workspace, once you finish the analysis.   



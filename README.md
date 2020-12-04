# Basic of Microbiome Analysis at KU-FOOD (BAC-FOOD)

## Introduction

BAC-FOOD is a respository that contains a general workflow of microbiome analysis (it does not include handling of DNA/RNA sequences, but rather what to do with an abundance table.
We think that with this repository we can provide the minimun/basic set of tools needed to start and explore microbiome datasets within BSc and MSc projects at KU-FOOD and other departments, where computational microbiology is not the main focus. The key features of BAC-FOOD are:

    Summarizing Microbiome Data (e.g. zOTU level and/or species level, rarefying for sequencing depth).
    
    Alpha-Diversity analysis (Observed Species, Shannon and Simpson indexes, ANOVA, Kruskal-Wallis, etc.). 
    
    Beta-diversity analysis (Weighted and unweighted distances, and analyses based on ANOSIM, ADONIS, db-RDA [distance-based Redundancy Anaysis])
    
    Variable selection (Multiple t-test, multiple Kruskal-Wallis and univariate correlation (Pearson, Spearman's rank)
    
There are a bunch of multivariate tools (a.k.a Machine Learning approaches) to carry out feature selection (e.g. PLS-DA, RandomForest, Support Vector Machine, etc.) that we often use in complex studies/datasets, but this is not the main focus of this repository. If you want to dig in into these approaches I highly recommend you to start with Exploratory Data Analysis | Chemometrics (KU-NFOB16000U).

By nature every dataset is different and therefore, I cannot come up with an "universal/standard" analysis to all datasets. Instead, with this workflow I hope you will get a glance and ideas of the different analysis (and combination of them) that you could apply to your own data in order to maxize the interpretation and the biological meaning of it.

You should know that there are many open-source toolboxes specialized for these kind of analyses (e.g., QIIME2 (written in python3|bash), PhyloSeq (written in R), metagenomeSeq (written in R), etc.). However, the reason why BAC does not include them, it is because our aim is to show students how to manipulate dataframes and vectors directly with R-base. 


## Example Dataset

In this example, a zOTU table, metadata (e.g. treatments and samples info) and taxonomy are provided.

You can open the files with Excel if you want, up to you, but please do not modify anything :-)
The data was collected from a mice study (MSc project – unpublised data), where rodents were subjected to either XXX or YYY, and fecal samples included in this example were collected at day 21, the end of the animal experiment.

## Analyses

### BETA DIVERSITY

    library(vegan)
    library(ggplot2)
    library(viridis)
    library(RColorBrewer)   # display.brewer.all() #will display RColorBrewer options
    library(GUniFrac)
    
    #%%%% Importing-Data
    import.otu.table = read.table('zOTU_table_sintax.txt', header = T, row.names = 1)
    import.taxonomy = read.table('sintax_taxaEt.txt', header = T)
    cure.data = read.table('mapping_IDA_cured.txt', header = T) 
    
    #%%%% Formnating taxonomy
    IDkey = as.numeric(import.taxonomy$taxonomy)
    import.taxonomy = cbind(import.taxonomy,IDkey)
    taxonomy.key = unique(import.taxonomy[,-1])

    #%%%% Summarizing at zOTU or Species level
    XL8 = t(import.otu.table)  #%%% L8 level

    OTUID = rownames(import.otu.table)
    tmp1 = cbind(OTUID,import.otu.table)
    tmp2 = merge(import.taxonomy, tmp1, all=FALSE)
    tmp3 = tmp2[,-c(1:2)]
    tmp4 = aggregate(tmp3, by=list(tmp3$IDkey), FUN=sum, na.rm=TRUE)
    tmp5 = tmp4[,-c(1:2)]
    IDkey = tmp4$Group.1
    tmp6 = cbind(IDkey,tmp5)
    tmp7 = merge(taxonomy.key , tmp6, all=FALSE)
    row.names(tmp7) <- tmp7$taxonomy
    tmp8 = tmp7[,-c(1:2)]
    XL7 = t(tmp8)             #%%%% L7 level


    #%%% Selecting samples and/or organizing samples according to your cure.data object | rows (samples) need to be placed in the same order
    samples = as.vector(cure.data$SampleID)
    XL8s = XL8[samples,]
    XL7s = XL7[samples,]

# Supporting links:
https://mb3is.megx.net/gustame

https://academic.oup.com/femsec/article/90/3/543/536864








# Maintenance

   Josue L. Castro-M. (Postdoc)            – jcame@food.ku.dk 
   
   Lukasz Krych (Associate Professor)      – krych@food.ku.dk 

    










## Welcome

This is the pipeline for the handson session on Data Overview, PCA and f-statisitics for the IBE phD course

### 1. DATA OVERVIEW

####   1.a. Missingness

Before performing any analysis, we would need to assess the coverage of our ancient samples. For this Hands-on we will be working with already genotyped data, for an 'array' of SNPs (Human Origins Array (HO), Axiom). The HO dataset has  around ~600k positions (although this dataset has been prunned`).

A simple and very easy way of assessing how good a sample has worked is to see how many of these positions have we recovered with plink --missing flag. The directory contains the reuslts for this step for a subset of only the ancient samples of our dataset.



```{bash}

cd 1a-Missingness_assesment/
plink --bfile v44.3_HO_public_reduced --missing --out v44.3_HO_public_reduced

```

How is the SNP missingness of present-day and ancient samples? (Tip: use 'grep' command to check important populations)


```{bash}

cd ..

```


####  1.b. Post-Mortem damage signal

Ancient samples have a characteresitc Post-Mortem signal caused by base degradation. Most importantly, deamination damage ( C -> T ). In paleogenomics labs, we check this damage due to 2 main reasons. Reason number 1 is tho check if the sample is worth analysing or if it's too degraded. If this is the case we could treat this damage as fasle mutations leading to misleading results. However, this can be helpful as we can assess if a sample is ancient, or if it's modern contamination. We count with several programs and approaches to check the damage pattern of ancient samples, one of the most simple and commonly used is MapDamage (https://ginolhac.github.io/mapDamage/). This is a template of how can we easly can execute it:


```{bash}

cd 1b-MapDamage/
mapDamage -i $pathtobamfile/mybam.bam -r $path_to_Reference_used_for_the_bam -d $path_to_output_directory

```

We have a selection of 5 outputs from MapDamage in the directory. Which samples are reliably ancient?


```

cd ..

```


### 2. Principal Component Analysis

#### 2.a Exploratory PCA (PCA_1)

A Principal Component Analysis (PCA) is a tool used in expolratory data analysis which permits dimensionality reduction, in other words, simplyfy a complex matrix of data into easily interpretable components. In a broad manner, it does it by correlating variability of the dataset into Principal Components that explain a certain amount of such variability. This method is severely affected by the type and the nature of the data provided. Let's explore our dataset using a PCA using 'smaprtpca' package from the EIGENSOFT program (https://github.com/chrchang/eigensoft). This program is designed specifically to work with ancient DNA data. The way it works is by providing a parameter file for the program with the desired options of computation. Let's take a look at the parameter file:

```

cd 2-PCA/PCA_1/
less pca_parameters

```


Now that we understand what we are asking the program to do, let's sen the PCA. The results have been provided in the ./results/ directory. We have a simple R script for plotting the PCA. This program, the coordinate file is the '.evec' file. Use R to plot the results.


```

smartpca -p ./pca_parameters

```

What can we conclude from this PCA? How helpful is it for explorig our data?


```

cd ..

```

####  2.b West-Eurasian PCA (PCA_2)

We are going to put a little bit more context in our PCA now. Smartpca allows to compute the PCs with only a subset of samples and inefer a projected position for the rest. By using a subset of West-Eurasian populations we could create a very recognisable structure in the PCA. Again we will run a PCA with an extra parameter in our parameter file to acheve this.

```

cd PCA_2/
less pca_parameters

smartpca -p ./pca_parameters

```

The results have been provided in the ./results/ directory. We have a simple R script for plotting the PCA. This program, the coordinate file is the '.evec' file. Use R to plot the results.


What can we conclude from this PCA? Does the structure help us to infer the ancestry of the ancient samples better?




### 3. f-Statisitcs

In a broad way, f-statisitcis are a set of statisitcal tests which are used in population genomics to measure the genetic similarity/dissimilarity in a population. There are several types of tests, most importantly F3 and F4, which are simple statistics that test for admixture. F3 requires just 3 populations, and is most useful for recent admixture at approximately equal proportions. F4 is suitable to more ancient admixture, but more sensitive. For this Hands-on we will apply F4 by means of qpAdm program of AdmixTools (https://github.com/DReichLab/AdmixTools). In the qpAdm scheme, you choose m “right” populations (or outgroups) and n “left” populations (target and references for qpAdm). Taking the first population on each side as the point of comparison, you build a (m−1)×(n−1) matrix of f4 statistics. With qpAdm we want to measure the how much proportion of admixture is present in the 'target' populations coming from the 'reference' populations.

We are going to measure the proportions of the main three ancestral components of West-Eurasian ancestry (WHG, Early farmers and Step pastoralists) from 3 different present-day populations.


```
cd 3-f-statisitics/Basque
less outgroups
less left_pop
less par_file
qpAdm -p /gpfs42/robbyfs/scratch/lab_clalueza/pcarrion/2022.07-Practical_Classes_for_IBE-phD_Course/4-f-statistics/Basque/par_file

cd ../Icelandic
less outgroups
less left_pop
less par_file
qpAdm -p /gpfs42/robbyfs/scratch/lab_clalueza/pcarrion/2022.07-Practical_Classes_for_IBE-phD_Course/4-f-statistics/Basque/par_file

cd ../Sards
less outgroups
less left_pop
less par_file
qpAdm -p /gpfs42/robbyfs/scratch/lab_clalueza/pcarrion/2022.07-Practical_Classes_for_IBE-phD_Course/4-f-statistics/Basque/par_file

```

We have the results already computed. Lets take a look at the results from each qpadm.log file in each directory.


How are the ancestral profile of each of the samples? are they admixed?



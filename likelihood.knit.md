---
# IMPORTANT: Change settings here, but DO NOT change the spacing. 
# Remove comments and add values where applicable. 
# The descriptions below should be self-explanatory

title: "Theory of Statistics \n Likelihood Assigment"
#subtitle: "This will appear as Right Header"

documentclass: "elsarticle"

# Comment: ----- Follow this pattern for up to 5 authors
Author1: "Sean Soutar STRSEA001"  # First Author
Ref1: "UCT Statistics Honours, Cape Town, South Africa" # First Author's Affiliation
Email1: "sean.soutar\\@gmail.com" # First Author's Email address

Author2: "Fabio Fehr FHRFAB001"
Ref2: "UCT Statistics Honours, Cape Town, South Africa"
Email2: "FHRFAB001\\@myuct.ac.za"
CommonAffiliation_12: FALSE # If Author 1 and 2 have a common affiliation. Works with _13, _23, etc.

#Author3: "John Doe"
#Email3: "JohnSmith\\@gmail.com"

#CorrespAuthor_1: TRUE  # If corresponding author is author 3, e.g., use CorrespAuthor_3: TRUE

keywords: "Likelihood \\sep Overdispersion \\sep Soek" # Use \\sep to separate
JELCodes: ""

# Comment: ----- Manage headers and footers:
#BottomLFooter: $Title$
#BottomCFooter:
#TopLHeader: \leftmark # Adds section name at topleft. Remove comment to add it.
BottomRFooter: "\\footnotesize Page \\thepage\\" # Add a '#' before this line to remove footer.
addtoprule: TRUE
addfootrule: TRUE               # Use if footers added. Add '#' to remove line.

# Setting page margins:
margin: 2.3 # Sides
bottom: 2 # bottom
top: 2.5 # Top

HardSet: TRUE # Hard-set the spacing of words in your document. This will stop LaTeX squashong text to fit on pages, e.g. This is done by hard-setting the spacing dimensions. Set to FALSE if you want LaTeX to optimize this for your paper. 
bibliography: Tex/ref.bib       # Do not edit: Keep this naming convention and location.
RemovePreprintSubmittedTo: TRUE  # Removes the 'preprint submitted to...' at bottom of titlepage
Journal: "Journal of Finance"   # Journal that the paper will be submitting to, if RemovePreprintSubmittedTo is set to TRUE.
toc: no                         # Add a table of contents
numbersections: yes             # Should sections (and thus figures and tables) be numbered?
fontsize: 11pt                  # Set fontsize
linestretch: 1.2                # Set distance between lines.
link-citations: TRUE            # This creates dynamic links to the papers in reference list.
output:
  pdf_document:
    keep_tex: TRUE
    template: Tex/TexDefault.txt
    fig_width: 3.5 # Adjust default figure sizes. This can also be done in the chunks of the text.
    fig_height: 3.5
    include:
      in_header: Tex/packages.txt # Reference file with extra packages
abstract: |
  This project will explore the Accidents dataset and try fit a Poisson, Negative Binomial, Mixture of 2 Poissons and zero inflated Poisson models to the data. The model with the strongest support will be chosen and discussed. Profile likelihoods and confidence intervals for the parameters will be found and displayed of the chosen model.
---

<!-- First: Set your default preferences for chunk options: -->

<!-- If you want a chunk's code to be printed, set echo = TRUE. message = FALSE stops R printing ugly package loading details in your final paper too. I also suggest setting warning = FALSE and checking for warnings in R, else you might find ugly warnings in your paper. -->



#Introduction

This assignment is an explorative report on a dataset containing accident counts. The aim of the report is to find and fit a model which accurately describes the accident dataset. This report will first explore the data then fit different adequate distributions and choose the most appropriate one. Once a model has been selected the profile likelihood and confidence intervals will be programmed and calculated from from first principles. The results will then be analysed critically and conclusions will be made and consider further considerations in the study.

##Exploratory data analysis

To better understand our data this report shall explore the following properties; Firstly we examine the type of data within the accidents dataset and discuss whether our data is discrete ordinal or continuous. After the symmetry of the data and bounds will be discussed. This leads the exploration to outliers and extreme values.

![](likelihood_files/figure-latex/unnamed-chunk-1-1.pdf)<!-- --> 

### Data type
There are many instances where zero accidents were observed. This accounts for approximately 25.18% of the data. This suggests that the zero-inflated Poisson should be considered as this proportion is much higher than what would be expected of a regular Poisson distribution. The accident counts are discrete random variables. Specifically, they are discrete positive definite random variables on the interval $R \in \{0;+ \infty\}$. 
Summary statistics of the data are shown below.


     Mean   Variance   Median
---------  ---------  -------
 6.917892   85.08584        4

In the Poisson distribution, the mean should equal the variance. The sample variance far exceeds the sample mean. This indicates overdispersion if the Poisson distribution were to be used. This is when the observations are more variable than what would be expected. This suggests that alternative count models and mixture distributions should be used. 


### Symmetry
This property is visually seen in the histogram and boxplot. All counts are greater than zero with a median value of 4 accidents. The largest accident observed is 70 accidents. THe histogram shows that the data are non-symetrical and positively skewed which is usually expected of count data.

### Outliers
From the boxplot it clear that many outliers exist. One common method of classifying a point as an extreme value or outlier is if it falls more than 1.5 times the inner-quartile range above the upper quartile. The proportion of outliers within our data set amount to 15.26%.




#Methods 

##Model Formulation

THe data is discrete, asymmetric, positive definite, contains many positive outliers and many zeros. This would suggest distributions such as Poisson, Negative Binomial, mixture distribution of 2 Poissons and a zero inflated Poisson. 

###Poisson 

\begin{align*} 
f(x) & = e^{-\lambda} \frac{\lambda^x}{x!},\ \ x\in \{0,1,\ldots,\infty\},\lambda>0 \\
\\
L(\lambda|x) & = \prod_{i=1}^n f(x_i) \\
\\
L(\lambda|x) & =e^{-n\lambda}\dfrac{\lambda^{\sum_{i=1}^n x_i}}{\prod_{i=1}^n x_i!}\\
\\
l(\lambda|x) & =-n\lambda +  \left(\sum_{i=1}^n x_i\right)\ln \lambda + \ln(\prod_{i=1}^n x_i!).
\end{align*}

-define all parameters
-fit to the data

###Negative Binomial


\begin{align*}
f(x) & = \left( \begin{array}{c}
x+j-1  \\
x  \end{array} \right)(1-\pi)^x\pi^j,\ \ x,\in \{0,1,\ldots,\infty\}, 0 \leq \pi \leq 1\\
\\
L(\lambda|x) & = \prod_{i=1}^n f(x_i) \\
\\
L(\lambda|x) & = \prod_{i=1}^n \left( \begin{array}{c}
x_i+j-1  \\
x_i  \end{array} \right)(1-\pi)^{\sum_{i=1}^n x_i}\pi^{nj}\\
\\
l(\lambda|x) & = \sum_{i=1}^n \ln \left( \begin{array}{c}
x_i+j-1  \\
x_i  \end{array} \right)+{\sum_{i=1}^n x_i} \ln (1-\pi) + {nj} \ln(\pi)
\end{align*}

-define all parameters
-fit to the data

###Mixture of 2 poissons

-Likelihood 
-define all parameters
-loglikelihood

-fit to the data

-Here I am assuming the mixture will be poisson with rate = sample mean and the other poisson will have a rate of 0.1 to take into account the zero inflation ? 

###Zero inflated Poisson

-Likelihood 
-loglikelihood
-define all parameters
-fit to the data
-We can use optimisers but we must program the likelihoods ourselves

##Model Selection

-compare models and choose the best one
-Illustrate how good the model is 

-We need to reparameterize parameters so that they are unbounded

##Profile Likelihood & Confidence Intervals

-Plot likelihood surface (two parameters at a time if necessary, fixing the
other parameters at their MLEs).

-Must be program the profile likelihoods, CI's ourselves

#Results 

#Conclusion

-What are the next steps and how can we improve the models

<!-- Make title of bibliography here: -->
<!-- \newpage -->
# References  
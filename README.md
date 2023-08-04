# ML-Methods-to-estimate-mutivalued-TE
This repo contains all replication files for my M.Sc thesis on "Machine Learning Methods to estimate treatment effects with multivalued treatment". 

# List of Files

1. ## RegBias.R
   -This R file runs simulations to illustrate the problem of regularization bias. The resulting plot can be found in chapter 3.2 of the thesis. Details on the DGP of the simulation can be found in the appendix of the thesis.
   - Requires R packages "glmnet" , "tidyverse" , "reshape2" and "ggplot2"
3. ## Main Analysis
   - Population_sim.R
        - This R file computes the true potential outcome means on the population level
   - DGP_sim.R
        - This R file programs the data generating process. It then defines one easily callable function that allows to draw data from this DGP.

   - MonteCarlo.R
        - This R file performs the MC simulation design and saves the results

# Replication 
To replicate my results, simply download the zip folder and run the “MonteCarlo.R” file. This script automatically sources the “DGP_sim.R” and the “Population_sim.R” files. To succesfuly run the program you need to 
- have installed the packages "xgboost" , "gbm", "glmnet" , "nnet" ,"trust", "Rcpp" , "MASS" and "mvtnorm"
- Set the working directory in line 13 of the "MonteCarlo.R" file. The working directory has to be set to the place where the downloaded zip folder is stored

# Details
While I use the DGP from Xu & Tan (2022) ,  I hardcoded the data generating process and did not use the “simu.data” function in the authors “mRCAL” R package. The reason is that the source code of the function sets a seed, so that it draws the exact same sample in each iteration of my monte carlo simulation. Because it is difficult to solve mathematically for the true potential outcome means in the population, the “Population_sim.R” draws a huge sample of size 10^6, computes potential outcome means and interprets them as the underlying population values. The “DGP_sim.R” file hardcodes the DGP and implements it in one callable function called “simu.dat” which takes the argument n (sample size) and returns a list object called “simu.data”. This list contains the covariate matrix, the treatment assignment matrix, the outcome vector,the 4 true potential outcome means, the true potential outcome means conditional on the treatment group, the true coefficients in the propensity score model and the true coefficients in the outcome regression model.
The “MonteCarlo.R” file ultimately produces the main results. The script is structured into three parts:
1. ## Start
   - Sources the “Population_sim.R” and “DGP_sim.R” and loads packages 
2. ## Data Simulation
   -Monte Carlo Simulation
   - Runs a loop of k=500 iterations
3. ## Store Results
  - Saves results
  - (1) dataframes with point estimates, bias and variance for all estimators ("res_estimates", "res_bias", "res_variance", "res_absbias")
  - (2) "summary" dataframe which averages absolut bias and variance for each estimator over all four potential outcome means
  - the results for each potential outcome mean (1) and averages over all potential outcome means (2) are displayed in tables in the text document of the thesis

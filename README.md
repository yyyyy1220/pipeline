
# Getting started

1. Download LD data
	1KGref_plinkfile
	LD_1kg
	LDpred2_lassosum2_corr_1kg

2. Download MAF data

3. Download plink from 

4. Launch R and install required libraries

    install.packages(c('optparse','bigreadr','readr','stringr', 'caret', 'SuperLearner', 'glmnet', 'MASS', 'Rcpp', 'RcppArmadillo', 'inline', 'doMC', ‘foreach'))


# Use PennPrs

These are two scripts, single_ancestry and multi_ancestry.

## Single Ancestry

    Rscript <path_to_single-ancestry.R> \
      --userID <userID> \
      --submissionID <submissionID> \
      --method <method> \
      --trait <trait> \
      --race <race> \
      --LDrefpanel <LDrefpanel> \
      --k <k> \
      --partitions <partitions> \
      --ndelta <ndelta> \
      --nlambda <nlambda> \
      --lambda.min.ratio <lambda.min.ratio> \
      --alpha <alpha> \
      --p_seq <p_seq> \
      --sparse <sparse> \
      --kb <kb> \
      --Pvalthr <Pvalthr> \
      --R2 <R2> \
      --ensemble <ensemble> \
      --verbose <verbose> \
      --temp_path <temp_path>

  **Parameters:**
--userID: User account ID. This is a unique identifier for the user running the job.<br>
--submissionID: Job ID. This is a unique identifier for the job submission.<br>
--methods: Specifies the methods to be used for analysis. Multiple methods can be provided as a comma-separated list.<br>
--trait: The name of the trait being analyzed.<br>
--race: Specifies the race of the individuals in the training GWAS. Available options are EUR (European), AFR (African), AMR (Mixed American, Hispanic/Latino), EAS (East Asian), or SAS (South Asian).<br>
--LDrefpanel: Specifies the linkage disequilibrium (LD) reference panel to be used. Options include '1kg' (1000 Genomes Project Phase 3) or 'ukbb' (UK Biobank).<br>
--k: The number of folds for k-fold Monte Carlo Cross Validation (MCCV) used in PUMAS. Must be an integer greater than or equal to 2.<br>
--partitions: Specifies the partitioning of the data for subsampling in PUMAS. The format should be '% training, % testing', where % testing equals 1 - % training.<br>
--delta: Candidate values for the shrinkage parameter in L2 regularization. Multiple values can be provided as a comma-separated list.<br>
--nlambda: The number of different candidate values for the lambda parameter (shrinkage parameter in L1 regularization).<br>
--lambda.min.ratio: Ratio between the lowest and highest candidate values of lambda.<br>
--alpha: A scaling factor for heritability, where H_2 = alpha * H_20. The default values recommended for the LDpred2 algorithm can be used.<br>
--p_seq: A sequence of candidate values for the proportion of causal SNPs. Values should be provided as a comma-separated list.<br>
--sparse: Whether to consider a sparse effect size distribution, where the majority of SNPs have effects shrunk to zero.<br>
--kb: SNPs within this range of the index SNP are considered for the Clumping (C) step in the analysis.
--Pvalthr: Specifies the p-value thresholds for the Thresholding (T) step. Multiple values can be provided as a comma-separated list.
--R2: SNPs with squared correlation higher than the specified R2 value with the index SNPs will be removed.
--ensemble: Whether to train a weighted combination of the single PRS models.
--verbose: Controls the verbosity of the output. Set to 0 for no output or 1 to print a log file.
--temp_path: Specifies the path to the GWAS data after quality control.



## Multi Ancestry

    Rscript /home/ubuntu/pennprs/backend/code/multi_ancestry.R \
      --userID <userID> \
      --submissionID <submissionID> \
      --method <method> \
      --trait <trait> \
      --race <race> \
      --LDrefpanel <LDrefpanel> \
      --k <k> \
      --partitions <partitions> \
      --ndelta <ndelta> \
      --nlambda <nlambda> \
      --lambda.min.ratio <lambda.min.ratio> \
      --Ll <Ll> \
      --Lc <Lc> \
      --verbose <verbose> \
      --temp_path <temp_path>

**Parameters:**
--userID: User account ID. This is a unique identifier for the user running the job.
--submissionID: Job ID. This is a unique identifier for the job submission.
--methods: Specifies the methods to be used for analysis. The default method is 'PROSPER'.
--trait: The name of the trait being analyzed.
--races: Specifies the races of the individuals in the training GWAS data. At least two races must be specified, separated by commas. Available options are EUR (European), AFR (African), AMR (Mixed American, Hispanic/Latino), EAS (East Asian), or SAS (South Asian).
--LDrefpanel: Specifies the linkage disequilibrium (LD) reference panel to be used. Options include '1kg' (1000 Genomes Project Phase 3) or 'ukbb' (UK Biobank).
--k: The number of folds for k-fold Monte Carlo Cross Validation (MCCV) used in PUMAS. Must be an integer greater than or equal to 2.
--partitions: Specifies the partitioning of the data for subsampling in PUMAS. The format should be '% training, % testing', where % testing equals 1 - % training.
--ndelta: The number of candidate values for the shrinkage parameter in L2 regularization. Must be a positive integer.
--nlambda: The number of different candidate values for the lambda parameter (shrinkage parameter in L1 regularization). Must be a positive integer.
--lambda.min.ratio: Ratio between the lowest and highest candidate values of lambda.
--Ll: Specifies the length of the path for the tuning parameter lambda in the PROSPER step.
--Lc: Specifies the length of the path for the tuning parameter c in the PROSPER step.
--verbose: Controls the verbosity of the output. Set to 0 for no output or 1 to print a log file.
--temp_path: Specifies the path to the GWAS data after quality control.




**Data requirements：**

| CHR | A1 | A2 | SNP        | MAF       | BETA                  | SE        | P       | N     |
|-----|----|----|------------|-----------|-----------------------|-----------|---------|-------|
| 1   | G  | A  | rs12562034 | 0.085736  | 0.012104              | 0.051204  | 0.8131  | 16162 |
| 1   | C  | A  | rs4970383  | 0.367982  | 0.0032909             | 0.027646  | 0.9052  | 17079 |
| 1   | C  | T  | rs4475691  | 0.402124  | -0.0021203000000000003| 0.026989  | 0.9374  | 17079 |


**Required columns in the files:**

SNP: SNP ID in the format of rsXXXX.
CHR: chromosome number in the format of 1,2,...,22.
BETA: SNP effect. Note that for binary traits, beta is the coefficient in logistic regression, i.e. log(OR).
SE: Standard error of beta.
A1: effective allele (counted allele in regression).
A2: alternative allele (non-A1 allele).
MAF: Minor Allele Frequency
P: P value
N: Sample size per variant. Note that for binary traits, it is effective sample sizes = 4 / (1 / N_control + 1 / N_case); and for continuous traits, it is simply the sample size.



# Output

The scripts single_ancestry.R will create directory 
$PATH_out/trait_race_method_userID_submissionID/ and writes trait.method.PRS.txt and PRS_model_training_info.txt.

The scripts multi_ancestry.R will create directory 
$PATH_out/trait_race_method_userID_submissionID/ and writes trait.method.PRS.txt and PRS_model_training_info.txt.

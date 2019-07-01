This repository provides functionality to compute the binding residue similarity of sequences for specific protein families (FunFams, EC) and to calculate consensus predictions for proteins within one FunFam. 

To calculate consensus predictions, binding residue predictions have to be calculate for all sequences within one FunFam. Residues that are predicted as binding for at least x% of the sequences are then considered as binding according to the consensus prediction with x being a parameter the user can choose. 

As an example, BindPredict-CC and BindPredict-CCS that predict binding residues from evolutionary couplings using clustering coefficients or cumulative coupling scores are provided. However, binding residues can be predicted using any other method available.

# Usage
## Data
FunFams [1] alignments can be obtained from the [CATH webserver](ftp://orengoftp.biochem.ucl.ac.uk/cath/releases/latest-release/sequence-data/)

## Prediction of binding residues
### Calculation of evolutionary couplings (ECs) using external software

- **EVcouplings** results (\*.di_scores, \*_CouplingScoresCompared_all.csv, \*_frequencies.csv, \*_alignment_statistics.csv): [EVcouplings](http://evfold.org/evfold-web/newmarkec.do) [2,3] is available as a Github repository. A detailed description on how to run EVcouplings can be found [here](https://github.com/debbiemarkslab/EVcouplings). EVcouplings only provides EC scores inferred by plmDCA. To calculate DI scores, one can use
  - **FreeContact** [4] which can be downloaded as a [debian package](https://packages.debian.org/search?keywords=freecontact). Using the alignment generated by EVcouplings, DI scores can be calculated using the option "evfold".
  - the **EVcouplings** [webserver](http://evfold.org/evfold-web/newmarkec.do). DI scores can be calculated by entering the UniProt identifier or the sequence in FASTA format and by choosing "DI" as coupling scoring.
  
### Calculation of cumulative coupling scores and clustering coefficients
TBD

## Compute binding residue similarity
`similarity.py` has the following command line parameters:
* `-families`  
    the path to a directory containing the FunFam dataset (with one sub-directory per superfamily).
* `-sites`  
    the path to a file with a mapping of UNIPROT IDs to binding site annotation (see /data).
* `-groupby` [funfam|ec]  
    the way in which the sequences should be grouped for similarity computations.
* `-limit` [funfam|ec]   optional  
    the groups which should not occur multiple times within the group specified by `groupby`.
* `-align`               optional  
    the path to a directory in which data for the generation of multiple sequence alignments can be stored.
* `-clustalw`            optional  
    the command to call the external clustalw MSE tool, necessary only if `groupby` == ec.

## Build and evaluate consensus prediction
`prediction.py` has the following command line parameters:
* `-consensus`  
    the consensus cut-off at positions are classified as binding.
* `-cc`  
    the cut-off above which a position is classified as binding by its clustering coefficient.
* `-ccs`  
    the cut-off above which a position is classified as binding by its cumulative coupling score.
* `-uniprot_ids`  
    path to a file containing all UNIPROT IDs for which data is available, one id per line.
* `-mapping`  
    path to a file with a mapping of FunFams to UNIPROT IDs.
* `-evc_info`  
    path to a directory with output files from [EVcouplings](http://evfold.org/evfold-web/newmarkec.do) (\_final.outcfg, .alignment_statistics.csv), [FreeContact](https://rostlab.org/owiki/index.php/FreeContact) (.di) as well as [bindPredict](https://github.com/Rostlab/bindPredict) (.cum_scores, .cluster_coeff). Data for each UNIPROT ID ought to be in a seperate subdirectory.
* `-funfam_data`  
    path to a file in FASTA FunFam format including mapped binding sites for each entry.
* `-families`  
    the path to a directory containing the FunFam dataset (with one sub-directory per superfamily).
* `-out`  
    the path to a directory to which output files will be written.
    
 # References
[1] Sillitoe I, Cuff AL, Dessailly BH, Dawson NL, Furnham N, Lee D, Lees JG, Lewis TE, Studer RA, Rentzsch R: New functional families (FunFams) in CATH to improve the mapping of conserved functional sites to 3D structures. Nucleic Acids Research 2012, 41(D1):D490-D498.
[2] Marks, D. S., Colwell, L. J., Sheridan, R., Hopf, T. A., Pagnani, A., Zecchina, R., Sander, C. (2011). [Protein 3D Structure Computed from Evolutionary Sequence Variation](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0028766). PLoS ONE **6**(12): e28766.

[3] Hopf, T. A., Schärfe, C. P. I., Rodrigues, J. P. G. L. M., Green, A. G., Kohlbacher, O., Sander, C., Bonvin, A. M. J. J., Debora S Marks, D.S. (2014) [Sequence co-evolution gives 3D contacts and structures of protein complexes](https://elifesciences.org/articles/03430). eLife; 3:e03430

[4] Kaján L., Hopf T. A., Kalaš M., Marks D. S., Rost B. (2014) [FreeContact: fast and free software for protein contact prediction from residue co-evolution.](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-15-85). BMC Bioinformatics **15**:58

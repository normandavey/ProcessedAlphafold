# Simple Alphafold Processing

Some initial invesitgations that Bálint Mészáros and I have run on AlphaFold. This is preliminary data and should be treated very sceptically but hopefully it will save you all some time and effort in answering the questions that you want to answer.

*Overview*

My research focus is motif discovery. As most motifs fall within intrinisically disordered regions (IDRs), IDR prediction is one of the key tools we have to define our motif search space. The software that we use in our team is IUPred and Balint has just developed IUPred2 so I was very interested to get his feedback on IUPred2 (our current workhorse) and AlphaFold (AF - the challenger). As he has all the scripts from benchmarking IUPred2, and I have all the scores parsed from the AlphaFold structures, it was easy to give this a go. We were a bit shocked by the improvement AlphaFold seemed to show solving a problem that it wasn't even try to tackle. Balint described it best when he stated the folding problem and the disorder problem are just two sides of the same problem! For us it's clear and we're going to start integrating AlphaFold for disorder prediction in our motif predictions straightaway.

*Processing:*
- Download AlphaFold structures for human
- Pull pLDDT scores from the b factor position in the PDB files
- Run DSSP on the PDB files
- Pull raw surface accessibility scores from the DSSP output 
- Normalise the surface accessibility scores by the max accessible residue surface accessibility in a GGXGG peptide
- Calculate a windowed normalised surface accessibility score using a window of +/-10 residue

*Comparison:*

We benchmarked both the modified pLDDT score and the calculated accesibility score of AlphaFold on the testing sets used for evaluating IUPred2. Since AlphaFold predictions aren't available for all sequences (yet), we only considered proteins for which they are. The positive set contains 56k residues from 464 disordered proteins manually filtered and re-curated from DisProt. The negative set contains 433k residues from 2723 proteins taken from the PDB by searching for monomeric single domain proteins without fragments and other floppy bits (according to CATH and CYRANGE) - see details in the downloadable lists.

We calulcated full ROC curves for both AF scores and IUPred2 to have a basis of comparison. We played around with smoothing the AF scores with various window sizes in the range of 5-50 residues. Smoothing improves the predictive power, especially so for the accessibility scores. We found that the predictive power is rather insensitive to the exact window size, but 15 seems to be a good number. With this, the areas under the ROC curve are the following:
- IUPred2: AUC=0.870
- AF accessibility (window=15): AUC=0.933
- AF pLDDT (window=15): AUC=0.902

Full ROC curves (downloadable below in high resolution):

<img src="https://user-images.githubusercontent.com/14313974/126916867-fd5f2b91-0c0d-4413-a30c-4116f9b5b6d4.png" width="500" height="454"><img src="https://user-images.githubusercontent.com/14313974/126916884-4f41405c-e675-4750-87f0-3d4b4e294d9b.png" width="500" height="454">

Keep in mind that the above calculations are by no means rigorous benchmarkings and should be treated as an indication rather than proof. However, it is quite striking that AF is able to achieve such a high performance despite that fact that it was neither trained for this application nor did it encounter any of the disordered data in this test.

*How to use:*

While ROC curves are informative, in a real life setting we need to use a defined cutoff. Both of the AF scores we generated are in the [0:1] range and to achieve a standard 5% false positive rate, the following cutoffs should be used:
- AF accessibility (window=15): cutoff value=0.55, achieves 80.3% true positive rate
- AF pLDDT (window=15): cutoff value=0.33, achieves 72.6% true positive rate
- IUPred2 (for comparison): cutoff value=0.56, achieves 58.6% true positive rate


*Data:*
- JSON files of the raw and windowed normalised surface accessibility scores per residue, and the modified pLDDT scores for the full human proteome
- TDT file with the residue offset, residue, raw score, windowed score for the full human proteome
- summary statistics of the testing sets and the evaluation measures
- list of protein regions in the positive and negative testing sets
- ROC curves in decent resolution

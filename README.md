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

*Data:*
- JSON files of the raw and windowed normalised surface accessibility scores per residue
- TDT file with the residue offset, residue, raw score, windowed score

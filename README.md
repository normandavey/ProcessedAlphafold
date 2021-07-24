# Simple Alphafold Processing

Some initial invesitgations that Bálint Mészáros and I have run on AlphaFold. This is preliminary data and very should be treated sceptically but hopefully it will save you all some time and effort in answering the questions that you want to answer.

*Overview*
My research focus is motif discovery. As most motifs fall within intrinisically disordered regions (IDRs), IDR prediction is one of the key tools we have to define our motif search space. The software that we use in our team is IUPred and Balint has just developed IUPred2 so I was very intersted to get his feedback on IUPred2 (our current workhorse) and AlphaFold (the challenger). As he has all the scripts from benchmarking IUPred2, and I have all the scores parsed from the AlphaFolds structures, it was easy to give this a go. We were a bit shocked by the improvement AlphaFold seemed to show solving a problem that it wasn't even try to tackle. Balint described it best when he stated the folding problem and the disorder problem are just two sides of te same problem! For us it's clear and we're going to start integrating AlphaFold for disorder prediction in our motif predictions straightaway. 

*Processing:*
- Download AlphaFold structures for human
- Pull pLDDT scores from the b factor position in the PDB files
- Run DSSP on the PDB files
- Pull raw surface accessibility scores from the DSSP output 
- Normalise the surface accessibility scores by the max accessible residue surface accessibility in a GGXGG peptide
- Calculate a windowed normalised surface accessibility score using a window of +/-10 residue

*Comparison:*


*Data:*
- JSON files of the raw and windowed normalised surface accessibility scores per residue
- TDT file with the residue offset, residue, raw score, windowed score

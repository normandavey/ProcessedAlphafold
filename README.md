# Simple Alphafold Processing

*Processing:*
- Download AlphaFold structures for human
- Run DSSP on the structures
- Pull raw surface accessibility scores from the DSSP output 
- Normalise the surface accessibility scores by the max accessible residue surface accessibility in a GGXGG peptide
- Calculate a windowed normalised surface accessibility score using a window of +/-10 residue

*Comparison:*


*Data:*
- JSON files of the raw and windowed normalised surface accessibility scores per residue
- TDT file with the residue offset, residue, raw score, windowed score

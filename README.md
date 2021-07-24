# Simple Alphafold Processing

*Processing:*
- Download alphafold human
- Run DSSP on the structures
- Pull raw surface accessibility scores from the DSSP output 
- Normalise the surface accessibility scores by the max accessible residue surface accessibility in a GGXGG peptide
- Push data to JSON

*Data:*
- JSON files of the raw normalised surface accessibility scores per residue
- TDT file with the residue offset, residue, raw score, windowed score

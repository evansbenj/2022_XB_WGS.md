# Pairwise alignment with alphafold

First go to alphafold (https://alphafold.ebi.ac.uk/) and download the structure files for the proteins of interest (here XL wnt3.L and wnt4.L)
https://alphafold.ebi.ac.uk/search/text/wnt3.L?suggested=true
https://alphafold.ebi.ac.uk/search/text/wnt4.L

Then go to the RCSB page (https://www.rcsb.org/alignment) and select file upload in the downward pointing carrot next to each entry. Upload the files, enter "A" for chain ID, and click "Compare"

# For XB

This has to be done on rorqual

https://docs.alliancecan.ca/wiki/AlphaFold3

First ask AI to convert a fasta file to json format for alphafold3

After this, you need to convert to pdb format using this script

```
python -c "import Bio; print(Bio.__version__)"
pip install --user biopython
```
```python
python - <<'PY'
from Bio.PDB import MMCIFParser, PDBIO

cif_file = "/scratch/ben/alphafold/inference-output/XB_wnt3W_proteinz/XB_wnt3W_proteinz_model.cif"

parser = MMCIFParser(QUIET=True)
structure = parser.get_structure("model", cif_file)

io = PDBIO()
io.set_structure(structure)
io.save("XB_wnt3W_proteinz.pdb")

print("Wrote XB_wnt3W_proteinz.pdb")
PY
```

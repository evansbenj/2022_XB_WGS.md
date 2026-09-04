# CRISPRitz

This program allows one to scan a genome and evaluate how many matches there are to a guide. You need to know the CAS recognition seq and the guide seq.

# Load the virtual environment
```
module load apptainer 
wget https://staff.sharcnet.ca/jhqin/containers/Ticket%230323093-CRISPRitz/CRISPRitz-update.sif
apptainer run CRISPRitz-update.sif crispritz.py
```

# index the genome
apptainer run CRISPRitz-update.sif crispritz.py index-genome Xbo_CRISPRitz /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz/Austin_genome/individ_chrs/ /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz/Cad9PAM.txt -bMax 2

# search fasta file
apptainer run CRISPRitz-update.sif crispritz.py search ../CRISPRitz/Austin_genome/individ_chrs/ /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz/Cad9PAM.txt /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz_update/wnt3W_Guide.txt wnt3W_guidesearch_in_Austinconcat -mm 4 -t

# search indexed genome (allows for bulges)
apptainer run CRISPRitz-update.sif crispritz.py search /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz_update/genome_library/NGG_2_Xbo_CRISPRitz/ /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz/Cad9PAM.txt /home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/Austin_concat_genome/CRISPRitz_update/wnt3W_Guide.txt wnt3W_guidesearch_in_Austinconcat -mm 4 -bDNA 1 -bRNA 1 -t



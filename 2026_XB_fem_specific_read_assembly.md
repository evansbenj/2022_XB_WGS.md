# Make kmer db from XB female and male WGS data:

Directory
```
/home/ben/projects/rrg-ben/ben/2022_XB_WGS/raw_and_trimmed_data
```
Using meryl:
femalez:
```
#!/bin/sh
#SBATCH --job-name=makemeryldb
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=166:00:00
#SBATCH --mem=128gb
#SBATCH --output=makemeryldb.%J.out
#SBATCH --error=makemeryldb.%J.err
#SBATCH --account=rrg-ben

/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl count tmp*/*fem*_trim.R[1,2].fq.gz threads=4 memory=128 k=29 output XB_WGS_femz_meryldb.out
```
malez:
```
#!/bin/sh
#SBATCH --job-name=makemeryldb
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=166:00:00
#SBATCH --mem=128gb
#SBATCH --output=makemeryldb.%J.out
#SBATCH --error=makemeryldb.%J.err
#SBATCH --account=rrg-ben

/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl count tmp*/*_mal*_trim.R[1,2].fq.gz threads=4 memory=128 k=29 output XB_WGS_malz_meryldb.out
```

# Take difference of fem_kmerz - mal_kmerz:
```
#!/bin/sh
#SBATCH --job-name=meryl_intersect
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=2:00:00
#SBATCH --mem=128gb
#SBATCH --output=meryl_intersect.%J.out
#SBATCH --error=meryl_intersect.%J.err
#SBATCH --account=rrg-ben

# sbatch 2026_meryl_intersect.sh fq_meryl kmermeryl
/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl difference ${1} ${2} output in_${1}_but_not_${2}.meryl
```
# Extract reads with fem_specific kmerz
```
#!/bin/sh
#SBATCH --job-name=makemeryldb
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=2:00:00
#SBATCH --mem=128gb
#SBATCH --output=makemeryldb.%J.out
#SBATCH --error=makemeryldb.%J.err
#SBATCH --account=rrg-ben


/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl-lookup -include \
  -sequence ${1}__trim_R1.fq.gz ${1}__trim_R2.fq.gz \
  -mers allo_XXXW_target_kmers.meryl \
  -output ${1}_XXX_R1.fq.gz ${1}_XXX_R2.fq.gz
```

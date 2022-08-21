# Using samtools:
```
#!/bin/sh
#SBATCH --job-name=samtools
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=4:00:00
#SBATCH --mem=2gb
#SBATCH --output=samtools.%J.out
#SBATCH --error=samtools.%J.err
#SBATCH --account=def-ben

# to execute type this:
# sbatch 2022_samtools_coverage.sh bamfile region_ChrXX:XX-XX

module load StdEnv/2020  gcc/9.3.0 samtools/1.13
samtools depth -H ${1} -r ${2} > ${1}_${2}_depth
```

I am calculating depth using only coding regions.

I made a bed file for the Chr9_10L region (gh1.L,arf1.L,nsf.L,wnt3.L,wnt9b.2.L) but not including a long noncoding RNA (LOC121398333):
```
/home/ben/projects/rrg-ben/ben/2021_XL_v10_refgenome/Chr9_10L_hetregion.bed
```
using grep and cut:
```
grep 'CDS' XENLA_10.1_GCF.gff3 | grep 'XP_041432589.1' | cut -f1,4,5 >> Chr9_10L_hetregion.bed
```
I made a bed file for all CDS like this:
```
grep 'CDS' XENLA_10.1_GCF.gff3 >XL_v10.1_all_CDS.bed
```
it is here too:
```
/home/ben/projects/rrg-ben/ben/2021_XL_v10_refgenome/XL_v10.1_all_CDS.bed
```


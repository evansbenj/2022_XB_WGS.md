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

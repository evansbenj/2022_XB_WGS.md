# Association tests with angsd

I did this for each chromosome using the bam files as input
```
#!/bin/sh
#SBATCH --job-name=angsd
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=24:00:00
#SBATCH --mem=312gb
#SBATCH --output=angsd.%J.out
#SBATCH --error=angsd.%J.err
#SBATCH --account=def-ben

# http://popgen.dk/angsd/index.php/Input
# this describes how to specify regions
# here we need to add the chromosome after the sbatch command
module load StdEnv/2020 angsd/0.939

angsd -yBin bin_sex.ybin -doAsso 1 -GL 1 -out out_${1}_additive_F1 -doMajorMinor 1 -doMaf 1 -SNP_pval 1e-6 -minInd 4 -b
am bam.filelist -r ${1}: -P 5 -doCounts 1 -setMinDepthInd 2 -setMaxDepthInd 100 -Pvalue 1
```

Save only highly significant values for plotting, for example:
```
zcat out_Chr9_10S_additive_F1.lrt0.gz | awk '$7 > 0.001 { next } { print }'> 2023_XB_mappedtoXL_Chr9_10S_P_gt_0.001_only.txt
```

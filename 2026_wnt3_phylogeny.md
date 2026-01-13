# Path
```
/home/ben/projects/rrg-ben/ben/2022_XB_WGS/Iridian_genomez/
```

# Download WGS data from NCBI
```
module load StdEnv/2020 gcc/9.3.0 'sra-toolkit/3.0.0'
fasterq-dump SRR35972780 SRR35973704 SRR35964371 SRR31189741	
```
this is for muel, fisch, fras, and cliv respectively
# Trim and map data as usual

# Calculate depth of mapped reads

```
#!/bin/sh
#SBATCH --job-name=samtools_depth
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=12:00:00
#SBATCH --mem=8gb
#SBATCH --output=samtools_depth.%J.out
#SBATCH --error=samtools_depth.%J.err
#SBATCH --account=rrg-ben

module load StdEnv/2023  gcc/12.3 samtools/1.20
samtools coverage -q 20 --plot-depth ${1} -o ${1}_coverage.txt
```

# Call bases and extract seqs from each wnt3 exon
```
sbatch ../../2021_Austin_XB_genome/ben_scripts/2021_HaplotypeCaller_onebam_emit_ref_too_one_region.sh ../../2021_Austin_XB_genome/Austin_genome/Xbo.v1.fa SRR35972780_trim_fastp_sorted_rg.bam XB_Austin_wnt3.S_ex1.bed
```
# Combine vcfs
```
bcftools concat -o Xmuel_wnt3L_concat.vcf SRR35972780_trim_fastp_sorted_rg.bam_XB_Austin_wnt3.L_ex5.bed.g.vcf SRR35972780_trim_fastp_sorted_rg.bam_XB_Austin_wnt3.L_ex4.bed.g.vcf SRR35972780_trim_fastp_sorted_rg.bam_XB_Austin_wnt3.L_ex3.bed.g.vcf SRR35972780_trim_fastp_sorted_rg.bam_XB_Austin_wnt3.L_ex2.bed.g.vcf SRR35972780_trim_fastp_sorted_rg.bam_XB_Austin_wnt3.L_ex1.bed.g.vcf
```

# Convert to tab
```
module load vcftools
cat XXX.vcf | vcf-to-tab > XXX.tab
```

# Process file with excel; check for heterozygous positions


I made an alt ref seq for a particular region like this
```
samtools faidx ../Austin_concat_genome/Xbo.v1_chrs_and_concatscafs.fa ChrX:XXX-XXX | bcftools consensus /home/ben/projects/rrg-ben/ben/2022_XB_WGS/bamfiles/vcfs_aftergenotyping_before_filtering/XB_WGS_not_filtered_allchrs.vcf.gz --iupac-codes --sample BJE4429_toXBconcat.bam -o ChrX:XXX-XXX_altref.fa
```

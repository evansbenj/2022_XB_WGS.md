# Tad expression

Goal: I need to evaluate how and whether each W-linked exon/transcript is expressed in XB tad gonads. To accomplish this I am doing the following:
* for each XL exon, I blast the XB genome
* I pull out the XB sequencing using bedtools
* I blast the XB sequence from each exon against the XB transcriptome to get the name of homologous transcripts
* I can also blast the XB sequence from each exon against the XB genome to see if there are any divergent positions. If not then this is an autosomal transcript
* For XB transcripts that have a hit (and divergence - especially at W-linked SNPs), I can get the count data from male and female tads at stage 46 and 48
* I may want to download the transcripts and genomic regions and align them to pinpoint whether the transcripts have W-linked variation



I have the joint tad transcriptome assembly and counts on nibi here:
```
/home/ben/projects/rrg-ben/ben/2022_XB_WGS/XB_RNAseq_gonad_transcriptome
```

I also have the tad files on info here (after transferring to info115):
```
/home/evanslab/borealis_tadpole_transcriptome/
```
and here for adult liver:
```
/home/evanslab/borealis_adultFamily_RNAseq/
```
Here is the XB tad st 46 and 48 transcriptome assembly:
```
/home/evanslab/borealis_tadpole_transcriptome/data/transcriptome/borealis_tad_goand_transcriptome.fasta
```
Here is the XB adult liver transcriptome assembly:
```
/home/evanslab/borealis_adultFamily_RNAseq/borealis_liver_transcriptome/build_transcriptome/borealis_adult_liver_transcriptome_trinityout.fasta
```
Here is an assembly made from adult gonads (testis, ovary) and also adult liver:
```
/home/evanslab/borealis_adultFamily_RNAseq/borealis_both_tissues_transcriptome/data/transcriptome/build_transcriptome/borealis_transcriptome_trinityOut.fasta
```

Counts are here for tads:
```
/home/evanslab/borealis_tadpole_transcriptome/data/transcriptome/combine_de_count_transcript/borTad_borGenome_deseq2_tpm_combine_st46.csv
/home/evanslab/borealis_tadpole_transcriptome/data/transcriptome/combine_de_count_transcript/borTad_borGenome_deseq2_tpm_combine_st48.csv
```
Here are counts by individual for adult liver:
```
/home/evanslab/borealis_adultFamily_RNAseq/borealis_liver_transcriptome/combine_de_count_info/borAdultLiver_laevisV92Genome_edgeR_tpm_combine_individualTrans.csv
```

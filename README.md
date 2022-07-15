# READ ME

Now have WGS data from a total of 9 females and 11 males from west Kenya.  Added to this we have WGS data from 2 lab individuals (1 M, 1 F) and two east individuals (1 M 1 F).  The sex and sample IDs are below.
```
BJE4429_toXBconcat.bam	F	West
BJE4430_toXBconcat.bam	M	West
BJE4431_toXBconcat.bam	M	West
BJE4433_toXBconcat.bam	F	West
BJE4441_toXBconcat.bam	F	West
BJE4442_toXBconcat.bam	M	West
BJE4443_toXBconcat.bam	M	West
BJE4444_toXBconcat.bam	F	West
BJE4445_toXBconcat.bam	F	West
BJE4446_toXBconcat.bam	F	West
BJE4447_toXBconcat.bam	M	West
BJE4459_toXBconcat.bam	F	West
BJE4460_toXBconcat.bam	M	West
BJE4461_toXBconcat.bam	F	West
BJE4462_toXBconcat.bam	M	West
BJE4473_toXBconcat.bam	M	West
BJE4477_toXBconcat.bam	M	West
BJE4478_toXBconcat.bam	M	West
BJE4479_toXBconcat.bam	F	West
BJE4480_toXBconcat.bam	M	West
BJE4515_toXBconcat.bam	F	East
BJE4536_toXBconcat.bam	M	East
SRR6357672.1_trim_fixed_sorted.bam	M	Lab
SRR6357673_trim_sorted.bam	F	Lab
```

I aligned and genotyped these data in exactly the same way as I did for the JEvolBio paper. Because the libraries were PCR-free, I did not dedeup them.

This includes trimming with cutadap and trimmomatic, alignment with bwa and samtools, and genotyping with HaplotypeCaller, GenomicsDB, and GenotypeGVCFs of GATK, using the same versions as previously.


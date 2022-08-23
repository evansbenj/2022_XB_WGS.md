# Working directory
```
/home/ben/projects/rrg-ben/ben/2022_XB_WGS/bamfiles_mappedtoXL/bams
```

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

Working in this directory:
```
/home/ben/projects/rrg-ben/ben/2022_XB_WGS/bamfiles_mappedtoXL/bams
```

Here is a bash script for using bedfiles to get depth per site from each bam (mapped to XL):
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
# sbatch 2022_samtools_coverage.sh bamfile path_and_bed_filename.bed prefix

module load StdEnv/2020  gcc/9.3.0 samtools/1.13
samtools depth -H ${1} -b ${2} > ${1}_${3}_depth
```
it is located here:
```
/home/ben/projects/rrg-ben/ben/2021_Austin_XB_genome/ben_scripts/2022_samtools_coverage_bedfile.sh
```

# Ploting depth ratio for interesting bits
Below is a script that plots the depth ratio for coding regions only in Chr9_10L and also interesting het regions for each individual. The medians are calculated from positions that have a depth of less than or equal to 100 for all individuals (above this is clearly due to repeats). The ratio is calculated from the median of coding regions in the het region compared to coding regions on Chr9_10L, even if the het region does not map to Chr9_10L. I would have used all coding regions but it took too much memory and wouldn't matter anyway.

```R
setwd("./")
#install.packages("tidyverse", repos="https://mirror.rcg.sfu.ca/mirror/CRAN/")
#install.packages("ggplot2", repos="https://mirror.rcg.sfu.ca/mirror/CRAN/")
library(ggplot2,lib.loc="/home/ben/local/R_libs/")
library(dplyr,lib.loc="/home/ben/local/R_libs/")
#library(tidyverse,lib.loc="/home/ben/local/R_libs/")
library(plyr,lib.loc="/home/ben/local/R_libs/")

# on graham
# module load r/4.2.1
# module load openmpi/4.0.3
# export R_LIBS=~/local/R_libs/

# Chr9_10L
# these are the files with depth for all CDS
file_list <- list.files(pattern ="*.bam_all_CDS_depth_9_10L")
# read all files into a list
d <- lapply(file_list, read.table, skip=1, sep="\t")
# give the list names
#names(d) <- gsub(".*/(.*)\\..*", "\\1", file_list)
names(d) <- gsub(".*/(.*)\\..*", "\\1", substr(file_list,1,9))
names(d)[length(d)-1] <- paste(substr(file_list[length(file_list)-1],1,10),"_m",sep="")
names(d)[length(d)] <- paste(substr(file_list[length(file_list)],1,10),"_f",sep="")
# check that it worked
str(d, give.attr = FALSE)

# these are the files with depth for just the SL region
file_list <- list.files(pattern ="*bam_Chr6L_het_region_depth")
# read all files into a list
sl <- lapply(file_list, read.table, skip=1, sep="\t")
# give the list names
names(sl) <- gsub(".*/(.*)\\..*", "\\1", file_list)
# check that it worked
str(sl, give.attr = FALSE)


# work on independent datasets
# first use [[ to extract the first element of the list, data set 1 (auto.dta)
# and then [ to extract the first 3 columns and call summary on those.
# this is the first dataset
# d[[1]][, 1:3]
# this is the fourth dataset
# d[[4]][, 1:3]
# this is the second dataset by name
# d[["BJE4430_toXBconcat.bam_rg.bam_Chr9_10L:6000000-9000000_depth"]][, 1:3]

# cycle through each dataset
for(i in 1:length(names(d))) { 
    # subset dataframe 
    df_notSL <- d[[i]][, 1:3]
    names(df_notSL)<-c("CHR","POS","DEPTH")
    df_notSL_2 <- df_notSL[(df_notSL$DEPTH < 100),]
    df_SL <- sl[[i]][, 1:3]
    names(df_SL)<-c("CHR","POS","DEPTH")
    df_SL_2 <- df_SL[(df_SL$DEPTH < 100),]
    df_notSL_2$SL_or_not <- "not_SL"
    df_SL_2$SL_or_not <- "SL"
    new_df <- rbind(df_notSL_2,df_SL_2)
    new_df$individual <- names(d)[i]
    if(exists("combined_dat")){
        combined_dat <- rbind(combined_dat, new_df)
    } else {
        combined_dat <- new_df
    }
}
# get median depth for each one and also calculate the ratio of these medians
meds <- ddply(combined_dat, .(individual, SL_or_not), summarize, med = median(DEPTH))
ratioz<-numeric()
for(i in seq(from=1, to=nrow(meds), by=2)) { 
    ratioz <- c(ratioz,round(meds$med[i+1]/meds$med[i],digits = 2))
}
ratioz
x<-rep(2.3,nrow(meds)/2)
y<-rep(85,nrow(meds)/2)
facet_ratioz <- data.frame(x,y,ratioz, individual =names(d))

# reorder facets so that we have females and then males 
combined_dat$individual_f <- factor(combined_dat$individual,
                       levels = c("BJE4429_f","BJE4433_f","BJE4441_f",
                                  "BJE4444_f","BJE4445_f","BJE4446_f",
                                  "BJE4459_f","BJE4461_f","BJE4479_f",
                                  "BJE4515_f","SRR6357673_f","BJE4430_m",
                                  "BJE4431_m","BJE4442_m","BJE4443_m",
                                  "BJE4447_m","BJE4460_m","BJE4462_m",
                                  "BJE4473_m","BJE4477_m","BJE4478_m",
                                  "BJE4480_m","BJE4536_m","SRR6357672_m"),
                                   ordered = TRUE)
facet_ratioz$individual_f <- factor(facet_ratioz$individual,
                       levels = c("BJE4429_f","BJE4433_f","BJE4441_f",
                                  "BJE4444_f","BJE4445_f","BJE4446_f",
                                  "BJE4459_f","BJE4461_f","BJE4479_f",
                                  "BJE4515_f","SRR6357673_f","BJE4430_m",
                                  "BJE4431_m","BJE4442_m","BJE4443_m",
                                  "BJE4447_m","BJE4460_m","BJE4462_m",
                                  "BJE4473_m","BJE4477_m","BJE4478_m",
                                  "BJE4480_m","BJE4536_m","SRR6357672_m"),
                                   ordered = TRUE)

meds$individual_f <- factor(meds$individual,
                       levels = c("BJE4429_f","BJE4433_f","BJE4441_f",
                                  "BJE4444_f","BJE4445_f","BJE4446_f",
                                  "BJE4459_f","BJE4461_f","BJE4479_f",
                                  "BJE4515_f","SRR6357673_f","BJE4430_m",
                                  "BJE4431_m","BJE4442_m","BJE4443_m",
                                  "BJE4447_m","BJE4460_m","BJE4462_m",
                                  "BJE4473_m","BJE4477_m","BJE4478_m",
                                  "BJE4480_m","BJE4536_m","SRR6357672_m"),
                                   ordered = TRUE)

# facet plot
p<-ggplot(combined_dat, aes(x=SL_or_not, y=DEPTH)) + 
    geom_boxplot() +
    geom_text(data = meds, aes(y = med, label = round(med,2)),size = 8, vjust = -0.5) +
    facet_wrap(~individual_f)+
    geom_text(data = facet_ratioz, mapping = aes(x = x, y = y, label = ratioz), size=10)+
    theme_classic(base_size=22)

png("depth_of_coverage_6L.png",width = 1400, height = 1080)
    p 
dev.off()
```


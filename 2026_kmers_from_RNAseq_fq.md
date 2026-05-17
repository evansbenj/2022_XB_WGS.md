# Count kmerz in RNAseq

directory:
```
/home/ben/projects/rrg-ben/ben/2022_XB_WGS/XB_mesoneph_46_48
```

First get seqs from alignment files that have 29mers spaning a transcript specific SNP. Then extract kmers using this script
```
#!/usr/bin/perl
use strict;
use warnings;

# Check for correct arguments
die "Usage: perl extract_kmers.pl <k_value> <input.fasta>\n" unless @ARGV == 2;

my ($k, $input_file) = @ARGV;

# Open the FASTA file
open(my $fh, '<', $input_file) or die "Could not open file '$input_file' $!";

# Set the record separator to '>' to process FASTA format record-by-record
local $/ = '>';

# Discard the first empty record created by the record separator
my $empty_header = <$fh>;

while (my $record = <$fh>) {
    chomp $record;
    
    # Split the header and the sequence
    my ($header, @seq_lines) = split(/\n/, $record);
    my $sequence = join('', @seq_lines);
    
    # Clean the sequence: remove spaces and newlines
    $sequence =~ s/\s+//g;
    $sequence = uc($sequence);
    
    my $seq_len = length($sequence);
    
    # Skip sequences that are shorter than k
    if ($seq_len < $k) {
        warn "Warning: Sequence '$header' is shorter than k=$k. Skipping.\n";
        next;
    }
    
    # Extract and print the k-mers
    print "--- K-mers for $header ---\n";
    for my $i (0 .. $seq_len - $k) {
        my $kmer = substr($sequence, $i, $k);
        print "$kmer\n";
    }
}

```

# Count kmers in fq files for each sample (it can do F and R reads together:
```
#!/bin/sh
#SBATCH --job-name=makemeryldb
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=48:00:00
#SBATCH --mem=128gb
#SBATCH --output=makemeryldb.%J.out
#SBATCH --error=makemeryldb.%J.err
#SBATCH --account=rrg-ben

/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl count ${1}_trim.R[1,2].fq.gz threads=4 memory=128 k=29 output ${1}_trim.R12_meryldb.out
```
# Count reads in target
The target is a fasta file with each kmer from the target seq
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


/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl count k=29 wnt3W_target_kmers.fa output wnt3W_target_kmers.meryl
```

# Intersect with counts from each sample
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


/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl intersect ${1} wnt3W_target_kmers.meryl output ${1}_wnt3W_counts.meryl
```
# print out the counts:
```
#!/bin/sh
#SBATCH --job-name=meryl_print
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --time=2:00:00
#SBATCH --mem=128gb
#SBATCH --output=meryl_print.%J.out
#SBATCH --error=meryl_print.%J.err
#SBATCH --account=rrg-ben


/home/ben/projects/rrg-ben/ben/2025_bin/meryl/build/bin/meryl print ${1}_wnt3W_counts.meryl > ${1}_wnt3W_kmers_with_counts.txt
```

This worked perfectly. Only females have kmers from XBW. Yeay!

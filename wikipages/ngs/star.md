---
layout: default
title: STAR
---

## Interpreting output logs 

Example taken from [here](http://seqanswers.com/forums/showthread.php?t=29769)

	Started job on |	Mar 15 04:59:21
	Started mapping on |	Mar 15 05:02:08
	Finished on |	Mar 15 05:29:49
	Mapping speed, Million of reads per hour |	99.16

	Number of input reads |	45750724
	Average input read length |	202
	UNIQUE READS:
	Uniquely mapped reads number |	3331104
	Uniquely mapped reads % |	7.28%
	Average mapped length |	199.28
	Number of splices: Total |	736416
	Number of splices: Annotated (sjdb) |	665151
	Number of splices: GT/AG |	733030
	Number of splices: GC/AG |	2159
	Number of splices: AT/AC |	569
	Number of splices: Non-canonical |	658
	Mismatch rate per base, % |	1.11%
	Deletion rate per base |	0.04%
	Deletion average length |	2.30
	Insertion rate per base |	0.03%
	Insertion average length |	2.02
	MULTI-MAPPING READS:
	Number of reads mapped to multiple loci |	578140
	% of reads mapped to multiple loci |	1.26%
	Number of reads mapped to too many loci |	43288
	% of reads mapped to too many loci |	0.09%
	UNMAPPED READS:
	% of reads unmapped: too many mismatches |	0.00%
	% of reads unmapped: too short |	1.19%
	% of reads unmapped: other |	90.17%

Alex Dobin's response

First check the uniquely mapped reads:

Average mapped length |	199.28 : good, close you your pair length of 202

Mismatch rate per base, % |	1.11% : a bit on the high side, you would get 0.5-0.8% for good libraries, 

The splices are dominated by annotated and canonical, which is good.

The indel rate is low.

So, the reads that actually mapped uniquely - as few as they are - look fine.

The ratio of unique to multimappers is 7.28%/1.26% ~ 6 is somewhat high, that is - for typical human cells, 

I am not sure what are you sequencing. Our typical value is 15-20.

% of reads mapped to too many loci |	0.09% : by default "too many loci" is >10, but this number is good so you are not missing much.

Finally - most importantly - unmapped reads.

% of reads unmapped: too short |	1.19% : this number would be large if you had poor sequencing quality, 

it is surprisingly small (we typically get ~5%).

% of reads unmapped: other |	90.17% : 

this where all the unmapped reads went and it is very unusual.

It means that for 90% of the reads STAR could not find good anchor seeds. Two main possibilities are:

1. Contamination. Most reads have very little homology with human genome. 

You can check it by BLASTing a few unmapped reads against everything.

2. Repeat regions dominate expression. The number of loci a seed could map to is limited by 

--winAnchorMultimapNmax = 50 by default. You could increase it to ~1000 to see if more reads get mapped

 (also increase --outFilterMultimapNmax to output them as multi-mappers).

Other comments:

We typically see higher than 80% mapping rate for our RNA-Seq differential expression projects as well

ribosomal RNA contamination of mRNA-Seq libraries can produce this type of result due to the high copy number of the rRNA clusters. Adapter dimers are another possible culprit.

If the number of unmapped reads "too short" is very high

There are three main explanations for poor mappability with large proportion of unmapped alignments reported as "too short": 

From [STAR google groups](https://groups.google.com/forum/#!topic/rna-star/7RwKkvNLmI4 thread)

1. Poor sequencing quality.

On your quality scores distribution plots, If the median quality score drops below 20 at a certain cycle, you will have problems mapping the tails.
Note that by default STAR requires mapped length to be > 2/3 of the total read length (i.e. 2/3*202=135b in case of PE101).
This is controlled by --outFilterMatchNminOverLread and --outFilterScoreMinOverLread.
You can try to reduce this parameters to, say, 0.4 to see if you get more reads mapped - but the alignments will be shorter, of course.
Also, you can try to map read1 and read2 separately to see if one of them is more problematic than the other.

'''2a. Contamination with exogenous sequences.'''

You can try to BLAST a few of unmapped reads agains the full NCBI database to see if you get any good matches.

'''2b. Contamination with ribosomal RNA.'''

If your samples are "total RNA", depleted with Ribo-Zero or Ribo-Minus kits, it is possible that the depletion did not work well. rRNA are typically multi-mappers (and you get plenty of those), however, not all rRNA repeats make it into the main chromosomal assembly, and in this case they will not be mapped and will be reported as "alignment too short". We have recently had many cases like that in our lab for human tissues. I believe for the fly genome, the unplaced contigs are in chrU and chrUextra - please try to include them in the genome if you have not done so.

'''2c. Contamination with primer-dimers.'''

You can try to clip off Illumina adapters with various clipper software. I think this is quite rare.

'''3. Inserts that are too short.'''

If this happens, you will be sequencing into your adapter at the end of the 2nd mate. STAR will try to trim it off, but  the resulting alignment might be too short.
You can check if this is the case by following the suggestions in 1 or 3c.

By default STAR extends alignments until the number of mismatches exceeds --outFilterMismatchNmax (=10 by default) or the number of mismatches divided by read length exceeds --outFilterMismatchNoverLmax (=0.3 default). If you have ~10% sequence divergence, for ~200b reads you would expect ~20 mismatches per read, so the reads will be cut short because of the first parameter. Please try to increase it to a larger number - I would make it 100 and control the allowed mismatch rate with  --outFilterMismatchNoverLmax - if you average mismatch rate is 10%, the default --outFilterMismatchNoverLmax 0.3 is probably a good choice.

To increase sensitivity for such a high mismatch rate, it's also useful to reduce --seedSearchStartLmax (=50 by default), I would try 25 or even lower.

Working with very short reads (25 nt, as for example from ribosomal profiling)

25b reads will be much harder to map to the splice junctions, of course. Using annotations at the genome generation step would be very beneficial, with --sjdbOverhang chosen to be the longest read length -1.

At the mapping step, you can increase sensitivity with --seedSearchStartLmax 20, or even 15.

If you are using SJ.out.tab file, I would reduce --outSJfilterOverhangMin from 30 12 12 12 to, say, 30  8 8 8, to increase sensitivity for unannotated junctions.

You may want to be more restrictive for minimum allowed mapped length/score, by increasing:
outFilterScoreMin               0

    int: alignment will be output only if its score is higher than this value

outFilterScoreMinOverLread      0.66

        float: outFilterScoreMin normalized to read length (sum of mates' lengths for paired-end reads)

outFilterMatchNmin              0
    int: alignment will be output only if the number of matched bases is higher than this value

outFilterMatchNminOverLread     0.66

    float: outFilterMatchNmin normalized to read length (sum of mates' lengths for paired-end reads)

You can trim 3' adapters from within  STAR with

clip3pAdapterSeq            -
    string(s): adapter sequences to clip from 3p of each mate.  If one value is given, it will be assumed the same for both mates.

clip3pAdapterMMp            0.1
    double(s): max proportion of mismatches for 3p adpater clipping for each mate.  If one value is given, it will be assumed the same for both mates.

Mapping to transcriptome only

Set the parameters
--alignIntronMax smaller than --alignIntronMin.
--alignIntronMax 0 has a special meaning and sets the max intron to be defined by window sizes, by default ~600kb
Choosing the correct value for Sjdboverhang Option

sjdbOverhang should be set as readlength -1


The "Overhang" in sjdbOverhang and  --alignSJDBoverhangMin has different meanings - bad naming choice, unfortunately.

The --sjdbOverhang is used only at the genome generation step, and tells STAR how many bases to concatenate from donor and acceptor sides of the junctions. If you have 100b reads, the ideal value of --sjdbOverhang is 99, which allows the 100b read to map 99b on one side, 1b on the other side. One can think of --sjdbOverhang as the maximum possible overhang for your reads.

On the other hand, --alignSJDBoverhangMin is used at the mapping step to define the minimum allowed overhang over splice junctions. For example, the default value of 3 would prohibit overhangs of 1b or 2b.

***

## What some of the output tags in the bam file mean 

From STAR google group: [here](https://groups.google.com/forum/#!topic/rna-star/dYI_O-9hDXc):

In STAR's output bam file there are lines with uT tags - unmapped type -, which are used for labeling unmapped reads:

* uT:A:0 - other (no good anchor seeds)
* uT:A:1 - too short
* uT:A:2 - too many mismatches
* uT:A:3 - mapped to too many loci

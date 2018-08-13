---
layout: default
title: Sequence with biomaRt
---

## Sequence with biomart

    getSequence {biomaRt}	R Documentation

	Retrieves sequences

	Description

	This function retrieves sequences given the chomosome, start and end position or a list of identifiers. Using getSequence in web service mode (default) generates 5' to 3' sequences of the requested type on the correct strand. The type of sequence returned can be specified by the seqType argument which takes the following values: 'cdna';'peptide' for protein sequences;'3utr' for 3' UTR sequences,'5utr' for 5' UTR sequences; 'gene_exon' for exon sequences only; 'transcript_exon_intron' gives the full unspliced transcript, that is exons + introns;'gene_exon_intron' gives the exons + introns of a gene;'coding' gives the coding sequence only;'coding_transcript_flank' gives the flanking region of the transcript including the UTRs, this must be accompanied with a given value for the upstream or downstream attribute;'coding_gene_flank' gives the flanking region of the gene including the UTRs, this must be accompanied with a given value for the upstream or downstream attribute; 'transcript_flank' gives the flanking region of the transcript exculding the UTRs, this must be accompanied with a given value for the upstream or downstream attribute; 'gene_flank' gives the flanking region of the gene excluding the UTRs, this must be accompanied with a given value for the upstream or downstream attribute. In MySQL mode the getSequence function is more limited and the sequence that is returned is the 5' to 3'+ strand of the genomic sequence, given a chromosome, as start and an end position. So if the sequence of interest is the minus strand, one has to compute the reverse complement of the retrieved sequence, which can be done using functions provided in the matchprobes package. The biomaRt vignette contains more examples on how to use this function.




	Usage

	getSequence( chromosome, start, end, id, type, seqType, upstream, downstream, mart, verbose=FALSE)
	Arguments

	chromosome	 Chromosome name
	start	 start position of sequence on chromosome
	end	 end position of sequence on chromosome
	id	 An identifier or vector of identifiers.
	type	 The type of identifier used. Supported types are hugo, ensembl, embl, entrezgene, refseq, ensemblTrans and unigene. Alternatively one can also use a filter to specify the type. Possible filters are given by the listFilters function
	seqType	 Type of sequence that you want to retrieve. Allowed seqTypes are: cdna, peptide, 3utr, 5utr, genomic
	upstream	 To add the upstream sequence of a specified number of basepairs to the output.
	downstream	 To add the downstream sequence of a specified number of basepairs to the output.
	mart	 object of class Mart created using the useMart function
	verbose	 If verbose = TRUE then the XML query that was send to the webservice will be displayed.

Examples

	if(interactive()){
	mart <- useMart("ensembl",dataset="hsapiens_gene_ensembl")

	seq = getSequence(id="BRCA1", type="hgnc_symbol", seqType="peptide", mart = mart)
	show(seq)

	seq = getSequence(id="1939_at", type="affy_hg_u95av2", seqType="gene_flank",upstream = 20, mart = mart)
	show(seq)

	}

	seq = getSequence(id=U1all$ensembl_gene_id, type="ensembl_gene_id", seqType="cdna", mart = ensembl)
	write.fasta(sequences=as.list(seq$cdna),names=seq$ensembl_gene_id,file.out="write.my.dna.fasta")
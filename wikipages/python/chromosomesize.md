---
layout: default
---

## Make chromosome size file from reference fasta
    
    import csv
    from Bio import SeqIO
    
    myfile = open("mm10_chrominfo.txt", "wb")
    spamwriter = csv.writer(myfile, delimiter='\t', quoting=csv.QUOTE_MINIMAL)
    for seq_record in SeqIO.parse("genome.fa", "fasta"):
        spamwriter.writerow([str(seq_record.id) , str(len(seq_record))])
    
    myfile.close()

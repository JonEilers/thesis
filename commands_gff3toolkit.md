# Fri 28 Aug 2020 03:14:40 PM PDT

gff3toolkit is what I think I used before for cleaning up braker2 gff3 files
```bash
gff3_QC \
	-g /home/jon/Working_Files/gff3toolkit/braker2.1.5_rnaseq_ajap-prot.gff3 \
	-f /home/jon/Working_Files/genome_assemblies/old_pcali_ajap.redundans.platanus.masked.filter-1k.fa \
	-o error.txt \
	-s statistics.txt
```

example
gff3_QC -g example_file/example.gff3 -f example_file/reference.fa -o error.txt -s statistic.txt

whoa boy, a lot of errors 


```bash
Error_code	Number_of_problematic_models	Error_tag
Esf0014	1	##gff-version" missing from the first line
Esf0012	1845	Found Ns in a feature using the external FASTA
Ema0003	2	This feature is not contained within the parent feature coordinates
Ema0006	1	Wrong phase
Ema0008	38	Warning for distinct isoforms that do not share any regions
Ema0002	1	Protein sequence contains internal stop codons
 
# examples of errors 

['Line 1']	Esf0014	["##gff-version" missing from the first line]

'Line 265']	Esf0012	[Found 1 Ns in CDS feature of length 276 using the external FASTA, consists of 1 segment (start, length): (128204, 1)]

['Line 8784']	Ema0008	[Warning for distinct isoforms that do not share any regions: ['jg4595.t2', 'jg4595.t1']]

['Line 716503']	Ema0002	[Protein sequence contains internal stop codons at bp 1231, and 1267, and 1291, and 1966]
```


huh, interesting, gonna try the fix thing now. 
```bash
gff3_fix \
	-qc_r error.txt \
	-g /home/jon/Working_Files/gff3toolkit/braker2.1.5_rnaseq_ajap-prot.gff3 \
	-og /home/jon/Working_Files/gff3toolkit/braker2.1.5_rnaseq_ajap-prot.corrected.gff3
```

annnd fixed. 

example 
gff3_fix -qc_r error.txt -g example_file/example.gff3 -og corrected.gff3